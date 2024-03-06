# Lab 05 - Defining Schema For and Consuming GraphQL APIs

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch09 (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.manning.com/books/microservice-apis) as solution detail for this lab.

In this lab, you will explore the Schema Definition Language (SDL) spec for a GraphQL API to be implemented in a future lab. For this lab, we will use `GraphQL Faker` (https://github.com/APIs-guru/graphql-faker) as a mock GraphQL API for exploring queries and mutations against an API.

Navigate to the `ch09` folder and install the `GraphQL Faker` mock server using either `npm install` or `yarn`. Execute `./node_modules/.bin/graphql-faker schema.graphql` to generate a mock server from the pre-defined SDL. This will expose 3 endpoints on the mock server:
* `/editor` - for GraphQL API development
* `/graphql` - interface to the GraphQL API
* `/voyager` - interactive display of the types, relationships, and dependencies exposed by the GraphQL API

Navigate to `http://localhost:9002/graphql` and `Docs` in the upper-right to review the queries and mutations exposed by the SDL.

## Queries and error handling

Try running the various queries exposed by the API. For example:

```
{
  allIngredients {
    name
  }
}
```

```
{
    ingredient(id: "602f2ab3-97bd-468e-a88b-bb9e00531fd0") {
        name,
        supplier {
            id,
            name,
            email
        }
    }
}
```

What happens if you attempt to execute the following:

```
{
  allIngredients {
  }
}
```

or

```
{
  ingredient() {
        name,
        supplier {
            id,
            name,
            email
        }
  }
}
```

or

```
{
  ingredient {
        name,
        supplier {
            id,
            name,
            email
        }
  }
}
```

Why are you receiving the results you're receiving?

## Using fragments

Try running the following query:

```
{
  allProducts {
    name
  }
}
```

Why are you receiving the results you're receiving?

Update your query to:

```
{
  allProducts {
    ...on ProductInterface {
      name
    }
    ...on Cake {
      hasFilling
    }
    ...on Beverage {
      hasCreamOnTopOption
    }
  }
}
```

Create reusable fragments using the following definition and rerun:

```
{
  allProducts {
    ...commonProperties
    ...cakeProperties
    ...beverageProperties
  }
}

fragment commonProperties on ProductInterface {
  name
}
 
fragment cakeProperties on Cake {
  hasFilling,
  hasNutsToppingOption
}
 
fragment beverageProperties on Beverage {
  hasCreamOnTopOption,
  hasServeOnIceOption
}
```

## Multiple queries and query aliasing

Try running the following to see multiple queries combined (using anonymous queries):

```
{
  allProducts {
    ...commonProperties
  }
  allIngredients {
    name
  }
}

fragment commonProperties on ProductInterface {
  name
}
```

Run the following to see the effects of query aliasing:

```
{
  products: allProducts {
    ...commonProperties
    ...cakeProperties
    ...beverageProperties
  }
  ingredients: allIngredients {
    name
  }
}
 
fragment commonProperties on ProductInterface {
  name
}

fragment cakeProperties on Cake {
  hasFilling,
  hasNutsToppingOption
}
 
fragment beverageProperties on Beverage {
  hasCreamOnTopOption,
  hasServeOnIceOption
}
```

## Running mutations

Try running the following mutation:

```
mutation {
  addProduct(name: "Mocha", type: beverage, input: {price: 10, size: BIG, ingredients: [{ingredient: 1, quantity: 1, unit: LITERS}]}) {
    ...commonProperties
  }
}
 
fragment commonProperties on ProductInterface {
  name
}
```

Execute the following to run a parameterized mutation:

```
# Query document
mutation CreateProduct(
  $name: String!
  $type: ProductType!
  $input: AddProductInput!
) {
  addProduct(name: $name, type: $type, input: $input) {
    ...commonProperties
  }
}
 
fragment commonProperties on ProductInterface {
  name
}
 
# Query variables
{
  "name": "Bear claw",
  "type": "cake",
  "input": {
    "price": 10,
    "size": "BIG",
    "ingredients": [{"ingredient": 1, "quantity": 1, "unit": "LITERS"}]
  }
}
```

Execute the following to demonstrate inclusion of multiple mutations as part of the same query:

```
# Query document
mutation CreateAndDeleteProduct(
  $name: String!
  $type: ProductType!
  $input: AddProductInput!
  $id: ID!
) {
  addProduct(name: $name, type: $type, input: $input) {
    ...commonProperties
  }
  deleteProduct(id: $id)
}
 
fragment commonProperties on ProductInterface {
  name
}

# Query variables
{
  "name": "Latte",
  "type": "beverage",
  "input": {
    "price": 12.50,
    "size": "BIG",
    "ingredients": [{"ingredient": 1, "quantity": 1, "unit": "LITERS"}]
  },
  "id": "Mocha"
}
```

## Executing GraphQL queries through Python code

While leaving the GraphQL mock server running, open a new terminal and navigate to the `ch09` folder. Run `pipenv shell` and `pipenv install` to setup a virtual environment with required dependencies installed. Review the `client.py` code in the folder to see examples of GraphQL queries that can be executed from Python. Run `python client.py` to test.
