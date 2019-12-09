## Introduction 

This project was created using the help of the tutorial provided by `reactjs`, and was meant as a starter project introducing me to many of the functionalities provided by React. 


## Setup

1. Ensure the latest version of `Node.js` is installed 
2. Run the following command for Create React App to make a new project 
```
npx create-react-app my-app
```
3. Delete all files in the `src/` folder of the new project - we will be replacing the default source files for this project in later steps 
```bash
cd my-app 
cd src
rm -f *
cd .. 
```
4. Add a file called `index.css` in the src folder 
5. Add a file called `index.js` in the src folder 
6. Add in the code and the following three lines at the top of `index.js` in the src folder: 

```
import React from 'react'; 
import ReactDOM from 'react-dom'; 
import './index.css'; 
```