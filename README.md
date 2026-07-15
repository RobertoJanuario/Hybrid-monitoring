# Hybrid Monitoring — Laboratório de Monitoramento Híbrido com Zabbix e AWS

Laboratório prático simulando um cenário de monitoramento híbrido (on-premises + cloud), com uma VM Ubuntu rodando Zabbix 7.0 monitorando uma instância EC2 privada na AWS, conectada via VPN Site-to-Site.

##  Objetivo

Simular um ambiente corporativo real onde um servidor de monitoramento local (on-premises) precisa enxergar recursos hospedados em uma VPC privada na AWS, sem expor a instância à internet — replicando um desafio comum em redes corporativas híbridas.

##  Arquitetura

- **On-premises (simulado):** VM Ubuntu com Zabbix Server 7.0
- **AWS:**
  - VPC com subnet pública e subnet privada
  - EC2 privada (sem IP público), monitorada via agente Zabbix
  - EC2 pública rodando StrongSwan como endpoint de VPN
- **Conectividade:** VPN Site-to-Site entre o ambiente local e a AWS

### O desafio do CGNAT

O ambiente local estava atrás de CGNAT (Carrier-Grade NAT), o que impede a criação direta de uma VPN Site-to-Site tradicional (não há IP público fixo do lado on-premises). A solução foi usar uma **EC2 pública rodando StrongSwan** como endpoint intermediário da VPN, contornando essa limitação e permitindo a comunicação segura entre o Zabbix (local) e a EC2 privada (AWS).

## Tecnologias utilizadas

- Zabbix Server 7.0
- Ubuntu (VM local via VirtualBox)
- AWS EC2 (instância pública e privada)
- AWS VPC (subnets pública e privada)
- StrongSwan (VPN Site-to-Site / IPsec)
- Linux (administração e hardening)

## Estrutura do repositório

```
Hybrid-monitoring/
├── README.md
├── docs/
│   └── teardown-checklist.md       # checklist de desmontagem do ambiente
├── diagrams/
│   └── arquitetura.png             # diagrama da arquitetura completa
├── network/
│   ├── vpc.png
│   ├── vpn.png
│   ├── public-subnet.png
│   ├── private-subnet.png
│   └── ec2-privada.png
└── metrics/
    ├── CPU-AWS.png
    ├── Ec2-AWS.png
    ├── Ec2-aws-metricas.png
    └── Ec2-network-interfaces.png
```



##  Evidências

### Rede e VPN
Configuração da VPC, subnets pública/privada e túnel VPN estabelecido entre o ambiente local e a AWS.

### Métricas de infraestrutura
Utilização de CPU, disco e interfaces de rede da instância EC2 monitorada.

## 🚀 Como reproduzir

1. Provisionar VPC na AWS com subnet pública e privada
2. Subir EC2 pública com StrongSwan configurado como endpoint IPsec
3. Subir EC2 privada (sem IP público) na subnet privada
4. Configurar VM local com Zabbix Server 7.0
5. Estabelecer túnel VPN Site-to-Site entre o ambiente local e a EC2 pública (StrongSwan)
6. Instalar Zabbix Agent na EC2 privada e adicionar o host no Zabbix Server
7. Validar coleta de métricas e criar triggers/alertas


##  Licença

Projeto de estudo pessoal, sem fins comerciais.

##  Autor

**Roberto Januario**
Enterprise Network Analyst | Em transição para Cloud Computing
Certificações: AWS Cloud Practitioner · AWS Solutions Architect Associate · AZ-900

[LinkedIn](#) · [GitHub]([https://github.com/RobertoJanuario](https://www.linkedin.com/in/roberto-januario-a450331b5/)
