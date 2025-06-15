<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamic Meal Prep Calendar</title>
    <style>
        /* Pastel Colors */
        :root {
            --pastel-green-light: #e6ffe6; /* Very light green */
            --pastel-green-medium: #c8f0c8; /* Medium light green */
            --pastel-green-dark: #8cde8c;   /* Slightly darker green for accents */
            --pastel-pink-light: #ffe6f0;   /* Very light pink */
            --pastel-pink-medium: #f0c8d8;  /* Medium light pink */
            --pastel-pink-dark: #de8cb2;    /* Slightly darker pink for accents */
            --text-color-dark: #333;
            --text-color-light: #fff;
            --background-light: #fdfdfd;
            --border-color: #eee;
        }

        /* General Body Styles */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--background-light);
            color: var(--text-color-dark);
            line-height: 1.6;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        /* Header Styles */
        h1 {
            color: var(--pastel-green-dark);
            text-align: center;
            margin-bottom: 30px;
            font-size: 2.5em;
            letter-spacing: 0.05em;
        }

        /* Tabs Navigation */
        .tabs {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            width: 100%;
            max-width: 1000px;
            border-bottom: 2px solid var(--border-color);
        }

        .tab-button {
            background-color: var(--background-light);
            color: var(--pastel-green-dark);
            border: none;
            padding: 15px 30px;
            text-align: center;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease, color 0.3s ease;
            border-top-left-radius: 8px;
            border-top-right-radius: 8px;
            margin: 0 5px;
            border-bottom: 2px solid transparent;
        }

        .tab-button.active {
            background-color: var(--pastel-green-light);
            color: var(--text-color-dark);
            border-bottom: 2px solid var(--pastel-green-dark);
            font-weight: bold;
        }

        .tab-button:hover:not(.active) {
            background-color: var(--pastel-green-light);
            color: var(--pastel-green-dark);
        }

        /* Content Area for Tabs */
        .tab-content {
            width: 100%;
            max-width: 1000px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            padding: 25px;
            margin-top: 10px;
        }

        /* Calendar Controls */
        .calendar-nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            max-width: 900px;
            margin-bottom: 20px;
            padding: 15px 25px;
            border-radius: 10px;
        }

        .calendar-nav button {
            background-color: var(--pastel-green-dark);
            color: var(--text-color-light);
            border: none;
            padding: 12px 20px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .calendar-nav button:hover {
            background-color: #72c372; /* Slightly darker green on hover */
            transform: translateY(-2px);
        }

        .calendar-nav button:active {
            transform: translateY(0);
        }

        #currentMonth {
            font-size: 1.8em;
            font-weight: bold;
            color: var(--pastel-green-dark);
        }

        /* Calendar Grid */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr); /* 7 columns for 7 days */
            gap: 15px; /* Space between days */
            width: 100%;
        }

        .day {
            background-color: var(--pastel-green-light); /* Light green for days */
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            min-height: 150px; /* Ensure days have enough height */
            transition: transform 0.2s ease;
            position: relative; /* For prep-indicator positioning */
            border: 1px solid var(--border-color);
        }

        .day:hover {
            transform: translateY(-5px);
        }

        .day-number {
            font-weight: bold;
            font-size: 1.1em;
            color: var(--text-color-dark);
            margin-bottom: 10px;
            border-bottom: 1px solid var(--pastel-green-medium);
            padding-bottom: 5px;
        }

        .prep-day {
            background-color: var(--pastel-pink-light); /* Light pink for prep days */
            border: 2px dashed var(--pastel-pink-dark);
        }

        .prep-indicator {
            position: absolute;
            top: 5px;
            right: 5px;
            background-color: var(--pastel-pink-dark);
            color: var(--text-color-light);
            padding: 3px 8px;
            border-radius: 5px;
            font-size: 0.75em;
            font-weight: bold;
            z-index: 10;
        }

        .meal {
            padding: 8px 10px;
            margin-bottom: 8px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.95em;
            transition: background-color 0.2s ease;
        }

        .meal:last-child {
            margin-bottom: 0;
        }

        .meal:hover {
            background-color: var(--pastel-green-medium);
        }

        .lunch {
            background-color: var(--pastel-green-light); /* Lighter green */
            border-left: 4px solid var(--pastel-green-dark);
        }

        .dinner {
            background-color: var(--pastel-pink-light); /* Lighter pink */
            border-left: 4px solid var(--pastel-pink-dark);
        }

        /* Meal Prep Tab Specific Styles */
        .meal-prep-content h2 {
            color: var(--pastel-green-dark);
            margin-bottom: 15px;
            font-size: 1.8em;
            border-bottom: 2px solid var(--pastel-green-medium);
            padding-bottom: 5px;
        }

        .meal-prep-content h3 {
            color: var(--pastel-pink-dark);
            margin-top: 25px;
            margin-bottom: 10px;
            font-size: 1.4em;
        }

        .grocery-list ul, .preparation-steps ol {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .grocery-list li, .preparation-steps li {
            background-color: var(--pastel-green-light);
            border-left: 5px solid var(--pastel-green-dark);
            margin-bottom: 8px;
            padding: 10px 15px;
            border-radius: 5px;
            font-size: 1.05em;
        }
        
        .preparation-steps li {
             background-color: var(--pastel-pink-light);
             border-left-color: var(--pastel-pink-dark);
        }

        /* Modal Styles (re-checked for pastel colors) */
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 100; /* Sit on top */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.6); /* Black w/ opacity */
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 30px;
            border-radius: 12px;
            width: 80%;
            max-width: 600px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.2);
            position: relative;
            animation-name: animatetop;
            animation-duration: 0.4s
        }

        @keyframes animatetop {
            from {top: -300px; opacity: 0}
            to {top: 0; opacity: 1}
        }

        .close-button {
            color: #aaa;
            float: right;
            font-size: 32px;
            font-weight: bold;
            position: absolute;
            top: 15px;
            right: 20px;
            cursor: pointer;
        }

        .close-button:hover,
        .close-button:focus {
            color: var(--pastel-pink-dark);
            text-decoration: none;
            cursor: pointer;
        }

        .modal-body h3 {
            color: var(--pastel-green-dark);
            margin-top: 20px;
            margin-bottom: 10px;
            font-size: 1.4em;
        }

        .ingredients-list, .instructions-list {
            list-style-type: none;
            padding: 0;
            margin-bottom: 20px;
        }

        .ingredients-list li, .instructions-list li {
            padding: 8px 0;
            border-bottom: 1px dashed var(--pastel-green-medium);
        }

        .instructions-list li {
            border-bottom: 1px dashed var(--pastel-pink-medium);
        }

        .ingredients-list li:last-child, .instructions-list li:last-child {
            border-bottom: none;
        }

        .modal-body ol li {
            margin-bottom: 10px;
        }
        
        /* Responsive Design */
        @media (max-width: 768px) {
            .calendar-grid {
                grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
                gap: 10px;
            }
            .day {
                min-height: 120px;
                padding: 10px;
            }
            .meal {
                font-size: 0.85em;
                padding: 6px 8px;
            }
            #currentMonth {
                font-size: 1.5em;
            }
            .calendar-nav {
                flex-direction: column;
                padding: 10px;
            }
            .calendar-nav button {
                width: 80%;
                margin: 5px 0;
            }
            .tabs {
                flex-direction: column;
                border-bottom: none;
            }
            .tab-button {
                width: 100%;
                margin: 2px 0;
                border-radius: 0;
                border-bottom: 1px solid var(--border-color);
            }
            .tab-button.active {
                border-bottom: none; /* Active tab doesn't need bottom border for column layout */
            }
            .tab-content {
                padding: 15px;
            }
        }

        @media (max-width: 480px) {
            h1 {
                font-size: 2em;
            }
            .day-number {
                font-size: 1em;
            }
            .day {
                min-height: 100px;
                padding: 8px;
            }
            .meal {
                font-size: 0.75em;
            }
            .modal-content {
                width: 95%;
                padding: 20px;
            }
            .close-button {
                font-size: 28px;
                top: 10px;
                right: 15px;
            }
        }
    </style>
