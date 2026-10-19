# Notification Service

A Spring Boot service that listens for user signup events on Kafka and sends a welcome email over SMTP.

## What it does

When a producer publishes a JSON message to the `signup` topic, this service:

1. Consumes the message as a Kafka consumer in group `emailService`
2. Maps it to `EmailDto` (`to`, `from`, `subject`, `body`)
3. Sends the email through Gmail SMTP (`smtp.gmail.com:587` with STARTTLS)

## Tech stack

- Java 17
- Spring Boot 3.5
- Spring Kafka
- JavaMail (`javax.mail`)
- Maven

## Prerequisites

- JDK 17+
- Maven (or use the included `./mvnw` / `mvnw.cmd`)
- A running Kafka broker (default Spring Kafka bootstrap: `localhost:9092`)
- SMTP credentials for the sender account (Gmail app password if using Gmail)

## Configuration

Application name is set in `src/main/resources/application.properties`.

Kafka and SMTP settings currently live in code (`KafkaConsumerWelcomeEmailClient`). Before running, set:

- Kafka bootstrap servers if you are not using `localhost:9092`
- SMTP username and password (do **not** commit real credentials)

Expected Kafka payload on topic `signup`:

```json
{
  "to": "user@example.com",
  "from": "sender@gmail.com",
  "subject": "Welcome",
  "body": "Thanks for signing up."
}
```

## Run

```bash
./mvnw spring-boot:run
```

On Windows:

```bat
mvnw.cmd spring-boot:run
```

## Tests

```bash
./mvnw test
```

## Project layout

```
src/main/java/org/example/notificationservice_aug2026/
  NotificationServiceAug2026Application.java   # Spring Boot entry point
  clients/KafkaConsumerWelcomeEmailClient.java # Kafka listener + SMTP session
  dtos/EmailDto.java                           # Email payload
  utils/EmailUtil.java                         # Sends the MIME message
```
