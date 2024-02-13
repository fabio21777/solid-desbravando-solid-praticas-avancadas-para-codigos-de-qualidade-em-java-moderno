
## PRINCÍPIO DA INVERSÃO DE DEPENDÊNCIAS (DIP): DEPENDÊNCIAS ESTÁVEIS



```java 
public class EmissorNotaFiscal {

private RegrasDeTributacao tributacao;

private LegislacaoFiscal legislacao;

private NotaFiscalDAO notas;

private EnviadorEmail email;

private EnviadorSMS sms;

  

// ...

public NotaFiscal gera(Fatura fatura) {

		List<Imposto> impostos = tributacao.verifica(fatura);
		
		List<Isencao> isencoes = legislacao.analisa(fatura);
		
		// método auxiliar
		
		NotaFiscal nota = aplica(impostos, isencoes);
		
		notas.salva(nota);
		
		String mensagemNota = String.format(
		
		"Sua nota fiscal foi gerada: %s",
		
		nota.getNumero());
		
		net.sargue.mailgun.Configuration configuration = new net.sargue.mailgun.Configuration()
		
		.domain("empresa.com")
		
		.apiKey(System.getenv("MAILGUN_API_KEY"))
		
		.from("Empresa", "contato@empresa.com");
		
		net.sargue.mailgun.Mail mail = net.sargue.mailgun.Mail.using(configuration)
		
		.to(fatura.getEmail())
		
		.subject("Sua nota fiscal")
		
		.text(mensagemNota)
		
		.build();
		
		email.envia(mail);
		
		com.twilio.rest.api.v2010.account.Message smsMessage = com.twilio.rest.api.v2010.account.Message.creator(
		
		new PhoneNumber("+14159352345"),
		
		new PhoneNumber(fatura.getTelefone()),
		
		mensagemNota)
		
		.create();
		
		sms.envia(smsMessage);
		
		return nota;
	
	}

}
```

Baseado no que foi visto, o SRP parece ser respeitado; porém, temos um problema relacionado às dependências.

## 4.1  ACOPLAMENTO, VOLATILIDADE

A dependência de classes consideradas estáveis, como as do Java Stream API, a classe String ou a API de leitura de arquivos `java.io`, geralmente não constitui um problema. No entanto, ao analisarmos o código mencionado, identificamos duas implementações que se mostram instáveis: `EnviadorEmail` e `EnviadorSMS`. Existem diversos cenários nos quais alterações nessas implementações se fazem necessárias. Quando isso acontece, todas as classes que dependem delas são impactadas, residindo aí a questão central do problema. As mudanças nas classes `EnviadorEmail` e `EnviadorSMS` não apenas exigem ajustes nas próprias classes, mas também afetam todas as outras que delas dependem, evidenciando a fragilidade dessa abordagem em termos de manutenção e evolução do código. 

## 4.2 ABSTRAÇÕES DEPENDÊNCIAS 

*Como minimizar os impactos de mudanças em dependências voláteis? Usando  abstrações! Podemos usar classes abstratas e,preferencialmente, interfaces. Abstrações são estáveis: mudam muito menos que implementações*.

![[Pasted image 20240212181506.png]]

*Repare bem nas setas da imagem anterior. Ambas as dependências de código apontam na direção da abstração*

### Evitando vazamento de detalhes de implementação
*Uma coisa importante é que devemos evitar que detalhes das dependências vazem nas nossas abstrações.*

*Um detalhe mais sutil na criação de abstrações é que precisamos evitar vinculá-las conceitualmente a tarefas muito particulares.*

### Abstrações, uma ideia antiga

*No artigo Design Principles and Design Patterns (MARTIN, 2000),
Uncle Bob declara que devemos "Depender de abstrações e não de
implementações.*

*No livro clássico Design Patterns (GAMMA etal., 1994)- gof  "Programe voltado à interface, não àimplementação.*

## 4.3 REGRAS DE NEGÓCIO X DETALHES

*Por isso, em suas palestras, Uncle Bob costuma dizer: a Web é um detalhe; o Banco de Dados é um detalhe. Deveríamos preparar o nosso código de negócio para "sobreviver" a mudanças completas nesses detalhes.*

## 4.4 CÓDIGO DE ALTO NÍVEL E BAIXO NÍVEL

Uncle Bob, no livro Agile Principles, Patterns, and Practices in C# (MARTIN, 2006), divide o código em dois tipos:
* Códigos de alto nível seriam os códigos que implementam regras de negócio.
* Códigos de baixo nível seriam os mecanismos de entrega, os detalhes de implementação mais técnicos.
### Alto/baixo nível e entradas/saídas
*No livro Clean Architecture (MARTIN, 2017), Uncle Bob diz que código de alto nível é aquele mais distante das entradas ou saídas do sistema e, por isso, muda menos frequentemente e por razões mais importantes, relacionadas ao negócio. Já o código de baixo nível, mais próximo das entradas ou saídas, muda mais frequentemente e com mais urgência.*

## 4.5 PRINCÍPIO DA INVERSÃO DE DEPENDÊNCIAS(DIP)

*DEPENDENCY INVERSION PRINCIPLE (DIP) Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações*.

*Se você está programando uma classe qualquer com regras de negócio e precisa depender de outro módulo, idealmente esse outro módulo deve ser uma abstração.*

### Módulos
*O que significa o termo "módulo" usado por Uncle Bob na definição do DIP? No livro Clean Architecture (MARTIN, 2017), Uncle Bob descreve um módulo como um conjunto coeso de
funções e dados ou, de maneira mais simples, um arquivo de código-fonte.
*

 Mecanismos de entrega - implementação concretas que o cliente terá acesso , regras de negócio não devem depender de mecanismos de entrega, mas de abstrações desses detalhes de implementação.


*"Interfaces de serviço, em geral, são do cliente (quem as usa)."Uncle Bob, no livro Agile Principles, Patterns, and Practices in C# (MARTIN, 2006).*

## 4.11 CONTRAPONTO: CRÍTICAS AO DIP

*No livro SOLID is not solid (COPELAND, 2019), David
Copeland critica o artigo Dependency Inversion Principle
(MARTIN, 1996c), em que há a argumentação inicial em torno do
DIP. Nesse artigo, Uncle Bob argumenta pelo DIP usando como
exemplo um programa que envia o que é digitado em um teclado
para uma impressora e que precisa ser adaptado para enviar os
dados para um arquivo. Segundo Copeland, é um exemplo
fabricado, longe do dia a dia de quem desenvolve. Além disso, o
exemplo usa I/O, que é um problema já resolvido na maioria das
linguagens de programação. Há ainda uma crítica à promessa de
flexibilidade e reúso que, de acordo com Copeland, leva a códigos
desnecessariamente complexos.*

