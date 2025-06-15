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
            max-width: 1200px; /* Increased max-width for better layout */
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            padding: 25px;
            margin-bottom: 30px; /* Space between calendar and bottom */
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
            background-color: var(--pastel-green-light); /* Light green for days */
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

        .day-name { /* Renamed from day-number for clarity */
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
        
        /* Week-specific tabs (Grocery/Prep) */
        .week-actions {
            display: flex;
            justify-content: center;
            margin-top: 20px;
            margin-bottom: 20px;
            gap: 15px;
        }

        .week-action-button {
            background-color: var(--pastel-green-medium);
            color: var(--text-color-dark);
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .week-action-button:hover {
            background-color: var(--pastel-green-dark);
            color: var(--text-color-light);
            transform: translateY(-2px);
        }

        .list-container {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.05);
            padding: 20px;
            margin-top: 15px;
            border: 1px solid var(--border-color);
        }

        .list-container h3 {
            color: var(--pastel-pink-dark);
            margin-top: 15px;
            margin-bottom: 10px;
            font-size: 1.3em;
            border-bottom: 1px dashed var(--pastel-pink-medium);
            padding-bottom: 5px;
        }

        .list-container ul, .list-container ol {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .list-container li {
            background-color: var(--pastel-pink-light);
            border-left: 4px solid var(--pastel-pink-dark);
            margin-bottom: 6px;
            padding: 8px 12px;
            border-radius: 4px;
            font-size: 0.95em;
        }
        .list-container li.category-header { /* Style for the category headers like "Proteins" */
            background-color: var(--pastel-green-light);
            border-left: 4px solid var(--pastel-green-dark);
            font-weight: bold;
            margin-top: 15px;
            margin-bottom: 8px;
            padding: 10px 12px;
        }

        /* Modal Styles */
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

        .ingredients-list, .instructions-list, .macros-list {
            list-style-type: none;
            padding: 0;
            margin-bottom: 20px;
        }

        .ingredients-list li, .instructions-list li, .macros-list li {
            padding: 8px 0;
            border-bottom: 1px dashed var(--pastel-green-medium);
        }

        .instructions-list li {
            border-bottom: 1px dashed var(--pastel-pink-medium);
        }

        .ingredients-list li:last-child, .instructions-list li:last-child, .macros-list li:last-child {
            border-bottom: none;
        }

        .modal-body ol li {
            margin-bottom: 10px;
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
            .week-actions {
                flex-direction: column;
                gap: 10px;
            }
            .week-action-button {
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

    <div class="main-container" id="mainContent">
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
        // Meal plan with specific meal types
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

        // Recipe database with ingredients (categorized) and instructions, and now MACROS
        const recipes = {
            "Kafta with Potatoes": {
                ingredients: {
                    proteins: ["500g ground lamb or beef"],
                    vegetables: ["4 medium potatoes, cubed", "1 large onion, finely chopped", "2 cloves garlic, minced", "1/4 cup fresh parsley, chopped"],
                    pantry: ["1 tsp allspice", "1 tsp cinnamon", "Salt and pepper to taste", "2 tbsp olive oil"]
                },
                instructions: ["Mix ground meat with half the onion, garlic, parsley, and spices.", "Form into small oval-shaped patties.", "Heat olive oil in a large pan over medium heat.", "Brown kafta patties on both sides, then remove.", "In same pan, sauté remaining onions until golden.", "Add potatoes and cook for 5 minutes.", "Return kafta to pan, add water to barely cover.", "Simmer covered for 25-30 minutes until potatoes are tender."],
                macros: { calories: "350 kcal", protein: "30g", carbs: "40g", fat: "10g" } // TODO: Add actual macro data here
            },
            "Lebanese Shawarma Bowl": {
                ingredients: {
                    proteins: ["500g chicken thighs, sliced thin"],
                    carbs: ["2 cups basmati rice"],
                    vegetables: ["1 cucumber, diced", "2 tomatoes, diced", "1/2 red onion, sliced", "Fresh parsley and mint"],
                    pantry: ["1/4 cup tahini", "2 tbsp lemon juice", "2 tsp shawarma spice blend"]
                },
                instructions: ["Marinate chicken in shawarma spices and lemon juice for 2 hours.", "Cook rice according to package instructions.", "Pan-fry chicken until golden and cooked through.", "Prepare vegetables and arrange in bowls.", "Mix tahini with lemon juice and water for sauce.", "Assemble bowls with rice, chicken, vegetables, and sauce."],
                macros: { calories: "450 kcal", protein: "40g", carbs: "50g", fat: "15g" } // TODO: Add actual macro data here
            },
            "Caesar Salad": {
                ingredients: {
                    proteins: ["4 anchovy fillets (optional)", "1 egg yolk"],
                    vegetables: ["2 large romaine lettuce heads", "2 cloves garlic"],
                    pantry: ["1/2 cup parmesan cheese, grated", "1 cup croutons", "1/4 cup lemon juice", "1/4 cup olive oil", "1 tsp Dijon mustard"]
                },
                instructions: ["Wash and chop romaine lettuce.", "Make dressing: blend garlic, anchovies, lemon juice, egg yolk, and mustard.", "Slowly add olive oil while blending.", "Toss lettuce with dressing.", "Top with parmesan and croutons.", "Serve immediately."],
                macros: { calories: "280 kcal", protein: "15g", carbs: "20g", fat: "18g" } // TODO: Add actual macro data here
            },
            "Chickpea Salad": {
                ingredients: {
                    proteins: [],
                    carbs: ["2 cans chickpeas, drained and rinsed"],
                    vegetables: ["1 cucumber, diced", "2 tomatoes, diced", "1/2 red onion, finely chopped", "1/4 cup fresh parsley"],
                    pantry: ["3 tbsp olive oil", "2 tbsp lemon juice", "1 tsp sumac", "Salt and pepper to taste"]
                },
                instructions: ["Drain and rinse chickpeas thoroughly.", "Dice all vegetables finely.", "Chop parsley and mint.", "Whisk together olive oil, lemon juice, and sumac.", "Combine all ingredients in a large bowl.", "Toss well and let marinate for 30 minutes.", "Adjust seasoning before serving."],
                macros: { calories: "250 kcal", protein: "10g", carbs: "35g", fat: "10g" } // TODO: Add actual macro data here
            },
            "Chicken with Roasted Veggies": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["2 bell peppers, chopped", "1 zucchini, chopped", "1 red onion, chopped"],
                    pantry: ["2 tbsp olive oil", "1 tsp dried oregano", "Salt and pepper to taste"]
                },
                instructions: ["Preheat oven to 200°C (400°F).", "Toss chicken and vegetables with olive oil, oregano, salt, and pepper.", "Spread on a baking sheet in a single layer.", "Roast for 20-25 minutes, or until chicken is cooked through and vegetables are tender-crisp.", "Serve hot."],
                macros: { calories: "320 kcal", protein: "35g", carbs: "20g", fat: "12g" } // TODO: Add actual macro data here
            },
            "Mediterranean Quinoa Salad": {
                ingredients: {
                    proteins: ["1/4 cup feta cheese, crumbled"],
                    carbs: ["1 cup quinoa, rinsed"],
                    vegetables: ["1 cucumber, diced", "1 cup cherry tomatoes, halved", "1/2 red onion, finely chopped", "1/2 cup Kalamata olives, pitted and halved", "1/4 cup fresh parsley, chopped", "1/4 cup fresh mint, chopped"],
                    pantry: ["2 cups vegetable broth or water", "3 tbsp olive oil", "2 tbsp lemon juice", "Salt and pepper to taste"]
                },
                instructions: ["Cook quinoa according to package directions using broth/water.", "Let cool completely.", "In a large bowl, combine cooked quinoa, cucumber, cherry tomatoes, red onion, olives, parsley, and mint.", "Whisk together olive oil, lemon juice, salt, and pepper.", "Pour dressing over salad and toss to combine.", "Stir in crumbled feta cheese just before serving."],
                macros: { calories: "380 kcal", protein: "12g", carbs: "45g", fat: "18g" } // TODO: Add actual macro data here
            },
            "Asian Chicken Salad": {
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded"],
                    vegetables: ["4 cups shredded cabbage (green or napa)", "1 cup shredded carrots", "1/2 red bell pepper, thinly sliced", "1/4 cup chopped green onions", "1/4 cup chopped cilantro"],
                    pantry: ["1/4 cup rice vinegar", "2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp honey or maple syrup", "1 tsp grated fresh ginger", "1/2 cup chopped peanuts or cashews (optional)"]
                },
                instructions: ["In a large bowl, combine shredded chicken, cabbage, carrots, red bell pepper, green onions, and cilantro.", "In a small bowl, whisk together rice vinegar, soy sauce, sesame oil, honey, and ginger until well combined.", "Pour dressing over the salad ingredients and toss to coat evenly.", "If desired, add chopped peanuts or cashews for crunch.", "Serve immediately or chill for later."],
                macros: { calories: "300 kcal", protein: "25g", carbs: "25g", fat: "12g" } // TODO: Add actual macro data here
            },
            "Shrimp Orzo": {
                ingredients: {
                    proteins: ["400g raw shrimp, peeled and deveined"],
                    carbs: ["1 cup orzo pasta"],
                    vegetables: ["1 onion, finely chopped", "2 cloves garlic, minced", "1/4 cup fresh parsley, chopped"],
                    pantry: ["2 tbsp olive oil", "1 can (400g) diced tomatoes", "1/2 cup white wine (optional)", "2 cups vegetable or chicken broth", "Salt and pepper to taste"]
                },
                instructions: ["Heat olive oil in a large skillet over medium heat.", "Add onion and cook until softened, about 5 minutes.", "Add garlic and cook for 1 minute more until fragrant.", "Stir in orzo and toast for 2 minutes.", "Pour in white wine (if using) and cook until absorbed.", "Add diced tomatoes and broth, bring to a simmer.", "Cover and cook for 10-12 minutes, stirring occasionally, until orzo is almost cooked.", "Stir in shrimp and cook for 3-5 minutes until pink and cooked through.", "Season with salt and pepper, stir in fresh parsley before serving."],
                macros: { calories: "380 kcal", protein: "25g", carbs: "45g", fat: "10g" } // TODO: Add actual macro data here
            },
            "Korean Beef Bowl": {
                ingredients: {
                    proteins: ["500g beef sirloin or flank steak, thinly sliced"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1/2 red bell pepper, sliced", "1 cup broccoli florets", "Sliced green onions for garnish"],
                    pantry: ["2 tbsp soy sauce", "1 tbsp sesame oil", "1 tbsp brown sugar", "2 cloves garlic, minced", "1 tsp grated fresh ginger", "Sesame seeds for garnish"]
                },
                instructions: ["In a bowl, combine soy sauce, sesame oil, brown sugar, garlic, and ginger to make the marinade.", "Add sliced beef to the marinade and toss to coat; let marinate for at least 30 minutes.", "Heat a large skillet or wok over medium-high heat.", "Add marinated beef and stir-fry until cooked through and slightly caramelized.", "Add red bell pepper and broccoli, stir-fry for 3-5 minutes until tender-crisp.", "Serve hot over cooked rice, garnished with sesame seeds and green onions."],
                macros: { calories: "420 kcal", protein: "38g", carbs: "40g", fat: "15g" } // TODO: Add actual macro data here
            },
            "Sweet and Sour Chicken": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 bell pepper (any color), chopped", "1 onion, chopped"],
                    pantry: ["1 can (227g) pineapple chunks, drained (reserve juice)", "2 tbsp cornstarch", "1/4 cup white vinegar", "1/4 cup sugar", "2 tbsp soy sauce", "1 tbsp ketchup", "1/2 cup chicken broth", "2 tbsp oil for cooking"]
                },
                instructions: ["Toss chicken cubes with 1 tbsp cornstarch.", "Heat oil in a large skillet or wok over medium-high heat.", "Stir-fry chicken until lightly browned and cooked through, remove from pan.", "In the same pan, add bell pepper and onion, stir-fry for 3-4 minutes until slightly softened.", "In a small bowl, whisk together remaining cornstarch with vinegar, sugar, soy sauce, ketchup, pineapple juice, and chicken broth.", "Pour sauce into the skillet, bring to a simmer, stirring until thickened.", "Return chicken to the pan along with pineapple chunks.", "Toss to coat everything in the sauce.", "Serve hot with rice or noodles."],
                macros: { calories: "360 kcal", protein: "30g", carbs: "40g", fat: "10g" } // TODO: Add actual macro data here
            },
            "Tuna Salad": {
                ingredients: {
                    proteins: ["2 cans (140g each) tuna in water, drained"],
                    vegetables: ["1 stalk celery, finely chopped", "1/4 cup red onion, finely chopped", "1 tbsp fresh dill, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise or Greek yogurt", "1 tbsp Dijon mustard", "Salt and black pepper to taste"]
                },
                instructions: ["In a medium bowl, flake the drained tuna with a fork.", "Add mayonnaise (or Greek yogurt), chopped celery, red onion, dill (if using), and Dijon mustard.", "Mix well to combine all ingredients.", "Season with salt and black pepper to taste.", "Serve as a sandwich filling, with crackers, or on a bed of lettuce."],
                macros: { calories: "220 kcal", protein: "20g", carbs: "5g", fat: "15g" } // TODO: Add actual macro data here
            },
            "Mediterranean Orzo Salad": {
                ingredients: {
                    proteins: ["1/4 cup feta cheese, crumbled"],
                    carbs: ["1 cup orzo pasta, cooked and cooled"],
                    vegetables: ["1 cup cherry tomatoes, halved", "1 cucumber, diced", "1/2 red onion, thinly sliced", "1/2 cup Kalamata olives, pitted and halved", "1/4 cup fresh parsley, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp red wine vinegar", "1 tsp dried oregano", "Salt and pepper to taste"]
                },
                instructions: ["Cook orzo pasta according to package directions; drain and rinse with cold water to cool, then set aside.", "In a large bowl, combine the cooled orzo, cherry tomatoes, cucumber, red onion, olives, and parsley.", "In a small bowl, whisk together olive oil, red wine vinegar, oregano, salt, and pepper to make the dressing.", "Pour the dressing over the orzo mixture and toss gently to coat all ingredients.", "Stir in the crumbled feta cheese.", "Serve immediately or chill for flavors to meld."],
                macros: { calories: "350 kcal", protein: "10g", carbs: "40g", fat: "16g" } // TODO: Add actual macro data here
            },
            "Quinoa Halloumi Salad": {
                ingredients: {
                    proteins: ["250g halloumi cheese, sliced or cubed"],
                    carbs: ["1 cup quinoa, rinsed"],
                    vegetables: ["1 red bell pepper, chopped", "1 yellow bell pepper, chopped", "1/2 red onion, thinly sliced", "1/4 cup fresh mint, chopped", "1/4 cup fresh parsley, chopped"],
                    pantry: ["2 cups water or vegetable broth", "2 tbsp olive oil", "3 tbsp lemon juice", "Salt and pepper to taste"]
                },
                instructions: ["Cook quinoa according to package directions with water or broth; let cool.", "Heat 1 tbsp olive oil in a non-stick skillet over medium heat.", "Add halloumi cheese and cook until golden brown on both sides, about 2-3 minutes per side; remove and set aside.", "In a large bowl, combine cooled quinoa, roasted bell peppers, red onion, mint, and parsley.", "Whisk together remaining olive oil, lemon juice, salt, and pepper for the dressing.", "Pour dressing over the salad and toss well.", "Gently fold in the cooked halloumi cheese.", "Serve warm or at room temperature."],
                macros: { calories: "400 kcal", protein: "20g", carbs: "35g", fat: "20g" } // TODO: Add actual macro data here
            },
            "Bazella with Carrots": {
                ingredients: {
                    proteins: ["500g ground beef or lamb (optional)"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "4 cups frozen peas and carrots mix"],
                    pantry: ["1 tbsp olive oil", "1 can (400g) crushed tomatoes", "1 cup water or beef broth", "1 tsp seven spice (baharat)", "Salt and pepper to taste", "Cooked rice for serving"]
                },
                instructions: ["If using meat, brown ground beef/lamb in olive oil in a large pot; drain excess fat and set aside.", "In the same pot, sauté onion until softened, then add garlic and cook for 1 minute more.", "Stir in crushed tomatoes, peas and carrots, water/broth, and seven spice.", "If using meat, return it to the pot.", "Bring to a simmer, then reduce heat, cover, and cook for 20-25 minutes, or until vegetables are tender.", "Season with salt and pepper to taste.", "Serve hot with rice."],
                macros: { calories: "380 kcal", protein: "28g", carbs: "35g", fat: "14g" } // TODO: Add actual macro data here
            },
            "Chicken Fajita Bowl": {
                ingredients: {
                    proteins: ["500g chicken breast, sliced into strips"],
                    carbs: ["1 cup cooked rice", "1/2 cup black beans, rinsed and drained"],
                    vegetables: ["2 bell peppers (different colors), sliced", "1 onion, sliced", "1/4 cup corn kernels (frozen or canned)", "Salsa", "Avocado slices"],
                    pantry: ["2 tbsp olive oil", "2 tsp fajita seasoning", "Lime wedges for serving"]
                },
                instructions: ["Heat olive oil in a large skillet over medium-high heat.", "Add chicken strips and cook until browned and cooked through.", "Add sliced bell peppers and onion, and sauté until tender-crisp.", "Stir in fajita seasoning and cook for another minute.", "Serve chicken and vegetable mixture over cooked rice, topped with black beans, corn, salsa, avocado, and lime wedges."],
                macros: { calories: "450 kcal", protein: "40g", carbs: "50g", fat: "15g" } // TODO: Add actual macro data here
            },
            "Meatballs with Marinara": {
                ingredients: {
                    proteins: ["500g ground beef or turkey", "1 egg"],
                    carbs: ["1/2 cup breadcrumbs"],
                    vegetables: ["2 tbsp fresh parsley, chopped", "1 clove garlic, minced"],
                    pantry: ["1/4 cup milk", "1/2 tsp salt", "1/4 tsp black pepper", "1 tbsp olive oil", "1 jar (680g) marinara sauce", "Cooked pasta or zucchini noodles for serving"]
                },
                instructions: ["In a large bowl, gently combine ground meat, breadcrumbs, egg, milk, parsley, garlic, salt, and pepper.", "Form into 1-inch meatballs.", "Heat olive oil in a large skillet over medium heat.", "Brown meatballs on all sides, then drain any excess fat.", "Pour marinara sauce over the meatballs, bring to a simmer.", "Reduce heat, cover, and cook for 15-20 minutes, or until meatballs are cooked through.", "Serve hot over cooked pasta or zucchini noodles."],
                macros: { calories: "420 kcal", protein: "35g", carbs: "30g", fat: "20g" } // TODO: Add actual macro data here
            },
            "Chicken Salad": {
                ingredients: {
                    proteins: ["2 cups cooked chicken, shredded or cubed"],
                    vegetables: ["1/4 cup celery, finely chopped", "2 tbsp red onion, finely chopped", "1 tbsp fresh dill or parsley, chopped (optional)"],
                    pantry: ["1/2 cup mayonnaise", "1 tsp Dijon mustard", "Salt and black pepper to taste", "Lettuce leaves or bread for serving"]
                },
                instructions: ["In a medium bowl, combine the cooked chicken, mayonnaise, chopped celery, and red onion.", "Stir in fresh dill or parsley (if using) and Dijon mustard.", "Mix all ingredients until well combined.", "Season with salt and black pepper to taste.", "Serve chilled on lettuce leaves, as a sandwich, or with crackers."],
                macros: { calories: "280 kcal", protein: "25g", carbs: "5g", fat: "18g" } // TODO: Add actual macro data here
            },
            "Chicken Curry": {
                ingredients: {
                    proteins: ["500g chicken breast, cubed"],
                    vegetables: ["1 onion, chopped", "2 cloves garlic, minced", "1/2 cup frozen peas"],
                    pantry: ["1 tbsp olive oil", "1 tbsp grated fresh ginger", "2 tbsp curry powder", "1 can (400ml) coconut milk", "1 cup chicken broth", "Cooked basmati rice for serving"]
                },
                instructions: ["Heat olive oil in a large pot or deep skillet over medium heat.", "Add chicken cubes and cook until lightly browned; remove and set aside.", "In the same pot, add onion and cook until softened, about 5 minutes.", "Stir in garlic, ginger, and curry powder; cook for 1 minute until fragrant.", "Return chicken to the pot.", "Pour in coconut milk and chicken broth.", "Bring to a simmer, then reduce heat, cover, and cook for 15-20 minutes, or until chicken is cooked through.", "Stir in frozen peas and cook for another 5 minutes.", "Serve hot over basmati rice."],
                macros: { calories: "400 kcal", protein: "35g", carbs: "30g", fat: "18g" } // TODO: Add actual macro data here
            },
            "Pepper and Cucumber Salad": {
                ingredients: {
                    proteins: [],
                    vegetables: ["2 cucumbers, diced", "2 bell peppers (any color), diced", "1/2 red onion, thinly sliced", "1/4 cup fresh dill, chopped"],
                    pantry: ["3 tbsp olive oil", "2 tbsp white wine vinegar", "1 tsp sugar (optional, for balance)", "Salt and black pepper to taste"]
                },
                instructions: ["Dice cucumbers and bell peppers into bite-sized pieces.", "Thinly slice the red onion and chop fresh dill.", "In a large bowl, combine the diced vegetables, red onion, and dill.", "In a small bowl, whisk together olive oil, white wine vinegar, sugar (if using), salt, and black pepper to create the dressing.", "Pour the dressing over the vegetables and toss gently to ensure everything is evenly coated.", "Serve immediately or chill in the refrigerator for at least 30 minutes to allow the flavors to meld."],
                macros: { calories: "180 kcal", protein: "2g", carbs: "15g", fat: "12g" } // TODO: Add actual macro data here
            },
            "Mexican Burrito Bowl": {
                ingredients: {
                    proteins: ["Optional: cooked seasoned chicken, beef, or tofu"],
                    carbs: ["1 cup cooked rice (brown or white)", "1 cup cooked black beans, rinsed and drained"],
                    vegetables: ["1 cup cooked corn kernels (fresh, frozen, or canned)", "1/2 cup salsa", "1/4 cup chopped fresh cilantro", "1/2 avocado, sliced or diced", "Lime wedges for serving"],
                    pantry: ["2 tbsp sour cream or Greek yogurt (optional)"]
                },
                instructions: ["Assemble the bowl by layering cooked rice as the base.", "Top with cooked black beans and corn kernels.", "Add salsa, fresh cilantro, and avocado slices/dices.", "If desired, add your choice of cooked and seasoned protein (chicken, beef, or tofu).", "Dollop with sour cream or Greek yogurt if using.", "Serve immediately with a lime wedge for squeezing over the top."],
                macros: { calories: "480 kcal", protein: "15g", carbs: "60g", fat: "20g" } // TODO: Add actual macro data here
            },
            "Teriyaki Chicken": {
                ingredients: {
                    proteins: ["500g chicken breast or thighs, cut into bite-sized pieces"],
                    carbs: ["Cooked rice for serving"],
                    vegetables: ["1 cup broccoli florets", "1 bell pepper (any color), sliced", "Sliced green onions for garnish"],
                    pantry: ["1 tbsp olive oil", "1/2 cup teriyaki sauce (store-bought or homemade)", "1 tbsp honey or brown sugar (optional, for extra glaze)", "Sesame seeds for garnish"]
                },
                instructions: ["Heat olive oil in a large skillet or wok over medium-high heat.", "Add chicken pieces and stir-fry until cooked through and lightly browned.", "Add broccoli florets and sliced bell pepper to the skillet, stir-fry for 3-5 minutes until tender-crisp.", "Pour teriyaki sauce over the chicken and vegetables, stir to coat.", "If desired, add honey or brown sugar and let the sauce simmer for 1-2 minutes until slightly thickened and glossy.", "Serve hot over cooked rice, garnished with sesame seeds and sliced green onions."],
                macros: { calories: "390 kcal", protein: "35g", carbs: "40g", fat: "12g" } // TODO: Add actual macro data here
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

        // Function to distribute meals for Sunday-Friday consumption
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

        // Main function to generate the entire calendar with weeks and controls
        function generateFullCalendar() {
            const mainContentEl = document.getElementById('mainContent');
            mainContentEl.innerHTML = ''; // Clear previous content

            const dayNamesFull = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

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

                // Get meal data for the *current* consumption week
                const currentConsumptionWeekKey = `week${(weekDisplay % 4) + 1}`;
                const currentConsumptionWeekData = mealPlan[currentConsumptionWeekKey];
                
                // Get meal data for the *next* week's prep (prepped on Sunday of THIS week)
                const nextPrepWeekKey = `week${((weekDisplay + 1) % 4) + 1}`;
                const nextPrepWeekData = mealPlan[nextPrepWeekKey];

                const distributedLunchesConsumption = distributeMealsForConsumption(currentConsumptionWeekData.lunches, 'lunches');
                const distributedDinnersConsumption = distributeMealsForConsumption(currentConsumptionWeekData.dinners, 'dinners');

                // Random meals for Sunday prep for the *next* week's consumption
                const sundayPrepLunch = getRandomMeal(nextPrepWeekData.lunches);
                const sundayPrepDinner = getRandomMeal(nextPrepWeekData.dinners);

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
                        // dayEl.innerHTML += '<p>Saturday - No Meals</p>'; // Optional text to indicate blank
                    } else { // Sunday to Friday (Consumption Days)
                        // All consumption meals (Sun-Fri) come from the 'distributedLunchesConsumption' and 'distributedDinnersConsumption' arrays.
                        // These arrays are 0-indexed for Sunday-Friday, matching the dayIndex here directly.
                        const lunch = distributedLunchesConsumption[dayIndex];
                        const dinner = distributedDinnersConsumption[dayIndex];

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

                        if (dayIndex === 0) { // Sunday - Also a Prep Day for the *next* week
                            dayEl.classList.add('prep-day');
                            const prepIndicator = document.createElement('div');
                            prepIndicator.classList.add('prep-indicator');
                            prepIndicator.textContent = 'Prep Day';
                            dayEl.appendChild(prepIndicator);

                            // Add the "Prep" meals (for next week's consumption) to Sunday's display
                            if (sundayPrepLunch) {
                                const prepLunchEl = document.createElement('div');
                                prepLunchEl.classList.add('meal', 'lunch'); // Style as lunch, but indicate prep
                                prepLunchEl.textContent = `Prep Lunch: ${sundayPrepLunch}`;
                                prepLunchEl.onclick = () => openModal(sundayPrepLunch);
                                dayEl.appendChild(prepLunchEl);
                            }
                            if (sundayPrepDinner) {
                                const prepDinnerEl = document.createElement('div');
                                prepDinnerEl.classList.add('meal', 'dinner'); // Style as dinner, but indicate prep
                                prepDinnerEl.textContent = `Prep Dinner: ${sundayPrepDinner}`;
                                prepDinnerEl.onclick = () => openModal(sundayPrepDinner);
                                dayEl.appendChild(prepDinnerEl);
                            }
                        }
                    }
                    weekCalendarGrid.appendChild(dayEl);
                }
                weekSection.appendChild(weekCalendarGrid);

                // Add Grocery List and Preparation buttons for the week
                const weekActionsDiv = document.createElement('div');
                weekActionsDiv.classList.add('week-actions');

                const groceryButton = document.createElement('button');
                groceryButton.classList.add('week-action-button');
                groceryButton.textContent = 'View Grocery List';
                groceryButton.onclick = () => toggleGroceryList(weekDisplay, weekActionsDiv);
                weekActionsDiv.appendChild(groceryButton);

                const prepButton = document.createElement('button');
                prepButton.classList.add('week-action-button');
                prepButton.textContent = 'View Preparations';
                prepButton.onclick = () => togglePreparationSteps(weekDisplay, weekActionsDiv);
                weekActionsDiv.appendChild(prepButton);

                weekSection.appendChild(weekActionsDiv);

                // Add placeholder for grocery list and prep steps (initially hidden)
                const groceryListContainer = document.createElement('div');
                groceryListContainer.id = `groceryList-${weekDisplay}`;
                groceryListContainer.classList.add('list-container');
                groceryListContainer.style.display = 'none'; // Hidden by default
                weekSection.appendChild(groceryListContainer);

                const preparationStepsContainer = document.createElement('div');
                preparationStepsContainer.id = `preparationSteps-${weekDisplay}`;
                preparationStepsContainer.classList.add('list-container');
                preparationStepsContainer.style.display = 'none'; // Hidden by default
                weekSection.appendChild(preparationStepsContainer);

                mainContentEl.appendChild(weekSection);
            }
        }

        // Toggle Grocery List visibility and content
        function toggleGroceryList(weekIndex, parentElement) {
            const listContainer = document.getElementById(`groceryList-${weekIndex}`);
            const prepContainer = document.getElementById(`preparationSteps-${weekIndex}`);

            // Hide the other list container for this week if open
            prepContainer.style.display = 'none';

            if (listContainer.style.display === 'none') {
                listContainer.style.display = 'block';
                generateGroceryList(weekIndex); // Generate content when shown
            } else {
                listContainer.style.display = 'none';
            }
        }

        // Toggle Preparation Steps visibility and content
        function togglePreparationSteps(weekIndex, parentElement) {
            const listContainer = document.getElementById(`preparationSteps-${weekIndex}`);
            const groceryContainer = document.getElementById(`groceryList-${weekIndex}`);

            // Hide the other list container for this week if open
            groceryContainer.style.display = 'none';

            if (listContainer.style.display === 'none') {
                listContainer.style.display = 'block';
                generatePreparationSteps(weekIndex); // Generate content when shown
            } else {
                listContainer.style.display = 'none';
            }
        }

        // Function to generate the grocery list for a specific week
        function generateGroceryList(weekIndex) {
            const groceryListEl = document.getElementById(`groceryList-${weekIndex}`);
            groceryListEl.innerHTML = ''; // Clear previous content

            // Get the current week's data for *consumption*
            const currentConsumptionWeekKey = `week${(weekIndex % 4) + 1}`;
            const currentConsumptionWeekData = mealPlan[currentConsumptionWeekKey];
            
            // Get the NEXT week's data for *prep* (which is prepped on Sunday of the current week)
            const nextPrepWeekKey = `week${((weekIndex + 1) % 4) + 1}`;
            const nextPrepWeekData = mealPlan[nextPrepWeekKey];

            // Meals that will be CONSUMED this week (Sunday-Friday)
            const consumedLunches = distributeMealsForConsumption(currentConsumptionWeekData.lunches, 'lunches');
            const consumedDinners = distributeMealsForConsumption(currentConsumptionWeekData.dinners, 'dinners');
            
            // Meals that will be PREPPED on Sunday of this week (for next week's consumption)
            const sundayPrepLunch = getRandomMeal(nextPrepWeekData.lunches);
            const sundayPrepDinner = getRandomMeal(nextPrepWeekData.dinners);

            // Combine all unique meals whose ingredients are needed for this week's prep/consumption
            const allMealsForIngredients = new Set();
            consumedLunches.forEach(meal => allMealsForIngredients.add(meal));
            consumedDinners.forEach(meal => allMealsForIngredients.add(meal));
            if (sundayPrepLunch) allMealsForIngredients.add(sundayPrepLunch);
            if (sundayPrepDinner) allMealsForIngredients.add(sundayPrepDinner);

            const categorizedIngredients = {
                proteins: new Set(),
                carbs: new Set(),
                vegetables: new Set(),
                pantry: new Set()
            };

            allMealsForIngredients.forEach(mealName => {
                const recipe = recipes[mealName];
                if (recipe && recipe.ingredients) {
                    for (const category in recipe.ingredients) {
                        recipe.ingredients[category].forEach(item => {
                            categorizedIngredients[category].add(item);
                        });
                    }
                }
            });

            const categoriesOrder = ['proteins', 'carbs', 'vegetables', 'pantry'];
            let contentAdded = false;

            categoriesOrder.forEach(category => {
                if (categorizedIngredients[category].size > 0) {
                    const categoryHeader = document.createElement('li');
                    categoryHeader.classList.add('category-header');
                    categoryHeader.textContent = category.charAt(0).toUpperCase() + category.slice(1);
                    groceryListEl.appendChild(categoryHeader);

                    const ul = document.createElement('ul');
                    Array.from(categorizedIngredients[category]).sort().forEach(item => {
                        const li = document.createElement('li');
                        li.textContent = item;
                        ul.appendChild(li);
                    });
                    groceryListEl.appendChild(ul);
                    contentAdded = true;
                }
            });

            if (!contentAdded) {
                const li = document.createElement('li');
                li.textContent = "No grocery items found for this week.";
                groceryListEl.appendChild(li);
            }
        }

        // Function to generate preparation steps for a specific week
        function generatePreparationSteps(weekIndex) {
            const preparationStepsEl = document.getElementById(`preparationSteps-${weekIndex}`);
            preparationStepsEl.innerHTML = ''; // Clear previous content

            // Get the NEXT week's data for *prep* (which is prepped on Sunday of the current week)
            const nextPrepWeekKey = `week${((weekIndex + 1) % 4) + 1}`;
            const nextPrepWeekData = mealPlan[nextPrepWeekKey];

            // Meals that will be PREPPED on Sunday of this week (for next week's consumption)
            const sundayPrepLunch = getRandomMeal(nextPrepWeekData.lunches);
            const sundayPrepDinner = getRandomMeal(nextPrepWeekData.dinners);
            
            const allPreppedMeals = new Set();
            if (sundayPrepLunch) allPreppedMeals.add(sundayPrepLunch);
            if (sundayPrepDinner) allPreppedMeals.add(sundayPrepDinner);

            if (allPreppedMeals.size === 0) {
                const li = document.createElement('li');
                li.textContent = "No specific prep steps needed for this week's Sunday prep.";
                preparationStepsEl.appendChild(li);
            } else {
                const ol = document.createElement('ol');
                allPreppedMeals.forEach(mealName => {
                    const recipe = recipes[mealName];
                    if (recipe && recipe.instructions) {
                        const liTitle = document.createElement('li');
                        liTitle.innerHTML = `<strong>${mealName}:</strong>`;
                        ol.appendChild(liTitle);
                        const ulInstructions = document.createElement('ul');
                        recipe.instructions.forEach(instruction => {
                            const li = document.createElement('li');
                            li.textContent = instruction;
                            ulInstructions.appendChild(li);
                        });
                        ol.appendChild(ulInstructions);
                    }
                });
                preparationStepsEl.appendChild(ol);
            }
        }


        // Modal functionality
        function openModal(recipeName) {
            const modal = document.getElementById('recipeModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            const recipe = recipes[recipeName];

            if (recipe) {
                modalTitle.textContent = recipeName;
                let ingredientsHtml = '';
                if (recipe.ingredients) {
                    ingredientsHtml += '<h3>Ingredients:</h3><ul class="ingredients-list">';
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
                }

                let instructionsHtml = '';
                if (recipe.instructions && recipe.instructions.length > 0) {
                    instructionsHtml = `
                        <h3>Instructions:</h3>
                        <ol class="instructions-list">
                            ${recipe.instructions.map(step => `<li>${step}</li>`).join('')}
                        </ol>
                    `;
                }

                let macrosHtml = '';
                if (recipe.macros) {
                    macrosHtml = `
                        <h3>Macros:</h3>
                        <ul class="macros-list">
                            <li>Calories: ${recipe.macros.calories || 'N/A'}</li>
                            <li>Protein: ${recipe.macros.protein || 'N/A'}</li>
                            <li>Carbs: ${recipe.macros.carbs || 'N/A'}</li>
                            <li>Fat: ${recipe.macros.fat || 'N/A'}</li>
                        </ul>
                    `;
                }

                modalBody.innerHTML = ingredientsHtml + instructionsHtml + macrosHtml;
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
            generateFullCalendar();
        });
    </script>
</body>
</html>
