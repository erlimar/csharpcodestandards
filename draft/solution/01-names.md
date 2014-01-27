## Solução 01: `NOMES`

#### Mas primeiro... `uma estorinha`...

Ao tentar compilar o exemplo pela primeira vez, o compilador irá informar que não é possível definir a função `public int count()` porque já há uma definição no mesmo escopo com esse nome (no caso a propriedade `public int count{}`).

Podemos resolver o problema simplesmente utilizando prefixação. Existem diversos padrões de pré e pós fixação disponíveis por aí. Existem os que prefixam os identificadores de acordo com o tipo de dado que ele contém, prefixo `i` para inteiros, `d` para double, `s` para string, e etc. Ainda existem os que prefixam os identificadores de acordo com o tipo do item, `v` para variáveis, `p` para propriedades, `m` para métodos, e assim por diante.

Ainda existem convenções de como esses prefixos são concatenados com os nomes propriamente ditos dos identificadores: alguns separam os prefixos do nome de identificador usando `_` (underline), outros por `Capitalização` (prefixo minúsculo e primeiro caractere do identificador em Maiúsculo).

Exemplo para prefixos de tipo de dado:
Uma variável do tipo `inteiro` chamada `valor`, poderia ser `i_valor` ou `iValor`.

Exemplo para prefixos de tipo de item:
Um método chamador `contar`, poderia ser `m_contar` ou `mContar`.

Isso resolveria parte do problema. Suponhamos que utilizaremos a prefixação de tipo de item citada acima, separando o nome do identificador por `_` (underline). Teríamos o seguinte resultado:

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int v_count;

        public int m_count(int v_count)
        {
            int v_count = v_count;
            v_count++;
            v_count = v_count;
            return v_count;
        }

        public int p_count
        {
            get { return v_count; }
        }
    }
}
```

#### Oh! No!

Resolvemos o conflito entre o nome da variável membro `int v_count`, do método `m_count()` e da propriedade `int p_count{}`, esses estão no mesmo escopo (definição da classe *MyClass*). Só que ainda temos o mesmo problema, agora no escopo do método **public int m_count()**.

Ao tentar compilar o programa com as modificações propostas acima, o compilador irá reclamar que você está tentando atribuir uma variável a ela mesma (seria por causa do trecho `int v_count = v_count`, e do trecho `v_count = v_count;`), e ainda diz que você não pode declarar uma variável com esse nome porque já existe uma outra declaração no mesmo escopo com esse nome (aqui ele está falando do nosso parâmetro `(int v_count)`.
Neste ponto o compilador está confuso, para nós humanos fica claro (*talvez não para todos*) que estamos querendo atribuir o parâmetro `v_count` à variável local `v_count`, mas o compilador não pensa como nós: *"Ele não presume algo. Ou ele sabe, ou não sabe"*, e nesse caso ele ***"Não sabe o que queremos dizer"***.

Podemos melhorar um pouco mais nosso *"siteminha de prefixação"*. Que tal se distinguíssemos entre as variáveis locais das demais variáveis? Podemos prefixar as variáveis locais com `lv` (de local variable - variável local) ao invés de somente `v`.

Nosso código ficaria assim:

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int v_count;

        public int m_count(int v_count)
        {
            int lv_count = v_count;
            lv_count++;
            v_count = lv_count;
            return v_count;
        }

        public int p_count
        {
            get { return v_count; }
        }
    }
}
```

### Ahá...

Agora compilou. Problema resolvido!

###Uhhmm...

Só porque compilou, não quer dizer que o problema está resolvido. Se você fizer um pequeno teste perceberá que nosso programa não está fazendo o que queremos.

Vamos ver qual era o objetivo do programa (objetivo meio sem noção é verdade, mas, vai que alguém realmente precise de algo assim - não duvide disso):

O objetivo do programa é: ***guardar um contador (`variável de classe v_count`), que pode ser atribuído através de um método `int m_count()`, esse por sua vez retorna imediatamente o valor do contador após atribuí-lo. Só que antes de atribuir o contador com o valor informado (`parâmetro v_count`), esse valor deve ser incrementado em 1, de forma que o valor atribuído será sempre maior do que o informado pelo usuário.***

