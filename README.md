🚀 Multi-Node Kafka KRaft Cluster & Observability

A complete guide and technical implementation of a 3-node Apache Kafka (v4.1.1) cluster running in KRaft mode. This project demonstrates an end-to-end data pipeline integrated with a full Prometheus/Grafana observability stack, featuring automated alerting for consumer health.
🏗 System Architecture

The environment is built on AWS EC2 using Ubuntu, leveraging /etc/hosts for hostname resolution to maintain clean, portable configuration files.

    Cluster Mode: 3-Node KRaft (ZooKeeper-less) for metadata quorum and data replication.

    Data Ingestion: Distributed Go producers running on all 3 nodes, simulating real-time system telemetry.

    Stream Processing: A centralized Go consumer service processing streams into a CSV-based data sink.

    Monitoring: Full-stack observability using Prometheus, Grafana, and JMX/Kafka exporters.

📁 Repository Documentation Guide

File	Description
Kafka_Installation.txt	Complete 3-node Kafka setup, KRaft property configurations, and systemd service management.
Go_codes.txt	Source code for producers/consumers, build instructions, and service lifecycle commands.
monitor_kafka.txt	Installation of Prometheus & Grafana, metric endpoint configuration, and alerting rule setup.
adding_broker_to_cluster.txt	Operational guide for scaling the cluster with additional broker-only nodes.
commands.txt	A "cheat sheet" for Kafka operations: health checks, topic management, and troubleshooting.
server.properties	A baseline configuration template used across the cluster.
📊 Observability & Alerting Logic

The project focuses heavily on Proactive Monitoring. We track the "Golden Signals" of Kafka health:
1. The Monitoring Loop

    Prometheus scrapes metrics via hostnames (configured in /etc/hosts) to track broker state and throughput.

    Kafka Exporter provides deep visibility into consumer group offsets.

2. Alerting Validation (The "Lag Test")

A critical feature of this setup is the Consumer Lag Alert:

    Threshold: Triggered if kafka_consumergroup_lag > 100.

    Validation: By stopping the kafka_to_csv.service (details in Go_codes.txt), the consumer lag intentionally builds up, successfully triggering a Prometheus email alert to verify the notification pipeline.

🛠 Operational Highlights

    Hostname-Based Networking: No hardcoded IPs; all configurations use hostnames for better scalability and readability.

    Production Readiness: Every component (Kafka, Go apps, Exporters) is managed as a systemd service, ensuring automatic restarts and standard log handling.

    Scale-Ready: Documentation includes the specific procedure to expand the cluster with new brokers without downtime.
