# Use Python 3.8 base image
FROM python:3.8

ENV PYTHONUNBUFFERED=1

RUN apt-get update && apt-get install -y python3-venv && rm -rf /var/lib/apt/lists/*

WORKDIR /consumer

# Create a virtual environment
RUN python3 -m venv /venv

# Upgrade pip and install Poetry inside the venv
RUN /venv/bin/pip install --upgrade pip && /venv/bin/pip install poetry

# Copy the shared pyproject.toml from the project root
COPY pyproject.toml ./

# Instruct Poetry to use the existing venv
ENV POETRY_VIRTUALENVS_CREATE=false
ENV POETRY_VIRTUALENVS_IN_PROJECT=false

# Install project dependencies
RUN /venv/bin/poetry install --no-root

# Copy the consumer script into the container
COPY consumer/consume_items.py .

# Run the consumer script using Poetry (which uses the venv)
CMD ["/venv/bin/poetry", "run", "python", "consume_items.py"]