Faça um simples teste com o programa abaixo:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var c = new MyClass();

        Console.WriteLine("p_count: {0}", c.p_count);
        Console.WriteLine("Retorno: {0}", c.m_count(5));
        Console.WriteLine("p_count: {0}", c.p_count);

        Console.Read();
    }
}
```

O resultado é esse:

    p_count: 0
    Retorno: 6
    p_count: 0

Veja que o retorno do método está certo, `Retorno: 6`. Isso porque estamos informando o valor `5` e ele está retornando o valor calculado `6` (incrementado) imediatamente. Mas veja também que o parâmetro `p_count` que é o nosso contador guardado, não está sendo modificado.

Isso acontece porque, no código, quando nós usamos `v_count = lv_count`, não estamos atribuindo o valor incrementado `lv_count` na *variável membro* `v_count`, e sim no *parâmetro* `v_count` (*porque os parâmetros são considerados como variáveis locais no escopo do método que os declara*).

Perceba que neste caso o compilador está fazendo exatamente o que a linguagem de programação define. Aqui está sendo aplicado o princípio do ***aninhamento de escopo***, onde a declaração de um identificador em um escopo *mais interno* sobrescreve a declaração de um mesmo identificador em um escopo *mais externo*.

O compilador fez exatamente o que está programado para fazer, mas ***nós*** fomos induzidos ao erro, e deixamos um erro básico **quebrar** nosso código.

Vamos melhorar ainda mais nosso *"siteminha de prefixação"*. Já que distinguimos entre as variáveis locais das demais variáveis, vamos também distinguir entre variáveis locais dos parâmetros. Podemos prefixar os parâmetros com `pv` (de parameter variable - variável de parâmetro) ao invés de somente `v`.

Vejamos como fica isso no código:

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int v_count;

        public int m_count(int pv_count)
        {
            int lv_count = pv_count;
            lv_count++;
            v_count = lv_count;
            return v_count;
        }

        public int p_count
        {
            get { return v_count; }
        }
    }
}
```

###Uhhhuu...

Agora sim! Compilou e está fazendo exatamente o que queremos. O problema agora está resolvido de vez.

###`NOTA`

O objetivo é chegar a um exemplo de código bastante padronizado, próximo ao seguinte:

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int counter;

        public int Count(int counter)
        {
            int counter_ = counter;
            counter_++;
            this.counter = counter_;
            return this.counter;

            // "counter_" é preferível ao invés de "_counter" pelo fato de que
            // "_" não faz parte do nome da variável, assim, deduzimos que o nome
            // da variável é "counter" quando retiramos o complemento "_".
            //
            // Daahhhh... E daí?
            //
            // Daí que, se retiramos o complemento do nome da variável ele deve
            // continuar sendo um nome válido de variável.
            //
            // (imagine o caso onde você tem uma ferramenta que gera a documentação
            // automática da definição de seus programas lendo o código fonte. Daí
            // essa ferramenta retira esses "complementos" e exibe somente o que é
            // relevante, ou seja, o nome da variável)
            //
            // E o que é um nome válido de variável?
            // Para a maioria das linguagens de programação é um nome que "inicia
            // com um '_' (underline) ou um caractere alfa (letras) minúsculo ou 
            // maiúsculo (a..zA..Z) sem nenhum símbolo especial e segue sem
            // espaçamentos com demais caracteres alfanuméricos (letras e números),
            // às vezes com limite na quantidade de caracteres.
            //
            // Portanto "_1variavel" é válido, mas "1variavel" NÃO É VÁLIDO, porque
            // inicia com um número (diferente de letra ou '_' underline, da mesma
            // forma que "-variavel" também é um nome inválido.
            //
            // E como dissemos anteriormente: se retiramos o complemento do nome da
            // variável ele deve continuar sendo um nome válido de variável.
            //
            // Portanto, se usarmos o complemento "_" de forma pré-fixada estamos
            // dando margem para erros, porque isso permitiria você usar
            // "_7passo", o que sem o complemento seria a variável "7passo", ou
            // seja, um nome inválido.
            //
            // Agora, se usarmos o complemento "_" de forma pós-fixada corrigimos
            // esse "BUG", porque ao tentar criar essa variável tentaríamos usar
            // "7passo_" para uma variável "7passo" e o compilador acusaria o erro.

        }

        public int Counter
        {
            get { return this.counter; }
        }
    }
}
```
