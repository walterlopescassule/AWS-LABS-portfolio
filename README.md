# AWS-LABS-portfolio
Laboratórios práticos realizados em AWS
# 🚀 Laboratório AWS EC2 - Meu Primeiro Servidor Web

Este repositório documenta meu primeiro laboratório prático como **AWS Developer**, onde configurei e hospedei uma aplicação simples utilizando **Amazon EC2**.

---

## 🔹 Objetivo
Criar e configurar um servidor web na **AWS EC2** utilizando **Amazon Linux 2023**, garantindo acesso via navegador e automatizando a instalação do Apache com *User Data*.

---

## 🛠️ Passos Realizados

1. **Criação da instância EC2**  
   - Console da AWS  
   - AMI: *Amazon Linux 2023*  
   - Tipo de instância: `t2.micro (Free Tier)`  

2. **Configuração de segurança**  
   - Par de chaves RSA (`.pem`) para acesso seguro  
   - **Security Group** liberando tráfego:  
     - Porta 22 (SSH)  
     - Porta 80 (HTTP)  

3. **Automatização com User Data**  
   Durante a criação da instância, utilizei o seguinte script *bash*:  

   ```bash
   #!/bin/bash
   dnf install -y httpd
   systemctl enable --now httpd
   echo '<html><h1>Olá, seu servidor web!</h1></html>' > /var/www/html/index.html

   ## 📸 Resultado

Ao acessar o **IP público** da instância EC2 no navegador, obtive a seguinte página:

![Servidor EC2](.images/servidor-ec2.jpg)

---

# Laboratório: AWS Budgets – Controle de Custos

## 🎯 Objetivo
Configurar um orçamento simples para controlar gastos na conta AWS e receber alertas por e-mail.

## 🛠️ Serviços Utilizados
- AWS Budgets
- AWS CloudWatch (alertas)

## 📋 Passos Realizados
1. Acessar AWS Budgets no console.
2. Criar orçamento de **US$ 10**.
3. Configurar alerta para **10% de uso**.
4. Testar notificação automática.

## ✅ Resultado
- Alerta recebido por e-mail ao atingir o limite configurado.
- Controle proativo de custos habilitado.

## 📸 Prints
![Exemplo do orçamento criado](./screenshot-budget.png)

---

# 🚀 Laboratório AWS Lambda + IAM + EventBridge

Este laboratório demonstra como **automatizar o encerramento de instâncias EC2** usando três serviços da AWS: **IAM (segurança), Lambda (automação)** e **EventBridge (gatilhos/eventos)**.

---

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Laboratório AWS Lambda + IAM + EventBridge</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
            background-color: #f9f9f9;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        pre {
            background-color: #ecf0f1;
            padding: 15px;
            border-radius: 5px;
            overflow-x: auto;
        }
        code {
            font-family: monospace;
        }
        .section {
            margin-bottom: 40px;
        }
        img {
            max-width: 100%;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
    </style>
</head>
<body>

<h1>🚀 Laboratório AWS Lambda + IAM + EventBridge</h1>
<p>Este laboratório demonstra como <strong>automatizar o encerramento de instâncias EC2</strong> usando três serviços da AWS: <strong>IAM (segurança), Lambda (automação)</strong> e <strong>EventBridge (gatilhos/eventos)</strong>.</p>

<div class="section">
    <h2>🔹 Etapas do Laboratório</h2>

    <h3>1. Criar Política IAM</h3>
    <p>Acesse <strong>IAM → Políticas → Criar política</strong>. Escolha o editor JSON e cole o código:</p>
    <pre><code>{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogStream",
        "logs:CreateLogGroup",
        "logs:PutLogEvents",
        "ec2:DescribeInstances",
        "ec2:TerminateInstances",
        "ec2:DescribeRegions"
      ],
      "Resource": "*"
    }
  ]
}</code></pre>
    <p>Nome da política: <strong>PoliticaTerminarEC2-WalterCassule</strong> → Criar ✅</p>

    <h3>2. Criar Função (Role) no IAM</h3>
    <ul>
        <li>IAM → Funções → Criar função</li>
        <li>Tipo de entidade confiável: <strong>Serviço da AWS → Lambda</strong></li>
        <li>Anexe a política criada <strong>PoliticaTerminarEC2-WalterCassule</strong></li>
        <li>Nome da função: <strong>RoleTerminarEC2-WalterCassule</strong></li>
        <li>Criar ✅</li>
    </ul>

    <h3>3. Criar Função Lambda</h3>
    <ul>
        <li>Acesse AWS Lambda → Criar função → Criar do zero</li>
        <li>Nome: <strong>LambdaTerminarEC2-WalterCassule</strong></li>
        <li>Runtime: <strong>Python 3.9</strong></li>
        <li>Role existente: <strong>RoleTerminarEC2-WalterCassule</strong></li>
    </ul>
    <p>Código Python:</p>
    <pre><code>import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    response = ec2.describe_instances(
        Filters=[{"Name": "instance-state-name", "Values": ["running"]}]
    )
    
    instances_to_terminate = []
    for reservation in response["Reservations"]:
        for instance in reservation["Instances"]:
            instances_to_terminate.append(instance["InstanceId"])
    
    if instances_to_terminate:
        ec2.terminate_instances(InstanceIds=instances_to_terminate)
        return f"Encerradas instâncias: {instances_to_terminate}"
    else:
        return "Nenhuma instância em execução encontrada."</code></pre>
    <p>Deploy ✅ | Timeout: 10s | Handler: <strong>Terminator.lambda_handler</strong></p>

    <h3>4. Configurar EventBridge (gatilho)</h3>
    <ul>
        <li>Na função Lambda, clique em <strong>Adicionar gatilho</strong></li>
        <li>Escolha <strong>EventBridge (CloudWatch Events)</strong></li>
        <li>Criar nova regra:
            <ul>
                <li>Nome: <strong>GatilhoTerminarEC2-WalterCassule</strong></li>
                <li>Tipo: Expressão de agendamento (ex: <code>rate(5 minutes)</code>)</li>
            </ul>
        </li>
        <li>Adicionar ✅</li>
    </ul>

    <h3>5. Testar o Laboratório</h3>
    <ol>
        <li>Crie uma instância EC2 de teste</li>
        <li>Aguarde o tempo definido no EventBridge</li>
        <li>A função Lambda deve encerrar a instância automaticamente</li>
    </ol>

    <h3>6. Finalizar</h3>
    <p>Para evitar cobranças, delete:</p>
    <ul>
        <li>Função Lambda</li>
        <li>Role IAM</li>
        <li>Política IAM</li>
        <li>Regra EventBridge</li>
    </ul>
</div>

<div class="section">
    <h2>📸 Exemplo de Resultado</h2>
    <p>Você pode adicionar uma imagem do fluxo do laboratório:</p>
    <img src="./images/lambda-lab.jpg" alt="Fluxo Lambda + EventBridge + EC2">
</div>

<div class="section">
    <h2>✍️ Aprendizados</h2>
    <ul>
        <li>Automatização de recursos AWS com Lambda</li>
        <li>Gestão de permissões usando IAM</li>
        <li>Criação de gatilhos/eventos com EventBridge</li>
        <li>Boas práticas de limpeza de recursos para evitar custos</li>
    </ul>
</div>

</body>
</html>



