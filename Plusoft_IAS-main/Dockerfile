# Use a imagem base apropriada
FROM python:3.11

# Instala dependências
RUN apt-get update && \
    apt-get install -y libaio1 wget unzip && \
    # Baixe e extraia o Oracle Instant Client
    wget https://download.oracle.com/otn_software/linux/instantclient/21.9/instantclient-basic-linux.x64-21.9.0.0.0dbru.zip && \
    unzip instantclient-basic-linux.x64-21.9.0.0.0dbru.zip -d /opt && \
    rm instantclient-basic-linux.x64-21.9.0.0.0dbru.zip && \
    # Crie um link simbólico
    ln -s /opt/instantclient_21_9/libclntsh.so /usr/lib/libclntsh.so && \
    ln -s /opt/instantclient_21_9/libocci.so /usr/lib/libocci.so

# Defina a variável de ambiente para as bibliotecas
ENV LD_LIBRARY_PATH /opt/instantclient_21_9

# Instale as dependências do seu aplicativo
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copie o código do seu aplicativo
COPY . /app
WORKDIR /app

# Comando para iniciar o Gunicorn
CMD ["gunicorn", "IA.app:app", "-b", "0.0.0.0:8080"]
