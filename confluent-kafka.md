KAFKA CHEAT SHEET


How to install docker engine

```
https://docs.docker.com/install/linux/docker-ce/ubuntu/
https://docs.docker.com/install/linux/docker-ce/centos/
```

How to install docker compose

```
https://docs.docker.com/compose/install/
```

How to install docker compose alternative method

```shell
sudo apt install -y docker;
sudo usermod -aG docker ec2-user;
sudo service docker start;
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose;
chmod +x /usr/local/bin/docker-compose;
docker-compose --version
```

Uninstall the Docker CE:

```shell
sudo apt-get purge docker-ce
```
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

```shell
sudo rm -rf /var/lib/docker
You must delete any edited configuration files manually.
```

Docker compose file = config file (obrazy ISO systemów które mają być uruchomione)

```
docker-compose.yaml
```
Display all containers running and stoped {a = all}
```
docker ps -a
```
Display last used container
```
docker ps -al
```


#  ┬┌─┌─┐┌─┐┬┌─┌─┐  ┌─┐┌─┐┌┐┌┌─┐┬  ┬ ┬┌─┐┌┐┌┌┬┐  ┬ ┬┌─┐┌─┐┬─┐┌─┐┌─┐┌─┐  
#  ├┴┐├─┤├┤ ├┴┐├─┤  │  │ ││││├┤ │  │ │├┤ │││ │   │ │├─┘│ ┬├┬┘├─┤│ ┬├┤   
#  ┴ ┴┴ ┴└  ┴ ┴┴ ┴  └─┘└─┘┘└┘└  ┴─┘└─┘└─┘┘└┘ ┴   └─┘┴  └─┘┴└─┴ ┴└─┘└─┘  

Poprostu wyedytuj wersję oprogramowania w pliku docker-compose.yaml
a następnie wykonaj polecenie: docker-compose up -d

------------------------------------------------

#  ┬┌─┌─┐┌─┐┬┌─┌─┐  ┌┬┐┌─┐┬┌─┬ ┬┌┬┐┌─┐┌┐┌┌┬┐┌─┐┌─┐ ┬┌─┐
#  ├┴┐├─┤├┤ ├┴┐├─┤   │││ │├┴┐│ ││││├┤ │││ │ ├─┤│   │├─┤
#  ┴ ┴┴ ┴└  ┴ ┴┴ ┴  ─┴┘└─┘┴ ┴└─┘┴ ┴└─┘┘└┘ ┴ ┴ ┴└─┘└┘┴ ┴

kopiowanie dokumentacji manual z poleceń Kafki
	root@kafka-1:~# kafka-topics &> kafka_topics.opis
copy file from docker container to host:
	docker cp <containerId>:/file/path/within/container /host/path/target
example of coping file from Docker to host:
	sudo docker cp kafka-1:/root/kafka_topics.opis .

------------------------------------------------

#  ┬┌─┌─┐┌─┐┬┌─┌─┐  ┬┌─┌─┐┌┬┐┌─┐┌─┐┌┐┌┌─┐┌┐┌┌┬┐┬ ┬
#  ├┴┐├─┤├┤ ├┴┐├─┤  ├┴┐│ ││││├─┘│ ││││├┤ │││ │ └┬┘
#  ┴ ┴┴ ┴└  ┴ ┴┴ ┴  ┴ ┴└─┘┴ ┴┴  └─┘┘└┘└─┘┘└┘ ┴  ┴ 

a)	root@base - to komponent konfluenta a nie Kafki
b)	Kafka ma tylko 2 komponenty i 99.99% operacji wykonuje się z poziomu BROKER-a lub ZOOKEEPER-a
c)	polecenia z książek o Kafka wykonujemy następująco - przykład dla Broker-a o nazwie kafka-1:
	root@kafka-1:/usr/bin# - tutaj są pliki wykonalne KAFKA

------------------------------------------------

#  ┬┌─┌─┐┌─┐┬┌─┌─┐  ┬ ┬  ┌┬┐┌─┐┌─┐┬┌─┌─┐┬─┐
#  ├┴┐├─┤├┤ ├┴┐├─┤  │││   │││ ││  ├┴┐├┤ ├┬┘
#  ┴ ┴┴ ┴└  ┴ ┴┴ ┴  └┴┘  ─┴┘└─┘└─┘┴ ┴└─┘┴└─

