# Workflow 
 
## Démarrage d'un PBI
### Démarrage du développement

Lors d'un sprint le démarrage du développement correspond au déplacement de la première tâche du PBI en état "in progress" dans Jira.

À ce moment le développeur qui commence son travail, devrait créer un branche git locale de la façon suivante:

```
# Création de la branche depuis un master à jour
$ git fetch
$ git co -b <PREFIX>/<ID_JIRA>_short_description origin/master
```


Les préfixes actuellement utilisés sont:

    story: le PBI est une US
    bug: le PBI est un bug
    task: le PBI est une tâche
    grooming: du code destiné à partir à la poubelle, il s'agit uniquement d'un prototype fait dans le cadre d'un grooming ou autre

Il peut alors commencer son travail. À n'importe quel moment il peut décider de publier sa branche sur le dépôt origin afin de donner accès au code à d'autres développeurs ou simplement sécuriser le code. Il n'y a aucun seuil de qualité à atteindre avant de publier la branche. Dans le pire cas, la branche pourra être supprimée.

Le choix de faire du pair programming être libre.

À ce stade, le code n'est pas lié à l'intégration continue.


### Intégration avec la CI

Pour lier la branche avec la CI faut créer une merge request dans GitLab. Elle doit répondre aux critères suivants:

    Non assignée
    Taggée sur le sprint courant
    Ajouter l'url de la merge request en commentaire du ticket Jira

À cette étape il n'est pas nécessaire de la documenter puisque le but est uniquement de bénéficier de toute l'automatisation dans Jenkins. Automatiquement un build se déclenchera lors de chaque commit sur la branche. Le résultat du build sera automatiquement publié en commentaire dans GitLab.

Le but des jobs CI de merge request est d'une part de vérifier qu'il n'y a pas de régression mais aussi d'automatiser la mise à disposition de toute donnée pouvant être utile à la validation de la branche (données ou job de référence etc.).

En fonction du retour de la CI la branche peut être mise à jour avec de nouveaux commits

## Écriture d'une merge request

L'objectif est de convaincre le(s) relecteur(s) & PO que le PBI est Done et peut être intégré au master. On cherche donc a faciliter le travail du relecteur pour qu'il ait confiance que le changement proposé est correcte.

Les grands principes permettant une revue efficace sont:

    Fournir un patch le plus propre possible
    Expliquer clairement le cadre, les choix forts effectués, ainsi que toute limitation
    Fournir toute information utile à la compréhension de la merge request
    Fournir des liens cliquable pour toute ressource externe et potentiellement un diff lorsque cela est utile.
    Essayer de faire en sorte que la plupart des informations soient consommables par des personnes en dehors de l'équipe. En particulier les étapes de validation.
    Enlever les sections non pertinentes (mais documenter les manques si il y en a)

```
# Context
<Briefly explain what the context of this development is, so to help the reader to understand what's at stake>
  
# Implementation
  
<Give
 the reader some insights about the key ideas that drove the
development, in particular if strong choices were made, explicit
them.>
  
# Documentation
  
<Describe which pages have been updated, use hyperlink if useful>
  
# Validation
  
## Unit  tests
  
<Give the reader some insight about the test strategy (if not trivial). List any shortcoming>
  
## Business facing tests
  
<Give the reader some insight about the test strategy (if not trivial). List any shortcoming>
  
## Non Regression tests
   
<Prove that all output have not changed (except expected changes)>
  
## Exploratory tests
  
<Prove that the feature works as expected, corner cases are handled, usability is good etc.>
  
# Conclusion
  
<If needed, discuss any outstanding question, doubt, shortcoming, open question etc.>

```

## Relecture d'une merge request

### Principe généraux

Le(s) relecteur(s) d'une merge request doit se considérer comme la personne responsable de l'évolution / fonctionnalité. Il lui incombe de décider si le changement proposé répond à la demande initiale et aux exigences de l'entreprise, du produit et de l'équipe.

Cela inclus principalement trois aspects:

    Code review: Le code soumis est-il d'une qualité suffisante pour à la fois répondre aux besoins fonctionnels et non-fonctionnels exprimés et promettre une maintenabilité suffisante. C'est donc une analyse en white-box
    Quality assurance: Le produit est analysé en black-box pour valider qu'il répond aux besoins fonctionnels et non-fonctionnels, qu'il est robuste, que son usabilité est bonne etc.
    Business: L'évolution est analysée dans le cadre du produit et de l'entreprise. S'inscrit-elle logiquement dans la continuité du produit ?

La répétition systématique et honnête de ce processus pour chaque évolution permet de:

    Améliorer la qualité du produit en se construisant un processus visant à empêcher les erreurs systémique (idée bien développée par James Shore dans Alternatives to Acceptance Testing)
    Rendre les pratiques de l'équipe cohérente au sein d'un produit, de diffuser les bonnes pratiques et les connaissances.


Il est intéressant d'approcher la relecture d'une merge request avec l'état d'esprit développé dans: Four NOs of a Serious Code Reviewer. La merge request doit convaincre le relecteur qui l'évolution doit être intégré. Un compromis n'est pas une bonne issue (attention cependant à bien faire la différence entre des critères objectifs et des préférences personnelles)

### Code review

    TRIVIAL Le code suit-il le coding style (voir Coding Style Java)?
    TRIVIAL Le code suit-il les règles concernant le logging (voir Réalisation - Gestion des logs applicatifs) ?
    MEDIUM Le code est-il facile à lire ? Respecte t-il les principes, idiomes et bonnes pratiques du langage utilisé (ex: SOLID en Java) ?
    MEDIUM Le design permet-il facile de tester le code ? (ex: Guide-Writing Testable Code)
    MEDIUM Les erreurs courantes liés à l'utilisation de fichier sont-elles prises en comptes ? (encoding, gestion des erreurs, parsing stricte ou tolérant, fin de ligne, séparateurs etc. voir Text handling grand tour)
    MEDIUM Le code est-il correctement testé en utilisant le bon niveau d'abstraction ? (unit, business facing, integration, end-to-end etc.)
    HARD Les cas d'erreurs sont-ils correctement et exhaustivement gérés ? Sont-ils testés ? La pratique montre que la plupart des erreurs critiques proviennent de problèmes dans la gestion des erreurs plutôt que dans le chemin nominal.

### QA

    TRIVIAL L'évolution est-elle correctement décrite dans les releases notes ? (de toutes les versions concernées)
    TRIVIAL Si besoin, le manuel utilisateur a t-il été mis à jour ?
    TRIVIAL Si besoin, la documentation fonctionnelle a t-elle été mise à jour ?
    MEDIUM Lorsque je fais des tests exploratoires l'application est-elle robuste ? Se comporte t-elle comme attendue ? Les messages d'erreurs sont-ils compréhensible pour l'utilisateur ? Permettre t-ils de diagnostiquer le problème ?
    MEDIUM En l'absence de test de performance automatique, lorsque je fais des tests exploratoires la performance de l'application, ou de l'évolution, me semble t-elle en adéquation avec les contraintes non fonctionnelles ?
    MEDIUM L'aspect général du manuel utilisateur et de la documentation fonctionnelle est-il satisfaisant ? (cohérence globale & style, facilité de trouver les informations, les informations sont correctes)

### Business

    MEDIUM Est-ce que je comprends la place de l'évolution dans le produit ? Suis-je convaincu qu'elle améliore le produit ? 

