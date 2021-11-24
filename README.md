#Como simular uma API com javaScript.

*Aqui estão as informações necessárias para simular uma API usando LocalStorage, setTimeout e Promise do javaScript.*

# Arquivos iniciais

### 1 - Crie na pasta src, raiz ou onde preferir a pasta 'services', com os arquivos 'APISimulation.js' e 'data.js':
*Se preferir usar o terminal, vá a pasta onde quer criar a simulação da sua API e use os comandos:*

```bash
mkdir services && touch services/APISimulation.js && touch services/data.js
```

# data.js

### 2 - No arquivo 'data.js', defina quais informações em forma de "array de objetos" javascript a sua API simulations deve retornar como uma Promise JSON.

- ex:

```js
const data = [
  {
    id: 1,
    name: 'Nelson',
    lastName: 'Mandela',
    aQuoteInEnglish: "A winner is a dreamer who never gives up.",
    aQuoteInPortuguese: "Um vencedor é um sonhador que nunca desiste.",
  },
  {
    id: 2,
    name: 'Albert',
    lastName: 'Eisten',
    aQuoteInEnglish: "Life is like riding a bicycle. In order to keep your balance, you must keep moving.",
    aQuoteInPortuguese: "A vida é como andar de bicicleta. Para manter o equilibrio, você precisa continuar em movimento.",
  },
  {
    id: 3,
    name: 'Thomas',
    lastName: 'Elliot',
    aQuoteInEnglish: "Only those who will risk going too far can possibly find out how far one can go.",
    aQuoteInPortuguese: "Somente auqles que arriscam ir longe demais podem descobrir até onde conseguem ir.",
  },
  {
    id: 4,
    name: 'Dalai',
    lastName: 'Lama',
    aQuoteInEnglish: "Happiness is not something ready made. It comes from your own actions.",
    aQuoteInPortuguese: "A felicidade não é algo pronto. Ela vem das suas ações.",
  },
  {
    id: 5,
    name: 'John',
    lastName: 'Maxwell',
    quoteInEnglish: "You will never change your life until you change something you do daily.",
    quoteInPortuguese: "Você jamais mudará a sua vida se não mudar algo que você faz diariamente.",
  },
];

export default data;

```

### 3 - Importe o 'data.js' para seu arquivo 'APISimulation.js'.

```js
// import data.js
import data from './data';
```

# APISimulation.js

### 4 - Use o LocalStorage para armazenar a variável importada 'data'.

```js
// Usaremos o LocalStorage para armazenar nosso data.
localStorage.setItem('data', JSON.stringify(data));

// A função readData, busca no LocalStorage o data armazenado. Disponibilizando para o uso das funções logo abaixo.
const readData = () => JSON.parse(localStorage.getItem('data'));

// A função saveData, seta a chave data no LocalStorage. Salvando alterações no objeto passado via parâmetro, também utilizada nas funções logo abaixo:
const saveData = (data) => localStorage.setItem('data', JSON.stringify(data));
```

### 5 - Defina uma constante armazenando o tempo, em milisegundos, que deve ser a resposta assíncrona de nossa API. Além de outra constante contendo ums string de status 'OK'.

```js
// Definimos o tempo simulado de uma requisição a uma API e um status de resposta 'OK'.
const TIMEOUT = 3000;
const SUCCESS_STATUS = 'OK';
```

### 6 - Crie uma função onde a simulação da requisição a API irá acontecer através do setTimeout.

```js
// Usamos enfim, o serTimeout para retornar uma resposta à nossa função.
const requestSimulation = (response) => (callback) => {
  setTimeout(() => {
    callback(response);
  }, TIMEOUT);
};
```

### 7 - Agora já podemos preparar nosso retorno de uma Promise. 
*A próxima função é a que você deve importar em qualquer parte de sua aplicação. Ela te retornará uma Promise como qualquer outra API.*

```js
// Criamos a função usando Promise para retornar de forma simulada nosso data como uma promise assímcrona.
export const getData = () => (
  new Promise((resolve) => {
    const data = readData();
    requestSimulation(data)(resolve);
  })
);
```

*Como toda API, a nossa também contém requisições específicas. A próxima função nos retorna uma promise, mas através da busca por um determinado 'id' passado via parâmetro.*

```js
export const getDataById = (id) => {
  const data = readData().find((info) => info.id === parseInt(id, 10));
  return new Promise((resolve) => {
    requestSimulation(data)(resolve);
  });
};
```

# CRUD
*Que tal então completarmos nossa simulação de uma API com as opções de update, create e delete?*

### Update:

```js
export const updateData = (updatedData) => (
  new Promise((resolve) => {
    const Data = readData().map((isData) => {
      if (isData.id === parseInt(updatedData.id, 10)) {
        return { ...isData, ...updatedData };
      }
      return isData;
    });
    saveUser(data);
    requestSimulation(SUCCESS_STATUS)(resolve);
  })
);
```

### Create:

```js
export const createData = (isData) => (
  new Promise((resolve) => {
    let data = readData();
    const nextId = data[isData, id: nextId };
    data = [...data, newData];
    saveData(data);
    requestSimulation(SUCCESS_STATUS)(resolve);
  })
);
```

### Delete:

```js
export const deleteData = (dataId) => {
  let data = readData();
  data = data.filter((isData) => isData.id !== parseInt(dataId, 10));
  saveData(data);

  return new Promise((resolve) => {
    requestSimulation({ status: SUCCESS_STATUS })(resolve);
  });
};
```
