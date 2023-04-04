# Projeto DynamoDB AWS :nut_and_bolt::wrench:

###  Descrição: Features Relacionais (SQL) e Não Relacionais (NoSQL) usando o mesmo banco de dados? Isso é possível? Com o DynamoDB sim! Entenda um pouco das possibilidades desse banco de dados totalmente gerenciado da AWS. Para isso, nosso super expert apresenta uma série de boas práticas para o DynamoDB.

## DynamoDB :pencil:

  O Amazon DynamoDB é um banco de dados NoSQL de valor-chave, totalmente gerenciado e sem servidor, projetado para executar aplicativos de alto desempenho em qualquer escala. O DynamoDB oferece segurança integrada, backups contínuos, replicação automatizada de várias regiões, cache na memória e ferramentas de importação e exportação de dados.
  
  ## AWS :floppy_disk:
  
  Amazon Web Services, também conhecido como AWS, é uma plataforma de serviços de computação em nuvem, que formam uma plataforma de computação na nuvem oferecida pela Amazon.com. Os serviços são oferecidos em várias áreas geográficas distribuídas pelo mundo.
  
 ## Comandos Utilizados no desafio:
 
 * Criando uma tabela:
 
 ```
 aws dynamodb create-table \
    --table-name NomeDaTebela \
    --attribute-definitions \
        AttributeName=NomeDoAtributo,AttributeType=S \
        AttributeName=NomeDoAtributo,AttributeType=S \
    --key-schema \
        AttributeName=NomeDoAtributo,KeyType=HASH \
        AttributeName=NomeDoAtributo,KeyType=RANGE \
    --provisioned-throughput \
        ReadCapacityUnits=10,WriteCapacityUnits=5
 ```
 
 * Inserindo um item:
 
```
aws dynamodb put-item \
    --table-name Music \
    --item file://itemmusic.json \
```
 
* Inserindo múltiplos itens:

```
aws dynamodb batch-write-item \
    --request-items file://batchmusic.json
```

* Criar um index global secundário:

```
aws dynamodb update-table \
    --table-name Music \
    --attribute-definitions\
        AttributeName=SongTitle,AttributeType=S \
        AttributeName=SongYear,AttributeType=S \
    --global-secondary-index-updates \
        "[{\"Create\":{\"IndexName\": \"SongTitleYear-index\",\"KeySchema\":[{\"AttributeName\":\"SongTitle\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"SongYear\",\"KeyType\":\"RANGE\"}], \
        \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
```

* Pesquisar:

```
aws dynamodb query \
    --table-name Music \
    --key-condition-expression "Artist = :artist and SongTitle = :title" \
    --expression-attribute-values file://keyconditions.json
```

* Pesquisa pelo index secundário:

```
aws dynamodb query \
    --table-name Music \
    --index-name AlbumTitle-index \
    --key-condition-expression "AlbumTitle = :name" \
    --expression-attribute-values  '{":name":{"S":"Fear of the Dark"}}'
```

