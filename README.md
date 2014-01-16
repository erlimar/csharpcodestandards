C# Code Standards
===================

Este é um documento de anotações pessoais com relação a forma como escrevo códigos fontes na linguagem C#. Talvez possa ser utilizado como uma referência por mais alguém na adotação de padronização em escrita de códigos nessa linguagem, mas a princípio é somente um rascunho sem mais pretenções.


```
Copyright (C) 2014 Erlimar Silva Campos (eu@erlimar.com)
Todos os direitos reservados
```

Vou tentar descrever os padrões que utilizo para codificação de programas em C# com o ~~próprio~~ motivo que me levou a codificar de tal maneira, ou seja, RESOLVER PROBLEMAS.

Eu encontrei alguns problemas, e tive que resolvê-los. Procurei fazer da maneira mais elegante possível (nem sempre isso foi possível), e acabei adotando isso como um padrão para evitar repetir os mesmos problemas.

Assim sendo, irei apresentar um determinado problema, e logo em seguida a forma como o resolvi culminando no padrão que utilizo. Talvez dessa forma você que está lendo possa entender que o padrão existe não somente como um enfeite, mas algo útil que auxilia no processo de desenvolvimento, além é claro, de tornar o código mais bonito (eu acredito que código de programação tem que ser não somente funcional e legível, tem que ser "bonito de se ver").
