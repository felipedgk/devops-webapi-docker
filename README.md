# Laboratório Docker

Aluno: Felipe Camargo do Nascimento — RA 22504535 — Turma A

API ASP.NET Core (.NET 10) em container, com versionamento de imagens, staging/production, health check e rollback.

## Construção
docker build -t devops-webapi:1.0.0 .

## Execução
docker run -d --name devops-api -p 8080:8080 devops-webapi:1.0.0

## Staging
docker run -d --name devops-api-staging -p 8081:8080 -e APP_ENVIRONMENT=staging -e APP_VERSION=1.1.0 devops-webapi:1.1.0

## Production
docker run -d --name devops-api-prod -p 8082:8080 -e APP_ENVIRONMENT=production -e APP_VERSION=1.1.0 devops-webapi:1.1.0

## Health Check
curl -f http://localhost:8081/health   (Windows: curl.exe)

## Nova versão
docker build -t devops-webapi:1.1.0 .

## Falha controlada
docker build -t devops-webapi:1.2.0 .   (endpoint /health retorna 500 de propósito)

## Rollback
docker rm -f devops-api-prod
docker run -d --name devops-api-prod -p 8082:8080 -e APP_ENVIRONMENT=production -e APP_VERSION=1.1.0 devops-webapi:1.1.0

## Docker Compose
docker compose up -d
docker compose ps
docker compose down
