---
up: []
---
#### **1. Lier un dépôt local à GitHub**
```bash
git remote add origin https://github.com/USERNAME/NOM_DU_REPO.git
```

#### **2. Ajoute tous les fichiers et faire un commit**
```bash
git add .
git commit -m "Première sauvegarde du vault Obsidian"
```

#### **3. Pousser les fichiers sur GitHub**
```bash
git branch -M main
git push -u origin main
```

---

### Pour les sauvegardes suivantes :
Ouvrir un terminal depuis Obsidian-vault et y exécuter les commandes suivantes :
```bash
git add .
git commit -m "Mise à jour du vault"
git push
```
