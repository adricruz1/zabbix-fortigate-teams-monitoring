# 🛡️ Zabbix 7.0 & FortiGate: Observabilidade de Redes & Automação de Alertas no Microsoft Teams

![Zabbix](https://img.shields.io/badge/Zabbix-7.0-RED?style=for-the-badge&logo=zabbix)
![Fortinet](https://img.shields.io/badge/Fortinet-FortiGate-EE3124?style=for-the-badge&logo=fortinet)
![Microsoft Teams](https://img.shields.io/badge/Microsoft_Teams-Webhook-6264A7?style=for-the-badge&logo=microsoftteams)
![SNMP](https://img.shields.io/badge/Protocol-SNMP%20v2c%2Fv3%20%26%20Traps-blue?style=for-the-badge)

Projeto prático de observabilidade e monitoramento contínuo para infraestrutura crítica de rede empresarial (Firewalls FortiGate, Switches L2/L3 Core e Acesso, Access Points e Impressoras), integrado com sistema de alertas em tempo real no Microsoft Teams via Webhook e Power Automate.

---

## 📐 Arquitetura da Solução

```mermaid
graph TD
    subgraph "Ambiente de Rede (Unidade Consolação)"
        FW[Firewall FortiGate 200E]
        SW_CORE[Switches Core / Stack]
        SW_ACC[Switches de Acesso L2]
        AP[Access Points Wi-Fi]
        IMP[Impressoras de Rede]
        CAM[Câmeras IP / CFTV]
        
        SW_ACC --> AP
        SW_ACC --> IMP
        SW_ACC --> CAM
        SW_CORE --> SW_ACC
        FW --> SW_CORE
    end

    subgraph "Camada de Observabilidade"
        ZBX[Zabbix Proxy / Server]
    end

    subgraph "Notificação & Incident Response"
        PA[Power Automate / Webhook]
        TEAMS[Microsoft Teams - #Alertas-NOC]
    end

    FW -- "SNMP Traps" --> ZBX
    FW -- "SNMP Polling" --> ZBX
    SW_CORE -- "SNMP Polling" --> ZBX
    SW_ACC -- "SNMP Polling" --> ZBX

    ZBX -- "Payload JSON" --> PA
    PA -- "Adaptive Cards" --> TEAMS
