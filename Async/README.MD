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
# Promises

