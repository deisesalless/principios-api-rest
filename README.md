### Os 6 Princípios da API RESTful

RESTful é um estilo de arquitetura para desenvolvimento de APIs que se baseia nos princípios da Web e no protocolo HTTP. Para que uma API seja considerada RESTful ela precisa todos seguir os princípios:

---

1. **Cliente-Servidor**
   - Esse princípio define uma separação clara entre o cliente (frontend, app mobile, etc.) e o servidor (backend que fornece os dados).
   - Exemplo: uma aplicação React consome os dados de uma API feita em Java com Spring Boot. O frontend só se preocupa com a interface e o backend com a lógica e o armazenamento.

---

2. **Interface Uniforme**
   - A comunicação entre cliente e servidor deve seguir um padrão bem definido, com:
     - URIs (Identificadores de Recurso Uniformes) organizadas
     - Métodos HTTP padronizados (GET, POST, PUT, DELETE)
     - Respostas previsíveis e bem estruturadas

   #### 🧠 O que é uma URI?
   URI (Uniform Resource Identifier) é o endereço usado para acessar um recurso da API. Exemplo: `/usuarios/1` representa o usuário com ID 1.

   #### 🔧 Exemplo em Java com Spring Boot

   - **GET: Buscar um usuário por ID**
     ```java
     @GetMapping("/{id}")
     public ResponseEntity<Usuario> buscarPorId(@PathVariable Long id) {
         Usuario usuario = usuarioService.buscarPorId(id);
         return ResponseEntity.ok(usuario);
     }
     ```
     - Requisição: `GET /usuarios/1`
     - Resposta: JSON com os dados do usuário de ID 1

   - **POST: Criar um novo usuário e retornar a URI do novo recurso**
     ```java
     @PostMapping
     public ResponseEntity<UsuarioDTO> criar(@RequestBody @Valid UsuarioCreateDTO dto) {
         UsuarioDTO novoUsuario = usuarioService.salvar(dto);

         URI location = ServletUriComponentsBuilder.fromCurrentRequest()
                 .path("/{id}")
                 .buildAndExpand(novoUsuario.getId())
                 .toUri();

         return ResponseEntity.created(location).body(novoUsuario);
     }
     ```
     - Requisição: `POST /usuarios` com JSON no corpo
     - Resposta:
       - **HTTP 201 Created**
       - Header: `Location: http://localhost:8080/usuarios/123`
       - Corpo: dados do usuário criado

---

3. **Stateless (Sem Estado)**
   - Cada requisição deve conter todas as informações necessárias, sem depender de estados mantidos no servidor.
   - Exemplo: o cliente envia o token de autenticação em todas as requisições no header `Authorization`.

---

4. **Cacheável**
   - As respostas devem informar se podem ou não ser armazenadas em cache pelo cliente ou por intermediários.
   - Exemplo: uma lista de produtos com pouca atualização pode ser cacheada por 24 horas para melhorar a performance e reduzir chamadas ao servidor.

---

5. **Camadas**
   - A API pode ser estruturada em camadas intermediárias, como proxies, gateways ou serviços de segurança, sem que o cliente precise saber disso.
   - Exemplo: o cliente faz requisições para um API Gateway, que repassa para o serviço certo, aplica autenticação, log, etc. Em ambientes em nuvem, também é comum o uso de CDNs (Content Delivery Networks), que distribuem o conteúdo da aplicação em servidores ao redor do mundo, tornando o acesso mais rápido para usuários de diferentes regiões.

---

6. **Código Sob Demanda (Opcional)**
   - A API pode fornecer código executável (como scripts JavaScript) para ser executado no cliente, se necessário. Este princípio é raro em APIs REST modernas, mas é permitido.

---

Seguir esses princípios ajuda a tornar sua API mais **organizada, previsível, escalável e fácil de manter**. É um padrão adotado mundialmente em aplicações modernas.

Bônus: [Status Code do HTTP, qual escolher de forma correta](https://github.com/deisesalless/principios-api-rest/blob/main/protocol-http.jpeg)
