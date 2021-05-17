# Desafio Backend
⚠️ Este desafio tem o intuito de avaliar novos candidatos para possível contratação.

## Descrição

A TriMania é uma loja que atua somente nas redes sociais mas que agora está avançando para a criação de um novo site de vendas. Para isso, seus desenvolvedores front-end precisam de uma api para realizar as seguintes ações: 
 - [ ] ***Cadastros de usuário***
 - [ ] ***Catálogo de produtos***
 - [ ] ***Cadastro de vouncher de descontos***
 - [ ] ***Venda de produtos***
 - [ ] ***Relatório de vendas***
 
 Onde
 
 ### Cadastro de Usuários
 
 Funcionalidade responsável por cadastrar, editar e excluir usuários da plataforma de vendas. As informações que devem compor o cadastro de usuário são:
 ```
{
  "codigo":"int"¹,
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
 * Deve ser possível pesquisar pelo usuário utilizando um filtro por termo, onde poderá ser filtrado pelos seus atributos. Mas atenção ⚠️ O usuário não poderá ser excluído se houver pedidos cadastrados para ele.
 * A listagem dos produtos devem ser paginadas de maneira que venha 10 produtos por página.
 * ⚠️ O usuário deverá conter um login e senha para autenticação. Fica a cargo do candidato em como fazer a implementação dessa funcionalidade, porém a autenticação deve utilizar o JWT (https://jwt.io/) para geração de token;
 
 ### Catálogo de Produtos
 Nesta funcionalidade deverá ser possível cadastrar, editar e excluir produtos da plataforma, cujo as informações são:
 ```
 {
  "codigo":"int"¹,
  "nome":"string"¹,
  "descricao":"string",
  "valor_custo":"decimal"¹,
  "valor_venda":"decimal"¹,
  "categoria"¹:{
      "codigo":"int",
      "descricao":"string"
      },
  "data_cadastro":"0001-01-01 00:00:00"¹,
  "autor_cadastro"¹:{
      "codigo":"int",
      "nome":"string"¹,
      "cpf":"string"¹,
      "email":"string"¹,
      "data_de_nascimento":"0001-01-01 00:00:00",
      "data_de_cadastro":"0001-01-01 00:00:00"¹
    },
  "ativo":"bool"¹
 }
 ```
 * Deve ser possível pesquisar pelo produto utilizando um filtro por termo onde poderá ser filtrado pelos seus atributos, e por um filtro onde eu possa buscar produtos ativos ou inativos
 * Produtos que ***possuírem*** alguma venda ***não*** poderão ser excluídos da plataforma, apenas desativados;
 
 ### Cadastro de vouncher de descontos
 
 Deverá ser possível cadastrar e *desativar* vouncher de descontos para serem oferecidos aos clientes. Suas informações devem conter:
 
 ```
 {
  "codigo":"int"¹,
  "vouncher":"string"¹,
  "valor":"decimal",
  "porcentagem":"decimal",
  "data_cadastro":"0001-01-01 00:00:00"¹,
  "ativo":"bool"¹
 }
 ```
 
 * O campo *vouncher* deverá ser um atributo randomico, gerado automaticamente pelo código;

### Venda de produtos

Está é a funcionalidade principal da API, onde será necessário desenvolver um carrinho de compras para que seja possível realizar a venda de produtos.
Deve ser seguido as seguintes regras:

* Um pedido poderá ter um ou mais produtos durante sua venda;
* Um pedido só pode pertencer a um único cliente;
* Um cliente poderá ter mais de um pedido cadastrado;
* Um pedido só poderá ter um vouncher aplicado;
* O vouncher utilizado em um pedido não poderá ser utilizado em qualquer outro pedido;
* O valor total do pedido deverá levar em conta o valor dos itens e o valor dos descontos;
* Um cliente só pode fazer um pedido por vez. Se o cliente começar um pedido, ele tem que finalizar ou cancelar o pedido que já está em andamento;
* Deverá ter formas de pagamento cartão de crédito, a vista ou boleto;
* Um pedido deverá ter o status *Aberto*, *Em andamento*, *Cancelado* e *Concluído*;
* O valor do item do pedido deve ser o valor de venda do produto;

A estrutura do pedido deve ser o seguinte:
```
{
  "codigo"¹:"int",
  "usuario"¹:<objeto_usuario_citado_acima>,
  "valor_bruto"¹:"decimal",
  "valor_liquido"¹:"decimal",
  "vouncher":<objeto_vouncher>,
  "items"¹:[
    {
      "codigo":"int",
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
Deverá seguir as seguintes regras:
* Deverá haver os seguintes filtros:
*   Período de vendas (Data de Início e Data de Fim)
*   Status (um ou mais status ao mesmo tempo)
*   Usuario (um ou mais usuários)

Devera conter a seguinte estrutura

```
{
  "total_pedidos_concluidos":"int",
  "total_pedidos_cancelados":"int",
  "valor_total_pedidos_sem_desconto":"decimal",
  "valor_total_pedidos_com_desconto":"decimal",
  "pedidos":<objeto_pedido>,
  "periodo_filtrado":"string",
  "status_filtrado":["enumeradores"],
  "usuarios_filtrados"²:["string"]
}
```

## Técnica

Para o desenvolvimento da api, a parte técnica deverá seguir as seguintes regras:
* Deverá ser utilizado aspnet core 3.1
* Deverá ser utilizado o ORM Entity Framework para a **escrita dos dados** e o MicroORM Dapper para a **consulta dos dados**;
* Deverá seguir o conceito de Arquitetura Limpa (https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures);
* Seguir os princípios do SOLID;
* Utilizar o Banco de Dados MySql (Deverá ser enviado o script de criação do banco de dados para avaliação e testes. Pode incluílo no repositório);
* A API deverá conter autenticação de usuário;
* Testes Unitários;

Desejável (Opcional)
* Implementação no padrão arquitetural CQRS(Command-Query Responsability Segregation);
* Implementação usando MediatR (Padrão mediator - https://github.com/jbogard/MediatR);
* Implementação de testes automatizados;

 ###### _________________________________________________________________________________________________________________________________________________________________
 ###### ¹ Informações obrigatórias
 ###### ² Apenas os nomes dos usuários
