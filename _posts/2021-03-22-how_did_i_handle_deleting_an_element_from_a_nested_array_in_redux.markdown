---
layout: post
title:      "How did I handle deleting an element from a nested array in redux??"
date:       2021-03-23 02:14:08 +0000
permalink:  how_did_i_handle_deleting_an_element_from_a_nested_array_in_redux
---


Redux is worth using in your application. Having one store for handling the state of the whole app is convenient. But the more complex your database is, the harder is to get your redux working properly.

This is how data in my app is structured:

```
{
"data": {
"id": "331",
"type": "day",
"attributes": {
"date": "2021-03-23",
"user_id": 3,
"foods": [
{
"id": 409,
"name": "Eggs",
"meal_type": "Breakfast",
"day_id": 331,
},
{
"id": 410,
"name": "Chicken Noodle Soup",
"meal_type": "Lunch",
"day_id": 331,
},
{
"id": 411,
"name": "Grapes",
"meal_type": "Snack",
"day_id": 331,
},
{
"id": 412,
"name": "Sweet Potato Puree",
"meal_type": "Dinner",
"day_id": 331,
}
]
}
}
}
```

I wanted to enable deletion of a selected meal of a day without impacting the rest of the other meals that belong to the same date and user.  I knew I needed to access both `day id` and `food id` which I easily achieved in the action creator and the fetch delete request. Every time the user would click on a delete button, a database was instantly updated. However, the redux store didn’t know how to handle a click on a button, and it seemed that the whole day would be deleted. Once the page would refresh, the redux store would update correctly.
So, how did I manage to delete one meal and render the state accurately??

The logic had to be created in the reducer and those are the steps I took:

1. Redux’s state in my application has an array of `days`. The very first step was to find a day which `id` matches the action `dayId` (that I passed in as the argument in the component where I handle rendering day’s foods(meals))

 ` const day = state.days.find(day => day.id === action.dayId)`
 
2. Than,  I filtered through  foods of the found day and assigned it to a variable

 `const filteredFood = day.attributes.foods.filter(food => food.id !== action.foodId)`
 
 
3. Foods is nested under `day.attributes.foods`. After I filtered out the food which `id` matches the action `foodId`, I reassigned the day foods with the newly created array of foods.
 
 `day.attributes.foods = filteredFood`
 
4. To update the state correctly, I had to copy the previous state of days, filter through them to remove that day which `id` is matching the action `dayId`. Once that was successful, the day it is created above with filtered food, I added it back to the `days` array.

The finished code in a reducer is underneath.

```
 case "DELETE_FOOD":  
            const day = state.days.find(day => day.id === action.dayId)
         
            const filteredFood = day.attributes.foods.filter(food => food.id !== action.foodId)
            day.attributes.foods = filteredFood
        
         
       return {
          days:[
           ...state.days.filter(day => day.id === action.dayId),
           day
          ]
            
           }

```

The purpose of this blog is for those who struggle with rendering the updated state.  I did assume that you understand the flow of Redux and that code snippets will be enough to understand how to manage nested data.
 
If you would like to check my GitHub Repo [click here](https://github.com/ValentinaPanic/baby-eats-frontend)!
