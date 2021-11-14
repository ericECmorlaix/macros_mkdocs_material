# Python pour les macros
Le plugin/framework [MkDocs-Macros](https://mkdocs-macros-plugin.readthedocs.io/en/latest/){target=_blank}
 offre de "libérer la puissance de Mkdocs,
  en utilisant des variables et des macros" à partir de scripts codés en Python...

## Installation

Dans Visual Studio Code (VSC), ouvrir un nouveau Terminal (menu `Terminal > New Terminal`) pour saisir :
```bash
pip install mkdocs-macros-plugin
```
Dans le dossier du projet, modifier les fichiers suivants tels que :
=== "`mkdocs.yml`"
    ```yaml hl_lines="5"
    plugins:
      - search
      - mkdocs-jupyter:
          include_source: True
      - macros
    ```
=== "`ci.yml`"
    ```yaml hl_lines="17"
    name: ci
    on:
      push:
        branches:
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - uses: actions/setup-python@v2
            with:
              python-version: 3.x      
          - run: pip install --upgrade pip
          - run: pip install mkdocs-material
          - run: pip install mkdocs-jupyter
          - run: pip install mkdocs-macros-plugin
          # C'est ici qu'il faut ajouter si besoin
          # les instructions pour installer avec pip
          # les autres plugins MkDocs ou extensions MarkDown
          # souhaités pour le déploiement du site...

          - run: mkdocs gh-deploy --force
    ```
Au prochain redémarrage avec la commande `mkdocs serve` en local
 et au prochain déploiement distant sur GitHub le plugin macros sera actif.

## Hello world :
A la racine du projet, à coté de `mkdocs.yml`, créer un fichier `main.py` tel que :
```py
def define_env(env):
    "Toto's hook"

    @env.macro
    def hello() :
        return "**Demat d'an holl**<br>_Toto est dans la place !_"
```

Dans n'importe quel fichier `source.md` on peut alors insérer le code MarkDown
suivant &lbrace;&lbrace; hello() &rbrace;&rbrace; pour appeler la macro `hello()`
et obtenir le rendu qu'elle génère :
=== "MarkDown généré"
    ```md
    {{ hello() }}
    ```
=== "Rendu"
    {{ hello() }}
    
!!! note "Remarques :"
    - Une macro comme `hello()` doit renvoyer une chaine de caractères constituée
    de MarkDown, $\LaTeX$, HTML, SVG, ...
    - Pour permettre l'affichage de la syntaxe MarkDown `{{ hello() }}` d'appel de la macro
    dans ce site, le plugin/framework MkDocs-Macros y est désactivé.
    - La configuration des macros présentées ici est donc réalisée
    dans le [dépôt](https://github.com/ericECmorlaix/macros_mkdocs_material/){target=_blank}
    et leur exécution est rendu visible sur le [site](https://ericecmorlaix.github.io/macros_mkdocs_material/){target=_blank}. 


--8<-- "includes/md/chantier.md"