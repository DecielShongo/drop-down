# 🎛️ Svelte Dropdown Component - Deciel Shongo

Ein animiertes, vollständig per Tastatur und Maus bedienbares Dropdown-Menü in [Svelte](https://svelte.dev/), mit Unterstützung für Fokus-Handling, Animation und Tailwind CSS.

---

## 🚀 Features

- Dynamische Liste von Optionen
- Tastatursteuerung (`ArrowUp`, `ArrowDown`, `Enter`)
- Mausnavigation mit Hover
- Fokus-Handling: Dropdown schließt bei Klick oder Fokusverlust
- Fly-in-Animation beim Öffnen (Svelte `fly`)
- Tailwind CSS für Layout und Styling

---

## 🔧 Props

| Name          | Typ        | Beschreibung                      |
| ------------- | ---------- | --------------------------------- |
| `placeholder` | `string`   | Platzhaltertext für den Startwert |
| `options`     | `string[]` | Liste der auswählbaren Optionen   |

---

## 🎹 Tastatursteuerung

| Taste       | Funktion                    |
| ----------- | --------------------------- | --- |
| `ArrowDown` | Auswahl nach unten bewegen  |
| `ArrowUp`   | Auswahl nach oben bewegen   |
| `Enter`     | Aktuelle Auswahl bestätigen |     |

---

## ✨ Animation

Beim Öffnen des Dropdowns erscheinen die Elemente mit einer gestaffelten `fly`-Transition von links. Beim Schließen wird ein `fade`-Effekt verwendet.

```svelte
in:fly={{ delay: i * 100, duration: 300, x: -50 }}
out:fade
```
