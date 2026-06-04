# 🚀 Guide complet — Lancer le tunnel sur Vercel

---

## ÉTAPE 1 — Créer ton webhook Tally (5 min)

### 1.1 Créer un compte Tally
- Va sur https://tally.so
- Clique **Sign up** — c'est gratuit
- Crée un compte avec ton email

### 1.2 Créer un formulaire Tally (nécessaire pour activer les webhooks)
- Clique **+ New form**
- Donne un nom : "Audit Landing Page — Réceptions"
- Ajoute un seul champ "Short answer" — peu importe, ce formulaire sert uniquement de récepteur
- Clique **Publish**

### 1.3 Activer le webhook
- Dans ton formulaire Tally, va dans **Integrate** (menu en haut)
- Clique **Webhooks**
- Clique **+ Add endpoint**
- Dans "Endpoint URL", entre temporairement : https://webhook.site/test (on changera après)
- Clique **Save**
- Tally te génère une URL qui ressemble à :
  https://hooks.tally.so/r/wkABCD1234

⚠️ COPIE CETTE URL — c'est ton webhook ID

### 1.4 Coller l'URL dans le fichier HTML
- Ouvre index.html dans un éditeur de texte (VSCode recommandé)
- Cherche cette ligne (ligne ~957) :
  const TALLY_WEBHOOK = 'https://hooks.tally.so/r/REMPLACE_PAR_TON_WEBHOOK_ID';
- Remplace par :
  const TALLY_WEBHOOK = 'https://hooks.tally.so/r/TON_VRAI_ID';

---

## ÉTAPE 2 — Recevoir les réponses par email (3 min)

### Dans Tally → Integrate → Email notifications
- Active "Send email notification"
- Entre ton adresse email
- Objet suggéré : "🔔 Nouvelle soumission d'audit — {{projet}}"
- Sauvegarde

Tu recevras un email à chaque nouvelle soumission avec toutes les données.

---

## ÉTAPE 3 — Déployer sur Vercel

### Option A — GitHub + Vercel (recommandée, déploiement auto)

**3A.1 Créer le repo GitHub**
- Va sur https://github.com/new
- Nom du repo : audit-landing (ou ce que tu veux)
- Visibility : Public ou Private (les deux marchent)
- Clique **Create repository**

**3A.2 Pousser les fichiers**

Si tu as Git installé :
```bash
git init
git add .
git commit -m "Initial commit — audit tunnel"
git branch -M main
git remote add origin https://github.com/TON_USERNAME/audit-landing.git
git push -u origin main
```

Sinon, via GitHub.com :
- Clique **uploading an existing file** sur la page du repo
- Glisse-dépose index.html ET vercel.json
- Clique **Commit changes**

**3A.3 Connecter à Vercel**
- Va sur https://vercel.com
- Clique **Add New Project**
- Clique **Continue with GitHub** — autorise l'accès
- Sélectionne ton repo **audit-landing**
- Framework Preset : **Other**
- Root Directory : laisser vide (ou `.`)
- Clique **Deploy**

✅ En 30 secondes ton site est en ligne sur :
https://audit-landing-XXXXXX.vercel.app

**3A.4 Ajouter un domaine personnalisé (optionnel)**
- Dans Vercel → Settings → Domains
- Clique **Add Domain**
- Entre : audit.tondomaine.com
- Suis les instructions DNS (ajoute un CNAME chez ton registrar)

---

### Option B — Drag & Drop (le plus rapide, 2 min)

1. Va sur https://vercel.com/new
2. Fais glisser le **dossier entier** `audit-tunnel` dans la zone de drop
3. Framework : **Other**
4. Clique **Deploy**

⚠️ Avec cette option, pas de redéploiement automatique. Tu devras re-drag-drop à chaque modification.

---

### Option C — Vercel CLI (pour les développeurs)

```bash
# Installer Vercel CLI
npm install -g vercel

# Aller dans le dossier
cd audit-tunnel

# Déployer
vercel

# Pour la production
vercel --prod
```

---

## ÉTAPE 4 — Tester le tunnel complet (2 min)

1. Ouvre ton URL Vercel
2. Remplis le formulaire de A à Z avec des données de test
3. Clique **Envoyer ma candidature**
4. Vérifie dans Tally → **Submissions** que la réponse est bien arrivée
5. Vérifie que tu reçois l'email de notification

---

## ÉTAPE 5 — Modifications fréquentes

### Changer le nom du site
Dans index.html, cherche et remplace :
- `<title>Audit gratuit de votre landing page</title>`
- `<a class="nav-logo" href="#">Audit<span>.</span></a>`

### Changer la couleur principale (orange → autre)
Dans le CSS, cherche `--accent: #c8522a;` et remplace par ta couleur.

### Changer les textes du hero
Cherche `<h1>Votre landing page mérite` dans le HTML.

### Ajouter Google Analytics
Colle ton tag GA4 juste avant `</head>` :
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

---

## RÉCAPITULATIF — Ordre des opérations

| # | Action | Durée | Outil |
|---|---|---|---|
| 1 | Créer compte Tally | 2 min | tally.so |
| 2 | Créer formulaire + webhook | 3 min | tally.so |
| 3 | Copier l'URL webhook dans index.html | 1 min | VSCode |
| 4 | Activer notifications email dans Tally | 1 min | tally.so |
| 5 | Créer repo GitHub | 2 min | github.com |
| 6 | Uploader les 2 fichiers | 1 min | github.com |
| 7 | Connecter à Vercel | 2 min | vercel.com |
| 8 | Tester le tunnel complet | 3 min | ton URL |
| TOTAL | | ~15 min | |

---

## STRUCTURE DES FICHIERS

```
audit-tunnel/
├── index.html     ← Le tunnel complet (landing + formulaire)
└── vercel.json    ← Config Vercel (routing statique)
```

---

## DONNÉES REÇUES DANS TALLY

À chaque soumission, Tally reçoit ce JSON :

```json
{
  "nom": "Marie Dupont",
  "email": "marie@monprojet.com",
  "projet": "Koopon",
  "role": "Fondateur / Co-fondateur",
  "url": "https://koopon.com",
  "description": "...",
  "probleme": "...",
  "clients": "...",
  "objectif": "Inscription / création de compte",
  "cta": "Essayer gratuitement",
  "action": "...",
  "defi": "...",
  "question": "...",
  "priorite": "...",
  "axes": "Marketing, Copywriting, UX, Conversion",
  "autorisations": "Analyse publique du site, Utilisation dans une vidéo",
  "soumis_le": "04/06/2026 à 14:32:10"
}
```
