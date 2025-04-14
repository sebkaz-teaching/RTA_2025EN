## 1️⃣ Check topic list
Go to the home directory:
```sh
cd ~
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092
```

## 2️⃣ Create new topis with name `mytopic`
```sh
kafka/bin/kafka-topics.sh --create --topic mytopic --bootstrap-server broker:9092
```

## 3 Recheck topic list
```sh
kafka/bin/kafka-topics.sh --list --bootstrap-server broker:9092 | grep mytopic
```

## Kafka producer 
```python
from kafka import KafkaConsumer
import json  

SERVER = "broker:9092"
TOPIC  = "mytopic"

# Konsumer do pobierania danych z Kafka
consumer = KafkaConsumer(
    TOPIC,
    bootstrap_servers=SERVER,
    auto_offset_reset='earliest',
    value_deserializer=lambda x: json.loads(x.decode('utf-8'))
)

# Pobieranie transakcji w niemal real-time i analiza
for message in consumer:
    transaction = message.value
    if transaction["values"] > 80:
        print(f"🚨 BAD TRANSACTION: {transaction}")
```