docker-compose up -d 					#create Kafka component && aktualizacja docker-compose.yml
docker-compose start
docker-compose stop						#zatrzymuje zdefiniowane komponenty z pliku docker-compose.yml
docker-compose ps						#procesy
docker-compose exec base /bin/bash		#łączenie się do base container jako root
docker-compose exec kafka-1 /bin/bash	#łączenie się do brokera jako root
docker-compose start kafka-1			#uruchamianie broker-a o nazwie kafka-1
docker-compose stop kafka-1				#stop broker

zatrzymaj i usuń całego brokera oraz jego dane:
	docker-compose stopafka-1 && docker-compose rm kafka-1 && docker volume rm confluent-ops_data-kafka-1

------------------------------------------------

#  ┬  ┌─┐┌─┐┌─┐
#  │  │ ││ ┬└─┐
#  ┴─┘└─┘└─┘└─┘

docker-compose logs kafka-2 | tail				#czytanie logów brokera 2 z pliku /var/log/kafka/server.log
docker-compose logs kafka-2 | tail -n 200		#czytanie 200 linii logów brokera 2 z pliku /var/log/kafka/server.log
docker-compose logs kafka-2 | head -n 200		#czytanie 200 linii logów brokera 2 z pliku /var/log/kafka/server.log
docker-compose logs kafka-2 | grep force		#czy doszło do kontrolowanego wyłączenia
docker-compose logs kafka-2 | grep Resigned		#sprawdzanie kiedy broker 2 oddał rolę kontrolera
docker-compose logs kafka-3 | grep PartitionLeaderEpoch		#strona 25 w dokumencie Confluent Excersise 
docker-compose exec kafka-1 ls /var/lib/kafka/data			#wylistuj dostępne logi na brokerze

odczytanie dowolnego loga:
	kafka-dump-log --print-data-log --files /var/lib/kafka/data/nazwa_topicu-0/00000000000000000000.log
wylistowanie procesów do pliku aby przeczytać obcięte przez tty wiersze:
	root@kafka-1:/# ps -ax > lista_procesow.txt
	
------------------------------------------------

#  ┌─┐┌─┐┌─┐┬┌─┌─┐┌─┐┌─┐┌─┐┬─┐
#  ┌─┘│ ││ │├┴┐├┤ ├┤ ├─┘├┤ ├┬┘
#  └─┘└─┘└─┘┴ ┴└─┘└─┘┴  └─┘┴└─

ustalanie który broker jest kontrolerem:
	zookeeper-shell zk-1:2181 get /controller
sprawdzanie brokerów zarejestrowany w zookeeper:
	zookeeper-shell zk-1:2181 ls /brokers/ids
	
------------------------------------------------

#  ┌┬┐┌─┐┌─┐┬┌─┐┌─┐
#   │ │ │├─┘││  └─┐
#   ┴ └─┘┴  ┴└─┘└─┘

wyświetlenie topiców (zarówno z base jak i z brokerów)
	root@base:/# kafka-topics --zookeeper zk-1:2181 --list
	root@kafka-2:/usr/bin# kafka-topics --zookeeper zk-1:2181 --list

utworzenie topic-u na 6 partycjach i z 2 replikami:
	kafka-topics --create --zookeeper 
	zk-1:2181 --topic nazwa_topicu --partitions 6 --replication-factor 2

utworzenie topic na 6 partycjach oraz replikacja na 3 brokerach:
	kafka-topics --zookeeper zk-1:2181 --create --topic nazwa_topicu --partitions 6 --replication-factor 3

utworzenie topic-u na 6 partycjach ale z 2 replikami ręcznie ustawionymi i zbalansowanymi (101,102 brokers IDs):
	kafka-topics --zookeeper zk-1:2181 --create --topic nazwa_topicu --replica-assignment 101:102,102:101,101:102,102:101,101:102,102:101

sprawdzanie statusu topic-u:
	kafka-topics --describe --zookeeper zk-1:2181 --topic nazwa_topicu

edycja topicu, zwiększenie ilości partycji topic-u:
	kafka-topics --zookeeper zk-1:2181 --alter --topic nazwa_topicu --partitions 12

delete topic, skasowanie topic-u:
	kafka-topics --zookeeper zk-1:2181 --delete --topic nazwa_topicu
	
------------------------------------------------

#  ┌┬┐┌─┐┌─┐┬┌─┐┌─┐  ┌┬┐┌─┐┌┬┐┌─┐
#   │ │ │├─┘││  └─┐   ││├─┤ │ ├─┤
#   ┴ └─┘┴  ┴└─┘└─┘  ─┴┘┴ ┴ ┴ ┴ ┴

