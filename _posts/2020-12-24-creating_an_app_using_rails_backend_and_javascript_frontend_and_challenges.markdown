---
layout: post
title:      "Creating an App using Rails Backend and JavaScript frontend and Challenges"
date:       2020-12-24 05:09:43 +0000
permalink:  creating_an_app_using_rails_backend_and_javascript_frontend_and_challenges
---


I have decided to create a cocktail app for this project. I was managing bars for a very long time and I’d written cocktail recipes everywhere, and most of the time I would lose the notes.
So, I thought, why not creating an app that will help other bartenders and bar managers to be more organized.
 
The setup of my app is quite simple, yet it was super challenging to make a functional app. 
 
I started with the backend.
For the app, I needed to create only two models – Cocktail and Ingredient, where Cocktail `has_many :ingredients` and Ingredient `belongs_to :cocktail`. 
Also, I decided to use `fast_json api` for my data. 
The way this gem structures data in the json format helps to access attributes of models quite easily. 
I added gem `gem fast_jsonapi` in the Gemfile and run `bundle install`. 
Having the gem installed first allowed me to run the following command to create Serializers for models.


`rails g serializer Cocktail name image_url instructions ingredients`

This created new controller named CocktailSerializer and it looks like this:

```
 class CocktailSerializer
  include FastJsonapi::ObjectSerializer
  attributes :name, :image, :instructions, :ingredients
end
```


And this is how the Serializer structures the data:

```
{
"data": [
{
"id": "1",
"type": "cocktail",
"attributes": {
"name": "Old-Fashioned",
"image": "https://cdn.diffords.com/contrib/stock-images/2020/01/5e34299193bb6.jpg",
"instructions": "Add the sugar and bitters to a rocks glass, then add water, and stir until sugar is nearly dissolved.\nFill the glass with large ice cubes, add the bourbon, and gently stir to combine.\nExpress the oil of an orange peel over the glass, then drop in",
"ingredients": [
{
"id": 1,
"name": "Bourbon - 2 oz",
"cocktail_id": 1,
"created_at": "2020-12-22T06:02:08.696Z",
"updated_at": "2020-12-22T06:02:08.696Z"
},
{
"id": 2,
"name": "Angostura Bitters - 3 dashes",
"cocktail_id": 1,
"created_at": "2020-12-22T06:02:08.703Z",
"updated_at": "2020-12-22T06:02:08.703Z"
},
{
"id": 3,
"name": "Sugar - 1/2 tsp",
"cocktail_id": 1,
"created_at": "2020-12-22T06:02:08.710Z",
"updated_at": "2020-12-22T06:02:08.710Z"
},
{
"id": 6,
"name": "Orange Peel",
"cocktail_id": 1,
"created_at": "2020-12-22T22:34:28.862Z",
"updated_at": "2020-12-22T22:34:28.862Z"
}
]
}
}
]}


```

To access this data structure, I added the following code to  methods in the controller.

```
def index
    cocktails = Cocktail.all

    render json: CocktailSerializer.new(cocktails)
  end
```

However, I forgot to put `CocktailSerializer.new` in the create method, which gave me a bit of trouble when I was fetching the data in JavaScript.

```
class Cocktail{

		static allCocktails = []

    constructor(cocktail){
    
        this.id = cocktail.id
        this.name = cocktail.attributes.name
        this.image = cocktail.attributes.image
        this.instructions = cocktail.attributes.instructions
        this.ingredients = cocktail.attributes.ingredients
      
        Cocktail.allCocktails.push(this)
        
    }

        static renderCocktails(){
        for (let cocktail of this.allCocktails){
           
            cocktail.renderCocktail()
        }
    }
         static fetchCocktails(){
             fetch(cocktailsURL)
            .then(response => response.json())
            .then(cocktails => {
    
             
                for(let cocktail of cocktails.data){
                let newCocktailList = new Cocktail(cocktail)
              newCocktailList.renderCocktail       
        }

        this.renderCocktails()
        })
     
    }}
```




 I will just include this code here to point to the Error I was getting.
 
 So, the user wants to create a new cocktail, fills in the information, clicks the submit button, and an error in the console shows up.
 
```
cocktail.js:8 Uncaught (in promise) TypeError: Cannot read property 'name' of undefined
    at new Cocktail (cocktail.js:8)
    at cocktail.js:95
```


I put `debugger` everywhere. I would always end up with the values I was expecting to get but was so blind and focused on JavaScript code, thinking that my constructor method was not correct.
But it was the backend . I would reload the page and the last cocktail entered would be rendered on the DOM and it would be created in the database. 
So what is wrong?? Why doesn’t it know what the `name` is?? Well… I forgot to put ` render json: CocktailSerializer.new(cocktail)` in the create method in the `CocktailController` .  The Rails backend didn't know how to read `cocktail.attributes.name` . 

 
 Once I was pointed to that missed step, JavaScript frontend and Rails Backend were communicating with each other.
 
 Check my GitHub repo [https://github.com/ValentinaPanic/cocktail-bible-frontend](http://)

