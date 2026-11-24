# Ejercicios de JavaScript y TypeScript

## JavaScript

### Ejercicio 01: Parseador de CSV

#### Enunciado

Un texto en formato CSV tiene el nombre de los campos en la primera fila y los
datos en el resto, separados por comas. Crea un parseador que reciba un string en
formato CSV y devuelva una colección de objetos. Utiliza destructuring, rest y
spread operator donde creas conveniente.

```js
const data = `id,name,surname,gender,email,picture
15519533,Raul,Flores,male,raul.flores@example.com,https://randomuser.me/api/portraits/men/42.jpg
82739790,Alvaro,Alvarez,male,alvaro.alvarez@example.com,https://randomuser.me/api/portraits/men/48.jpg
37206344,Adrian,Pastor,male,adrian.pastor@example.com,https://randomuser.me/api/portraits/men/86.jpg
58054375,Fatima,Guerrero,female,fatima.guerrero@example.com,https://randomuser.me/api/portraits/women/74.jpg
35133706,Raul,Ruiz,male,raul.ruiz@example.com,https://randomuser.me/api/portraits/men/78.jpg
79300902,Nerea,Santos,female,nerea.santos@example.com,https://randomuser.me/api/portraits/women/61.jpg
89802965,Andres,Sanchez,male,andres.sanchez@example.com,https://randomuser.me/api/portraits/men/34.jpg
62431141,Lorenzo,Gomez,male,lorenzo.gomez@example.com,https://randomuser.me/api/portraits/men/81.jpg
05298880,Marco,Campos,male,marco.campos@example.com,https://randomuser.me/api/portraits/men/67.jpg
61539018,Marco,Calvo,male,marco.calvo@example.com,https://randomuser.me/api/portraits/men/86.jpg`;

const fromCSV = (csv) => {};

const result = fromCSV(data);
console.log(result);
```

Formato del resultado esperado:

```js
[
  {
    id: '15519533',
    name: 'Raul',
    surname: 'Flores',
    gender: 'male',
    email: 'raul.flores@example.com',
    picture: 'https://randomuser.me/api/portraits/men/42.jpg',
  },
  {
    id: '82739790',
    name: 'Alvaro',
    surname: 'Alvarez',
    gender: 'male',
    email: 'alvaro.alvarez@example.com',
    picture: 'https://randomuser.me/api/portraits/men/48.jpg',
  },
  // ...
];
```

Extra: Añade un segundo argumento a la función para indicar el número de
atributos a añadir. Si dicho argumento no es informado cada objeto tendrá todos
los atributos.

```js
const fromCSV = (csv, nAttrs) => {};

console.log(fromCSV(data)); // Cada usuario tendrá todos los atributos como la implementación original
console.log(fromCSV(data, 2)); // cada usuario tendrá sólo `id` y `name`
console.log(fromCSV(data, 3)); // cada usuario tendrá sólo `id`, `name` y `surname`
console.log(fromCSV(data, 4)); // cada usuario tendrá sólo `id`, `name`, `surname` y `gender`
```

#### Solución

```js
// versión uno
const fromCSV = (csv) => {
  const [header, ...rows] = csv.split('\n');
  const people = [];

  for (const item of rows) {
    const [id, name, surname, gender, email, picture] = item.split(',');

    people.push({ id, name, surname, gender, email, picture });
  }

  return people;
};
```

```js
// versión extra
const fromCSV = (csv, nAttrs) => {
  const [header, ...rows] = csv.split('\n').filter((line) => line !== '');
  const allKeys = header.split(',');
  const keys = nAttrs ? allKeys.slice(0, nAttrs) : allKeys;
  const people = [];

  for (const item of rows) {
    const values = item.split(',');
    const person = {};

    for (let i = 0; i < keys.length; i++) {
      person[keys[i]] = values[i];
    }

    people.push(person);
  }

  return people;
};
```

### Ejercicio 02: replaceAt

#### Enunciado

Implementar una funcion `replaceAt` que tome como primer argumento un array, como segundo argumento un índice
y como tercer argumento un valor y reemplace el elemento dentro del array en el índice indicado.
**El array de entrada no debe de ser mutado**, eso es, que debes crear un nuevo array sin modificar el existente.
Utiliza _spread operator_, y `slice` para conseguirlo.

```js
const elements = ['lorem', 'ipsum', 'dolor', 'sit', 'amet'];
const index = 2;
const newValue = 'furor';

