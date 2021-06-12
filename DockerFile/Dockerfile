FROM ubuntu:18.04

WORKDIR /src
# update package
RUN set -x && \
    apt-get update &&\
    apt-get install git curl wget tar unzip -y &&\
    apt-get install locales dialog nodejs npm -y

# 日本語フォント
RUN apt-get install -y fonts-noto-cjk
RUN apt-get install fonts-ipafont fonts-ipaexfont -y
# RUN fc-cache -fv

RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ="Asia/Tokyo"

RUN npm install -g n

RUN n stable



ENV PYVERSION = 3.8.8
RUN wget https://www.python.org/ftp/python/3.8.8/Python-3.8.8.tgz
RUN tar xzf Python-3.8.8.tgz

RUN mkdir /usr/local/python3
RUN mv Python-3.8.8 /usr/local/python3
ENV PATH $PATH:/usr/local/python3/Python-3.8.8

RUN apt-get install python3-pip -y

# https://takemikami.com/2020/12/12/PyTorchjupytervisdomdockerimage.html
# RUN pip3 install numpy pandas matplotlib jupyter visdom streamlit
RUN pip3 install --upgrade pip setuptools
ADD requirements.txt /src
RUN pip3 install -r requirements.txt
RUN jupyter serverextension enable --py jupyterlab_code_formatter
RUN jupyter labextension install \
    @ryantam626/jupyterlab_code_formatter

RUN cd && \
    wget https://linux.kite.com/dls/linux/current && \
    chmod 777 current && \
    sed -i 's/"--no-launch"//g' current > /dev/null && \
    ./current --install ./kite-installer

ENTRYPOINT [ "jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token='jupyter'", "--notebook-dir=/src" ]
