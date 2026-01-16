# 🦠 Projet Ransomware Pédagogique

**Module :** Malware et sécurité offensive en Python  
**Objectif :** Comprendre les mécanismes internes d'un ransomware moderne  
**Environnement :** Machine virtuelle dédiée (environnement de laboratoire)

---

## ⚠️ AVERTISSEMENT IMPORTANT

Ce projet est **strictement à but pédagogique**. Il doit être exécuté **UNIQUEMENT** dans un environnement de laboratoire isolé (machine virtuelle dédiée).

**⚠️ DANGER :** L'exécution de ce code en dehors d'un environnement contrôlé peut endommager votre système.

---

## 📁 Structure du projet
```
projet-ransomware/
│
├── crypto_utils.py      # Génération de clé et chiffrement XOR
├── machine_info.py      # Identification de la machine (UUID)
├── file_manager.py      # Gestion des fichiers (parcours, chiffrement)
├── serveur_c2.py        # Serveur Command & Control
├── client_malware.py    # Client malware
├── .gitignore           # Fichiers à ignorer par Git
└── README.md            # Cette documentation
```

---

## ✅ Fonctionnalités implémentées

### Côté Client
- ✅ Génération de clé de chiffrement aléatoire (A-Z, 32 caractères)
- ✅ Identification unique de la machine (UUID depuis `/proc/sys/kernel/random/uuid`)
- ✅ Chiffrement récursif des fichiers (algorithme XOR)
- ✅ Connexion au serveur C2
- ✅ Exfiltration des données (UUID + clé)
- ✅ Exécution de commandes système (sans privilèges admin)
- ✅ Upload de fichiers (client → serveur)
- ✅ Download de fichiers (serveur → client)
- ✅ Chiffrement/Déchiffrement à distance

### Côté Serveur
- ✅ Serveur multi-clients (multi-threading)
- ✅ Stockage des informations clients en mémoire
- ✅ Interface de commande interactive
- ✅ Envoi de commandes aux clients
- ✅ Gestion des uploads/downloads

---

## 🚀 Installation et utilisation

### Prérequis
- Python 3.7+
- Machine Linux (VM recommandée)
- Aucune dépendance externe

### Lancement

**1. Démarrer le serveur C2 :**
```bash
python3 serveur_c2.py
```

**2. Lancer le client malware (dans un autre terminal) :**
```bash
python3 client_malware.py
```

**3. Interagir avec le serveur :**
```bash
# Dans le terminal du serveur
C2> list          # Lister les clients connectés
C2> send          # Envoyer une commande
C2> exit          # Arrêter le serveur
```

---

## 🏗️ Architecture
```
CLIENT (Machine infectée)          SERVEUR C2
       │                                │
       │  1. Connexion TCP              │
       ├───────────────────────────────>│
       │                                │
       │  2. UUID + Clé + Infos         │
       ├───────────────────────────────>│
       │                                │ (Stockage)
       │  3. Confirmation               │
       │<───────────────────────────────┤
       │                                │
       │  4. Commandes                  │
       │<───────────────────────────────┤
       │                                │
       │  5. Résultats                  │
       ├───────────────────────────────>│
```

---

## 🔐 Protocole de communication (JSON)

### Message initial du client
```json
{
  "uuid": "550e8400-e29b-41d4-a716-446655440000",
  "cle": "ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEF",
  "infos": {
    "systeme": "Linux",
    "nom_machine": "vm-test",
    "version": "5.15.0",
    "architecture": "x86_64"
  }
}
```

### Commande d'exécution système
```json
{
  "type": "execute",
  "commande": "whoami"
}
```

### Réponse d'exécution
```json
{
  "type": "execute_response",
  "resultat": {
    "succes": true,
    "sortie": "user\n",
    "erreur": "",
    "code_retour": 0
  }
}
```

---

## ⚠️ Limites et faiblesses (à des fins pédagogiques)

### Points faibles intentionnels
1. **Chiffrement XOR simple** (facilement cassable)
   - Dans un vrai ransomware : AES-256 ou RSA-4096
2. **Clé transmise en clair**
   - Dans un vrai ransomware : Chiffrement asymétrique
3. **Communication non chiffrée**
   - Dans un vrai ransomware : SSL/TLS
4. **Stockage en mémoire**
   - Dans un vrai ransomware : Base de données persistante
5. **Pas de persistance côté client**
   - Dans un vrai ransomware : Modification de cron/systemd
6. **Code non obfusqué**
   - Dans un vrai ransomware : Obfuscation et packing

---

## 📚 Objectifs pédagogiques

Ce projet permet d'apprendre :
1. La programmation réseau (sockets TCP, protocole JSON)
2. Le multi-threading
3. La manipulation de fichiers et répertoires
4. Les algorithmes de chiffrement
5. L'architecture client-serveur
6. Les concepts de sécurité offensive et défensive

---

## 👨‍💻 Auteur

Projet réalisé dans le cadre du module "Malware et sécurité offensive en Python".

---

## 📄 Licence

Ce projet est à usage éducatif uniquement. Toute utilisation malveillante est strictement interdite et illégale.

**Lois applicables :**
- Code pénal français : Article 323-1
- Passible de 2 ans d'emprisonnement et 60 000€ d'amende