const replaceAt = (arr, index, newElement) => {};

const result = replaceAt(elements, index, newValue);
console.log(result === elements); // false
console.log(result); // ['lorem', 'ipsum', 'furor', 'sit', 'amet'];
```

#### Solución

```js
const replaceAt = (arr, index, newElement) => {
  return [...arr.slice(0, index), newElement, ...arr.slice(index + 1)];
};
```

### Ejercicio 03: Top 3 por año

#### Enunciado

Crea una función que reciba una colección de objetos y un año. Dicha función
deberá de mostrar los nombres de las tres personas con el ranking más alto del
año.

```js
const data = [
  {
    ranking: 6.3,
    year: 1998,
    name: 'Monroe',
    gender: 'Genderfluid',
    id: 1450,
    surname: 'Jerde',
  },
  {
    ranking: 5.4,
    year: 1999,
    name: 'Maxie',
    gender: 'Bigender',
    id: 1652,
    surname: 'Keebler',
  },
  {
    ranking: 8.7,
    year: 2000,
    name: 'Emilee',
    gender: 'Genderqueer',
    id: 4779,
    surname: 'Ritchie',
  },
  {
    ranking: 6.5,
    year: 2001,
    name: 'Rudy',
    gender: 'Bigender',
    id: 7105,
    surname: 'Gusikowski',
  },
  {
    ranking: 7.1,
    year: 1998,
    name: 'Randy',
    gender: 'Genderqueer',
    id: 5950,
    surname: 'Lebsack',
  },
  {
    ranking: 4.9,
    year: 2000,
    name: 'Esteban',
    gender: 'Genderqueer',
    id: 7987,
    surname: 'Fritsch',
  },
  {
    ranking: 5.3,
    year: 2001,
    name: 'Leonard',
    gender: 'Male',
    id: 6268,
    surname: 'Frami',
  },
  {
    ranking: 8.8,
    year: 2002,
    name: 'Lang',
    gender: 'Polygender',
    id: 1033,
    surname: 'Dietrich',
  },
  {
    ranking: 9.1,
    year: 2000,
    name: 'Lettie',
    gender: 'Agender',
    id: 6403,
    surname: 'Gutmann',
  },
  {
    ranking: 6.0,
    year: 1998,
    name: 'Shonda',
    gender: 'Agender',
    id: 1324,
    surname: 'Borer',
  },
  {
    ranking: 7.3,
    year: 2003,
    name: 'Francene',
    gender: 'Agender',
    id: 6836,
    surname: 'Blanda',
  },
  {
    ranking: 6.8,
    year: 2003,
    name: 'Everett',
    gender: 'Polygender',
    id: 4937,
    surname: "O'Keefe",
  },
  {
    ranking: 5.3,
    year: 1998,
    name: 'Bernardo',
    gender: 'Agender',
    id: 8148,
    surname: 'Baumbach',
  },
  {
    ranking: 9.3,
    year: 2003,
    name: 'Brianna',
    gender: 'Female',
    id: 7716,
    surname: 'Schamberger',
  },
  {
    ranking: 9.7,
    year: 1998,
    name: 'Douglass',
    gender: 'Male',
    id: 4152,
    surname: 'Hilpert',
  },
  {
    ranking: 4.8,
    year: 1998,
    name: 'Angel',
    gender: 'Female',
    id: 355,
    surname: "O'Hara",
  },
  {
    ranking: 5.7,
    year: 2000,
    name: 'Hugh',
    gender: 'Male',
    id: 9600,
    surname: 'Hilll',
  },
  {
    ranking: 8.5,
    year: 1999,
    name: 'Graciela',
    gender: 'Agender',
    id: 871,
    surname: 'Kerluke',
  },
  {
    ranking: 2.4,
    year: 2000,
    name: 'Chassidy',
    gender: 'Agender',
    id: 4313,
    surname: 'Hegmann',
  },
  {
    ranking: 3.4,
    year: 1999,
    name: 'Abdul',
    gender: 'Agender',
    id: 367,
    surname: 'Weimann',
  },
  {
    ranking: 7.1,
    year: 2002,
    name: 'Coleen',
    gender: 'Non-binary',
    id: 1428,
    surname: 'Feil',
  },
  {
    ranking: 8.7,
    year: 2001,
    name: 'Eleanora',
    gender: 'Genderfluid',
    id: 984,
    surname: 'Barton',
  },
  {
    ranking: 9.7,
    year: 2002,
    name: 'Sean',
    gender: 'Agender',
    id: 5689,
    surname: 'Runolfsson',
  },
  {
    ranking: 4.5,
    year: 1999,
    name: 'Ike',
    gender: 'Female',
    id: 8445,
    surname: 'Haag',
  },
  {
    ranking: 7.7,
    year: 2001,
    name: 'Rachele',
    gender: 'Genderqueer',
    id: 6978,
    surname: 'Grady',
  },
  {
    ranking: 9.1,
    year: 2001,
    name: 'Sam',
    gender: 'Bigender',
    id: 1321,
    surname: 'Fritsch',
  },
  {
    ranking: 9.0,
    year: 2000,
    name: 'Eddy',
    gender: 'Polygender',
    id: 8273,
    surname: 'Kemmer',
  },
  {
    ranking: 4.6,
    year: 1999,
    name: 'Jamar',
    gender: 'Female',
    id: 6052,
    surname: 'Grant',
  },
  {
    ranking: 9.3,
    year: 2001,
    name: 'Dino',
    gender: 'Genderfluid',
    id: 5671,
    surname: 'Erdman',
  },
  {
    ranking: 7.6,
    year: 1999,
    name: 'Ervin',
    gender: 'Non-binary',
    id: 9945,
    surname: 'Powlowski',
  },
];

