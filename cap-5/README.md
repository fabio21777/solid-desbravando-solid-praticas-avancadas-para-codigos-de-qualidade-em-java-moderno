# 5  PRINCÍPIO ABERTO/FECHADO (OCP): OBJETOS FLEXÍVEIS

*Não é estranho que toda abstração tenha somente uma
implementação? Talvez isso seja uma indicação de que podemos
repensar a organização do nosso código e criar abstrações
melhores. É o que faremos neste capítulo!*

## 5.1 EM BUSCA DA ABSTRAÇÃO PERFEITA

```java
public class EmissorNotaFiscal {
	private RegrasDeTributacao tributacao;
	private LegislacaoFiscal legislacao;
	private NotaFiscalDAO notas;
	private EnviadorEmail email;
	private EnviadorSMS sms;
	//...
	public NotaFiscal gera(Fatura fatura) {
		List<Imposto> impostos = tributacao.verifica(fatura);
		List<Isencao> isencoes = legislacao.analisa(fatura);
		// método auxiliar
		NotaFiscal nota = aplica(impostos, isencoes);
		notas.salva(nota);
		String mensagemNota = String.format(
		"Sua nota fiscal foi gerada: %s",
		nota.getNumero());
		email.envia(mensagemNota, fatura.getEmail());
		sms.envia(mensagemNota, fatura.getTelefone());
		return nota;
	}
}
```
Repare que, ao final do método gera de EmissorNotaFiscal , são feitas várias ações:
* Persistir a nota gerada no BD através de NotaFiscalDAO e sua implementação NotaFiscalDAOComHibernate .
* Enviar uma notificação por e-mail através de EnviadorEmail e sua implementação EnviadorEmailComMailgun .
* Enviar uma notificação por SMS através de EnviadorSMS e sua implementação EnviadorSMSComTwilio .

Mas podemos ir além: todas essas ações, de persistência ou de notificação, são um conjunto de ações que devem ser feitas após a emissão da nota fiscal. Que tal uma abstração chamada AcaoPosEmissao ? Tanto NotaFiscalDAOComHibernate quanto
EnviadorEmailComMailgun e EnviadorSMSComTwilio seriam implementações.

![[Pasted image 20240213182943.png]]


*Depende da funcionalidade que queremos implementar. Código mais genérico é mais flexível mas, por outro lado, pode ser mais difícil de entender.*

```java
public class EmissorNotaFiscal {
	private RegrasDeTributacao tributacao;
	private LegislacaoFiscal legislacao;
	private List<AcaoPosEmissao> acoes;
	//...
	public NotaFiscal gera(Fatura fatura) {
		List<Imposto> impostos = tributacao.verifica(fatura);
		List<Isencao> isencoes = legislacao.analisa(fatura);
		// método auxiliar
		NotaFiscal nota = aplica(impostos, isencoes);
		// modificado
		for (AcaoPosEmissao acao: acoes) {
			acao.faz(nota);
		}
		return nota;
	}
}
```

Para esse caso específico, utilizaríamos qualquer ação pós-emissão que implemente a interface AcaoPosEmissao, através do pattern chamado command.

*Protegidas no artigo Protected
Variation (LARMAN, 2001): "Identifique pontos previstos de
variação e crie uma interface estável em torno deles."
*
*"Encontrar os pontos onde sua modelagem precisa ser flexível e
onde não precisa é um desafio."
Maurício Aniche, no livro OO e SOLID para Ninjas
	(ANICHE, 2015)*
## 5.3 O PRINCÍPIO ABERTO/FECHADO (OCP)
Entidades de software (classes, módulos, funções etc.) devem ser abertas para extensão, mas fechadas para modificação.

### OCP e DIP

No mesmo artigo Design Principles and Design Patterns, Uncle Bob compara os princípios DIP e OCP, mostrando que são complementares: "Se o OCP declara o objetivo de uma arquitetura OO, o DIP declara o seu mecanismo fundamental." O DIP fomenta a classificação de dependências em alto/baixo nível e a inversão das dependências em direção às regras de negócio. Isso conduz a abstrações que levam a um código flexível, que pode ser estendido sem modificá-lo: o intuito do OCP.


### 5.7 DAS CONDICIONAIS AO POLIMORFISMO

*"Objetos têm um mecanismo fabuloso, mensagens polimórficas,
que permitem expressar lógica condicional de maneira flexível
mas clara.
Ao trocar condicionais explícitos por mensagens polimórficas,
comumente você consegue:
reduzir duplicação, tornar seu código mais claro e aumentar a flexibilidade.
Tudo ao mesmo tempo."
Kent Beck, no livro Refactoring (FOWLER et al., 1999)