wyprodukowanie danych losowych 6000 rekordów po 100 bajtów:
	kafka-producer-perf-test --topic nazwa_topicu --num-records 6000 --record-size 100 --throughput 1000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092,kafka-3:9092

wyprodukowanie (100000 bajtów x 100 bajtów na rekord = 100MB) danych dla topic-u o przepustowości 1000 bajtów/sec:
	kafka-producer-perf-test --topic nazwa_topicu --num-records 1000000 --record-size 100 --throughput 1000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092

wygenerowanie (2 milionów bajtów x 1000 bajtów na rekord = 2 GB) danych do topic-u o przepustowości 1GB/sec:
	kafka-producer-perf-test --topic nazwa_topicu --num-records 2000000 --record-size 1000 --throughput 1000000000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092

console producer / ręczne dane:
	kafka-console-producer --broker-list kafka-1:9092,kafka-2:9092,kafka-3:9092 --topic nazwa_topicu
	kafka-console-producer --broker-list kafka-1:9092,kafka-2:9092 --topic nazwa_topicu (na 2 brokery)
	
------------------------------------------------

#  ┌─┐┌─┐┌┬┐┌┬┐┬┌┬┐
#  │  │ ││││││││ │ 
#  └─┘└─┘┴ ┴┴ ┴┴ ┴ 


recovery-point-offset-checkpoint = odczytanie dokąd dane zostały flushed to disks:
	cat /var/lib/kafka/data/recovery-point-offset-checkpoint | grep nazwa_topicu

replication-point-offset-checkpoint = odczytanie dokąd dane zostały zczytane czyli "last committed offset":
	cat /var/lib/kafka/data/replication-offset-checkpoint | grep nazwa_topicu
	
------------------------------------------------

#  ┌─┐┌─┐┬─┐┌┬┐┬ ┬┌─┐ ┬┌─┐
#  ├─┘├─┤├┬┘ │ └┬┘│   │├┤ 
#  ┴  ┴ ┴┴└─ ┴  ┴ └─┘└┘└─┘

wyświetlenie, sprawdzanie partycji topic-u:
	ls -l /var/lib/kafka/data/nazwa_topicu-0

historia liderów partycji (checkpoint):
	cat /var/lib/kafka/data/nazwa_topicu-0/leader-epoch-checkpoint

------------------------------------------------

#  ┌─┐┌─┐┬─┐┌┬┐┬ ┬  ┌─┐┌─┐┬─┐┌─┐┬ ┬┌┬┐┌─┐┌─┐┌┐┌┬┌─┐
#  ├─┘│ │├┬┘ │ └┬┘  └─┐├─┘├┬┘├─┤│││ ││┌─┘├─┤││││├┤ 
#  ┴  └─┘┴└─ ┴  ┴   └─┘┴  ┴└─┴ ┴└┴┘─┴┘└─┘┴ ┴┘└┘┴└─┘

a) docker ps
b) telnet localhost 2181
c) telnet localhost 8088

------------------------------------------------

#  ┬┌─┌─┐┌┐┌┌─┐┬ ┬┌┬┐┌─┐┌─┐ ┬┌─┐
#  ├┴┐│ ││││└─┐│ ││││├─┘│   │├─┤
#  ┴ ┴└─┘┘└┘└─┘└─┘┴ ┴┴  └─┘└┘┴ ┴

konsumpcja danych z topic-u:
	kafka-consumer-perf-test --broker-list kafka-1:9092,kafka-2:9092 --topic nazwa_topicu --group test-group --threads 1 --show-detailed-stats --timeout 1000000 --reporting-interval 5000 --messages 10000000

konsumpcja od początku zawartości topic-u:
	kafka-console-consumer --from-beginning --topic nazwa_topicu --bootstrap-server kafka-1:9092,kafka-2:9092

konsumowanie wiadomości nie tylko real-time ale również przechowywanych w kafce czyli wiadomości offsetowych/archiwalnych:
	kafka-console-consumer --topic __consumer_offsets --bootstrap-server kafka-1:9092,kafka-2:9092 --formatter "kafka.coordinator.group.GroupMetadataManager\$OffsetsMessageFormatter" | grep nazwa_topicu
	
------------------------------------------------

