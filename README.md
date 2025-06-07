<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurant Recommender AI</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #000000, #1a1a1a); /* Black to very dark grey gradient */
            color: #f0f0f0; /* Off-white text */
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden; /* Prevents scrollbars */
            position: relative;
        }

        /* Subtle futuristic background elements - adjusted for monochrome */
        body::before, body::after {
            content: '';
            position: absolute;
            border-radius: 50%;
            opacity: 0.1; /* Reduced opacity for subtlety */
            filter: blur(80px);
            z-index: -1;
        }

        body::before {
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, #4a4a4a, transparent 50%); /* Darker grey glow */
            top: -100px;
            left: -100px;
            animation: moveGlow1 20s infinite alternate;
        }

        body::after {
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, #8a8a8a, transparent 50%); /* Lighter grey glow */
            bottom: -100px;
            right: -100px;
            animation: moveGlow2 25s infinite alternate;
        }

        @keyframes moveGlow1 {
            0% { transform: translate(0, 0) scale(1); }
            50% { transform: translate(50px, 50px) scale(1.05); }
            100% { transform: translate(0, 0) scale(1); }
        }

        @keyframes moveGlow2 {
            0% { transform: translate(0, 0) scale(1); }
            50% { transform: translate(-50px, -50px) scale(1.05); }
            100% { transform: translate(0, 0) scale(1); }
        }

        .screen {
            background: rgba(255, 255, 255, 0.05); /* Slightly translucent white */
            backdrop-filter: blur(10px); /* Frosted glass effect */
            -webkit-backdrop-filter: blur(10px);
            border-radius: 1.5rem; /* More rounded corners */
            padding: 2.5rem;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5), inset 0 0 20px rgba(255, 255, 255, 0.1); /* Darker shadow */
            width: 90vw; /* Responsive width */
            max-width: 800px; /* Max width for larger screens */
            min-height: 70vh; /* Ensure content space */
            display: none; /* Hidden by default */
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.5s ease-out, transform 0.5s ease-out;
            border: 1px solid rgba(255, 255, 255, 0.1); /* Subtle white border */
        }

        .screen.active {
            display: flex;
            opacity: 1;
            transform: translateY(0);
        }

        h1, h2 {
            font-weight: 700;
            color: #ffffff; /* Pure white for headings */
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.4); /* Subtle white glow */
            margin-bottom: 1.5rem;
            text-align: center;
        }

        h1 {
            font-size: 2.5rem; /* Larger for welcome */
        }

        h2 {
            font-size: 2rem;
        }

        p {
            font-size: 1.125rem; /* Base paragraph size */
            line-height: 1.6;
            margin-bottom: 1.5rem;
            text-align: center;
            color: #d0d0d0; /* Lighter grey for paragraphs */
        }

        label {
            font-size: 1.1rem;
            margin-bottom: 0.75rem;
            display: flex;
            align-items: center;
            cursor: pointer;
            padding: 0.75rem 1rem;
            background: rgba(255, 255, 255, 0.04); /* Very subtle light background */
            border-radius: 0.75rem;
            border: 1px solid rgba(255, 255, 255, 0.15); /* Light border */
            transition: background 0.3s ease, border-color 0.3s ease;
        }

        label:hover {
            background: rgba(255, 255, 255, 0.08); /* Slightly more prominent on hover */
            border-color: #ffffff; /* White border on hover */
        }

        input[type="radio"], input[type="checkbox"] {
            margin-right: 0.75rem;
            transform: scale(1.2);
            accent-color: #ffffff; /* White accent for checkboxes/radios */
        }

        input[type="text"], input[type="number"], textarea {
            background: rgba(255, 255, 255, 0.1); /* Light transparent background */
            border: 1px solid #c0c0c0; /* Light gray border */
            border-radius: 0.75rem;
            padding: 0.75rem 1rem;
            width: 100%;
            color: #f0f0f0; /* Off-white text */
            font-size: 1rem;
            transition: border-color 0.3s ease, box-shadow 0.3s ease;
        }

        input[type="text"]:focus, input[type="number"]:focus, textarea:focus {
            outline: none;
            border-color: #ffffff; /* White border on focus */
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5); /* White glow on focus */
        }

        textarea {
            min-height: 100px;
            resize: vertical;
        }

        button {
            background: linear-gradient(45deg, #e0e0e0, #ffffff); /* Light grey to white gradient */
            color: #1a1a1a; /* Dark text for buttons */
            border: none;
            border-radius: 0.75rem;
            padding: 0.8rem 1.8rem;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(255, 255, 255, 0.2); /* Subtle white shadow */
            transition: all 0.3s ease;
            flex-shrink: 0;
            margin: 0.5rem;
        }

        button:hover {
            background: linear-gradient(45deg, #ffffff, #e0e0e0); /* Reverse gradient on hover */
            box-shadow: 0 8px 20px rgba(255, 255, 255, 0.4), 0 0 10px rgba(255, 255, 255, 0.8); /* More prominent white glow */
            transform: translateY(-2px);
        }

        .button-group {
            display: flex;
            justify-content: space-between;
            width: 100%;
            margin-top: auto; /* Pushes buttons to the bottom */
            flex-wrap: wrap; /* Allows wrapping on small screens */
            gap: 1rem; /* Space between buttons */
            padding-top: 1rem;
        }

        .button-group.centered {
            justify-content: center;
        }

        .option-group {
            display: flex;
            flex-direction: column;
            gap: 0.75rem;
            width: 100%;
            margin-bottom: 1.5rem;
            max-height: 40vh; /* Make the option group scrollable */
            overflow-y: auto;
            padding-right: 10px; /* Add padding for scrollbar */
        }

        #loading-screen {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            font-size: 1.5rem;
            height: 100%;
            width: 100%;
        }

        .spinner {
            border: 8px solid rgba(255, 255, 255, 0.2); /* Lighter border */
            border-top: 8px solid #ffffff; /* White spinning part */
            border-radius: 50%;
            width: 60px;
            height: 60px;
            animation: spin 1s linear infinite;
            margin-bottom: 1.5rem;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        #results-screen pre {
            background: rgba(0, 0, 0, 0.6); /* Darker background for results */
            padding: 1.5rem;
            border-radius: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.3); /* Lighter border */
            white-space: pre-wrap;
            word-break: break-word;
            font-size: 1.1rem;
            line-height: 1.7;
            max-height: 300px;
            overflow-y: auto;
            text-align: left;
            width: 100%;
            color: #f0f0f0; /* Off-white text */
        }

        /* Chatbox styles - adapted for black and white */
        #chatbox-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 350px;
            background: rgba(0, 0, 0, 0.8); /* Darker background for chatbox */
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.2); /* Subtle white glow */
            border-radius: 18px;
            font-family: 'Inter', sans-serif;
            font-size: 15px;
            z-index: 1000;
            border: 1px solid rgba(255, 255, 255, 0.1); /* Subtle border */
            display: none;
            flex-direction: column;
        }

        #chatbox-header {
            background: linear-gradient(90deg, #3a3a3a, #5a5a5a); /* Grey gradient for header */
            color: white;
            padding: 12px 18px;
            border-radius: 17px 17px 0 0;
            font-weight: bold;
            font-size: 1.2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        #chatbox-close-btn {
            background: none;
            border: none;
            color: white;
            font-size: 1.8rem;
            cursor: pointer;
            padding: 0 8px;
            margin: 0;
            transition: transform 0.2s ease;
            box-shadow: none;
        }

        #chatbox-close-btn:hover {
            transform: rotate(90deg);
            box-shadow: none;
        }

        #chatbox {
            height: 250px;
            overflow-y: auto;
            padding: 15px;
            background: rgba(0, 0, 0, 0.6); /* Slightly lighter dark background for chat messages */
            color: #f0f0f0; /* Off-white text */
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            flex-grow: 1;
        }

        #chatbox p {
            margin: 10px 0;
            line-height: 1.5;
        }

        #chat-input-container {
            display: flex;
            padding: 12px;
            background: rgba(0, 0, 0, 0.8); /* Darker background for input area */
            border-radius: 0 0 17px 17px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        #chatInput {
            width: calc(100% - 80px);
            padding: 12px;
            border: 1px solid #8a8a8a; /* Light grey border */
            border-radius: 10px;
            margin: 0;
            font-size: 1.1rem;
            background-color: #333333; /* Darker input background */
            color: #f0f0f0; /* Off-white text */
            flex-grow: 1;
        }

        #sendBtn {
            padding: 12px 18px;
            margin-left: 12px;
            background: linear-gradient(45deg, #8a8a8a, #ffffff); /* Grey to white gradient */
            color: #1a1a1a; /* Dark text */
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.1rem;
            box-shadow: 0 0 12px rgba(255, 255, 255, 0.2); /* Subtle white shadow */
            transition: all 0.3s ease;
            flex-shrink: 0;
        }
        #sendBtn:hover {
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.4), 0 0 5px rgba(255, 255, 255, 0.8);
            transform: translateY(-2px);
        }

        .loading-dots::after {
            content: '';
            animation: loading 1.5s infinite;
        }

        @keyframes loading {
            0% { content: '.'; }
            33% { content: '..'; }
            66% { content: '...'; }
            100% { content: '.'; }
        }

        #chat-toggle-button {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            background: linear-gradient(45deg, #8a8a8a, #ffffff); /* Grey to white gradient */
            color: #1a1a1a; /* Dark text */
            font-size: 2.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0 0 25px rgba(255, 255, 255, 0.3), inset 0 0 12px rgba(255, 255, 255, 0.2);
            z-index: 1001;
            transition: all 0.3s ease;
        }

        #chat-toggle-button:hover {
            transform: scale(1.08);
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.5), inset 0 0 20px rgba(255, 255, 255, 0.4);
        }

        @media (max-width: 768px) {
            .screen {
                padding: 1.5rem;
                border-radius: 1rem;
            }
            h1 { font-size: 2rem; }
            h2 { font-size: 1.75rem; }
            p { font-size: 1rem; }
            label { font-size: 1rem; padding: 0.6rem 0.8rem; border-radius: 0.6rem; }
            button { padding: 0.7rem 1.4rem; font-size: 1rem; margin: 0.3rem; border-radius: 0.6rem; }
            input[type="text"], input[type="number"], textarea { font-size: 0.9rem; padding: 0.6rem 0.8rem; border-radius: 0.6rem; }
            #chatbox-container { width: calc(100vw - 40px); right: 20px; left: 20px; bottom: 20px; border-radius: 1rem; }
            #chatbox-header { padding: 10px 15px; font-size: 1rem; border-radius: 0.9rem 0.9rem 0 0; }
            #chatbox-close-btn { font-size: 1.5rem; }
            #chatbox { height: 180px; padding: 10px; }
            #chatInput { font-size: 0.9rem; padding: 10px; border-radius: 8px; }
            #sendBtn { font-size: 0.9rem; padding: 10px 15px; border-radius: 8px; }
            #chat-toggle-button { width: 55px; height: 55px; font-size: 2rem; }
            #results-screen pre { font-size: 1rem; padding: 1rem; }
        }
    </style>
