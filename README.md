## Orientação a Objetos em Ruby

> Ruby é uma linguagem orientada a objetos, porém **usar objetos** é diferente de **saber programar orientado a objetos**.

**Objetivo:** Entender os fundamentos para escrever código verdadeiramente orientado a objetos.

---

## O que é uma Classe?

Uma classe é um **"molde"** ou **"forma"**. É um modelo que define:
- **Características** (atributos/propriedades)
- **Comportamentos** (métodos/ações)

```ruby
class User
  # Características
  attr_accessor :name, :email
  
  # Comportamentos
  def initialize(name, email)
    @name = name
    @email = email
  end
  
  def greet
    "Hello, I'm #{@name}"
  end
end
```

**Classe = Receita de bolo**
**Objeto = Bolo feito com a receita**

---

## O que são Objetos?

Um objeto é a **instância de uma classe**.

Características dos objetos:
- **Estado**: valores dos atributos em um momento específico
- **Comportamentos**: métodos que pode executar
- **Identidade**: cada objeto é único na memória

```ruby
# Criando objetos (instâncias)
user1 = User.new("João", "joao@email.com")
user2 = User.new("Maria", "maria@email.com")

# Cada objeto tem seu próprio estado
puts user1.name    # "João"
puts user2.name    # "Maria"

# Mas compartilham os mesmos comportamentos
puts user1.greet   # "Hello, I'm João"
puts user2.greet   # "Hello, I'm Maria"
```
---

## Herança: Reutilizando e Especializando
Herança permite que uma classe **"herde" características e comportamentos de outra classe.**

- A classe pai é chamada de superclass, ela que fornece atributo e comportamento para outras classes.
- A classe filha de subclass, ela pode especializar ou sobrescrever métodos (polimorfismo).

Você pode usar herança para especializar comportamentos, por exemplo:

```ruby

class Usuario
  def initialize(nome, email)
    @nome = nome
    @email = email
  end
  
  def perfil
    "#{@nome} (#{@email})"
  end
  
  def pode_editar?(recurso)
    false  # Usuário comum não pode editar
  end
end

class Moderador < Usuario
  def pode_editar?(recurso)
    recurso.tipo == 'comentario'  # Só pode editar comentários
  end
  
  def moderar_conteudo
    "Moderando conteúdo..."
  end
end

```

- ✅ Hierarquia Lógica
- ✅ Evita duplicação (DRY)
- ✅ Especialização - Refinamento de Comporamentos (2 tipos de usuários por exemplo)

---

## Polimorfismo
Polimorfismo = "muitas formas". Regras diferentes para a mesma situação.

```ruby
class Forma
  def area
    raise "Método deve ser implementado pela subclasse"
  end
  
  def perimetro
    raise "Método deve ser implementado pela subclasse"
  end
end

class Retangulo < Forma
  def initialize(largura, altura)
    @largura = largura
    @altura = altura
  end
  
  def area
    @largura * @altura
  end
  
  def perimetro
    2 * (@largura + @altura)
  end
end

class Circulo < Forma
  def initialize(raio)
    @raio = raio
  end
  
  def area
    Math::PI * @raio * @raio
  end
  
  def perimetro
    2 * Math::PI * @raio
  end
end

formas = [
  Retangulo.new(5, 3),
  Circulo.new(2),
  Retangulo.new(10, 4)
]

# Mesmo método, comportamentos diferentes
formas.each do |forma|
  puts "Área: #{forma.area.round(2)}"
  puts "Perímetro: #{forma.perimetro.round(2)}"
  puts "---"
end

```

### Polimorfismo por Duck Typing (Ruby Style)

Duck Typing é um princípio que diz:

> "Se anda como um pato, grasna como um pato, então é um pato".

Ou seja, o tipo de um objeto não importa, contanto que ele implemente os métodos necessários.
Em Ruby, não checamos o tipo (classe), mas sim se o objeto responde ao método que queremos chamar.

