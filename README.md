C# Code Standards
=================

Este é um documento de anotações pessoais com relação a forma como escrevo códigos fontes na linguagem C#. Talvez possa ser utilizado como uma referência por mais alguém na adotação de padronização em escrita de códigos nessa linguagem, mas a princípio é somente um rascunho sem mais pretenções.


```
Copyright (C) 2014 Erlimar Silva Campos (eu@erlimar.com)
Todos os direitos reservados
```

Vou tentar descrever os padrões que utilizo para codificação de programas em C# com o ~~próprio~~ motivo que me levou a codificar de tal maneira, ou seja, `RESOLVER PROBLEMAS`.

Eu encontrei alguns problemas, e tive que resolvê-los. Procurei fazer da maneira mais elegante possível (nem sempre isso foi possível), e acabei adotando isso como um padrão para evitar repetir os mesmos problemas.

Assim sendo, irei apresentar um determinado problema, e logo em seguida a forma como o resolvi culminando no padrão que utilizo. Talvez dessa forma você que está lendo possa entender que o padrão existe não somente como um enfeite, mas algo útil que auxilia no processo de desenvolvimento, além é claro, de tornar o código mais bonito.

> "Eu acredito que código de programação tem que ser não somente funcional e legível, tem que ser bonito de se ver".

Também vou separar este documento em duas seções principais:

Na seção Rascunhos estarei escrevendo ainda os padrões, são ideias que as vezes nem eu ainda utilizo em todos os projetos, estão nascendo ainda, e posteriormente poderão virar padrões.

Na seção Padrões, estarei descrevendo os padrões que já utilizo de alguma forma e com um rasoável sucesso.


-----------------

# Rascunhos


## Problema 1: `NOMES`
> Como diferenciar variáveis locais, variáveis membro de classe, parâmetros de métodos e os próprios métodos, sem o uso de prefixos, e sem conflito de nomes?

No caso abaixo nós temos um membro de classe `int count`, um parâmetro `(int count)`, uma função `public int count` e uma variável de escopo `int count = 0`, ambas com o mesmo nome.
Não é fácil distringuí-las, além do que haverá conflitos na utilização deste exemplo, experimente compilar.

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int count;

        public int count(int count)
        {
            int count = count;
            return count;
        }
    }
}
```

## Solução 01

Ao tentar compilar o exemplo pela primeira vez você receberá um alerta informando erro de definição. O compilador irá informar que não é possível definir a função `public int count` porque já há uma definição no mesmo escopo com esse nome (no caso a variável membro `int count`). Também não consegue definir a variável local `int count = ...` porque já existe uma outra variável no mesmo escopo com o nome pretendido (o parâmetro `int count`).

Podemos resolver o problema simplesmente utilizando prefixação, existem diversos padrões de pre e pós fixaxão para identificadores disponíveis por aí. Existem os que prefixam os identificadores de acordo com o tipo de dado que ele contém, para inteiros usa-se o prefixo `i`, para double `d`, string `s`, métodos `m`, e assim por diante. Há ainda os que separam os prefixos do nome de identificador usando `_` (underline), outros por `Capitalização` (prefixo minúsculo e primeiro caractere do identificador em Maiúsculo). Ficaria assim: `i_nome` ou `iNome`.

Isso resolveria parte do problema. Suponhamos que utilizaremos a prefixação citada acima, separando o nome do identificador por `_` (underline). Teríamos o seguinte resultado:

```csharp
namespace My.Namespace
{
    public class MyClass
    {
        int i_count;

        public int m_count(int i_count)
        {
            int i_count = i_count;
        }
    }
}
```

Padrões
=========

Nenhum padrão ainda
