# PRINCÍPIO DA RESPONSABILIDADE ÚNICA (SRP): CLASSES COESAS


## 3.1 exemplo é inspirado no livro UML for Java Programmers de Robert C. Martin

```java
public class Empregado {
	public BigDecimal calculaPagamento() {
	//...
	}
	public BigDecimal calculaTaxas() {
	//...
	}
	public BigDecimal calculaHorasExtras(List<Hora> horas) {
	//...
	}
	public void salva() {
	// persiste no Banco de Dados...
	}
	public static Empregado buscaPorId(Long id) {
	// busca do Banco de Dados ...
	}
	public String convertePraXML() {
	//...
	}
	public static Empregado leXML(String xml) {
	//...
	}
}
```

Como você pode ver no código acima, a classe Empregado realiza as seguintes tarefas:

* Calcula o pagamento do empregado;
* Calcula taxas referentes ao empregado;
* Calcula as horas extras do empregado;
* Salva o empregado no Banco de Dados;
* Busca o empregado do Banco de Dados pelo id;
* Converte o empregado para um arquivo XML;
* Lê o empregado de um arquivo XML.

*Para analisarmos se essa classe tem um bom design, podemos
agrupar essas tarefas em alguns conjuntos de "interessados", tanto
relacionados ao negócio como à infraestrutura técnica*

Como a classe Empregados há muitos interessados, ao modificamos qualquer um dos interesses, podemos modificar interesses que não estão associados ao interesse modificado. por isso a classe empregado não possui um bom designer. 

*Cada responsabilidade de uma classe é
um motivo diferente para mudarmos seu código. Portanto, muitas
responsabilidades são muitas razões para a classe ser modificada.*


## 3.1 O PRINCÍPIO DA RESPONSABILIDADE ÚNICA (SRP)

*Uma classe deve ter um, e apenas um, motivo para ser modificada. - Uncle Bob*

*Responsabilidade Única não é o mesmo que fazer apenas uma coisa*

No livro Clean Architecture, a uma citação de  Uncle Bob, que o mesmo questiona o nome SRP, que uma classe não deve apenas fazer uma coisa, o conceito que vale para o srp é na verdade é motivo que leva uma classe a ser modificada 

*Um módulo deve ser responsável por um, e apenas um, ator. - Uncle Bob* *


## 3.2
## artigo Cohesion

* adesão, quando algo externo, como uma cola ou fita crepe, gruda coisas não relacionadas;
* coesão quando a união é natural, como dois pedaços de argila ou duas peças de uma máquina.

### coesão
quando uma linha de raciocínio é composta por pensamentos que se encaixam, se relacionam e que caminham juntos.Os conceitos que essas classes representam estariam relacionados e separá-los seria pouco natural


## 3.3  RESPONSABILIDADES E SRP NO COTUBA

### Ouvindo os imports

*Uma maneira de avaliar se uma classe adere ao SRP é verificar se suas dependências são coerentes entre si.*

### 3.4 NÃO SE REPITA

Evite repetições, normalmente quando uma trecho de código começar a repetir é o  sinal que uma nova classe precisa ser crrada 


*Todo bloco de conhecimento deve ter uma representação única, sem ambiguidades e dominante num sistema. - Pragmatic Programmer (HUNT; THOMAS,1999),*

*Dependência é o principal problema em desenvolvimento de software. Duplicação é o sintoma. Mas, ao contrário da maioria dos problemas na vida, nos quais eliminar os sintomas faz com que um problema mais grave apareça em outro lugar, eliminar duplicação nos programas elimina a dependência.- Kent Beck, no livro TDD by Example (BECK, 2002)*


### 3.6  MVC, ENTIDADES E CASOS DE USO

é um pattern arquitetural usado como um molde pra distribuição de responsabilidades em trechos de código que tratam de interfaces com o usuário (UI). Há três responsabilidades preestabelecidas:

* View: contém a lógica que monta a UI (telas ou equivalente) e que trata a entrada de dados, recebendo eventos do usuário (clique, digitação etc.). As interações do usuário são repassadas para o Controller.  A View pode buscar dados diretamente do Model para exibição na UI.
* Controller: o "meio de campo", recebe interações do usuário da View e colabora com o Model para enviar e obter dados, que são repassados para a View.
* Model: o resto do código, que não tem a ver com UI. Realiza cálculos, regras de negócio persistência, integrações com outros sistemas etc.
#### No livro Patterns of Enterprise Application Architecture
No livro Patterns of Enterprise Application Architecture (FOWLER et al., 2002), Martin Fowler destaca que a separação da apresentação do modelo é uma das ideias mais fundamentais do bom design porque:(FOWLER et al., 2002), Martin Fowler destaca que a separação da apresentação do modelo é uma das ideias mais fundamentais do

bom design porque:
* apresentação e modelo têm a ver com diferentes preocupações
* o mesmo modelo pode ter diferentes apresentações
* é mais fácil de criar testes automatizados para a lógica de domínio do que para o código de UI 

#### Entidades e casos de uso

As entidades são objetos que contém dados e funções que disponibilizam regras de negócio críticas. Devem ser totalmente independentes de como os dados são armazenados em um Bancos de Dados, de como são apresentados em uma UI e dos frameworks
utilizados.

Já os casos de uso são objetos que especificam as requisições do usuário, as respostas que serão produzidas e os passos de processamento envolvidos em produzir essas respostas. Casos de uso são específicos para uma aplicação e controlam como e quando as regras de negócio das entidades são invocadas.

*"Casos de uso controlam a dança das entidades." Uncle Bob, no livro Clean Architecture (MARTIN, 2017)*


### 3.7 RESPONSABILIDADES EM PACOTES E MÉTODOS

*No artigo Modularity and testability (BROWN, 2014), Simon Brown faz uma provocação: "devemos fazer uma doação para a caridade toda vez que digitarmos public class sem pensar se essa classe realmente precisa ser pública."*


*para Uncle Bob, um método deveria fazer apenas uma coisa. Se estiver fazendo muitas coisas, devemos quebrá-los em métodos auxiliares, menores e, provavelmente, privados*

### 3.8  CONTRAPONTO: CRÍTICAS AO SRP

*Em seu vídeo SOLID #1: Uma reflexão sobre o Princípio da responsabilidade única (SOUZA, 2020), Alberto Souza critica a subjetividade do SRP. Aponta ainda a confusão na interpretação do princípio pela indústria de software, o que muitas vezes leva a uma proliferação de classes com apenas um método. Alberto indica o uso de abordagens mais como a técnica CDD (Cognitive Driven Development), que sugere um design de código focado em torno da facilidade de entendimento do código.*

*No livro SOLID is not solid (COPELAND, 2019), David Copeland também critica o SRP por ser vago na definição do que é "responsabilidade" e no que seriam as razões para modificar uma classe.*

*escreva código simples de entender, dado que haja conhecimento na linguagem de programação e no domínio de negócio. - CUPID - the back story (NORTH, 2021)*

