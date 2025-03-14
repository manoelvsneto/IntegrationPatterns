"# IntegrationPatterns" 

| Categoria                                  | Padrão/Abordagem                          | Quando Utilizar                                                                 | Como Utilizar                                                                                                        | Ferramentas / Tecnologias                                                                                       |
|--------------------------------------------|-------------------------------------------|---------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| **Integração de Sistemas (Sincrônica)**    | APIs (REST, SOAP, gRPC)                   | Comunicação que exige resposta imediata                                        | Expor serviços via API Gateway, SOA ou gRPC. Pode ser implementada de forma tradicional ou serverless                 | Spring Boot, Microsoft WCF, Kong, Apigee, gRPC frameworks; AWS Lambda, Azure Functions (Serverless)            |
| **Integração de Sistemas (Assíncrona)**     | Mensageria (Filas/Tópicos)                | Comunicação desacoplada sem resposta imediata                                   | Produtores enviam mensagens para filas/tópicos e consumidores processam de forma independente                         | Apache Kafka, RabbitMQ, Google Pub/Sub                                                                          |
| **Integração de Sistemas (Assíncrona)**     | Arquitetura Orientada a Eventos (EDA)       | Quando é necessário manter histórico de eventos e promover reatividade         | Armazenar e processar eventos para rastrear alterações e permitir reações em cadeia                                   | Axon Framework, EventStoreDB                                                                                      |
| **Integração de Sistemas (Notificação)**    | Webhook                                   | Notificar clientes ou sistemas imediatamente após um evento                    | Configurar callbacks para envio de notificações assim que os eventos ocorrem                                          | Frameworks customizados; pode ser usado com AWS Lambda (Serverless)                                             |
| **Integração de Sistemas (Notificação)**    | Polling                                   | Quando o cliente precisa verificar periodicamente se há atualizações           | Cliente consulta periodicamente um endpoint para obter atualizações                                                 | Endpoints com suporte a polling; implementações customizadas                                                     |
| **Integração de Sistemas (Notificação)**    | Push Notification                         | Enviar notificações em tempo real aos clientes                                 | Servidor envia notificações diretamente para os clientes, utilizando serviços de mensageria push                        | Firebase Cloud Messaging, Apple Push Notification Service                                                       |
| **Integração de Sistemas (Fluxo)**          | Orquestração                              | Quando é necessário um controle centralizado do fluxo de processos             | Um orquestrador coordena a sequência de chamadas entre os serviços                                                  | Camunda, IBM Integration Bus                                                                                      |
| **Integração de Sistemas (Fluxo)**          | Coreografia                               | Quando os serviços devem agir de forma autônoma, reagindo a eventos sem centralização | Cada serviço reage aos eventos de forma independente, sem um coordenador central                                    | Apache Kafka (com microserviços), padrões de eventos                                                              |
| **Integração de Dados (Batch ETL)**         | ETL                                       | Para cargas periódicas e consolidação para Data Warehousing                     | Processos periódicos de extração, transformação e carga em lote                                                      | Informatica PowerCenter, Talend, Microsoft SSIS, Spring Batch, Apache NiFi                                        |
| **Integração de Dados**                      | Transmissão por Arquivos (File Transfer)    | Quando a troca de dados ocorre via arquivos, de forma programada ou agendada     | Transferência programada de arquivos entre sistemas                                                                  | FTP, SFTP, Azure Data Factory (File-based), AWS Data Pipeline                                                     |
| **Integração de Dados (Tempo Real/Big Data)**| ELT e Arquitetura Medallion               | Para grandes volumes de dados ou atualização contínua                          | Organizar dados em camadas: <br>- Bronze: dados brutos<br>- Silver: limpeza e transformação<br>- Gold: dados prontos para consumo | Apache Spark, Databricks, AWS S3, Azure Data Lake, Snowflake                                                      |
| **Integração de Dados (Streaming)**         | Processamento de Streaming                | Para processamento contínuo e análise em tempo real                            | Processar dados em fluxo contínuo, possibilitando insights imediatos                                                 | Apache Flink, Apache Kafka Streams                                                                                |
| **Integração de Dados**                      | CDC (Change Data Capture)                 | Capturar alterações de dados em tempo real sem sobrecarregar o sistema           | Capturar alterações via logs ou triggers e propagar as mudanças para os sistemas consumidores                           | Debezium, Oracle GoldenGate, SQL Server CDC                                                                       |