#  ┌─┐┬─┐┌─┐┌─┐┌─┐┬ ┬┌─┐┌┬┐┌─┐┬ ┬┌─┐┌─┐┌─┐    ┌┬┐┬ ┬┬─┐┌─┐┬ ┬┌─┐┬ ┬┌─┐┬ ┬┌┬┐
#  ├─┘├┬┘┌─┘├┤ ├─┘│ │└─┐ │ │ │││││ │└─┐│       │ ├─┤├┬┘│ ││ ││ ┬├─┤├─┘│ │ │ 
#  ┴  ┴└─└─┘└─┘┴  └─┘└─┘ ┴ └─┘└┴┘└─┘└─┘└─┘     ┴ ┴ ┴┴└─└─┘└─┘└─┘┴ ┴┴  └─┘ ┴ 

Confluent Auto Data Balancer (wyrównanie ilości danych na Brokerach z przepustowością 1MB/s)
	confluent-rebalancer execute --zookeeper zk-1:2181 --metrics-bootstrap-server kafka-1:9092,kafka-2:9092 --throttle 1000000 --verbose --force

sprawdzanie limitów przepustowości na brokerach
	kafka-configs --describe --zookeeper zk-1:2181 --entity-type brokers

monitorowanie rebalancowania
	confluent-rebalancer status --zookeeper zk-1:2181

zmiana balansowania przepustowości do 1GB/s
	confluent-rebalancer execute --zookeeper zk-1:2181 --metrics-bootstrap-server kafka-1:9092,kafka-2:9092 --throttle 1000000000 --verbose

------------------------------------------------

#  ┬  ┌┐┌  ┌┬┐  ┌─┐  ─┐ ┬
#  │  │││   ││  ├┤   ┌┴┬┘
#  ┴  ┘└┘  ─┴┘  └─┘  ┴ └─

wyświetlanie zawartości pliku index:
	kafka-run-class kafka.tools.DumpLogSegments --files /var/lib/kafka/data/nazwa_topicu-0/00000000000000000000.index
	
------------------------------------------------

#  ┬  ┌─┐┌─┐┌┬┐┌─┐┬─┐  ┌─┐┌─┐┌─┐┌─┐┬ ┬
#  │  ├┤ ├─┤ ││├┤ ├┬┘  ├┤ ├─┘│ ││  ├─┤
#  ┴─┘└─┘┴ ┴─┴┘└─┘┴└─  └─┘┴  └─┘└─┘┴ ┴

wyświetlenie leader-epoch-checkpoint:
	docker-compose exec kafka-1 cat /var/lib/kafka/data/nazwa_topicu-0/leader-epoch-checkpoint
	docker-compose logs kafka-3 | grep PartitionLeaderEpoch (strona 25 Confluent Excersise)
	
------------------------------------------------

KAFKA PRODUKCJA WIADOMOSCI MANUALNYCH ORAZ KONSUMPCJA W TERMINALU

PRODUKCJA:  kafka-console-producer --broker-list kafka-1:9092,kafka-2:9092 --topic nazwa_topicu
KONSUMPCJA z group.id: kafka-console-consumer --consumer-property group.id=test-consumer-group --from-beginning --topic nazwa_topicu --bootstrap-server kafka-1:9092,kafka-2:9092


------------------------------------------------

UTWORZENIE CONSUMER GROUP
	kafka-consumer-groups --bootstrap-server kafka-1:9092,kafka-2:9092 --group test-consumer-group --describe

------------------------------------------------

#  ┬┌─  ┌─┐  ┌─┐   ┬      ┌─┐┬  ┬
#  ├┴┐  └─┐  │─┼┐  │      │  │  │
#  ┴ ┴  └─┘  └─┘└  ┴─┘    └─┘┴─┘┴

https://docs.confluent.io/current/quickstart/ce-docker-quickstart.html

START KSQL CLI
	docker-compose exec ksql-cli ksql http://ksql-server:8088




------------------------------------------------

#  ┌─┐┬ ┬┌┐┌┌─┐┬ ┬┬─┐┌─┐┌┐┌┬┌─┐┌─┐┌─┐ ┬┌─┐
#  └─┐└┬┘││││  ├─┤├┬┘│ │││││┌─┘├─┤│   │├─┤
#  └─┘ ┴ ┘└┘└─┘┴ ┴┴└─└─┘┘└┘┴└─┘┴ ┴└─┘└┘┴ ┴

synchroniczne przesyłanie wiadomości = 1 partycja jednak nie oznacza że brakuje redundancji - o redundancji decyduje ilość ustawionych replik

------------------------------------------------

