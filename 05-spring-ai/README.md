# DIO Spring Boot - Final Project 05: Spring AI (budgeting)

## Introduction

This final module applies Spring AI in a budgeting API while preserving the same layered architecture used across the track.

The goal is to integrate AI capabilities without bypassing domain and use case boundaries.

## Code Context

The project processes voice commands to create and query financial transactions.

Primary flow:

1. Client uploads an audio file.
2. Audio is transcribed into text.
3. The model selects an application tool/use case.
4. The use case persists or queries transaction data.
5. The final response is converted to audio.

## Project Structure

- `src/main/java/dio/budgeting/domain`
  - Domain model and repository contract.
- `src/main/java/dio/budgeting/application`
  - Use cases used by both REST and AI tool calling.
- `src/main/java/dio/budgeting/infrastructure`
  - HTTP adapters, JPA adapters, and integration glue.

## Module-Specific Topics

### Speech-to-text

- Uses `TranscriptionModel` for audio transcription.
- Model settings are configured in `application.properties`.

### Tool calling

- `ChatClient` registers use-case tools.
- `@Tool` methods expose business capabilities to the model.

### Text-to-speech

- `TextToSpeechModel` produces MP3 output from final text.
- AI endpoint returns generated audio.

## Spring AI Documentation

- Spring AI Reference: https://docs.spring.io/spring-ai/reference/index.html
- ChatModel API: https://docs.spring.io/spring-ai/reference/api/chatmodel.html
- ChatClient API: https://docs.spring.io/spring-ai/reference/api/chatclient.html
- Tools API: https://docs.spring.io/spring-ai/reference/api/tools.html
- Audio Transcriptions API: https://docs.spring.io/spring-ai/reference/api/audio/transcriptions.html
- Audio Speech API: https://docs.spring.io/spring-ai/reference/api/audio/speech.html

## Shared Architecture References

Common architecture concepts are documented in the root README:

- [DDD layers](../README.md#ddd-layered-architecture)
- [Class vs record](../README.md#java-class-vs-java-record-in-domain-modeling)
- [Strong typed identifiers](../README.md#strong-typed-identifiers)
- [Repository pattern](../README.md#repository-pattern)
- [Use cases and Clean Architecture](../README.md#use-cases-and-clean-architecture)
- [Docker Compose support](../README.md#docker-compose-support-in-development)

## How to Run

Set your OpenAI API key:

```bash
export OPENAI_API_KEY="your_api_key_here"
```

Run the application and tests:

```bash
./gradlew bootRun
./gradlew test
```

## Notes

- Educational final project focused on AI plus architectural discipline.
- External provider integration tests may require active credentials.


# Minha evolução do projeto — Validação de transações

# Objetivo da melhoria

Implementei validações na API de orçamento para impedir o cadastro de transações com informações inválidas.

# Validações implementadas

- O valor da transação deve ser maior que zero.
- A descrição da transação não pode ser vazia, nula ou conter apenas espaços.
- Quando uma dessas regras é violada, a API retorna o status HTTP 400 (Bad Request).

As validações foram adicionadas ao arquivo `PersistTransactionUseCase.java`, antes da persistência da transação no banco de dados.

# Tecnologias utilizadas

- Java
- Spring Boot
- Spring AI
- Spring Data JPA
- MySQL
- Docker Compose
- Gradle
- Git e GitHub

# Como executar e testar

O projeto utiliza um banco de dados MySQL executado pelo Docker Compose.

No Windows, com Docker Desktop em execução, abra um terminal na pasta `05-spring-ai` e execute:

```powershell
docker compose up -d
.\gradlew.bat bootRun
```

Para testar o cadastro de uma transação válida, envie uma requisição POST para `/transactions` com o seguinte JSON:

```json
{
  "description": "Compra no supermercado",
  "category": "GROCERIES",
  "amount": 150
}
```

Para testar as validações, envie uma transação com `amount` igual a `-100` ou com `description` igual a `""`.

Nos testes realizados, a API retornou HTTP 400 para ambos os casos inválidos.

Também é possível consultar as transações da categoria GROCERIES com uma requisição GET para `/transactions/GROCERIES`.

# O que aprendi

Durante o desafio, pratiquei a configuração de uma aplicação Spring Boot, a integração com MySQL e Docker, a execução de requisições HTTP e a validação de dados antes da persistência.

Também aprendi a utilizar Git e GitHub para registrar alterações, criar commits e publicar minha evolução do projeto.

O projeto original apresenta recursos de Spring AI para transcrição de áudio, interpretação de comandos e geração de respostas. Minha contribuição foi fortalecer a validação das transações financeiras.