# honcho

Stack Docker Swarm para hospedar o Honcho localmente e usar como backend de memória do Hermes.

## Arquivos

- `docker-compose.yml`: API Honcho + PostgreSQL/pgvector + Redis.
- `.env`: variáveis locais do stack, com senhas geradas e `LLM_OPENAI_API_KEY` a preencher.
- `.env.example`: template sem segredos.

## Pré-requisitos

O Honcho precisa de um provedor LLM para extração/sumarização/dialectic reasoning. Preencha `LLM_OPENAI_API_KEY` em `.env` antes do deploy, ou adapte as variáveis `*_MODEL_CONFIG__*` para outro provedor/endpoint compatível com OpenAI.

## Deploy

```bash
cd /home/pyu/docker
make deploy-honcho
```

A API fica publicada em `http://127.0.0.1:8000` para o Hermes local. Em Docker Swarm o publish é via ingress; se quiser restringir ao host, controle via firewall/rede do servidor.

## Verificação

```bash
curl http://127.0.0.1:8000/health
docker service logs -f honcho_api
```

## Hermes

Configuração esperada no Hermes:

- `memory.provider=honcho`
- `/home/pyu/.hermes/honcho.json` com `baseUrl: http://127.0.0.1:8000`