```mermaid
graph TD
    A[Início: Escolha o Tipo de Integração]
    A --> B{Integração de SISTEMAS ou DADOS?}
    
    %% Integração de SISTEMAS
    B -- "SISTEMAS" --> C{Sincrônica ou Assíncrona?}
    
    %% Sincrônica
    C -- "Sincrono" --> D[Utilizar APIs 'REST, SOAP, gRPC']
    D --> D_A[Deseja usar Serverless?]
    D_A -- "Sim" --> D_B[Utilizar Serverless API]
    D_B --> D_B1[Como utilizar: Implementar funções sem servidor para expor APIs]
    D_B1 --> D_B2[Sugestões: AWS Lambda, Azure Functions]
    D_A -- "Não" --> D_C[Como utilizar: Expor serviços via API Gateway, SOA ou gRPC]
    D_C --> D_C1[Sugestões: Spring Boot, Microsoft WCF, Kong, Apigee, gRPC frameworks]
    
    %% Assíncrona
    C -- "Assíncrono" --> E{Utiliza Mensageria?}
    
    E -- "Sim" --> F[Utilizar Filas/Tópicos]
    F --> F1[Como utilizar: Produtores enviam mensagens; consumidores processam de forma independente]
    F1 --> F2[Sugestões: Apache Kafka, RabbitMQ, Google Pub/Sub]
    
    E -- "Não / Baseada em Eventos" --> G[Utilizar Arquitetura Orientada a Eventos 'EDA']
    G --> G1[Como utilizar: Armazenar eventos para histórico e reatividade]
    G1 --> G2[Sugestões: Axon Framework, EventStoreDB]
    
    %% Mecanismos de Notificação Adicionais para Assíncrono
    F --> H{Necessita Notificação ao Consumidor?}
    H -- "Sim" --> I{Escolha o Mecanismo}
    I -- "Webhook" --> I1[Utilizar Webhooks]
    I1 --> I2[Como utilizar: Configurar callbacks para notificar clientes quando eventos ocorrem]
    I1 --> I3[Sugestões: Frameworks customizados, AWS Lambda 'Serverless']
    I -- "Polling" --> J[Utilizar Polling]
    J --> J1[Como utilizar: Cliente periodicamente verifica atualizações no servidor]
    J1 --> J2[Sugestões: Endpoints com suporte a polling, implementações customizadas]
    I -- "Push Notification" --> K[Utilizar Push Notification]
    K --> K1[Como utilizar: Servidor envia notificações em tempo real aos clientes]
    K1 --> K2[Sugestões: Firebase Cloud Messaging, Apple Push Notification Service]
    
    %% Decisão: Orquestração vs Coreografia para Sistemas Assíncronos
    F --> L{Necessita de controle centralizado do fluxo?}
    L -- "Sim" --> M[Utilizar Orquestração]
    M --> M1[Como utilizar: Um orquestrador coordena a sequência de chamadas]
    M1 --> M2[Sugestões: Camunda, IBM Integration Bus]
    L -- "Não" --> N[Utilizar Coreografia]
    N --> N1[Como utilizar: Cada serviço reage autonomamente a eventos]
    N1 --> N2[Sugestões: Apache Kafka 'com microserviços', padrões de eventos]
    
    %% Integração de DADOS
    B -- "DADOS" --> O{Atualização em Tempo Real ou Batch?}
    
    %% Processamento Batch
    O -- "Batch" --> P[Utilizar ETL]
    P --> P1[Como utilizar: Processos periódicos de extração, transformação e carga]
    P1 --> P2[Sugestões: Informatica PowerCenter, Talend, Microsoft SSIS]
    P1 --> P3[Sugestões Adicionais: Spring Batch, Apache NiFi]
    
    %% Transmissão por Arquivos (File Transfer)
    O -- "Transmissão por Arquivos" --> U[Utilizar File Transfer]
    U --> U1[Como utilizar: Transferência programada de arquivos entre sistemas]
    U1 --> U2[Sugestões: FTP, SFTP, Azure Data Factory 'File-based', AWS Data Pipeline]
    
    %% Processamento em Tempo Real / Streaming
    O -- "Tempo Real/Grandes Volumes" --> Q[Utilizar ELT e Arquitetura Medallion]
    Q --> Q1[Como utilizar: 
      - Bronze: Armazenar dados brutos  
      - Silver: Limpeza e transformação  
      - Gold: Dados refinados para consumo]
    Q1 --> Q2[Sugestões: Apache Spark, Databricks, AWS S3, Azure Data Lake, Snowflake]
    
    %% Streaming de Dados (Processamento Contínuo)
    O -- "Streaming de Dados" --> T[Utilizar Processamento de Stream]
    T --> T1[Como utilizar: Processar dados em tempo real com fluxo contínuo]
    T1 --> T2[Sugestões: Apache Flink, Apache Kafka Streams]
    
    %% CDC para Dados em Tempo Real
    Q --> R{Necessita Captura de Alterações?}
    R -- "Sim" --> S[Utilizar CDC 'Change Data Capture']
    S --> S1[Como utilizar: Capturar alterações via logs ou triggers]
    S1 --> S2[Sugestões: Debezium, Oracle GoldenGate, SQL Server CDC]

```