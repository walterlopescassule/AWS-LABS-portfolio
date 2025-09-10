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

![Servidor Web na EC2](./imgem/)

