# Ambiente de emulação Firebase (Beta)
**Nota: Este projeto está atualmente em fase Beta e requer testes com outros desenvolvedores para validar sua praticidade.**

O Ambiente de Emulação Firebase foi criado para simular o comportamento do Firestore e do Authentication do Firebase localmente, auxiliando no teste e desenvolvimento de apps.

Quando necessário é possível expandir esse ambiente para incluir mais serviços Firebase no futuro porém no momento contamos apenas com `authentication` e `firebase`.

Seu feedback é crucial para aprimorar essa ferramenta.

Agradecemos por participar da fase Beta de testes e contribuir para a evolução do Ambiente de Emulação Firebase.

> Favor não fazer commits nesse repositório, o projeto deve ser padrão a todos.

## 1. Como emular o firebase?
É necessário que você tenha o `firebase-tools` instalado globalmente.

### 1.1 Instalar firebase-tools
```sh
npm i -g firebase-tools
```

Por favor, verifique se está utilizando a versão mais recente do software de acordo com a documentação do firebase.

[Acesse as versões aqui](https://firebase.google.com/support/releases).

```sh
firebase --version
```

### 1.2 JavaJDK e Node.js
No caso de ausência, por favor, proceda com a instalação do ambiente [Java JDK 11 ou mais recente](https://jdk.java.net/) e do [NodeJS 16.0 ou mais recente](https://nodejs.org/en/download).

É aconselhável escolher versões ditas estáveis ou suportada a longo prazo (LTS).

### 1.3 Variáveis de ambiente
 Incorpore as variáveis de ambiente ao seu projeto.

###### Nessa documentação fornecerei variáveis de conexão com o firebase de um servidor particular, mas isso deve ser ajustado no futuro com um servidor oficial.

#### 1.3.1 Para o frontend em `.env`:
```py
# Environment
REACT_APP_NODE_ENV=development

# Firebase Connection
REACT_APP_API_KEY=AIzaSyB9Gk7D967jdp0S2IXw6gAQVYJIs3RVH4k
REACT_APP_AUTH_DOMAIN=skcoders-firebase-emulators.firebaseapp.com
REACT_APP_PROJECT_ID=skcoders-firebase-emulators
REACT_APP_STORAGE_BUCKET=skcoders-firebase-emulators.appspot.com
REACT_APP_MESSAGING_SENDER_ID=1033973860909
REACT_APP_APP_ID=1:1033973860909:web:e28b5887acc9a98d5ac6a4
REACT_APP_MEASUREMENT_ID=G-WK5YQB779Q
```

**Atenção: Não faça testes com as variáveis da seção "Firebase Connection" apontando para o servidor de deploy.**

#### 1.3.2 Para o backend (Ainda em estudo):
Quando se busca estabelecer uma conexão intermediária entre o frontend e o banco de dados por meio de uma API, é comum recorrer à biblioteca `firebase-admin`. Essa abordagem envolve a utilização de contas de serviço para autenticar um administrador responsável por gerenciar as modificações no banco de dados.

A configuração desse processo é efetuada da seguinte maneira:

```js
const admin = require("firebase-admin");

const serviceAccount = require("path/to/serviceAccountKey.json");

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: "https://nomeprojeto-default-rtdb.firebaseio.com",
});
```

Um requisito crucial é a geração da chave serviceAccountKey que é feita pelo administrador do firebase.

Prosseguindo para a ativação do emulador, a configuração das seguintes variáveis de ambiente é necessária:

```py
# Ambiente
NODE_ENV=development

# Conexão com Emuladores
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099
FIRESTORE_EMULATOR_HOST=127.0.0.1:8080
```


A investigação do comportamento no backend ainda está em andamento. Ao utilizar o pacote firebase-admin com Node.js, a documentação atual sugere que ao configurar as variáveis de ambiente especificadas, a biblioteca estabelecerá automaticamente a conexão apropriada com os emuladores locais.

### 1.4 Iniciando emulador
Neste ponto, é importante considerar a possibilidade de ocorrer um erro associado às portas. No entanto, saiba que abordaremos essa questão em detalhes posteriormente.

Para iniciar o emulador, utilize o seguinte comando:
```sh
firebase emulators:start
```

### 1.5 Tratando erros de conexão nas portas
Os emuladores utilizam portas que podem conflitar de acordo com as configurações locais de cada máquina. Caso ocorram problemas de conexão, sugiro que ajuste os valores das portas no arquivo `firebase.json` para que as mesmas não conflitem com outras em uso. (Substitua nos XXXX e YYYY destacados)

> Não faça commits com alterações no arquivo firebase.json. Mantenha a modificação apenas localmente.

```js
{
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "emulators": {
    "auth": {
      "port": XXXX // Modifique aqui
    },
    "firestore": {
      "port": YYYY // Modifique aqui
    },
    "ui": {
      "enabled": true
    },
    "singleProjectMode": true
  }
}

```

A próxima etapa de correção envolve a configuração das variáveis de ambiente do projeto em consideração. O seguinte trecho de código exemplifica essas configurações:

Para o frontend em `firebaseSdk/index.js`, substitua as portas:
```js
  connectAuthEmulator(auth, "http://127.0.0.1:XXXX"); // auth
  connectFirestoreEmulator(firestoreDB, '127.0.0.1', YYYY); // firestore
```

> O firebase parece não gostar de usar variáveis de ambiente nessas funções.

Para o backend em `.env`:
```py
FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:XXXX
FIRESTORE_EMULATOR_HOST=127.0.0.1:YYYY
```

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
