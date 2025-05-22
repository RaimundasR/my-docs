<!-- ---
title: My docs and findings
layout: default
---

## Žingsniai, kaip apriboti tiesioginį įkėlimą į `master` šaką, leidžiant PR sujungimus be patvirtinimo

1. Nueikite į savo **GitHub repozitoriją** -> **Settings** -> **Branches**

2. Skiltyje **Branch protection rules** spauskite **"Add rule"**

3. Laukelyje **Branch name pattern** įrašykite:

   ```
   master
   ```

4. Pažymėkite šias parinktis:

   * ✅ **Require a pull request before merging**  *(reikalauti PR prieš sujungiant)*

     * 🔲 **Atžemėkite** "Require approvals" *(jeigu nenorite, kad PR būtų privalomai peržiūrėtas)*
   * ✅ *(Pasirinktinai)* **Require status checks to pass before merging** *(jeigu norite privalomų CI patikrinimų)*
   * ✅ **Restrict who can push to matching branches**

     * **Niekas neturi būti pridėtas**, kad **tik PR būtų leidžiami**

5. Spauskite **"Create" arba "Save changes"**

---

### Rezultatas:

* 🔒 **Negalima tiesiogiai push'inti į `master`**
* ✅ **Visi pakeitimai eina per Pull Request**
* ✅ **Patvirtinimas nebūtinas**, jei nepažymėjote "Require approvals"

Jei norėsite papildomai:

* Reikalauti **peržiūrėto PR** (code review)
* Leisti **auto-merge** arba **auto-approve** tam tikriems naudotojams ar botams

duok ženklą — padėsiu sukonfigūruoti! -->
