# crowdsec-nginx

Protéger son reverse proxy Nginx avec CrowdSec — configuration complète sur Debian/Ubuntu.

> Guide détaillé disponible sur [thadji.fr](https://thadji.fr/proteger-nginx-npm-swag-avec-crowdsec-guide-complet/)

---

## Ce que ça fait

- Installe CrowdSec sur l'hôte et l'entraîne sur les logs Nginx
- Connecte le bouncer Nginx pour bloquer les IPs malveillantes au niveau du reverse proxy
- Compatible SWAG et Nginx Proxy Manager

## Prérequis

- Debian 11/12 ou Ubuntu 22.04+
- Nginx installé (natif ou Docker)
- Accès root

## Installation rapide

```bash
# 1. Installer CrowdSec
curl -s https://packagecloud.io/install/repositories/crowdsec/crowdsec/script.deb.sh | sudo bash
sudo apt install crowdsec -y

# 2. Installer les collections
sudo cscli collections install crowdsecurity/nginx
sudo cscli collections install crowdsecurity/base-http-scenarios
sudo cscli collections install crowdsecurity/http-cve
sudo cscli collections install crowdsecurity/linux

# 3. Installer le bouncer
sudo apt install crowdsec-nginx-bouncer -y
```

## Configuration

### Acquisition des logs

Copiez le fichier selon votre setup :

```bash
# Nginx natif
sudo cp nginx-native.yaml /etc/crowdsec/acquis.d/nginx.yaml

# SWAG (Docker)
sudo cp nginx-swag.yaml /etc/crowdsec/acquis.d/nginx.yaml
# Adaptez le chemin vers vos logs SWAG
```

### Rechargement

```bash
sudo systemctl reload crowdsec
sudo systemctl reload nginx
```

## Vérification

```bash
# Vérifier que les logs sont lus
sudo cscli metrics

# Lister les décisions actives
sudo cscli decisions list

# Lister les bouncers connectés
sudo cscli bouncers list
```

## Structure du repo

```
crowdsec-nginx/
├── nginx-native.yaml    # Nginx installé en natif
├── nginx-swag.yaml      # SWAG en Docker
└── README.md
```

## Licence

MIT — libre de réutilisation, lien vers [thadji.fr](https://thadji.fr) apprécié.