#  ┌─┐┬┌─┬ ┬┌┬┐┬ ┬┬  ┌─┐┌─┐ ┬┌─┐  ┌┬┐┌─┐┌┐┌┬ ┬┌─┐┬ ┬
#  ├─┤├┴┐│ │││││ ││  ├─┤│   │├─┤   ││├─┤│││└┬┘│  ├─┤
#  ┴ ┴┴ ┴└─┘┴ ┴└─┘┴─┘┴ ┴└─┘└┘┴ ┴  ─┴┘┴ ┴┘└┘ ┴ └─┘┴ ┴

Broker ma poczekać na nagromadzenie większej ilości danych, zanim odpowie konsumentowi = większa przepustowość, mniejsze obciążenie brokera, ale może zwiększyć opóźnienie:
root@base:/# echo "fetch.min.bytes=100000" >> /data/consumer.properties



#  ██████╗ ██████╗  ██████╗ ██╗  ██╗███████╗██████╗     ███╗   ███╗ ██████╗ ███╗   ██╗██╗████████╗ ██████╗ ██████╗ ██╗███╗   ██╗ ██████╗ 
#  ██╔══██╗██╔══██╗██╔═══██╗██║ ██╔╝██╔════╝██╔══██╗    ████╗ ████║██╔═══██╗████╗  ██║██║╚══██╔══╝██╔═══██╗██╔══██╗██║████╗  ██║██╔════╝ 
#  ██████╔╝██████╔╝██║   ██║█████╔╝ █████╗  ██████╔╝    ██╔████╔██║██║   ██║██╔██╗ ██║██║   ██║   ██║   ██║██████╔╝██║██╔██╗ ██║██║  ███╗
#  ██╔══██╗██╔══██╗██║   ██║██╔═██╗ ██╔══╝  ██╔══██╗    ██║╚██╔╝██║██║   ██║██║╚██╗██║██║   ██║   ██║   ██║██╔══██╗██║██║╚██╗██║██║   ██║
#  ██████╔╝██║  ██║╚██████╔╝██║  ██╗███████╗██║  ██║    ██║ ╚═╝ ██║╚██████╔╝██║ ╚████║██║   ██║   ╚██████╔╝██║  ██║██║██║ ╚████║╚██████╔╝
#  ╚═════╝ ╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝    ╚═╝     ╚═╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝   ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝ ╚═════╝ 

------------------------------------------------

OBSERWACJA BROKER-a GDY WYSYŁA WIADOMOŚCI

1. tworzymy topic
kafka-topics --zookeeper zk-1:2181 --create --topic i-love-logs --partitions 1 --replication-factor 1
2. zasypujemy topic 10 milionami wiadomości
kafka-producer-perf-test --topic i-love-logs --num-records 10000000 --record-size 100 --throughput 1000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092 interceptor.classes=io.confluent.monitoring.clients.interceptor.MonitoringProducerInterceptor
3. Create a file named  consumer.properties  in the  ~/confluent-ops/data folder. Add this content to the file:
interceptor.classes=io.confluent.monitoring.clients.interceptor.MonitoringConsumerInterceptor
4. Skonsumuj trochę danych:
kafka-consumer-perf-test --broker-list kafka-1:9092,kafka-2:9092 --topic i-love-logs --group cg --messages 10000000 --threads 1 --show-detailed-stats --reporting-interval 5000 --consumer.config /data/consumer.properties
5. sprawdź obciążenie komputera
docker-compose exec kafka-3 top -n1
6. Dodaj pozycję fetch.min.bytes = 100000 do pliku /data/consumer.properties
root@base:/# echo "fetch.min.bytes=100000" >> /data/consumer.properties
This configuration tells the Broker to wait for larger amounts of data to accumulate before responding to the
Consumer. This generally improves throughput and reduces load on the Broker but can increase latency
7. Odczytaj trochę danych dla tematu i-love-logs z nową grupą konsumentów o nazwie cg-fetch-min
kafka-consumer-perf-test --broker-list kafka-1:9092,kafka-2:9092 --topic i-love-logs --group cg-fetch-min --messages 10000000 --threads 1 --show-detailed-stats --reporting-interval 5000 --consumer.config /data/consumer.properties

------------------------------------------------

RESETOWANIE OFFSET-u
	kafka-consumer-groups --bootstrap-server kafka-1:9092,kafka-2:9092 --group cg --reset-offsets --to-earliest --all-topics --execute

------------------------------------------------

UTWORZENIE TOPIC-u z 6 partycjami oraz 3 replikami
	kafka-topics --zookeeper zk-1:2181 --create --topic performance --partitions 6 --replication-factor 3 --execute

------------------------------------------------

