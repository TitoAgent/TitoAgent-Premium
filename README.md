# 🤖 TitoAgent TitoAgent — Premium Edition 

> **Agent IA local et cloud pour développeurs** — Un assistant puissant, extensible et multi-modèles pour une productivité maximale.

---

## 📋 Table des matières

- [Vue d'ensemble](#-vue-densemble)
- [✨ Fonctionnalités principales](#-fonctionnalités-principales)
- [🚀 Installation](#-installation)
- [⚙️ Configuration](#️-configuration)
- [💬 Guide des commandes](#-guide-des-commandes)
- [🔀 Système de skills](#-système-de-skills)
- [🧠 Mémoire et RAG](#-mémoire-et-rag)
- [🗂️ Architecture du projet](#️-architecture-du-projet)
- [🔒 Sécurité](#-sécurité)
- [📊 Providers supportés](#-providers-supportés)
- [📄 Licence](#-licence)

---

## 🎯 Vue d'ensemble

**TitoAgent** est un agent IA conversationnel conçu pour les développeurs. Il combine la puissance du raisonnement multi-étapes (ReAct), une mémoire vectorielle persistante (ChromaDB), et le support de multiples providers LLM — qu'ils soient locaux (Ollama, llama.cpp, LM Studio) ou cloud (Claude, Gemini, OpenAI).

### Cas d'usage
- 🔍 Analyse de code et débogage
- 📚 Documentation et RAG (Retrieval-Augmented Generation)
- 🎨 Développement avec vision par IA
- 💾 Gestion de projets avec mémoire persistante
- 🌐 Recherche web intégrée et extraction YouTube
- ⚡ Automatisation via skills extensibles

---

## ✨ Fonctionnalités principales

### 🧠 Intelligence avancée

| Fonctionnalité | Description |
|---|---|
| **Boucle ReAct** | Raisonnement multi-étapes autonome avec accès à des outils |
| **Streaming temps réel** | Les réponses s'affichent token par token pour une interaction fluide |
| **Mémoire vectorielle** | ChromaDB persistant — l'agent se souvient entre les sessions même après redémarrage |
| **RAG complet** | Indexe et interroge ton code/documents avec recherche sémantique |
| **Vision multimodale** | Analyse d'images en local ou via APIs cloud |
| **Compactage de session** | Résume la conversation et la stocke pour libérer la mémoire |

### 🔌 Multi-providers LLM

Bascule **à chaud** entre différents providers via `/model-menu` — pas de redémarrage nécessaire.

| Provider | Type | Modèles disponibles |
|---|---|---|
| **llama.cpp** | Local | Serveur OpenAI-compatible sur port 8080 |
| **Ollama** | Local | Tous les modèles Ollama (Mistral, Llama, Dolphin…) |
| **LM Studio** | Local | Détection automatique sur port 1234 |
| **Gemini** | Cloud | Google AI Studio (tier gratuit disponible) |
| **Claude** | Cloud | Sonnet 4, Haiku, Opus — Anthropic API |
| **OpenAI** | Cloud | GPT-4o, GPT-4o-mini, GPT-4-turbo |

### 📂 Système de fichiers complet

Opérations natives : **read**, **write**, **append**, **delete**, **move**, **copy**, **mkdir**, **grep**, **find**, **diff**.
- 📸 **Snapshots automatiques** — restaure facilement avec `/undo`
- 🔄 **Historique complet** — suivi des modifications

### 🔀 Skills extensibles

Ajoute des capacités **sans toucher au code core** — un simple fichier Python dans `skills/` suffit.

**Skills inclus** :
- **Git** — `/git status`, `/git commit`, `/git push`, `/git log`, `/git branch`

### 🌐 Web et YouTube

- 🔍 **Recherche web** via DuckDuckGo avec profils (tech, hardware, actualité, science, finance, santé)
- 🎬 **YouTube intégré** — recherche de vidéos + extraction automatique de sous-titres
- 🤖 **Détection intelligente** — le LLM décide si une requête est web ou YouTube

### 🖼️ Vision multimodale

Analyse d'images avec :
- **Local** : llama.cpp (Gemma 3, Qwen2-VL…) ou Ollama (llava, moondream…)
- **Cloud** : Claude, Gemini, OpenAI

---

## 🚀 Installation

### Prérequis

- **Python 3.10+**
- GPU recommandé (NVIDIA/AMD/Apple Silicon) pour usage local
- **Ollama**, **llama.cpp**, ou **LM Studio** pour les modèles locaux
- Clés API (Gemini/Claude/OpenAI) pour usage cloud

### Étape 1 : Cloner et installer les dépendances

```bash
# Cloner le dépôt
git clone <repo-url>
cd TitoAgent

# Installer les dépendances
pip install -r requirements.txt

# Pour Arch Linux / CachyOS
pip install -r requirements.txt --break-system-packages
```

### Dépendances principales

| Package | Rôle |
|---|---|
| `chromadb` | Mémoire vectorielle persistante |
| `rich` | Interface terminal colorée et élégante |
| `ollama` | Client Ollama |
| `ddgs` | Recherche web DuckDuckGo |
| `requests` | Appels HTTP / API |
| `Pillow` | Traitement d'images (vision) |
| `pdfplumber` | Lecture de PDFs |
| `python-docx` | Lecture de fichiers Word |
| `tiktoken` | Comptage de tokens |
| `PyJWT` | Authentification / licences |
| `supabase` | Backend cloud (optionnel) |
| `yt-dlp` | Transcription YouTube |
| `beautifulsoup4` | Parsing HTML |

### Étape 2 : Lancer l'agent

```bash
python3 titoagent.py
```

**Au premier lancement**, le **Setup Wizard** se déclenche automatiquement et :
- ✅ Détecte ton GPU (NVIDIA, AMD, Apple Silicon)
- ✅ Scanne tes modèles installés (Ollama, llama.cpp, LM Studio)
- ✅ Génère une configuration optimale pour ton système

---

## ⚙️ Configuration

### 🧙 Setup Wizard (recommandé)

Le wizard est lancé automatiquement si `config.json` est absent ou incomplet. Pour le relancer manuellement :

```bash
# Depuis l'agent
/model-menu
```

**Le wizard détecte automatiquement :**
- 🖥️ **GPU** — NVIDIA (`nvidia-smi`), AMD (`rocm-smi`), Apple Silicon
- 🦙 **Ollama** — modèles installés et disponibles
- 🔧 **llama.cpp** — binaires et fichiers `.gguf`
- 🎮 **LM Studio** — instance active sur `localhost:1234`

### 📝 Configuration manuelle (config.json)

```json
{
  "main_model": {
    "backend": "ollama",
    "name": "mistral:latest",
    "host": "127.0.0.1",
    "port": 11434,
    "temperature": 0.7,
    "max_tokens": 8192
  },
  "embed_model": {
    "name": "nomic-embed-text:latest",
    "provider": "ollama",
    "host": "127.0.0.1",
    "port": 11434
  },
  "vision_model": {
    "backend": "gemini",
    "max_tokens": 1024
  }
}
```

### 🔐 Gestion des clés API

Les clés API sont **jamais** dans `config.json`. Elles sont stockées de façon sécurisée :

```bash
# Menu interactif pour ajouter/supprimer des clés
/model-menu
# → Sélectionner "Clés API"
```

**Stockage** :
- 📁 `~/.titoagent/api_keys.json` (permissions `600`)
- Variables d'environnement supportées : `ANTHROPIC_API_KEY`, `GEMINI_API_KEY`, `OPENAI_API_KEY`
- Les variables d'environnement ont **priorité** sur le fichier

### 👤 Profil utilisateur (tito_profile.json)

Généré automatiquement au premier lancement. Stocke tes préférences locales :

```json
{
  "username": "Tito",
  "projects_dir": "/home/tito/Documents",
  "language": "fr",
  "os": "CachyOS (Arch Linux)",
  "python": "python3.10",
  "gpu": "NVIDIA GeForce RTX 3090 (24576 MiB)",
  "ollama": true
}
```

---

## 💬 Guide des commandes

### 📂 Commandes fichiers

| Commande | Description |
|---|---|
| `/ls [path]` | Lister un dossier |
| `/read [file]` | Lire un fichier (avec coloration syntaxique) |
| `/diff [file]` | Diff vs dernier snapshot |
| `/undo [file]` | Restaurer un snapshot antérieur |
| `/grep [path] [pattern]` | Chercher dans les fichiers (expression régulière) |

### ⚡ Commandes d'exécution

| Commande | Description |
|---|---|
| `/run [path]` | Lancer un projet avec auto-repair en cas d'erreur |
| `/debug [erreur]` | Analyser une erreur détaillée |
| `/plan [tâche]` | Planifier avant d'agir (questions interactives) |
| `/sandbox` | Afficher le statut du sandbox |

### 🤖 Modèles et vision

| Commande | Description |
|---|---|
| `/model-menu` | Menu interactif complet (switcher providers, gérer clés API, tests) |
| `/vision [image] [prompt]` | Analyser une image avec la vision IA |

### 🧠 Mémoire et RAG

| Commande | Description |
|---|---|
| `/rag-menu` | Menu RAG complet — indexer dossiers, chatter avec, statistiques |
| `/compact` | Résumer la session + stockage ChromaDB (libère mémoire) |
| `/memory [question]` | Recherche sémantique dans tout l'historique |
| `/checkpoint` | Sauvegarder la conversation actuelle |
| `/checkpoint load [path]` | Recharger un checkpoint |
| `/checkpoint list` | Lister tous les checkpoints |
| `/status` | Afficher les stats ChromaDB |

### 🌐 Web et YouTube

| Commande | Description |
|---|---|
| `/search [requête]` | Recherche web (DuckDuckGo) — détection auto YouTube |
| `web_search` | Le LLM peut appeler web_search avec `mode: "youtube"` |

### 🔀 Skills

| Commande | Description |
|---|---|
| `/skills` | Voir tous les skills chargés |
| `/git [sous-commande]` | Git direct : `status`, `commit`, `push`, `log`, `branch`, `stash`… |

#### Exemples Git
```bash
/git status              # État du dépôt
/git commit "msg"        # Commit tous les changements
/git push                # Push vers origin
/git log 5               # 5 derniers commits
/git branch feature      # Créer une branche "feature"
/git stash pop           # Appliquer le dernier stash
```

### 🛠️ Commandes système

| Commande | Description |
|---|---|
| `/config` | Afficher la configuration brute (JSON) |
| `/clear` | Vider le contexte de la conversation |
| `/history` | Afficher les 20 dernières actions |
| `/reset` | Réinitialiser le profil utilisateur |
| `/?` | Aide complète (catégorisée) |
| `/exit` | Quitter l'agent |

---

## 🔀 Système de skills

Les **skills** ajoutent des capacités au LLM sans modifier le code core. Un skill = un fichier Python dans `skills/`.

### Skills inclus

#### 🔗 Git Skill (`skills/git.py`)

Commandes Git intégrées :
```bash
/git status          # État du dépôt
/git commit "msg"    # Commit
/git push            # Push
/git log 5           # 5 derniers commits
/git branch feature  # Créer une branche
/git stash pop       # Appliquer stash
```

### 📝 Créer un skill personnalisé

Voir `skills/Skills.md` pour le guide complet.

**Structure minimale** :
```python
# skills/mon_skill.py

NAME        = "mon_skill"
DESCRIPTION = "Description courte de ce que fait le skill"

def _mon_action(params: dict) -> dict:
    """
    Action exécutée par le LLM.
    params: dictionnaire des paramètres
    return: dictionnaire avec résultat
    """
    return {
        "ok": True,
        "resultat": "..."
    }

ACTIONS  = {"mon_action": _mon_action}
COMMANDS = {}

PROMPT   = """
### Mon skill
- **mon_action** → params: param (str)
"""
```

---

## 🧠 Mémoire et RAG

TitoAgent utilise **ChromaDB** pour une mémoire **persistante entre les sessions**.

### Flux de mémoire

```
Session 1          Session 2          Session 3
    │                  │                  │
    ▼                  ▼                  ▼
/compact           Au démarrage      /memory "comment
    │              charge auto        j'ai résolu X ?"
    ▼              le résumé               │
ChromaDB ──────────────────────────────────▶ LLM répond
                                           avec contexte
```

### Commandes de mémoire

| Commande | Effet |
|---|---|
| `/compact` | Résume la conversation par le LLM et la stocke dans ChromaDB. Libère la VRAM. |
| **Redémarrage** | Le dernier résumé est automatiquement rechargé dans le system prompt |
| `/memory [question]` | Recherche sémantique dans tout l'historique + code indexé |
| `/rag-menu` | Indexe tes projets pour que le LLM connaisse ton code |

### RAG (Retrieval-Augmented Generation)

Le RAG permet au LLM d'avoir accès à **tout ton codebase** en tant que contexte.

```bash
/rag-menu
# → Choisir : Indexer un dossier, Chatter avec le code, Purger
```

**Modèle d'embeddings recommandé** : `nomic-embed-text` via Ollama (274 Mo, très performant)

```bash
ollama pull nomic-embed-text
```

---

## 🗂️ Architecture du projet

```
TitoAgent/
│
├── 📄 titoagent.py                   # Point d'entrée principal
├── 📄 config.json                    # Configuration (auto-généré par wizard)
├── 📄 tito_profile.json              # Profil utilisateur (auto-généré)
├── 📄 requirements.txt               # Dépendances Python
│
├── 📁 commands/
│   └── router.py                     # Routing des commandes + boucle ReAct
│
├── 📁 core/
│   ├── model.py                      # Client LLM multi-provider
│   ├── memory.py                     # Mémoire vectorielle (ChromaDB)
│   ├── prompt.py                     # Système de prompt dynamique
│   ├── skill_loader.py               # Chargeur de skills
│   └── setup_wizard.py               # Wizard de configuration
│
├── 📁 tools/
│   ├── filesystem.py                 # Opérations fichiers + snapshots
│   ├── vision.py                     # Analyse d'images (local + cloud)
│   ├── web.py                        # Recherche web + YouTube
│   ├── sandbox.py                    # Exécution de code sécurisée
│   └── rag_menu.py                   # Menu RAG interactif
│
├── 📁 skills/                        # ← Ajoute tes skills ici
│   ├── git.py
│   └── Skills.md                     # Guide de création de skills
│
├── 📁 documentation/                 # Documentation interne
│
└── 📁 ui/
    └── display.py                    # Affichage Rich
```

---

## 🔒 Sécurité

TitoAgent respecte les meilleures pratiques de sécurité :

✅ **Les clés API ne sont jamais dans `config.json`**
- Stockées dans `~/.titoagent/api_keys.json` avec permissions `600`
- Variables d'environnement supportées et prioritaires
- Exclusion automatique du `.gitignore`

✅ **Profil utilisateur sécurisé**
- `tito_profile.json` ne contient aucune clé ou secret
- Préférences locales uniquement

✅ **Sandbox d'exécution**
- Code exécuté dans un environnement isolé
- Accès au filesystem contrôlé

✅ **Mémoire chiffrée**
- ChromaDB persistant avec accès local sécurisé

---

## 📊 Providers supportés

### Locaux (gratuits, sans API)

#### 🦙 Ollama
- Installation : https://ollama.ai
- Modèles : Mistral, Llama 2, Dolphin, Gemma, Qwen…
- Port par défaut : `11434`

#### 🔧 llama.cpp
- Serveur OpenAI-compatible sur port `8080`
- Supporte tous les modèles GGUF
- Parfait pour GPU faible

#### 🎮 LM Studio
- Interface graphique simple
- Détection automatique sur `localhost:1234`
- Idéal pour débuter

### Cloud (APIs optionnelles)

#### 🌐 Claude (Anthropic)
- Modèles : Sonnet 4, Haiku, Opus
- https://console.anthropic.com

#### 🔍 Gemini (Google)
- Modèles : Gemini Pro, Ultra
- Tier gratuit disponible
- https://ai.google.dev

#### 🤖 OpenAI
- Modèles : GPT-4o, GPT-4-turbo
- https://platform.openai.com/api-keys

---

## 📚 Fonctionnalités complètes

TitoAgent inclut :
- ✅ Mémoire vectorielle persistante (ChromaDB)
- ✅ RAG (Retrieval-Augmented Generation)
- ✅ Vision et analyse d'images
- ✅ Skills extensibles (Git, etc.)
- ✅ Snapshots et undo
- ✅ Gestion multi-utilisateurs
- ✅ Tous les autres outils listés ci-dessus

---

## 📄 Licence

TitoAgent est fourni avec une licence open source. Tu peux l'utiliser, le modifier et l'adapter à tes besoins.

---

## 🤝 Support et contribution

Pour toute question, bug, ou suggestion :
1. Consulte la documentation dans `documentation/`
2. Vérifie les FAQs dans le wiki du projet
3. Ouvre une issue avec détails précis

**Contributions de skills** : Rejoins notre communauté et partage tes skills ! 🎉

---

## 📞 Contact

Créé avec ❤️ pour les développeurs.

**Version** : 1.0.0 (Intègre /model-menu, RAG, vision, mémoire persistante, YouTube, et commandes étendues)

---

<div align="center">

**Commence maintenant** 🚀

```bash
python3 titoagent.py
```

</div>