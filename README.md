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

        /* Header Styles - ONLY the main title is here, hidden below */
        .main-title {
            /* This section is now hidden as per request */
            display: none; /* Hides the blue title */
        }

        /* Main Content Wrapper - Holds the entire calendar */
        .main-container {
            width: 100%;
            max-width: 1200px; /* Max width for desktop */
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            padding: 25px;
            margin-bottom: 30px;
        }

        /* Calendar Grid for the entire month */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr); /* 7 columns for Sun-Sat */
            gap: 10px; /* Reduced gap for more compact view */
            width: 100%;
        }

        /* Hides the month name that is generated as a day-header */
        .day-header.month-name {
            display: none;
        }

        .day-header {
            font-weight: bold;
            font-size: 1.1em;
            color: var(--pastel-green-dark);
            text-align: center;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--pastel-green-dark);
            margin-bottom: 5px; /* Reduced margin */
        }

        .day {
            background-color: var(--pastel-green-light); /* Light green for days */
            border-radius: 8px; /* Slightly smaller border-radius */
            padding: 12px; /* Reduced padding */
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05); /* Softer shadow */
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            min-height: 150px; /* Reduced min-height */
            transition: transform 0.2s ease;
            position: relative;
            border: 1px solid var(--border-color);
            overflow: hidden; /* Ensure content stays within day box */
        }

        .day.empty-day {
            background-color: #f0f0f0;
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.03);
            color: #888;
            text-align: center;
            justify-content: center;
            align-items: center;
            font-style: italic;
            display: flex;
            flex-direction: column;
            gap: 8px; /* Reduced gap */
            padding: 12px;
        }

        .day:hover:not(.empty-day) {
            transform: translateY(-3px); /* Smaller lift on hover */
        }

        .week-number {
            position: absolute;
            top: 5px;
            left: 5px;
            font-size: 0.75em;
            font-weight: bold;
            color: #888;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 2px 5px;
            border-radius: 3px;
        }

        .day-name {
            font-weight: bold;
            font-size: 1em; /* Slightly smaller font */
            color: var(--text-color-dark);
            margin-bottom: 8px; /* Reduced margin */
            border-bottom: 1px solid var(--pastel-green-medium);
            padding-bottom: 4px; /* Reduced padding */
        }
        .day-date {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 10px;
        }

        /* Hide the "(saturday no meal)" text specifically */
        .day.empty-day .no-meal-text {
            display: none;
        }

        .prep-day {
            background-color: var(--pastel-pink-light); /* Light pink for Sundays (prep days) */
            border: 2px dashed var(--pastel-pink-dark);
        }

        .prep-indicator {
            position: absolute;
            top: 3px; /* Adjusted position */
            right: 3px; /* Adjusted position */
            background-color: var(--pastel-pink-dark);
            color: var(--text-color-light);
            padding: 2px 6px; /* Reduced padding */
            border-radius: 4px; /* Smaller border-radius */
            font-size: 0.7em; /* Smaller font */
            font-weight: bold;
            z-index: 10;
        }

        .meal {
            padding: 6px 8px; /* Reduced padding */
            margin-bottom: 6px; /* Reduced margin */
            border-radius: 5px; /* Smaller border-radius */
            cursor: pointer;
            font-size: 0.9em; /* Slightly smaller font */
            transition: background-color 0.2s ease;
            display: flex;
            align-items: center; /* Align emoji and text */
            gap: 5px; /* Space between emoji and text */
        }

        .meal:last-child {
            margin-bottom: 0;
        }

        .meal:hover {
            background-color: var(--pastel-green-medium);
        }

        .lunch {
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
        }

        .dinner {
            background-color: var(--pastel-pink-light);
            border-left: 4px solid var(--pastel-pink-dark);
        }

        /* Specific styles for buttons inside Saturday's box */
        .week-action-button {
            background-color: var(--pastel-pink-dark);
            color: var(--text-color-light);
            border: none;
            padding: 6px 10px; /* Reduced padding */
            border-radius: 4px; /* Smaller border-radius */
            cursor: pointer;
            font-size: 0.8em; /* Slightly smaller font */
            font-weight: bold;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1); /* Softer shadow */
            width: 100%;
            text-align: center;
        }

        .week-action-button:hover {
            background-color: #c42f73;
            transform: translateY(-1px);
        }

        /* Modal Styles (for Recipe, Grocery, and Preparations) */
        .modal {
            display: none;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.6);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 25px; /* Reduced padding */
            border-radius: 10px; /* Slightly smaller border-radius */
            width: 85%; /* Wider modal for lists */
            max-width: 750px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 6px 20px rgba(0,0,0,0.18); /* Softer shadow */
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
            font-size: 30px; /* Slightly smaller */
            font-weight: bold;
            position: absolute;
            top: 10px; /* Adjusted position */
            right: 15px; /* Adjusted position */
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
            margin-top: 15px; /* Reduced margin */
            margin-bottom: 8px; /* Reduced margin */
            font-size: 1.3em; /* Slightly smaller */
        }

        /* Styles for lists inside modals (ingredients, instructions, grocery, prep) */
        .ingredients-list, .instructions-list, .macros-list, .modal-list {
            list-style-type: none;
            padding: 0;
            margin-bottom: 15px; /* Reduced margin */
        }

        .ingredients-list li, .instructions-list li, .macros-list li, .modal-list li {
            padding: 6px 0; /* Reduced padding */
            border-bottom: 1px dashed var(--pastel-green-medium);
            font-size: 0.9em; /* Slightly smaller font */
        }

        .instructions-list li {
            border-bottom: 1px dashed var(--pastel-pink-medium);
        }

        .ingredients-list li:last-child, .instructions-list li:last-child, .macros-list li:last-child, .modal-list li:last-child {
            border-bottom: none;
        }

        .modal-body ol li {
            margin-bottom: 8px; /* Reduced margin */
        }

        /* Grocery/Preparation Modal specific list styles */
        .modal-list .category-header {
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
            font-weight: bold;
            margin-top: 12px; /* Reduced margin */
            margin-bottom: 6px; /* Reduced margin */
            padding: 8px 10px; /* Reduced padding */
            font-size: 1em; /* Slightly smaller */
        }
        .modal-list ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .modal-list ul li {
            background-color: var(--pastel-pink-light);
            border-left: 4px solid var(--pastel-pink-dark);
            margin-bottom: 5px; /* Reduced margin */
            padding: 6px 10px; /* Reduced padding */
            border-radius: 3px;
        }
        .modal-list ol li { /* For preparation steps that are numbered */
            margin-bottom: 8px;
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
            padding: 6px 10px;
            border-radius: 3px;
        }

        /* Responsive Design - Optimized for iPad Pro 11-inch (834px x 1194px) and general tablets */
        @media (min-width: 768px) and (max-width: 1200px) {
            .main-container {
                padding: 15px;
            }
            .calendar-grid {
                gap: 8px;
            }
            .day-header {
                font-size: 1em;
                padding-bottom: 8px;
            }
            .day {
                min-height: 130px; /* Adjust height for tablets */
                padding: 10px;
            }
            .day-name {
                font-size: 0.95em;
                margin-bottom: 5px;
            }
            .day-date {
                font-size: 0.8em;
                margin-bottom: 8px;
            }
            .meal {
                font-size: 0.85em;
                padding: 5px 6px;
                margin-bottom: 5px;
            }
            .week-action-button {
                font-size: 0.75em;
                padding: 5px 8px;
            }
            .modal-content {
                width: 90%;
            }
            .close-button {
                font-size: 28px;
            }
            .modal-body h3 {
                font-size: 1.2em;
            }
            .ingredients-list li, .instructions-list li, .macros-list li, .modal-list li {
                font-size: 0.85em;
                padding: 5px 0;
            }
        }

        @media (max-width: 767px) { /* General mobile/smaller tablet */
            body {
                padding: 10px;
            }
            .main-title {
                font-size: 1.8em;
                margin-bottom: 20px;
            }
            .main-container {
                padding: 10px;
            }
            .calendar-grid {
                grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); /* Allow more flexibility */
                gap: 5px;
            }
            .day-header {
                font-size: 0.9em;
                padding-bottom: 5px;
            }
            .day {
                min-height: 90px;
                padding: 8px;
            }
            .day-name {
                font-size: 0.8em;
                margin-bottom: 5px;
            }
            .day-date {
                font-size: 0.75em;
                margin-bottom: 5px;
            }
            .meal {
                font-size: 0.75em;
                padding: 4px 5px;
                margin-bottom: 4px;
            }
            .week-action-button {
                font-size: 0.7em;
                padding: 4px 6px;
            }
            .modal-content {
                width: 95%;
                padding: 15px;
            }
            .close-button {
                font-size: 24px;
                top: 8px;
                right: 8px;
            }
            .modal-body h3 {
                font-size: 1.1em;
            }
            .ingredients-list li, .instructions-list li, .macros-list li, .modal-list li {
                font-size: 0.8em;
                padding: 4px 0;
            }
        }
    </style>
