#  Spring Security Application

## Descrição

Esta aplicação é um exemplo de como configurar e utilizar o Spring Security com OAuth2 e JWT em uma aplicação Spring Boot. A aplicação possui rotas públicas e privadas, além de demonstrar como obter informações do usuário autenticado e do token JWT.

## Estrutura do Projeto

- `src/main/java/com/example/aulaospringsecurity/`
  - `AulaoSpringSecurityApplication.java`: Classe principal da aplicação.
  - `HttpController.java`: Controlador REST que define as rotas da aplicação.
- `src/test/java/com/example/demo/`
  - `DemoApplicationTests.java`: Classe de testes para verificar se o contexto da aplicação carrega corretamente.

## Dependências

A aplicação utiliza as seguintes dependências principais:

- Spring Boot Starter Web
- Spring Boot Starter Security
- Spring Security OAuth2 Client
- Spring Security OAuth2 Resource Server
- Spring Boot Starter Test (para testes)

## Configuração de Segurança

A configuração de segurança é definida na classe `SecurityConfig`. Esta classe configura a segurança da aplicação, incluindo a definição de um `JwtDecoder` para decodificar tokens JWT.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
                .authorizeHttpRequests(
                        authorizeConfig -> {
                            authorizeConfig.requestMatchers("/public").permitAll();
                            authorizeConfig.requestMatchers("/logout").permitAll();
                            authorizeConfig.anyRequest().authenticated();
                        })
                .oauth2Login(Customizer.withDefaults())
                .oauth2ResourceServer(config -> {
                    config.jwt(Customizer.withDefaults());
                })
                .build();
    }

    @Bean
    public JwtDecoder jwtDecoder() {
        return NimbusJwtDecoder.withJwkSetUri("https://example.com/.well-known/jwks.json").build();
    }
}

Rotas
A aplicação define as seguintes rotas:  
GET /public: Rota pública acessível a todos.
GET /private: Rota privada acessível apenas a usuários autenticados.
GET /cookie: Rota que exibe informações do usuário autenticado via OAuth2.
GET /jwt: Rota que exibe informações do token JWT.
Executando a Aplicação
Para executar a aplicação, utilize o comando Maven:

mvn spring-boot:run

Testes
Para executar os testes, utilize o comando Maven:

mvn test
