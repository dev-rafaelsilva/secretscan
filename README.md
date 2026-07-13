# secretscan

CLI em Go que escaneia repositórios em busca de segredos expostos — chaves de API, senhas, tokens — tanto no código atual quanto no **histórico de commits**, onde a maioria das ferramentas não olha.
🔍 Escaneando .
[ALTO]  .env:2                  → Stripe API Key detectado
[MEDIO] docker-compose.yml:4    → Generic API Key detectado
[ALTO]  src/config.js:2         → AWS Access Key detectado
⚠️  .gitignore: 4 padrão(ões) sensível(is) ausente(s): [.env *.pem *.key *.p12]
3 problema(s) encontrado(s) (2 alto, 1 médio)

## Por que isso importa

Apagar um arquivo com uma chave exposta **não remove ela do histórico do Git**. Qualquer pessoa com acesso ao repositório ainda pode encontrar essa chave rodando `git log -p`. O `secretscan history` existe exatamente pra pegar esse ponto cego.

## Instalação

```bash
go install github.com/dev-rafaelsilva/secretscan@latest
```

Ou clone e compile localmente:

```bash
git clone https://github.com/dev-rafaelsilva/secretscan.git
cd secretscan
go build -o secretscan .
```

## Uso

```bash
secretscan run .            # escaneia o diretório atual
secretscan history           # escaneia todo o histórico de commits
secretscan init              # gera um .secretscan.yml de configuração
secretscan                   # abre o menu interativo
```

### Modo estrito e CI/CD

```bash
secretscan run . --strict
```

Retorna exit code `1` se encontrar qualquer achado — inclusive severidade baixa. Isso permite travar um pipeline automaticamente:

```yaml
# .github/workflows/secretscan.yml
- name: Scan for secrets
  run: |
    go install github.com/dev-rafaelsilva/secretscan@latest
    secretscan run . --strict
```

## O que ele detecta

- Chaves de acesso e secret keys da AWS
- Chaves de API da Stripe
- Tokens de acesso do GitHub
- Tokens JWT
- Chaves privadas RSA
- Senhas hardcoded em variáveis
- Chaves de API genéricas
- Arquivos sensíveis (`.env`, `*.pem`, `*.key`, `*.p12`) ausentes do `.gitignore`

## Status

🚧 Em desenvolvimento ativo. Feedback e issues são muito bem-vindos.
