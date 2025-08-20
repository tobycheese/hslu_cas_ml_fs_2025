FROM python:3.12.2-slim-bookworm
LABEL maintainer "tobycheese https://github.com/tobycheese/"

# set timezone
RUN ln -sf /usr/share/zoneinfo/Europe/Zurich /etc/localtime

# install gcc, needed to compile twofish python package, needed for hopsworks
# todo: save 200MB of image space by using a two step container build
RUN apt update && apt install -y gcc

# install jupyter
RUN pip install --no-cache-dir jupyter

# generate a config directory and the default config
RUN jupyter notebook --generate-config

# configure jupyter for easy access (don't do this in production!!!)
RUN echo "c.NotebookApp.open_browser = False" >> /root/.jupyter/jupyter_notebook_config.py && \
    echo "c.JupyterApp.answer_yes = True" >> /root/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.ip = '*'" >> /root/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.allow_origin = '*'" >> /root/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.token = ''" >> /root/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.password = ''" >> /root/.jupyter/jupyter_notebook_config.py

# install rest of required libraries 
# tag sklearn because of mlflow autolog() support
# flask is needed by mlflow when validating signatures
RUN pip install --no-cache-dir pandas matplotlib scikit-learn==1.4.2 mlflow-skinny \
    pyarrow s3fs quixstreams kafka-python-ng evidently statsd loguru hopsworks flask wandb

WORKDIR /notebooks
EXPOSE 8888
ENTRYPOINT ["/usr/local/bin/jupyter-notebook", "-y", "--allow-root"]
