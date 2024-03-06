# Demo 01 - Query Parameters for Filtering

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch06/kitchen (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.amazon.com/Microservice-APIs-Jose-Haro-Peralta/dp/1617298417) as source material for this demo.

In this demo, we're going to enhance the API by adding supported query parameters for filtering.

Review the provided `oas.yaml` file in that folder to explore API contract. This example implements configuration and properties within the OpenAPI specification to prohibit unknown inputs (as a safeguard against unexpected failures from the client).

To try the provided project, navigate to the `kitchen` folder in the `ch06` folder (where `app.py` exists). Run `pipenv shell` and `pipenv install` to setup a virtual environment with required dependencies installed. Run `flask --app app run` to launch the API. Use `http://localhost:5000/docs/kitchen` to launch an interactive browser for testing the API operations.

Test each operation exposed by the API to verify that it successfully enables maintenance of kitchen orders through the API. Also, try submitting invalid inputs on the API requests to verify that invalid data is caught and errors are reported. Pay special attention to the routes that support query string parameters and to errors received on attempts to send in unsupported attributes in the requests.