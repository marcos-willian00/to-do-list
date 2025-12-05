# To-Do List API

Uma aplicação backend de gerenciamento de tarefas desenvolvida com **Spring Boot 3.0** e **Java 21**, com autenticação de usuários e segurança integrada.

## 📋 Sobre o Projeto

Esta é uma API REST para gerenciamento de tarefas que permite:
- **Criar e gerenciar usuários** com autenticação segura
- **Criar, listar e atualizar tarefas** associadas a cada usuário
- **Controle de acesso** garantindo que usuários só vejam suas próprias tarefas
- **Validação de datas** para garantir que as tarefas tenham prazos válidos

## 🛠️ Tecnologias Utilizadas

- **Java 21**
- **Spring Boot 3.0.11**
- **Spring Data JPA** - ORM para acesso a dados
- **H2 Database** - Banco de dados em memória para desenvolvimento
- **BCrypt** - Hash seguro de senhas
- **Maven** - Gerenciador de dependências

## 📦 Dependências Principais

```xml
- spring-boot-starter-web (REST API)
- spring-boot-starter-data-jpa (Persistência)
- h2 (Banco de dados)
- BCrypt (Criptografia de senhas)
- spring-boot-starter-test (Testes)
```

## 🚀 Como Executar

### Pré-requisitos
- Java 21+ instalado
- Maven instalado

### Passos

1. **Clone o repositório**
   ```bash
   git clone https://github.com/marcos-willian00/to-do-list.git
   cd todolist
   ```

2. **Compile e execute a aplicação**
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```

3. **Acesse a aplicação**
   - API está disponível em: `http://localhost:8080`
   - Console H2: `http://localhost:8080/h2-console`

## 📚 Endpoints da API

### Usuários

#### Criar Usuário
```http
POST /users/
Content-Type: application/json

{
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

**Resposta (201 Created):**
```json
{
  "id": "uuid",
  "username": "seu_usuario",
  "password": "hash_bcrypt"
}
```

### Tarefas

#### Criar Tarefa
```http
POST /tasks/
Authorization: Basic [base64(username:password)]
Content-Type: application/json

{
  "description": "Descrição da tarefa",
  "title": "Título da tarefa",
  "startAt": "2024-12-10T09:00:00",
  "endAt": "2024-12-10T17:00:00",
  "priority": "HIGH"
}
```

#### Listar Tarefas
```http
GET /tasks/
Authorization: Basic [base64(username:password)]
```

**Resposta (200 OK):**
```json
[
  {
    "id": "uuid",
    "idUser": "uuid",
    "title": "Título da tarefa",
    "description": "Descrição",
    "startAt": "2024-12-10T09:00:00",
    "endAt": "2024-12-10T17:00:00",
    "priority": "HIGH",
    "createdAt": "2024-12-05T10:00:00"
  }
]
```

#### Atualizar Tarefa
```http
PUT /tasks/{id}
Authorization: Basic [base64(username:password)]
Content-Type: application/json

{
  "description": "Nova descrição",
  "title": "Novo título",
  "priority": "MEDIUM"
}
```

## 🔐 Autenticação

A aplicação utiliza **autenticação básica HTTP** com:
- Verificação de credenciais no filtro `FilterTaskAuth`
- Senhas criptografadas com BCrypt (12 rounds)
- Validação em todas as operações de tarefas

### Exemplo com cURL:
```bash
curl -u usuario:senha http://localhost:8080/tasks/
```

## 🏗️ Estrutura do Projeto

```
src/main/java/br/com/marcoswillian/todolist/
├── TodolistApplication.java          # Classe principal
├── user/
│   ├── UserController.java           # Endpoints de usuário
│   ├── UserModel.java                # Modelo de usuário
│   └── IUserRepository.java          # Repository do usuário
├── task/
│   ├── TaskController.java           # Endpoints de tarefa
│   ├── TaskModel.java                # Modelo de tarefa
│   └── ITaskRepository.java          # Repository de tarefa
├── filter/
│   └── FilterTaskAuth.java           # Filtro de autenticação
├── errors/
│   └── ExceptionHandlerController.java # Tratamento de erros
└── utils/
    └── Utils.java                    # Utilitários
```

## 🗄️ Configuração do Banco de Dados

O projeto utiliza **H2 Database** (banco em memória) configurado em `application.properties`:

```properties
spring.datasource.url=jdbc:h2:mem:todolist
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=admin
spring.datasource.password=admin
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

## 🐳 Docker

Um `Dockerfile` está incluído no projeto para containerização:

```bash
docker build -t todolist-api .
docker run -p 8080:8080 todolist-api
```

## ✅ Validações

- ✔️ Usuários duplicados são rejeitados
- ✔️ Senhas são obrigatoriamente criptografadas
- ✔️ Datas de início e fim devem ser no futuro
- ✔️ Data de início deve ser menor que data de fim
- ✔️ Usuários só acessam suas próprias tarefas

## 📝 Testes

Execute os testes unitários com:

```bash
mvn test
```

## 👤 Autor

**Marcos Willian**
- GitHub: [@marcos-willian00](https://github.com/marcos-willian00)

## 📄 Licença

Este projeto é fornecido como está. Consulte o arquivo LICENSE para mais detalhes.

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests.

---

**Desenvolvido durante o bootcamp Rocketseat** 🚀
