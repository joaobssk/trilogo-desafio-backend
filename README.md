# Desafio Backend
⚠️ Este desafio tem o intuito de avaliar novos candidatos para possível contratação.

⚠️ Caso o candidato não finalizar todo o desafio, enviar até o ponto em que conseguiu fazer, pois o mesmo ainda contará para avaliação;

⚠️ Deverá ser enviado o link do repositório no github onde o código possa ser baixado para avaliação;

⚠️ Colocar as instruções no arquivo README.md de como deve ser preparado o ambiente para testar a aplicação;

## Descrição

A TriMania é uma loja que atua somente nas redes sociais mas que agora está avançando para a criação de um novo site de vendas. Para isso, seus desenvolvedores front-end precisam de uma api para realizar as seguintes ações: 
 - [x] ***Cadastros de usuário***
 - [x] ***Catálogo de produtos***
 - [x] ***Cadastro de voucher de descontos***
 - [x] ***Venda de produtos***
 - [x] ***Relatório de vendas***
 
 Onde:
 
 ### Cadastro de Usuários
 
 Funcionalidade responsável por cadastrar, editar e excluir usuários da plataforma de vendas. As informações que devem compor o cadastro de usuário são:
 ```
{
  "id":"int"¹,
  "nome":"string"¹,
  "cpf":"string"¹,
  "email":"string"¹,
  "data_de_nascimento":"0001-01-01 00:00:00",
  "data_de_cadastro":"0001-01-01 00:00:00"¹,
  "endereco":{
    "rua":"string",
    "bairro":"string",
    "numero":"string",
    "cidade":"string",
    "estado":"string"
  }
}
 ```
 * Deve ser possível pesquisar pelo usuário utilizando um filtro por termo, onde poderá ser filtrado pelos seus atributos. 
 * ⚠️ O usuário não poderá ser excluído se houver pedidos cadastrados para ele.
 * A listagem dos produtos deve ser paginada de maneira que venha 10 produtos por página.
 * ⚠️ O usuário deverá conter um login e senha para autenticação. Fica a cargo do candidato como fazer a implementação dessa funcionalidade, porém a autenticação deve utilizar o JWT (https://jwt.io/) para geração de token;
 
 ### Catálogo de Produtos
 Nesta funcionalidade deverá ser possível cadastrar, editar e excluir produtos da plataforma, cujas informações são:
 ```
 {
  "id"¹:"int",
  "nome"¹:"string",
  "descricao":"string",
  "valor_custo"¹:"decimal",
  "valor_venda"¹:"decimal",
  "categoria"¹:{
      "codigo":"int",
      "descricao":"string"
      },
  "data_cadastro"¹:"0001-01-01 00:00:00",
  "autor_cadastro"¹:<objeto_usuario>,
  "ativo"¹:"bool"
 }
 ```
 * Deve ser possível pesquisar pelo produto utilizando um filtro por termo que, por sua vez, irá pesquisar pelos atributos de texto do Produto, e por um filtro onde eu possa buscar produtos ativos ou inativos;
 * Produtos que ***possuírem*** alguma venda ***não*** poderão ser excluídos da plataforma, apenas desativados;
 
 ### Cadastro de voucher de descontos
 
 Deverá ser possível cadastrar e *desativar* voucher de descontos para serem oferecidos aos clientes. Suas informações devem conter:
 
 ```
 {
  "id"¹:"int",
  "voucher"¹:"string",
  "valor":"decimal",
  "porcentagem":"decimal",
  "data_cadastro"¹:"0001-01-01 00:00:00",
  "ativo"¹:"bool"
 }
 ```
 
 * O campo *voucher* deverá ser um atributo randomico, gerado automaticamente pelo código;
 * O voucher deverá ter o campo valor preenchido OU o campo percentual preenchido, nunca os dois preenchidos em um único voucher;

### Venda de produtos

Está é a funcionalidade principal da API, onde será necessário desenvolver um carrinho de compras para que seja possível realizar a venda de produtos.
Deve ser seguido as seguintes regras:

* Um pedido poderá ter um ou mais produtos;
* Um pedido só pode pertencer a um único cliente;
* Um cliente poderá ter mais de um pedido cadastrado;
* Um pedido só poderá ter um voucher aplicado;
* O voucher utilizado em um pedido não poderá ser utilizado em qualquer outro pedido;
* O valor total do pedido deverá levar em conta o valor dos itens e o valor dos descontos;
* Um cliente só pode fazer um pedido por vez. Se o cliente começar um pedido, ele tem que finalizar ou cancelar o pedido que já está em andamento;
* Deverá ter as seguintes formas de pagamento: 'cartão de crédito', 'à vista' ou 'boleto';
* Um pedido deverá ter o status *Aberto*, *Em andamento*, *Cancelado* e *Concluído*;
* O valor do item do pedido deve ser o valor de venda do produto;

A estrutura do pedido deve ser o seguinte:
```
{
  "id"¹:"int",
  "usuario"¹:<objeto_usuario_citado_acima>,
  "valor_bruto"¹:"decimal",
  "valor_liquido"¹:"decimal",
  "voucher":<objeto_voucher>,
  "items"¹:[
    {
      "id":"int",
      "nome":"string",
      "descricao":"string",
      "valor_unitario":"decimal",
      "valor_total":"decimal",
      "produto_id":"int"
    }
  ],
  "data_criacao"¹:"0001-01-01 00:00:00",
  "data_cancelamento":"0001-01-01 00:00:00",
  "status"¹:"enumerador",
  "data_conclusao":"0001-01-01 00:00:00"
}
```
### Relatório de Vendas
Essa funcionalidade deverá retornar informações para que seja montado um relatório de vendas para a loja.
Devem ser seguidas as seguintes regras:
* Deverão haver os seguintes filtros::
*   Período de vendas (Data de Início e Data de Fim) ¹
*   Status (um ou mais status ao mesmo tempo)
*   Usuario (um ou mais usuários)

Devera conter a seguinte estrutura

```
{
  "total_pedidos_concluidos"¹:"int",
  "total_pedidos_cancelados"¹:"int",
  "valor_total_pedidos_sem_desconto"¹:"decimal",
  "valor_total_pedidos_com_desconto"¹:"decimal",
  "pedidos"¹:<objeto_pedido>,
  "periodo_filtrado"¹:"string",
  "status_filtrado":["enumeradores"],
  "usuarios_filtrados"²:["string"]
}
```

## Técnica

Para o desenvolvimento da api, a parte técnica deverá seguir as seguintes regras:
* Deverá ser utilizado aspnet core 3.1 ou superior;
* Deverá ser utilizado Entity Framework Core 3.1 ou superior;
* Deverá seguir o conceito de Arquitetura Limpa (https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures);
* Seguir os princípios do SOLID;
* Utilizar o Banco de Dados MySql (Deverá ser enviado o script de criação do banco de dados para avaliação e testes. Pode incluí-lo no repositório);
* A API deverá conter autenticação de usuário;
* Implementação de testes de unidade;

Desejável (Opcional)
* Implementação no padrão arquitetural CQRS(Command-Query Responsability Segregation);
* Implementação usando MediatR (Padrão mediator - https://github.com/jbogard/MediatR);
* Implementação de testes automatizados;
* Utilizar Entity Framework para a escrita dos dados e Dapper para a consulta dos dados;

 ###### _____________________________________________________________________________________________________________________________________
 ###### ¹ Informações obrigatórias
 ###### ² Apenas os nomes dos usuários