const winnerByYear = (arr, year) => {};

console.log(winnerByYear(data, 1998)); // [ 'Douglass', 'Randy', 'Monroe' ]
console.log(winnerByYear(data, 1999)); // [ 'Graciela', 'Ervin', 'Maxie' ]
console.log(winnerByYear(data, 2000)); // [ 'Lettie', 'Eddy', 'Emilee' ]
console.log(winnerByYear(data, 2001)); // [ 'Dino', 'Sam', 'Eleanora' ]
console.log(winnerByYear(data, 2002)); // [ 'Sean', 'Lang', 'Coleen' ]
console.log(winnerByYear(data, 2003)); // [ 'Brianna', 'Francene', 'Everett' ]
console.log(winnerByYear(data, 2004)); // []
```

#### Solución

```js
const winnerByYear = (arr, year) => {
  const yearFilter = arr.filter((item) => item.year === year);
  const order = yearFilter.sort((a, b) => b.ranking - a.ranking);

  return order.slice(0, 3).map((item) => item.name);
};
```

### Ejercicio 04: Normalizar colección

#### Enunciado

Crear funcion para normalizar una colección de objetos a un objeto, de tal
manera que devuelva un objeto que tenga como claves las ids de los objetos y como
valores los objetos en sí pero sin la id.

```js
const collection = [
  {
    id: 'f0b6930c-331a-43e1-80db-e6c46ed552aa',
    nationality: 'Barbadians',
    language: 'English',
    capital: 'Belgrade',
    national_sport: 'taekwondo',
    flag: '🇮🇩',
  },
  {
    id: '3e690823-fc74-4376-999a-501e5f9ae4be',
    nationality: 'Congolese',
    language: 'German',
    capital: 'Kinshasa',
    national_sport: 'wrestling',
    flag: '🇺🇾',
  },
  {
    id: '9edd87d6-2f4f-4701-8ec4-272a361dbfe9',
    nationality: 'Libyans',
    language: 'Tagalog',
    capital: 'Jakarta',
    national_sport: 'buzkashi',
    flag: '🇬🇼',
  },
  {
    id: '9873a1ed-6dc5-4034-8214-1f428c8951bd',
    nationality: 'Guineans',
    language: 'Hakka',
    capital: 'Ankara',
    national_sport: 'gymnastics',
    flag: '🇹🇷',
  },
  {
    id: '4679c4a4-4e2e-4470-a900-2445dc1f4a1e',
    nationality: 'Ugandans',
    language: 'German',
    capital: 'Beijing',
    national_sport: 'dandi biyo',
    flag: '🇮🇳',
  },
  {
    id: '4274ad62-5089-4b8e-a002-b2c1c3c74926',
    nationality: 'Finns',
    language: 'Swedish',
    capital: 'Djibouti',
    national_sport: 'bull fighting',
    flag: '🇭🇲',
  },
  {
    id: '2bb25c20-7962-47b7-82d9-d62a9493308f',
    nationality: 'Poles',
    language: 'Swedish',
    capital: 'Beirut',
    national_sport: 'cricket',
    flag: '🇰🇭',
  },
  {
    id: '9b3e09da-7484-49f3-aed0-13ccc7e77fff',
    nationality: 'Guineans',
    language: 'Portuguese',
    capital: 'Guatemala City',
    national_sport: 'cricket',
    flag: '🇩🇪',
  },
  {
    id: '903fb062-647c-46f8-857f-c1eba0cbbc9b',
    nationality: 'Ivoirians',
    language: 'Nepali',
    capital: 'Juba',
    national_sport: 'cricket',
    flag: '🇫🇮',
  },
  {
    id: '21bcd231-1d8f-49f5-826a-1dc986c52f0d',
    nationality: 'Hungarians',
    language: 'Russian',
    capital: 'Tarawa Atoll',
    national_sport: 'gymnastics',
    flag: '🇲🇴',
  },
];

