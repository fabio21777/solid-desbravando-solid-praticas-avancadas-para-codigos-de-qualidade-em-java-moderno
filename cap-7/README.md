# PRINCÍPIO DE SUBSTITUIÇÃO DE LISKOV (LSP): HERANÇA DO JEITO CERTO

## 7.4 - OO não é só herança

Logo depois de uma introdução a OO, a ideia de herança é aque fica. Ao falar em OO, o que vem à mente, em geral, é: especialização, subclasses e sobrescrita de métodos. A tentação de herdar de uma superclasse para reaproveitar código é muito forte. Mas um especialista em OO raramente usa esses recursos. Então, quando usar herança é justificado?

*"Se um gato possui raça e patas, e um cachorro possui raça,
patas e tipoDoPelo, logo Cachorro extends Gato ? Pode
parecer engraçado, mas é (...) herança por preguiça, por
comodismo, porque vai dar uma ajudinha. A relação “é um”
não se encaixa aqui, e vai nos gerar problemas."
Paulo Silveira, no artigo Como não aprender orientação a
objetos: Herança (SILVEIRA, 2006)*

## 7.4 O PRINCÍPIO DA SUBSTITUIÇÃO DE LISKOV (LSP)

É o princípio da substituibilidade. Quando usamos herança do jeito certo, deve ser possível usar qualquer método das classes filhas ao recebermos uma variável cujo tipo é o da classe mãe.


## 7.5 FAVORECENDO COMPOSIÇÃO À HERANÇA

*Ter uma referência de um objeto como atributo privado, conforme o código anterior, é algo que chamamos de composição.Fazer uma composição com uma dependência é preferível a usar herança, criando um tipo especial dessa dependência.*

* Design Patterns (GAMMA et al., 1994): "Favoreça composição à herança."

* Effective Java (BLOCH, 2001): "Item 14: Prefira composição à herança."
### Quando usar herança?

*Joshua Bloch diz, ainda no mesmo livro, que herança só é apropriada em circunstâncias em que a subclasse é realmente um subtipo da superclasse, ou seja, se um relacionamento "é-um" existir entre as duas classes. Segundo o autor, o uso de herança deve ser precedido da pergunta: cada B é realmente um A? Quando a resposta é "não", geralmente é o caso em que B deve conter uma instância privada de A e expor uma API menor e mais simples: A não é uma parte essencial de B, apenas um detalhe de sua*

