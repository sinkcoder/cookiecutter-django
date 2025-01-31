FROM python:3.6-slim-stretch

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

RUN echo 'deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib \n\
deb http://mirrors.aliyun.com/debian-security stretch/updates main' > /etc/apt/sources.list

RUN apt-get update \
    # dependencies for building Python packages
    && apt-get install --no-install-recommends -y \
      build-essential \
      vim \
      less \
      gcc \
      libmariadbclient-dev \
      python3-dev \
      procps \
      net-tools \
    # Translations dependencies
    && apt-get install -y gettext \
    # Uncomment below lines to enable Sphinx output to latex and pdf
    # && apt-get install -y texlive-latex-recommended \
    # && apt-get install -y texlive-fonts-recommended \
    # && apt-get install -y texlive-latex-extra \
    # && apt-get install -y latexmk \
    # cleaning up unused files
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && rm -rf /var/lib/apt/lists/*

# Requirements are installed here to ensure they will be cached.
COPY ./requirements /requirements
# All imports needed for autodoc.
RUN pip install -r /requirements/local.txt \
  -r /requirements/production.txt  \
  -i https://mirrors.aliyun.com/pypi/simple \
  --no-cache-dir

COPY ./compose/local/docs/start /start-docs
RUN sed -i 's/\r$//g' /start-docs
RUN chmod +x /start-docs

WORKDIR /docs
