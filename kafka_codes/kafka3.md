# Kafka Consumer

Befor we start go to the home directory
```bash
cd ~
```

## 1️⃣ Check topic list


```bash
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

## 2️⃣ Create new topis with name `mytopic`
```bash
kafka/bin/kafka-topics.sh --create --topic mytopic --bootstrap-server broker:9092
```

## 3️⃣ Recheck topic list
```bash
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092 | grep mytopic
```

## Kafka Consumer code

Kafka consumer - check if transaction value is $ > 80$.

```python
from kafka import KafkaConsumer
import json  

SERVER = "broker:9092"
TOPIC  = "mytopic"

# Consumer config
consumer = KafkaConsumer(
    TOPIC,
    bootstrap_servers=SERVER,
    auto_offset_reset='earliest',
    value_deserializer=lambda x: json.loads(x.decode('utf-8'))
)

for message in consumer:
    transaction = message.value
    if transaction["values"] > 80:
        print(f"🚨 BAD TRANSACTION: {transaction}")
```