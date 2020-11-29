FROM ubuntu:focal

VOLUME /project

# install stuff
RUN apt-get update
RUN apt-get install -y python3 python3-pip
RUN pip3 install --upgrade pip

# jupyter
RUN pip install jupyterlab

# misc packages
RUN pip install pandas sklearn scipy
RUN pip install matplotlib
RUN pip install numpy seaborn imbalanced-learn

# plotly library
ENV TZ=Asia/Jerusalem
ENV DEBIAN_FRONTEND=noninteractive
RUN apt install -y nodejs npm
RUN pip install jupyterlab "ipywidgets>=7.5"
RUN jupyter labextension install jupyterlab-plotly
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager plotlywidget
RUN pip install plotly

# jupyterlab prettify extension
RUN jupyter labextension install @ryantam626/jupyterlab_code_formatter
RUN pip install jupyterlab_code_formatter
RUN jupyter serverextension enable --py jupyterlab_code_formatter
RUN pip install isort yapf

# lightgbm
RUN apt-get install -y --no-install-recommends cmake build-essential gcc g++ git
RUN git clone --recursive --branch stable --depth 1 https://github.com/Microsoft/LightGBM
RUN (cd LightGBM/python-package/ && python3 setup.py install)

# jupyter config saving command
RUN echo "cp -r /root/.jupyter /project/_docker/res/root/ && rm -rf /project/_docker/res/.jupyter/lab/workspaces && echo \"done\"" >> /usr/bin/save-jupyter-config
RUN chmod +x /usr/bin/save-jupyter-config

# copy config files
COPY ./res /

# startup command
RUN echo "jupyter lab --allow-root --ip=0.0.0.0 --no-browser --NotebookApp.token='' &" >> /cmd
RUN echo "sleep infinity" >> /cmd

# cleanup
RUN rm -rf /var/lib/apt/lists/*

WORKDIR /project
CMD ["sh", "/cmd"]
