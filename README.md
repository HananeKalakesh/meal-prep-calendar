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
            --pastel-green-dark: #28a745;   /* Darker green for accents */
            --pastel-pink-light: #ffe6f0;   /* Very light pink */
            --pastel-pink-medium: #f0c8d8;  /* Medium light pink */
            --pastel-pink-dark: #e83e8c;    /* Darker pink for accents */
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

        /* Main Content Wrapper */
        .main-container {
            width: 100%;
            max-width: 1200px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            padding: 25px;
            margin-bottom: 30px;
        }

        /* Global Action Buttons (Grocery/Preparation) */
        .global-actions {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px; /* Space between buttons and first week */
        }

        .global-action-button {
            background-color: var(--pastel-pink-dark); /* Using pastel pink for global actions */
            color: var(--text-color-light);
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: bold;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.15);
        }

        .global-action-button:hover {
            background-color: #c42f73; /* Slightly darker pink on hover */
            transform: translateY(-2px);
        }

        /* Calendar Section for Each Week */
        .calendar-section {
            margin-bottom: 40px;
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 20px;
            background-color: #f9f9f9;
        }

        .week-title {
            font-size: 1.8em;
            font-weight: bold;
            color: var(--pastel-green-dark);
            margin-bottom: 20px;
            text-align: center;
            border-bottom: 2px solid var(--pastel-green-medium);
            padding-bottom: 10px;
        }

        /* Calendar Grid */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr); /* 7 columns for Sun-Sat */
            gap: 15px; /* Space between days */
            width: 100%;
        }

        .day-header {
            font-weight: bold;
            font-size: 1.1em;
            color: var(--pastel-green-dark);
            text-align: center;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--pastel-green-dark);
            margin-bottom: 10px;
        }

        .day {
            background-color: var(--pastel-green-light); /* Light green for consumption days */
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            min-height: 180px; /* Ensure days have enough height */
            transition: transform 0.2s ease;
            position: relative; /* For prep-indicator positioning */
            border: 1px solid var(--border-color);
        }

        .day.empty-day {
            background-color: #f0f0f0; /* Gray for empty Saturday */
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.05);
            color: #888;
            text-align: center;
            justify-content: center;
            align-items: center;
            font-style: italic;
        }

        .day:hover:not(.empty-day) {
            transform: translateY(-5px);
        }

        .day-name {
            font-weight: bold;
            font-size: 1.1em;
            color: var(--text-color-dark);
            margin-bottom: 10px;
            border-bottom: 1px solid var(--pastel-green-medium);
            padding-bottom: 5px;
        }

        .prep-day {
            background-color: var(--pastel-pink-light); /* Light pink for Sundays (prep days) */
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

        /* Specific style for Prep Meals on Sunday to distinguish them */
        .meal.prep-meal {
            background-color: #f5f5f5; /* Lighter grey for prep meals to visually separate */
            border-left: 4px solid #888; /* Grey border */
            font-style: italic;
            font-size: 0.88em;
            color: #555;
            margin-top: 10px; /* Add some space from consumption meals */
        }
        .meal.prep-meal:hover {
            background-color: #e0e0e0;
        }

        /* Modal Styles (for Recipe, Grocery, and Preparations) */
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
            max-width: 700px; /* Slightly wider modal for lists */
            max-height: 90vh; /* Max height to allow scrolling */
            overflow-y: auto; /* Enable vertical scrolling */
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

        /* Styles for lists inside modals (ingredients, instructions, grocery, prep) */
        .ingredients-list, .instructions-list, .macros-list, .modal-list {
            list-style-type: none;
            padding: 0;
            margin-bottom: 20px;
        }

        .ingredients-list li, .instructions-list li, .macros-list li, .modal-list li {
            padding: 8px 0;
            border-bottom: 1px dashed var(--pastel-green-medium);
            font-size: 0.95em;
        }

        .instructions-list li {
            border-bottom: 1px dashed var(--pastel-pink-medium);
        }

        .ingredients-list li:last-child, .instructions-list li:last-child, .macros-list li:last-child, .modal-list li:last-child {
            border-bottom: none;
        }

        .modal-body ol li {
            margin-bottom: 10px;
        }

        /* Grocery/Preparation Modal specific list styles */
        .modal-list .category-header {
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
            font-weight: bold;
            margin-top: 15px;
            margin-bottom: 8px;
            padding: 10px 12px;
            font-size: 1.1em;
        }
        .modal-list ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .modal-list ul li {
            background-color: var(--pastel-pink-light);
            border-left: 4px solid var(--pastel-pink-dark);
            margin-bottom: 6px;
            padding: 8px 12px;
            border-radius: 4px;
        }
        .modal-list ol li { /* For preparation steps that are numbered */
            margin-bottom: 10px;
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
            padding: 8px 12px;
            border-radius: 4px;
        }


        /* Responsive Design */
        @media (max-width: 992px) {
            .calendar-grid {
                grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            }
        }
        @media (max-width: 768px) {
            .calendar-grid {
                grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
                gap: 10px;
            }
            .day {
                min-height: 100px;
                padding: 10px;
            }
            .meal {
                font-size: 0.85em;
                padding: 6px 8px;
            }
            h1 {
                font-size: 2em;
            }
            .week-title {
                font-size: 1.5em;
            }
            .global-actions {
                flex-direction: column;
                gap: 10px;
            }
            .global-action-button {
                width: 80%;
                margin: 0 auto;
            }
            .modal-content {
                width: 90%;
            }
        }

        @media (max-width: 480px) {
            .day-name {
                font-size: 0.9em;
            }
            .day {
                min-height: 80px;
                padding: 8px;
            }
            .meal {
                font-size: 0.7em;
            }
            .modal-content {
                width: 95%;
                padding: 15px;
            }
            .close-button {
                font-size: 24px;
                top: 10px;
                right: 10px;
            }
        }
    </style>
</head>
<body>

    <h1>Meal Prep Calendar</h1>

    <div class="main-container">
        <div class="global-actions">
            <button class="global-action-button" onclick="openGroceryListModal()">View Full Grocery List</button>
            <button class="global-action-button" onclick="openPreparationStepsModal()">View Full Preparations</button>
        </div>

        <div id="calendarDisplay">
            </div>
    </div>

    <div id="universalModal" class="modal">
        <div class="modal-content">
            <span class="close-button" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle"></h2>
            <div id="modalBody">
                </div>
        </div>
    </div>

    <script>
        // Global variables
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

        // Recipe database with ingredients (categorized) and instructions, and MACROS
        const recipes = {
            "Kafta with Potatoes": {
                ingredients: {
                    proteins: ["500g ground lamb or beef"],
                    vegetables: ["4 medium potatoes, cubed", "1 large onion, finely chopped", "2 cloves garlic, minced", "1/4 cup fresh parsley, chopped"],
                    pantry: ["1 tsp allspice", "1 tsp cinnamon", "Salt and pepper to taste", "2 tbsp olive oil"]
                },
                instructions: ["Mix ground meat with half the onion, garlic, parsley, and spices.", "Form into small oval-shaped patties.", "Heat olive oil in a large pan over medium heat.", "Brown kafta patties on both sides, then remove.", "In same pan, sauté remaining onions until golden.", "Add potatoes and cook for 5 minutes.", "Return kafta to pan, add water to barely cover.", "Simmer covered for 25-30 minutes until potatoes are tender."],
                macros: { calories: "350 kcal", protein: "30g", carbs: "40g", fat: "10g" }
            },
            "Lebanese Shawarma Bowl": {
                ingredients: {
                    proteins: ["500g chicken thighs, sliced thin"],
                    carbs: ["2 cups basmati rice"],
                    vegetables: ["1 cucumber, diced", "2 tomatoes, diced", "1/2 red onion, sliced", "Fresh parsley and mint"],
                    pantry: ["1/4 cup tahini", "2 tbsp lemon juice", "2 tsp shawarma spice blend"]
                },
                instructions: ["Marinate chicken in shawarma spices and lemon juice for 2 hours.", "Cook rice according to package instructions.", "Pan-fry chicken until golden and cooked through.", "Prepare vegetables and arrange in bowls.", "Mix tahini with lemon juice and water for sauce.", "Assemble bowls with rice, chicken, vegetables, and sauce."],
                macros: { calories: "450 kcal", protein: "40g", carbs: "50g", fat: "15g" }
            },
            "Caesar Salad": {
                ingredients: {
                    proteins: ["4 anchovy fillets (optional)", "1 egg yolk"],
                    vegetables: ["2 large romaine lettuce heads", "2 cloves garlic"],
                    pantry: ["1/2 cup parmesan cheese, grated", "1 cup croutons", "1/4 cup lemon juice", "1/4 cup olive oil", "1 tsp Dijon mustard"]
                },
                instructions: ["Wash and chop romaine lettuce.", "Make dressing: blend garlic, anchovies, lemon juice, egg yolk, and mustard.", "Slowly add olive oil while blending.", "Toss lettuce with dressing.", "Top with parmesan and croutons.", "Serve immediately."],
                macros: { calories: "280 kcal", protein: "15g", carbs: "20g", fat: "18g" }
            },
            "Chickpea Salad": {
                ingredients: {
                    proteins: [],
                    carbs: ["2 cans chickpeas, drained and rinsed"],
                    vegetables: ["1 cucumber, diced", "2 tomatoes, diced", "1/2 red onion, finely chopped", "1/4 cup fresh parsley"],
                    pantry: ["3 tbsp olive oil", "2 tbsp lemon juice", "1 tsp sumac", "Salt and pepper to taste"]
                },
                instructions: ["Drain and rinse chickpeas thoroughly.", "Dice all vegetables finely.", "Chop parsley and mint.", "Whisk together olive oil, lemon juice, and sumac.", "Combine all ingredients in a large bowl.", "Toss well and let marinate for 30 minutes.", "Adjust seasoning before serving."],
                macros: { calories: "250 kcal", protein: "10g", carbs: "35g", fat: "10g" }
            },
            "Chicken with Roasted Veggies": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["2 bell peppers, chopped", "1 zucchini, chopped", "1 red onion, chopped"],
                    pantry: ["2 tbsp olive oil", "1 tsp dried oregano", "Salt and pepper to taste"]
                },
                instructions: ["Preheat oven to 200°C (400°F).", "Toss chicken and vegetables with olive oil, oregano, salt, and pepper.", "Spread on a baking sheet in a single layer.", "Roast for 20-25 minutes, or until chicken is cooked through and vegetables are tender-crisp.", "Serve hot."],
                macros: { calories: "320 kcal", protein: "35g", carbs: "20g", fat: "12g" }
            },
            "Mediterranean Quinoa Salad": {
                ingredients: {
                    proteins: ["1/4 cup feta cheese, crumbled"],
                    carbs: ["1 cup quinoa, rinsed"],
                    vegetables: ["1 cucumber, diced", "1 cup cherry tomatoes, halved", "1/2 red onion, finely chopped", "1/2 cup Kalamata olives, pitted and halved", "1/4 cup fresh parsley, chopped", "1/4 cup fresh mint, chopped"],
                    pantry: ["2 cups vegetable broth or water", "3 tbsp olive oil", "2 tbsp lemon juice", "Salt and pepper to taste"]
                },
                instructions: ["Cook quinoa according to package directions using broth/water.", "Let cool completely.", "In a large bowl, combine cooked quinoa, cucumber, cherry tomatoes, red onion, olives, parsley, and mint.", "Whisk together olive oil, lemon juice, salt, and pepper.", "Pour dressing over salad and toss to combine.", "Stir in crumbled feta cheese just before serving."],
                macros: { calories: "380 kcal", protein: "12g", carbs: "45g", fat: "18g" }
            },
            "Asian Chicken Salad": {
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded"],
                    vegetables: ["4 cups shredded cabbage (green or napa)", "1 cup shredded carrots", "1/2 red bell pepper, thinly sliced", "1/4 cup chopped green onions", "1/4 cup chopped cilantro"],
                    pantry: ["1/4 cup rice vinegar", "2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp honey or maple syrup", "1 tsp grated fresh ginger", "1/2 cup chopped peanuts or cashews (optional)"]
                },
                instructions: ["In a large bowl, combine shredded chicken, cabbage, carrots, red bell pepper, green onions, and cilantro.", "In a small bowl, whisk together rice vinegar, soy sauce, sesame oil, honey, and ginger until well combined.", "Pour dressing over the salad ingredients and toss to coat evenly.", "If desired, add chopped peanuts or cashews for crunch.", "Serve immediately or chill for later."],
                macros: { calories: "300 kcal", protein: "25g", carbs: "25g", fat: "12g" }
            },
            "Shrimp Orzo": {
                ingredients: {
                    proteins: ["400g raw shrimp, peeled and deveined"],
                    carbs: ["1 cup orzo pasta"],
                    vegetables: ["1 onion, finely chopped", "2 cloves garlic, minced", "1/4 cup fresh parsley, chopped"],
                    pantry: ["2 tbsp olive oil", "1 can (400g) diced tomatoes", "1/2 cup white wine (optional)", "2 cups vegetable or chicken broth", "Salt and pepper to taste"]
                },
                instructions: ["Heat olive oil in a large skillet over medium heat.", "Add onion and cook until softened, about 5 minutes.", "Add garlic and cook for 1 minute more until fragrant.", "Stir in orzo and toast for 2 minutes.", "Pour in white wine (if using) and cook until absorbed.", "Add diced tomatoes and broth, bring to a simmer.", "Cover and cook for 10-12 minutes, stirring occasionally, until orzo is almost cooked.", "Stir in shrimp and cook for 3-5 minutes until pink and cooked through.", "Season with salt and pepper, stir in fresh parsley before serving."],
                macros: { calories: "380 kcal", protein: "25g", carbs: "45g", fat: "10g" }
            },
            "Korean Beef Bowl": {
                ingredients: {
                    proteins: ["500g beef sirloin or flank steak, thinly sliced"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1/2 red bell pepper, sliced", "1 cup broccoli florets", "Sliced green onions for garnish"],
                    pantry: ["2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp brown sugar", "2 cloves garlic, minced", "1 tsp grated fresh ginger", "Sesame seeds for garnish"]
                },
                instructions: ["In a bowl, combine soy sauce, sesame oil, brown sugar, garlic, and ginger to make the marinade.", "Add sliced beef to the marinade and toss to coat; let marinate for at least 30 minutes.", "Heat a large skillet or wok over medium-high heat.", "Add marinated beef and stir-fry until cooked through and slightly caramelized.", "Add red bell pepper and broccoli, stir-fry for 3-5 minutes until tender-crisp.", "Serve hot over cooked rice, garnished with sesame seeds and green onions."],
                macros: { calories: "420 kcal", protein: "38g", carbs: "40g", fat: "15g" }
            },
            "Sweet and Sour Chicken": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 bell pepper (any color), chopped", "1 onion, chopped"],
                    pantry: ["1 can (227g) pineapple chunks, drained (reserve juice)", "2 tbsp cornstarch", "1/4 cup white vinegar", "1/4 cup sugar", "2 tbsp soy sauce", "1 tbsp ketchup", "1/2 cup chicken broth", "2 tbsp oil for cooking"]
                },
                instructions: ["Toss chicken cubes with 1 tbsp cornstarch.", "Heat oil in a large skillet or wok over medium-high heat.", "Stir-fry chicken until lightly browned and cooked through, remove from pan.", "In the same pan, add bell pepper and onion, stir-fry for 3-4 minutes until slightly softened.", "In a small bowl, whisk together remaining cornstarch with vinegar, sugar, soy sauce, ketchup, pineapple juice, and chicken broth.", "Pour sauce into the skillet, bring to a simmer, stirring until thickened.", "Return chicken to the pan along with pineapple chunks.", "Toss to coat everything in the sauce.", "Serve hot with rice or noodles."],
                macros: { calories: "360 kcal", protein: "30g", carbs: "40g", fat: "10g" }
            },
            "Tuna Salad": {
                ingredients: {
                    proteins: ["2 cans (140g each) tuna in water, drained"],
                    vegetables: ["1 stalk celery, finely chopped", "1/4 cup red onion, finely chopped", "1 tbsp fresh dill, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise or Greek yogurt", "1 tbsp Dijon mustard", "Salt and black pepper to taste"]
                },
                instructions: ["In a medium bowl, flake the drained tuna with a fork.", "Add mayonnaise (or Greek yogurt), chopped celery, red onion, dill (if using), and Dijon mustard.", "Mix well to combine all ingredients.", "Season with salt and black pepper to taste.", "Serve as a sandwich filling, with crackers, or on a bed of lettuce."],
                macros: { calories: "220 kcal", protein: "20g", carbs: "5g", fat: "15g" }
            },
            "Mediterranean Orzo Salad": {
                ingredients: {
                    proteins: ["1/4 cup feta cheese, crumbled"],
                    carbs: ["1 cup orzo pasta, cooked and cooled"],
                    vegetables: ["1 cup cherry tomatoes, halved", "1 cucumber, diced", "1/2 red onion, thinly sliced", "1/2 cup Kalamata olives, pitted and halved", "1/4 cup fresh parsley, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp red wine vinegar", "1 tsp dried oregano", "Salt and pepper to taste"]
                },
                instructions: ["Cook orzo pasta according to package directions; drain and rinse with cold water to cool, then set aside.", "In a large bowl, combine the cooled orzo, cherry tomatoes, cucumber, red onion, olives, and parsley.", "In a small bowl, whisk together olive oil, red wine vinegar, oregano, salt, and pepper to make the dressing.", "Pour the dressing over the orzo mixture and toss gently to coat all ingredients.", "Stir in the crumbled feta cheese.", "Serve immediately or chill for flavors to meld."],
                macros: { calories: "350 kcal", protein: "10g", carbs: "40g", fat: "16g" }
            },
            "Quinoa Halloumi Salad": {
                ingredients: {
                    proteins: ["250g halloumi cheese, sliced or cubed"],
                    carbs: ["1 cup quinoa, rinsed"],
                    vegetables: ["1 red bell pepper, chopped", "1 yellow bell pepper, chopped", "1/2 red onion, thinly sliced", "1/4 cup fresh mint, chopped", "1/4 cup fresh parsley, chopped"],
                    pantry: ["2 cups water or vegetable broth", "2 tbsp olive oil", "3 tbsp lemon juice", "Salt and pepper to taste"]
                },
                instructions: ["Cook quinoa according to package directions with water or broth; let cool.", "Heat 1 tbsp olive oil in a non-stick skillet over medium heat.", "Add halloumi cheese and cook until golden brown on both sides, about 2-3 minutes per side; remove and set aside.", "In a large bowl, combine cooled quinoa, roasted bell peppers, red onion, mint, and parsley.", "Whisk together remaining olive oil, lemon juice, salt, and pepper for the dressing.", "Pour dressing over the salad and toss well.", "Gently fold in the cooked halloumi cheese.", "Serve warm or at room temperature."],
                macros: { calories: "400 kcal", protein: "20g", carbs: "35g", fat: "20g" }
            },
            "Bazella with Carrots": {
                ingredients: {
                    proteins: ["500g ground beef or lamb (optional)"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "4 cups frozen peas and carrots mix"],
                    pantry: ["1 tbsp olive oil", "1 can (400g) crushed tomatoes", "1 cup water or beef broth", "1 tsp seven spice (baharat)", "Salt and pepper to taste", "Cooked rice for serving"]
                },
                instructions: ["If using meat, brown ground beef/lamb in olive oil in a large pot; drain excess fat and set aside.", "In the same pot, sauté onion until softened, then add garlic and cook for 1 minute more.", "Stir in crushed tomatoes, peas and carrots, water/broth, and seven spice.", "If using meat, return it to the pot.", "Bring to a simmer, then reduce heat, cover, and cook for 20-25 minutes, or until vegetables are tender.", "Season with salt and pepper to taste.", "Serve hot with rice."],
                macros: { calories: "380 kcal", protein: "28g", carbs: "35g", fat: "14g" }
            },
            "Chicken Fajita Bowl": {
                ingredients: {
                    proteins: ["500g chicken breast, sliced into strips"],
                    carbs: ["1 cup cooked rice", "1/2 cup black beans, rinsed and drained"],
                    vegetables: ["2 bell peppers (different colors), sliced", "1 onion, sliced", "1/4 cup corn kernels (frozen or canned)", "Salsa", "Avocado slices"],
                    pantry: ["2 tbsp olive oil", "2 tsp fajita seasoning", "Lime wedges for serving"]
                },
                instructions: ["Heat olive oil in a large skillet over medium-high heat.", "Add chicken strips and cook until browned and cooked through.", "Add sliced bell peppers and onion, and sauté until tender-crisp.", "Stir in fajita seasoning and cook for another minute.", "Serve chicken and vegetable mixture over cooked rice, topped with black beans, corn, salsa, avocado, and lime wedges."],
                macros: { calories: "450 kcal", protein: "40g", carbs: "50g", fat: "15g" }
            },
            "Meatballs with Marinara": {
                ingredients: {
                    proteins: ["500g ground beef or turkey", "1 egg"],
                    carbs: ["1/2 cup breadcrumbs"],
                    vegetables: ["2 tbsp fresh parsley, chopped", "1 clove garlic, minced"],
                    pantry: ["1/4 cup milk", "1/2 tsp salt", "1/4 tsp black pepper", "1 tbsp olive oil", "1 jar (680g) marinara sauce", "Cooked pasta or zucchini noodles for serving"]
                },
                instructions: ["In a large bowl, gently combine ground meat, breadcrumbs, egg, milk, parsley, garlic, salt, and pepper.", "Form into 1-inch meatballs.", "Heat olive oil in a large skillet over medium heat.", "Brown meatballs on all sides, then drain any excess fat.", "Pour marinara sauce over the meatballs, bring to a simmer.", "Reduce heat, cover, and cook for 15-20 minutes, or until meatballs are cooked through.", "Serve hot over cooked pasta or zucchini noodles."],
                macros: { calories: "420 kcal", protein: "35g", carbs: "30g", fat: "20g" }
            },
            "Chicken Salad": {
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded or cubed"],
                    vegetables: ["1/4 cup celery, finely chopped", "2 tbsp red onion, finely chopped", "1 tbsp fresh dill or parsley, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise", "1 tsp Dijon mustard", "Salt and black pepper to taste", "Lettuce leaves or bread for serving"]
                },
                instructions: ["In a medium bowl, combine the cooked chicken, mayonnaise, chopped celery, and red onion.", "Stir in fresh dill or parsley (if using) and Dijon mustard.", "Mix all ingredients until well combined.", "Season with salt and black pepper to taste.", "Serve chilled on lettuce leaves, as a sandwich, or with crackers."],
                macros: { calories: "280 kcal", protein: "25g", carbs: "5g", fat: "18g" }
            },
            "Chicken Curry": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "1/2 cup frozen peas"],
                    pantry: ["1 tbsp olive oil", "1 tbsp grated fresh ginger", "2 tbsp curry powder", "1 can (400ml) coconut milk", "1 cup chicken broth", "Cooked basmati rice for serving"]
                },
                instructions: ["Heat olive oil in a large pot or deep skillet over medium heat.", "Add chicken cubes and cook until lightly browned; remove and set aside.", "In the same pot, add onion and cook until softened, about 5 minutes.", "Stir in garlic, ginger, and curry powder; cook for 1 minute until fragrant.", "Return chicken to the pot.", "Pour in coconut milk and chicken broth.", "Bring to a simmer, then reduce heat, cover, and cook for 15-20 minutes, or until chicken is cooked through.", "Stir in frozen peas and cook for another 5 minutes.", "Serve hot over basmati rice."],
                macros: { calories: "400 kcal", protein: "35g", carbs: "30g", fat: "18g" }
            },
            "Pepper and Cucumber Salad": {
                ingredients: {
                    proteins: [],
                    vegetables: ["2 cucumbers, diced", "2 bell peppers (any color), diced", "1/2 red onion, thinly sliced", "1/4 cup fresh dill, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp white wine vinegar", "1 tsp sugar (optional, for balance)", "Salt and black pepper to taste"]
                },
                instructions: ["Dice cucumbers and bell peppers into bite-sized pieces.", "Thinly slice the red onion and chop fresh dill.", "In a large bowl, combine the diced vegetables, red onion, and dill.", "In a small bowl, whisk together olive oil, white wine vinegar, sugar (if using), salt, and black pepper to create the dressing.", "Pour the dressing over the vegetables and toss gently to ensure everything is evenly coated.", "Serve immediately or chill in the refrigerator for at least 30 minutes to allow the flavors to meld."],
                macros: { calories: "180 kcal", protein: "2g", carbs: "15g", fat: "12g" }
            },
            "Mexican Burrito Bowl": {
                ingredients: {
                    proteins: ["Optional: cooked seasoned chicken, beef, or tofu"],
                    carbs: ["1 cup cooked rice (brown or white)", "1 cup cooked black beans, rinsed and drained"],
                    vegetables: ["1 cup cooked corn kernels (fresh, frozen, or canned)", "1/2 cup salsa", "1/4 cup chopped fresh cilantro", "1/2 avocado, sliced or diced", "Lime wedges for serving"],
                    pantry: ["2 tbsp sour cream or Greek yogurt (optional)"]
                },
                instructions: ["Assemble the bowl by layering cooked rice as the base.", "Top with cooked black beans and corn kernels.", "Add salsa, fresh cilantro, and avocado slices/dices.", "If desired, add your choice of cooked and seasoned protein (chicken, beef, or tofu).", "Dollop with sour cream or Greek yogurt if using.", "Serve immediately with a lime wedge for squeezing over the top."],
                macros: { calories: "480 kcal", protein: "15g", carbs: "60g", fat: "20g" }
            },
            "Teriyaki Chicken": {
                ingredients: {
                    proteins: ["500g chicken breast or thighs, cut into bite-sized pieces"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1 cup broccoli florets", "1 bell pepper (any color), sliced", "Sliced green onions for garnish"],
                    pantry: ["1 tbsp olive oil", "1/2 cup teriyaki sauce (store-bought or homemade)", "1 tbsp honey or brown sugar (optional, for extra glaze)", "Sesame seeds for garnish"]
                },
                instructions: ["Heat olive oil in a large skillet or wok over medium-high heat.", "Add chicken pieces and stir-fry until cooked through and lightly browned.", "Add broccoli florets and sliced bell pepper to the skillet, stir-fry for 3-5 minutes until tender-crisp.", "Pour teriyaki sauce over the chicken and vegetables, stir to coat.", "If desired, add honey or brown sugar and let the sauce simmer for 1-2 minutes until slightly thickened and glossy.", "Serve hot over cooked rice, garnished with sesame seeds and sliced green onions."],
                macros: { calories: "390 kcal", protein: "35g", carbs: "40g", fat: "12g" }
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
            if (mealList.length === 0) return null;
            return mealList[Math.floor(Math.random() * mealList.length)];
        }

        // Function to distribute meals for Sunday-Friday consumption (6 meals)
        function distributeMealsForConsumption(mealOptions, type) {
            let assignedMeals = [];
            let shuffledOptions = [...mealOptions]; 
            shuffleArray(shuffledOptions); 

            if (type === 'lunches') {
                // L1, L1, L2, L2, L3, L3 for Sunday-Friday (6 meals)
                for (let i = 0; i < 3; i++) { 
                    const meal = shuffledOptions[i % shuffledOptions.length]; 
                    assignedMeals.push(meal, meal); 
                }
            } else if (type === 'dinners') {
                // D1, D2, D3, D1, D2, D3 for Sunday-Friday (6 meals)
                for (let i = 0; i < 6; i++) { 
                    assignedMeals.push(shuffledOptions[i % shuffledOptions.length]);
                }
            }
            return assignedMeals.slice(0, 6); // Ensure exactly 6 meals for Sun-Fri
        }

        // Global arrays to store all meals for the entire 4-week cycle
        let allConsumptionMeals = []; // Stores all LUNCH and DINNER meals actually consumed in the 4 weeks
        let allPrepMeals = []; // Stores all LUNCH and DINNER meals to be prepped on Sundays for the *following* weeks

        // Main function to generate the entire calendar
        function generateFullCalendar() {
            const calendarDisplayEl = document.getElementById('calendarDisplay');
            calendarDisplayEl.innerHTML = ''; // Clear previous content

            const dayNamesFull = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

            allConsumptionMeals = []; // Reset global arrays
            allPrepMeals = [];

            for (let weekDisplay = 0; weekDisplay < 4; weekDisplay++) { // Loop for 4 weeks
                const weekSection = document.createElement('div');
                weekSection.classList.add('calendar-section');

                const weekTitle = document.createElement('h2');
                weekTitle.classList.add('week-title');
                weekTitle.textContent = `Week ${weekDisplay + 1}`;
                weekSection.appendChild(weekTitle);

                const weekCalendarGrid = document.createElement('div');
                weekCalendarGrid.classList.add('calendar-grid');

                // Day Headers (Sun-Sat)
                dayNamesFull.forEach(dayName => {
                    const header = document.createElement('div');
                    header.classList.add('day-header');
                    header.textContent = dayName;
                    weekCalendarGrid.appendChild(header);
                });

                // Get meal data for the *current* week's consumption
                const currentConsumptionWeekKey = `week${(weekDisplay % 4) + 1}`;
                const currentConsumptionWeekData = mealPlan[currentConsumptionWeekKey];
                
                // Get meal data for the *next* week's consumption (these are the meals to be prepped on Sunday of *this* week)
                const nextConsumptionWeekKey = `week${((weekDisplay + 1) % 4) + 1}`;
                const nextConsumptionWeekData = mealPlan[nextConsumptionWeekKey];

                const distributedLunchesConsumption = distributeMealsForConsumption(currentConsumptionWeekData.lunches, 'lunches');
                const distributedDinnersConsumption = distributeMealsForConsumption(currentConsumptionWeekData.dinners, 'dinners');

                // Random meals to be PREPPED on Sunday of the *current* week for *next* week's consumption
                const sundayPrepLunchForNextWeek = getRandomMeal(nextConsumptionWeekData.lunches);
                const sundayPrepDinnerForNextWeek = getRandomMeal(nextConsumptionWeekData.dinners);

                // Create days for Sunday to Saturday
                for (let dayIndex = 0; dayIndex < 7; dayIndex++) { 
                    const dayEl = document.createElement('div');
                    dayEl.classList.add('day');

                    const dayNameEl = document.createElement('div');
                    dayNameEl.classList.add('day-name');
                    dayNameEl.textContent = dayNamesFull[dayIndex];
                    dayEl.appendChild(dayNameEl);

                    if (dayIndex === 6) { // Saturday is blank
                        dayEl.classList.add('empty-day');
                        dayEl.innerHTML += '<p style="margin-top: 10px;">No Meals</p>'; 
                    } else { // Sunday to Friday (Consumption Days)
                        const currentDayLunch = distributedLunchesConsumption[dayIndex];
                        const currentDayDinner = distributedDinnersConsumption[dayIndex];

                        // Add to global consumption list
                        if (currentDayLunch) allConsumptionMeals.push(currentDayLunch);
                        if (currentDayDinner) allConsumptionMeals.push(currentDayDinner);

                        // Display consumption meals
                        if (currentDayLunch) {
                            const lunchEl = document.createElement('div');
                            lunchEl.classList.add('meal', 'lunch');
                            lunchEl.textContent = `Lunch: ${currentDayLunch}`;
                            lunchEl.onclick = () => openRecipeModal(currentDayLunch);
                            dayEl.appendChild(lunchEl);
                        }

                        if (currentDayDinner) {
                            const dinnerEl = document.createElement('div');
                            dinnerEl.classList.add('meal', 'dinner');
                            dinnerEl.textContent = `Dinner: ${currentDayDinner}`;
                            dinnerEl.onclick = () => openRecipeModal(currentDayDinner);
                            dayEl.appendChild(dinnerEl);
                        }

                        if (dayIndex === 0) { // Sunday - Also a Prep Day
                            dayEl.classList.add('prep-day');
                            const prepIndicator = document.createElement('div');
                            prepIndicator.classList.add('prep-indicator');
                            prepIndicator.textContent = 'Prep Day';
                            dayEl.appendChild(prepIndicator);

                            // Add the "Prep" meals (for next week's consumption) to Sunday's display
                            // These are distinct from the consumption meals of the current Sunday.
                            if (sundayPrepLunchForNextWeek) {
                                const prepLunchEl = document.createElement('div');
                                prepLunchEl.classList.add('meal', 'prep-meal'); // Use new class for distinct styling
                                prepLunchEl.textContent = `Prep Lunch (for Mon): ${sundayPrepLunchForNextWeek}`;
                                prepLunchEl.onclick = () => openRecipeModal(sundayPrepLunchForNextWeek);
                                dayEl.appendChild(prepLunchEl);
                                allPrepMeals.push(sundayPrepLunchForNextWeek); // Add to global prep list
                            }
                            if (sundayPrepDinnerForNextWeek) {
                                const prepDinnerEl = document.createElement('div');
                                prepDinnerEl.classList.add('meal', 'prep-meal'); // Use new class for distinct styling
                                prepDinnerEl.textContent = `Prep Dinner (for Mon): ${sundayPrepDinnerForNextWeek}`;
                                prepDinnerEl.onclick = () => openRecipeModal(sundayPrepDinnerForNextWeek);
                                dayEl.appendChild(prepDinnerEl);
                                allPrepMeals.push(sundayPrepDinnerForNextWeek); // Add to global prep list
                            }
                        }
                    }
                    weekCalendarGrid.appendChild(dayEl);
                }
                weekSection.appendChild(weekCalendarGrid);
                calendarDisplayEl.appendChild(weekSection);
            }
        }

        // Universal Modal Functions
        const universalModal = document.getElementById('universalModal');
        const modalTitle = document.getElementById('modalTitle');
        const modalBody = document.getElementById('modalBody');

        function openModal(title, contentHtml) {
            modalTitle.textContent = title;
            modalBody.innerHTML = contentHtml;
            universalModal.style.display = 'flex';
        }

        function closeModal() {
            universalModal.style.display = 'none';
            modalTitle.textContent = '';
            modalBody.innerHTML = '';
        }

        // Close modal when clicking outside of it
        window.onclick = function(event) {
            if (event.target === universalModal) {
                closeModal();
            }
        }

        // --- Recipe Modal Content Generation ---
        function openRecipeModal(recipeName) {
            const recipe = recipes[recipeName];

            if (recipe) {
                let ingredientsHtml = '<h3>Ingredients:</h3><ul class="ingredients-list">';
                const categoriesOrder = ['proteins', 'carbs', 'vegetables', 'pantry'];
                categoriesOrder.forEach(category => {
                    if (recipe.ingredients[category] && recipe.ingredients[category].length > 0) {
                        ingredientsHtml += `<li class="category-header">${category.charAt(0).toUpperCase() + category.slice(1)}</li>`;
                        recipe.ingredients[category].forEach(item => {
                            ingredientsHtml += `<li>${item}</li>`;
                        });
                    }
                });
                ingredientsHtml += '</ul>';

                const instructionsHtml = recipe.instructions && recipe.instructions.length > 0
                    ? `<h3>Instructions:</h3><ol class="instructions-list">${recipe.instructions.map(step => `<li>${step}</li>`).join('')}</ol>`
                    : '';

                const macrosHtml = recipe.macros
                    ? `
                        <h3>Macros:</h3>
                        <ul class="macros-list">
                            <li>Calories: ${recipe.macros.calories || 'N/A'}</li>
                            <li>Protein: ${recipe.macros.protein || 'N/A'}</li>
                            <li>Carbs: ${recipe.macros.carbs || 'N/A'}</li>
                            <li>Fat: ${recipe.macros.fat || 'N/A'}</li>
                        </ul>
                    `
                    : '';

                openModal(recipeName, ingredientsHtml + instructionsHtml + macrosHtml);
            } else {
                openModal("Recipe Not Found", "<p>Sorry, the recipe for this meal is not available.</p>");
            }
        }

        // --- Full Grocery List Modal Content Generation ---
        function openGroceryListModal() {
            const aggregatedIngredients = {
                proteins: new Set(),
                carbs: new Set(),
                vegetables: new Set(),
                pantry: new Set()
            };

            // Combine ingredients from ALL consumption meals across all 4 weeks
            new Set(allConsumptionMeals).forEach(mealName => { // Use Set to avoid duplicate processing of same recipe
                const recipe = recipes[mealName];
                if (recipe && recipe.ingredients) {
                    for (const category in recipe.ingredients) {
                        recipe.ingredients[category].forEach(item => {
                            aggregatedIngredients[category].add(item);
                        });
                    }
                }
            });

            // Combine ingredients from ALL prep meals across all 4 weeks
            new Set(allPrepMeals).forEach(mealName => { // Use Set to avoid duplicate processing of same recipe
                const recipe = recipes[mealName];
                if (recipe && recipe.ingredients) {
                    for (const category in recipe.ingredients) {
                        recipe.ingredients[category].forEach(item => {
                            aggregatedIngredients[category].add(item);
                        });
                    }
                }
            });

            let groceryHtml = '<ul class="modal-list">';
            const categoriesOrder = ['proteins', 'carbs', 'vegetables', 'pantry'];
            let contentFound = false;

            categoriesOrder.forEach(category => {
                if (aggregatedIngredients[category].size > 0) {
                    groceryHtml += `<li class="category-header">${category.charAt(0).toUpperCase() + category.slice(1)}</li>`;
                    groceryHtml += '<ul>';
                    Array.from(aggregatedIngredients[category]).sort().forEach(item => {
                        groceryHtml += `<li>${item}</li>`;
                    });
                    groceryHtml += '</ul>';
                    contentFound = true;
                }
            });

            if (!contentFound) {
                groceryHtml += '<li>No grocery items found for the entire cycle.</li>';
            }
            groceryHtml += '</ul>';

            openModal("Full Grocery List", groceryHtml);
        }

        // --- Full Preparation Steps Modal Content Generation ---
        function openPreparationStepsModal() {
            const uniquePrepMeals = new Set(allPrepMeals); // Get unique prep meals from global list

            let prepHtml = '';
            if (uniquePrepMeals.size === 0) {
                prepHtml = '<p>No specific preparation steps needed for this cycle.</p>';
            } else {
                prepHtml = '<h3>Preparation Steps for the Cycle:</h3><ol class="modal-list">';
                uniquePrepMeals.forEach(mealName => {
                    const recipe = recipes[mealName];
                    if (recipe && recipe.instructions && recipe.instructions.length > 0) {
                        prepHtml += `<li><strong>${mealName}:</strong>`;
                        prepHtml += `<ul>${recipe.instructions.map(step => `<li>${step}</li>`).join('')}</ul>`;
                        prepHtml += `</li>`;
                    }
                });
                prepHtml += '</ol>';
            }
            
            openModal("Full Preparation Steps", prepHtml);
        }

        // Initial generation on page load
        document.addEventListener('DOMContentLoaded', generateFullCalendar);
    </script>
</body>
</html>
