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
    background: linear-gradient(135deg, #4A772F 0
