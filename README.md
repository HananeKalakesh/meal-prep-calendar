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
