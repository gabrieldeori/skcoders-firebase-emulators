# SKCoders Firebase Emulators
Projeto ainda em Beta, são necessários alguns testes com outros desenvolvedores.

## 1. Como criar um emulador firebase?
É necessário que você tenha o `firebase-tools` instalado globalmente.

### 1.1 Instalar firebase-tools
```sh
npm i -g firebase-tools
```

Cheque se está utilizando a versão mais recente.

A versão mais recente que chequei é a `12.4.8`
```sh
firebase --version
```

### 1.2 Java
Caso não tenha, instale o Java, recomendo a versão mais recente, pois os emuladores não funcionam com Java nas versões menores que a 12. Recomendo a versão 17 ou superior que seja estável, LTS.

> [Download Java](https://www.oracle.com/java/technologies/downloads)


### 1.3 Variáveis de ambiente
Ainda não está implementado no frontend nem no backend as funções para conexão com o banco mas o próximo passo seria:

Adicionar as variáveis de ambiente no seu projeto:

```js
// Front
REACT_APP_NODE_ENV=development

// Back (Admin SDK)
NODE_ENV=development
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099
FIRESTORE_EMULATOR_HOST=127.0.0.1:8080
```

### 1.4 Iniciando emulador
Agora inicie o emulador
```sh
firebase emulators:start
```

### 1.5 Portas
Cheque se está tudo funcionando, pode ser que as portas atualmente configuradas conflitem.
No caso entre em firebase.json e troque as portas conflitantes.
Altere também nas variáveis de ambiente.

> Atençao: Pode ser que o front ou back ainda não esteja configurado para interagir com os emuladores.

## Procedimentos ainda em teste

Ainda não entendi se é necessário estar logado na conta do firebase para utilizar os emuladores do firestore e do authentication da forma como estão configurados.

Preciso realizar testes na minha máquina local estando deslogado e também com outros devs para ter certeza.
