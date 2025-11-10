# 🚀 Projeto: Infraestrutura AWS Elástica com Auto Scaling

Este projeto demonstra a implementação de uma arquitetura web robusta, altamente disponível e escalável na AWS. A infraestrutura se ajusta automaticamente à demanda (picos de tráfego) e distribui a carga de forma eficiente, com a configuração inicial feita via **AWS CLI**.

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ![Amazon EC2](https://img.shields.io/badge/Amazon%20EC2-F29100?style=for-the-badge&logo=amazon-ec2&logoColor=white) ![AWS CLI](https://img.shields.io/badge/AWS%20CLI-232F3E?style=for-the-badge&logo=aws-cli&logoColor=white) ![Elastic Load Balancing](https://img.shields.io/badge/Elastic%20Load%20Balancing-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)

---

## 🏛️ Arquitetura Final

A arquitetura final utiliza múltiplas Zonas de Disponibilidade (Multi-AZ) para garantir alta disponibilidade. O tráfego é direcionado para um Application Load Balancer, que o distribui entre instâncias EC2 em sub-redes privadas. O Auto Scaling Group monitora e ajusta a capacidade automaticamente.

![Diagrama da Arquitetura Final](arquitetura-final.png.png)

---

## 🛠️ Tecnologias e Serviços Utilizados

* **AWS CLI:** Usada para criar e configurar a instância EC2 base (`WebServer`).
* **Amazon EC2 (t3.micro):** Servidores virtuais onde a aplicação web é executada.
* **Amazon Machine Image (AMI):** Uma imagem personalizada (`WebServerAMI`) foi criada para garantir que todas as novas instâncias sejam idênticas.
* **Application Load Balancer (ELB):** Distribui o tráfego HTTP de entrada entre as instâncias ativas em múltiplas Zonas de Disponibilidade.
* **Auto Scaling Group (ASG):** Gerencia o ciclo de vida das instâncias, garantindo a quantidade correta em execução.
* **Launch Template:** Define o "molde" (`web-app-launch-template`) para as novas instâncias (AMI, tipo de instância, etc.).
* **AWS VPC:** A infraestrutura foi provisionada em uma VPC customizada (`Lab VPC`), com sub-redes públicas (para o ELB) e privadas (para as instâncias).
* **Security Groups:** Filtra o tráfego, permitindo acesso HTTP apenas através do Load Balancer (`HTTPAccess`).

---

## ⚙️ Etapas da Implementação

1.  **Criação da Instância Base:** Lançamento de uma instância EC2 (`WebServer`) via AWS CLI, usando um script `UserData` para instalar um servidor web (httpd, php) e uma app de teste de estresse.
2.  **Criação da AMI Personalizada:** Uma AMI (`WebServerAMI`) foi criada a partir da instância base para servir como "imagem dourada".
3.  **Configuração do Load Balancer:** Criação de um Application Load Balancer (`WebServerELB`) voltado para a internet e um Target Group (`webserver-app`) para monitorar a saúde das instâncias no caminho `/index.php`.
4.  **Criação do Auto Scaling:**
    * Um **Launch Template** (`web-app-launch-template`) foi criado usando a `WebServerAMI`.
    * Um **Auto Scaling Group** (`Web App Auto Scaling Group`) foi configurado para usar o template e manter a aplicação escalável.

---

## 📈 Política de Escalabilidade e Teste

O poder do projeto foi validado pela sua política de escalabilidade e um teste de estresse.

### Política de Auto Scaling:
* **Tipo:** Target Tracking (Rastreamento de meta)
* **Métrica:** Utilização Média de CPU
* **Valor Alvo:** 50%

### Configuração de Capacidade:
* **Mínimo:** 2 instâncias
* **Desejado:** 2 instâncias
* **Máximo:** 4 instâncias

### Validação (Teste de Estresse):
* **Ação:** O teste "Start Stress" foi ativado via DNS do Load Balancer.
* **Reação:** O script aumentou o uso da CPU, e o Auto Scaling Group detectou que a média ultrapassou a meta de 50%.
* **Resultado (Scale-Out):** O ASG automaticamente provisionou novas instâncias (até o máximo de 4) para lidar com a demanda, provando a elasticidade da arquitetura.

---

## 📄 Documentação Original

* **[LAB 175: Usando Auto Scaling na AWS (Linux).pdf](LAB%20175_%20Usando%20Auto%20Scaling%20na%20AWS%20(Linux).pdf)**
    * Este é o documento PDF que documenta o laboratório que serviu como base para este projeto.
