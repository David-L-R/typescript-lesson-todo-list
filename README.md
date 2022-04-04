# Typescript

Short summary of a typescript beginner's lesson

# Why

Because it is AWESOME!

- avoid bugs
- make code clear
- more maintainable

Not for nothing it is falvor #1

![state of javascript](https://s3.eu-west-1.amazonaws.com/cd.sseu.re/items/bLuxng8E/3af806a4-6bc9-4cd9-99f2-9916d1cfece2.jpg?v=b98886f90248690c47ab0beaa1045ae9?v=b98886f90248690c47ab0beaa1045ae9)

## Errors

1. syntax errors (`{name: "David, email: "david.rajcher@codairsseur"`)
2. Static type errors (`"Thanks for buying [object Object], we hope to see you again!"`)
3. Dynamic type errors (`Uncaught TypeError: Cannot read property 'title' of undefined`)
4. Exceptions (catching a 500 error... Typescript does not help)
5. Logical 👑 (Typescript does not help)

## Easy Error To Make:

```javascript
const createFullName = (firstName, lastName) => {
  return `${firstName} ${lastName}`;
};

createFullName({ firstName: "david", lastName: "rajcher" });
```

This Will print out

```javascript
"[object object] undefined";
```

This is because we passed an object, which is just ONE value, into the parameters. Which means it only passed into `firstName`. Thus, `firstName` was printed as `[object object]` but `lastName` was printed as `undefined`.

## TYPES

### Primitive

- string
- boolean
- number
- undefined (when we expected something but nothing was returned)
- null (when we decide that the variable can be "empty")

```javascript
// Syntax -> variable: type = value
let name: string = "David";
let age: 20;
let isAdult: boolean = false;

age = "David"; // error! string cannot be assigned to number
```

### Complex Types

- Objects
- Array

In order to create more complex types we are going to use the keywords:

- type
- interface (for objects)

In the following example, we want to create a "Pet" type.

A pet has few qualities:

- name
- age
- kind
- allergies
- vaccines

So a `type Pet` might look like this

```javascript
type Pet = {
  name: string,
  age: number,
  kind: string,
  allergies: string[],
  vaccines: string[],
};
```

But maybe a "kind" is not "just a string".
We can add specific options:

```javascript
type Pet = {
  name: string,
  age: number,
  kind: "dog" | "cat" | "mouse",
  allergies: boolean,
  vaccines: string[],
};
```

Now, we might want to use this "kinds" in different places in the app, so we can create a new type for them:

```javascript
type Kind = "dog" | "cat" | "mouse";

type Pet = {
  name: string,
  age: number,
  kind: Kind,
  allergies: boolean,
  vaccines: string[],
};
```

Now, we might want specific options for vaccines (and vaccines can me multiple so we want an array of them):

```javascript
type Animal = "dog" | "cat" | "mouse";
type Vaccine = "arvovirus" | "distemper" | "canine hepatitis" | "rabies";

type Pet = {
  name: string,
  age: number,
  kind: Kind,
  allergies: boolean,
  vaccines: Vaccine[],
};
```

And lastly, a "kind" might be an actual object that host two other values:

- kind of animal
- breed of the animal

Then it might look like this:

```javascript
type Animal = "dog" | "cat" | "mouse";
type Vaccine = "arvovirus" | "distemper" | "canine hepatitis" | "rabies";
type Breed = "golden" | "chi" | "rot";

type Kind = {
  animal: Animal,
  breed: Breed,
};

type Pet = {
  name: string,
  age: number,
  kind: Kind,
  allergies: boolean,
  vaccines: Vaccine[],
};

const pet: Pet = {
  name: "Roxy",
  age: 12,
  kind: {
    animal: "dog",
    breed: "chi",
  },
  allergies: false,
  vaccines: ["arvovirus", "distemper"],
};
```

## Interface

Interfaces will define the blueprint of an object.
We can use types to do that, but interfaces are recommended by the official docs.

For the scope of this workshop, we will not explain why, but just use interface when defining objects.

```javascript
interface User {
  name: string;
  age: number;
  isAdult: boolean;
  email: string;
}
```

And we can add optional prop, if a specific property might not be included in every instance of a "user":

Look at the email property, we defined it as optional by using "?"

```javascript
interface User {
  name: string;
  age: number;
  isAdult: boolean;
  email?: string;
}
```

## Functions

Functions have a bit more syntax to them.
They have two parts:

1. type for the parameters passed to the function
2. type for the return value

The syntax goes something like

```javascript
const func = (param1: type1, params2: type2): returnType => {
  /* function logic */
};
```

Lets see an actual example:

```javascript
const addNumber = (num1: number, num2: number): number => {
  return num1 + num2;
};
```

Lets step it up, the syntax of a function that receives an object:

```javascript
const func = ({
  param1,
  params2,
}: {
  param1: type1,
  param2: type2,
}): returnType => {
  /* function logic */
};
```

Also, we can create an interface to avoid writing the types inside the parameters:

```javascript
interface IFunc {
  param1: type1;
  param2: type2;
}

const func = ({ param1, params2 }: IFunc): returnType => {
  /* function logic */
};
```

We can even abstract more by defining the function type:

```javascript
interface IFunc {
  param1: type1;
  param2: type2;
}

type Func = (params: IFunc) => returnType;

const func: Func = ({ param1, param2 }) => {
  /* function logic */
};
```

Lets take a look of another example. A function that filters users by age and returns the filtered list:

```javascript
interface User {
  name: string;
  age: number;
  email: string;
  isActive: boolean;
}

const filterUsersByAge = ({ users, minAge }: Props): User[] => {
  if (!minAge) return users;

  return users.filter((user) => user.age > minAge);
};
```

And a more complex one, a function that finds a specific user by name.

It is more complex because we have 3 different situations:

1. if no name given, return all users (Array of users)
2. if user was found, return user by name
3. if user was not found, return `undefined` (because `.find()` returns `undefined` when no user was found)

```javascript
const findUserByName = ({ users, name }) => {
  if (!name) return users;

  return users.find((user) => user.name === name);
};
```

Now lets add types to the parameters:

```javascript
const findUserByName = ({ users, name }: { users: User[], name: string }) => {
  if (!name) return users;

  return users.find((user) => user.name === name);
};
```

now lets add type to the return value:

The return value can be `User[]` or `User` or `undefined`

1. if no name given, return all users (`User[]`)
2. if user was found, return user by name (`User`)
3. if user was not found, return `undefined`

To write OR in Typescript we use `|`:
`User[] | User | undefined`

```javascript
const findUserByName = ({
  users,
  name,
}: {
  users: User[],
  name: string,
}): User[] | User | undefined => {
  if (!name) return users;

  return users.find((user) => user.name === name);
};
```

And if we really want to kill it, we can create a new type for our return value: `User[] | User | undefined`:

```javascript
type Return = User[] | User | undefined;

const findUserByName = ({
  users,
  name,
}: {
  users: User[],
  name: string,
}): ReturnValue => {
  if (!name) return users;

  return users.find((user) => user.name === name);
};
```

### Async Await

For promises (function that take time to process), we have a bit of a different syntax.
We want to add `Promise` around the return type:

```javascript
type GetUserType = () => Promise<User[] | undefined>;
```

For example:

```javascript
const getUsers = async (): Promise<User[]> => {
  const res = await axios.get("localhost:4000/users");
  return res.data;
};
```

But, we did not use `try-catch` to handle errors from the server.

```javascript
const getUsers = async (): Promise<User[]> => {
  try {
    const res = await axios.get("localhost:4000/users");
    return res.data;
  } catch (err) {
    console.log(err);
  }
};
```

This will actually gives an error, because if everything goes right, we will return users, but if something goes wrong, we will just `console.log`.

When we `console.log()` we are NOT RETURNING ANYTHING.
When not returning anything, the return type is `void`

So this function can return either:

1. the users (`User[]`)
2. nothing (`void`)

```javascript
const getUsers = async (): Promise<User[] | void> => {
  /* logic... */
};
```

## Void

When a function does not return anything, the return type is void

```javascript
const [filteredUsers, setFilteredUsers] = useState(users);

const filterBy = (users, filter, filterValue) => {
  setFilteredUsers(users.filter((user) => user[filter] === filterValue));
};
```

When adding Typescript, we should see that there is no `return` keyword...
Instead, after we filter, we are passing the result to the `setFilteredUsers` function.

In this case, the `return` value is nothing, which means it should be type `void`.

```javascript
const [filteredUsers, setFilteredUsers] = useState(users);

const filterBy = (users, filter, filterValue): void => {
  setFilteredUsers(users.filter((user) => user[filter] === filterValue));
};
```