const normalize = (arr) => {};

const result = normalize(collection);
console.log(result);
/*
Resultado:
{
  "f0b6930c-331a-43e1-80db-e6c46ed552aa": {
    nationality: "Barbadians",
    language: "English",
    capital: "Belgrade",
    national_sport: "taekwondo",
    flag: "🇮🇩"
  },
  "3e690823-fc74-4376-999a-501e5f9ae4be": {
    nationality: "Congolese",
    language: "German",
    capital: "Kinshasa",
    national_sport: "wrestling",
    flag: "🇺🇾"
  },
  ...
}
*/
```

Opcional: Si tu solución previa utiliza bucles, crea una versión sin bucles,
basándote en los métodos funcionales de `Array.prototype`.

#### Solución

```js
const normalize = (arr) => {
  return arr.reduce(
    (result, { id, ...rest }) => ({
      ...result,
      [id]: rest,
    }),
    {},
  );
};
```

### Ejercicio 05: Eliminar valores falsy

#### Enunciado

Implementa una función para eliminar valores _falsys_ de una estructura de datos.
Si el argumento es un objeto, deberá eliminar sus propiedades _falsys_.
Si el argumento es un array, deberá eliminar los elementos _falsys_.
Si el argumento es un objeto o un array no deberán ser mutados. Siempre deberá de crear una estructura nueva.
Si no es ni un objeto ni un array deberá de devolver dicho argumento.

```js
const elements = [0, 1, false, 2, '', 3];

const compact = (arg) => {};

