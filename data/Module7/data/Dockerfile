FROM python:3.8-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y default-jre-headless wget && \
    rm -rf /var/lib/apt/lists/*

RUN pip install --default-timeout=1000 --no-cache-dir pyspark==3.3.0 jupyter boto3

RUN mkdir -p /opt/spark/jars && \
    wget -q https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar -P /opt/spark/jars && \
    wget -q https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.375/aws-java-sdk-bundle-1.11.375.jar -P /opt/spark/jars

EXPOSE 8888 4040

ENV PYSPARK_SUBMIT_ARGS="--jars /opt/spark/jars/hadoop-aws-3.2.0.jar,/opt/spark/jars/aws-java-sdk-bundle-1.11.375.jar pyspark-shell"

WORKDIR /home/jovyan

CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--no-browser", "--NotebookApp.token=", "--NotebookApp.password=", "--allow-root"]
