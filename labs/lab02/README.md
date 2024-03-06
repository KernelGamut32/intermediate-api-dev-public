# Lab 02 - Baseline Orders API

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch02/orders (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.manning.com/books/microservice-apis) as solution detail for this lab.

Using the provided `oas.yaml` file in that folder (and the language of your choice), generate models and a new API that allows you to process CRUD (Create, Retrieve, Update, and Delete) operations in a baseline orders API for the fictional "Coffee Mesh" storefront. This will leverage a simple in-memory construct for runtime persistence of the order data.

You can either build out the object definitions by hand or, if you're using a language like Python, you can generate pydantic models by installing and using the `datamodel-code-generator` utility (https://docs.pydantic.dev/latest/integrations/datamodel_code_generator/).

```
pip install datamodel-code-generator
datamodel-codegen --input oas.yaml --input-file-type openapi --output schemas.py
```

To try the provided solution, navigate to the `orders` folder in the `ch02` folder (where `app.py` exists). Run `pipenv shell` and `pipenv install` to setup a virtual environment with required dependencies installed. Run `uvicorn app:app --reload` to launch the API. Use `http://localhost:8000/docs` to launch an interactive browser for testing the API operations.

Test each operation exposed by the API to verify that it successfully enables maintenance of orders through the API. Also, try submitting invalid inputs on the API requests to verify that invalid data is caught and errors are reported. This will be critical to ensuring a robust and stable API for long-term (validated against a well-defined contract).