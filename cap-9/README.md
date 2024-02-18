# UM POUCO DE IMUTABILIDADE E ENCAPSULAMENTO

## 9.1 EM DIREÇÃO À IMUTABILIDADE

Algumas vantagens de objetos imutáveis citadas por Bloch no mesmo livro:

* São simples: só há um estado possível.
* São thread-safe: como não há mudança de estado, não há problemas no acesso concorrente aos mesmos objetos
* Podem ser compartilhados e reutilizados livremente.

*"Classes devem ser imutáveis ao menos que haja uma boa razão para torná-las mutáveis. (...) Se uma classe não puder ser imutável, limite sua mutabilidade o máximo possível." Joshua Bloch, no livro Effective Java (BLOCH, 2001)*


*Na plataforma Java, é muito difícil criar código 100% imutável.
As especificações e as bibliotecas que as implementam, em geral,
requerem JavaBeans. É assim com JPA/Hibernate, com o JSF, com
o JAX-B, com o JavaFX, entre outros. E um JavaBean precisa de
getters, setters e de um construtor sem parâmetros.
Poderíamos deixar nossos objetos de alto nível como
imutáveis, criando JavaBeans específicos para cada tecnologia da
plataforma Java. Quando necessário, faríamos a conversão de/para
os objetos imutáveis. Porém, quanto mais intermediários no
código, mais complexidade.*

## 9.5 FAVORECENDO IMUTABILIDADE COM RECORDS

Um record é mais um tipo que pode ser declarado no Java, além de class , interface , @interface e enum . Um record possui um nome e "componentes", seus parâmetros, que declaram seu estado e são usados para gerar automaticamente:

* um atributo private final , imutável, para cada componente, com o mesmo nome e tipo
* um método de acesso public para cada componente, com o mesmo nome do componente e com retorno do mesmo tipo;
* um construtor "canônico", que recebe cada componente e atribui ao atributos privados;


## 9.6 ENCAPSULAMENTO

A classe Tributacao é invejosa. Veja dados de outras classes que são manipulados:

nota.getEndereco().getEstado()
nota.getEmpresa().getSegmento().getSetor()
nota.getEmpresa().getRegime()
nota.getItens()
item.getValor()

*No mesmo artigo, Andy Hunt e Dave Thomas sugerem que código bom é tímido: "O objetivo fundamental (...) é escrever código tímido: código que não revela muito de si para ninguém e não conversa com os outros mais do que o necessário. Código tímido evita contato com os outros, não é como aquele vizinho fofoqueiro que está envolvido nas idas e vindas de todo mundo. Código tímido nunca mostraria suas coisas “privadas” para os “amigos” (...). Assim como no mundo real, boas cercas fazem bons vizinhos – contantoque você não olhe pela cerca."*

