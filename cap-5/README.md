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

## Usando o poder das enums

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