console.log(compact(123)); // 123
console.log(compact(null)); // null
console.log(compact([0, 1, false, 2, '', 3])); // [1, 2, 3]
console.log(compact({})); // {}
console.log(
  compact({
    price: 0,
    name: 'cloud',
    altitude: NaN,
    taste: undefined,
    isAlive: false,
  }),
); // {name: "cloud"}
```

#### Solución

```js
const compact = (arg) => {
  if (arg === null || typeof arg !== 'object') return arg;

  if (Array.isArray(arg)) {
    return arg.filter((value) => Boolean(value));
  }

  return Object.keys(arg).reduce((result, key) => {
    const value = arg[key];

    if (value) {
      return { ...result, [key]: value };
    }

    return result;
  }, {});
};
```

## TypeScript

### Ejercicio 01: Interfaz Student

#### Enunciado

Dados el siguiente snippet de código, crea la interfaz `Student` y úsala para
sustituir los `unknown`:

```ts
const students: unknown = [
  {
    name: 'Patrick Berry',
    age: 25,
    occupation: 'Medical scientist',
  },
  {
    name: 'Alice Garner',
    age: 34,
    occupation: 'Media planner',
  },
];

const logStudent = ({ name, age }: unknown) => {
  console.log(`  - ${name}, ${age}`);
};

console.log('Students:');
students.forEach(logStudent);
```

#### Solución

```ts
interface Student {
  name: string;
  age: number;
  occupation: string;
}

const students: Student[] = [
  {
    name: 'Patrick Berry',
    age: 25,
    occupation: 'Medical scientist',
  },
  {
    name: 'Alice Garner',
    age: 34,
    occupation: 'Media planner',
  },
];

const logStudent = ({ name, age }: Student) => {
  console.log(`  - ${name}, ${age}`);
};

console.log('Students:');
students.forEach(logStudent);
```

### Ejercicio 02: Tipo User

#### Enunciado

Utilizando la interfaz `Student` del ejercicio anterior, crea la definición de
`User` de tal manera que pueda ser o `Student` o `Teacher`.
Aplica la definición de `User` donde sea requerido solventar los errores de
tipos.

```ts
interface Teacher {
  name: string;
  age: number;
  subject: string;
}

const users: Teacher[] = [
  {
    name: 'Luke Patterson',
    age: 32,
    occupation: 'Internal auditor',
  },
  {
    name: 'Jane Doe',
    age: 41,
    subject: 'English',
  },
  {
    name: 'Alexandra Morton',
    age: 35,
    occupation: 'Conservation worker',
  },
  {
    name: 'Bruce Willis',
    age: 39,
    subject: 'Biology',
  },
];

const logUser = ({ name, age }: Teacher) => {
  console.log(`  - ${name}, ${age}`);
};

users.forEach(logUser);
```

#### Solución

```ts
interface Student {
  name: string;
  age: number;
  occupation: string;
}

interface Teacher {
  name: string;
  age: number;
  subject: string;
}

type User = Student | Teacher;

const users: User[] = [
  {
    name: 'Luke Patterson',
    age: 32,
    occupation: 'Internal auditor',
  },
  {
    name: 'Jane Doe',
    age: 41,
    subject: 'English',
  },
  {
    name: 'Alexandra Morton',
    age: 35,
    occupation: 'Conservation worker',
  },
  {
    name: 'Bruce Willis',
    age: 39,
    subject: 'Biology',
  },
];

const logUser = ({ name, age }: User) => {
  console.log(`  - ${name}, ${age}`);
};

users.forEach(logUser);
```

### Ejercicio 03: Guardas de tipo

#### Enunciado

Con el resultado del ejercicio 2, sustituye la función `logUser` por la
siguiente y modifica el código aplicando las guardas que creas conveniente para
corregir los errores de compilación:

```ts
const logUser = (user: User) => {
  let extraInfo: string;
  if (user.occupation) {
    extraInfo = user.occupation;
  } else {
    extraInfo = user.subject;
  }
  console.log(`  - ${user.name}, ${user.age}, ${extraInfo}`);
};
```

> Extra: Crea dos funciones `isStudent` e `isTeacher` que apliquen la guarda y
> úsalas en la función `logUser`.
> Aplica tipado completo en la función (argumentos y valor de retorno). Utiliza
> [`is`](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates).

#### Solución

```ts
interface Student {
  name: string;
  age: number;
  occupation: string;
}

interface Teacher {
  name: string;
  age: number;
  subject: string;
}

type User = Student | Teacher;

