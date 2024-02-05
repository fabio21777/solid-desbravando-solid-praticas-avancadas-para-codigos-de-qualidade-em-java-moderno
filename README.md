# Introdução

Este projeto é um estudo do livro "Desbravando SOLID: Práticas Avançadas para Códigos de Qualidade em Java Moderno". A ideia inicial é que, a cada capítulo do livro, seja documentado para servir como um guia pessoal de consulta.

## Capítulo 1: Para que Serve a Orientação a Objetos

### OO como Ferramenta de Modelagem do Domínio

- OO (Orientação a Objetos) é uma ferramenta de modelagem do domínio, ou seja, uma forma de representar o mundo real em software.

### OO como Ferramenta de Gerenciamento de Dependências

- OO também é uma ferramenta de gerenciamento de dependências, organizando o código para que as partes dependentes sejam facilmente identificadas.

#### Técnicas de Gerenciamento de Dependências Independentes do Domínio

- **GRASP**: Guia criado por Craig Larman que ajuda a identificar onde alocar responsabilidades, com técnicas como baixo acoplamento, alta coesão, indireção, polimorfismo, entre outras.
- **Injeção de Dependências**: Técnica que permite injetar dependências em uma classe por meio de construtores, métodos ou propriedades.
- **Design Patterns**: Conjunto de padrões de projeto que resolvem problemas comuns de design de software.
- **SOLID**: Conjunto de princípios para criar código mais flexível, robusto e reutilizável, que é o foco deste livro.

### Os Princípios SOLID

- **SRP (Single Responsibility Principle)**: Uma classe deve ter um, e somente um, motivo para mudar.
- **OCP (Open/Closed Principle)**: Deve ser possível estender o comportamento de uma classe sem modificá-la.
- **LSP (Liskov Substitution Principle)**: Subtipos devem ser substituíveis por seus tipos de base.
- **ISP (Interface Segregation Principle)**: Clientes não devem ser forçados a depender de métodos que não usam.
- **DIP (Dependency Inversion Principle)**: Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.

### Princípios da Coesão de Módulo

Focam na organização e coesão entre módulos:

- **REP (Release Reuse Equivalency Principle)**: Componentes de um módulo devem ser reutilizáveis de forma independente.
- **CCP (Common Closure Principle)**: Classes que mudam juntas devem estar no mesmo módulo.
- **CRP (Common Reuse Principle)**: Classes reutilizadas juntas devem estar no mesmo módulo.

### Princípios de Acoplamento de Módulos

- **ADP (Acyclic Dependencies Principle)**: Não deve haver dependências cíclicas entre módulos.
- **SDP (Stable Dependencies Principle)**: Módulos devem depender de módulos mais estáveis.
- **SAP (Stable Abstractions Principle)**: Módulos mais estáveis devem ser mais abstratos.

### Os 10 Mandamentos da Orientação a Objetos

1. Entidades de software devem ser abertas para extensão, mas fechadas para modificação (Open/Closed Principle).
2. Classes derivadas devem ser substituíveis por suas classes bases (Liskov Substitution Principle).
3. Detalhes devem depender de abstrações; abstrações não devem depender de detalhes (Dependency Inversion Principle).
4. A granularidade das classes deve ser suficiente para que possam ser reutilizadas (Release Reuse Equivalency Principle).
5. Classes de um componente devem compartilhar um fechamento comum; se uma precisa ser modificada, todas as outras provavelmente precisarão ser modificadas.
6. Classes de um componente devem ser reutilizadas juntas; é impossível separar uma das outras para que sejam menos que um todo.
7. A estrutura de dependência entre os componentes deve ser acíclica.
8. A direção da dependência entre os componentes deve ser em direção à estabilidade.
9. Quanto mais estável um componente, mais abstrato ele deve ser.
10. Use padrões de projeto para resolver problemas de design e, ao atravessar dois paradigmas, construa uma camada que os separe, evitando misturar os paradigmas.
