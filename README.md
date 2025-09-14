# AWS-LABS-portfolio
Laboratórios práticos realizados em AWS
# 🚀 Laboratório 1: AWS EC2 - Meu Primeiro Servidor Web

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

![Image](https://github.com/user-attachments/assets/ff665e7d-b699-4e00-a9c0-cc9dcd8bf26e)

---

# Laboratório 2: AWS Budgets – Controle de Custos

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
![Image](https://github.com/user-attachments/assets/f49c5d0a-dbf8-4572-a27b-450ac0bdcce2)

---

<h1>🚀 Laboratório 3: AWS Lambda + IAM + EventBridge</h1>
<p>Este laboratório demonstra como <strong>automatizar o encerramento de instâncias EC2</strong> usando três serviços da AWS: <strong>IAM (segurança), Lambda (automação)</strong> e <strong>EventBridge (gatilhos/eventos)</strong>.</p>

![Image](https://github.com/user-attachments/assets/99d1c3ca-5a39-4dd9-be44-ddf063188a18)
![Image](https://github.com/user-attachments/assets/a02342e4-1f07-48c5-8d8a-0f74421ca668)



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

---

# 📬 Laboratório 4: AWS – SNS + SQS + DLQ

Este projeto demonstra o uso do **Amazon SNS** como barramento de eventos, integrado a uma **fila SQS principal** com suporte a **Dead-Letter Queue (DLQ)**.  
O objetivo é simular um fluxo de mensagens resiliente em uma arquitetura baseada em microsserviços.

---

## 🎯 Objetivos

- Criar uma **fila SQS DLQ** (Dead-Letter Queue)  
- Criar uma **fila SQS principal** configurada para redirecionar mensagens para a DLQ  
- Criar um **tópico SNS** e associá-lo à fila SQS principal  
- Testar a publicação de mensagens via SNS e verificar o consumo pela SQS  
- Simular falhas de processamento para redirecionar mensagens à DLQ  

---

## 🏗️ Arquitetura

![Image](https://github.com/user-attachments/assets/96b7e0a4-6f9f-4339-b47b-dd84eef1aefd)
---

## ⚙️ Passo a Passo

1. **Criação das filas SQS**  
   - Criar a fila **DLQ**  
   - Criar a fila **principal** e configurar a DLQ como destino de mensagens falhas  

2. **Criação do tópico SNS**  
   - Criar um tópico padrão  
   - Copiar o ARN para uso posterior  

3. **Assinatura da SQS no SNS**  
   - Inscrever a fila principal no tópico SNS  
   - Ajustar a **Policy da SQS** para permitir envio do SNS  

4. **Teste de fluxo normal**  
   - Publicar mensagem no **SNS**  
   - Mensagem deve chegar na **fila principal (SQS)**  

5. **Teste de falha (DLQ)**  
   - Não processar a mensagem (deixar expirar tempo de visibilidade)  
   - Após o número máximo de tentativas (ex: 3), a mensagem é enviada para a **DLQ**
  
 ![Image](https://github.com/user-attachments/assets/58d069db-2868-4e4f-a218-5006592fe2aa)
![Image](https://github.com/user-attachments/assets/05efd60f-56e6-4b86-8e6c-c1155fec1340)  
![Image](https://github.com/user-attachments/assets/048bed24-d229-42fe-97ab-15b1ab32686b)

---

## 🧪 Exemplo de Mensagem Publicada

![Image](https://github.com/user-attachments/assets/9eb50a72-4f20-461e-8d80-195d657d9778)


