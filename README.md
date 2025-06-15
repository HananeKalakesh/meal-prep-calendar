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

        /* Header Styles - Only one will be rendered via JS now */
        .main-title {
            color: var(--pastel-green-dark);
            text-align: center;
            margin-bottom: 30px;
            font-size: 2.5em;
            letter-spacing: 0.05em;
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
                    proteins: ["500g ground beef or turkey", "1 egg"],
                    carbs: ["1/2 cup breadcrumbs"],
                    vegetables: ["2 tbsp fresh parsley, chopped", "1 clove garlic, minced"],
                    pantry: ["1/4 cup milk", "1/2 tsp salt", "1/4 tsp black pepper", "1 tbsp olive oil", "1 jar (680g) marinara sauce", "Cooked pasta or zucchini noodles for serving"]
                },
                instructions: ["In a large bowl, gently combine ground meat, breadcrumbs, egg, milk, parsley, garlic, salt, and pepper.", "Form into 1-inch meatballs.", "Heat olive oil in a large skillet over medium heat.", "Brown meatballs on all sides, then drain any excess fat.", "Pour marinara sauce over the meatballs, bring to a simmer.", "Reduce heat, cover, and cook for 15-20 minutes, or until meatballs are cooked through.", "Serve hot over cooked pasta or zucchini noodles."],
                macros: { calories: "420 kcal", protein: "35g", carbs: "30g", fat: "20g" }
            },
            "Chicken Salad": {
                emoji: "🥗",
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded or cubed"],
                    vegetables: ["1/4 cup celery, finely chopped", "2 tbsp red onion, finely chopped", "1 tbsp fresh dill or parsley, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise", "1 tsp Dijon mustard", "Salt and black pepper to taste", "Lettuce leaves or bread for serving"]
                },
                instructions: ["In a medium bowl, combine the cooked chicken, mayonnaise, chopped celery, and red onion.", "Stir in fresh dill or parsley (if using) and Dijon mustard.", "Mix all ingredients until well combined.", "Season with salt and black pepper to taste.", "Serve chilled on lettuce leaves, as a sandwich, or with crackers."],
                macros: { calories: "280 kcal", protein: "25g", carbs: "5g", fat: "18g" }
            },
            "Chicken Curry": {
                emoji: "🍛",
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "1/2 cup frozen peas"],
                    pantry: ["1 tbsp olive oil", "1 tbsp grated fresh ginger", "2 tbsp curry powder", "1 can (400ml) coconut milk", "1 cup chicken broth", "Cooked basmati rice for serving"]
                },
                instructions: ["Heat olive oil in a large pot or deep skillet over medium heat.", "Add chicken cubes and cook until lightly browned; remove and set aside.", "In the same pot, add onion and cook until softened, about 5 minutes.", "Stir in garlic, ginger, and curry powder; cook for 1 minute until fragrant.", "Return chicken to the pot.", "Pour in coconut milk and chicken broth.", "Bring to a simmer, then reduce heat, cover, and cook for 15-20 minutes, or until chicken is cooked through.", "Stir in frozen peas and cook for another 5 minutes.", "Serve hot over basmati rice."],
                macros: { calories: "400 kcal", protein: "35g", carbs: "30g", fat: "18g" }
            },
            "Pepper and Cucumber Salad": {
                emoji: "🥒",
                ingredients: {
                    proteins: [],
                    vegetables: ["2 cucumbers, diced", "2 bell peppers (any color), diced", "1/2 red onion, thinly sliced", "1/4 cup fresh dill, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp white wine vinegar", "1 tsp sugar (optional, for balance)", "Salt and black pepper to taste"]
                },
                instructions: ["Dice cucumbers and bell peppers into bite-sized pieces.", "Thinly slice the red onion and chop fresh dill.", "In a large bowl, combine the diced vegetables, red onion, and dill.", "In a small bowl, whisk together olive oil, white wine vinegar, sugar (if using), salt, and black pepper to create the dressing.", "Pour the dressing over the vegetables and toss gently to ensure everything is evenly coated.", "Serve immediately or chill in the refrigerator for at least 30 minutes to allow the flavors to meld."],
                macros: { calories: "180 kcal", protein: "2g", carbs: "15g", fat: "12g" }
            },
            "Mexican Burrito Bowl": {
                emoji: "🌶️",
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
                emoji: "🍣",
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

        // Main function to generate the entire calendar
        function generateFullCalendar() {
            const calendarDisplayEl = document.getElementById('calendarDisplay');
            calendarDisplayEl.innerHTML = ''; // Clear previous content

            const dayNamesFull = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
            const startDate = new Date(); // Start from today's date
            startDate.setDate(startDate.getDate() - startDate.getDay()); // Set to the most recent Sunday

            // Add day headers once at the top
            dayNamesFull.forEach(dayName => {
                const header = document.createElement('div');
                header.classList.add('day-header');
                header.textContent = dayName;
                calendarDisplayEl.appendChild(header);
            });

            for (let weekDisplay = 0; weekDisplay < 4; weekDisplay++) { // Loop for 4 weeks
                // Get meal data for the *current* week's consumption
                const currentConsumptionWeekKey = `week${(weekDisplay % 4) + 1}`;
                const currentConsumptionWeekData = mealPlan[currentConsumptionWeekKey];

                const distributedLunchesConsumption = distributeMealsForConsumption(currentConsumptionWeekData.lunches, 'lunches');
                const distributedDinnersConsumption = distributeMealsForConsumption(currentConsumptionWeekData.dinners, 'dinners');

                // Store meals for this specific week to be used by its grocery/prep buttons
                const currentWeekMealsForGroceryAndPrep = {
                    lunches: [],
                    dinners: []
                };

                // Create days for Sunday to Saturday
                for (let dayIndex = 0; dayIndex < 7; dayIndex++) {
                    const currentDay = new Date(startDate);
                    currentDay.setDate(startDate.getDate() + (weekDisplay * 7) + dayIndex);

                    const dayEl = document.createElement('div');
                    dayEl.classList.add('day');

                    // Add week number for visual clarity in monthly view
                    if (dayIndex === 0) { // Only on Sunday of each week
                        const weekNumberIndicator = document.createElement('div');
                        weekNumberIndicator.classList.add('week-number');
                        weekNumberIndicator.textContent = `Week ${weekDisplay + 1}`;
                        dayEl.appendChild(weekNumberIndicator);
                    }

                    const dayDateEl = document.createElement('div');
                    dayDateEl.classList.add('day-date');
                    dayDateEl.textContent = currentDay.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
                    dayEl.appendChild(dayDateEl);

                    if (dayIndex === 6) { // Saturday is blank + contains week-specific buttons
                        dayEl.classList.add('empty-day');
                        dayEl.innerHTML += '<p style="margin-top: 5px;">Saturday</p>';
                        dayEl.innerHTML += '<p>No Meals</p>';

                        // Add Grocery List and Preparation buttons for THIS week inside Saturday's box
                        const groceryButton = document.createElement('button');
                        groceryButton.classList.add('week-action-button');
                        groceryButton.textContent = 'Grocery (This Week)';
                        groceryButton.onclick = () => openWeeklyGroceryListModal(currentWeekMealsForGroceryAndPrep);
                        dayEl.appendChild(groceryButton);

                        const prepButton = document.createElement('button');
                        prepButton.classList.add('week-action-button');
                        prepButton.textContent = 'Preparation (This Week)';
                        prepButton.onclick = () => openWeeklyPreparationStepsModal(currentWeekMealsForGroceryAndPrep);
                        dayEl.appendChild(prepButton);

                    } else { // Sunday to Friday (Consumption Days)
                        const currentDayLunch = distributedLunchesConsumption[dayIndex];
                        const currentDayDinner = distributedDinnersConsumption[dayIndex];

                        // Add consumption meals to the current week's specific list
                        if (currentDayLunch) currentWeekMealsForGroceryAndPrep.lunches.push(currentDayLunch);
                        if (currentDayDinner) currentWeekMealsForGroceryAndPrep.dinners.push(currentDayDinner);

                        // Display consumption meals
                        if (currentDayLunch) {
                            const lunchEl = document.createElement('div');
                            lunchEl.classList.add('meal', 'lunch');
                            const mealEmoji = recipes[currentDayLunch]?.emoji || '';
                            lunchEl.innerHTML = `${mealEmoji} Lunch: ${currentDayLunch}`;
                            lunchEl.onclick = () => openRecipeModal(currentDayLunch);
                            dayEl.appendChild(lunchEl);
                        }

                        if (currentDayDinner) {
                            const dinnerEl = document.createElement('div');
                            dinnerEl.classList.add('meal', 'dinner');
                            const mealEmoji = recipes[currentDayDinner]?.emoji || '';
                            dinnerEl.innerHTML = `${mealEmoji} Dinner: ${currentDayDinner}`;
                            dinnerEl.onclick = () => openRecipeModal(currentDayDinner);
                            dayEl.appendChild(dinnerEl);
                        }

                        if (dayIndex === 0) { // Sunday - a prep day, but only showing consumption meals
                            dayEl.classList.add('prep-day');
                            const prepIndicator = document.createElement('div');
                            prepIndicator.classList.add('prep-indicator');
                            prepIndicator.textContent = 'Prep Day';
                            dayEl.appendChild(prepIndicator);
                        }
                    }
                    calendarDisplayEl.appendChild(dayEl);
                }
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

                openModal(`${recipe.emoji} ${recipeName}`, ingredientsHtml + instructionsHtml + macrosHtml);
            } else {
                openModal("Recipe Not Found", "<p>Sorry, the recipe for this meal is not available.</p>");
            }
        }

        // --- Weekly Grocery List Modal Content Generation ---
        function openWeeklyGroceryListModal(weekMeals) {
            const aggregatedIngredients = {
                proteins: new Set(),
                carbs: new Set(),
                vegetables: new Set(),
                pantry: new Set()
            };

            // Combine ingredients for all unique lunches and dinners of this specific week
            const uniqueMealsForWeek = new Set([...weekMeals.lunches, ...weekMeals.dinners]);

            uniqueMealsForWeek.forEach(mealName => {
                const recipe = recipes[mealName];
                if (recipe && recipe.ingredients) {
                    for (const category in recipe.ingredients) {
                        if (aggregatedIngredients[category]) { // Ensure category exists
                            recipe.ingredients[category].forEach(item => {
                                aggregatedIngredients[category].add(item);
                            });
                        }
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
                groceryHtml += '<li>No grocery items found for this week.</li>';
            }
            groceryHtml += '</ul>';

            openModal("🛒 Grocery List (This Week)", groceryHtml);
        }

        // --- Weekly Preparation Steps Modal Content Generation ---
        function openWeeklyPreparationStepsModal(weekMeals) {
            const uniqueMealsForWeek = new Set([...weekMeals.lunches, ...weekMeals.dinners]);

            let prepHtml = '';
            if (uniqueMealsForWeek.size === 0) {
                prepHtml = '<p>No specific preparation steps needed for this week.</p>';
            } else {
                prepHtml = '<h3>Preparation Steps for This Week:</h3><ol class="modal-list">';
                uniqueMealsForWeek.forEach(mealName => {
                    const recipe = recipes[mealName];
                    if (recipe && recipe.instructions && recipe.instructions.length > 0) {
                        prepHtml += `<li><strong>${recipes[mealName]?.emoji || ''} ${mealName}:</strong>`;
                        prepHtml += `<ul>${recipe.instructions.map(step => `<li>${step}</li>`).join('')}</ul>`;
                        prepHtml += `</li>`;
                    }
                });
                prepHtml += '</ol>';
            }

            openModal("👩‍🍳 Preparation Steps (This Week)", prepHtml);
        }

        // Initial generation on page load
        document.addEventListener('DOMContentLoaded', generateFullCalendar);
    </script>
</body>
</html>