</head>
<body>

    <h1 class="main-title">Meal Prep Calendar</h1>

    <div class="main-container">
        <div id="calendarDisplay" class="calendar-grid">
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

        // Recipe database with ingredients (categorized) and instructions, and MACROS, now with emojis
        const recipes = {
            "Kafta with Potatoes": {
                emoji: "🥔",
                ingredients: {
                    proteins: ["500g ground lamb or beef"],
                    vegetables: ["4 medium potatoes, cubed", "1 large onion, finely chopped", "2 cloves garlic, minced", "1/4 cup fresh parsley, chopped"],
                    pantry: ["1 tsp allspice", "1 tsp cinnamon", "Salt and pepper to taste", "2 tbsp olive oil"]
                },
                instructions: ["Mix ground meat with half the onion, garlic, parsley, and spices.", "Form into small oval-shaped patties.", "Heat olive oil in a large pan over medium heat.", "Brown kafta patties on both sides, then remove.", "In same pan, sauté remaining onions until golden.", "Add potatoes and cook for 5 minutes.", "Return kafta to pan, add water to barely cover.", "Simmer covered for 25-30 minutes until potatoes are tender."],
                macros: { calories: "350 kcal", protein: "30g", carbs: "40g", fat: "10g" }
            },
            "Lebanese Shawarma Bowl": {
                emoji: "🥙",
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
                emoji: "🥗",
                ingredients: {
                    proteins: ["4 anchovy fillets (optional)", "1 egg yolk"],
                    vegetables: ["2 large romaine lettuce heads", "2 cloves garlic"],
                    pantry: ["1/2 cup parmesan cheese, grated", "1 cup croutons", "1/4 cup lemon juice", "1/4 cup olive oil", "1 tsp Dijon mustard"]
                },
                instructions: ["Wash and chop romaine lettuce.", "Make dressing: blend garlic, anchovies, lemon juice, egg yolk, and mustard.", "Slowly add olive oil while blending.", "Toss lettuce with dressing.", "Top with parmesan and croutons.", "Serve immediately."],
                macros: { calories: "280 kcal", protein: "15g", carbs: "20g", fat: "18g" }
            },
            "Chickpea Salad": {
                emoji: "🥣",
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
                emoji: "🍗",
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["2 bell peppers, chopped", "1 zucchini, chopped", "1 red onion, chopped"],
                    pantry: ["2 tbsp olive oil", "1 tsp dried oregano", "Salt and pepper to taste"]
                },
                instructions: ["Preheat oven to 200°C (400°F).", "Toss chicken and vegetables with olive oil, oregano, salt, and pepper.", "Spread on a baking sheet in a single layer.", "Roast for 20-25 minutes, or until chicken is cooked through and vegetables are tender-crisp.", "Serve hot."],
                macros: { calories: "320 kcal", protein: "35g", carbs: "20g", fat: "12g" }
            },
            "Mediterranean Quinoa Salad": {
                emoji: "🍚",
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
                emoji: "🥢",
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded"],
                    vegetables: ["4 cups shredded cabbage (green or napa)", "1 cup shredded carrots", "1/2 red bell pepper, thinly sliced", "1/4 cup chopped green onions", "1/4 cup chopped cilantro"],
                    pantry: ["1/4 cup rice vinegar", "2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp honey or maple syrup", "1 tsp grated fresh ginger", "1/2 cup chopped peanuts or cashews (optional)"]
                },
                instructions: ["In a large bowl, combine shredded chicken, cabbage, carrots, red bell pepper, green onions, and cilantro.", "In a small bowl, whisk together rice vinegar, soy sauce, sesame oil, honey, and ginger until well combined.", "Pour dressing over the salad ingredients and toss to coat evenly.", "If desired, add chopped peanuts or cashews for crunch.", "Serve immediately or chill for later."],
                macros: { calories: "300 kcal", protein: "25g", carbs: "25g", fat: "12g" }
            },
            "Shrimp Orzo": {
                emoji: "🍤",
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
                emoji: "🥩",
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
                emoji: "🍍",
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 bell pepper (any color), chopped", "1 onion, chopped"],
                    pantry: ["1 can (227g) pineapple chunks, drained (reserve juice)", "2 tbsp cornstarch", "1/4 cup white vinegar", "1/4 cup sugar", "2 tbsp soy sauce", "1 tbsp ketchup", "1/2 cup chicken broth", "2 tbsp oil for cooking"]
                },
                instructions: ["Toss chicken cubes with 1 tbsp cornstarch.", "Heat oil in a large skillet or wok over medium-high heat.", "Stir-fry chicken until lightly browned and cooked through, remove from pan.", "In the same pan, add bell pepper and onion, stir-fry for 3-4 minutes until slightly softened.", "In a small bowl, whisk together remaining cornstarch with vinegar, sugar, soy sauce, ketchup, pineapple juice, and chicken broth.", "Pour sauce into the skillet, bring to a simmer, stirring until thickened.", "Return chicken to the pan along with pineapple chunks.", "Toss to coat everything in the sauce.", "Serve hot with rice or noodles."],
                macros: { calories: "360 kcal", protein: "30g", carbs: "40g", fat: "10g" }
            },
            "Tuna Salad": {
                emoji: "🐟",
                ingredients: {
                    proteins: ["2 cans (140g each) tuna in water, drained"],
                    vegetables: ["1 stalk celery, finely chopped", "1/4 cup red onion, finely chopped", "1 tbsp fresh dill, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise or Greek yogurt", "1 tbsp Dijon mustard", "Salt and black pepper to taste"]
                },
                instructions: ["In a medium bowl, flake the drained tuna with a fork.", "Add mayonnaise (or Greek yogurt), chopped celery, red onion, dill (if using), and Dijon mustard.", "Mix well to combine all ingredients.", "Season with salt and black pepper to taste.", "Serve as a sandwich filling, with crackers, or on a bed of lettuce."],
                macros: { calories: "220 kcal", protein: "20g", carbs: "5g", fat: "15g" }
            },
            "Mediterranean Orzo Salad": {
                emoji: "🍝",
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
                emoji: "🧀",
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
                emoji: "🥕",
                ingredients: {
                    proteins: ["500g ground beef or lamb (optional)"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "4 cups frozen peas and carrots mix"],
                    pantry: ["1 tbsp olive oil", "1 can (400g) crushed tomatoes", "1 cup water or beef broth", "1 tsp seven spice (baharat)", "Salt and pepper to taste", "Cooked rice for serving"]
                },
                instructions: ["If using meat, brown ground beef/lamb in olive oil in a large pot; drain excess fat and set aside.", "In the same pot, sauté onion until softened, then add garlic and cook for 1 minute more.", "Stir in crushed tomatoes, peas and carrots, water/broth, and seven spice.", "If using meat, return it to the pot.", "Bring to a simmer, then reduce heat, cover, and cook for 20-25 minutes, or until vegetables are tender.", "Season with salt and pepper to taste.", "Serve hot with rice."],
                macros: { calories: "380 kcal", protein: "28g", carbs: "35g", fat: "14g" }
            },
            "Chicken Fajita Bowl": {
                emoji: "🌶️",
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
                emoji: "🍝",
                ingredients: {
                    proteins: ["500g ground beef or turkey"],
                    carbs: ["Cooked pasta or zucchini noodles for serving"],
                    vegetables: ["1/2 onion, finely chopped", "2 cloves garlic, minced"],
                    pantry: ["1 egg", "1/4 cup breadcrumbs", "1/4 cup milk", "1 tsp dried oregano", "Salt and pepper to taste", "1 can (800g) crushed tomatoes", "1 tbsp olive oil", "Fresh basil for garnish"]
                },
                instructions: ["In a large bowl, combine ground meat, chopped onion, minced garlic, egg, breadcrumbs, milk, oregano, salt, and pepper. Mix until just combined, don't overmix.", "Form into 1-inch meatballs.", "Heat olive oil in a large skillet or pot over medium-high heat. Brown meatballs on all sides, then remove.", "Pour crushed tomatoes into the same skillet, bring to a simmer.", "Return meatballs to the sauce, cover, and simmer for 20-25 minutes, or until meatballs are cooked through.", "Serve hot over pasta or zucchini noodles, garnished with fresh basil."],
                macros: { calories: "400 kcal", protein: "35g", carbs: "30g", fat: "18g" }
            },
            "Chicken Salad": {
                emoji: "🥪",
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded"],
                    vegetables: ["1 celery stalk, finely chopped", "1/4 cup red onion, finely chopped"],
                    pantry: ["1/2 cup mayonnaise or Greek yogurt", "1 tbsp Dijon mustard", "Salt and pepper to taste", "Optional: 1/4 cup grapes, halved, or chopped pecans"]
                },
                instructions: ["In a medium bowl, combine shredded chicken, chopped celery, and red onion.", "In a small bowl, whisk together mayonnaise (or Greek yogurt), Dijon mustard, salt, and pepper.", "Pour the dressing over the chicken mixture and mix until well combined.", "If using, stir in grapes or pecans.", "Serve as a sandwich filling, with crackers, or on lettuce cups."],
                macros: { calories: "280 kcal", protein: "28g", carbs: "5g", fat: "18g" }
            },
            "Chicken Curry": {
                emoji: "🍛",
                ingredients: {
                    proteins: ["500g chicken breast or thighs, cubed"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "1-inch ginger, grated", "1 bell pepper, chopped"],
                    pantry: ["1 tbsp olive oil", "2 tbsp curry powder", "1 can (400ml) coconut milk", "1/2 cup chicken broth", "1 tbsp tomato paste", "Salt and pepper to taste", "Fresh cilantro for garnish"]
                },
                instructions: ["Heat olive oil in a large pot or deep skillet over medium heat.", "Add chicken and cook until lightly browned; remove from pot.", "Add onion to the pot and sauté until softened.", "Stir in garlic, ginger, and curry powder; cook for 1 minute until fragrant.", "Add bell pepper, coconut milk, chicken broth, and tomato paste. Bring to a simmer.", "Return chicken to the pot. Cover and simmer for 20-25 minutes, or until chicken is cooked through and sauce has thickened slightly.", "Season with salt and pepper. Serve hot over cooked rice, garnished with fresh cilantro."],
                macros: { calories: "420 kcal", protein: "35g", carbs: "40g", fat: "18g" }
            },
            "Teriyaki Chicken": {
                emoji: "🥢",
                ingredients: {
                    proteins: ["500g chicken breast or thighs, cubed"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1 cup broccoli florets", "1 carrot, sliced into rounds", "Sliced green onions for garnish"],
                    pantry: ["1/2 cup soy sauce", "1/4 cup mirin (sweet rice wine)", "2 tbsp sake (optional)", "2 tbsp brown sugar", "1 tbsp grated fresh ginger", "2 cloves garlic, minced", "1 tbsp cornstarch (mixed with 2 tbsp water for slurry)", "1 tbsp sesame oil"]
                },
                instructions: ["In a bowl, whisk together soy sauce, mirin, sake (if using), brown sugar, ginger, and garlic to create the teriyaki sauce.", "Heat sesame oil in a large skillet or wok over medium-high heat.", "Add chicken and cook until browned on all sides. Remove from pan.", "Add broccoli and carrots to the skillet, stir-fry for 3-5 minutes until tender-crisp.", "Pour the teriyaki sauce into the skillet and bring to a simmer. Stir in the cornstarch slurry and cook, stirring, until the sauce thickens.", "Return chicken to the skillet, toss to coat everything in the sauce.", "Serve hot over cooked rice, garnished with green onions."],
                macros: { calories: "380 kcal", protein: "30g", carbs: "45g", fat: "10g" }
            },
            "Pepper and Cucumber Salad": {
                emoji: "🥒",
                ingredients: {
                    proteins: [],
                    carbs: [],
                    vegetables: ["2 cucumbers, diced", "2 bell peppers (any color), diced", "1/2 red onion, thinly sliced", "1/4 cup fresh mint, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp white wine vinegar", "1 tsp sumac", "Salt and pepper to taste"]
                },
                instructions: ["In a large bowl, combine diced cucumbers, bell peppers, red onion, and chopped fresh mint.", "In a small bowl, whisk together olive oil, white wine vinegar, sumac, salt, and pepper to make the dressing.", "Pour the dressing over the vegetables and toss gently to combine.", "Serve immediately or chill for a short period."],
                macros: { calories: "150 kcal", protein: "3g", carbs: "15g", fat: "10g" }
            },
            "Mexican Burrito Bowl": {
                emoji: "🌶️",
                ingredients: {
                    proteins: ["400g ground beef or black beans"],
                    carbs: ["1 cup cooked rice", "1 can (400g) black beans, rinsed and drained", "1 can (200g) corn kernels, drained"],
                    vegetables: ["1 bell pepper, diced", "1 onion, diced", "1/2 cup salsa", "1/4 cup chopped cilantro", "Avocado slices or guacamole"],
                    pantry: ["1 tbsp olive oil", "2 tsp chili powder", "1 tsp cumin", "Salt and pepper to taste", "Lime wedges", "Optional: shredded cheese, sour cream"]
                },
                instructions: ["If using ground beef, heat olive oil in a large skillet over medium-high heat and brown the beef, breaking it up with a spoon. Drain excess fat.", "Add diced bell pepper and onion to the skillet; sauté until softened.", "Stir in chili powder, cumin, salt, and pepper; cook for 1 minute until fragrant.", "Add black beans and corn to the skillet; stir to combine and heat through.", "Serve the mixture over cooked rice. Top each bowl with salsa, cilantro, avocado slices, and any optional toppings like cheese or sour cream.", "Serve with lime wedges."],
                macros: { calories: "480 kcal", protein: "30g", carbs: "60g", fat: "18g" }
            }
        };

        const mealTypes = {
            lunch: "Lunch",
            dinner: "Dinner"
        };

        const currentYear = new Date().getFullYear();
        const startMonth = new Date().getMonth(); // Current month
        const numberOfMonths = 1; // Display only the current month

        // Function to get the week number of the year for a given date
        function getWeekNumber(d) {
            d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
            d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay() || 7));
            var yearStart = new Date(Date.UTC(d.getUTCFullYear(), 0, 1));
            var weekNo = Math.ceil((((d - yearStart) / 86400000) + 1) / 7);
            return weekNo;
        }

        function renderCalendar() {
            const calendarDisplay = document.getElementById('calendarDisplay');
            calendarDisplay.innerHTML = ''; // Clear previous content

            const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            const dayNames = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

            for (let m = 0; m < numberOfMonths; m++) {
                const monthDate = new Date(currentYear, startMonth + m, 1);
                const monthName = monthNames[monthDate.getMonth()];
                const year = monthDate.getFullYear();
                const firstDayOfMonth = new Date(year, monthDate.getMonth(), 1).getDay(); // 0 for Sunday, 1 for Monday, etc.
                const daysInMonth = new Date(year, monthDate.getMonth() + 1, 0).getDate();

                // Month Name Header (now hidden by CSS)
                const monthHeader = document.createElement('div');
                monthHeader.classList.add('day-header', 'month-name'); // Add a class to specifically hide this
                monthHeader.style.gridColumn = 'span 7';
                monthHeader.textContent = `${monthName} ${year}`;
                calendarDisplay.appendChild(monthHeader);

                // Day Names Header (Sun, Mon, Tue...)
                dayNames.forEach(dayName => {
                    const header = document.createElement('div');
                    header.classList.add('day-header');
                    header.textContent = dayName;
                    calendarDisplay.appendChild(header);
                });

                // Empty placeholders for days before the 1st
                for (let i = 0; i < firstDayOfMonth; i++) {
                    const emptyDay = document.createElement('div');
                    emptyDay.classList.add('day', 'empty-day');
                    calendarDisplay.appendChild(emptyDay);
                }

                // Render actual days
                for (let day = 1; day <= daysInMonth; day++) {
                    const currentDay = new Date(year, monthDate.getMonth(), day);
                    const dayOfWeek = currentDay.getDay(); // 0-Sunday, 1-Monday, ..., 6-Saturday
                    const weekNo = getWeekNumber(currentDay);
                    const weekIndex = (weekNo % 4 === 0) ? 4 : (weekNo % 4); // Map week numbers to 1-4
                    const weekKey = `week${weekIndex}`;

                    const dayDiv = document.createElement('div');
                    dayDiv.classList.add('day');
                    if (dayOfWeek === 0) { // Sunday is a prep day
                        dayDiv.classList.add('prep-day');
                    } else if (dayOfWeek === 6) { // Saturday no meal
                        dayDiv.classList.add('empty-day');
                    }

                    // Add week number
                    const weekNumberSpan = document.createElement('span');
                    weekNumberSpan.classList.add('week-number');
                    weekNumberSpan.textContent = `Week ${weekNo}`;
                    dayDiv.appendChild(weekNumberSpan);

                    const dayNameSpan = document.createElement('div');
                    dayNameSpan.classList.add('day-name');
                    dayNameSpan.textContent = dayNames[dayOfWeek];
                    dayDiv.appendChild(dayNameSpan);

                    const dayDateSpan = document.createElement('div');
                    dayDateSpan.classList.add('day-date');
                    dayDateSpan.textContent = `${monthName.substring(0, 3)} ${day}, ${year}`;
                    dayDiv.appendChild(dayDateSpan);

                    if (dayOfWeek === 6) { // Saturday
                        const noMealText = document.createElement('div');
                        noMealText.classList.add('no-meal-text'); // Add class to hide this specific text
                        noMealText.textContent = "(No Meal Scheduled)";
                        dayDiv.appendChild(noMealText);

                        const prepButton = document.createElement('button');
                        prepButton.classList.add('week-action-button');
                        prepButton.textContent = "View Weekly Prep";
                        prepButton.onclick = () => showPreparations(weekKey);
                        dayDiv.appendChild(prepButton);

                        const groceryButton = document.createElement('button');
                        groceryButton.classList.add('week-action-button');
                        groceryButton.textContent = "View Grocery List";
                        groceryButton.onclick = () => showGroceryList(weekKey);
                        dayDiv.appendChild(groceryButton);
                    } else if (dayOfWeek === 0) { // Sunday
                        const prepIndicator = document.createElement('span');
                        prepIndicator.classList.add('prep-indicator');
                        prepIndicator.textContent = 'Prep Day!';
                        dayDiv.appendChild(prepIndicator);

                        // Add meals for Sunday (if any)
                        if (mealPlan[weekKey] && mealPlan[weekKey].lunches) {
                            mealPlan[weekKey].lunches.forEach(mealName => {
                                if (recipes[mealName]) {
                                    const mealDiv = document.createElement('div');
                                    mealDiv.classList.add('meal', 'lunch');
                                    mealDiv.innerHTML = `${recipes[mealName].emoji} ${mealName}`;
                                    mealDiv.onclick = () => showRecipeDetails(mealName);
                                    dayDiv.appendChild(mealDiv);
                                }
                            });
                        }
                    } else { // Monday to Friday
                        if (mealPlan[weekKey]) {
                            // Lunch
                            if (mealPlan[weekKey].lunches && mealPlan[weekKey].lunches.length > 0) {
                                const lunchIndex = (dayOfWeek - 1) % mealPlan[weekKey].lunches.length; // Cycle through lunches Mon-Fri
                                const lunchMealName = mealPlan[weekKey].lunches[lunchIndex];
                                if (recipes[lunchMealName]) {
                                    const mealDiv = document.createElement('div');
                                    mealDiv.classList.add('meal', 'lunch');
                                    mealDiv.innerHTML = `${recipes[lunchMealName].emoji} ${lunchMealName}`;
                                    mealDiv.onclick = () => showRecipeDetails(lunchMealName);
                                    dayDiv.appendChild(mealDiv);
                                }
                            }

                            // Dinner
                            if (mealPlan[weekKey].dinners && mealPlan[weekKey].dinners.length > 0) {
                                const dinnerIndex = (dayOfWeek - 1) % mealPlan[weekKey].dinners.length; // Cycle through dinners Mon-Fri
                                const dinnerMealName = mealPlan[weekKey].dinners[dinnerIndex];
                                if (recipes[dinnerMealName]) {
                                    const mealDiv = document.createElement('div');
                                    mealDiv.classList.add('meal', 'dinner');
                                    mealDiv.innerHTML = `${recipes[dinnerMealName].emoji} ${dinnerMealName}`;
                                    mealDiv.onclick = () => showRecipeDetails(dinnerMealName);
                                    dayDiv.appendChild(mealDiv);
                                }
                            }
                        }
                    }
                    calendarDisplay.appendChild(dayDiv);
                }
            }
        }

        function showRecipeDetails(mealName) {
            const recipe = recipes[mealName];
            if (!recipe) return;

            const modal = document.getElementById('universalModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            modalTitle.textContent = `${recipe.emoji} ${mealName}`;
            modalBody.innerHTML = ''; // Clear previous content

            // Ingredients
            if (Object.keys(recipe.ingredients).length > 0) {
                const ingredientsHeading = document.createElement('h3');
                ingredientsHeading.textContent = 'Ingredients';
                modalBody.appendChild(ingredientsHeading);

                const ingredientsList = document.createElement('ul');
                ingredientsList.classList.add('ingredients-list');
                for (const category in recipe.ingredients) {
                    if (recipe.ingredients[category].length > 0) {
                        const categoryHeader = document.createElement('li');
                        categoryHeader.classList.add('category-header');
                        categoryHeader.textContent = category.charAt(0).toUpperCase() + category.slice(1);
                        ingredientsList.appendChild(categoryHeader);
                        recipe.ingredients[category].forEach(item => {
                            const listItem = document.createElement('li');
                            listItem.textContent = item;
                            ingredientsList.appendChild(listItem);
                        });
                    }
                }
                modalBody.appendChild(ingredientsList);
            }

            // Instructions
            if (recipe.instructions && recipe.instructions.length > 0) {
                const instructionsHeading = document.createElement('h3');
                instructionsHeading.textContent = 'Instructions';
                modalBody.appendChild(instructionsHeading);

                const instructionsList = document.createElement('ol');
                instructionsList.classList.add('instructions-list');
                recipe.instructions.forEach(step => {
                    const listItem = document.createElement('li');
                    listItem.textContent = step;
                    instructionsList.appendChild(listItem);
                });
                modalBody.appendChild(instructionsList);
            }

            // Macros
            if (recipe.macros && Object.keys(recipe.macros).length > 0) {
                const macrosHeading = document.createElement('h3');
                macrosHeading.textContent = 'Macros (Approximate)';
                modalBody.appendChild(macrosHeading);

                const macrosList = document.createElement('ul');
                macrosList.classList.add('macros-list');
                for (const macro in recipe.macros) {
                    const listItem = document.createElement('li');
                    listItem.textContent = `${macro.charAt(0).toUpperCase() + macro.slice(1)}: ${recipe.macros[macro]}`;
                    macrosList.appendChild(listItem);
                }
                modalBody.appendChild(macrosList);
            }

            modal.style.display = 'flex'; // Use flex to center
        }

        function showGroceryList(weekKey) {
            const modal = document.getElementById('universalModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            modalTitle.textContent = `🛒 Grocery List for ${weekKey.replace('week', 'Week ')}`;
            modalBody.innerHTML = '';

            const allIngredients = {};

            // Aggregate ingredients from all meals in the specified week
            const mealsInWeek = [
                ...(mealPlan[weekKey]?.lunches || []),
                ...(mealPlan[weekKey]?.dinners || [])
            ];

            mealsInWeek.forEach(mealName => {
                const recipe = recipes[mealName];
                if (recipe) {
                    for (const category in recipe.ingredients) {
                        recipe.ingredients[category].forEach(item => {
                            if (!allIngredients[category]) {
                                allIngredients[category] = new Set();
                            }
                            allIngredients[category].add(item);
                        });
                    }
                }
            });

            if (Object.keys(allIngredients).length === 0) {
                modalBody.innerHTML = '<p>No meals planned for this week to generate a grocery list.</p>';
            } else {
                const groceryList = document.createElement('ul');
                groceryList.classList.add('modal-list');
                for (const category in allIngredients) {
                    const categoryHeader = document.createElement('li');
                    categoryHeader.classList.add('category-header');
                    categoryHeader.textContent = category.charAt(0).toUpperCase() + category.slice(1);
                    groceryList.appendChild(categoryHeader);

                    const itemsList = document.createElement('ul');
                    Array.from(allIngredients[category]).sort().forEach(item => {
                        const listItem = document.createElement('li');
                        listItem.textContent = item;
                        itemsList.appendChild(listItem);
                    });
                    groceryList.appendChild(itemsList);
                }
                modalBody.appendChild(groceryList);
            }

            modal.style.display = 'flex';
        }

        function showPreparations(weekKey) {
            const modal = document.getElementById('universalModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            modalTitle.textContent = `📝 Weekly Preparations for ${weekKey.replace('week', 'Week ')}`;
            modalBody.innerHTML = '';

            const prepSteps = new Set();
            const mealsInWeek = [
                ...(mealPlan[weekKey]?.lunches || []),
                ...(mealPlan[weekKey]?.dinners || [])
            ];

            mealsInWeek.forEach(mealName => {
                const recipe = recipes[mealName];
                if (recipe && recipe.instructions && recipe.instructions.length > 0) {
                    // Just take the first few (or all) instructions as prep suggestions
                    recipe.instructions.slice(0, 2).forEach(step => { // Example: first 2 steps
                        prepSteps.add(`For ${mealName}: ${step}`);
                    });
                }
            });

            if (prepSteps.size === 0) {
                modalBody.innerHTML = '<p>No specific preparation steps found for meals this week.</p>';
            } else {
                const prepList = document.createElement('ol');
                prepList.classList.add('modal-list');
                Array.from(prepSteps).sort().forEach(step => {
                    const listItem = document.createElement('li');
                    listItem.textContent = step;
                    prepList.appendChild(listItem);
                });
                modalBody.appendChild(prepList);
            }

            modal.style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('universalModal').style.display = 'none';
        }

        // Initial render
        document.addEventListener('DOMContentLoaded', renderCalendar);
    </script>
</body>
</html>