TEST WYDAJNOŚCI PRODUCER-a W ZALEŻNOŚCI OD PARAMETRÓW
	kafka-producer-perf-test --topic performance --num-records 10000000 --record-size 100 --throughput 10000000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092 <VARIABLE_HERE> gdzie acks=1 acks=all acks=1 batch.size=1000000 linger.ms=1000 acks=all batch.size=1000000 linger.ms=1000

------------------------------------------------

WYŁĄCZENIE KAFKI ORAZ WYCZYSZCZENIE ZAWARTOŚCI
	docker-compose down && docker volume prune -f

------------------------------------------------

#  ███████╗███████╗██╗     
#  ██╔════╝██╔════╝██║     
#  ███████╗███████╗██║     
#  ╚════██║╚════██║██║     
#  ███████║███████║███████╗
#  ╚══════╝╚══════╝╚══════╝


URUCHAMIANIE SSL wykonaj w ../confluent-ops/secure-cluster/scripts/security
	docker container run --rm -it -v $(pwd):/scripts/security -w /scripts/security openjdk:8-jdk-slim ./certs-create.sh

------------------------------------------------

AKTYWOWANIE SSL NA BROKERACH ~/confluent-ops/secure-cluster/docker-compose.yml
a) w zależności czy Kafka jest w Docker czy Linuxie zawartość może trochę się różnić ale trzeba poszukać czy jest wzmianka o SSL i HTTPS np: SSL://kafka-1:9093 (sprawdzić dla Brokerów które będą korzystać z SSL)

KAFKA_ADVERTISED_LISTENERS: SSL://kafka-1:9093,PLAINTEXT://kafka-1:9092
KAFKA_SSL_KEYSTORE_FILENAME: kafka.kafka-1.keystore.jks
KAFKA_SSL_KEYSTORE_CREDENTIALS: kafka-1_keystore_creds
KAFKA_SSL_KEY_CREDENTIALS: kafka_sslkey_creds
KAFKA_SSL_TRUSTSTORE_FILENAME: kafka.kafka-1.truststore.jks
KAFKA_SSL_TRUSTSTORE_CREDENTIALS: kafka-1_truststore_creds
KAFKA_SSL_ENDPOINT_IDENTIFICATION_ALGORITHM: "HTTPS"

b) w tym samym pliku musi być również włączone mapowanie woluminów dla certyfikatów
$PWD/scripts/security:/etc/kafka/secrets

c) uruchom Kafka
cd ~/confluent-ops/secure-cluster && docker-compose up -d

------------------------------------------------

SPRAWDZANIE CZY SSL DZIAŁA NA BROKERZE

root@kafka-1:/# openssl s_client -connect kafka-1:9093 -tls1
root@kafka-1:/# openssl s_client -connect kafka-2:9093 -tls1
itd...

------------------------------------------------

PROBLEM Z CERTYFIKATEM SSL => utwórz konfigurację dla PRODUCER-a do połączenia z Klastrem

utwórz plik producer_ssl.properties w folderze ~/confluent-ops/secure-cluster/scripts/security/ i dodaj:
	security.protocol=SSL
	ssl.truststore.location=/etc/kafka/secrets/kafka.client.truststore.jks
	ssl.truststore.password=mypassword
	ssl.key.password=mypassword

------------------------------------------------

UTWÓRZ ssl-topic z 1 partycją i 3 replikami
	root@kafka-1:/# kafka-topics --zookeeper zk-1:2181 --create --topic ssl-topic --partitions 1 --replication-factor 3

------------------------------------------------

KONSOLA PRODUKCJI dla TOPIC-u ssl-topic
------------------------------------------------
root@kafka-1:/# kafka-console-producer --broker-list kafka-1:9093,kafka-2:9093 --topic ssl-topic --producer.config /etc/kafka/secrets/producer_ssl.properties
------------------------------------------------
Konfiguracja dla CONSUMER-a do połączenia z Klastrem
------------------------------------------------
utwórz plik consumer_ssl.properties w folderze ~/confluent-ops/secure-cluster/scripts/security/ i dodaj:
	security.protocol=SSL
	ssl.truststore.location=/etc/kafka/secrets/kafka.client.truststore.jks
	ssl.truststore.password=mypassword
	ssl.key.password=mypassword
------------------------------------------------
KONSOLA KONSUMPCJI dla TOPIC-u ssl-topic
------------------------------------------------
root@kafka-1:/# kafka-console-consumer --consumer.config /etc/kafka/secrets/consumer_ssl.properties --from-beginning --topic ssl-topic --bootstrap-server kafka-1:9093,kafka-2:9093


