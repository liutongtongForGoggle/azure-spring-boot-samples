---
page_type: sample
languages:
- java
products:
- azure-event-hubs
description: "Azure Spring Cloud Stream Binder Sample project for Event Hub client library"
urlFragment: "azure-spring-cloud-sample-eventhubs-kafka"
---

# Spring Cloud Azure Stream Kafka Binder for Event Hub Code Sample shared library for Java

## Key concepts

This code sample demonstrates how to use the Spring Cloud Stream Kafka
binder for Azure Event Hub. The sample app exposes a RESTful API to receive
string message. Then message is sent through Azure Event Hub to a bean `consumer`
which simply logs the message.

## Getting started


Running this sample will be charged by Azure. You can check the usage and bill at
[this link][azure-account].




### Create Azure resources

1. Create a service principal for use in by your app. Please follow 
   [create service principal from Azure CLI][create-sp-using-azure-cli].

1. Create [Azure Event Hubs][create-event-hubs]. 

## Examples

1.  Update
    [application.yaml][application.yaml]
    file
    
    ```yaml
    spring:
      cloud:
        azure:
          client-id: [service-principal-id]
          client-secret: [service-principal-secret]
          tenant-id: [tenant-id]
          resource-group: [resource-group]
          eventhub:
            namespace: [eventhub-namespace]
        stream:
          function:
            definition: consume;supply
          bindings:
            consume-in-0:
              destination: [eventhub-name]
              group: [consumer-group]
            supply-out-0:
              destination: [the-same-eventhub-name-as-above]
    ```

1.  Run the `mvn spring-boot:run` in the root of the code sample to get the app running.

1.  Send a POST request

        $ curl -X POST http://localhost:8080/messages?message=hello

1.  Verify in your app’s logs that a similar message was posted:

    `New message received: hello`

1.  Delete the resources on [Azure Portal][azure-portal] to avoid unexpected charges.

## Troubleshooting

- Meet with  `Creating topics with default partitions/replication factor are only supported in CreateTopicRequest version 4+` error.
  
  ```text
  o.s.c.s.b.k.p.KafkaTopicProvisioner      : Failed to create topics
    org.apache.kafka.common.errors.UnsupportedVersionException: Creating topics with default partitions/replication factor are only supported in CreateTopicRequest version 4+. The following topics need values for partitions and replicas
  ```

  When this error is found, add this configuration item `spring.cloud.stream.kafka.binder.replicationFactor`, with the value set to at least 1. For more information, see [Spring Cloud Stream Kafka Binder Reference Guide](https://docs.spring.io/spring-cloud-stream-binder-kafka/docs/current/reference/html/spring-cloud-stream-binder-kafka.html).

## Next steps

## Contributing

<!-- LINKS -->
[azure-account]: https://azure.microsoft.com/account/
[azure-portal]: https://ms.portal.azure.com/
[create-event-hubs]: https://docs.microsoft.com/azure/event-hubs/
[create-sp-using-azure-cli]: https://github.com/Azure-Samples/azure-spring-boot-samples/blob/main/create-sp-using-azure-cli.md
[application.yaml]: https://github.com/Azure-Samples/azure-spring-boot-samples/blob/main/eventhubs/azure-spring-cloud-sample-eventhubs-kafka/src/main/resources/application.yaml