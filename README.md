# Amazon SageMaker Ludwig Transformer

This repository provides a notebook for training and deploying a NLP transformer deep learning model using the open source [Ludwig](https://github.com/uber/ludwig) AutoML framework and [HuggingFace](https://github.com/huggingface/transformers) library built on TensorFlow.

### Dataset

We will train a multi-class classification model to classify Amazon product reviews with a star rating in the range of `1-5`.  We will be using a HuggingFace pre-trained [yelp sentiment](https://huggingface.co/gilf/english-yelp-sentiment) model as part of our training and fine-tuning.

### Prerequisites

This notebook will [Build Your Own](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers-adapt-your-own.html) Docker container.  So you will require some additional permissions for your notebook role to publish to Amazon Elastic Container Registry [ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html).

The easiest way to add these permissions is to add the managed policy to the role that you used to start your notebook instance in the SageMaker dashboard. To this end, open your notebook instance, go to the **Permissions and encryption** section to to navigate to the IAM role.

![Notebook Role](docs/notebook_role.png)

After you choose the role, you can attach the `AmazonEC2ContainerRegistryFullAccess` policy. There’s no need to restart your notebook instance when you do this, the new permissions will be available immediately.

![Notebook Role](docs/managed_ecr_permissions.png)

### Notebooks

Launch the [train-ludwig.ipynb](train-ludwig.ipynb) notebook to begin.

This notebook will step you through the process of creating three containers.

1. `ludwig-training` - The base container build on TensorFlow 2.3 which clones and installs the latest Ludwig source and Amazon SageMaker [Training Toolkit](https://github.com/aws/sagemaker-training-toolkit)
2. `ludwig-inference` - The container for hosting Ludwig models extends `ludwig-training` with a model server leveraging the Amazon SageMaker [Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit)
3. `ludwig-reviews` - The reviews training container that extends `ludwig-training` to add model configuration.

You will be shown how to train and evaluate the reviews model before deploying in local mode and launching [test-ludwig.ipynb](test-ludwig.ipynb) to evaluate CSV and JSON test payloads before deploying to an Amazon SageMaker Endpoint.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.