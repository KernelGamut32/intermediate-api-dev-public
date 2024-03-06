# Lab 03 - Orders API with Parameters & Validation

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch06/orders (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.amazon.com/Microservice-APIs-Jose-Haro-Peralta/dp/1617298417) as solution detail for this lab.

In this lab, you will enhance the API by adding supported query parameters for filtering. You will also look at adding validation for the parameters to ensure the API interface remains robust and user-friendly.

Using the provided `oas.yaml` file in that folder (and either Python or GoLang), generate models and a new API that allows you to process defined operations against the orders API. This example implements configuration and properties within the OpenAPI specification to prohibit unknown inputs (as a safeguard against unexpected failures from the client).

To try the provided solution, navigate to the `orders` folder in the `ch06` folder (where `app.py` exists). Run `pipenv shell` and `pipenv install` to setup a virtual environment with required dependencies installed. Run `uvicorn app:app --reload` to launch the API. Use `http://localhost:8000/docs/orders` to launch an interactive browser for testing the API operations.

Test each operation exposed by the API to verify that it successfully enables maintenance of orders through the API. Also, try submitting invalid inputs on the API requests to verify that invalid data is caught and errors are reported. Pay special attention to the routes that support query string parameters and to errors received on attempts to send in unsupported attributes in the requests.