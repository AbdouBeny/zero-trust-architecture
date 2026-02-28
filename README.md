# Zero Trust Architecture

Ce projet implémente une **architecture Zero Trust complète** pour sécuriser un système d'information d'entreprise. Il démontre les principes fondamentaux du modèle Zero Trust : **"Ne jamais faire confiance, toujours vérifier"**.

### Objectifs du projet

- Mettre en œuvre une authentification centralisée avec SSO et MFA
- Implémenter un contrôle d'accès basé sur les rôles (RBAC)
- Déployer une segmentation réseau stricte (VLANs)
- Configurer un point d'entrée unique via Gateway
- Intégrer un système de supervision et traçabilité (SIEM)
- Documenter et tester l'architecture complète

---

## Architecture

### Segmentation réseau

| VLAN | Réseau | Description | VMs |
|------|--------|-------------|-----|
| Management | 192.168.56.0/24 | Gestion et administration | Toutes |
| DMZ | 10.10.10.0/24 | Zone démilitarisée (clients) | VM3, VM4 |
| Apps | 10.10.20.0/24 | Applications métier | VM2, VM3 |

---

## Fonctionnalités

### Sécurité

- **Authentification forte** : SSO avec Keycloak (OAuth2/OIDC)
- **Zero Trust Gateway** : Point d'entrée unique avec OAuth2 Proxy
- **Firewall iptables** : Segmentation stricte entre VLANs
- **Blocage d'accès direct** : Applications accessibles uniquement via Gateway
- **RBAC** : Contrôle d'accès basé sur les rôles (employee, admin)
- **SIEM** : Monitoring et détection d'intrusion avec Wazuh

### Infrastructure

- **Infrastructure as Code** : Vagrant + Ansible
- **Reproductible** : Déploiement automatisé en quelques commandes
- **Modulaire** : Architecture en composants indépendants
- **Optimisé** : Fonctionne sur laptop avec 8GB RAM

---

## Installation

### 1. Cloner le repository

```bash
git clone https://github.com/abdoubeny/zero-trust-architecture.git
cd zero-trust-architecture
```

### 2. Vérifier les prérequis

```bash
# Vérifier VirtualBox
vboxmanage --version

# Vérifier Vagrant
vagrant --version

# Vérifier Ansible
ansible --version
```

### 3. Déploiement complet

#### Option A : Déploiement automatique (toutes les VMs)

```bash
vagrant up
```

### 4. Configuration initiale de Keycloak

Une fois VM1 démarrée, accédez à Keycloak :

```
http://192.168.56.10:8080
```

**Identifiants admin :**
- Username : `admin`
- Password : `admin123`
---

## 💻 Utilisation

### Accès à l'application via Zero Trust Gateway

#### Depuis le client interne (VM4)

```bash
vagrant ssh vm4

# Démarrer l'interface graphique
startx
```

Dans Firefox sur VM4 :
1. Ouvrir `http://10.10.10.10`
2. OAuth2 Proxy redirige vers Keycloak
3. Se connecter avec les identifiants
4. Accès au dashboard de l'application


### Tests de sécurité

#### Vérifier le principe Zero Trust

```bash
vagrant ssh vm4

# Test 1 : Accès via Gateway (DOIT fonctionner)
curl http://10.10.10.10

# Test 2 : Accès direct à l'app (DOIT échouer - bloqué par firewall)
curl http://10.10.20.10:5000
# → Timeout (Zero Trust activé )

```

### Accès au SIEM Wazuh

```
https://192.168.56.60:443
```

---


## Auteur

**A.beny**
- LinkedIn: [Votre Profil](https://www.linkedin.com/in/abderezak-benyoucef/)
