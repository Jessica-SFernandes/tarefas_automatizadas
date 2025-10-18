# Projeto Serverless de Processamento de Arquivos com AWS (S3, Lambda, DynamoDB e API Gateway)

Este projeto demonstra uma arquitetura serverless na AWS para processar arquivos enviados para um bucket S3, extrair informações e armazená-las no DynamoDB, com a exposição dos dados por meio de um API Gateway.

## 🚀 Visão Geral do Projeto

O objetivo principal deste projeto é criar um fluxo automatizado e escalável para ingestão e processamento de dados. Ele simula um **Sistema de Processamento de Notas Fiscais** (mencionado no diagrama) ou qualquer outro tipo de arquivo (CSV ou JSON).

### Fluxo do Projeto

1.  **Upload do Arquivo:** O usuário faz o upload de um arquivo (ex: CSV, JSON) contendo dados (como informações de nota fiscal) em um bucket do **Amazon S3**.
2.  **Trigger da Função:** O evento de upload no S3 dispara automaticamente uma **AWS Lambda Function** (escrita em Python).
3.  **Processamento e Gravação:** A função Lambda processa o conteúdo do arquivo (ex: lê o JSON, extrai campos como número, cliente, valor) e grava esses dados relevantes em uma tabela no **Amazon DynamoDB**.
4.  **Exposição da API:** Uma segunda função Lambda (ou a mesma com diferentes *endpoints*) é invocada por meio de um **Amazon API Gateway** para consultar a tabela do DynamoDB e expor os dados extraídos via API RESTful.

**Diagrama de Arquitetura:**
![Diagrama do Fluxo de Processamento de Arquivos com Lambda e S3](https://github.com/Jessica-SFernandes/tarefas_automatizadas/blob/main/diagrama/projeto.drawio.png)

## 🛠️ Tecnologias e Serviços AWS Utilizados

| Serviço | Descrição | Principais Vantagens |
| :--- | :--- | :--- |
| **Amazon S3** | Serviço de armazenamento de objetos seguro e escalável. Armazena os arquivos de entrada. | **Durabilidade** (Altamente confiável, com redundância), **Escalabilidade** (Ajusta a capacidade automaticamente), **Segurança** (Criptografia e controle de acesso). |
| **AWS Lambda** | Serviço de computação serverless. Executa o código de processamento em Python. | **Execução sob demanda** (código executado apenas quando necessário), **Escalabilidade automática**, **Custo eficiente** (paga apenas pelo tempo de execução), **Integração** com outros serviços AWS. |
| **Amazon DynamoDB** | Banco de dados NoSQL totalmente gerenciado. Armazena os dados processados e extraídos. | Armazenamento dos dados extraídos. |
| **Amazon API Gateway** | Serviço para criação, publicação e gerenciamento de APIs. Expõe os dados do DynamoDB. | Exposição de dados por API RESTful. |
| **AWS IAM** | Gerenciamento de Identidade e Acesso. (Opcional, mas crucial) | Gerenciamento de permissões para a função Lambda acessar S3 e DynamoDB. |

**Descrições de Serviços e Vantagens (extraídas das imagens):**

* **Amazon S3:** É um serviço de armazenamento em nuvem da AWS que permite "armazenar e acessar" dados de forma segura e escalável. Suporta qualquer tipo de arquivo (vídeo, áudio, imagens, documentos, etc.) e é ideal para backup e armazenamento de objetos.
* **AWS Lambda:** É um serviço de computação serverless que permite executar código em resposta a eventos, sem a necessidade de gerenciar servidores. Basta fazer o upload do código e o Lambda se encarrega de executar automaticamente, escalando conforme a demanda.

## 💻 Configuração e Execução

### Pré-requisitos

* Conta AWS ativa.
* AWS CLI configurado (opcional).

### Passos de Configuração:

1.  **Criação do Bucket S3:** Crie um bucket S3 que receberá os arquivos de upload.
2.  **Criação da Tabela DynamoDB:** Crie uma tabela no DynamoDB para armazenar os dados processados (Definir Chave Primária, ex: `ID` ou `numero_nota`).
3.  **Configuração da Função Lambda (Processamento):**
    * Crie uma função Lambda (ex: `S3ProcessorFunction`) em Python.
    * Configure as permissões IAM para que esta função possa **ler** do S3 e **escrever** no DynamoDB.
    * Configure um **Gatilho (Trigger)** no S3 para que a função seja invocada em cada evento de `ObjectCreated`.
    * Implemente a lógica de leitura, extração e gravação em Python (ver `lambda_function.py`).
4.  **Configuração da Função Lambda (API Consulta - Opcional):**
    * Crie uma segunda função Lambda (ex: `DynamoDBQueryFunction`).
    * Configure as permissões IAM para que esta função possa **ler** do DynamoDB.
5.  **Configuração do API Gateway (Opcional):**
    * Crie uma API REST no API Gateway.
    * Configure um recurso e um método (ex: `GET /notas/{id}`) e integre-o com a `DynamoDBQueryFunction`.

## 📂 Arquivos Adicionais


Imagens Importantes: [Link para a pasta de imagens](https://github.com/Jessica-SFernandes/tarefas_automatizadas/tree/main/imagens)
