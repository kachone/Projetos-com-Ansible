ESC, :wq, Ente

# 🚀 Automação de Infraestrutura: Zabbix & Grafana com Ansible

Este repositório contém playbooks Ansible modulares e reutilizáveis para automatizar a implantação, configuração e integração de uma stack completa de monitoramento em servidores **Oracle Linux 9**.

---

## 🏗️ Arquitetura da Stack
A automação provisiona e configura os seguintes componentes:
* **Zabbix Server (v7.0):** Coleta de métricas e motor principal de monitoramento.
* **Banco de Dados MySQL:** Armazenamento relacional otimizado para o Zabbix com suporte a `utf8mb4`.
* **Nginx & PHP-FPM:** Servidor web para a interface gráfica do Zabbix (frontend).
* **Zabbix Agent 2:** Monitoramento ativo do próprio nó de infraestrutura.
* **Grafana:** Painel de visualização avançada com o plugin do Zabbix integrado de forma automática via API.

---

## 📂 Estrutura do Projeto

```text
.
├── inventario.ini          # Inventário de hosts (ignorado pelo git)
├── variaveis.yml           # Variáveis globais e senhas (ignorado pelo git)
├── zabbix_deploy.yaml      # Playbook de instalação e config do Zabbix Server + MySQL + Nginx
├── zabbix_agent.yml        # Playbook de configuração do Zabbix Agent 2
├── grafana.yml             # Playbook de instalação do Grafana, plugin e injeção da API
└── README.md               # Documentação do projeto

	⚙️ Pré-requisitos

    Controlador: Máquina com Ansible instalado.

    Servidor Alvo: Oracle Linux 9 com acesso SSH configurado via chave ou usuário com privilégios de sudo.

    Python: Pacotes básicos de gerenciamento (python3-PyMySQL instalados automaticamente pelos playbooks).

	
	🚀 Como Executar

	1. Clonar o repositório e configurar os arquivos locais

Antes de rodar os playbooks, crie os seus arquivos locais de inventário e variáveis a partir das estruturas necessárias:

    Crie o arquivo inventario.ini:

[zabbix_servers]
seu_ip_ou_host ansible_user=seu_usuario

Crie o arquivo variaveis.yml com as credenciais do banco e da API:

db_user: zabbix
db_password: "sua_senha_segura"
db_name: zabbix
zabbix_server_ip: "ip_do_servidor"
grafana_port: 3000
zabbix_api_url: "http://localhost/api_jsonrpc.php"
zabbix_api_user: "Admin"
zabbix_api_password: "zabbix"

	2. Executar os Playbooks

Execute os comandos abaixo informando o arquivo de inventário e solicitando a senha de become (-K):

Provisionar o Zabbix Server e banco de dados:
Bash

ansible-playbook -i inventario.ini zabbix_deploy.yaml -K

Configurar o Agente Zabbix:
Bash

ansible-playbook -i inventario.ini zabbix_agent.yml -K

Instalar o Grafana e integrar com o Zabbix:
Bash

ansible-playbook -i inventario.ini grafana.yml -K

🔒 Segurança

Por questões de segurança, os arquivos sensíveis contendo credenciais de banco de dados, senhas de API e IPs de servidores (inventario.ini e variaveis.yml) são listados no .gitignore e não são versionados no controle de código fonte.


---

Depois de colar e salvar, basta fazer o envio final para o GitHub:
```bash
git add README.md
git commit -m "docs: adiciona documentacao completa no README.md"
git push origin main