</head>
<body>

    <div id="welcome-screen" class="screen active">
        <h1>Welcome to the Restaurant Recommender!</h1>
        <p>Answer a few questions about your preferences, and our AI will suggest the perfect place for you to eat.</p>
        <div class="button-group centered">
            <button onclick="nextScreen('welcome-screen', 'cuisine-screen')">Start Survey</button>
        </div>
    </div>

    <div id="cuisine-screen" class="screen">
        <h2>What type of cuisine are you craving?</h2>
        <div class="option-group">
            <label><input type="radio" name="cuisine" value="Italian" required> Italian</label>
            <label><input type="radio" name="cuisine" value="Mexican"> Mexican</label>
            <label><input type="radio" name="cuisine" value="Asian"> Asian (General)</label>
            <label><input type="radio" name="cuisine" value="American"> American (General)</label>
            <label><input type="radio" name="cuisine" value="Mediterranean"> Mediterranean</label>
            <label><input type="radio" name="cuisine" value="Indian"> Indian</label>
            <label><input type="radio" name="cuisine" value="Thai"> Thai</label>
            <label><input type="radio" name="cuisine" value="Japanese"> Japanese</label>
            <label><input type="radio" name="cuisine" value="Chinese"> Chinese</label>
            <label><input type="radio" name="cuisine" value="French"> French</label>
            <label><input type="radio" name="cuisine" value="Spanish"> Spanish</label>
            <label><input type="radio" name="cuisine" value="Greek"> Greek</label>
            <label><input type="radio" name="cuisine" value="Vietnamese"> Vietnamese</label>
            <label><input type="radio" name="cuisine" value="Korean"> Korean</label>
            <label><input type="radio" name="cuisine" value="Middle Eastern"> Middle Eastern</label>
            <label><input type="radio" name="cuisine" value="Brazilian"> Brazilian</label>
            <label><input type="radio" name="cuisine" value="Ethiopian"> Ethiopian</label>
            <label><input type="radio" name="cuisine" value="Caribbean"> Caribbean</label>
            <label><input type="radio" name="cuisine" value="African"> African</label>
            <label><input type="radio" name="cuisine" value="German"> German</label>
            <label><input type="radio" name="cuisine" value="British"> British</label>
            <label><input type="radio" name="cuisine" value="Russian"> Russian</label>
            <label><input type="radio" name="cuisine" value="Moroccan"> Moroccan</label>
            <label><input type="radio" name="cuisine" value="Peruvian"> Peruvian</label>
            <label><input type="radio" name="cuisine" value="Argentinian"> Argentinian</label>
            <label><input type="radio" name="cuisine" value="Irish"> Irish</label>
            <label><input type="radio" name="cuisine" value="Turkish"> Turkish</label>
            <label><input type="radio" name="cuisine" value="Hawaiian"> Hawaiian</label>
            <label><input type="radio" name="cuisine" value="Soul Food"> Soul Food</label>
            <label><input type="radio" name="cuisine" value="Other"> Other (please specify in comments)</label>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('cuisine-screen', 'welcome-screen')">Back</button>
            <button onclick="nextScreen('cuisine-screen', 'dietary-screen')">Next</button>
        </div>
    </div>

    <div id="dietary-screen" class="screen">
        <h2>Do you have any dietary restrictions or preferences?</h2>
        <div class="option-group">
            <label><input type="checkbox" name="dietary" value="Vegetarian"> Vegetarian</label>
            <label><input type="checkbox" name="dietary" value="Vegan"> Vegan</label>
            <label><input type="checkbox" name="dietary" value="Gluten-Free"> Gluten-Free</label>
            <label><input type="checkbox" name="dietary" value="Dairy-Free"> Dairy-Free</label>
            <label><input type="checkbox" name="dietary" value="Nut-Allergy"> Nut Allergy</label>
            <label><input type="checkbox" name="dietary" value="Kosher"> Kosher</label>
            <label><input type="checkbox" name="dietary" value="Halal"> Halal</label>
            <label><input type="checkbox" name="dietary" value="Shellfish-Allergy"> Shellfish Allergy</label>
            <label><input type="checkbox" name="dietary" value="Soy-Free"> Soy-Free</label>
            <label><input type="checkbox" name="dietary" value="Low-Carb"> Low-Carb</label>
            <label><input type="checkbox" name="dietary" value="Keto"> Keto</label>
            <label><input type="checkbox" name="dietary" value="Paleo"> Paleo</label>
            <label><input type="checkbox" name="dietary" value="Diabetic-Friendly"> Diabetic-Friendly</label>
            <label><input type="checkbox" name="dietary" value="Low-Sodium"> Low-Sodium</label>
            <label><input type="checkbox" name="dietary" value="Spicy-Food-Avoidance"> Avoid Spicy Food</label>
            <label><input type="checkbox" name="dietary" value="Pescetarian"> Pescetarian</label>
            <label><input type="checkbox" name="dietary" value="Egg-Allergy"> Egg Allergy</label>
            <label><input type="checkbox" name="dietary" value="Corn-Allergy"> Corn Allergy</label>
            <label><input type="checkbox" name="dietary" value="Sesame-Allergy"> Sesame Allergy</label>
            <label><input type="checkbox" name="dietary" value="Fish-Allergy"> Fish Allergy</label>
            <label><input type="checkbox" name="dietary" value="Low-Fat"> Low-Fat</label>
            <label><input type="checkbox" name="dietary" value="High-Protein"> High-Protein</label>
            <label><input type="checkbox" name="dietary" value="Organic-Preference"> Organic Preference</label>
            <label><input type="checkbox" name="dietary" value="Sustainable-Sourcing"> Sustainable Sourcing</label>
            <label><input type="checkbox" name="dietary" value="Locally-Sourced-Preference"> Locally Sourced Preference</label>
            <label><input type="checkbox" name="dietary" value="Child-Friendly-Options"> Child-Friendly Options</label>
            <label><input type="checkbox" name="dietary" value="No-Pork"> No Pork</label>
            <label><input type="checkbox" name="dietary" value="No-Beef"> No Beef</label>
            <label><input type="checkbox" name="dietary" value="Sugar-Free"> Sugar-Free</label>
            <label><input type="checkbox" name="dietary" value="None"> None</label>
            <label>Other: <input type="text" name="dietary_other" placeholder="e.g., allergies, specific cooking styles"></label>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('dietary-screen', 'cuisine-screen')">Back</button>
            <button onclick="nextScreen('dietary-screen', 'price-screen')">Next</button>
        </div>
    </div>

    <div id="price-screen" class="screen">
        <h2>What's your preferred price range per person?</h2>
        <div class="option-group">
            <label><input type="radio" name="price" value="$" required> $ (Under $15)</label>
            <label><input type="radio" name="price" value="$$"> $$ ($15 - $30)</label>
            <label><input type="radio" name="price" value="$$$"> $$$ ($30 - $60)</label>
            <label><input type="radio" name="price" value="$$$$"> $$$$ (Over $60)</label>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('price-screen', 'dietary-screen')">Back</button>
            <button onclick="nextScreen('price-screen', 'ambiance-screen')">Next</button>
        </div>
    </div>

    <div id="ambiance-screen" class="screen">
        <h2>What kind of ambiance or experience are you looking for?</h2>
        <div class="option-group">
            <label><input type="radio" name="ambiance" value="Casual" required> Casual & Relaxed</label>
            <label><input type="radio" name="ambiance" value="Lively"> Lively & Energetic</label>
            <label><input type="radio" name="ambiance" value="Cozy"> Cozy & Intimate</label>
            <label><input type="radio" name="ambiance" value="Upscale"> Upscale & Formal</label>
            <label><input type="radio" name="ambiance" value="Outdoor"> Outdoor Seating</label>
            <label><input type="radio" name="ambiance" value="Family-Friendly"> Family-Friendly</label>
            <label><input type="radio" name="ambiance" value="Romantic"> Romantic</label>
            <label><input type="radio" name="ambiance" value="Trendy"> Trendy & Modern</label>
            <label><input type="radio" name="ambiance" value="Quiet"> Quiet & Peaceful</label>
            <label><input type="radio" name="ambiance" value="Historic"> Historic / Classic</label>
            <label><input type="radio" name="ambiance" value="Live-Music"> Live Music</label>
            <label><input type="radio" name="ambiance" value="Sports-Bar"> Sports Bar Vibe</label>
            <label><input type="radio" name="ambiance" value="Artistic"> Artistic / Bohemian</label>
            <label><input type="radio" name="ambiance" value="Industrial"> Industrial / Urban</label>
            <label><input type="radio" name="ambiance" value="Rustic"> Rustic / Country</label>
            <label><input type="radio" name="ambiance" value="Waterfront"> Waterfront View</label>
            <label><input type="radio" name="ambiance" value="City-View"> City View / Rooftop</label>
            <label><input type="radio" name="ambiance" value="Kid-Friendly"> Kid-Friendly (with play area)</label>
            <label><input type="radio" name="ambiance" value="Pet-Friendly"> Pet-Friendly Patio</label>
            <label><input type="radio" name="ambiance" value="Quick-Service"> Quick Service / Fast Casual</label>
            <label><input type="radio" name="ambiance" value="Fine-Dining"> Fine Dining (very formal)</label>
            <label><input type="radio" name="ambiance" value="Counter-Service"> Counter Service</label>
            <label><input type="radio" name="ambiance" value="Buffet"> Buffet Style</label>
            <label><input type="radio" name="ambiance" value="Themed-Decor"> Themed Decor</label>
            <label><input type="radio" name="ambiance" value="Dim-Lighting"> Dim Lighting</label>
            <label><input type="radio" name="ambiance" value="Bright-Lighting"> Bright Lighting</label>
            <label><input type="radio" name="ambiance" value="Wheelchair-Accessible"> Wheelchair Accessible</label>
            <label><input type="radio" name="ambiance" value="Good-for-Groups"> Good for Large Groups</label>
            <label><input type="radio" name="ambiance" value="Solo-Dining-Friendly"> Solo Dining Friendly</label>
            <label><input type="radio" name="ambiance" value="Live-Cooking-Show"> Live Cooking Show</label>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('ambiance-screen', 'price-screen')">Back</button>
            <button onclick="nextScreen('ambiance-screen', 'group-size-screen')">Next</button>
        </div>
    </div>

    <div id="group-size-screen" class="screen">
        <h2>How many people are in your dining group?</h2>
        <div class="option-group">
            <input type="number" id="group-size-input" name="group_size" min="1" max="50" value="1" class="text-center" required>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('group-size-screen', 'ambiance-screen')">Back</button>
            <button onclick="nextScreen('group-size-screen', 'location-screen')">Next</button>
        </div>
    </div>

    <!-- New Screen: Location Input -->
    <div id="location-screen" class="screen">
        <h2>Where are you looking to eat?</h2>
        <p>Allow us to detect your current location, or enter a city/address manually for a personalized recommendation.</p>
        <div class="option-group">
            <button class="w-full" onclick="getLocation()">
                <i class="fas fa-map-marker-alt mr-2"></i> Get My Current Location
            </button>
            <p id="location-status" class="text-sm text-center mt-2"></p>
            <div class="text-center my-4 text-gray-400">OR</div> <!-- Changed OR color -->
            <label for="manual-location-input">Enter City or Address:</label>
            <input type="text" id="manual-location-input" name="manual_location" placeholder="e.g., New York, NY or 123 Main St">
        </div>
        <div class="button-group">
            <button onclick="prevScreen('location-screen', 'group-size-screen')">Back</button>
            <button onclick="nextScreen('location-screen', 'comments-screen')">Next</button>
        </div>
    </div>

    <div id="comments-screen" class="screen">
        <h2>Any additional comments or preferences?</h2>
        <p class="text-sm text-center opacity-75">This helps the AI give a more accurate recommendation!</p>
        <div class="option-group">
            <textarea id="comments-input" name="comments" placeholder="e.g., 'Looking for a place near the university', 'Must have good dessert options', 'Prefer less crowded places'."></textarea>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('comments-screen', 'location-screen')">Back</button>
            <button onclick="nextScreen('comments-screen', 'summary-screen')">Review Answers</button>
        </div>
    </div>

    <div id="summary-screen" class="screen">
        <h2>Review Your Answers</h2>
        <div id="survey-summary" class="bg-gray-800 bg-opacity-40 p-6 rounded-lg w-full mb-6 text-left">
            <p><strong>Cuisine:</strong> <span id="summary-cuisine"></span></p>
            <p><strong>Dietary:</strong> <span id="summary-dietary"></span></p>
            <p><strong>Price:</strong> <span id="summary-price"></span></p>
            <p><strong>Ambiance:</strong> <span id="summary-ambiance"></span></p>
            <p><strong>Group Size:</strong> <span id="summary-group-size"></span></p>
            <p><strong>Location:</strong> <span id="summary-location"></span></p>
            <p><strong>Comments:</strong> <span id="summary-comments"></span></p>
        </div>
        <div class="button-group">
            <button onclick="prevScreen('summary-screen', 'comments-screen')">Edit Answers</button>
            <button onclick="submitSurvey()">Get Recommendation</button>
        </div>
    </div>

    <div id="loading-screen" class="screen">
        <div class="spinner"></div>
        <p>Finding the perfect restaurant for you...</p>
        <p class="mt-4">Please wait, our AI is working its magic!</p>
    </div>

    <div id="results-screen" class="screen">
        <h2>Your Restaurant Recommendation:</h2>
        <pre id="ai-recommendation" class="w-full text-left text-2xl font-bold"></pre> <!-- Increased text size for recommendation -->
        <div class="button-group centered">
            <button onclick="resetSurvey()">Start Over</button>
        </div>
    </div>

    <!-- Chatbox hidden by default -->
    <div id="chatbox-container">
        <div id="chatbox-header">
            Need Help? Ask AI
            <button id="chatbox-close-btn"><i class="fas fa-times"></i></button>
        </div>
        <div id="chatbox"></div>
        <div id="chat-input-container">
            <input type="text" id="chatInput" placeholder="Type your question here..." />
            <button id="sendBtn">Send</button>
        </div>
    </div>

    <!-- Chat Toggle Button always visible -->
    <button id="chat-toggle-button">
        <i class="fas fa-comments"></i>
    </button>

    <script>
        let currentScreenId = 'welcome-screen';
        const surveyData = {
            location: null // Initialize location as null
        };
        const locationStatusElement = document.getElementById('location-status');
        const manualLocationInput = document.getElementById('manual-location-input');

        function nextScreen(currentId, nextId) {
            // Validate current screen before moving to the next
            if (!validateScreen(currentId)) {
                return; // Stop if validation fails
            }

            document.getElementById(currentId).classList.remove('active');
            document.getElementById(nextId).classList.add('active');
            currentScreenId = nextId;

            // Populate summary if moving to summary screen
            if (nextId === 'summary-screen') {
                populateSummary();
            }
        }

        function prevScreen(currentId, prevId) {
            document.getElementById(currentId).classList.remove('active');
            document.getElementById(prevId).classList.add('active');
            currentScreenId = prevId;
        }

        function validateScreen(screenId) {
            switch (screenId) {
                case 'cuisine-screen':
                    const cuisineSelected = document.querySelector('input[name="cuisine"]:checked');
                    if (!cuisineSelected) {
                        alert('Please select a cuisine preference.');
                        return false;
                    }
                    surveyData.cuisine = cuisineSelected.value;
                    break;
                case 'dietary-screen':
                    const dietaryCheckboxes = document.querySelectorAll('input[name="dietary"]:checked');
                    const dietaryOtherInput = document.querySelector('input[name="dietary_other"]'); // Get the other input field
                    const dietaryOther = dietaryOtherInput.value.trim();
                    const dietaryRestrictions = Array.from(dietaryCheckboxes).map(cb => cb.value);

                    // Logic to ensure "None" is exclusive
                    if (dietaryRestrictions.includes('None') && dietaryRestrictions.length > 1) {
                         alert('If "None" is selected, no other dietary restrictions should be chosen.');
                         // Automatically uncheck other options if "None" is chosen (and vice-versa is handled below)
                         document.querySelectorAll('input[name="dietary"]').forEach(cb => {
                             if (cb.value !== 'None') {
                                 cb.checked = false;
                             }
                         });
                         dietaryOtherInput.value = ''; // Clear other text if None is selected
                         return false; // Prevent moving forward until user re-selects or confirms
                    }

                    // Add "Other" text input value if it's not empty and "None" is not selected
                    if (dietaryOther && !dietaryRestrictions.includes('None')) {
                        dietaryRestrictions.push(`Other: ${dietaryOther}`);
                    }
                    surveyData.dietary = dietaryRestrictions.length > 0 ? dietaryRestrictions.join(', ') : 'None';
                    break;
                case 'price-screen':
                    const priceSelected = document.querySelector('input[name="price"]:checked');
                    if (!priceSelected) {
                        alert('Please select a price range.');
                        return false;
                    }
                    surveyData.price = priceSelected.value;
                    break;
                case 'ambiance-screen':
                    const ambianceSelected = document.querySelector('input[name="ambiance"]:checked');
                    if (!ambianceSelected) {
                        alert('Please select an ambiance preference.');
                        return false;
                    }
                    surveyData.ambiance = ambianceSelected.value;
                    break;
                case 'group-size-screen':
                    const groupSizeInput = document.getElementById('group-size-input');
                    const groupSize = parseInt(groupSizeInput.value, 10);
                    if (isNaN(groupSize) || groupSize < 1 || groupSize > 50) {
                        alert('Please enter a valid group size between 1 and 50.');
                        return false;
                    }
                    surveyData.group_size = groupSize;
                    break;
                case 'location-screen':
                    const manualLocation = manualLocationInput.value.trim();
                    // If no auto-location and no manual location, alert
                    if (!surveyData.location && !manualLocation) {
                        alert('Please allow location access or enter a city/address manually.');
                        return false;
                    }
                    // If manual location is entered, and auto location wasn't successful/provided
                    if (manualLocation && !surveyData.location) {
                        surveyData.location = `Manual Input: ${manualLocation}`;
                    }
                    break;
                case 'comments-screen':
                    surveyData.comments = document.getElementById('comments-input').value.trim();
                    break;
            }
            return true;
        }

        // Function to get user's geolocation
        function getLocation() {
            if (navigator.geolocation) {
                locationStatusElement.textContent = "Getting your location...";
                locationStatusElement.style.color = '#f0f0f0'; /* Default text color */
                navigator.geolocation.getCurrentPosition(showPosition, showError);
            } else {
                locationStatusElement.textContent = "Geolocation is not supported by this browser.";
                // Clear any previously stored auto-location if browser doesn't support
                surveyData.location = null;
                locationStatusElement.style.color = '#ff6666'; /* Error color */
            }
        }

        function showPosition(position) {
            const lat = position.coords.latitude;
            const lon = position.coords.longitude;
            surveyData.location = `Latitude: ${lat}, Longitude: ${lon}`;
            locationStatusElement.textContent = `Location obtained! Lat: ${lat.toFixed(2)}, Lon: ${lon.toFixed(2)}`;
            locationStatusElement.style.color = '#a7f3d0'; /* Green for success */
            manualLocationInput.value = ''; // Clear manual input if auto-location succeeds
        }

        function showError(error) {
            switch(error.code) {
                case error.PERMISSION_DENIED:
                    locationStatusElement.textContent = "Location access denied. Please enter manually.";
                    break;
                case error.POSITION_UNAVAILABLE:
                    locationStatusElement.textContent = "Location information is unavailable.";
                    break;
                case error.TIMEOUT:
                    locationStatusElement.textContent = "The request to get user location timed out.";
                    break;
                case error.UNKNOWN_ERROR:
                    locationStatusElement.textContent = "An unknown error occurred.";
                    break;
            }
            locationStatusElement.style.color = '#ff6666'; /* Red for error */
            surveyData.location = null; // Clear location if error occurs
        }

        function populateSummary() {
            document.getElementById('summary-cuisine').textContent = surveyData.cuisine || 'Not specified';
            document.getElementById('summary-dietary').textContent = surveyData.dietary || 'None';
            document.getElementById('summary-price').textContent = surveyData.price || 'Not specified';
            document.getElementById('summary-ambiance').textContent = surveyData.ambiance || 'Not specified';
            document.getElementById('summary-group-size').textContent = surveyData.group_size || 'Not specified';
            document.getElementById('summary-location').textContent = surveyData.location || 'Not provided'; // Display location summary
            document.getElementById('summary-comments').textContent = surveyData.comments || 'None';
        }

        async function submitSurvey() {
            document.getElementById('summary-screen').classList.remove('active');
            document.getElementById('loading-screen').classList.add('active');
            currentScreenId = 'loading-screen';

            const prompt = `As a restaurant recommender AI, based on the following student preferences, suggest one suitable restaurant. Provide ONLY the restaurant name. Nothing else.
            Preferences:
            - Cuisine: ${surveyData.cuisine}
            - Dietary Restrictions: ${surveyData.dietary}
            - Price Range: ${surveyData.price}
            - Ambiance: ${surveyData.ambiance}
            - Group Size: ${surveyData.group_size}
            - Location: ${surveyData.location || 'Not provided'}
            - Additional Comments: ${surveyData.comments || 'None'}`;


            try {
                const apiKey = ""; // Canvas will automatically provide the API key
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
                const chatHistory = [{ role: "user", parts: [{ text: prompt }] }];
                const payload = { contents: chatHistory };

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    let aiText = result.candidates[0].content.parts[0].text;
                    // Extract only the first line, assuming it's the restaurant name
                    const restaurantName = aiText.split('\n')[0].trim();
                    document.getElementById('ai-recommendation').textContent = restaurantName;
                } else {
                    document.getElementById('ai-recommendation').textContent = "Sorry, I couldn't generate a recommendation at this time. Please try again later or adjust your preferences.";
                    console.error("Gemini API response structure unexpected:", result);
                }
            } catch (error) {
                document.getElementById('ai-recommendation').textContent = "An error occurred while getting a recommendation. Please check your internet connection and try again.";
                console.error("Error calling Gemini API:", error);
            } finally {
                document.getElementById('loading-screen').classList.remove('active');
                document.getElementById('results-screen').classList.add('active');
                currentScreenId = 'results-screen';
            }
        }

        function resetSurvey() {
            // Reset all input fields
            document.querySelectorAll('input[type="radio"]').forEach(input => input.checked = false);
            document.querySelectorAll('input[type="checkbox"]').forEach(input => input.checked = false);
            document.querySelector('input[name="dietary_other"]').value = '';
            document.getElementById('group-size-input').value = '1';
            document.getElementById('comments-input').value = '';
            manualLocationInput.value = ''; // Clear manual location input
            locationStatusElement.textContent = ''; // Clear location status message
            surveyData.location = null; // Clear stored location

            // Clear surveyData object
            for (const key in surveyData) {
                delete surveyData[key];
            }
            surveyData.location = null; // Ensure location is explicitly reset

            // Hide results and show welcome screen
            document.getElementById('results-screen').classList.remove('active');
            document.getElementById('welcome-screen').classList.add('active');
            currentScreenId = 'welcome-screen';
        }

        // Chatbox and Gemini API integration for general help
        const chatbox = document.getElementById('chatbox');
        const chatInput = document.getElementById('chatInput');
        const sendBtn = document.getElementById('sendBtn');
        const chatboxContainer = document.getElementById('chatbox-container');
        const chatToggleButton = document.getElementById('chat-toggle-button');
        const chatboxCloseBtn = document.getElementById('chatbox-close-btn');

        sendBtn.addEventListener('click', sendChat);
        chatInput.addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                sendChat();
            }
        });

        chatToggleButton.addEventListener('click', () => {
            chatboxContainer.style.display = 'flex';
            chatToggleButton.style.display = 'none';
            chatbox.scrollTop = chatbox.scrollHeight;
            sendChat("What is the problem?", true); // Initial query for help
        });

        chatboxCloseBtn.addEventListener('click', () => {
            chatboxContainer.style.display = 'none';
            chatToggleButton.style.display = 'flex';
        });

        async function sendChat(predefinedMessage = null, isInitialQuery = false) {
            let userMsg = predefinedMessage || chatInput.value.trim();
            if (!userMsg && !predefinedMessage) return;

            if (!isInitialQuery) {
                let userP = document.createElement('p');
                userP.textContent = "You: " + userMsg;
                chatbox.appendChild(userP);
                chatInput.value = "";
            } else {
                chatbox.innerHTML = '';
            }

            chatbox.scrollTop = chatbox.scrollHeight;

            let loadingP = document.createElement('p');
            loadingP.innerHTML = "<strong>AI Helper:</strong> Thinking<span class='loading-dots'></span>";
            chatbox.appendChild(loadingP);
            chatbox.scrollTop = chatbox.scrollHeight;

            const prompt = `The user is currently using a restaurant recommendation survey app. Their current survey data is: ${JSON.stringify(surveyData)}. User's question for general help: "${userMsg}". Provide a concise and helpful answer related to using the app or general restaurant/food inquiries.`;

            try {
                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
                const chatHistory = [{ role: "user", parts: [{ text: prompt }] }];
                const payload = { contents: chatHistory };

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const aiText = result.candidates[0].content.parts[0].text;
                    loadingP.innerHTML = `<strong>AI Helper:</strong> ${aiText}`;
                } else {
                    loadingP.innerHTML = "<strong>AI Helper:</strong> Sorry, I couldn't get a response. Please try again.";
                    console.error("Gemini API response structure unexpected:", result);
                }
            } catch (error) {
                loadingP.innerHTML = "<strong>AI Helper:</strong> An error occurred while generating the hint. Please try again.";
                console.error("Error calling Gemini API:", error);
            }
            chatbox.scrollTop = chatbox.scrollHeight;
        }

        // Handle "None" checkbox behavior for dietary restrictions and new Kosher/Halal options
        document.addEventListener('change', function(event) {
            if (event.target.name === 'dietary' && event.target.type === 'checkbox') {
                const noneCheckbox = document.querySelector('input[name="dietary"][value="None"]');
                const dietaryOtherInput = document.querySelector('input[name="dietary_other"]');

                if (event.target.value === 'None') {
                    // If "None" is checked, uncheck all other dietary options and clear "Other" text
                    if (event.target.checked) {
                        document.querySelectorAll('input[name="dietary"]').forEach(cb => {
                            if (cb.value !== 'None') {
                                cb.checked = false;
                            }
                        });
                        dietaryOtherInput.value = ''; // Clear other text
                    }
                } else {
                    // If any other dietary option is checked, uncheck "None"
                    if (event.target.checked && noneCheckbox.checked) {
                        noneCheckbox.checked = false;
                    }
                }
            }
        });
    </script>
</body>
</html>
