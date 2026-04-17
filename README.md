# Projeto Ionic + Vue - Login e Consumo de API

## Sobre o projeto

Este projeto foi desenvolvido utilizando Vue.js com Ionic Framework.

A aplicação possui:

* Tela de login
* Validação simples de usuário e senha
* Navegação entre páginas utilizando Vue Router
* Consumo de API pública
* Listagem dinâmica de usuários
* Interface responsiva utilizando componentes do Ionic

A API utilizada no projeto é a JSONPlaceholder.

---

## Tecnologias utilizadas

* Vue.js
* Ionic Framework
* Vue Router
* TypeScript
* Capacitor
* Fetch API
* CSS

---

## Funcionalidades

### Tela de Login

O sistema possui uma tela de login com:

* Campo de e-mail
* Campo de senha
* Mensagem de erro para credenciais inválidas
* Redirecionamento para a tela principal após login

### Consumo de API

Após o login, a aplicação consome dados da API JSONPlaceholder.

Os dados retornados são exibidos em uma lista de usuários contendo:

* Nome
* E-mail

---

## Estrutura do projeto

```bash
src/
 ├── router/
 │    └── index.ts
 ├── views/
 │    ├── LoginPage.vue
 │    └── FolderPage.vue
 ├── App.vue
 └── main.ts
```

---

## Como executar o projeto

### Instalar dependências

```bash
npm install
```

### Rodar em ambiente de desenvolvimento

```bash
ionic serve
```

ou

```bash
npm run dev
```

---

## Como gerar Android

```bash
npm run build
npx cap sync
npx cap open android
```

---

## API utilizada

JSONPlaceholder:

[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)

---

## Autor

Desenvolvido por Rafael Pancinha.

Projeto criado para estudo de Vue.js, Ionic, rotas, login, consumo de API e desenvolvimento mobile.

---

## Próximas melhorias

* Salvar login com localStorage
* Logout
* Tela de cadastro
* Consumo de API real
* Integração com banco de dados
* Melhorias visuais
* Geração de APK
