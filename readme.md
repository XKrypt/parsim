# Parsin

Parsin é um mini banco de dados  no mesmo estilo do SQLite, onde cria um arquivo para salvar dados localmente, porém utilizando JSON. Foi feito para ser utilizado em aplicações que guardam localmente dados de configuração ou pequenas informações. Parsin é simples e pequeno, o nome foi escolhido do latim _**parvis et simplicibus**_ (Pequeno e simples) .

>Par: *parvis*, 
>
>sin: *simplicibus*

---

## Como funciona?

Parsin salva os dados em grupos. Dentro do banco de dados, ele cria um grupo onde seus dados serão guardados, então cada dado é salvo dentro de uma caixa que chamamos de `DataBox`. Dentro do `DataBox` é salvo o dado e um `id`.

### Group

Seus **DataBox** são salvos dentro dos grupos, sua estrutura é como no exemplo abaixo:
```yml
{
  key: string,
  idCount: number,
  data: DataBox[]
}
```

### DataBox

Seus dados são salvos dentro de um tipo chamado **DataBox**, sua estrutura é como no exemplo abaixo:

```yml
{
  id: number,
  data: any
}
```

---

## Instalando

```bash
npm install parsin
```

### Importação

#### Require

```js
const { parsin } = require('parsin');
```

#### Import
```js
import { Parsin } from Parsin;
```

## Inicializando

Para iniciar, primeiro aponte o diretório do arquivo a ser criado. Parsin criará automaticamente o arquivo caso ele não exista, mas não criará o diretório, então certifique-se que as pastas a serem utilizadas existam.
```js
const database = new Parsim('seu/diretorio/database.json', true, 'utf8');
```

O segundo argumento diz se deve carregar todos os dados em memória ou não, se sim ele carrega todos os grupos de dados dentro da classe, se não ele vai constantemente acessar o arquivo para gravar e ler os dados, o terceiro diz que codificação deve ser usada para gravar os dados. Todas as modificações feitas são salvas automaticamente.

> **Obs.: Não é necessário que o arquivo seja .json.**

### Criando grupos de dados

Para criar um grupo de dados é simples, apenas diga o nome do grupo e opcionalmente a partir de qual numero o id deve começar a contar.
```js
database.addGroup("nome do grupo", 5)
```

Com isso o primeiro dado vai ter o id 5 o seguinte 6 e assim por diante.

### Adicionando dados

Basta dizer o nome do grupo e como segundo parâmetro o dado.
```js
database.addData("nome do grupo", {algum : "valor"})
```

### Acessando dados

Você pode acessar um único dado, todos os dados ou uma seleção de dados.

#### Um único dado

retorna o primeiro dado encontrado.
```js
database.getSingleData("nome do grupo", dataBox => dataBox.id == 0)
```
Se não for encontrado será retornado `undefined`

#### Todos os dados
```js
database.getAllData("nome do grupo")
```
Se não for encontrado será retornado um array vazio.

#### Uma seleção de dados
```js
database.getMultipleData("nome do grupo", dataBox => dataBox.id => 5)
```
Se não for encontrado será retornado um array vazio.

Lembre-se que você pode acessar o seu dado dentro do DataBox, exemplo :
```js
database.getSingleData("nome do grupo", dataBox => dataBox.data.nome == 'Parsin')
```

### Editando dados

Para editar um dado é necessário saber sua `id` , e o nome do grupo de dados.
```js
database.editData("nome do grupo", 1, {alguma:'coisa'})
```
### Removendo dados

Como no exemplo acima basta passar uma arrow function como segundo parâmetro para assim selecionar o dado a ser excluído.
```js
database.removeData("nome do grupo", valor => valor.id == 5)
```

Lembrando que o valor é um **DataBox**, então para acessar o dado pode utilizar `valor.data`. 

Você pode também remover vários dados usado a mesma função da seguinte forma
```js
database.removeData("nome do grupo", valor => valor.id > 2)
```

`removeData` filtra os dados e remove aqueles que retornam `true`.

### Removendo grupos

Você pode também remover um ou vários grupos de dados, lembrando que todos os dados dentro do grupo serão apagados.
```js
database.removeGroup(grupo => grupo.key == "meu grupo")
```

### Outras funções

#### Seleciona um grupo de dados
```js
getGroup('seu grupo')
```

#### Seleciona múltiplos grupos
```js
getMultipleGroups(group => group.data.length > 0)
```

#### Substitui um grupo
```js
replaceGroup('nome do grupo',{
  key : 'novo nome',
  idCount: '3',
  data: []
})
```

---

## Other languages:
Coming soon