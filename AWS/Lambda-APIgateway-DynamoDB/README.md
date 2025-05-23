# 📚 Documentação Cloud Computing: AWS Lambda + API Gateway + Dynamo DB

## 🔧 Objetivo
Construir uma API para cadastrar e consultar treinos de usuários utilizando os serviços da AWS:
- **DynamoDB** para armazenamento
- **AWS Lambda** como backend serverless
- **API Gateway** para expor os endpoints `POST /workouts` e `GET /workouts`

---

## 🗃️ Etapa 1 – Configuração do DynamoDB

### ✅ Tabela Criada: `Workouts`

**Estrutura da tabela:**
- **Partition key**: `id` (string, UUID)
- Campos adicionais:
  - `user_id` (string)
  - `data` (string, ISO 8601)
  - `tipo` (string)
  - `exercicios` (lista de objetos com `nome`, `carga`, `reps`)

> ⚠️ Observação: o campo `id` é único por treino, permitindo múltiplos treinos por usuário.

---

## 🌐 Etapa 2 – Configuração do API Gateway

Inicialmente foi utilizada uma **REST API**, mas diversos desafios surgiram. A solução foi migrar para uma **HTTP API**, muito mais simples e eficiente.

### ❌ Desafios com REST API:

1. **Interface complicada**: configuração das rotas, integrações e estágios era confusa e cheia de etapas manuais.
2. **Deploy manual necessário**: alterações exigiam "Deploy to Stage" para surtirem efeito.
3. **Problemas com payload**: a função Lambda não recebia o `body` da requisição corretamente, a menos que fosse feita uma integração com mapping templates ou ativado o "Lambda Proxy Integration".
4. **Roteamento confuso**: `routeKey` vinha com `/dev/rota` e gerava problemas no roteamento dentro da função.

### ✅ Solução com HTTP API:

- Criação direta da API com apenas dois endpoints:
  - `GET /workouts`
  - `POST /workouts`
- Uso do **default stage (`$default`)** que já gera uma URL limpa, sem a necessidade de criar `/dev`, `/v1`, etc.
- **Lambda Proxy Integration** ativado por padrão, permitindo que o payload chegasse corretamente no `event['body']`.

> Resultado: API configurada em poucos minutos com funcionamento 100%.

---

## ⚙️ Etapa 3 – Função Lambda

### 🧠 Linguagem utilizada:
- Python 3.12

### 📌 Estrutura da função:

```python
import json
import boto3
import uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
tabela = dynamodb.Table('Workouts')

def lambda_handler(event, context):
    try:
        route_key = event['requestContext']['routeKey']

        if route_key == 'POST /workouts':
            body = json.loads(event['body'])

            treino = {
                'id': str(uuid.uuid4()),
                'user_id': body['user_id'],
                'data': body.get('data') or datetime.utcnow().isoformat(),
                'tipo': body['tipo'],
                'exercicios': body['exercicios']
            }

            tabela.put_item(Item=treino)

            return {
                'statusCode': 201,
                'body': json.dumps({'message': 'Treino salvo com sucesso!', 'treino': treino})
            }

        elif route_key == 'GET /workouts':
            response = tabela.scan()
            return {
                'statusCode': 200,
                'body': json.dumps(response.get('Items', []), default=str)
            }

        else:
            return {
                'statusCode': 404,
                'body': json.dumps({'mensagem': 'Rota não encontrada'})
            }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'erro': str(e)})
        }
```

### 🔁 Testes realizados via Postman:

**POST**  
- Endpoint: `POST /workouts`
- Headers: `Content-Type: application/json`
- Body:
```json
{
  "user_id": "pedro123",
  "tipo": "upper",
  "exercicios": [
    {
      "nome": "Supino",
      "carga": 100,
      "reps": 10
    }
  ]
}
```

**GET**
- Endpoint: `GET /workouts`
- Retorna todos os treinos cadastrados.

---

## 💡 Aprendizados

- **Evitar REST API do API Gateway** para casos simples: embora poderosa, sua configuração é burocrática e facilmente causa erros se mal configurada.
- **HTTP API é ideal** para projetos com Lambda, principalmente quando o objetivo é agilidade e simplicidade.
- **Lambda Proxy Integration é fundamental** para que o `event['body']` receba corretamente o payload da requisição.
- **Organização de rotas via `routeKey`** é essencial para identificar o método e o caminho e executar a lógica correta.

---

## ✅ Resultado final

API 100% funcional, leve, escalável e sem necessidade de servidor, utilizando os principais serviços da AWS:

- DynamoDB para persistência
- Lambda para lógica de negócio
- HTTP API Gateway para exposição da API
