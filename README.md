# Um *pipeline* para detecção de fraudes em transações bancárias usando Apache Flink
**Projeto criado para fazer uma implementação em scala de um dos exercícios do curso [ Apache Flink | A Real Time & Hands-On course on Flink ](https://www.udemy.com/course/apache-flink-a-real-time-hands-on-course-on-flink/). Foram adicionados algumas features a mais como filtro pela quantidade de followers, inserção no elasticsearch e filtro para tratar o json de resposta da API. Na busca de Tweets são procurar tweets com as palavras "Bolsonaro" e "Moro" devido a acontecimentos recentes em nosso cenário político.**

---
#### To see this ReadMe in english [click here](https://github.com/thiagobeppe/flinkFraudDetection/blob/master/READMEEN.md) :us:
---
# Tecnologias
* Scala
* Apache Flink
* Apache Kafka
* Docker
* Shell

---
# Recursos necessários
* Java 11 
* [Docker](https://www.docker.com/) e [Docker-Compose](https://docs.docker.com/compose/install/)
* [Scala](https://www.scala-lang.org/download/) (2.12.11)
* [SBT](https://www.scala-sbt.org/download.html) (1.3.10)

---
# Importante
* Garanta que não há nenhum serviço rodando nas portas 2181 e 9092 em seu computador, caso contrário o docker não conseguirá subir as imagens, para contonar isto pode-se mudar a porta onde o docker entra na aplicação [1].
---

# Estrutura do projeto
```
├── analisysResult
├── datasets
│   ├── alarmed_cust.txt
│   ├── bank_data.txt
│   └── lost_cards.txt
├── docker-compose.yml
├── fraudDetection
│   ├── build.sbt
│   └── src
│       ├── main
│       │   └── scala
│       │       └── com
│       │           └── github
│       │               └── example
│       │                   ├── filters
│       │                   │   ├── alarmedCustomCheck.scala
│       │                   │   ├── cityChangeCheck.scala
│       │                   │   ├── excessiveTransactionCheck.scala
│       │                   │   └── lostCardsCheck.scala
│       │                   ├── fraudDetection.scala
│       │                   └── kafka
│       │                       └── producerKafka.scala
│       └── test
│           └── scala
└── logs
    └── data
```
* **analisysResult**  - *pasta responsável por guardar as análises ao final da execução do arquivo*
* **datasets**  - *arquivos com os dados utilizados para a análise*
* **docker-compose.yml**  - *arquivo com as imagens dockers para a aplicação*
* **fraudDetection/../filters**  - *todos os filtros utilizados para fazer a detecção de fraude de acordo com as regras de negócio*
* **fraudDetection/../kafka**  - *produtor kafka para ingetar os dados como se fosse uma API*
* **logs/data**  - *pasta onde ficam inseridos os logs do apache Kafka*

---

# Como executar (Linux - Ubuntu)
* Em seu terminal, navegue até a raíz do projeto e execute o comando "```./run.sh```" para começar a execução do projeto.
* Após o fim da execução do projeto será gerado um arquivo de log dentro da pasta  ```analisysResult```, para testar outras configurações pode-se só mexer nos parametros dos filtros
* Para finalizar a execução do programa feche os terminais que abriram com o ```run.sh``` e na pasta raíz do projeto, abra o terminal e execute ```docker-compose down --volumes```.


---
[1] Na tag ```ports```, o valor da esquerda é qual porta do seu computador você irá liberar para o docker mapear.
```
zookeeper:
    container_name: zookeeper
    image: wurstmeister/zookeeper
    restart: "always"
    ports:
      -  "2181:2181"
```