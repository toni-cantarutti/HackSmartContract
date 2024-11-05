## Ex1
La boucle à L33 n’a pas de limite. Il est donc possible d’utiliser `store` un grand nombre de fois pour que le coût en gas de `take` soit plus grand que la `gaslimit`.  
Il faut en général que les fonctions soient en O(1) ou O(log n) pour que les coûts soient raisonnables.

## Ex2
À partir de 3 objets, il n’est plus possible de payer le prix exact car il faudrait 1/2 de wei, mais il n’est pas possible de payer des fractions de wei.

## Ex3
Il est possible de connaître la valeur de `lastChoiceHead` en regardant la chaîne ou les TXs du smart contract.

## Ex4
Il est possible d’attaquer ce contrat en profitant de la possibilité de réentrance à L115.  
Le contrat attaquant peut rappeler `redeem` à L115 avant que la balance soit mise à 0.

## Ex5
Puisque n’importe qui peut `guess`, le joueur A peut regarder la mempool.  
Si il voit que B va perdre, il le laisse perdre.  
Si il voit que B va gagner, il peut frontrun la TX en jouant contre lui-même.  
Donc A va soit gagner, soit annuler le jeu (en jouant contre lui-même).

## Ex6
Les `balances` et `amounts` sont des `int` et non des `uint`. Il est donc possible d’envoyer des quantités négatives et des quantités que l’on ne possède pas.

## Ex7
L'implémentation ne résulte pas en une bonded curve linéaire.  
La courbe achète et se vend au prix actuel. Il est donc possible d’acheter (le prix augmente) et de vendre pour plus cher. En répétant cela, on peut drainer la courbe.

## Ex8
`closeAccount` met le nombre de coffres à 0 mais ne met pas leurs balances à 0.  
On peut donc recréer les coffres qui auront la même quantité que lors de leurs destructions et drainer le contrat.

## Ex9
Il est possible d’abuser de L309 en mettant un `amount_` tel que `scalingFactor * _amount < address(this).balance`.  
De cette façon, `toRemove` sera arrondi à 0, ce qui permet de retirer `amount_` sans diminuer la balance du coffre.  
On peut répéter ceci pour drainer le contrat.

## Ex10
Les deux parties peuvent empêcher le paiement des rewards. Pour cela, ils peuvent utiliser un smart contract qui n’accepte pas les ETH et payer un peu plus que demandé.  
Le remboursement du surplus à L380 échouera, ce qui fera échouer la fonction, empêchant l’autre partie d’être payée.

## Ex11
Il est possible d’usurper l’ID d’un autre utilisateur car il dépend de la simple concaténation de ses composants.  
Ex: `Clément Lesaege` / `Clémen tLesaege`

## Ex12
Il est possible de créer des tokens en s’envoyant des tokens à soi-même.  
Ex: Alice a 1 token. Elle se l’envoie. L457 met sa balance à 0, mais L460 réécrit dessus et la met à 2.

## Ex13
Un joueur peut utiliser un nombre très élevé (proche de `max(uint)`) de sorte à ce que L552 échoue pour les autres joueurs (overflow menant à revert).

## Ex14
Il est possible d’envoyer des ETH au piggy bank même s’il n’a pas de fonction payable, de sorte à ce que la balance soit plus élevée que 10 ETH. Or, le contrat demande exactement 10 ETH, donc les fonds seront bloqués à jamais.  
La façon la plus simple d’envoyer des ETH à des contrats sans fallback payable est de créer un contrat et d’utiliser `selfdestruct`.

## Ex15
À L692, la fonction `delete` n’a pas d’effet sur les `mapping`. Le `mapping isAllowed` ne sera pas remis à 0 et les joueurs pourront alors réclamer des rewards à chaque round.
