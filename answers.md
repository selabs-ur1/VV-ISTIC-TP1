VIAUD Benjamin
LAIR Arthur

# TP1

### 1) "Files Deletion Bug in Windows 10 October 2018 Update"

Source : https://www.askvg.com/reason-behind-users-files-deletion-bug-in-windows-10-october-2018-updte/
https://www.extremetech.com/computing/279368-windows-10-1809-may-have-another-file-deleting-bug-problem

Le bug de suppression de fichiers de la mise à jour Windows 10 version 1809, également connu sous le nom de "File Deletion Bug", a eu un impact mondial en affectant de nombreux utilisateurs suite à leur passage à cette version spécifique du système d'exploitation. Ce bug a causé la suppression involontaire de fichiers en lien avec la fonctionnalité de "Known Folder Redirection", qui ne parvenait pas à créer correctement de nouveaux dossiers dans l'emplacement de redirection, entraînant la perte apparente de données importantes pour les utilisateurs.

Les répercussions pour les clients ont été considérables, entraînant la perte de dossiers entiers ou de fichiers critiques sans avertissement préalable. Pour Microsoft, l'entreprise derrière Windows 10, ce bug a nui à sa réputation et a suscité des critiques concernant la qualité de ses mises à jour logicielles. Pour éviter de tels problèmes à l'avenir, des tests plus approfondis, notamment des simulations de migrations de données et des scénarios variés de redirection de dossiers, auraient pu contribuer à découvrir et corriger ce bug avant sa diffusion auprès du public.

### 2)

#### lien : 
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-826?filter=doneissues
    
#### Bug :
    BloomFilter: move CountingLongPredicate

#### Explication du bug:

    Le bug se situe dans la classe CountingLongPredicate dans la méthode forEachRemaining. Cette méthode devrait être un détail d'implémentation, mais elle est actuellement déclarée comme package private dans une interface, ce qui la rend accessible publiquement.

    Si la méthode func.test renvoie false dans la boucle while de forEachRemaining, la boucle se termine avant que idx n'atteigne la longueur de ary. Cependant, cela permet à forEachRemaining d'être appelé à nouveau, réutilisant la même position dans le tableau. Si cette méthode était publique, elle pourrait être invoquée plusieurs fois, entraînant un comportement indéfini, surtout étant donné l'absence de commentaire sur ce point.

    De plus, une variable idx est initialisée à zéro, mais cette initialisation est redondante et peut être supprimée.
    
#### Global ou local ?

    Le bug est plutôt local, car il concerne spécifiquement la méthode forEachRemaining de la classe CountingLongPredicate. Il n'affecte pas d'autres parties du code ou d'autres fonctionnalités de manière étendue.


#### Solution ?

    Il est suggéré de déplacer la classe CountingLongPredicate en dehors de l'interface afin que la classe soit package private. Cela garantirait que la méthode forEachRemaining n'est pas accessible publiquement. De plus, des améliorations d'optimisation et de documentation sont suggérées pour clarifier le comportement de la méthode forEachRemaining.


#### Ont-il ajouter des nouveaux test ?
    
    Il n'ont pas ajouter des nouveaux test mais un comentaire pour spécifier sont fonctionement.


### 3) Netflix

Concrètement, les expériences de Chaos Engineering décrites dans l'article sont variées et se concentrent sur des scénarios de défaillance simulée ou réelle dans les systèmes distribués de Netflix. Quelques exemples d'expériences réalisées incluent :

    Chaos Monkey : Cette expérimentation consiste à arrêter aléatoirement des instances de machines virtuelles hébergeant des services en production pendant les heures de travail pour tester la résilience des systèmes face à de telles défaillances.

    Chaos Kong : Ces exercices simulent la défaillance d'une région entière d'Amazon EC2 pour évaluer la capacité de Netflix à basculer efficacement vers d'autres régions en cas de panne.

    Tests d'injection de défaillance (FIT) : Ils consistent à rendre intentionnellement défaillantes les requêtes entre les services Netflix pour vérifier la dégradation contrôlée du système.

Les exigences pour ces expériences incluent la capacité à réaliser des tests en production, à simuler des défaillances de manière contrôlée et à observer les métriques et indicateurs clés qui reflètent la stabilité et la fiabilité du système. Parmi les variables observées, on retrouve des métriques telles que le nombre de démarrages de streaming par seconde (SPS) pour évaluer l'état de santé global du système.

Les principaux résultats obtenus par ces expériences sont l'amélioration de la conception des services Netflix pour résister aux défaillances individuelles, l'identification de mécanismes de dégradation contrôlée en cas de panne et l'augmentation de la confiance dans la capacité du système à maintenir son comportement en cas de perturbation.

Netflix n'est pas la seule entreprise à réaliser de telles expériences. L'article mentionne également Amazon, Google, Microsoft et Facebook comme d'autres entreprises appliquant des techniques similaires pour tester la résilience de leurs systèmes.

Ces expériences pourraient être menées dans d'autres organisations en adaptant les types d'expériences aux services spécifiques qu'elles offrent. Par exemple, dans une organisation de commerce électronique, des expériences pourraient être menées pour simuler des défaillances dans le processus de paiement en observant des métriques telles que le nombre de transactions réussies par seconde. De manière générale, les variables observées pendant les expériences doivent être des métriques représentatives du comportement normal du système et de son état de santé global. Les expériences devraient également tenir compte des interactions entre les différents composants du système pour évaluer la résilience de l'ensemble du système.

### 4) WebAssembly

#### Expliquez quels sont les principaux avantages d'une spécification formelle pour WebAssembly ?
    L'utilisation d'une spécification formelle pour WebAssembly offre plusieurs avantages. Tout d'abord, elle permet d'établir une base solide pour son évolution, favorisant ainsi la cohérence et la compréhension claire des fonctionnalités. De plus, cette approche formelle facilite la communication entre les développeurs en définissant de manière précise le comportement attendu de WebAssembly. En outre, elle joue un rôle crucial dans la réduction des ambiguïtés et des interprétations divergentes, contribuant ainsi à une mise en œuvre plus uniforme et conforme aux normes.

#### À votre avis, cela signifie-t-il que les implémentations de WebAssembly ne devraient pas être testées ?
    Bien que WebAssembly bénéficie d'une spécification formelle, il est impératif de souligner que les implémentations ne sont pas exemptes d'erreurs potentielles. Par conséquent, il est crucial de continuer à tester rigoureusement les implémentations. Les tests permettent de vérifier la conformité aux spécifications, d'identifier d'éventuels écarts et de garantir que les modifications apportées au code n'introduisent pas d'erreurs inattendues. Les tests demeurent un élément essentiel du processus de développement, assurant la fiabilité et la stabilité continues de WebAssembly dans des contextes réels d'utilisation.

### 5) Isabelle

