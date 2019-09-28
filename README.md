# Open Compute @ SUTD

The SUTD Open Compute project is intended to serve members of the SUTD community who require computational resources (such as GPUs) that are otherwise costly to obtain. Due to capacity crunch, it will not literally be open access, but anyone with a reasonable use case will be allowed to use it.

To apply for access, please use the following [form](https://forms.office.com/Pages/ResponsePage.aspx?id=drd2NJDpck-5UGJImDFiPTpJx5vRVY5AqrPkE_loV1NUMUNQWEhUR0dRTlhKRVo3R0RBSUs4N0VWTy4u) (requires SUTD account).

## Resources

| Type          | Resource      | GPUs | Status      | Information      |
| ------------- | ------------- | ---- | ----------- | ---------------- |
| Jupyter       | Nimbus        | 12   | Maintenance | [Link](#nimbus)  |
| Jupyter       | Artemis       | 4    | Maintenance | [Link](#artemis) |
| Batch jobs    | Apollo        | 4    | Operational | [Link](#apollo)  |

## Support

For technical support, please use the appropriate channel in the [Slack](https://join.slack.com/t/sutd-compute/shared_invite/enQtMzAwMDEwNzg1MTA2LWY1MGI2M2NkNGQzODIzYTUyNDkxOTc0ZDgwOWRiMzk5ZWJlY2ZhMTMxOWM4ZDliZmM2YTkzY2YwNDVlZjYxOTM) group.

# JupyterHub Servers

Artemis/Nimbus support interactive computing through the Jupyter Notebook/Lab environment. For those who like to use the command line, JupyterLab also provides a fantastic terminal interface. The software stack on Artemis/Nimbus is JupyterHub running on top of Kubernetes. After you are granted access, you only need to login at the portal to be granted access to a GPU-accelerated Jupyter environment. 

On Artemis/Nimbus, a [pre-configured environment](https://github.com/NVAITC/ai-lab) with most typical packages is provided. This includes the Python data science stack (Numpy, Pandas etc.), DL frameworks (TensorFlow, Keras, PyTorch) and additional web-based IDE and desktop environments (for running GUI applications). Users can also install their own packages, but since the environment is designed to be ephemeral (so no user can accidentally destroy it), the process has to be repeated for each new session. If you have suggestions for improvements, please open an issue [here](https://github.com/NVAITC/ai-lab/issues).

Note: the environment (packages) is ephemeral but your user data (anything in the main `/home/jovyan/` folder) is persistent.

## Nimbus

![](https://img.shields.io/badge/status-maintenance-red)

Nimbus is a cluster of GPU servers to support educational (coursework) use of GPUs for courses such as Machine Learning and Deep Learning. 

## Artemis

![](https://img.shields.io/badge/status-maintenance-red)

The Artemis server is a high-performance server with 4 Titan XP GPUs to support Deep Learning projects and research.

# Apollo

![](https://img.shields.io/badge/status-operational-brightgreen)

The Apollo server is a high-performance server with 4 Titan XP GPUs to support Deep Learning projects and research. Access is provided via SSH. 

It is meant for advanced users who cannot use our provided environments due to highly specific requirements. This also means that we may not be provide technical support to specific problems (package error, installation etc.) encountered on Apollo. 

### Apollo Usage Guidelines

Users found violating the below usage guidelines may have access revoked from Apollo.

* Use Docker or virtual environments whenever possible to avoid dependency interference between users.
* **Running Jupyter notebooks on Apollo is strictly prohibited.** This is because Jupyter notebooks do not release the GPU memory unless shut down by the user. This prevents other users from using the GPU even if the GPU is not being utilised. 
* TensorFlow's default behaviour is to allocate memory on all available GPUs. Please prevent this by:
  * Set environment variable `CUDA_VISIBLE_DEVICES` to the GPU you intend to use (GPU 0/1/2/3)
  * For TF 1.x: `config.gpu_options.allow_growth = True` ([Read more](https://github.com/tensorflow/docs/blob/master/site/en/r1/guide/using_gpu.md#allowing-gpu-memory-growth))
  * For TF 2.0: `tf.config.experimental.set_memory_growth(gpu, True)` ([Read more](https://www.tensorflow.org/guide/gpu#limiting_gpu_memory_growth))
* Avoid using multiple GPUs or running jobs that run for more than 24h unless strictly necessary
* To reduce runtime of your job, please enable XLA to improve GPU compute efficiency:
  * For TF 1.x: `config.graph_options.optimizer_options.global_jit_level = tf.OptimizerOptions.ON_1` ([Read more](https://medium.com/@xianbao.qian/use-xla-with-keras-3ca5d0309c26))
  * For TF 2.0: `tf.config.optimizer_set_jit(True)` ([Read more](https://www.tensorflow.org/xla#enable_xla_for_tensorflow_models))