</head>
<body>

    <h1>Meal Prep Calendar</h1>

    <div class="tabs">
        <button class="tab-button active" onclick="showTab('calendarTab')">Calendar</button>
        <button class="tab-button" onclick="showTab('mealPrepTab')">Meal Prep</button>
    </div>

    <div id="calendarTab" class="tab-content">
        <div class="calendar-nav">
            <button onclick="changeCycle(-1)">Previous Cycle</button>
            <span id="currentMonth">Meal Plan Cycle 1</span>
            <button onclick="changeCycle(1)">Next Cycle</button>
        </div>
        <div id="calendar" class="calendar-grid">
            </div>
    </div>

    <div id="mealPrepTab" class="tab-content" style="display: none;">
        <div class="meal-prep-content">
            <h2>Meal Preparation for This Cycle</h2>
            <div class="grocery-list">
                <h3>Grocery List:</h3>
                <ul id="groceryList">
                    </ul>
            </div>
            <div class="preparation-steps">
                <h3>Preparation Steps:</h3>
                <ol id="preparationSteps">
                    </ol>
            </div>
        </div>
    </div>

    <div id="recipeModal" class="modal">
        <div class="modal-content">
            <span class="close-button" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle"></h2>
            <div id="modalBody">
                </div>
        </div>
    </div>

    <script>
        // Global variables
        let currentCycleIndex = 0; // Represents the current "cycle" in a cycle of 4 weeks
        
        // Enhanced meal plan with specific meal types
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
            }
        };

        // Recipe database with ingredients and instructions
        const recipes = {
            "Kafta with Potatoes": {
                ingredients: ["500g ground lamb or beef", "4 medium potatoes", "1 large onion", "2 cloves garlic", "1/4 cup fresh parsley", "1 tsp allspice", "1 tsp cinnamon", "Salt", "Pepper", "2 tbsp olive oil"],
                instructions: ["Mix meat, onion, garlic, parsley, spices.", "Form patties.", "Brown patties in olive oil.", "Sauté remaining onions.", "Add potatoes.", "Return kafta, add water, simmer 25-30 min."]
            },
            "Lebanese Shawarma Bowl": {
                ingredients: ["500g chicken thighs", "2 cups basmati rice", "1 cucumber", "2 tomatoes", "1/2 red onion", "1/4 cup tahini", "2 tbsp lemon juice", "2 tsp shawarma spice blend", "Fresh parsley", "Fresh mint"],
                instructions: ["Marinate chicken.", "Cook rice.", "Pan-fry chicken.", "Prepare veggies.", "Make tahini sauce.", "Assemble bowls."]
            },
            "Caesar Salad": {
                ingredients: ["2 romaine lettuce heads", "1/2 cup parmesan", "1 cup croutons", "4 anchovy fillets (optional)", "2 cloves garlic", "1/4 cup lemon juice", "1/4 cup olive oil", "1 egg yolk", "1 tsp Dijon mustard"],
                instructions: ["Wash lettuce.", "Blend dressing ingredients.", "Toss lettuce with dressing.", "Top with parmesan and croutons."]
            },
            "Chickpea Salad": {
                ingredients: ["2 cans chickpeas", "1 cucumber", "2 tomatoes", "1/2 red onion", "1/4 cup fresh parsley", "3 tbsp olive oil", "2 tbsp lemon juice", "1 tsp sumac", "Salt", "Pepper"],
                instructions: ["Drain chickpeas.", "Dice veggies.", "Chop herbs.", "Whisk dressing.", "Combine all ingredients."]
            },
            "Chicken with Roasted Veggies": {
                ingredients: ["500g chicken breast", "2 bell peppers", "1 zucchini", "1 red onion", "2 tbsp olive oil", "1 tsp dried oregano", "Salt", "Pepper"],
                instructions: ["Preheat oven.", "Toss chicken and veggies with oil and spices.", "Roast 20-25 min."]
            },
            "Mediterranean Quinoa Salad": {
                ingredients: ["1 cup quinoa", "2 cups vegetable broth", "1 cucumber", "1 cup cherry tomatoes", "1/2 red onion", "1/2 cup Kalamata olives", "1/4 cup fresh parsley", "1/4 cup fresh mint", "1/4 cup feta cheese", "3 tbsp olive oil", "2 tbsp lemon juice", "Salt", "Pepper"],
                instructions: ["Cook quinoa, cool.", "Combine quinoa, veggies, herbs.", "Whisk dressing.", "Pour dressing, toss.", "Stir in feta."]
            },
            "Asian Chicken Salad": {
                ingredients: ["2 cups cooked chicken", "4 cups shredded cabbage", "1 cup shredded carrots", "1/2 red bell pepper", "1/4 cup green onions", "1/4 cup cilantro", "1/4 cup rice vinegar", "2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp honey", "1 tsp fresh ginger", "1/2 cup chopped peanuts (optional)"],
                instructions: ["Combine salad ingredients.", "Whisk dressing ingredients.", "Pour dressing, toss."]
            },
            "Beef Cubes with Potato": {
                ingredients: ["500g beef stew meat", "3 medium potatoes", "1 large onion", "2 carrots", "2 stalks celery", "4 cups beef broth", "2 tbsp olive oil", "1 tbsp tomato paste", "1 tsp dried thyme", "Salt", "Pepper"],
                instructions: ["Brown beef.", "Sauté veggies.", "Add tomato paste, thyme.", "Return beef, add broth, simmer 1.5-2 hrs.", "Add potatoes, cook 20-30 min."]
            },
            "Shrimp Orzo": {
                ingredients: ["400g raw shrimp", "1 cup orzo pasta", "2 tbsp olive oil", "1 onion", "2 cloves garlic", "1 can diced tomatoes", "1/2 cup white wine (optional)", "2 cups broth", "1/4 cup fresh parsley", "Salt", "Pepper"],
                instructions: ["Sauté onion, garlic.", "Add orzo, toast.", "Add wine, tomatoes, broth, simmer.", "Stir in shrimp, cook 3-5 min.", "Add parsley."]
            },
            "Korean Beef Bowl": {
                ingredients: ["500g beef sirloin", "2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp brown sugar", "2 cloves garlic", "1 tsp fresh ginger", "1/2 red bell pepper", "1 cup broccoli florets", "Cooked rice", "Sesame seeds", "Green onions"],
                instructions: ["Make marinade.", "Marinate beef.", "Stir-fry beef.", "Add veggies, stir-fry.", "Serve over rice."]
            },
            "Sweet and Sour Chicken": {
                ingredients: ["500g chicken breast", "1 bell pepper", "1 onion", "1 can pineapple chunks", "2 tbsp cornstarch", "1/4 cup white vinegar", "1/4 cup sugar", "2 tbsp soy sauce", "1 tbsp ketchup", "1/2 cup chicken broth", "2 tbsp oil"],
                instructions: ["Toss chicken with cornstarch.", "Stir-fry chicken.", "Sauté bell pepper, onion.", "Whisk sauce ingredients.", "Pour sauce, simmer.", "Return chicken, pineapple."]
            },
            "Tuna Salad": {
                ingredients: ["2 cans tuna", "1/2 cup mayonnaise or Greek yogurt", "1 stalk celery", "1/4 cup red onion", "1 tbsp fresh dill (optional)", "1 tbsp Dijon mustard", "Salt", "Black pepper"],
                instructions: ["Flake tuna.", "Combine with mayonnaise, celery, onion, dill, mustard.", "Mix well."]
            },
            "Mediterranean Orzo Salad": {
                ingredients: ["1 cup orzo pasta", "1 cup cherry tomatoes", "1 cucumber", "1/2 red onion", "1/2 cup Kalamata olives", "1/4 cup fresh parsley", "1/4 cup feta cheese", "3 tbsp olive oil", "2 tbsp red wine vinegar", "1 tsp dried oregano", "Salt", "Pepper"],
                instructions: ["Cook orzo, cool.", "Combine orzo, veggies, olives, parsley.", "Whisk dressing.", "Pour dressing, toss.", "Stir in feta."]
            },
            "Quinoa Halloumi Salad": {
                ingredients: ["1 cup quinoa", "2 cups water or vegetable broth", "250g halloumi cheese", "2 tbsp olive oil", "1 red bell pepper", "1 yellow bell pepper", "1/2 red onion", "1/4 cup fresh mint", "1/4 cup fresh parsley", "3 tbsp lemon juice", "Salt", "Pepper"],
                instructions: ["Cook quinoa, cool.", "Fry halloumi.", "Combine quinoa, peppers, onion, herbs.", "Whisk dressing.", "Pour dressing, toss.", "Fold in halloumi."]
            },
            "Bazella with Carrots": {
                ingredients: ["500g ground beef or lamb (optional)", "1 tbsp olive oil", "1 onion", "2 cloves garlic", "1 can crushed tomatoes", "4 cups frozen peas and carrots mix", "1 cup water or beef broth", "1 tsp seven spice", "Salt", "Pepper", "Cooked rice"],
                instructions: ["Brown meat (if used).", "Sauté onion, garlic.", "Stir in tomatoes, peas/carrots, broth, spice.", "Return meat (if used), simmer 20-25 min."]
            },
            "Chicken Fajita Bowl": {
                ingredients: ["500g chicken breast", "2 bell peppers", "1 onion", "2 tbsp olive oil", "2 tsp fajita seasoning", "1 cup cooked rice", "1/2 cup black beans", "1/4 cup corn kernels", "Salsa", "Avocado slices", "Lime wedges"],
                instructions: ["Stir-fry chicken.", "Add peppers, onion, sauté.", "Stir in seasoning.", "Serve over rice with toppings."]
            },
            "Meatballs with Marinara": {
                ingredients: ["500g ground beef or turkey", "1/2 cup breadcrumbs", "1 egg", "1/4 cup milk", "2 tbsp fresh parsley", "1 clove garlic", "1/2 tsp salt", "1/4 tsp black pepper", "1 tbsp olive oil", "1 jar marinara sauce", "Cooked pasta or zucchini noodles"],
                instructions: ["Combine meatball ingredients, form meatballs.", "Brown meatballs.", "Pour marinara, simmer 15-20 min.", "Serve over pasta."]
            },
            "Chicken Salad": {
                ingredients: ["2 cups cooked chicken", "1/2 cup mayonnaise", "1/4 cup celery", "2 tbsp red onion", "1 tbsp fresh dill or parsley (optional)", "1 tsp Dijon mustard", "Salt", "Black pepper"],
                instructions: ["Combine chicken, mayonnaise, celery, onion.", "Stir in dill, mustard.", "Mix well."]
            },
            "Chicken Curry": {
                ingredients: ["500g chicken breast", "1 tbsp olive oil", "1 onion", "2 cloves garlic", "1 tbsp fresh ginger", "2 tbsp curry powder", "1 can coconut milk", "1 cup chicken broth", "1/2 cup frozen peas", "Cooked basmati rice"],
                instructions: ["Brown chicken.", "Sauté onion, garlic, ginger, curry powder.", "Return chicken, add coconut milk, broth, simmer.", "Stir in peas."]
            },
            "Pepper and Cucumber Salad": {
                ingredients: ["2 cucumbers", "2 bell peppers", "1/2 red onion", "1/4 cup fresh dill", "3 tbsp olive oil", "2 tbsp white wine vinegar", "1 tsp sugar (optional)", "Salt", "Black pepper"],
                instructions: ["Dice veggies.", "Slice onion, chop dill.", "Combine veggies.", "Whisk dressing.", "Pour dressing, toss."]
            },
            "Mexican Burrito Bowl": {
                ingredients: ["1 cup cooked rice", "1 cup cooked black beans", "1 cup cooked corn kernels", "1/2 cup salsa", "1/4 cup fresh cilantro", "1/2 avocado", "2 tbsp sour cream or Greek yogurt (optional)", "Lime wedges", "Optional: cooked seasoned protein"],
                instructions: ["Layer rice, beans, corn.", "Top with salsa, cilantro, avocado.", "Add protein (optional), sour cream (optional)."]
            },
            "Burghul Banadoura": {
                ingredients: ["1 cup coarse bulgur", "2 tbsp olive oil", "1 large onion", "2 cloves garlic", "1 can crushed tomatoes", "2 cups water or vegetable broth", "1 tsp dried mint", "Salt", "Black pepper"],
                instructions: ["Rinse bulgur.", "Sauté onion, garlic.", "Stir in tomatoes, water/broth, dried mint, seasonings.", "Add bulgur, simmer 20-25 min."]
            },
            "Teriyaki Chicken": {
                ingredients: ["500g chicken breast or thighs", "1 tbsp olive oil", "1/2 cup teriyaki sauce", "1 tbsp honey or brown sugar (optional)", "1 cup broccoli florets", "1 bell pepper", "Cooked rice", "Sesame seeds", "Green onions"],
                instructions: ["Stir-fry chicken.", "Add broccoli, bell pepper.", "Pour teriyaki sauce, simmer.", "Serve over rice."]
            }
        };

        // Utility function to shuffle an array
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Utility function to get a random item from an array
        function getRandomMeal(mealList) {
            return mealList[Math.floor(Math.random() * mealList.length)];
        }

        // Function to distribute meals for Monday-Saturday based on your specific pattern
        function distributeMeals(mealOptions, type) {
            let assignedMeals = [];
            let shuffledOptions = [...mealOptions]; // Create a copy to shuffle
            shuffleArray(shuffledOptions); // Randomize the order of the available meals

            if (type === 'lunches') {
                // L1, L1, L2, L2, L3, L3
                for (let i = 0; i < 3; i++) {
                    const meal = shuffledOptions[i % shuffledOptions.length]; // Cycle through if less than 3 unique
                    assignedMeals.push(meal, meal);
                }
            } else if (type === 'dinners') {
                // D1, D2, D3, D1, D2, D3
                for (let i = 0; i < 6; i++) {
                    assignedMeals.push(shuffledOptions[i % shuffledOptions.length]);
                }
            }
            return assignedMeals;
        }

        // Function to generate the calendar display
        function generateCalendar() {
            const calendarEl = document.getElementById('calendar');
            calendarEl.innerHTML = ''; // Clear previous calendar content

            // Update the current cycle display
            document.getElementById('currentMonth').textContent = `Meal Plan Cycle ${currentCycleIndex + 1}`;

            const weekLength = 7; // Days in a week

            for (let weekDisplay = 0; weekDisplay < 4; weekDisplay++) { // Loop for 4 weeks to display
                // Calculate which mealPlan week corresponds to the current 'weekDisplay'
                const actualMealPlanWeekIndex = (currentCycleIndex + weekDisplay) % 4;
                const actualMealPlanWeekKey = `week${actualMealPlanWeekIndex + 1}`;
                const mealsForThisDisplayedWeek = mealPlan[actualMealPlanWeekKey];

                // Determine the *next* actual meal plan week for Sunday's meals (prep for the *next* week)
                const nextActualMealPlanWeekIndex = (actualMealPlanWeekIndex + 1) % 4;
                const nextActualMealPlanWeekKey = `week${nextActualMealPlanWeekIndex + 1}`;
                const mealsForNextDisplayedWeek = mealPlan[nextActualMealPlanWeekKey];

                // Distribute lunches and dinners for Monday-Saturday based on patterns
                const distributedLunchesMonSat = distributeMeals(mealsForThisDisplayedWeek.lunches, 'lunches');
                const distributedDinnersMonSat = distributeMeals(mealsForThisDisplayedWeek.dinners, 'dinners');

                // Get random meals for Sunday prep from the *next* week's available meals
                const sundayPrepLunch = getRandomMeal(mealsForNextDisplayedWeek.lunches);
                const sundayPrepDinner = getRandomMeal(mealsForNextDisplayedWeek.dinners);

                for (let dayOfWeek = 0; dayOfWeek < weekLength; dayOfWeek++) { // Loop for 7 days (Sun-Sat)
                    const dayEl = document.createElement('div');
                    dayEl.classList.add('day');

                    const dayNameEl = document.createElement('div');
                    dayNameEl.classList.add('day-number');
                    const dayNames = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
                    
                    if (dayOfWeek === 0) { // Sunday (Prep Day)
                        dayNameEl.textContent = `Week ${weekDisplay + 1} ${dayNames[dayOfWeek]}`; // Week number + Day Name
                        dayEl.classList.add('prep-day');
                        const prepIndicator = document.createElement('div');
                        prepIndicator.classList.add('prep-indicator');
                        prepIndicator.textContent = 'Prep Day';
                        dayEl.appendChild(prepIndicator);

                        // Sunday meals are for PREP for the *next* week's consumption
                        if (sundayPrepLunch) {
                            const lunchEl = document.createElement('div');
                            lunchEl.classList.add('meal', 'lunch');
                            lunchEl.textContent = `Prep Lunch: ${sundayPrepLunch}`;
                            lunchEl.onclick = () => openModal(sundayPrepLunch);
                            dayEl.appendChild(lunchEl);
                        }

                        if (sundayPrepDinner) {
                            const dinnerEl = document.createElement('div');
                            dinnerEl.classList.add('meal', 'dinner');
                            dinnerEl.textContent = `Prep Dinner: ${sundayPrepDinner}`;
                            dinnerEl.onclick = () => openModal(sundayPrepDinner);
                            dayEl.appendChild(dinnerEl);
                        }

                    } else { // Monday to Saturday (Consumption Days)
                        dayNameEl.textContent = dayNames[dayOfWeek];
                        const dayIndexForMeals = dayOfWeek - 1; // 0 for Monday, 1 for Tuesday... 5 for Saturday

                        const lunch = distributedLunchesMonSat[dayIndexForMeals % distributedLunchesMonSat.length];
                        const dinner = distributedDinnersMonSat[dayIndexForMeals % distributedDinnersMonSat.length];

                        if (lunch) {
                            const lunchEl = document.createElement('div');
                            lunchEl.classList.add('meal', 'lunch');
                            lunchEl.textContent = `Lunch: ${lunch}`;
                            lunchEl.onclick = () => openModal(lunch);
                            dayEl.appendChild(lunchEl);
                        }

                        if (dinner) {
                            const dinnerEl = document.createElement('div');
                            dinnerEl.classList.add('meal', 'dinner');
                            dinnerEl.textContent = `Dinner: ${dinner}`;
                            dinnerEl.onclick = () => openModal(dinner);
                            dayEl.appendChild(dinnerEl);
                        }
                    }
                    dayEl.prepend(dayNameEl); // Ensure day name is at the top
                    calendarEl.appendChild(dayEl);
                }
            }
            // After generating calendar, generate meal prep list for the *current* cycle's first week (which is the actual meal plan week)
            generateMealPrepList();
        }

        // Function to generate the grocery list and preparation steps
        function generateMealPrepList() {
            const groceryListEl = document.getElementById('groceryList');
            const preparationStepsEl = document.getElementById('preparationSteps');
            groceryListEl.innerHTML = '';
            preparationStepsEl.innerHTML = '';

            // Get the current week for which prep is being done
            const actualMealPlanWeekKey = `week${(currentCycleIndex % 4) + 1}`;
            const currentWeekData = mealPlan[actualMealPlanWeekKey];

            // Determine the *next* actual meal plan week for Sunday's prep meals
            const nextActualMealPlanWeekIndex = (currentCycleIndex + 1) % 4;
            const nextActualMealPlanWeekKey = `week${nextActualMealPlanWeekIndex + 1}`;
            const nextWeekData = mealPlan[nextActualMealPlanWeekKey];

            // Collect all meals that will be consumed in the CURRENT displayed cycle (Mon-Sat)
            const mealsToCook = [
                ...distributeMeals(currentWeekData.lunches, 'lunches'),
                ...distributeMeals(currentWeekData.dinners, 'dinners')
            ];

            // Collect the meals that will be PREPPED on the Sunday of the CURRENT displayed cycle
            // These are randomly selected from the *next* week's pool
            const sundayPrepLunch = getRandomMeal(nextWeekData.lunches);
            const sundayPrepDinner = getRandomMeal(nextWeekData.dinners);
            
            // Add Sunday prep meals to the list of meals to cook for ingredient gathering
            if(sundayPrepLunch) mealsToCook.push(sundayPrepLunch);
            if(sundayPrepDinner) mealsToCook.push(sundayPrepDinner);

            const ingredientsCount = {};
            const prepRecipes = new Set(); // To store unique recipe names for prep steps

            // Aggregate ingredients from all meals
            mealsToCook.forEach(mealName => {
                const recipe = recipes[mealName];
                if (recipe) {
                    recipe.ingredients.forEach(ingredient => {
                        ingredientsCount[ingredient] = (ingredientsCount[ingredient] || 0) + 1;
                    });
                }
            });

            // Add ingredients for Sunday prep meals to the list of meals to cook for ingredient gathering
            // This is crucial: the actual prep being done on Sunday is for *these* specific meals.
            if(sundayPrepLunch) {
                const recipe = recipes[sundayPrepLunch];
                if(recipe) {
                    prepRecipes.add(sundayPrepLunch);
                }
            }
            if(sundayPrepDinner) {
                const recipe = recipes[sundayPrepDinner];
                if(recipe) {
                    prepRecipes.add(sundayPrepDinner);
                }
            }


            // Populate Grocery List
            for (const ingredient in ingredientsCount) {
                const li = document.createElement('li');
                // For simplicity, we just list the ingredient. If quantities need aggregation,
                // the `recipes` object would need more structured quantity data.
                li.textContent = `${ingredient}`; // (x${ingredientsCount[ingredient]}) if you want counts
                groceryListEl.appendChild(li);
            }

            // Populate Preparation Steps (only for the meals prepped on Sunday)
            if (prepRecipes.size === 0) {
                const li = document.createElement('li');
                li.textContent = "No specific prep steps found for Sunday's meals.";
                preparationStepsEl.appendChild(li);
            } else {
                prepRecipes.forEach(recipeName => {
                    const recipe = recipes[recipeName];
                    if (recipe && recipe.instructions && recipe.instructions.length > 0) {
                        const liTitle = document.createElement('li');
                        liTitle.innerHTML = `<strong>${recipeName}:</strong>`;
                        preparationStepsEl.appendChild(liTitle);
                        recipe.instructions.forEach(instruction => {
                            const li = document.createElement('li');
                            li.textContent = instruction;
                            preparationStepsEl.appendChild(li);
                        });
                    }
                });
            }
        }


        // Function to change the current meal plan cycle
        function changeCycle(direction) {
            currentCycleIndex = (currentCycleIndex + direction + 4) % 4; // Ensure positive modulo result (0-3)
            generateCalendar(); // Regenerate calendar with new cycle
            generateMealPrepList(); // Regenerate grocery/prep list for the new cycle
        }

        // Function to show/hide tabs
        function showTab(tabId) {
            // Hide all tab contents
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.style.display = 'none';
            });
            // Deactivate all tab buttons
            document.querySelectorAll('.tab-button').forEach(button => {
                button.classList.remove('active');
            });

            // Show the selected tab content
            document.getElementById(tabId).style.display = 'block';

            // Activate the clicked button
            document.querySelector(`.tab-button[onclick="showTab('${tabId}')"]`).classList.add('active');
        }

        // Modal functionality
        function openModal(recipeName) {
            const modal = document.getElementById('recipeModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            const recipe = recipes[recipeName];

            if (recipe) {
                modalTitle.textContent = recipeName;
                modalBody.innerHTML = `
                    <div class="recipe-section">
                        <h3>Ingredients:</h3>
                        <ul class="ingredients-list">
                            ${recipe.ingredients.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                    <div class="recipe-section">
                        <h3>Instructions:</h3>
                        <ol class="instructions-list">
                            ${recipe.instructions.map(step => `<li>${step}</li>`).join('')}
                        </ol>
                    </div>
                `;
                modal.style.display = 'flex'; 
            } else {
                modalTitle.textContent = "Recipe Not Found";
                modalBody.innerHTML = "<p>Sorry, the recipe for this meal is not available.</p>";
                modal.style.display = 'flex';
            }
        }

        function closeModal() {
            const modal = document.getElementById('recipeModal');
            modal.style.display = 'none';
        }

        // Close modal when clicking outside of it
        window.onclick = function(event) {
            const modal = document.getElementById('recipeModal');
            if (event.target == modal) {
                modal.style.display = "none";
            }
        }

        // Initial generation on page load
        document.addEventListener('DOMContentLoaded', () => {
            generateCalendar();
            showTab('calendarTab'); // Show calendar tab by default
        });
    </script>
</body>
</html>