```ruby
# Não precisamos de herança para polimorfismo em Ruby!
class EmailNotificador
  def enviar(mensagem)
    puts "📧 Email: #{mensagem}"
  end
end

class SlackNotificador
  def enviar(mensagem)
    puts "💬 Slack: #{mensagem}"
  end
end

class SMSNotificador
  def enviar(mensagem)
    puts "📱 SMS: #{mensagem}"
  end
end

class SistemaNotificacao
  def initialize
    @notificadores = []
  end
  
  def adicionar_notificador(notificador)
    @notificadores << notificador
  end
  
  def notificar_todos(mensagem)
    @notificadores.each do |notificador|
      notificador.enviar(mensagem)  # Polimorfismo!
    end
  end
end

# Uso polimórfico
sistema = SistemaNotificacao.new
sistema.adicionar_notificador(EmailNotificador.new)
sistema.adicionar_notificador(SlackNotificador.new)
sistema.adicionar_notificador(SMSNotificador.new)

sistema.notificar_todos("Sistema atualizado!")

```

---

## Fundamentos: Object

**Tudo em Ruby é um objeto!**

```ruby
# Números são objetos
42.class          # Integer
42.methods.count  # 162 métodos!

# Strings são objetos
"hello".class     # String
"hello".upcase    # "HELLO"

# Classes também são objetos
User.class        # Class
User.methods.include?(:new)  # true

# Até nil é objeto
nil.class         # NilClass
```

**Object** é a superclasse de todas as classes em Ruby:

```ruby
class MinhaClasse
end

MinhaClasse.superclass  # Object
Object.superclass       # BasicObject
```

---

## Fundamentos: Class

Classes definem o comportamento dos objetos:

```ruby
class Produto
  # Método de classe (chamado na classe)
  def self.categoria_padrao
    "Geral"
  end
  
  # Método de instância (chamado no objeto)
  def initialize(nome, preco)
    @nome = nome
    @preco = preco
  end
  
  def descricao
    "#{@nome}: R$ #{@preco}"
  end
end

# Método de classe
Produto.categoria_padrao  # "Geral"

# Método de instância
produto = Produto.new("Notebook", 2500)
produto.descricao  # "Notebook: R$ 2500"
```

**Classes são fábricas de objetos**


## Encapsulamento: Controlando o Acesso
Encapsulamento = controlar quem pode acessar o quê. 

Em Ruby temos 3 níveis:

Public    -> Métodos podem ser chamados por qualquer código. São herdados.
Private   -> Métodos privados só podem ser chamados pelo próprio objeto. Não podem ser herdados.
Protected -> Métodos protegidos podem ser chamados por objetos da mesma classe. São herdados.


---

## Fundamentos: Module

Modules são **"caixas de ferramentas"** que podem ser:
- **Namespaces**: organizar código
- **Mixins**: compartilhar funcionalidades

```ruby
# Como Namespace
module Financeiro
  class Conta
    def saldo
      @saldo ||= 0
    end
  end
end

conta = Financeiro::Conta.new

# Como Mixin
module Auditavel
  def log_acao(acao)
    puts "[#{Time.now}] #{acao}"
  end
end

class Usuario
  include Auditavel
  
  def criar
    log_acao("Usuário criado")
  end
end
```

**Modules = Código reutilizável sem herança**

---

## Fundamentos: Qual a diferença entre Class, Modules e Mixins

Características das Classes:

- ✅ Podem ser instanciadas
- ✅ Herança (uma superclass apenas)
- ✅ Encapsulam estado e comportamento
- ❌ Não podem ser "misturadas" em outras classes

Modules são contêineres de código que NÃO podem ser instanciados:

