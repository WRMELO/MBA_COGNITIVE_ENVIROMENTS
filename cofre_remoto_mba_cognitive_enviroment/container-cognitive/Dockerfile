FROM python:3.10-slim

# Instalações básicas do sistema
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    build-essential \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

# Diretório de trabalho
WORKDIR /app

# Copia o arquivo de dependências
COPY requirements.txt .

# Instala dependências Python
RUN pip install --upgrade pip && pip install -r requirements.txt

# Instala Jupyter
RUN pip install notebook jupyterlab

# Expõe a porta padrão do Jupyter
EXPOSE 8888

# Copia o restante dos arquivos
COPY . .

# Comando default para iniciar o Jupyter Notebook
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''"]
