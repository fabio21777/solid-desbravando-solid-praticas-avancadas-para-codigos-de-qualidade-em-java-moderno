# PRINCÍPIO DA SEGREGAÇÃO DE INTERFACES (ISP): CLIENTES SEPARADOS, INTERFACES SEPARADAS

## 8.2

```java
public class NotaFiscal {
	private Cliente cliente;
	private Endereco entrega;
	private Endereco cobranca;
	private List<Item> itens = new ArrayList<>();
	private List<Desconto> descontos = new ArrayList<>();
	private FormaDePagamento pagamento;
	private BigDecimal valorTotal;
	// getters...
	// restante do código...
}
```
*A classe NotaFiscal do código anterior possui vários atributos relacionados à emissão de notas fiscais. Vamos considerar que a complexidade da classe é justificada pela dificuldade do problema que está sendo resolvido.*

```java
public class CalculadoraDeImposto {
	private static final BigDecimal VALOR_LIMITE = new BigDecimal(1000);
	private static final BigDecimal TAXA_MAIOR = new BigDecimal("0.02");
	private static final BigDecimal TAXA_MENOR = new BigDecimal("0.01");
	// nota fiscal usada aqui
	public BigDecimal calcula(NotaFiscal nota) {
		BigDecimal total = BigDecimal.ZERO;
		for (Item item : nota.getItens()) { // e também aqui
		if (item.getValor().compareTo(VALOR_LIMITE) > 0) {
			total = total.add(item.getValor().multiply(TAXA_MAIOR));
		} else {
			total = total.add(item.getValor().multiply(TAXA_MENOR));
		}
		}
		return total;
	}
}
```

Perceba que a classe CalculadoraDeImposto usa apenas o método getItens da NotaFiscal . Para o cálculo de impostos, não precisamos de outras informações da nota fiscal.


No caso da CalculadoraDeImposto , poderíamos separar apenas aquilo que desejamos da classe NotaFiscal por meio de uma interface bem enxuta. Nesse caso específico, estamos apenas interessados nos itens da nota:

```j́ava
public class NotaFiscal implements Tributavel {
	public List<Item> itensASeremTributados() {
		return itens;
	}
}
```

*Não teríamos mais acesso a todas as informações da nota fiscal. Apenas às informações necessárias.*

*"Se você tiver uma classe que tenha vários clientes, em vez de carregar a classe com todos os métodos de que os clientes precisam, crie interfaces específicas para cada cliente e implemente-as na classe." Uncle Bob, no artigo Design Principles and Design Patterns (MARTIN, 2000)*


## 8.4 - CONTRAPONTO: CRÍTICAS AO ISP

*Dan North, no artigo CUPID - the back story (NORTH, 2021),
conta a história que o ISP foi cunhado por Uncle Bob enquanto
quebrava uma classe massiva de um software de impressão da
Xerox em classes menores. Por isso, North diz que o ISP não é um
princípio, mas um pattern: uma estratégia que funciona em um
determinado contexto e traz vantagens e desvantagens. O autor
argumenta ainda que não haveria a necessidade de quebrar uma
classe grande e emaranhada se o código já fosse composto por
classes menores.*

*David Copeland diz, no livro SOLID is not solid (COPELAND,
2019), que o ISP só faz sentido em linguagens com tipagem estática
como Java e Scala. Copeland diz ainda que a necessidade do ISP só
surge quando há classes pouco coesas e que o ISP levado ao
extremo leva a uma proliferação de interfaces de apenas um
método. O autor conclui afirmando que o ISP é uma reformulação
complicada sobre a necessidade de coesão.*

*Ted Kaminski argumenta, em seu artigo Deconstructing SOLID
design principles (KAMINSKI, 2019), o ISP é vago e abre espaço
para algumas interpretações. Uma das interpretações é que
interfaces pouco coesas são tão destrutivas quanto objetos pouco
coesos e que múltiplas interfaces são melhores que uma
megainterface única porque controlam e limitam dependências
públicas. Uma outra interpretação está ligada à evolução de
plugins e APIs que precisam dar suporte a código legado e que precisam manter versões anteriores de suas interfaces públicas.
Essa segunda interpretação, para Kaminski, não é um princípio
para ser adotado inicialmente, mas uma ferramenta para lidar com
as fronteiras de um sistema.

*


