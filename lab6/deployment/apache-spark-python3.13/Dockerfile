# ==========================================
# Stage 1: Grab the specific Python version
# ==========================================
FROM python:3.13.17-bookworm AS python-source

# ==========================================
# Stage 2: Final Spark image
# ==========================================
FROM apache/spark:4.2.0 AS final

# switch to root user
USER root

# Copy the Python installation from the first stage
COPY --from=python-source /usr/local /usr/local

# CRITICAL: Configure Spark to use the newly injected Python path
ENV PYSPARK_PYTHON=/usr/local/bin/python3
ENV PYSPARK_DRIVER_PYTHON=/usr/local/bin/python3

# Copy requirements.txt  to the working directory
COPY requirements.txt .
# Install required python packages
RUN  apt-get update -y && apt-get install -y python3-wheel
RUN python3 -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"

RUN python3 -m pip install --no-cache-dir -r requirements.txt

# switch to spark user
USER spark