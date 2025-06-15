
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-sc ale=1.0">
    <title>Monthly Meal Prep Calendar</title>
    <style>
        .calendar-container {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    border-radius: 15px;
    box-shadow: 0 20px 40px rgba(0,0,0,0.1);
    overflow: hidden;
}
    
.header {
    background: linear-gradient(135deg, #4A772F 0%, #6A9955 100%); /* Deep earthy green gradient */
    color: white;
    padding: 20px;
    text-align: center;
}
    
.header h1 {
    margin: 0;
    font-size: 2.2em;
    font-weight: 300;
}
    
.header p {
    margin: 10px 0 0 0;
    opacity: 0.9;
    font-size: 1.1em;
}
    
.navigation {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 20px;
    background: #6A9955; /* Earthy green */
    color: white;
}
    
.nav-button {
    background: #FFC0CB; /* Pastel pink */
    color: #4A772F; /* Deep green text for contrast */
    border: none;
    padding: 10px 20px;
    border-radius: 25px;
    cursor: pointer;
    font-size: 1em;
    font-weight: bold;
    transition: all 0.3s ease;
}
    
.nav-button:hover {
    background: #FFB3C3; /* Slightly darker pink on hover */
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}
    
.current-month {
    font-size: 1.3em;
    font-weight: bold;
}
    
.week-header {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    background: #4A772F; /* Deep earthy green */
    color: white;
}
    
.week-header div {
    padding: 15px;
    text-align: center;
    font-weight: bold;
    font-size: 1.1em;
}
    
.calendar {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 1px;
    background: #F7E7ED; /* Very light pink background for grid gaps */
}
    
.day {
    background: white;
    min-height: 140px;
    padding: 12px;
    position: relative;
    transition: all 0.3s ease;
    border-bottom: 1px solid #E0BBE4; /* Light pinkish-purple for day borders */
}
    
.day:hover {
    background: #FFF0F5; /* Very light pink on hover */
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
}
    
.day-number {
    font-weight: bold;
    font-size: 1.2em;
    color: #4A772F; /* Deep green for day numbers */
    margin-bottom: 8px;
}
    
.week-indicator {
    position: absolute;
    top: 8px;
    right: 8px;
    background: linear-gradient(45deg, #FFC0CB, #FFB3C3); /* Pastel pink gradient */
    color: #4A772F; /* Deep green text */
    padding: 4px 8px;
    border-radius: 12px;
    font-size: 0.7em;
    font-weight: bold;
}
    
.meal {
    margin: 4px 0;
    padding: 6px 8px;
    border-radius: 8px;
    font-size: 0.75em;
    line-height: 1.2;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
}
    
.meal:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}
    
.lunch {
    background: linear-gradient(135deg, #FFB6C1 0%, #FFD1DC 100%); /* Lighter pastel pink */
    color: #4A772F; /* Deep green text */
    border-left: 4px solid #E0BBE4; /* Pastel pink border */
}
    
.dinner {
    background: linear-gradient(135deg, #A2CC90 0%, #C3E3B5 100%); /* Lighter earthy green */
    color: #4A772F; /* Deep green text */
    border-left: 4px solid #4A772F; /* Deep green border */
}
    
.prep-day {
    background: linear-gradient(135deg, #6A9955 0%, #8FB37F 100%); /* Earthy green gradient */
    color: white;
}
    
.prep-day .day-number {
    color: white;
}
    
.prep-indicator {
    background: #FFC0CB; /* Pastel pink */
    color: #4A772F; /* Deep green text */
    padding: 8px;
    border-radius: 8px;
    text-align: center;
    font-weight: bold;
    font-size: 0.9em;
    margin-top: 20px;
    cursor: pointer;
    transition: all 0.2s ease;
}
    
.prep-indicator:hover {
    background: #FFB3C3; /* Slightly darker pink on hover */
    transform: translateY(-1px);
}
    
.legend {
    display: flex;
    justify-content: center;
    gap: 30px;
    padding: 20px;
    background: #F7E7ED; /* Very light pink */
    border-top: 1px solid #E0BBE4; /* Light pinkish-purple border */
}
    
.legend-item {
    display: flex;
    align-items: center;
    gap: 10px;
}
    
.legend-color {
    width: 20px;
    height: 20px;
    border-radius: 4px;
}
    
.legend-lunch {
    background: linear-gradient(135deg, #FFB6C1 0%, #FFD1DC 100%); /* Lighter pastel pink */
    border-left: 4px solid #E0BBE4; /* Pastel pink border */
}
    
.legend-dinner {
    background: linear-gradient(135deg, #A2CC90 0%, #C3E3B5 100%); /* Lighter earthy green */
    border-left: 4px solid #4A772F; /* Deep green border */
}
    
.legend-prep {
    background: linear-gradient(135deg, #6A9955 0%, #8FB37F 100%); /* Earthy green gradient */
}
    
.rest-day {
    background: #F7E7ED; /* Very light pink */
    opacity: 0.7;
}
    
.rest-indicator {
    background: #B0B0B0; /* Neutral grey, can be customized further */
    color: white;
    padding: 8px;
    border-radius: 8px;
    text-align: center;
    font-weight: bold;
    font-size: 0.9em;
    margin-top: 20px;
}
    
.other-month {
    background: #F7E7ED; /* Very light pink */
    opacity: 0.3;
}
    
.today {
    background: linear-gradient(135deg, #FFC0CB 0%, #FF9AA4 100%); /* Brighter pastel pink for 'today' */
    color: #4A772F; /* Deep green text */
    border: 2px solid #4A772F; /* Added a border for more emphasis */
}
    
.today .day-number {
    color: #4A772F; /* Deep green for day number */
}
    
/* Modal Styles */
.modal {
    display: none;
    position: fixed;
    z-index: 1000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.5);
    animation: fadeIn 0.3s ease;
}
    
.modal-content {
    background-color: white;
    margin: 5% auto;
    padding: 0;
    border-radius: 15px;
    width: 90%;
    max-width: 600px;
    max-height: 80vh;
    overflow-y: auto;
    box-shadow: 0 20px 40px rgba(0,0,0,0.3);
    animation: slideIn 0.3s ease;
}
    
.modal-header {
    background: linear-gradient(135deg, #4A772F 0%, #6A9955 100%); /* Deep earthy green gradient */
    color: white;
    padding: 20px;
    border-radius: 15px 15px 0 0;
    position: relative;
}
    
.modal-header h2 {
    margin: 0;
    font-size: 1.5em;
}
    
.close {
    position: absolute;
    right: 20px;
    top: 20px;
    color: white;
    font-size: 28px;
    font-weight: bold;
    cursor: pointer;
    width: 30px;
    height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: background 0.2s ease;
}
    
.close:hover {
    background: rgba(255,255,255,0.2);
}
    
.modal-body {
    padding: 20px;
}
    
.recipe-section {
    margin-bottom: 20px;
}
    
.recipe-section h3 {
    color: #4A772F; /* Deep green for recipe headings */
    margin-bottom: 10px;
    font-size: 1.2em;
    border-bottom: 2px solid #FFC0CB; /* Pastel pink border */
    padding-bottom: 5px;
}
    
.ingredients-list, .instructions-list {
    list-style: none;
    padding: 0;
}
    
.ingredients-list li {
    background: #F7E7ED; /* Very light pink background */
    margin: 5px 0;
    padding: 8px 12px;
    border-radius: 5px;
    border-left: 4px solid #FFC0CB; /* Pastel pink border */
}
    
.instructions-list li {
    background: #EAF7E2; /* Very light green background */
    margin: 8px 0;
    padding: 10px 15px;
    border-radius: 5px;
    border-left: 4px solid #6A9955; /* Earthy green border */
    counter-increment: step-counter;
}
    
.instructions-list li::before {
    content: counter(step-counter);
    background: #6A9955; /* Earthy green */
    color: white;
    font-weight: bold;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;
    font-size: 0.9em;
}
    
.instructions-list {
    counter-reset: step-counter;
}
    
.grocery-section {
    background: #EAF7E2; /* Very light green */
    padding: 15px;
    border-radius: 10px;
    margin-bottom: 15px;
}
    
.prep-task {
    background: #FFF7ED; /* Light orange-ish tint for prep task */
    padding: 10px;
    margin: 8px 0;
    border-radius: 5px;
    border-left: 4px solid #FFC0CB; /* Pastel pink border */
}
    
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}
    
@keyframes slideIn {
    from { transform: translateY(-50px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
}
    
@media (max-width: 768px) {
    .calendar {
        grid-template-columns: repeat(7, 1fr);
        gap: 1px;
    }
        
    .week-header div {
        font-size: 0.9em;
        padding: 10px 5px;
    }
        
    .day {
        min-height: 100px;
        padding: 8px;
    }
        
    .meal {
        font-size: 0.65em;
        padding: 4px 6px;
        margin: 2px 0;
    }
        
    .legend {
        flex-direction: column;
        align-items: center;
        gap: 15px;
    }
        
    .navigation {
        flex-direction: column;
        gap: 10px;
    }
        
    .modal-content {
        width: 95%;
        margin: 10% auto;
    }
}
    </style>
</head>
<body>
    <div class="calendar-container">
        <div class="header">
            <h1>Monthly Meal Prep Calendar</h1>
            <p>Rotating 4-Week Meal Plan | 2 Servings Each Meal</p>
        </div>
        
        <div class="navigation">
            <button class="nav-button" onclick="changeMonth(-1)">← Previous</button>
            <div class="current-month" id="currentMonth"></div>
            <button class="nav-button" onclick="changeMonth(1)">Next →</button>
        </div>
        
        <div class="week-header">
            <div>Sunday</div>
            <div>Monday</div>
            <div>Tuesday</div>
            <div>Wednesday</div>
            <div>Thursday</div>
            <div>Friday</div>
            <div>Saturday</div>
        </div>
        
        <div class="calendar" id="calendar">
            </div>
        
        <div class="legend">
            <div class="legend-item">
                <div class="legend-color legend-lunch"></div>
                <span>Lunch (2 servings)</span>
            </div>
            <div class="legend-item">
                <div class="legend-color legend-dinner"></div>
                <span>Dinner (2 servings)</span>
            </div>
            <div class="legend-item">
                <div class="legend-color legend-prep"></div>
                <span>Prep Day</span>
            </div>
        </div>
    </div>

    <div id="recipeModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 id="modalTitle"></h2>
                <span class="close" onclick="closeModal()">&times;</span>
            </div>
            <div class="modal-body" id="modalBody">
                </div>
        </div>
    </div>

    <script>
        // Global variables
        let currentMonth = new Date().getMonth();
        let currentYear = new Date().getFullYear();
        
        // Enhanced meal plan with shuffled dinners for variety
        const mealPlan = {
            week1: {
                lunches: [
                    "Kafta with Potatoes",
                    "Lebanese Shawarma Bowl", 
                    "Chicken with Roasted Veggies"
                ],
                dinners: [
                    "Caesar Salad",
                    "Mediterranean Quinoa Salad",
                    "Asian Chicken Salad"
                ],
                sunday_meals: {
                    lunch: "Beef Cubes with Potato",
                    dinner: "Chickpea Salad"
                }
            },
            week2: {
                lunches: [
                    "Shrimp Orzo",
                    "Korean Beef Bowl",
                    "Sweet and Sour Chicken"
                ],
                dinners: [
                    "Tuna Salad",
                    "Chickpea Salad",
                    "Mediterranean Orzo Salad"
                ],
                sunday_meals: {
                    lunch: "Teriyaki Chicken",
                    dinner: "Quinoa Halloumi Salad"
                }
            },
            week3: {
                lunches: [
                    "Bazella with Carrots",
                    "Chicken Fajita Bowl",
                    "Meatballs with Marinara"
                ],
                dinners: [
                    "Mediterranean Orzo Salad",
                    "Asian Chicken Salad",
                    "Chicken Salad"
                ],
                sunday_meals: {
                    lunch: "Lebanese Shawarma Bowl",
                    dinner: "Caesar Salad"
                }
            },
            week4: {
                lunches: [
                    "Chicken Curry",
                    "Kafta with Potatoes",
                    "Teriyaki Chicken"
                ],
                dinners: [
                    "Quinoa Halloumi Salad",
                    "Pepper and Cucumber Salad",
                    "Mexican Burrito Bowl"
                ],
                sunday_meals: {
                    lunch: "Sweet and Sour Chicken",
                    dinner: "Burghul Banadoura"
                }
            }
        };

        // Recipe database with ingredients and instructions
        const recipes = {
            "Kafta with Potatoes": {
                ingredients: [
                    "500g ground lamb or beef",
                    "4 medium potatoes, cubed",
                    "1 large onion, finely chopped",
                    "2 cloves garlic, minced",
                    "1/4 cup fresh parsley, chopped",
                    "1 tsp allspice",
                    "1 tsp cinnamon",
                    "Salt and pepper to taste",
                    "2 tbsp olive oil"
                ],
                instructions: [
                    "Mix ground meat with half the onion, garlic, parsley, and spices",
                    "Form into small oval-shaped patties",
                    "Heat olive oil in a large pan over medium heat",
                    "Brown kafta patties on both sides, then remove",
                    "In same pan, sauté remaining onions until golden",
                    "Add potatoes and cook for 5 minutes",
                    "Return kafta to pan, add water to barely cover",
                    "Simmer covered for 25-30 minutes until potatoes are tender"
                ]
            },
            "Lebanese Shawarma Bowl": {
                ingredients: [
                    "500g chicken thighs, sliced thin",
                    "2 cups basmati rice",
                    "1 cucumber, diced",
                    "2 tomatoes, diced",
                    "1/2 red onion, sliced",
                    "1/4 cup tahini",
                    "2 tbsp lemon juice",
                    "2 tsp shawarma spice blend",
                    "Fresh parsley and mint"
                ],
                instructions: [
                    "Marinate chicken in shawarma spices and lemon juice for 2 hours",
                    "Cook rice according to package instructions",
                    "Pan-fry chicken until golden and cooked through",
                    "Prepare vegetables and arrange in bowls",
                    "Mix tahini with lemon juice and water for sauce",
                    "Assemble bowls with rice, chicken, vegetables, and sauce"
                ]
            },
            "Caesar Salad": {
                ingredients: [
                    "2 large romaine lettuce heads",
                    "1/2 cup parmesan cheese, grated",
                    "1 cup croutons",
                    "4 anchovy fillets (optional)",
                    "2 cloves garlic",
                    "1/4 cup lemon juice",
                    "1/4 cup olive oil",
                    "1 egg yolk",
                    "1 tsp Dijon mustard"
                ],
                instructions: [
                    "Wash and chop romaine lettuce",
                    "Make dressing: blend garlic, anchovies, lemon juice, egg yolk, and mustard",
                    "Slowly add olive oil while blending",
                    "Toss lettuce with dressing",
                    "Top with parmesan and croutons",
                    "Serve immediately"
                ]
            },
            "Chickpea Salad": {
                ingredients: [
                    "2 cans chickpeas, drained and rinsed",
                    "1 cucumber, diced",
                    "2 tomatoes, diced",
                    "1/2 red onion, finely chopped",
                    "1/4 cup fresh parsley",
                    "3 tbsp olive oil",
                    "2 tbsp lemon juice",
                    "1 tsp sumac",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Drain and rinse chickpeas thoroughly",
                    "Dice all vegetables finely",
                    "Chop parsley and mint",
                    "Whisk together olive oil, lemon juice, and sumac",
                    "Combine all ingredients in a large bowl",
                    "Toss well and let marinate for 30 minutes",
                    "Adjust seasoning before serving"
                ]
            },
            "Chicken with Roasted Veggies": {
                ingredients: [
                    "500g chicken breast, cubed",
                    "2 bell peppers, chopped",
                    "1 zucchini, chopped",
                    "1 red onion, chopped",
                    "2 tbsp olive oil",
                    "1 tsp dried oregano",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Preheat oven to 200°C (400°F)",
                    "Toss chicken and vegetables with olive oil, oregano, salt, and pepper",
                    "Spread on a baking sheet in a single layer",
                    "Roast for 20-25 minutes, or until chicken is cooked through and vegetables are tender-crisp",
                    "Serve hot"
                ]
            },
            "Mediterranean Quinoa Salad": {
                ingredients: [
                    "1 cup quinoa, rinsed",
                    "2 cups vegetable broth or water",
                    "1 cucumber, diced",
                    "1 cup cherry tomatoes, halved",
                    "1/2 red onion, finely chopped",
                    "1/2 cup Kalamata olives, pitted and halved",
                    "1/4 cup fresh parsley, chopped",
                    "1/4 cup fresh mint, chopped",
                    "1/4 cup feta cheese, crumbled",
                    "3 tbsp olive oil",
                    "2 tbsp lemon juice",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Cook quinoa according to package directions using broth/water",
                    "Let cool completely",
                    "In a large bowl, combine cooked quinoa, cucumber, cherry tomatoes, red onion, olives, parsley, and mint",
                    "Whisk together olive oil, lemon juice, salt, and pepper",
                    "Pour dressing over salad and toss to combine",
                    "Stir in crumbled feta cheese just before serving"
                ]
            },
            "Asian Chicken Salad": {
                ingredients: [
                    "2 cups cooked chicken, shredded",
                    "4 cups shredded cabbage (green or napa)",
                    "1 cup shredded carrots",
                    "1/2 red bell pepper, thinly sliced",
                    "1/4 cup chopped green onions",
                    "1/4 cup chopped cilantro",
                    "1/4 cup rice vinegar",
                    "2 tbsp soy sauce",
                    "1 tbsp sesame oil",
                    "1 tbsp honey or maple syrup",
                    "1 tsp grated fresh ginger",
                    "1/2 cup chopped peanuts or cashews (optional)"
                ],
                instructions: [
                    "In a large bowl, combine shredded chicken, cabbage, carrots, red bell pepper, green onions, and cilantro",
                    "In a small bowl, whisk together rice vinegar, soy sauce, sesame oil, honey, and ginger until well combined",
                    "Pour dressing over the salad ingredients and toss to coat evenly",
                    "If desired, add chopped peanuts or cashews for crunch",
                    "Serve immediately or chill for later"
                ]
            },
            "Beef Cubes with Potato": {
                ingredients: [
                    "500g beef stew meat, cubed",
                    "3 medium potatoes, peeled and cubed",
                    "1 large onion, chopped",
                    "2 carrots, sliced",
                    "2 stalks celery, sliced",
                    "4 cups beef broth",
                    "2 tbsp olive oil",
                    "1 tbsp tomato paste",
                    "1 tsp dried thyme",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Heat olive oil in a large pot or Dutch oven over medium-high heat",
                    "Brown beef cubes on all sides, then remove and set aside",
                    "Add onion, carrots, and celery to the pot and cook until softened, about 5-7 minutes",
                    "Stir in tomato paste and thyme, cook for 1 minute",
                    "Return beef to the pot, add beef broth, salt, and pepper",
                    "Bring to a simmer, then reduce heat, cover, and cook for 1.5 - 2 hours until beef is tender",
                    "Add potatoes and continue cooking for another 20-30 minutes, or until potatoes are tender",
                    "Adjust seasoning as needed and serve"
                ]
            },
            "Shrimp Orzo": {
                ingredients: [
                    "400g raw shrimp, peeled and deveined",
                    "1 cup orzo pasta",
                    "2 tbsp olive oil",
                    "1 onion, finely chopped",
                    "2 cloves garlic, minced",
                    "1 can (400g) diced tomatoes",
                    "1/2 cup white wine (optional)",
                    "2 cups vegetable or chicken broth",
                    "1/4 cup fresh parsley, chopped",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Heat olive oil in a large skillet over medium heat",
                    "Add onion and cook until softened, about 5 minutes",
                    "Add garlic and cook for 1 minute more until fragrant",
                    "Stir in orzo and toast for 2 minutes",
                    "Pour in white wine (if using) and cook until absorbed",
                    "Add diced tomatoes and broth, bring to a simmer",
                    "Cover and cook for 10-12 minutes, stirring occasionally, until orzo is almost cooked",
                    "Stir in shrimp and cook for 3-5 minutes until pink and cooked through",
                    "Season with salt and pepper, stir in fresh parsley before serving"
                ]
            },
            "Korean Beef Bowl": {
                ingredients: [
                    "500g beef sirloin or flank steak, thinly sliced",
                    "2 tbsp soy sauce",
                    "1 tbsp sesame oil",
                    "1 tbsp brown sugar",
                    "2 cloves garlic, minced",
                    "1 tsp grated fresh ginger",
                    "1/2 red bell pepper, sliced",
                    "1 cup broccoli florets",
                    "Cooked rice for serving",
                    "Sesame seeds and sliced green onions for garnish"
                ],
                instructions: [
                    "In a bowl, combine soy sauce, sesame oil, brown sugar, garlic, and ginger to make the marinade",
                    "Add sliced beef to the marinade and toss to coat; let marinate for at least 30 minutes",
                    "Heat a large skillet or wok over medium-high heat",
                    "Add marinated beef and stir-fry until cooked through and slightly caramelized",
                    "Add red bell pepper and broccoli, stir-fry for 3-5 minutes until tender-crisp",
                    "Serve hot over cooked rice, garnished with sesame seeds and green onions"
                ]
            },
            "Sweet and Sour Chicken": {
                ingredients: [
                    "500g chicken breast, cubed",
                    "1 bell pepper (any color), chopped",
                    "1 onion, chopped",
                    "1 can (227g) pineapple chunks, drained (reserve juice)",
                    "2 tbsp cornstarch",
                    "1/4 cup white vinegar",
                    "1/4 cup sugar",
                    "2 tbsp soy sauce",
                    "1 tbsp ketchup",
                    "1/2 cup chicken broth",
                    "2 tbsp oil for cooking"
                ],
                instructions: [
                    "Toss chicken cubes with 1 tbsp cornstarch",
                    "Heat oil in a large skillet or wok over medium-high heat",
                    "Stir-fry chicken until lightly browned and cooked through, remove from pan",
                    "In the same pan, add bell pepper and onion, stir-fry for 3-4 minutes until slightly softened",
                    "In a small bowl, whisk together remaining cornstarch with vinegar, sugar, soy sauce, ketchup, pineapple juice, and chicken broth",
                    "Pour sauce into the skillet, bring to a simmer, stirring until thickened",
                    "Return chicken to the pan along with pineapple chunks",
                    "Toss to coat everything in the sauce",
                    "Serve hot with rice or noodles"
                ]
            },
            "Tuna Salad": {
                ingredients: [
                    "2 cans (140g each) tuna in water, drained",
                    "1/2 cup mayonnaise or Greek yogurt",
                    "1 stalk celery, finely chopped",
                    "1/4 cup red onion, finely chopped",
                    "1 tbsp fresh dill, chopped (optional)",
                    "1 tbsp Dijon mustard",
                    "Salt and black pepper to taste"
                ],
                instructions: [
                    "In a medium bowl, flake the drained tuna with a fork",
                    "Add mayonnaise (or Greek yogurt), chopped celery, red onion, dill (if using), and Dijon mustard",
                    "Mix well to combine all ingredients",
                    "Season with salt and black pepper to taste",
                    "Serve as a sandwich filling, with crackers, or on a bed of lettuce"
                ]
            },
            "Mediterranean Orzo Salad": {
                ingredients: [
                    "1 cup orzo pasta, cooked and cooled",
                    "1 cup cherry tomatoes, halved",
                    "1 cucumber, diced",
                    "1/2 red onion, thinly sliced",
                    "1/2 cup Kalamata olives, pitted and halved",
                    "1/4 cup fresh parsley, chopped",
                    "1/4 cup feta cheese, crumbled",
                    "3 tbsp olive oil",
                    "2 tbsp red wine vinegar",
                    "1 tsp dried oregano",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Cook orzo pasta according to package directions; drain and rinse with cold water to cool, then set aside",
                    "In a large bowl, combine the cooled orzo, cherry tomatoes, cucumber, red onion, olives, and parsley",
                    "In a small bowl, whisk together olive oil, red wine vinegar, oregano, salt, and pepper to make the dressing",
                    "Pour the dressing over the orzo mixture and toss gently to coat all ingredients",
                    "Stir in the crumbled feta cheese",
                    "Serve immediately or chill for flavors to meld"
                ]
            },
            "Quinoa Halloumi Salad": {
                ingredients: [
                    "1 cup quinoa, rinsed",
                    "2 cups water or vegetable broth",
                    "250g halloumi cheese, sliced or cubed",
                    "2 tbsp olive oil",
                    "1 red bell pepper, chopped",
                    "1 yellow bell pepper, chopped",
                    "1/2 red onion, thinly sliced",
                    "1/4 cup fresh mint, chopped",
                    "1/4 cup fresh parsley, chopped",
                    "3 tbsp lemon juice",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "Cook quinoa according to package directions with water or broth; let cool",
                    "Heat 1 tbsp olive oil in a non-stick skillet over medium heat",
                    "Add halloumi cheese and cook until golden brown on both sides, about 2-3 minutes per side; remove and set aside",
                    "In a large bowl, combine cooled quinoa, roasted bell peppers, red onion, mint, and parsley",
                    "Whisk together remaining olive oil, lemon juice, salt, and pepper for the dressing",
                    "Pour dressing over the salad and toss well",
                    "Gently fold in the cooked halloumi cheese",
                    "Serve warm or at room temperature"
                ]
            },
            "Bazella with Carrots": {
                ingredients: [
                    "500g ground beef or lamb (optional)",
                    "1 tbsp olive oil",
                    "1 onion, chopped",
                    "2 cloves garlic, minced",
                    "1 can (400g) crushed tomatoes",
                    "4 cups frozen peas and carrots mix",
                    "1 cup water or beef broth",
                    "1 tsp seven spice (baharat)",
                    "Salt and pepper to taste",
                    "Cooked rice for serving"
                ],
                instructions: [
                    "If using meat, brown ground beef/lamb in olive oil in a large pot; drain excess fat and set aside",
                    "In the same pot, sauté onion until softened, then add garlic and cook for 1 minute more",
                    "Stir in crushed tomatoes, peas and carrots, water/broth, and seven spice",
                    "If using meat, return it to the pot",
                    "Bring to a simmer, then reduce heat, cover, and cook for 20-25 minutes, or until vegetables are tender",
                    "Season with salt and pepper to taste",
                    "Serve hot with rice"
                ]
            },
            "Chicken Fajita Bowl": {
                ingredients: [
                    "500g chicken breast, sliced into strips",
                    "2 bell peppers (different colors), sliced",
                    "1 onion, sliced",
                    "2 tbsp olive oil",
                    "2 tsp fajita seasoning",
                    "1 cup cooked rice",
                    "1/2 cup black beans, rinsed and drained",
                    "1/4 cup corn kernels (frozen or canned)",
                    "Salsa, avocado slices, and lime wedges for serving"
                ],
                instructions: [
                    "Heat olive oil in a large skillet over medium-high heat",
                    "Add chicken strips and cook until browned, about 5-7 minutes; remove and set aside",
                    "Add sliced bell peppers and onion to the same skillet; cook for 5-7 minutes until tender-crisp",
                    "Stir in fajita seasoning and cook for 1 minute more",
                    "Return chicken to the skillet and toss to combine with vegetables",
                    "To assemble bowls: spoon cooked rice into bowls, top with chicken and vegetable mixture",
                    "Add black beans and corn",
                    "Serve with salsa, avocado slices, and lime wedges"
                ]
            },
            "Meatballs with Marinara": {
                ingredients: [
                    "500g ground beef or turkey",
                    "1/2 cup breadcrumbs",
                    "1/4 cup grated Parmesan cheese",
                    "1 egg",
                    "2 cloves garlic, minced",
                    "1/4 cup fresh parsley, chopped",
                    "Salt and pepper to taste",
                    "1 tbsp olive oil",
                    "1 jar (680g) marinara sauce",
                    "Cooked pasta or zucchini noodles for serving"
                ],
                instructions: [
                    "In a large bowl, combine ground meat, breadcrumbs, Parmesan, egg, garlic, parsley, salt, and pepper",
                    "Mix gently until just combined, do not overmix",
                    "Form mixture into 1-inch meatballs",
                    "Heat olive oil in a large skillet over medium heat",
                    "Brown meatballs on all sides, working in batches if necessary",
                    "Drain any excess fat from the skillet",
                    "Pour marinara sauce over the browned meatballs",
                    "Bring to a simmer, then reduce heat, cover, and cook for 15-20 minutes, or until meatballs are cooked through",
                    "Serve hot over cooked pasta or zucchini noodles"
                ]
            },
            "Chicken Salad": {
                ingredients: [
                    "2 cups cooked chicken, shredded or diced",
                    "1/2 cup mayonnaise or Greek yogurt",
                    "1/4 cup celery, finely chopped",
                    "2 tbsp red onion, finely chopped",
                    "1 tbsp fresh dill or chives, chopped (optional)",
                    "1 tsp Dijon mustard",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "In a medium bowl, combine the cooked shredded or diced chicken",
                    "Add mayonnaise (or Greek yogurt), chopped celery, red onion, dill/chives (if using), and Dijon mustard",
                    "Mix well until all ingredients are thoroughly combined and the chicken is coated",
                    "Season with salt and pepper to your taste",
                    "Serve on bread for sandwiches, with crackers, or as a topping for green salads"
                ]
            },
            "Chicken Curry": {
                ingredients: [
                    "500g chicken breast or thighs, cubed",
                    "1 tbsp olive oil",
                    "1 onion, chopped",
                    "2 cloves garlic, minced",
                    "1 tbsp ginger, grated",
                    "2 tbsp curry powder",
                    "1 can (400ml) coconut milk",
                    "1 cup chicken broth",
                    "1 bell pepper, chopped",
                    "1 cup spinach",
                    "Cooked rice for serving"
                ],
                instructions: [
                    "Heat olive oil in a large pot or deep skillet over medium heat",
                    "Add onion and cook until softened, about 5 minutes",
                    "Stir in garlic and ginger, cook for 1 minute until fragrant",
                    "Add curry powder and cook for 1 minute more, stirring constantly",
                    "Add chicken cubes and cook until lightly browned on all sides",
                    "Pour in coconut milk and chicken broth, bring to a simmer",
                    "Add bell pepper, then reduce heat, cover, and cook for 15-20 minutes, or until chicken is cooked through",
                    "Stir in spinach until wilted",
                    "Serve hot with cooked rice"
                ]
            },
            "Pepper and Cucumber Salad": {
                ingredients: [
                    "2 cucumbers, diced",
                    "2 bell peppers (any color), diced",
                    "1/2 red onion, thinly sliced",
                    "1/4 cup fresh dill, chopped",
                    "3 tbsp olive oil",
                    "2 tbsp white wine vinegar",
                    "Salt and pepper to taste"
                ],
                instructions: [
                    "In a large bowl, combine diced cucumbers, diced bell peppers, thinly sliced red onion, and fresh dill",
                    "In a small bowl, whisk together olive oil, white wine vinegar, salt, and pepper to make the dressing",
                    "Pour the dressing over the vegetables and toss gently to ensure everything is well coated",
                    "Let the salad sit for at least 15 minutes before serving to allow the flavors to meld",
                    "Serve chilled as a refreshing side salad"
                ]
            },
            "Mexican Burrito Bowl": {
                ingredients: [
                    "1 cup cooked rice (brown or white)",
                    "1 can (400g) black beans, rinsed and drained",
                    "1 cup cooked corn kernels",
                    "1 cup cooked chicken or ground beef (optional), seasoned with taco seasoning",
                    "1/2 avocado, diced",
                    "1/4 cup salsa",
                    "2 tbsp plain Greek yogurt or sour cream",
                    "1 tbsp chopped fresh cilantro",
                    "Lime wedges for serving"
                ],
                instructions: [
                    "In individual bowls, layer the ingredients starting with cooked rice",
                    "Add black beans, corn, and seasoned chicken or ground beef",
                    "Top with diced avocado, salsa, and a dollop of Greek yogurt/sour cream",
                    "Garnish with fresh cilantro and serve with lime wedges"
                ]
            },
            "Burghul Banadoura": {
                ingredients: [
                    "1 cup coarse bulgur wheat, rinsed",
                    "2 tbsp olive oil",
                    "1 large onion, finely chopped",
                    "2 cloves garlic, minced",
                    "1 can (800g) crushed tomatoes",
                    "2 cups water or vegetable broth",
                    "1/2 tsp cumin",
                    "1/4 tsp cinnamon",
                    "Salt and pepper to taste",
                    "Fresh mint or parsley for garnish"
                ],
                instructions: [
                    "Heat olive oil in a large pot or deep skillet over medium heat",
                    "Add onion and cook until softened and translucent, about 5-7 minutes",
                    "Add garlic and cook for 1 minute until fragrant",
                    "Stir in rinsed bulgur wheat, coating it in the oil for 1-2 minutes",
                    "Pour in crushed tomatoes and water/broth",
                    "Add cumin, cinnamon, salt, and pepper; stir well to combine",
                    "Bring the mixture to a boil, then reduce heat to low, cover, and simmer for 20-25 minutes, or until the bulgur is tender and has absorbed the liquid",
                    "Fluff with a fork and let it sit covered for 5 minutes before serving",
                    "Garnish with fresh mint or parsley"
                ]
            }
            // Add more recipes as needed...
        };

        // Grocery lists for each week
        const groceryLists = {
            week1: {
                proteins: ["500g ground lamb/beef", "500g chicken thighs", "500g beef cubes"],
                vegetables: ["Potatoes", "Onions", "Garlic", "Tomatoes", "Cucumbers", "Romaine lettuce", "Fresh parsley", "Fresh mint"],
                pantry: ["Rice", "Olive oil", "Shawarma spice blend", "Tahini", "Parmesan cheese", "Chickpeas", "Croutons", "Allspice", "Cinnamon", "Lemon juice"],
                prep_tasks: [
                    "Marinate chicken for shawarma",
                    "Pre-chop all vegetables (cucumbers, tomatoes, onions)",
                    "Cook rice in batches",
                    "Prepare kafta patties",
                    "Make Caesar dressing (if preferred fresh)",
                    "Prepare Chickpea Salad dressing"
                ]
            },
            week2: {
                proteins: ["500g shrimp", "500g beef strips (e.g., sirloin, flank)", "500g chicken breast"],
                vegetables: ["Bell peppers", "Onions", "Carrots", "Broccoli", "Mixed greens (for salads)"],
                pantry: ["Orzo pasta", "Soy sauce", "Sesame oil", "Ginger", "Garlic", "Brown sugar", "White vinegar", "Ketchup", "Cornstarch", "Pineapple chunks (canned)", "Tuna (canned)", "Mayonnaise/Greek yogurt", "Celery", "Dijon mustard"],
                prep_tasks: [
                    "Marinate beef in Korean sauce",
                    "Clean and devein shrimp",
                    "Prep all stir-fry vegetables (bell peppers, onions, broccoli)",
                    "Cook orzo pasta in batches",
                    "Prepare Sweet and Sour sauce ingredients",
                    "Make Tuna Salad base"
                ]
            },
            week3: {
                proteins: ["500g ground beef or turkey", "500g chicken breast"],
                vegetables: ["Carrots", "Peas", "Bell peppers", "Onions", "Mushrooms", "Cherry tomatoes", "Cucumber"],
                pantry: ["Quinoa", "Halloumi cheese", "Tomato sauce/Marinara", "Fajita seasoning", "Breadcrumbs", "Eggs", "Seven spice (Baharat)", "Black beans (canned)", "Corn kernels (canned)", "Salsa", "Avocado", "Lime"],
                prep_tasks: [
                    "Prepare meatballs and freeze half for later use (if desired)",
                    "Cook quinoa in batches",
                    "Prep fajita vegetables (bell peppers, onions)",
                    "Cube halloumi cheese for easy cooking",
                    "Chop herbs for salads"
                ]
            },
            week4: {
                proteins: ["500g chicken thighs", "Ground turkey or beef (for Mexican bowl)", "Canned tuna"],
                vegetables: ["Mixed vegetables for curry (e.g., potatoes, green beans)", "Lettuce", "Peppers", "Avocado", "Cucumbers", "Mint"],
                pantry: ["Rice", "Coconut milk", "Bulgur wheat", "Black beans", "Tortillas (optional for burrito bowl)", "Curry powder", "Cumin", "Cinnamon"],
                prep_tasks: [
                    "Prepare curry paste/spice mix",
                    "Cook bulgur wheat",
                    "Prep burrito bowl ingredients (chop veggies, season meat if using)",
                    "Make pepper and cucumber salad dressing",
                    "Cook rice for the week"
                ]
            }
        };

        const monthNames = [
            "January", "February", "March", "April", "May", "June",
            "July", "August", "September", "October", "November", "December"
        ];

        function updateMonthHeader() {
            document.getElementById('currentMonth').textContent = 
                `${monthNames[currentMonth]} ${currentYear}`;
        }

        function changeMonth(direction) {
            currentMonth += direction;
            if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            } else if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            }
            generateCalendar();
        }

        function showRecipe(recipeName) {
            const recipe = recipes[recipeName];
            if (!recipe) {
                alert('Recipe details coming soon!');
                return;
            }

            document.getElementById('modalTitle').textContent = recipeName;
            
            const modalBody = document.getElementById('modalBody');
            modalBody.innerHTML = `
                <div class="recipe-section">
                    <h3>🛒 Ingredients</h3>
                    <ul class="ingredients-list">
                        ${recipe.ingredients.map(ingredient => `<li>${ingredient}</li>`).join('')}
                    </ul>
                </div>
                <div class="recipe-section">
                    <h3>👨‍🍳 Instructions</h3>
                    <ol class="instructions-list">
                        ${recipe.instructions.map(instruction => `<li>${instruction}</li>`).join('')}
                    </ol>
                </div>
            `;
            
            document.getElementById('recipeModal').style.display = 'block';
        }

        function showPrepDay(weekNum) {
            const weekData = groceryLists[`week${weekNum}`];
            if (!weekData) {
                alert('Prep details coming soon!');
                return;
            }

            document.getElementById('modalTitle').textContent = `Week ${weekNum} - Prep Day`;
            
            const modalBody = document.getElementById('modalBody');
            modalBody.innerHTML = `
                <div class="grocery-section">
                    <h3>🛒 Grocery List</h3>
                    <div class="recipe-section">
                        <h4>Proteins</h4>
                        <ul class="ingredients-list">
                            ${weekData.proteins.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                    <div class="recipe-section">
                        <h4>Vegetables & Fresh</h4>
                        <ul class="ingredients-list">
                            ${weekData.vegetables.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                    <div class="recipe-section">
                        <h4>Pantry Items</h4>
                        <ul class="ingredients-list">
                            ${weekData.pantry.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                </div>
                <div class="recipe-section">
                    <h3>📋 Prep Tasks</h3>
                    ${weekData.prep_tasks.map(task => `<div class="prep-task">${task}</div>`).join('')}
                </div>
            `;
            
            document.getElementById('recipeModal').style.display = 'block';
        }

        function closeModal() {
            document.getElementById('recipeModal').style.display = 'none';
        }

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('recipeModal');
            if (event.target == modal) {
                modal.style.display = 'none';
            }
        }

        function generateCalendar() {
            const calendar = document.getElementById('calendar');
            calendar.innerHTML = '';
            
            updateMonthHeader();
            
            const firstDay = new Date(currentYear, currentMonth, 1).getDay();
            const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
            const daysInPrevMonth = new Date(currentYear, currentMonth, 0).getDate();
            
            const today = new Date();
            const isCurrentMonth = currentMonth === today.getMonth() && currentYear === today.getFullYear();
            const todayDate = today.getDate();
            
            const totalCells = 42; // Ensures 6 rows for consistent layout
            
            for (let i = 0; i < totalCells; i++) {
                const dayElement = document.createElement('div');
                dayElement.className = 'day';
                
                let dayNumber, isCurrentMonthDay = false;
                
                if (i < firstDay) {
                    dayNumber = daysInPrevMonth - (firstDay - 1 - i);
                    dayElement.classList.add('other-month');
                } else if (i < firstDay + daysInMonth) {
                    dayNumber = i - firstDay + 1;
                    isCurrentMonthDay = true;
                    
                    if (isCurrentMonth && dayNumber === todayDate) {
                        dayElement.classList.add('today');
                    }
                } else {
                    dayNumber = i - firstDay - daysInMonth + 1;
                    dayElement.classList.add('other-month');
                }
                
                const dayNumberElement = document.createElement('div');
                dayNumberElement.className = 'day-number';
                dayNumberElement.textContent = dayNumber;
                dayElement.appendChild(dayNumberElement);
                
                if (isCurrentMonthDay) {
                    const dayOfWeek = (i % 7); // 0 = Sunday, 1 = Monday, etc.
                    const weekNumber = Math.floor((dayNumber - 1) / 7) + 1; // Week 1-4 for meal plan rotation
                    const mealWeek = ((weekNumber - 1) % 4) + 1; // Rotates between 1, 2, 3, 4
                    
                    const weekIndicator = document.createElement('div');
                    weekIndicator.className = 'week-indicator';
                    weekIndicator.textContent = `W${mealWeek}`;
                    dayElement.appendChild(weekIndicator);
                    
                    if (dayOfWeek === 0) { // Sunday - Prep Day with specific Sunday meals
                        dayElement.classList.add('prep-day');
                        
                        const currentWeekPlan = mealPlan[`week${mealWeek}`];
                        
                        const lunch = document.createElement('div');
                        lunch.className = 'meal lunch';
                        lunch.innerHTML = `🍽️ ${currentWeekPlan.sunday_meals.lunch}`;
                        lunch.onclick = () => showRecipe(currentWeekPlan.sunday_meals.lunch);
                        dayElement.appendChild(lunch);
                        
                        const dinner = document.createElement('div');
                        dinner.className = 'meal dinner';
                        dinner.innerHTML = `🥗 ${currentWeekPlan.sunday_meals.dinner}`;
                        dinner.onclick = () => showRecipe(currentWeekPlan.sunday_meals.dinner);
                        dayElement.appendChild(dinner);
                        
                        const prepIndicator = document.createElement('div');
                        prepIndicator.className = 'prep-indicator';
                        prepIndicator.textContent = '🍳 PREP DAY';
                        prepIndicator.onclick = () => showPrepDay(mealWeek);
                        dayElement.appendChild(prepIndicator);
                    } else if (dayOfWeek === 6) { // Saturday - Rest Day (no meals shown in this version)
                        dayElement.classList.add('rest-day');
                        const restIndicator = document.createElement('div');
                        restIndicator.className = 'rest-indicator';
                        restIndicator.textContent = '🧘‍♀️ REST DAY';
                        dayElement.appendChild(restIndicator);
                    } else { // Weekdays (Monday to Friday)
                        const currentWeekPlan = mealPlan[`week${mealWeek}`];
                        
                        // Determine which meal index to use for lunches and dinners for this day of the week
                        // Since Monday-Friday, we map 1 to 0, 2 to 1, ..., 5 to 4 for array indexing
                        const mealIndex = (dayOfWeek - 1) % currentWeekPlan.lunches.length;

                        // Add Lunch
                        const lunch = document.createElement('div');
                        lunch.className = 'meal lunch';
                        const lunchName = currentWeekPlan.lunches[mealIndex];
                        lunch.innerHTML = `🍽️ ${lunchName}`;
                        lunch.onclick = () => showRecipe(lunchName);
                        dayElement.appendChild(lunch);

                        // Add Dinner
                        const dinner = document.createElement('div');
                        dinner.className = 'meal dinner';
                        const dinnerName = currentWeekPlan.dinners[mealIndex];
                        dinner.innerHTML = `🥗 ${dinnerName}`;
                        dinner.onclick = () => showRecipe(dinnerName);
                        dayElement.appendChild(dinner);
                    }
                }
                calendar.appendChild(dayElement);
            }
        }

        // Initialize calendar on load
        document.addEventListener('DOMContentLoaded', generateCalendar);
    </script>
</body>
</html>