```ruby
module Matematica
  PI = 3.14159
  
  def self.area_circulo(raio)
    PI * raio * raio
  end
end

# Não funciona! Modules não podem ser instanciados
# Matematica.new  # NoMethodError

# Mas podemos usar seus métodos e constantes
puts Matematica::PI                    # 3.14159
puts Matematica.area_circulo(5)        # 78.53975

```

### Modules como Namespace

```ruby
# Sem namespace - pode gerar conflitos
class Conta
  def saldo
    "Conta bancária: R$ 1000"
  end
end

class Conta  # Ops! Redefinindo a classe anterior
  def saldo
    "Conta de email: 5 mensagens"
  end
end

```

```ruby
module WebServiceParceirosConfig

  def self.by_token(token)
    WEBSERVICE_PARCEIROS.fetch(token, nil)
  end

  # represents webservice parceiro
  class Parceiro
    attr_reader :nome

    def initialize(nome)
      raise StandardError, 'webservice parceiros invalid configuration' unless nome

      @nome = nome.upcase
    end

    def chatbot?
      nome === 'CHATBOT'
    end

    def vivo?
      nome === 'VIVO'
    end

    def igreen?
      nome === 'IGREEN'
    end

    def inclusao_api_name
      nome.downcase
    end
  end
end

```

Características dos Modules:

- ❌ NÃO podem ser instanciados
- ✅ Podem ser usados como namespace
- ✅ Podem ser incluídos em classes (Mixin)
- ✅ Podem ter métodos de classe
- ✅ Podem ter constantes


Mixin: Compartilhando Funcionalidades
Mixin = Module incluído em uma classe para compartilhar comportamentos:


### Module como Mixin - funcionalidade compartilhada

```ruby

module Notificavel
  def notificar(mensagem, canal = :email)
    case canal
    when :email
      enviar_email(mensagem)
    when :sms
      enviar_sms(mensagem)
    when :push
      enviar_push(mensagem)
    end
    
    log_notificacao(mensagem, canal)
  end
  
  private
  
  def log_notificacao(mensagem, canal)
    puts "[#{Time.now}] Notificação via #{canal}: #{mensagem}"
  end
end

module Servicos
  module Pagamento
    class PagSeguro
      include Notificavel  # Usando o mixin
      
      def processar_pagamento(valor)
        # lógica do pagamento
        notificar("Pagamento de R$ #{valor} processado", :email)
      end
    end
    
    class Stripe
      include Notificavel  # Mesmo mixin, implementação diferente
      
      def processar_pagamento(valor)
        # lógica diferente do pagamento
        notificar("Payment of $#{valor} processed", :push)
      end
    end
  end
  
  module Email
    class Gmail
      include Notificavel
      
      def enviar_email(destinatario, assunto)
        notificar("Email enviado para #{destinatario}", :sms)
      end
    end
  end
end

# Uso organizado e claro
pagseguro = Servicos::Pagamento::PagSeguro.new
stripe = Servicos::Pagamento::Stripe.new
gmail = Servicos::Email::Gmail.new

pagseguro.processar_pagamento(100)
stripe.processar_pagamento(200)
gmail.enviar_email("user@email.com", "Bem-vindo")

```

Quando Usar Cada Um?
Use Class quando:

- ✅ Precisa criar múltiplas instâncias
- ✅ Cada instância tem estado próprio
- ✅ Representa uma "coisa" do mundo real
- ✅ Exemplo: User, Product, Order

Use Module como Namespace quando:

- ✅ Quer organizar classes relacionadas
- ✅ Evitar conflitos de nomes
- ✅ Criar hierarquias lógicas
- ✅ Exemplo: Admin::User, Api::V1::Controller

Use Module como Mixin quando:

- ✅ Quer compartilhar comportamentos
- ✅ Precisa de "herança múltipla"
- ✅ Funcionalidade cross-cutting (auditoria, log, cache)
- ✅ Exemplo: Timestampable, Cacheable, Searchable


**No Rails, Concerns são modules organizados especificamente como Mixins nos models**
