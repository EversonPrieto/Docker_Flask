# Trabalho de Docker Flask

### Documentação:

O objetivo deste trabalho é criar uma aplicação web simples utilizando o Flask, contêinerizar a aplicação usando Docker, e expor o serviço via porta mapeada.

### 1. Configuração Inicial
Passo 1: Instalação do Docker
Antes de iniciar, foi necessário instalar o Docker, no meu caso eu usei o sistema operacional Linux Ubuntu. Para isso, os seguintes comandos foram utilizados no Terminal:

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
```
Após a instalação, o Docker foi verificado para garantir que estivesse ativo:

```
sudo systemctl status docker
```

Passo 2: Criação do Diretório e Aplicação Flask
Foi criado um diretório para o projeto:

```
mkdir flask_docker
cd flask_docker
```

Um ambiente virtual foi criado para isolar as dependências:

```
python3 -m venv venv
source venv/bin/activate
```

Instale o Flask:

```
pip install Flask
```

Em seguida, foi criado um arquivo chamado app.py, contendo o código de uma aplicação web simples usando Flask:

```
python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Olá, Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

### 2. Criação do Dockerfile

No mesmo diretório do projeto, foi criado um arquivo Dockerfile:

Passo 3: Editando o Dockerfile
No Dockerfile, foi adicionado o seguinte conteúdo:

```
# Usar uma imagem base do Python
FROM python:3.9-slim

# Definir o diretório de trabalho no contêiner
WORKDIR /app

# Copiar o arquivo requirements.txt e instalar dependências
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

# Copiar o código da aplicação para dentro do contêiner
COPY . .

# Expor a porta utilizada pela aplicação
EXPOSE 8080

# Comando para iniciar a aplicação Flask
CMD ["python", "app.py"]
```

Passo 4: Gerando o requirements.txt
Para garantir que as dependências fossem instaladas corretamente dentro do contêiner, o arquivo requirements.txt foi gerado com o seguinte comando:

```
pip freeze > requirements.txt
```

### 3. Construção e Execução da Imagem Docker
Passo 5: Construindo a Imagem Docker
Com o Dockerfile configurado, a imagem Docker foi construída com o seguinte comando:

```
docker build -t flask_app .
```

Esse comando instrui o Docker a construir uma imagem chamada flask_app com base nas instruções do Dockerfile.

Passo 6: Executando o Contêiner
Após a construção da imagem, o contêiner foi iniciado e a aplicação foi exposta na porta 8080:

```
docker run -d -p 8080:8080 flask_app
```

Esse comando executa o contêiner em segundo plano (-d) e mapeia a porta 8080 do contêiner para a porta 8080 do host.

A aplicação foi verificada no navegador através do endereço:

```
http://localhost:8080
```

### 4. Captura de Tela
A aplicação foi acessada no navegador e uma captura de tela foi realizada como evidência de funcionamento.
![53512484-0ea5-425b-a9e0-4702eea1020b](https://github.com/user-attachments/assets/15f0087c-5d21-415f-a328-2c6a0573205d)


### Integrantes:
Éverson Prieto
