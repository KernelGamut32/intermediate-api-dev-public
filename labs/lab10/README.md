# Lab 10 - Orders API Including AuthN/AuthZ

Use https://github.com/KernelGamut32/microservice-apis/tree/master/ch11 (from a GitHub repo forked from https://github.com/abunuwas/microservice-apis that accompanies a great book called "Microservice APIs" - see https://www.manning.com/books/microservice-apis) as solution detail for this lab.

In this lab, you will enhance the API previously configured with the database backend by adding secured access to the API. In this lab, you will build a module to generate a JWT (JSON Web Token) using a self-signed cert created via `openssl`. You will also add middleware to your API to verify the inclusion of a valid JWT on each request to the service.

To try the provided solution, navigate to the `ch11` folder.

* Run `openssl req -x509 -nodes -newkey rsa:2048 -keyout new_key.pem -out new_key.pem -subj "/CN=mycompany"`
* Start up a virtual environment using `pipenv shell` and install all required dependencies by running `pipenv install --dev`
* In the virtual environment, run `python jwt_generator.py` to generate a new JWT; copy that JWT to a text file for ongoing usage
* Copy the resulting JWT, head over to `https://jwt.io`, and paste the JWT contents into the page
* You should be able to clearly read the header and the payload (since they are just Base64 encoded)
* Near the bottom of the text entry where you pasted your token, you should see an `Invalid Signature` error
* Execute `openssl x509 -pubkey -noout < new_key.pem > newkey.pem` to extract the public key into a new file
* In `jwt.io`, copy the contents of this new public key file in the space provided for public key under `Verify Signature`
* Once that is complete, the `Invalid Signature` error should change over to a much happier `Signature Verified` message
* Make sure you are in the folder where the `alembic.ini` configuration is defined, and run `alembic upgrade head` to generate the SQLLite database (`orders.db`) from the provided migrations
* Review the middleware used to check for and validate the inclusion of a valid bearer token in the header on the request
* Review the middleware used to enable CORS (Cross-Origin Resource Sharing) to allow the API to be called from a separate frontend website
* The solution includes an environment variable that allows you to turn Authorization on or off (can be nice for debugging/development)
* Run `AUTH_ON=True uvicorn orders.web.app:app --reload`
* Try running the `GET /orders` operation - it will fail with an error about missing or invalid access token
* Click the `Authorize` button, scroll down to the `bearerAuth` section, and paste in the JWT generated in the previous step; click `Authorize`
* Click `Close` and try the `GET /orders` operation again - it should succeed
* In the sample `curl` statement provided, you'll see application of the bearer token in the `Authorization: Bearer` request header
* Create a few new orders and demonstrate their presence by running the `GET /orders` operation again
* Click the `Authorize` button, scroll down to `bearerAuth`, click `Logout` and `Close`
* If you try the `GET /orders` operation again, it will fail (because of lack of valid token)
* Hit `Ctrl+C` to stop the site
* In the same terminal, run `python jwt_generator.py` again to generate a new JWT (simulating a different user); copy that JWT to a text file for ongoing usage
* Run `AUTH_ON=True uvicorn orders.web.app:app --reload` again to restart the API
* Click the `Authorize` button, scroll down to the `bearerAuth` section, and paste in the **second** JWT generated in the previous step; click `Authorize`
* Click `Close` and try the `GET /orders` operation again - it should succeed but the response indicates no orders (you can try logging in using the first user's JWT to verify that they're still there)
* This is the Authorization in action - users can only operate on orders that they have created
* If you open a new instance of the Swagger page in a separate incognito window, you can try executing operations on another user's orders to see what happens
