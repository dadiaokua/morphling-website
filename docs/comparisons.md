---
sidebar_position: 10
---

# Comparisons

## Comparisons with Kubeflow

KubeDL supports training, modeling lineage versioning, inference, and automatic tuning for model deployment on Kubernetes.
Kubeflow is a machine learning toolkit for Kubernetes that contains several projects for different purposes.

Both projects share the same goal to enable machine learning run on Kubernetes, but also have distinctions in functionalities,
and can complement each other in this area.

- KubeDL modeling lineage and versioning: KubeDL tracks the history of a model in the native Kubernetes CRD format which
is unique and easy for existing Kubernetes users to adopt and can leverage broad Kubernetes tooling ecosystem.
It is integrated natively with the training and serving CRD to enable the full automation with controller.
That is, from the training job, to the model generated, to the deployment of the model all are linearly tracked and versioned
within Kubernetes. KubeDL model can supposedly work together with Kubeflow as well with some code changes in certain operators,
to enable model lineage and versioning in Kuberentes.

- KubeDL auto tuning for model deployment tries to figure out the best configurations for inference container with respect to
its container resources or running parameters . It leverages data and machine learning models to find the optimal
configuration set for its performance or container resource demand, or balance the tradeoff between them. The
architecture is implemented in standard Kubernetes CRD with controller and can be easily adopted by existing Kubernetes users, whereas this functionality does not exist in Kubeflow

- KubeDL training shares some functionalities with certain Kubeflow training operators to enable training jobs run on Kubernetes.
But there are noticeable feature differences in KubeDL from in Kubeflow:
  - KubeDL enables more machine learning frameworks particularly the two open sourced projects by Alibaba mentioned below,
  in addition to industry trending frameworks(TensorFlow, PyTorch, MPI) in a single controller.
    - https://github.com/mars-project/mars  Mars is a tensor-based unified framework for large-scale data computation
     which scales Numpy, pandas, Scikit-learn and Python functions. Think of it as the distributed version of numpy, but there are more than that.
    - https://github.com/alibaba/x-deeplearning An industrial deep learning framework for high-dimension sparse data.
     This project stems form Alibaba internal large scale deep learning production practices targeted for high dimensional sparse data typically found in e-commerce.
    
  - Integrate multi framework support in a single controller so that coherent metrics and monitoring support is possible,
    and that reduces operational burdens and resource allocations (compared with per framework per controller in Kubeflow.)
  - Built-in metadata and events persistent controller to save job metadata and events in external storages
    such as MySQL to outlive api-server state. This functionality does not exist in Kubeflow and can potentially complement that in Kubeflow.
  - Schedule jobs in a cron-like way.
  - Enable jobs run in host network mode in heterogeneous cluster environments. Due to large complexity in Alibaba and Alibaba's customer's environments,
   host network sometimes becomes a must-have feature to enable machine learning job run on Kubernetes.
  - Automatic file sync into container to include external artifacts on job launch.
  - Built-in advanced scheduling strategy practiced in Alibaba internal world's largest GPU and CPU clusters.
  - Dag scheduling strategy across different roles in distributed machine learning setting.
  - And more...

That said, some features can be enabled by running KubeDL as-is, to complement same missing functionalities in Kubeflow.
