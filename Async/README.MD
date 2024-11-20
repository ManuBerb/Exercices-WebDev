# Exercices sur les fonctions asynchrones et l'API Fetch

Ce dossier contient des exercices sur les notions de base des fonctions asynchrones et l'API `Fetch` pour la communication HTTP dans un site web.

Chaque dossier contient un fichier `README` avec l'objectif de l'exercice ainsi qu'une solution possible.

Vous êtes fortement encouragés d'essayer de compléter l'exercice par vous-mêmes avant de regarder la solution, mais vous pouvez à tout temps consulter la solution finale pour mieux comprendre le fonctionnement des différentes notions.

La documentation de [DOM sur Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) et l'API [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) vous sera également utile pour les exercices.

## Table des matières

Voici les liens vers les différents exercices et leurs énoncés. Vous êtes conseillés de comencer par l'exercice d'introduction :

- [`Retry`](./Retry/README.MD) : exercices sur les notions des objets `Promise`
- [`Class`](./Class/README.MD) : exercices sur l'utilisation de l'API `Fetch` pour communiquer avec un serveur HTTP



////////////////////////////////////////////////////////////////


Cours de revision pour manu


# Synchronous programming:
    * You wait for each line to execute before executing the next *
    Take for a example you run a code and there is a call for a function, it'll wait till the function is over for the code to continue to run
    Lets say we run the following code:
   
    '''
    let pizza
    function orderPizza(){
        console.log('Order Pizza')
        setTimeout(() => {
            pizza = 'pizzaEmoji'
        },2000)
        console.log('Pizza was ordered)
    }


    orderPizza()
    console.log('Eat ${pizza}')
    '''
    Here we will get:
    Order Pizza
    Pizza was ordered
    Eat *UNDEFINED*


    The logger doesn't wait for setTimeout (good we dont wanna pause everthing while the pizza is being made)
    That's where asynchronous programming comes in


# Asynchronous programming:
    Let's rewrite this without and we wanna call our frind Qoli before eating the pizza;
    If we run the following code we will get

    '''
    let pizza
    function orderPizza(){
        console.log('Order Pizza')
        setTimeout(() => {
            pizza = '🍕'
  //        console.log('{$pizza} is ready')
        },2000)
        console.log('Pizza was ordered)
    }

    orderPizza()
  //console.log('Call Qoli')
    console.log('Eat ${pizza}')
    '''

    The output will be:
    Order Pizza
    Pizza was ordered
    Call Qoli
    Eat *UNDEFINED*

    2secs later...

    🍕 is ready    

    ///////////////////////////

    Now we will want to do the pizza order into it's own process without having to wait, we will do asynchronous programming
   
    function orderPizza(callback){
        setTimeout(() => {
            const pizza = '🍕'
            callback(pizza)
        }, 2000)
    }


    function pizzaReady(pizza){
        console.log('Eat the ${pizza}')
    }

    orderPizza(pizzaReady) //Once you are done call this
    console.log('Call Qoli')

    OUTPUT:
    Call Qoli
    *wait 2 sec*
    Eat the 🍕

    -- Some example of asynchronous code --

    // When the user clicks this button or window in this case run the function
    window.addEventListener('click', function(){
        console.log('Clicked')
    })

    //when user clicks button
     window.addEventListener('click', callback){
        function callback(){
            console.log('Clicked')
        }
    }

    -- PROBLEM --
    If you have many callbacks in each other we have CALLBACK HELL !!!

    So that's where -- Promises -- and -- Async/Await -- comes in

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
### Promises   ✅✅

