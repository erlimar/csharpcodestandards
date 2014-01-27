## Problema 1: `NOMES`
> Como diferenciar variáveis locais, variáveis membro e propriedades de classe, parâmetros de métodos e os próprios métodos, sem o uso de prefixos, e sem conflito de nomes?

No caso abaixo nós temos um membro de classe `int count`, um parâmetro `(int count)`, uma função `public int count`, uma variável local `int count = count` e uma propriedade `public int count`, ambas com o mesmo nome.
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
            count++;
            count = count;
            return count;
        }
        
        public int count
        {
            get { return count; }
        }
    }
}
```
