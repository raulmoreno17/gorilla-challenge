# gorilla-challenge
Front-end challenge using Vue.js

This project takes his data source from the following file: https://secureassets.evolvemediallc.com/internal/RH/Data%20Sample.xlsx
Read the xlsx file and convert it into a json object.

It has 3 main modules: Dashboard, Users, Products and Orders.

Dashboard shows 5 tiles with:
  * latest 5 Users created
  * latest 5 products created
  * latest 5 orders created
  * Chart with the 5 most sale products
  * Chart with the 12 months/num or orders of that month
  
For Users, Products and Orders, show tables with pagination with 10 items per page as well as arrows/numbers to navigate.

Orders should show the user’s information as well as the list of products of that order and display the calculated total

# Requirements achieved:
  * Page is responsive
  * All data manipulation have been done with arrays/objects, NO hardcoded values
  * The code must be uploaded to a repository (Github, Bitbucket, SourceForge, etc) 
  * Dependencies with node/yarn
  * Serve/Build Code
  * README.md

## Project setup
This project uses some external dependencies like Vuetify and xlsx , please execute the following command before running the project
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

