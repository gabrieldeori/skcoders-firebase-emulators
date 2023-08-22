# Ambiente de emulação Firebase (Beta)
**Nota: Este projeto está atualmente em fase Beta e requer testes abrangentes com outros desenvolvedores para garantir sua confiabilidade e funcionalidade.**

O Ambiente de Emulação Firebase foi criado para simular o comportamento do Firestore e do Authentication do Firebase localmente, auxiliando no teste e desenvolvimento de apps.

Quando necessário vamos expandindo esse ambiente para incluir mais serviços Firebase no futuro. Seu feedback é crucial para aprimorar essa ferramenta.

Agradecemos por participar da fase Beta de testes e contribuir para a evolução do Ambiente de Emulação Firebase.

## 1. Como criar um emulador firebase?
É necessário que você tenha o `firebase-tools` instalado globalmente.

### 1.1 Instalar firebase-tools
```sh
npm i -g firebase-tools
```

Por favor, verifique se está utilizando a versão mais recente do software de acordo com a [documentação do firebase](https://firebase.google.com/support/releases).
```sh
firebase --version
```

### 1.2 JavaJDK e Node.js
No caso de ausência, por favor, proceda com a instalação do ambiente [Java JDK 11 ou mais recente](https://jdk.java.net/) e do [NodeJS 16.0 ou mais recente](https://nodejs.org/en/download).

É aconselhável escolher versões ditas estáveis ou suportada a longo prazo (LTS).

### 1.3 Variáveis de ambiente
 **Nota: É possível que as funções de conexão com o banco de dados local ainda não tenham sido implementadas, nem no frontend nem no backend.**
 
 Agora basta incorporar as variáveis de ambiente ao seu projeto:

```js
// Front
REACT_APP_NODE_ENV=development
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099
FIRESTORE_EMULATOR_HOST=127.0.0.1:8080

// Back (Admin SDK)
NODE_ENV=development
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099
FIRESTORE_EMULATOR_HOST=127.0.0.1:8080
```

### 1.4 Iniciando emulador
Neste ponto, é importante considerar a possibilidade de ocorrer um erro associado às portas. No entanto, saiba que abordaremos essa questão em detalhes posteriormente.

Para iniciar o emulador, utilize o seguinte comando:
```sh
firebase emulators:start
```

### 1.5 Tratando erros de conexão nas portas
Os emuladores utilizam portas que podem conflitar de acordo com as configurações locais de cada máquina. Caso ocorram problemas de conexão, sugiro que ajuste os valores das portas no arquivo `firebase.json`. Entretanto, é fundamental destacar que quaisquer alterações nesses valores não devem ser confirmadas por meio de commits no git do projeto:

```js
{
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "emulators": {
    "auth": {
      "port": XXXX
    },
    "firestore": {
      "port": YYYY
    },
    "ui": {
      "enabled": true
    },
    "singleProjectMode": true
  }
}

```

A próxima etapa envolve a configuração nas variáveis de ambiente do projeto em consideração. O seguinte trecho de código exemplifica essa configuração:
```js
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:XXXX
FIRESTORE_EMULATOR_HOST=127.0.0.1:YYYY
```

Recomenda-se verificar se o sistema está funcionando corretamente, pois pode ocorrer conflito com as portas atualmente configuradas. Caso seja o caso, é possível efetuar as seguintes modificações: ajustar as portas em conflito no arquivo firebase.json e também nas variáveis de ambiente.

## Outros recursos
Existem vários recursos que podem ser emulados:
- Authentication
- Realtime Database
- Firestore
- Storage
- Functions
- Extensões

A viabilidade de alguns recursos ainda está sendo testada.

Um ponto a ser observado é a necessidade de autenticação para criar projetos, que abrangem áreas como armazenamento (Storage), Banco de Dados em Tempo Real (Realtime Database) e Funções (Functions). Além disso, para o funcionamento da aplicação, é essencial possuir um projeto real hospedado na nuvem.

Vale mencionar que a ausência de uma conexão direta do firebaseConfig com o emulador foi deliberada por questões de segurança. Uma abordagem mais recomendada consiste em estabelecer um projeto na nuvem, de tipo "developer", em que possa se compartilhar com os desenvolvedores. Essa abordagem simplificaria a configuração da conexão de maneira segura e eficaz.
