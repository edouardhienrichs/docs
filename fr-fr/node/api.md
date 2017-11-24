# Référence API

Chaque nœud dans Neo-CLI donne accès à une interface API permettant d'obtenir les données de la chaîne à partir d'un nœud, rendant le développement des applications sur la chaîne beaucoup plus aisé. L'interface communique via le language [JSON-RPC](http://wiki.geekdream.com/Specification/json-rpc_2.0.html) et le protocole sous-jacent utilise HTTP/HTTPS pour communiquer. Pour démarrer un nouœd qui donne accès au service RPC, entrez les inscriptions suivantes :

`dotnet neo-cli.dll /rpc`

Pour accéder au serveur RPC via HTTPS, vous avez besoin de modifier le fichier de configuration `config.json` avant de lancer le nœud et y indiquer le nom de domaine, le certificat et son mot de passe :

```json
{
  "ApplicationConfiguration": {
    "DataDirectoryPath": "Chain",
    "NodePort": 10333,
    "WsPort": 10334,
    "UriPrefix": [ "https://*:10331", "http://*:10332" ],
    "SslCert": "YourSslCertFile.xxx",
    "SslCertPassword": "YourPassword"
  }
}                                          
```

Après le démarrage du serveur JSON-RPC, il va monitorer les ports suivants, correspondant au réseau principal ou au réseau de test :

|                | MainNet      | TestNet       |
| -------------- | ------------ | ------------- |
| JSON-RPC HTTPS | 10331        | 20331         |
| JSON-RPC HTTP  | 10332        | 20332         |

Pour plus d'informations sur le P2P ou les WebSockets, voir l'[introduction aux nœuds NEO](introduction.md).

## Liste des commandes

| Commande                                          | Référence                                     | Explication                                                                                                                   | Commentaires                              |
| ------------------------------------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| [getaccountstate](api/getaccountstate.md)         | \<address>                                    | Retourne les informations sur les actifs d'un compte selon l'adresse du compte indiquée                                       |                                           |
| [getassetstate](api/getassetstate.md)             | \<asset_id>                                   | Demande les informations d'un actif selon l'ID de l'adresse spécifiée                                                         |                                           |
| [getbalance](api/getbalance.md)                   | \<asset_id>                                   | Retourne le solde des actifs correspondants dans le portefeuille en fonction à l'ID de l'actif spécifié                       | Nécessite un porte-feuille ouvert.        |
| [getbestblockhash](api/getbestblockhash.md)       |                                               | Obtientle hash du plus grand bloc dans la chaîne principale                                                                   |                                           |
| [getblock](api/getblock.md)                       | \<hash> [verbose=0]                           | Retourne l'information du bloc correspondant à la valeur de hashage indiquée                                                  |                                           |
| [getblock](api/getblock2.md)                      | \<index> [verbose=0]                          | Retourne l'information du bloc correspondant à l'index indiqué                                                                |                                           |
| [getblockcount](api/getblockcount.md)             |                                               | Obtient le nombre de blocs de la chaîne principale                                                                            |                                           |
| [getblockhash](api/getblockhash.md)               | \<index>                                      | Retourne la valeur de hashage correspondant au bloc de l'index indiqué                                                        |                                           |
| [getblocksysfee](api/getblocksysfee.md)           | \<index>                                      | Returns the system fees before the block according to the specified index                                                     |                                           |
| [getconnectioncount](api/getconnectioncount.md)   |                                               | Obtient le nombre de connexion au noeud                                                                                       |                                           |
| [getrawmempool](api/getrawmempool.md)             |                                               | Obtient le nombre de transaction non-confirmée en mémoire                                                                     |                                           |
| [getrawtransaction](api/getrawtransaction.md)     | \<txid> [verbose=0]                           | Retourne les informations de la transaction correspondante basé sur la valeur de hashage spécifiée                            |                                           |
| [getstorage](api/getstorage.md)                   | \<script_hash>                                | Retourne la valeur stockée en fonction du hash de script du contrat et de la clé stockée                                      |                                           |
| [gettxout](api/gettxout.md)                       | \<txid> \<n>                                  | Retourne l'information de sortie de la transaction correspondante basé sur la valeur de hashage spécifiée et sur son index    |                                           |
| [sendrawtransaction](api/sendrawtransaction.md)   | \<hex>                                        | Diffuse une transaction dans le réseau. Voir la documentation [Protocol du réseau](network-protocol.md).                      |                                           |
| [sendtoaddress](api/sendtoaddress.md)             | \<asset_id> \<address> \<value> [fee=0]       | Transfert à l'adresse indiquée                                                                                                | Nécessite un porte-feuille ouvert.        |
| [sendmany](api/sendmany.md)                       | \<outputs_array> \[fee=0] \[change_address]   | Bulk transfer order                                                                                                           | Nécessite un porte-feuille ouvert.        |
| [getnewaddress](api/getnewaddress.md)             |                                               | Create a new address                                                                                                          | Nécessite un porte-feuille ouvert.        |
| [dumpprivkey](api/dumpprivkey.md)                 | \<address>                                    | Export the private key of the specified address                                                                               | Nécessite un porte-feuille ouvert.        |
| submitblock                                       | \<hex>                                        | Submit new blocks                                                                                                             | Nécessite d'être un nœud de consensus.    |
| [validateaddress](api/validateaddress.md)         | \<address>                                    | Verify that the address is a correct NEO address                                                                              |                                           |
| [getpeers](api/getpeers.md)                       |                                               | Get a list of nodes that are currently connected/disconnected by this node                                                    |                                           |

## Exemple de requête GET

Le format typique d'une requête GET en JSON-RPC se présente comme suit :

Voici par exemple comment obtenir le nombre de blocs dans la chaîne principale.

URL de la requête :

```
http://somewebsite.com:10332?jsonrpc=2.0&method=getblockcount&params=[]&id=1
```

Après avoir envoyé la requête, vous allez recevoir la réponse suivante :

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": 909129
}
```

## Exemple de requête POST

 Le format typique de requète POST JSON-RPC se présente comme suit :

Par exemple, voici comment avoir le nombre de blocs dans la chaîne principale.

URL de la requète :

```
http://somewebsite.com:10332
```

Corps de la requête :

```json
{
  "jsonrpc": "2.0",
  "method": "getblockcount",
  "params":[],
  "id": 1
}
```

Après avoir envoyé la requête, vous allez recevoir la réponse suivante :

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": 909129
}
```

## Outils de test

Vous pouvez utiliser [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop), une extention Chrome, pour faire des tests. (L'installation de l'extention Chrome requière une connexion Internet).

![image](/zh-cn/node/assets/api_2.jpg)

Voici la capture d'écran d'un expemple de test :

![image](/assets/api_3.jpg)

## Voir aussi

[Liste des commandes JSON-RPC en C#](https://github.com/chenzhitong/CSharp-JSON-RPC/blob/master/json_rpc/Program.cs)
