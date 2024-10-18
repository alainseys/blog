FROM python:3.10-bullseye
LABEL maintainer "Seys Alain"
ENV container "docker"

WORKDIR /app/blog

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt
RUN python3 -m pip install --upgrade pip
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git git-lfs
RUN python3 -m pip install --upgrade mkdocs
# Git Config

#CMD [""]