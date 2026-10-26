# Microsserviços de Câmbio e Conversão de Moedas

Dois microsserviços em **Spring Boot** que conversam entre si via HTTP: um informa a taxa de câmbio entre duas moedas e o outro usa essa taxa para converter um valor.

Trabalho em grupo da disciplina de **Engenharia de Software II** da PUCRS.

## Arquitetura

```
Cliente ──► currency-conversion (porta 8100) ──► currency-exchange (porta 8000) ──► banco H2
```

| Serviço | Porta | Responsabilidade |
| --- | --- | --- |
| `currency-exchange-service` | 8000 | Guarda as taxas de câmbio em um banco H2 em memória (JPA) e as expõe por REST |
| `currency-conversion-service` | 8100 | Recebe moeda de origem, destino e quantidade, consulta o serviço de câmbio e devolve o valor convertido |

A chamada entre os serviços é feita de duas formas, para comparar as abordagens: com `RestTemplate` e com **OpenFeign** (cliente HTTP declarativo).

## Endpoints

```
GET /currency-exchange/from/{from}/to/{to}
GET /currency-conversion/from/{from}/to/{to}/quantity/{quantity}
GET /currency-conversion-feign/from/{from}/to/{to}/quantity/{quantity}
```

Exemplo:

```
http://localhost:8100/currency-conversion-feign/from/USD/to/INR/quantity/10
```

## Como rodar

Pré-requisito: Java 17 ou mais recente. Suba primeiro o serviço de câmbio e depois o de conversão, cada um em um terminal:

```bash
cd currency-exchange-service
./mvnw spring-boot:run
```

```bash
cd currency-conversion-service
./mvnw spring-boot:run
```

O console do banco H2 fica disponível em `http://localhost:8000/h2-console`.

## Tecnologias

Java 17 · Spring Boot 3 · Spring Data JPA · H2 · Spring Cloud OpenFeign · Maven
