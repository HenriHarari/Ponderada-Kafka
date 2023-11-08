# Ponderada-Kafka

## Visão Geral do Docker Compose
Este guia oferece uma visão detalhada do arquivo docker-compose disponibilizado para configurar os serviços do Kafka e Zookeeper. Além disso, apresenta um exemplo prático de produção e consumo de mensagens.

Serviços
### 1. Zookeeper:
Imagem: Utiliza a versão mais atualizada do Zookeeper da Bitnami.
Portas: 2181 (porta padrão do Zookeeper).
Variáveis de Ambiente:
ALLOW_ANONYMOUS_LOGIN=yes: Habilita login anônimo no Zookeeper.
### 2. Kafka:
Imagem: Emprega a versão mais recente do Kafka da Bitnami.
Reinicialização: Recomeça em caso de falha do contêiner.
Portas: 9092 (porta padrão do Kafka).
Volumes: Armazena dados persistentes em kafka-data.
Variáveis de Ambiente:
KAFKA_BROKER_ID=1: Identificação única para o broker.
KAFKA_LISTENERS: Endereço/porta de escuta.
KAFKA_ADVERTISED_LISTENERS: Endereço/porta anunciados.
KAFKA_ZOOKEEPER_CONNECT: String de conexão com o Zookeeper.
KAFKA_NUM_PARTITIONS=3: Partições por tópico.
KAFKA_ALLOW_PLAINTEXT_LISTENER=yes: Permite conexões sem criptografia.
Depende de: O Kafka inicializa após o serviço do Zookeeper.
Volumes
kafka-data: Armazena os dados persistentes do Kafka.
Exemplo de Produção e Consumo de Mensagens
Produtor:
O script Python ilustra um produtor Kafka:

Configuração via bootstrap.servers.
Laço de inserção de mensagens pelo usuário.
As mensagens são enviadas para o tópico meu_topico.
Finaliza com sair.
Consumidor:
Exemplo de um consumidor Kafka:

Configura o servidor e define group.id.
Registra-se no tópico meu_topico.
Laço de leitura de mensagens.
Exibe as mensagens recebidas.
Mostra eventuais erros.
Finaliza com CTRL + C.
Instruções para Execução
Execute docker-compose up para iniciar.
Em um novo terminal: python producer.py.
Em outro terminal: python consumer.py.
Comunique-se entre o produtor e o consumidor.
Encerre com CTRL + C e docker-compose down.

``version: "3"``

``services:``
``  meu_zookeeper:``
 ``   image: bitnami/zookeeper:latest``
``    ports:``
``      - 2181:2181``
``    environment:``
 ``     - ALLOW_ANONYMOUS_LOGIN=yes``

 `` meu_kafka:``
   `` image: bitnami/kafka:latest``
``    restart: on-failure``
    ``ports:``
      ``- 9092:9092``
    ``volumes:``
      ``- kafka-dados:/bitnami/kafka``
    ``environment:``
      ``- KAFKA_BROKER_ID=1``
      ``- KAFKA_LISTENERS=PLAINTEXT://:9092``
      ``- KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092``
      ``- KAFKA_ZOOKEEPER_CONNECT=meu_zookeeper:2181``
      ``- KAFKA_NUM_PARTITIONS=3``
      ``- KAFKA_ALLOW_PLAINTEXT_LISTENER=yes``
    ``depends_on:``
      ``- meu_zookeeper``

``volumes:
  ``kafka-dados:``