*provocadora campanha Anti-IF (CIRILLO, 2009): "Muitos times
querem ser ágeis, mas têm dificuldade em reduzir a complexidade de
código. Vamos começar com algo concreto: saber como usar objetos
de uma maneira que permita que os desenvolvedores eliminem os
IFs ruins, aqueles que frequentemente comprometem a flexibilidade
e a habilidade de evoluir do software."*

### Usando o poder das enums

Com essa solução removeremos o if do código

```java
/*	if ("pdf".equals(formato)) {
	gerador = new GeradorPDF();
	} else if ("epub".equals(formato)) {
	gerador = new GeradorEPUB();
	} else {
	throw new IllegalArgumentException(
	"Formato do ebook inválido: " + formato);
	}
	return gerador*/

// novo código
public enum FormatoEbook {
	PDF(new GeradorPDF()),
	EPUB(new GeradorEPUB());
	private GeradorEbook gerador;

	FormatoEbook(GeradorEbook gerador) {
		this.gerador = gerador;
	}

	public GeradorEbook getGerador() {
		return gerador;
	}
}

static GeradorEbook cria(FormatoEbook formato) {
	return formato.getGerador();
}

```

### 5.8

#### Para saber mais: OCP e Spring

*Com um framework de Dependency Injection como o Spring,
podemos implementar o OCP de maneira muito elegante e flexível.
Vamos considerar que temos a interface GeradorEbook com três
implementações: a classe GeradorPDF , a classe GeradorEPUB e a
classe GeradorHTML . Todas essas implementações estariam
anotadas com @Component , conforme mencionado no capítulo
sobre o DIP.
Em um projeto cujas dependências são gerenciadas pelo
Spring, ao injetarmos listas de uma determinada interface,
obtemos todos os componentes gerenciados pelo framework que
são implementações da interface.
Podemos modificar a classe Cotuba , que transformamos em
um @Component do Spring no capítulo anterior, para que receba
uma lista com todos os*

#### Para saber mais: o OCP de Bertrand Meyer

*Os critérios de Meyer para código modular são:
decomponibilidade,
composibilidade,
compreensibilidade,
continuidade e proteção.
As regras da modularidade do autor são: mapeamento direto,
poucas interfaces, interfaces pequenas, interfaces explícitas e
ocultação de informação (information hiding).*

### 5.9 CONTRAPONTO: CRÍTICAS AO OCP

*Dan North, no artigo CUPID - the back story (NORTH, 2021),
liga o OCP a um contexto já ultrapassado, em que modificar
software era caro e arriscado. Com técnicas modernas como
refatoração e TDD, o código é muito mais maleável. Dessa forma,
código deixa de ser um ativo a ser preservado e passa a ser um
custo a ser minimizado. De acordo com North, nesse contexto
atual, escrever código simples e mais específico passaria a fazer
mais sentido.*

*David Copeland diz, no livro SOLID is not solid (COPELAND,
2019), que o OCP de Meyer estaria relacionado com um contexto
em que a compilação de código era muito demorada; software
ainda não era distribuído pela internet, mas por cópias físicas
como CD-ROM; e sistemas eram construídos em torno de
bibliotecas compartilhadas. Nesse contexto, recompilar e
redistribuir o sistema como um todo era bastante oneroso tanto
em tempo como em dinheiro. Por isso, seria interessante criar
softwares extensíveis, mesmo que com código mais difícil de
entender.*

*Copeland critica também a descrição do OCP por Uncle Bob,
que daria a entender que todas as classes deveriam ser altamente
extensíveis. Copeland conclui: "(...) não adicione toneladas de
flexibilidade da primeira vez só porque você pode precisar mudar as
coisas depois."*

*Kevlin Henney, na palestra The SOLID Design Principles
Deconstructed (HENNEY, 2013), resgata o texto do livro Object-
Oriented Software Construction (MEYER, 1988) de Bertrand
Meyer, que é citado por Uncle Bob como a inspiração do OCP.*

*Henney mostra que a motivação por trás do OCP original era
reúso e que estava ligado a versionamento de módulos. Diz ainda
que quando o livro foi escrito, na década de 80, o uso de sistemas
de controle de versão e gerenciamento de dependências ainda não
era comum. Henney tece um paralelo com o conceito de published
interfaces (interfaces publicadas) do livro Refactoring (FOWLER et
al., 1999) de Martin Fowler: classes ou interfaces que são usadas
fora da base de código que as define.*

*Ted Kaminski, em seu artigo Deconstructing SOLID design
principles (KAMINSKI, 2019), argumenta que extensibilidade
deveria ser um objetivo apenas nas bordas do sistema: as interfaces
expostas publicamente para desenvolvedores de terceiros. Código
que não está exposto nas bordas do sistema é barato de modificar e
não precisa ser extensível: "(...) você pode até dizer que deveríamos
ser abertos para modificação."*
