# Lab 06 - Implementing a GraphQL API Server

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch10 (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.manning.com/books/microservice-apis) as solution detail for this lab.

In this lab, you will create a GraphQL API from a pre-defined Schema Definition Language (SDL) spec. The solution to this lab uses Ariadne and FastAPI to host the API but you can use a different set of libraries if you so choose.

To run the solution, navigate to the `ch10` folder. Run `pipenv shell` and `pipenv install --dev` to setup a virtual environment with required dependencies installed. Run `uvicorn server:server --reload` to launch the API server and navigate to `http://localhost:8000` to explore the API and try various operations.

## Queries

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

## Mutations

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
  "id": "<id of one of the products>"
}
```

Feel free to explore other operations exposed by the GraphQL API.