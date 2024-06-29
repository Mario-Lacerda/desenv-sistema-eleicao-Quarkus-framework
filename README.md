
# Desafio Dio - Desenvolvendo um Sistema para Eleição Usando Quarkus Framework



**Projeto Desenvolver um Sistema para Eleição Usando Quarkus Framework com Códigos Completos**



O Quarkus é um framework Java moderno e de alto desempenho projetado para desenvolver aplicações nativas em nuvem. Ele oferece recursos como inicialização rápida, baixo consumo de memória e suporte a microsserviços, tornando-o uma escolha ideal para criar sistemas escaláveis e eficientes.



### **Requisitos**

- Java 11 ou superior
- Maven ou Gradle
- Quarkus Framework



### **Estrutura do Projeto**

O projeto será estruturado em vários módulos, incluindo:

- **Módulo de API:** Contém as APIs RESTful para gerenciar eleições, candidatos e votos.
- **Módulo de Serviço:** Implementa a lógica de negócios para as APIs, incluindo validação de dados e persistência.
- **Módulo de Dados:** Gerencia a persistência de dados usando um banco de dados relacional ou NoSQL.
- **Módulo de Segurança:** Fornece autenticação e autorização para as APIs.



### **Implementação**



### **Módulo de API**



java



```java
@Path("/eleicoes")
public class EleicaoResource {

    @Inject
    EleicaoService eleicaoService;

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public Response criarEleicao(Eleicao eleicao) {
        try {
            eleicaoService.criarEleicao(eleicao);
            return Response.status(201).build();
        } catch (Exception e) {
            return Response.status(400).entity(e.getMessage()).build();
        }
    }

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Eleicao> listarEleicoes() {
        return eleicaoService.listarEleicoes();
    }
}
```



### **Módulo de Serviço**

java



```java
public class EleicaoService {

    @Inject
    EleicaoRepository eleicaoRepository;

    public void criarEleicao(Eleicao eleicao) {
        eleicaoRepository.save(eleicao);
    }

    public List<Eleicao> listarEleicoes() {
        return eleicaoRepository.findAll();
    }
}
```



### **Módulo de Dados**

java



```java
@Entity
@Table(name = "eleicoes")
public class Eleicao {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private Date dataInicio;
    private Date dataFim;

    // Outros atributos e relacionamentos
}

public interface EleicaoRepository extends JpaRepository<Eleicao, Long> {}
```



### **Módulo de Segurança**

java



```java
@ApplicationPath("/api")
public class SecurityConfig extends Application {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
                .withUser("admin")
                .password("{noop}admin")
                .roles("ADMIN");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/eleicoes/**").hasRole("ADMIN")
                .anyRequest().permitAll()
                .and()
                .formLogin()
                .and()
                .httpBasic();
    }
}
```

 



### **Conclusão**

Este projeto fornece uma base abrangente para desenvolver um sistema de eleição usando o Quarkus Framework. Ele aborda aspectos essenciais como gerenciamento de eleições, candidatos e votos, garantindo segurança e escalabilidade. Com sua inicialização rápida e baixo consumo de memória, o Quarkus torna o desenvolvimento e a implantação de sistemas de eleição eficientes e confiáveis.

