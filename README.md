# 🎚️ Svelte MultiSelect Toggle Demo  
Petite app de **démonstration** (switch + multi-select + options) – parfaite pour tester ou intégrer un composant `<MultiSelect>` réutilisable.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Svelte 5](https://img.shields.io/badge/Svelte-5-orange)

---

## ✨ Ce que montre la démo
- Toggle « classique » vs « luxe » (switch animé)  
- `<MultiSelect>` avec :  
  – **search** on/off  
  – **affichage** des éléments sélectionnés on/off  
- Zone live affichant le tableau `selected`  
- Tout en **TypeScript** et **runes Svelte 5**

---

## 📦 Installation rapide

```bash
git clone https://github.com/SalvadorGriaule/MultiSelectToggle.git
cd MultiSelectToggle
npm i
npm run dev
```

Ouvre `http://localhost:5173`

---

## 🧩 Structure

```
src/
├── App.svelte              # ce fichier
├── lib/
│   └── MultiSelect.svelte  # le composant réutilisable
└── assets/
    ├── catg.json           # data « classique »
    └── luxe.json           # data « luxe »
```

---

## 🕹️ Props du `<MultiSelect>`

| Prop          | Type       | Défaut | Description |
|---------------|------------|--------|-------------|
| `dataForMS`   | `{id,label}[]` | — | Liste des options |
| `name`        | `string`   | — | Label du champ |
| `mainClick`   | `boolean`  | `false` | Ouvre la liste au clic principal |
| `affSelected` | `boolean`  | `false` | Affiche les tags sélectionnés |
| `searchMode`  | `boolean`  | `false` | Barre de recherche |
| `selected`    | `string[]` | `[]` | **bindable** – éléments choisis |

---

## 🎨 Style

Le switch est **pure CSS** :
- `bg-slate-500` → `bg-green-500`
- translate `ml-0` → `ml-[75%]` (duration-150)

---

## 📄 Licence

MIT – copiez, découpez, remixez.

---

