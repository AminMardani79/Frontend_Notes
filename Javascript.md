### How to convert date to persian or other languages

```Javascript
const date = new Date(dateformat)
const dateString = date.toLocaleDateString('fa');
const timeString = date.toLocaleTimeString('fa');

```
***
### Mock data with json-server

##### Create db.json file in the root of project

```Javascript
{
    "users":[
        {
            "name":"amin",
            "family":"mardani"
        }
    ]
}
``` 

```Javascript
npm i json-server
"scripts":{
  "serve-json": "json-server --watch db.json --port 4000",
}
npm run serve-json
```
***
