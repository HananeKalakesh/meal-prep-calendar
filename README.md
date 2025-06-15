<script>
    // Global variables
    let currentMonthIndex = 0; // Represents the current "month" in a cycle of 4
    const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
    
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
            // Sunday meals for Week 1 now conceptually come from Week 2's general meals
            // These will be dynamically assigned in generateCalendar
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
            // Sunday meals for Week 2 now conceptually come from Week 3's general meals
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
            // Sunday meals for Week 3 now conceptually come from Week 4's general meals
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
            // Sunday meals for Week 4 now conceptually come from Week 1's general meals
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
                "Add chicken strips and cook until browned and cooked through",
                "Add sliced bell peppers and onion, and sauté until tender-crisp",
                "Stir in fajita seasoning and cook for another minute",
                "Serve chicken and vegetable mixture over cooked rice, topped with black beans, corn, salsa, avocado, and lime wedges"
            ]
        },
        "Meatballs with Marinara": {
            ingredients: [
                "500g ground beef or turkey",
                "1/2 cup breadcrumbs",
                "1 egg",
                "1/4 cup milk",
                "2 tbsp fresh parsley, chopped",
                "1 clove garlic, minced",
                "1/2 tsp salt",
                "1/4 tsp black pepper",
                "1 tbsp olive oil",
                "1 jar (680g) marinara sauce",
                "Cooked pasta or zucchini noodles for serving"
            ],
            instructions: [
                "In a large bowl, gently combine ground meat, breadcrumbs, egg, milk, parsley, garlic, salt, and pepper",
                "Form into 1-inch meatballs",
                "Heat olive oil in a large skillet over medium heat",
                "Brown meatballs on all sides, then drain any excess fat",
                "Pour marinara sauce over the meatballs, bring to a simmer",
                "Reduce heat, cover, and cook for 15-20 minutes, or until meatballs are cooked through",
                "Serve hot over cooked pasta or zucchini noodles"
            ]
        },
        "Chicken Salad": {
            ingredients: [
                "2 cups cooked chicken, shredded or cubed",
                "1/2 cup mayonnaise",
                "1/4 cup celery, finely chopped",
                "2 tbsp red onion, finely chopped",
                "1 tbsp fresh dill or parsley, chopped (optional)",
                "1 tsp Dijon mustard",
                "Salt and black pepper to taste",
                "Lettuce leaves or bread for serving"
            ],
            instructions: [
                "In a medium bowl, combine the cooked chicken, mayonnaise, chopped celery, and red onion",
                "Stir in fresh dill or parsley (if using) and Dijon mustard",
                "Mix all ingredients until well combined",
                "Season with salt and black pepper to taste",
                "Serve chilled on lettuce leaves, as a sandwich, or with crackers"
            ]
        },
        "Chicken Curry": {
            ingredients: [
                "500g chicken breast, cubed",
                "1 tbsp olive oil",
                "1 onion, chopped",
                "2 cloves garlic, minced",
                "1 tbsp grated fresh ginger",
                "2 tbsp curry powder",
                "1 can (400ml) coconut milk",
                "1 cup chicken broth",
                "1/2 cup frozen peas",
                "Cooked basmati rice for serving"
            ],
            instructions: [
                "Heat olive oil in a large pot or deep skillet over medium heat",
                "Add chicken cubes and cook until lightly browned; remove and set aside",
                "In the same pot, add onion and cook until softened, about 5 minutes",
                "Stir in garlic, ginger, and curry powder; cook for 1 minute until fragrant",
                "Return chicken to the pot, pour in coconut milk and chicken broth",
                "Bring to a simmer, then reduce heat, cover, and cook for 15-20 minutes, or until chicken is cooked through",
                "Stir in frozen peas and cook for another 5 minutes",
                "Serve hot over basmati rice"
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
                "1 tsp sugar (optional, for balance)",
                "Salt and black pepper to taste"
            ],
            instructions: [
                "Dice cucumbers and bell peppers into bite-sized pieces",
                "Thinly slice the red onion and chop fresh dill",
                "In a large bowl, combine the diced vegetables, red onion, and dill",
                "In a small bowl, whisk together olive oil, white wine vinegar, sugar (if using), salt, and black pepper to create the dressing",
                "Pour the dressing over the vegetables and toss gently to ensure everything is evenly coated",
                "Serve immediately or chill in the refrigerator for at least 30 minutes to allow the flavors to meld"
            ]
        },
        "Mexican Burrito Bowl": {
            ingredients: [
                "1 cup cooked rice (brown or white)",
                "1 cup cooked black beans, rinsed and drained",
                "1 cup cooked corn kernels (fresh, frozen, or canned)",
                "1/2 cup salsa",
                "1/4 cup chopped fresh cilantro",
                "1/2 avocado, sliced or diced",
                "2 tbsp sour cream or Greek yogurt (optional)",
                "Lime wedges for serving",
                "Optional: cooked seasoned chicken, beef, or tofu"
            ],
            instructions: [
                "Assemble the bowl by layering cooked rice as the base",
                "Top with cooked black beans and corn kernels",
                "Add salsa, fresh cilantro, and avocado slices/dices",
                "If desired, add your choice of cooked and seasoned protein (chicken, beef, or tofu)",
                "Dollop with sour cream or Greek yogurt if using",
                "Serve immediately with a lime wedge for squeezing over the top"
            ]
        },
        "Burghul Banadoura": {
            ingredients: [
                "1 cup coarse bulgur",
                "2 tbsp olive oil",
                "1 large onion, finely chopped",
                "2 cloves garlic, minced",
                "1 can (800g) crushed tomatoes",
                "2 cups water or vegetable broth",
                "1 tsp dried mint (Na'na)",
                "Salt and black pepper to taste",
                "Fresh mint or parsley for garnish (optional)"
            ],
            instructions: [
                "Rinse bulgur thoroughly in a fine-mesh sieve, then drain well",
                "Heat olive oil in a large pot or deep skillet over medium heat",
                "Add finely chopped onion and cook until softened and translucent, about 5-7 minutes",
                "Add minced garlic and cook for another minute until fragrant",
                "Stir in the crushed tomatoes, water or vegetable broth, dried mint, salt, and black pepper",
                "Bring the mixture to a boil, then add the rinsed bulgur",
                "Stir well, reduce the heat to low, cover the pot, and simmer for 20-25 minutes, or until the bulgur has absorbed all the liquid and is tender",
                "Fluff with a fork before serving",
                "Garnish with fresh mint or parsley if desired"
            ]
        },
        "Teriyaki Chicken": {
            ingredients: [
                "500g chicken breast or thighs, cut into bite-sized pieces",
                "1 tbsp olive oil",
                "1/2 cup teriyaki sauce (store-bought or homemade)",
                "1 tbsp honey or brown sugar (optional, for extra glaze)",
                "1 cup broccoli florets",
                "1 bell pepper (any color), sliced",
                "Cooked rice for serving",
                "Sesame seeds and sliced green onions for garnish"
            ],
            instructions: [
                "Heat olive oil in a large skillet or wok over medium-high heat",
                "Add chicken pieces and stir-fry until cooked through and lightly browned",
                "Add broccoli florets and sliced bell pepper to the skillet, stir-fry for 3-5 minutes until tender-crisp",
                "Pour teriyaki sauce over the chicken and vegetables, stir to coat",
                "If desired, add honey or brown sugar and let the sauce simmer for 1-2 minutes until slightly thickened and glossy",
                "Serve hot over cooked rice, garnished with sesame seeds and sliced green onions"
            ]
        }
    };

    function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
    }

    // Function to get a random item from an array
    function getRandomMeal(mealList) {
        return mealList[Math.floor(Math.random() * mealList.length)];
    }

    // Function to generate the calendar
    function generateCalendar() {
        const calendarEl = document.getElementById('calendar');
        calendarEl.innerHTML = ''; // Clear previous calendar

        // Update the current month display to show "Month Cycle X" or similar, since actual months are less relevant now
        document.getElementById('currentMonth').textContent = `Meal Plan Cycle ${currentMonthIndex + 1}`;

        const weekLength = 7; // Days in a week

        // Get the current week's data
        const currentWeekKey = `week${(currentMonthIndex % 4) + 1}`;
        const currentWeekData = mealPlan[currentWeekKey];

        // Determine the next week's key for Sunday's meals
        const nextWeekIndex = (currentMonthIndex + 1) % 4; // Cycles 0 -> 1, 1 -> 2, 2 -> 3, 3 -> 0
        const nextWeekKey = `week${nextWeekIndex + 1}`;
        const nextWeekData = mealPlan[nextWeekKey];

        // Shuffle dinners for the *current* week's Monday-Saturday to add variety
        const shuffledDinnersCurrentWeek = [...currentWeekData.dinners];
        shuffleArray(shuffledDinnersCurrentWeek);

        // Shuffle lunches and dinners for the *next* week to pick Sunday's meals
        const shuffledLunchesNextWeek = [...nextWeekData.lunches];
        shuffleArray(shuffledLunchesNextWeek);
        const shuffledDinnersNextWeek = [...nextWeekData.dinners];
        shuffleArray(shuffledDinnersNextWeek);


        for (let w = 0; w < 4; w++) { // Loop for 4 weeks display
            // This outer loop is actually redundant for displaying *a single* 4-week cycle.
            // The logic below will generate a full 4-week grid based on currentMonthIndex.
            // Let's refactor this to simply build out the 4 weeks.

            // The original intent of 'currentMonthIndex' was to cycle through months.
            // With "Week X" display, we are always showing the full 4-week cycle
            // but the `currentMonthIndex` can still be used to offset the starting week
            // if we want to change which week is "Week 1" in the display.

            // For your request, we just need to render the 4 weeks,
            // and the `currentMonthIndex` acts as a "starting point" for the cycle.

            // Reset weekCounter for each full display generation
            let weekCounter = 1;
            calendarEl.innerHTML = ''; // Clear for fresh generation

            for (let weekDisplay = 0; weekDisplay < 4; weekDisplay++) { // Loop for 4 weeks to display
                // Calculate which mealPlan week corresponds to the current 'weekDisplay'
                // This creates the cycling effect for the 4 weeks
                const actualMealPlanWeekIndex = (currentMonthIndex + weekDisplay) % 4;
                const actualMealPlanWeekKey = `week${actualMealPlanWeekIndex + 1}`;
                const mealsForThisDisplayedWeek = mealPlan[actualMealPlanWeekKey];

                // Determine the *next* actual meal plan week for Sunday's meals
                const nextActualMealPlanWeekIndex = (actualMealPlanWeekIndex + 1) % 4;
                const nextActualMealPlanWeekKey = `week${nextActualMealPlanWeekIndex + 1}`;
                const mealsForNextDisplayedWeek = mealPlan[nextActualMealPlanWeekKey];


                // Shuffle dinners for this displayed week's Monday-Saturday
                const shuffledDinnersForMonSat = [...mealsForThisDisplayedWeek.dinners];
                shuffleArray(shuffledDinnersForMonSat);

                // Shuffle lunches and dinners from the *next* week for this week's Sunday
                const shuffledLunchesForSundayPrep = [...mealsForNextDisplayedWeek.lunches];
                shuffleArray(shuffledLunchesForSundayPrep);
                const shuffledDinnersForSundayPrep = [...mealsForNextDisplayedWeek.dinners];
                shuffleArray(shuffledDinnersForSundayPrep);

                for (let dayOfWeek = 0; dayOfWeek < weekLength; dayOfWeek++) { // Loop for 7 days
                    const dayEl = document.createElement('div');
                    dayEl.classList.add('day');

                    const dayNumberEl = document.createElement('div');
                    dayNumberEl.classList.add('day-number');

                    if (dayOfWeek === 0) { // Sunday
                        dayNumberEl.textContent = `Week ${weekDisplay + 1}`; // Display "Week 1", "Week 2", etc.
                        dayEl.classList.add('prep-day'); // Mark Sunday as prep day
                        const prepIndicator = document.createElement('div');
                        prepIndicator.classList.add('prep-indicator');
                        prepIndicator.textContent = 'Prep Day';
                        dayEl.appendChild(prepIndicator);

                        // **Sunday meals from the *next* actual meal plan week's general meals**
                        const sundayLunch = getRandomMeal(shuffledLunchesForSundayPrep);
                        const sundayDinner = getRandomMeal(shuffledDinnersForSundayPrep);

                        if (sundayLunch) {
                            const lunchEl = document.createElement('div');
                            lunchEl.classList.add('meal', 'lunch');
                            lunchEl.textContent = `Prep Lunch: ${sundayLunch}`; // Indicate it's for prep
                            lunchEl.onclick = () => openModal(sundayLunch);
                            dayEl.appendChild(lunchEl);
                        }

                        if (sundayDinner) {
                            const dinnerEl = document.createElement('div');
                            dinnerEl.classList.add('meal', 'dinner');
                            dinnerEl.textContent = `Prep Dinner: ${sundayDinner}`; // Indicate it's for prep
                            dinnerEl.onclick = () => openModal(sundayDinner);
                            dayEl.appendChild(dinnerEl);
                        }

                    } else { // Monday to Saturday
                        // Meals for Monday-Saturday come from the *current* actual meal plan week
                        const lunch = mealsForThisDisplayedWeek.lunches[(dayOfWeek - 1) % mealsForThisDisplayedWeek.lunches.length];
                        const dinner = shuffledDinnersForMonSat[(dayOfWeek - 1) % shuffledDinnersForMonSat.length];

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

                    calendarEl.appendChild(dayEl);
                }
            }
        }

    // Function to change which 'cycle' of weeks is currently viewed
    function changeMonth(direction) {
        currentMonthIndex = (currentMonthIndex + direction + 4) % 4; // Ensure positive modulo result
        generateCalendar();
    }

    // Modal functionality (remains the same)
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
            modal.style.display = 'block';
        } else {
            modalTitle.textContent = "Recipe Not Found";
            modalBody.innerHTML = "<p>Sorry, the recipe for this meal is not available.</p>";
            modal.style.display = 'block';
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

    // Initial calendar generation on page load
    document.addEventListener('DOMContentLoaded', generateCalendar);
</script>