#  ██████╗     ██╗    ██╗ █████╗ ██╗   ██╗    ███████╗███████╗██╗     
#  ╚════██╗    ██║    ██║██╔══██╗╚██╗ ██╔╝    ██╔════╝██╔════╝██║     
#   █████╔╝    ██║ █╗ ██║███████║ ╚████╔╝     ███████╗███████╗██║     
#  ██╔═══╝     ██║███╗██║██╔══██║  ╚██╔╝      ╚════██║╚════██║██║     
#  ███████╗    ╚███╔███╔╝██║  ██║   ██║       ███████║███████║███████╗
#  ╚══════╝     ╚══╝╚══╝ ╚═╝  ╚═╝   ╚═╝       ╚══════╝╚══════╝╚══════╝


Enabling Two-Way 2 WAY SSL Authentication
------------------------------------------------
a) Uncomment for all SSL BROKERS ~/confluent-ops/secure-cluster/docker-compose.yml
	KAFKA_SSL_CLIENT_AUTH: "required"
b) restart SSL Brokers
	cd ~/confluent-ops/secure-cluster && docker-compose up -d
------------------------------------------------
URUCHAMIANIE KONSUMPCJI W TRYBIE 2 WAY SSL
------------------------------------------------
a) KONSOLA KONSUMPCJI dla TOPIC-u 2 WAY ssl-topic
	root@kafka-1:/# kafka-console-consumer --consumer.config /etc/kafka/secrets/consumer_ssl.properties --from-beginning --topic ssl-topic --bootstrap-server kafka-1:9093,kafka-2:9093
b) Jeżeli a) się nie udało to SPRAWDŹ CZY JEST W PLIKACH producer_ssl.properties oraz consumer_ssl.properties w folderze ~/confluent-ops/secure-cluster/scripts/security:
	ssl.keystore.location=/etc/kafka/secrets/kafka.client.keystore.jks
	ssl.keystore.password=mypassword
c) ponów komendę a)


#  ███████╗███████╗██╗         ██╗   ██╗███████╗       ███╗   ██╗ ██████╗       ███████╗███████╗██╗     
#  ██╔════╝██╔════╝██║         ██║   ██║██╔════╝       ████╗  ██║██╔═══██╗      ██╔════╝██╔════╝██║     
#  ███████╗███████╗██║         ██║   ██║███████╗       ██╔██╗ ██║██║   ██║█████╗███████╗███████╗██║     
#  ╚════██║╚════██║██║         ╚██╗ ██╔╝╚════██║       ██║╚██╗██║██║   ██║╚════╝╚════██║╚════██║██║     
#  ███████║███████║███████╗     ╚████╔╝ ███████║██╗    ██║ ╚████║╚██████╔╝      ███████║███████║███████╗
#  ╚══════╝╚══════╝╚══════╝      ╚═══╝  ╚══════╝╚═╝    ╚═╝  ╚═══╝ ╚═════╝       ╚══════╝╚══════╝╚══════╝


UTWORZENIE TOPIC-u no-ssl-topic
------------------------------------------------
kafka-topics --zookeeper zk-1:2181 --create --topic no-ssl-topic --partitions 1 --replication-factor 3
------------------------------------------------
#  ┌┬┐┌─┐┌─┐┬┌─┐  ┌┬┐┌─┐┌┬┐┌─┐
#   │ │ │├─┘││     ││├─┤ │ ├─┤
#   ┴ └─┘┴  ┴└─┘  ─┴┘┴ ┴ ┴ ┴ ┴
------------------------------------------------
(NO-SSL) Produce a high rate of data to Topic / generowanie dużej szybkości danych do Topicu no-ssl-topic
------------------------------------------------
	root@kafka-1:/# kafka-producer-perf-test --topic no-ssl-topic --num-records 400000 --record-size 1000 --throughput 1000000 --producer-props bootstrap.servers=kafka-1:9092,kafka-2:9092
------------------------------------------------
(SSL) Produce a high rate of data to Topic / generowanie dużej szybkości danych do Topicu ssl-topic
------------------------------------------------
	kafka-producer-perf-test --topic ssl-topic --num-records 400000 --record-size 1000 --throughput 1000000 --producer-props bootstrap.servers=kafka-1:9093,kafka-2:9093 --producer.config /etc/kafka/secrets/producer_ssl.properties


