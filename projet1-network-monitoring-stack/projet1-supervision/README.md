# 📡 Network Monitoring Stack — Prometheus / Grafana / Alertmanager

Stack de supervision réseau complète, déployée via **Docker Compose** et provisionnée automatiquement avec **Ansible**.  
Conçue pour monitorer la disponibilité et les performances d'une infrastructure réseau multi-hôtes.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                   SUPERVISION STACK                  │
│                                                     │
│  ┌─────────────┐    ┌──────────────┐               │
│  │ Prometheus  │───▶│   Grafana    │ :3000          │
│  │   :9090     │    │  Dashboards  │               │
│  └──────┬──────┘    └──────────────┘               │
│         │                                           │
│         ├──── Node Exporter     (métriques système) │
│         ├──── SNMP Exporter     (métriques réseau)  │
│         └──── Blackbox Exporter (disponibilité)     │
│                                                     │
│  ┌──────────────┐                                   │
│  │ Alertmanager │ :9093  ──▶  Email / Slack         │
│  └──────────────┘                                   │
└─────────────────────────────────────────────────────┘
```

---

## 🚀 Déploiement rapide

### Prérequis
- Docker & Docker Compose
- Ansible 2.9+
- Python 3.8+

### 1. Cloner le dépôt
```bash
git clone https://github.com/mohanebdr/network-monitoring-stack.git
cd network-monitoring-stack
```

### 2. Configurer les variables
```bash
cp ansible/vars/vault.example.yml ansible/vars/vault.yml
# Éditer vault.yml avec vos informations (email SMTP, etc.)
ansible-vault encrypt ansible/vars/vault.yml
```

### 3. Déployer avec Ansible
```bash
ansible-playbook -i ansible/inventory.ini ansible/deploy.yml --ask-vault-pass
```

### 4. Ou lancer directement avec Docker Compose
```bash
cd docker/
docker-compose up -d
```

---

## 📁 Structure du projet

```
network-monitoring-stack/
├── ansible/
│   ├── deploy.yml              # Playbook principal
│   ├── inventory.ini           # Inventaire des hôtes
│   ├── vars/
│   │   └── vault.example.yml   # Template des variables sensibles
│   └── roles/
│       ├── prometheus/         # Installation et configuration Prometheus
│       ├── grafana/            # Installation et dashboards
│       └── alertmanager/       # Règles d'alerting
├── docker/
│   ├── docker-compose.yml      # Stack complète
│   └── .env.example            # Variables d'environnement
├── dashboards/
│   └── network-overview.json   # Dashboard Grafana exporté
├── docs/
│   └── architecture.md         # Documentation technique
└── README.md
```

---

## 📊 Métriques supervisées

| Métrique | Source | Description |
|----------|--------|-------------|
| CPU / RAM / Disque | Node Exporter | Ressources système des hôtes |
| Latence réseau | Blackbox Exporter | Ping ICMP vers cibles définies |
| Disponibilité services | Blackbox Exporter | HTTP/TCP checks |
| Interfaces réseau | SNMP Exporter | Trafic entrant/sortant (octets/s) |
| Erreurs réseau | SNMP Exporter | CRC errors, drops sur interfaces |

---

## 🔔 Alertes configurées

- **HostDown** — Un hôte ne répond plus depuis > 1 min
- **HighLatency** — Latence ICMP > 100ms sur 5 min
- **InterfaceErrors** — Taux d'erreurs réseau > 1%
- **DiskFull** — Utilisation disque > 85%
- **ServiceDown** — Un service HTTP ne répond plus

---

## 🛠️ Technologies

![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=grafana&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat&logo=ansible&logoColor=white)

---

## 📄 Licence

MIT — Projet personnel à des fins d'apprentissage.