const users: User[] = [
  {
    name: 'Luke Patterson',
    age: 32,
    occupation: 'Internal auditor',
  },
  {
    name: 'Jane Doe',
    age: 41,
    subject: 'English',
  },
  {
    name: 'Alexandra Morton',
    age: 35,
    occupation: 'Conservation worker',
  },
  {
    name: 'Bruce Willis',
    age: 39,
    subject: 'Biology',
  },
];

const isStudent = (user: User): user is Student => 'occupation' in user;
const isTeacher = (user: User): user is Teacher => 'subject' in user;

const logUser = (user: User) => {
  let extraInfo = '';

  if (isStudent(user)) {
    extraInfo = user.occupation;
  } else if (isTeacher(user)) {
    extraInfo = user.subject;
  }

  console.log(`  - ${user.name}, ${user.age}, ${extraInfo}`);
};

users.forEach(logUser);
```

### Ejercicio 04: Filtrar estudiantes por criterios

#### Enunciado

Utilizando las misma interfaz de `Student`, de los ejercicios anteriores arregla
los errores de TypeScript para podamos pasar aquellos criterios que necesitemos
sin tener que pasar toda la información de un `Student`.
Arregla los subsiguientes errores que aparezcan al asignar tipo a `criteria`.

```ts
const students: Student[] = [
  {
    name: 'Luke Patterson',
    age: 32,
    occupation: 'Internal auditor',
  },
  {
    name: 'Emily Coleman',
    age: 25,
    occupation: 'English',
  },
  {
    name: 'Alexandra Morton',
    age: 35,
    occupation: 'Conservation worker',
  },
  {
    name: 'Bruce Willis',
    age: 39,
    occupation: 'Placement officer',
  },
];

const filterStudentsBy = (
  students: Student[],
  criteria: unknown,
): Student[] => {
  return students.filter((user) => {
    const criteriaKeys = Object.keys(criteria);
    return criteriaKeys.every((fieldName) => {
      return criteria[fieldName] === student[fieldName];
    });
  });
};

const logStudent = ({ name, occupation }: Student) => {
  console.log(`  - ${name}, ${occupation}`);
};

console.log('Students of age 35:');
filterStudentsBy(students, { age: 35 }).forEach(logStudent);
```

#### Solución

```ts
interface Student {
  name: string;
  age: number;
  occupation: string;
}

const students: Student[] = [
  {
    name: 'Luke Patterson',
    age: 32,
    occupation: 'Internal auditor',
  },
  {
    name: 'Emily Coleman',
    age: 25,
    occupation: 'English',
  },
  {
    name: 'Alexandra Morton',
    age: 35,
    occupation: 'Conservation worker',
  },
  {
    name: 'Bruce Willis',
    age: 39,
    occupation: 'Placement officer',
  },
];

const filterStudentsBy = (
  students: Student[],
  criteria: Partial<Student>,
): Student[] => {
  const criteriaKeys = Object.keys(criteria) as (keyof Student)[];

  return students.filter((user) =>
    criteriaKeys.every((fieldName) => criteria[fieldName] === user[fieldName]),
  );
};

const logStudent = ({ name, occupation }: Student) => {
  console.log(`  - ${name}, ${occupation}`);
};

console.log('Students of age 35:');
filterStudentsBy(students, { age: 35 }).forEach(logStudent);
```

### Ejercicio 05: Swap con genéricos y tuplas

#### Enunciado

Mediante genéricos y tuplas, tipa de forma completa la función para solventar
los errores de compilación.

```ts
const swap = (arg1, arg2) => {
  return [arg2, arg1];
};

let age: number, occupation: string;

[occupation, age] = swap(39, 'Placement officer');
console.log('Occupation: ', occupation);
console.log('Age: ', age);
```

#### Solución

```ts
const swap = <T, U>(arg1: T, arg2: U): [U, T] => {
  return [arg2, arg1];
};

let age: number, occupation: string;

[occupation, age] = swap(39, 'Placement officer');
console.log('Occupation: ', occupation);
console.log('Age: ', age);
```
