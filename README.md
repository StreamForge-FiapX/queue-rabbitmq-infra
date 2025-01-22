# StreamForge - Infraestrutura RabbitMQ no Kubernetes

Este repositório contém os arquivos de configuração necessários para implantar o RabbitMQ em um cluster Kubernetes com auto-scaling.

## Componentes

### RabbitMQ Deployment
- Implanta um pod RabbitMQ com interface de gerenciamento
- Credenciais gerenciadas via AWS Secrets Manager
- Imagem: rabbitmq:3-management
- Portas expostas:
  - 5672: AMQP
  - 15672: Interface de Gerenciamento

### Service
- Expõe o RabbitMQ para outros serviços
- Tipo: NodePort
- Portas mapeadas:
  - 5672 -> 5672 (AMQP)
  - 15672 -> 30001 (Interface Web)

### HPA (Horizontal Pod Autoscaler)
- Escalonamento automático baseado em CPU
- Mínimo: 1 réplica
- Máximo: 10 réplicas
- Meta de utilização de CPU: 80%

## Pipeline CI/CD

O deploy é automatizado via GitHub Actions e inclui:

1. Configuração de credenciais AWS
2. Configuração do kubectl para o cluster EKS
3. Recuperação de secrets do AWS Secrets Manager
4. Criação de secrets no Kubernetes
5. Deploy dos manifestos Kubernetes

## Pré-requisitos
- Cluster EKS configurado
- Secrets configurados no AWS Secrets Manager
- Credenciais AWS com permissões adequadas

## Acesso
- Interface de gerenciamento: http://[NODE-IP]:30001
- Credenciais: Gerenciadas via AWS Secrets Manager
