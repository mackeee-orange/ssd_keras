#################################################################################################################
# Base Images
#################################################################################################################
FROM ubuntu:14.04

#################################################################################################################
#           ENV Setting
#################################################################################################################
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH

#################################################################################################################
#           Initial Setting
#################################################################################################################
RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    apt-get update && \
    apt-get install -y wget git libhdf5-dev g++ graphviz && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    /bin/bash /Miniconda3-latest-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-latest-Linux-x86_64.sh
RUN apt-get update -qq && \
    apt-get install --no-install-recommends -y \
    build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

#################################################################################################################
#           User Setting
#################################################################################################################
ENV NB_USER ssd_keras
ENV NB_UID 1000

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir -p $CONDA_DIR && \
    chown ssd_keras $CONDA_DIR -R && \
    mkdir -p /src && \
    chown ssd_keras /src


#################################################################################################################
#           Python Setting
#################################################################################################################
# Python
ARG python_version=3.5.3-0
ARG python_qt_version=4
RUN conda install -y python=${python_version} && \
    pip install numpy six tensorflow==1.2.0 keras==2.0.5 prettytensor && \
    pip install ipdb pytest pytest-cov python-coveralls coverage==3.7.1 pytest-xdist pep8 pytest-pep8 pydot_ng && \
    conda install Pillow scikit-learn notebook pandas matplotlib seaborn pyyaml h5py && \
    conda install -y pyqt=${python_qt_version} && \
    conda clean -yt

RUN conda install -y --channel https://conda.anaconda.org/menpo opencv3 && \
    conda clean -yt

ENV PYTHONPATH='/src/:$PYTHONPATH'


#################################################################################################################
#           Visualize
#################################################################################################################
RUN pip install pycallgraph
#################################################################################################################
#           Japanses Setting
#################################################################################################################
RUN apt-get update && apt-get -y install language-pack-ja-base language-pack-ja ibus-mozc && \
    update-locale LANG=ja_JP.UTF-8 LANGUAGE="ja_JP:ja"

ENV LC_CTYPE=ja_JP.UTF-8
#################################################################################################################
#           Improve speed
#################################################################################################################
RUN pip install Cython
#################################################################################################################
#           WORK Jupyter
#################################################################################################################
ADD ssd_keras /src/
WORKDIR /src
USER ssd_keras

EXPOSE 8888

CMD jupyter notebook --port=8888 --ip=0.0.0.0