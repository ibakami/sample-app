FROM python:3.7 AS builder

EXPOSE 8000

# Sets utf-8 encoding for Python
ENV LANG=C.UTF-8

# Turns off writing .pyc files. Superfluous on an ephemeral container.
ENV PYTHONDONTWRITEBYTECODE=1

# Seems to speed things up
ENV PYTHONUNBUFFERED=1

# Ensures that the python and pip executables used
# in the image will be those from our virtualenv.
ENV PATH="/venv/bin:$PATH"

# Set working directory
WORKDIR /usr/src/app

# Install OS package dependencies.
# Do all of this in one RUN to limit final image size.
RUN apt-get update \
    && apt-get install -y \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Setup the virtualenv
RUN python -m venv /venv

# Install Python deps
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["gunicorn", "app:app", "-b", "0.0.0.0:8000", "--reload", "-k", "gevent", "--workers", "1", "--keep-alive", "80"]
