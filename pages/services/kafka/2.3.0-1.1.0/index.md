---
layout: layout.pug
navigationTitle: Kafka 2.3.0-1.1.0
excerpt:
title: Kafka 2.3.0-1.1.0
menuWeight: 8
model: /services/kafka/data.yml
render: mustache
---

<!-- Imported from https://github.com/mesosphere/dcos-commons.git:sdk-0.40 -->

DC/OS {{ model.techName }} is an automated service that makes it easy to deploy and manage {{ model.techName }} on Mesosphere DC/OS, eliminating nearly all of the complexity traditionally associated with managing a {{ model.techShortName }} cluster. {{ model.techName }} is a distributed high-throughput publish-subscribe messaging system with strong ordering guarantees. {{ model.techShortName }} clusters are highly available, fault tolerant, and very durable. For more information on {{ model.techName }}, see the {{ model.techName }} [documentation](http://kafka.apache.org/documentation.html). DC/OS {{ model.techShortName }} gives you direct access to the {{ model.techShortName }} API so that existing producers and consumers can interoperate. You can configure and install DC/OS {{ model.techShortName }} in moments. Multiple {{ model.techShortName }} clusters can be installed on DC/OS and managed independently, so you can offer {{ model.techShortName }} as a managed service to your organization.

## Benefits

DC/OS {{ model.techShortName }} offers the following benefits of a semi-managed service:

*   Easy installation
*   Multiple {{ model.techShortName }} clusters
*   Elastic scaling of brokers
*   Replication and graceful shutdown for high availability
*   {{ model.techShortName }} cluster and broker monitoring

## Features

DC/OS {{ model.techShortName }} provides the following features:

*   Single-command installation for rapid provisioning
*   Multiple clusters for multiple tenancy with DC/OS
*   High availability runtime configuration and software updates
*   Storage volumes for enhanced data durability, known as Mesos Dynamic Reservations and Persistent Volumes
*   Integration with syslog-compatible logging services for diagnostics and troubleshooting
*   Integration with statsd-compatible metrics services for capacity and performance monitoring

# Related Services

*   [DC/OS Spark](/services/spark/)
