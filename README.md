# flink-with-kafka-local
Apache Flink and Kafka test project

## Prerequisites

In order to run this project locally, you will need:
- Docker
- Java 11
- Gradle

## Running the Apache Flink and Kafka

```shell
docker-compose up -d
```

Now you should be able to access the Apache Flink dashboard at http://localhost:8081

## Starting the producer of dummy messages

- Build the producer app:
  ```
  ./gradlew --build-file kafka-dummy-producer/build.gradle shadowJar
  ```
- Run the producer app:
  ```
  java -jar kafka-dummy-producer/build/libs/kafka-dummy-producer-0.1-SNAPSHOT-all.jar input-topic
  ```

Now your `input-topic` is receiving messages in the following format:

```json
{
  "articleId": "9",
  "action": "CLICK",
  "eventTime": "2024-07-10T20:42:16"
}
```

## Building the Flink JAR

To package your job for submission to Flink, use the following.
Note that we are setting the `JAVA_HOME` environment variable to ensure that the correct version of Java is used
to build the JAR file (Java 11). Change the path to the correct one if needed.

```bash
JAVA_HOME=/usr/lib/jvm/jdk-11.0.13
./gradlew shadowJar
```

The jar will be created here: `./build/libs/ArticleClickStreamApp-0.1-SNAPSHOT-all.jar`

Go to http://localhost:8081/#/submit, and upload that JAR file. After uploading, click on it, and click SUBMIT.

![](assets/upload-jar.png)

Now the Flink job should be running. You can see the stream data below (the `input-topic` on the left and the `output-topic` on the right):

![](assets/stream-preview.gif)