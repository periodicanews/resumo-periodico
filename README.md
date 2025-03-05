# Resumo Periódico: Telegram Bot

Ferramenta experimental desenvolvida para gerar resumos de artigos científicos diretamente de arquivos PDF. Esses resumos são aproveitados no processo de curadoria da [newsletter Periódica](https://periodica.substack.com/).

O modelo de linguagem `GPT-4o mini` é utilizado em conjunto com a biblioteca de aprendizado de máquina GROBID, responsável pela extração de informações acadêmicas dos artigos.

# Funcionalidades

- Resumo via comando (`/resumo`)
- Controle de acesso ao chatbot via nome de usuário

# Metodologia

A aplicação é desenvolvida em Python.

O bot do Telegram é executado em um contêiner Docker e intermedia a comunicação com a API da OpenAI para gerar respostas.

O back-end executa uma imagem do GROBID, com apenas modelos CRF, utilizados para segmentação e estruturação de documentos acadêmicos em elementos semânticos, como título, autores, afiliações, referências bibliográficas, etc. A [documentação do GROBID](https://grobid.readthedocs.io/en/latest/Grobid-docker/) também disponibiliza a versão completa (10GB), porém, para esta etapa experimental, usamos a [versão leve](https://hub.docker.com/r/lfoppiano/grobid/) (300MB).

O GROBID é uma biblioteca de aprendizado de máquina para extração, parsing e reestruturação de documentos brutos, como PDFs, convertendo-os para TEI (Text Encoding Initiative), um formato XML voltado para representação de textos acadêmicos e científicos de forma estruturada.

A integração entre o GROBID e a API da OpenAI ocorre pelo chatbot com o cliente Python do GROBID (v0.0.9). O usuário envia um artigo em PDF, que é enviado ao servidor para processamento. O GROBID converte o conteúdo para TEI/XML e retorna o documento reestruturado. Em seguida, o prompt e o código TEI/XML são enviados ao modelo de linguagem. A resposta gerada é retornada ao usuário.

A reestruturação do artigo ocorre apenas uma vez, após o envio do documento. O servidor não mantém o histórico de prompts nem recupera arquivos processados (PDF e TEI), embora os armazene temporariamente em diretórios organizados por data e hora.

Anteriormente, foi realizada prova de conceito do chatbot com o framework Streamlit e o modelo de linguagem `claude-3-5-haiku-20241022`. O código está disponível [aqui](https://github.com/claromes/resumo-periodico-poc).

# BotFather

O [BotFather](https://core.telegram.org/bots/features#botfather) é o serviço principal do Telegram para o registro de bots. Use-o para criar novas contas de bot e gerenciar bots existentes.

# Pré-requisitos

- Python 3.11+
- Docker Compose
- Telegram Bot Token
- OpenAI API Key
- Configuração das variáveis de ambiente

    ```
    cp .example.env .env
    ```

# Instalação

```
docker compose up -d
```

# Contribuição

```
pre-commit install
```
