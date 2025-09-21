# 🚀 Automação do Amazon Forecast com AWS CloudFormation

> Este guia mostra como implantar uma infraestrutura automatizada para previsões de séries temporais com **AWS CloudFormation**, baseada no tutorial oficial da AWS.

---

## 📑 Sumário

1. [Visão geral](#-visão-geral)
2. [Arquitetura (resumo)](#-arquitetura-resumo)
3. [Pré-requisitos](#-pré-requisitos)
4. [Obter o template](#-obter-o-template)
5. [Deploy — Console](#-deploy--console-passo-a-passo)
6. [Deploy — AWS CLI](#-deploy--aws-cli-exemplo)
7. [Verificar e usar a pipeline](#-verificar-e-usar-a-pipeline-pós-deploy)
8. [Limpeza / remoção](#-limpeza--remoção)
9. [Custos e permissões](#-custos-e-permissões)
10. [Troubleshooting](#-dicas-de-troubleshooting)
11. [Referências](#-referências)

---

## 🔎 Visão geral

Este projeto guia a implantação de uma infraestrutura que provisiona:

* Buckets **S3** para dados de demonstração;
* Recursos do **Amazon Forecast** (datasets, grupos, previsores);
* **Step Functions + Lambda** para orquestrar o pipeline;
* **SNS** para notificações;
* Integração opcional com **QuickSight/SageMaker**.

O fluxo é baseado no dataset de **NYC Taxi** e pode ser adaptado para outros cenários.

---

## 🏗 Arquitetura (resumo)

* **Amazon S3** — dados de treino, relacionados e resultados.
* **IAM** — roles/policies para acesso aos serviços.
* **Step Functions + Lambda** — pipeline de importação → treinamento → previsão.
* **Amazon Forecast** — datasets, import jobs, predictors, forecasts.
* **Amazon SNS** — notificações de conclusão.
* **Opcional** — QuickSight e SageMaker para visualização.

---

## ✅ Pré-requisitos

* Conta AWS ativa com permissões **CloudFormation + IAM + Forecast**.
* AWS CLI v2 configurado (`aws configure`) se usar via CLI.
* Região AWS compatível (veja documentação oficial).
* Atenção: serviços **geram custos** (Forecast, Step Functions, Lambda, etc.).

---

## 📥 Obter o template

* Use o **Template URL oficial** fornecido pela AWS no tutorial.
* Para customização, copie o arquivo `.yaml` para este repositório em `templates/`.

---

## 🖥 Deploy — Console (passo a passo)

1. Acesse **CloudFormation** no Console AWS.
2. Clique em **Create stack → With new resources (standard)**.
3. Em *Template*, selecione **S3 template URL** e cole o link oficial.
4. Defina o **Stack name** (ex.: `forecast-automation-demo`).
5. Preencha parâmetros (ex.: email para notificações).
6. Marque permissões para criar recursos IAM.
7. Clique em **Create stack** e aguarde até `CREATE_COMPLETE`.

---

## 💻 Deploy — AWS CLI (exemplo)

```bash
aws cloudformation create-stack \
  --stack-name forecast-automation-demo \
  --template-url "<TEMPLATE_URL>" \
  --parameters ParameterKey=Email,ParameterValue="you@example.com" \
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
  --region <REGION>

# Monitorar progresso
aws cloudformation describe-stacks --stack-name forecast-automation-demo --region <REGION>

# Excluir stack
aws cloudformation delete-stack --stack-name forecast-automation-demo --region <REGION>
```

---

## 🔍 Verificar e usar a pipeline (pós-deploy)

* Confirme status `CREATE_COMPLETE` no CloudFormation.
* Verifique no **S3** se os dados de demonstração foram carregados.
* No **Forecast**, veja o dataset group criado.
* Monitore execução no **Step Functions**.
* Confira logs no **CloudWatch**.
* Receba alertas via **SNS** (se configurado).

---

## 🧹 Limpeza / remoção

```bash
aws cloudformation delete-stack --stack-name forecast-automation-demo --region <REGION>
```

> Nota: alguns buckets podem ser preservados pelo template. Revise a política de *retention*.

---

## 💰 Custos e permissões

* Custos: Forecast, Step Functions, Lambda, QuickSight.
* Permissões: `cloudformation:*`, `forecast:*`, `iam:CreateRole`, `s3:*`, `states:*`, `sns:*`.

---

## 🛠 Dicas de troubleshooting

* **CREATE\_IN\_PROGRESS parado** → verifique *Events* e CloudWatch.
* **AccessDenied** → confirme permissões IAM.
* **Dados ausentes no Forecast** → revise permissões de leitura no bucket S3.

---

## 📚 Referências

* [Tutorial oficial da AWS — CloudFormation com Forecast](https://docs.aws.amazon.com/pt_br/forecast/latest/dg/tutorial-cloudformation.html)
* [AWS Solutions — Improving Forecast Accuracy with ML](https://aws.amazon.com/solutions/implementations/improving-forecast-accuracy-with-ml/)

---