#   ██████╗ ██████╗ ███╗   ██╗███╗   ██╗███████╗ ██████╗████████╗ ██████╗ ██████╗ ███████╗
#  ██╔════╝██╔═══██╗████╗  ██║████╗  ██║██╔════╝██╔════╝╚══██╔══╝██╔═══██╗██╔══██╗██╔════╝
#  ██║     ██║   ██║██╔██╗ ██║██╔██╗ ██║█████╗  ██║        ██║   ██║   ██║██████╔╝███████╗
#  ██║     ██║   ██║██║╚██╗██║██║╚██╗██║██╔══╝  ██║        ██║   ██║   ██║██╔══██╗╚════██║
#  ╚██████╗╚██████╔╝██║ ╚████║██║ ╚████║███████╗╚██████╗   ██║   ╚██████╔╝██║  ██║███████║
#   ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝  ╚═══╝╚══════╝ ╚═════╝   ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚══════╝
#     ██╗       ██████╗ ██╗██████╗ ███████╗██╗     ██╗███╗   ██╗███████╗
#     ██║       ██╔══██╗██║██╔══██╗██╔════╝██║     ██║████╗  ██║██╔════╝
#  ████████╗    ██████╔╝██║██████╔╝█████╗  ██║     ██║██╔██╗ ██║█████╗  
#  ██╔═██╔═╝    ██╔═══╝ ██║██╔═══╝ ██╔══╝  ██║     ██║██║╚██╗██║██╔══╝  
#  ██████║      ██║     ██║██║     ███████╗███████╗██║██║ ╚████║███████╗
#  ╚═════╝      ╚═╝     ╚═╝╚═╝     ╚══════╝╚══════╝╚═╝╚═╝  ╚═══╝╚══════╝


Create the SQLite | utwórz bazę danych database my.db in the folder ~/confluent-ops/data
------------------------------------------------
	cd ~/confluent-ops && docker container run -it --rm -v $(pwd)/data:/data nouchka/sqlite3 /data/my.db
------------------------------------------------
UTWÓRZ TABELĘ o nazwie years WRAZ Z KILKOMA WPISAMI
------------------------------------------------
	create table years(id INTEGER PRIMARY KEY AUTOINCREMENT, name VARCHAR(50), year INTEGER);
	insert into years(name,year) values('Hamlet',1600);
	insert into years(name,year) values('Julius Caesar',1599);
	insert into years(name,year) values('Macbeth',1605);
------------------------------------------------
WYŚWIETL zawartość tabeli years
------------------------------------------------
sqlite> SELECT * FROM years;
	1|Hamlet|1600
	2|Julius Caesar|1599
	3|Macbeth|1605


docker-compose exec kafka-1 kafka-topics --zookeeper zk-1:2181 --create --topic shakespeare-years --partitions 1 --replication-factor 1

docker-compose exec connect kafka-avro-console-consumer --bootstrap-server kafka-1:9092, kafka-2:9092 --property schema.registry.url=http://schema-registry:8081 --from-beginning --topic shakespeare_years

------------------------------------------------
Add a new sink connector to read data from the Kafka topic  shakespeare-years  and write to the file /data/test.sink.txt  on the Connect worker system
------------------------------------------------
docker-compose exec connect curl -s -X POST -H "Content-Type: application/json" --data '{"name": "File-Sink-Connector","config":{"topics":"shakespeare_years","connector.class":"org.apache.kafka.connect.file.FileStreamSinkConnector","value.converter": "io.confluent.connect.avro.AvroConverter","value.converter.schema.registry.url": "http://schema-registry:8081","file": "/data/test.sink.txt"}}' http://connect:8083/connectors
------------------------------------------------
docker container run -it --rm -v $(pwd)/data:/data nouchka/sqlite3 /data/my.db
------------------------------------------------



#   ██████╗ ██████╗ ██████╗
#  ██╔════╝██╔════╝██╔════╝
#  ██║     ██║     ██║     
#  ██║     ██║     ██║     
#  ╚██████╗╚██████╗╚██████╗
#   ╚═════╝ ╚═════╝ ╚═════╝

                    
CCC > SYSTEM HEALTH > TOPICS > nazwa_topicu > suwakiem do Segment size		#sprawdzanie wielkości danych w topic-u
CCC > SYSTEM HEALTH > TOPICS > nazwa_topicu > View details					#sprawdzanie na jakich partycjach jest topic
CCC > SYSTEM HEALTH > BROKERS > na dole suwak Broker ID > Segment Size		#ilość danych na brokerze
------------------------------------------------
