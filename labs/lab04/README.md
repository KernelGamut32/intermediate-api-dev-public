# Lab 04 - Orders API with Backing Datastore

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch07 (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.amazon.com/Microservice-APIs-Jose-Haro-Peralta/dp/1617298417) as solution detail for this lab.

In this lab, you will enhance the API by adding a database backend used to persist your order detail across API restarts. For this lab, the goal will be to implement a well-partitioned and loosely-coupled architecture for the API. This means, keeping front end (routes) properly separated from business logic (services) and database code (repositories). By driving these types of separation into your architecture, you are better positioned for changes over time. Additionally, loosely-coupled components are more testable (using techniques like dependency injection and mocking).

Using the provided `oas.yaml` file in that folder (and either Python or GoLang), generate models and a new API schema that support the defined operations for the orders API.

Using a framework like SQLAlchemy (if generating the API in Python), create code definitions for your table structures that match up with the models used to represent the business objects processed by your API.

Build three separate layers (data access, business logic, and front-end). Wire the components together to enable end-to-end operations through your API to a local SQLLite database.

To try the provided solution, navigate to the `ch07` folder. Run `pipenv shell` and `pipenv install --dev` to setup a virtual environment with required dependencies installed. Make sure you are in the folder where the `alembic.ini` configuration is defined, and run `alembic upgrade head` to generate the SQLLite database (`orders.db`) from the included initial migration.

Execute `uvicorn orders.web.app:app --reload` to launch the app. Navigate to `http://localhost:8000/docs/orders` and test each operation exposed by the API to verify that it successfully enables maintenance of orders through the API. Exit from the browser and use `Ctrl+C` to stop the web app. Restart it using `uvicorn orders.web.app:app --reload` to confirm that your data has been persisted to the database.