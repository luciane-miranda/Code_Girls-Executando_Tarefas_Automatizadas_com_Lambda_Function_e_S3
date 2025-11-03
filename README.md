# Code_Girls-Executando_Tarefas_Automatizadas_com_Lambda_Function_e_S3
Desafio do Curso da DIO: Santander Code Girls - 2025, Módulo Executando Tarefas Automatizadas com Lambda Function e S3

---

## Objetivo do Desafio

Demonstrar o conhecimento sobre:
- Aplicar os conceitos aprendidos em um ambiente prático; 
- Documentar processos técnicos de forma clara e estruturada;
- Utilizar o GitHub como ferramenta para compartilhamento de documentação técnica.
- Demonstrar como usar infraestrutura como código para provisionar um ponto de acesso do S3 Object Lambda que invoca uma função Lambda, permitindo transformar ou processar objetos do S3 automaticamente, com os seguintes objetivos:
  - Provisionar automaticamente recursos (bucket S3, roles IAM, função Lambda, ponto de acesso) via CloudFormation. 
  - Integrar o bucket S3 e a função Lambda para que os objetos possam ser processados ou modificados quando acessados.
  - Habilitar monitoramento (via CloudWatch) e boas práticas de segurança na arquitetura. 
  - Criar um template repetível e versionável, alinhado com práticas de Produto/TI para operação e manutenção.

---

## Principais Etapas Realizadas

### 1. Criação do Template CloudFormation  
- Usei formato YAML (ou JSON) para definir recursos como:  
  - Bucket S3 (e possivelmente ponto de acesso de suporte)  
  - Função AWS Lambda (com código base para transformação ou “pass-through”)  
  - Access Point S3 Object Lambda, que conecta o bucket + Lambda. 
  - Roles IAM necessárias para que a função Lambda e o ponto de acesso possam operar com segurança.  
- Parâmetros opcionais definidos: por exemplo `CreateNewSupportingAccessPoint=true`, `LambdaFunctionPayload="format=json"`, `EnableCloudWatchMonitoring=true`.

### 2. Deploy da Stack  
- Utilizei o console da AWS ou AWS CLI: `aws cloudformation deploy --template-file … --stack-name … --parameter-overrides …` conforme instrução. 
- Acompanhei o progresso da stack via guia “Events” até o status `CREATE_COMPLETE`.  
- Verifiquei os recursos provisionados no console (S3 bucket, função Lambda, access point).

### 3. Verificação e Testes  
- Usei o ARN do ponto de acesso do S3 Object Lambda como parâmetro `--bucket` em comando `aws s3api get-object` para acessar objetos via o access point.   
- Testei se objetos no bucket eram retornados corretamente — no template padrão a função Lambda **não transforma** os objetos, apenas os encaminha. 
- Em seguida, modifiquei o código da Lambda para aplicar transformação ou lógica personalizada, como modificação de cabeçalhos, novo status code ou processamento de range/part-number. 

### 4. Monitoramento e Boas Práticas  
- Habilitei o monitoramento do Amazon CloudWatch para métricas de solicitações ao access point, criando alarmes para erros no cliente/servidor. 
- Avaliei os custos potenciais associados ao CloudWatch e à simultaneidade provisionada da função Lambda (se usada).

---

## Aprendizados e Insights
- O S3 permite o armazenamento de qualquer tipo de arquivo (texto, vídeo, aúdio, etc).
- O Lambda Executa códigos em resposta a eventos, tem escalabilidade automática e tem custo eficiente (de acordo com tempo de execução e quantidade de solicitações).
- O Localstack permite que trabalhemos localmente simulando o ambiente da AWS.
- 

---

> *Desenvolvido por Luciane Silva de Miranda como parte do desafio de projeto Executando Tarefas Automatizadas com Lambda Function e S3*