IRL : In promises you have the person that made the promise and the person that the promise was made to
Same thing for JS promises you have:
# The maker : is a function that creates a promise object and returns it --> asynchronous function
Because the async process takes an unknown amount of time and we need to return something in the meantime (or else it'll return undefined). We return a promise in the meantime till we get the data we want (weather data for ex).

A promise can have 3 states:
# Pending
# Resolve -- if the promise suceeds
# Reject -- if the promise doesn't suceeds

function getWeather() {
    return new Promise(function(resolve, reject){
        setTimeout(function(){
            reject('Error')
        })
    })
}

# The receiver : calls the maker and receives the promise and does something with it
It's where we call our async function from
function getWeather() {
    return new Promise(function(resolve, reject){
        resolve('Sunny')
    })
}

***receiver***
const promise = getWeather()
promise.then(function(data){
    console.log(data)
}) 

***.then*** --> called when promise resolves successfully (verify if we called reject or success)
***OUTPUT*** -- > 'Sunny'

Now if we incorporate the reject ut would look like this

function getWeather() {
    return new Promise(function(resolve, reject){
        reject('Sunny')
    })
}
***receiver***
const promise = getWeather()
promise.then(
    function(data){
    console.log('First param {$data}')
},
function(data){
    console.log('Second param {$data}')
}
) 

***OUTPUT*** -- > 'Second param Sunny'


## We can also write them like this
function getWeather() {
    return new Promise(function(resolve, reject){
        reject('Sunny')
    })
}

function onSuccess(data) {
    console.log('Success ${data}')
}

function onError(error) {
    console.log('Error ${error}')
}

getWeather().then(onSuccess, onError)

# EXAMPLE
function getWeather() {
    return new Promise(function(resolve, reject){
       setTimeout(() => {
        resolve('Cloudy')
       },100)
    })
}
function getWeatherIcon(weather){
    return new Promise(function(resolve, reject){
        setTimeout(() => {
            switch(weather) {
                case: 'Sunny':
                    resolve('☀️')
                case 'Cloudy':
                    resolve('☁️')
                default:
                reject('NO ICON FOUND)
            }
        },100)
    })
}
function onSuccess(data) {
    console.log('Success ${data}')
}

function onError(error) {
    console.log('Error ${error}')
}

getWeather()
    then.(getWeatherIcon) 
    then.(onSuccess, onError)
*** CHAINING - whatever gets resolved in the first one (that data goes into the next function as the parameter [Cloudy here]) ***
# EXAMPLE 2 
function fun1() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject('404')
        },100)
    })
}

function fun2() {
    return new Promise((resolve, reject) => {
        setTimeout(()=> {
            resolved('🤓')
        },100)
    })
}

function onSuccess(data){
    console.log(data)
}

function onError(errorCode) {
    console.log('ERROR: ${errorCode}')
}

fun1()
    .then(fun2) 
    //When it catches reject on fun1 it directly jumps to catch(onError) *** if we write .then(fun2, onError) it will do the next line and wont jump directly to  .catch(onError) ***
    .then(onSuccess)
    .catch(onError)

***OUTPUT*** ERROR : 404


# ASYNC / AWAIT

In order to use await  you have to use async , not necessarily the other way around
1) Async and await must be used together
2) Async/await must be used together
3) You can await any functio that returns a Promise
4) Any function can be converted to async
5) All async functions return a Promise
6) Error handling with try/catch 

## Example 1
function getData() {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(46)
        }, 1)
    })
}

//nicer 
async function start() {
    const result = await getData()
    console.log(result)
}

//same as  but start() nicer
async function start2() {
    getData()
        .then(Result => {
            console.log(result)
        })
}

start()

*** OUTPUT: 46 ***

**fetch** is a native browser feature --

## Error handling with try/catch

function getData() {
    return new Promise(resolve => {
        setTimeout(() => {
            reject('Something went wrong!')
        }, 1)
    })
}

async function start2() {
    const result = await getData()
        .catch(error => {
            console.log('ERROR: ${error}')
      })
    console.log (result)
}

**OUTPUT** : 'ERROR : Something went wrong'
                undefined //we dont want this line to run so thats why we implement it like:

function onSuccess(){
    console.log('Success : ${result}')
}

function onFailure{
    console.log('ERROR: ${error}')
}

async function start() {
    try {
        const result = await getData()
        onSuccess()
    } catch (error) {
        onFailure()   
    }
}





# FETCH API 

***GET***
const url =
'http://...'
// api_token
async function getData() {
    const response = await fetch(url)
    const data = await response.json()
    consol.log(data)
}

getData()

**some API's need tokens**
--> maintainer uses limits on how many times you can call them

Sometimes the API needs you to pass **Headers** to get access in a more secure way
So in the case you need to create a Response object

for example to access spotify's API you need to have a token

const url =
'http://...'


async function getData() {
    const request = new Request(url, {
        headers: {
            'Authorization': 'Bearer' 
            ...token
        }
    })
    const response = await fetch(request)
    const data = await response.json()
    console.log(data)
}

getData()