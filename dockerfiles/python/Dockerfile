FROM ubuntu:20.04

RUN apt-get update && apt-get install -y python3.8 ipython3 python3-pip

LABEL Description="Python 3.8"

ENTRYPOINT pip3 install pipenv && echo 'export PIPENV_ENV_IN_PROJECT=1' >> ~/.bashrc && source ~/.bashrc