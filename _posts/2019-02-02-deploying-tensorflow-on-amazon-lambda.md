---
layout: posts
title: "Deploying Tensorflow on AWS Lambda serverless service"
author_profile: true
---

A few weeks ago I was trying to write a [webapp](/machinelearning/) for machine learning. It turns out that the `tensorflow` library is way too big for the deployment on AWS Lambda. After many hours of googling, I found that we have to reduce the size of the library. There are certainly several options to accomplish this task. Here I focus on the following technologies:
1. AWS Lambda <i class="fab fa-aws"></i>
2. python libraries: numpy and tensorflow
3. python library for the webapp: `flask`
4. `zappa`: a tool to deploy `flask` webapp onto AWS Lambda.

I use this combination simply because I am more comfortable to do everything in python than relying on `node.js` <i class="fab fa-node-js"></i> etc for the web.

## Deployment

Everything AWS here.

For large files, we can use Amazon S3 for the hosting and let the Lambda function to request it. 

We first create a `s3 bucket`:
```
aws s3 mb s3://ml-bucket-3
```
Here `ml-bucket-3` is the bucket name. We should also include this name in our `config.json` below such that our script can get back our files on s3.

###  Virtual environment

As in any `flask` webapp, we need to create a virtual environment. I used `virtualenv` to create it. After activating the new python environment (`activate`), we need to install the required packages:

```
pip install flask uuid zappa boto3 numpy tensorflow
```

The `webapp` folder is now 676 MB, which is way about the limit. Some of the unimportant stuff need to be removed:

```
find . -name "tests" | xargs rm -rf
find . -name "*.pyc" -delete
find . -name "*.so" | xargs strip
```

We use `zappa` to deploy the package on the `Amazon Lambda`. We first run `zappa init` to initialize the project. Here are our sample configuration file:

```json
{
    "dev": {
        "app_function": "webapp.app",
        "aws_region": "us-east-1",
        "profile_name": "default",
        "project_name": "webapp",
        "runtime": "python3.6",
        "s3_bucket": "YOUR-ZAPPA-BUCKET-NAME",
	    "slim_handler": true,
	    "use_precompiled_packages": false,
        "memory_size": 1024
    }
}
```

Remember to fill in the bucket name you created in the previous step. In addition to the default options, we also added the following: `slim_handler`, `use_precompiled_packages`, and `memory_size`.

And...this is how I deployed my webapp for text analytics ([GitHub Repo](https://github.com/wingkitlee0/arxiv_explore) <i class="fab fa-fw fa-github"></i>)