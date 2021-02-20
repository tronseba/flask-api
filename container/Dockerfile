FROM python:3.9.2-slim
ARG PYTHON_MODULES

RUN useradd -d /project/api apiusr; \
    mkdir -p /project/api/src;\
    chown -R apiusr:apiusr /project 

USER apiusr

WORKDIR /project/api/src

RUN pip install ${PYTHON_MODULES}

COPY --chown=apiusr:apiusr ./src/. .

ENTRYPOINT ["python", "api.py"]