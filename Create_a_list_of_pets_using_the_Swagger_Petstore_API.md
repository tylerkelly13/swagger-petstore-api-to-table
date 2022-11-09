# Create a list of pets using the Swagger Petstore API

The [Swagger Petstore API](https://petstore.swagger.io/) allows developers to access the Pets, Users, and Sales details of the Swagger Petstore. To retrieve a list of all pets, including those previously sold, we will use the `/pet/findBy/Status` API. We will then add some details, such as the pet's name, status (available, pending, or sold), and the pet's ID.

## Using the `/pet/findByStatus` endpoint

The full endpoint address for the `/pet/findByStatus` is:

```text
https://petstore.swagger.io/v2/pet/findByStatus
```

To use the endpoint, we need to provide the status or statuses as a search string (or [query string](https://developer.mozilla.org/en-US/docs/Web/API/URL/search)).

This endpoint has three valid statuses (`status`):

- `available`
- `pending`
- `sold`

With the search string appended, the endpoint URL we will be using is:

```text
https://petstore.swagger.io/v2/pet/findByStatus?status=available&status=pending&status=sold
```

To view the data returned by this endpoint, you can use cURL, such as:

```sh
curl -X 'GET' \
  'https://petstore.swagger.io/v2/pet/findByStatus?status=available&status=pending&status=sold' \
  -H 'accept: application/json'
```

## Procedure

To retrieve a list of pets and render the results as a table on an web page:

1. Create an empty HTML page containing a table, such as:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>Pets list - Swagger petstore</title>
      <style>

      </style>
    </head>
    <body>
      <h1>Swagger petstore</h1>
      <h2>Pets list</h2>
      <table>
        <caption>List of all the Swagger petstore pets</caption>
        <thead>

        </thead>
        <tbody>

        </tbody>
      </table>
      <script>

      </script>
    </body>
    </html>
    ```

1. Update the table header `<thead>` to include names of the three columns: Name, Status, and ID. For example:

    ```html
    <thead>
      <tr>
        <th>Name</th>
        <th>Status</th>
        <th>ID</th>
      </tr>
    </thead>
    ```

1. Within a `<script>` element at the end of the `<body`, add a variable containing the three JSON properties related to the columns in the same order as the table header, such as:

    ```js
    const fields = ['name', 'status', 'id'];
    ```

1. Add the `id` attribute to the table body `<tbody>`. This will allow the table to be populated when our script is run. For example:

    ```html
    <tbody id="pets-table">

    </tbody>
    ```

1. Use the `getElementById` function and the `id` attribute set in the previous step, to get the table body and store the result as a variable within the `<script>`, such as:

    ```js
    const tableBody = document.getElementById('pets-table');
    ```

1. After the two variables we created `fields` and `tablebody`, start an asynchronous function, with input arguments for:

    - the endpoint URL
    - the table body element
    - the array of fields

    This function will be used to retrieve the data and populate the table. For example:

    ```js
    async function petsTable(URL, elem, fields) {

    }
    ```

1. Add the [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch) function to retrieve the data from the input URL and convert the JSON data to a JavaScript object using the [`Response.json()`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch) function. For example:

    ```js
    async function petsTable(URL, elem, fields) {
      // fetch the data from the URL
      const petsRaw = await fetch(URL)
      // convert the JSON response to a javascript object
      .then(response => response.json())
      // always catch your errors
      .catch(error => console.error('Error:', error))
    }
    ```

1. Iterate (loop) over the data retrieved, using the property names in the `fields` array to generate the table rows and append the rows to the table body (`tableBody`). For example:

    ```js
    async function petsTable(URL, elem, columns) {
      // fetch the data from the URL
      const petsRaw = await fetch(URL)
      // convert the JSON response to a javascript object
      .then(response => response.json())
      // always catch your errors
      .catch(error => console.error('Error:', error))
      petsRaw.forEach(pet => {
        const row = document.createElement('tr')
        columns.forEach(column => {
          const cell = document.createElement('td')
          cell.innerText = (pet[column])
          row.appendChild(cell)
        })
        elem.appendChild(row)
      });
    }
    ```

1. At the end of the `<script>` block, run the function (`petsTable`), passing the endpoint URL, table body element (`tableBody`), and the array of property names (`fields`). For example:

    ```js
    petsTable('https://petstore.swagger.io/v2/pet/findByStatus?status=available&status=pending&status=sold', tableBody, fields)
    ```

1. (Optional) Add CSS to the `<style>` block in the header, such as:

    ```css
    body {
      font-family: Arial, Helvetica, sans-serif;
    }
    table {
      border: 2px solid black;
      border-collapse: collapse;
    }
    thead {
      border: 1px solid black;
    }
    th, td {
      border: 1px dotted black;
      padding: 5px;
    }
    th {
      font-weight: bold;
    }
    ```

1. Save the HTML file and open the file in a web browser to view the results. Remember to open the developer tools and review the console log if the table fails to render or renders incorrectly.

## Next steps

- For details on the Swagger Petstore API, visit [https://petstore.swagger.io/](https://petstore.swagger.io/).
- For information on using the `/pet/findByStatus` API, visit [Swagger Petstore API - `/pet/findByStatus`](https://petstore.swagger.io/#/pet/findPetsByStatus). - For details on the data returned by the `/pet/` API endpoints, including `/pet/findByStatus`, visit [Swagger Petstore API - Pet model](https://petstore.swagger.io/#model-Pet)

## Full example

The following is a completed example of the HTML application.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Pets list - Swagger petstore</title>
  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
    }
    table {
      border: 2px solid black;
      border-collapse: collapse;
    }
    thead {
      border: 1px solid black;
    }
    th, td {
      border: 1px dotted black;
      padding: 5px;
    }
    th {
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Swagger petstore</h1>
  <h2>Pets list</h2>
  <table>
    <caption>List of all the Swagger petstore pets</caption>
    <thead>
      <tr>
        <th>Name</th>
        <th>Status</th>
        <th>ID</th>
      </tr>
    </thead>
    <tbody id="pets-table">

    </tbody>
  </table>
  <script>
    const fields = ['name', 'status', 'id'];
    const tableBody = document.getElementById('pets-table');
    async function petsTable(URL, elem, columns) {
      const petsRaw = await fetch(URL)
      .then(response => response.json())
      .catch(error => console.error('Error:', error))
      petsRaw.forEach(pet => {
        const row = document.createElement('tr')
        columns.forEach(column => {
          const cell = document.createElement('td')
          cell.innerText = (pet[column])
          row.appendChild(cell)
        })
        elem.appendChild(row)
      });
    }
    petsTable('https://petstore.swagger.io/v2/pet/findByStatus?status=available&status=pending&status=sold', tableBody, fields )
  </script>
</body>
</html>
```
