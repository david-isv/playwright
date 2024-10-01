## Table des Matières

1. [Aperçu de l'API Playwright 🌍](#playwright-api-overview)
2. [Plus qu'un outil d'automatisation de navigateur 🌟](#more-than-a-browser-automation-tool)
3. [Assertions Web-first similaires à Jest 🔧](#jest-like-web-first-assertions)
4. [Playwright Test 🟢](#playwright-test)
5. [Mise à l'échelle et répartition avec Playwright 🏗️](#scaling-and-sharding-with-playwright)
6. [Génération de tests avec Codegen ✍️](#test-generation-with-codegen)
7. [Utilisation des locators avec getByRole 🎯](#using-locators-with-getbyrole)
8. [Attente automatique : gestion des pop-ups 🕒](#auto-waiting-handling-pop-ups)
9. [Variante 1 : Utilisation d'attributs personnalisés (`data-testid`) 🎯](#variant-1-using-custom-attributes-data-testid)
10. [Variante 2 : Utilisation des rôles ARIA 🎯](#variant-2-using-aria-roles)
11. [Chaînage de locators et sélection par texte 📝](#chaînage-de-locators-et-sélection-par-texte)

---

Cette version commence maintenant directement avec l'aperçu de l'API Playwright.

### Passer à la version anglaise EN

[Passer à la version anglaise](README.en.md)

### Aperçu de l'API Playwright 🌍

Playwright fournit une API unifiée permettant aux développeurs d'interagir avec plusieurs navigateurs. Il prend en charge trois moteurs principaux :

- **Chromium (Chrome) 🟢**
- **Firefox 🦊**
- **WebKit (Safari 🍎)**

Chaque navigateur est accessible via une API dédiée, mais l'interface de Playwright abstrait ces détails. Cela vous permet d'écrire un seul ensemble de tests qui s'exécute sur tous les navigateurs pris en charge, assurant ainsi la compatibilité multiplateforme et simplifiant les tests.

---

### Plus qu'un outil d'automatisation de navigateur 🌟

Playwright n'est pas seulement conçu pour l'automatisation des navigateurs ; c'est aussi un puissant framework pour réaliser des tests de bout en bout (E2E) ✅. Il vous permet de simuler des scénarios utilisateur réels, d'interagir avec les interfaces web et de valider le comportement des applications à chaque étape.

Avec Playwright, vous pouvez :

- **Automatiser les interactions utilisateur** (clics, saisies, navigation) 🖱️
- Tester des applications complexes sur différents navigateurs en parallèle 🕹️
- **Simuler les conditions réseau**, les **dimensions d'écran** et des **géolocalisations** spécifiques 📱🌍
- Capturer des **captures d'écran** ou **enregistrer des vidéos** des sessions de test 📸

---

### Assertions Web-first similaires à Jest 🔧

Playwright propose des **assertions web-first** similaires à **Jest**, facilitant la validation de l'état et des propriétés des éléments d'une page :

- **États des éléments :** `toBeChecked()`, `toBeVisible()`, `toBeInViewport()`
- **Propriétés :** `toHaveClass()`, `toHaveId()`, `toHaveText()`
- **Pages :** `toHaveTitle()`, `toHaveURL()`, `toHaveScreenshot()`

---

### Playwright Test 🟢

Playwright inclut son propre framework de test, offrant plusieurs fonctionnalités clés :

- Génération de tests avec **codegen** ✍️
- Extensions pour **VSCode** et **JetBrains** (payantes)
- Rapports de **tests automatisés** 📊
- **Exécution parallèle des tests** pour des exécutions plus rapides ⚡

Bien que Playwright soit conçu pour **JavaScript/TypeScript**, son utilisation ***n'est pas obligatoire*** !

---

### Mise à l'échelle et répartition avec Playwright 🏗️

- **Mise à l'échelle locale :** Vous pouvez exécuter plusieurs navigateurs en parallèle en utilisant plusieurs workers. Par exemple :
  ```bash
  npx playwright test --workers 2
  ```
  Cela accélère les tests en distribuant les tâches sur plusieurs instances de navigateur.

- **Répartition en CI :** Lors de l'exécution en CI, vous pouvez répartir les tests sur plusieurs tâches. Par exemple, en allouant des groupes spécifiques de tests à différents workers :
  ```bash
  npx playwright test --shard=1/3
  npx playwright test --shard=2/3
  npx playwright test --shard=3/3
  ```
  Cela vous permet d'attribuer des ensembles de tests spécifiques (par exemple, les 30 premiers tests au worker 1, les 40 suivants au worker 2, etc.), optimisant ainsi le temps de traitement.

---

### Génération de tests avec Codegen ✍️

Playwright propose un outil de **codegen** qui enregistre vos interactions sur une page et génère automatiquement du code de test. Pour l'utiliser :

```bash
npx playwright codegen
```

Toutes vos actions (clics, saisies, navigation) sont converties en scripts, simplifiant ainsi la création de scénarios de test sans avoir à écrire chaque étape manuellement.

---

### Utilisation des locators avec getByRole 🎯

Playwright vous permet de cibler précisément les éléments en utilisant des **locators** et la méthode `getByRole`. En définissant à l'avance des rôles ARIA ou des attributs, vous simplifiez la création de tests accessibles et robustes.

### Définir à l'avance les rôles et attributs dans HTML 💻
```html
<button role="button" aria-label="Envoyer le formulaire">Envoyer</button>
```

### Test avec Playwright 🚀
Vous pouvez ensuite cibler cet élément en fonction de son rôle et de ses attributs ARIA :

```javascript
const submitButton = page.getByRole('button', { name: 'Envoyer le formulaire' });
await submitButton.click();
```

### Utilisation d'attributs personnalisés 🔧
Avec des attributs comme `data-testid` :

```html
<button data-testid="submit-button">Envoyer</button>
```

Et dans le test Playwright :

```javascript
const submitButton = page.locator('[data-testid="submit-button"]');
await submitButton.click();
```

---

### Auto-waiting: : gestion des pop-ups 🕒

Playwright simplifie la gestion des interactions avec des éléments dynamiques comme les **pop-ups**. Grâce à l'attente automatique, Playwright attend automatiquement que les éléments soient prêts avant de réaliser une action, évitant ainsi les erreurs causées par des éléments qui apparaissent ou disparaissent de manière asynchrone.

#### Exemple : Interaction avec un pop-up

---

### Variante 1 : Utilisation d'attributs personnalisés (`data-testid`) 🎯

Playwright vous permet d'utiliser des attributs personnalisés comme `data-testid` pour cibler les éléments plus précisément.

#### Exemple :
```javascript
// Cliquer sur un bouton qui affiche le pop-up, en utilisant un attribut personnalisé
await page.click('[data-testid="show-popup-button"]');

// Attendre que le pop-up soit visible, puis interagir avec lui
await page.waitForSelector('[data-testid="popup-dialog"]', { state: 'visible' });
await page.fill('[data-testid="popup-input"]', 'exemple');

// Fermer le pop-up en utilisant un bouton spécifique, basé sur un attribut personnalisé
await page.click('[data-testid="close-popup-button"]');

// Attendre que le pop-up soit masqué
await page.waitForSelector('[data-testid="popup-dialog"]', { state: 'hidden' });
```

---

### Variante 2 : Utilisation des rôles ARIA 🎯

Playwright vous permet de cibler des éléments en utilisant les **rôles ARIA**, rendant ainsi vos tests plus robustes et accessibles.

#### Exemple :
```javascript
// Cliquer sur le bouton qui affiche le pop-up, en fonction du rôle ARIA "button"
await page.getByRole('button', { name: 'Ouvrir le pop-up' }).click();

// Attendre que le pop-up avec le rôle "dialog" devienne visible et interagir avec lui
await page.getByRole('dialog', { name: 'Boîte de dialogue du pop-up' }).waitForElementState('visible');
await page.getByRole('textbox', { name: 'Entrée du pop-up' }).fill('exemple');

// Fermer le pop-up en cliquant sur le bouton de fermeture, en fonction du rôle "button"
await page.getByRole('button', { name: 'Fermer le pop-up' }).click();

// Attendre que le pop-up soit masqué après la fermeture
await page.getByRole('dialog', { name: 'Boîte de dialogue du pop-up' }).waitForElementState('hidden');
```

---

### Chaînage de locators et sélection par texte 📝

Playwright vous permet de chaîner des locators et de cibler des éléments avec précision. Dans cet exemple, nous combinons un sélecteur CSS et un texte spécifique, puis nous ciblons un autre élément directement via son texte.

#### Exemple :
```javascript
await page
  .locator('.shoes-card_card', { hasText: 'Doc Martins' }) // Sélectionner un élément avec la classe

 CSS et le texte 'Doc Martins'
  .locator('text=VOIR DÉTAILS PRODUIT') // À l'intérieur, sélectionner un élément avec le texte 'VOIR DÉTAILS PRODUIT'
  .click(); // Cliquer sur le bouton
```

**Explication** :
1. **Premier locator** : Sélectionne un élément en utilisant le **sélecteur CSS** (`.shoes-card_card`) qui **contient** le texte `'Doc Martins'`.
2. **Deuxième locator** : À l'intérieur de cet élément, sélectionne un sous-élément dont **le texte est exactement** `'VOIR DÉTAILS PRODUIT'`.
3. **Action** : Une fois l'élément trouvé, il clique dessus.
