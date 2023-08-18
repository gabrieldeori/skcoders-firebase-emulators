# SKCoders Firebase Emulators

## Como criar um emulador firebase?
É necessário que você tenha o `firebase-tools` instalado globalmente.

```sh
npm i -g firebase-tools
```

Cheque se está utilizando a versão mais recente.

A versão mais recente que chequei é a `12.4.8`
```sh
firebase --version
```

Instale o Java, instale a versão mais recente, pois os emuladores não funcionam com Java de versão menor que a 12. Recomendo a versão 17 ou LTS superior.

> [Download Java](https://www.oracle.com/java/technologies/downloads)


Adicione as variáveeis de ambiente no seu projeto:
```js
// Front
REACT_APP_NODE_ENV=development

// Back (Admin SDK)
NODE_ENV=development
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099
FIRESTORE_EMULATOR_HOST=127.0.0.1:8080
```

Agora inicie o emulador
```sh
firebase emulators:start
```
Cheque se está tudo funcionando, pode ser que as portas atualmente configuradas conflitem.
No caso entre em firebase.json e troque as portas conflitantes.
Altere também nas variáveis de ambiente.

> Atençao: Pode ser que o front ou back ainda não esteja configurado para interagir com os emuladores.
