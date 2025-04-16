# Calculate
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Life Stats - Interactive Calculator</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #4F46E5;
            --secondary-color: #EC4899;
            --accent-color: #8B5CF6;
            --background-color: #F9FAFB;
            --card-color: #FFFFFF;
            --text-color: #1F2937;
            --light-text: #6B7280;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            overflow-x: hidden;
        }

        .gradient-text {
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .stat-card {
            border-radius: 16px;
            background-color: var(--card-color);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            overflow: hidden;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
        }

        .stat-icon {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 60px;
            height: 60px;
            border-radius: 12px;
            font-size: 24px;
            margin-bottom: 20px;
        }

        .counter {
            font-weight: 700;
            font-size: 2.5rem;
            margin: 10px 0;
            transition: all 0.8s ease;
        }

        .birthday-select {
            background-color: white;
            border: 2px solid #E5E7EB;
            border-radius: 10px;
            padding: 12px 16px;
            font-size: 16px;
            transition: all 0.3s ease;
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 24 24' stroke='%234B5563'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M19 9l-7 7-7-7'%3E%3C/path%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 12px center;
            background-size: 16px;
        }

        .birthday-select:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.2);
        }

        .calculate-btn {
            background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
            color: white;
            border: none;
            border-radius: 12px;
            padding: 14px 28px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(79, 70, 229, 0.3);
        }

        .calculate-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(79, 70, 229, 0.4);
        }

        .calculate-btn:active {
            transform: translateY(0);
        }

        .stats-container {
            max-width: 1200px;
            margin: 60px auto;
            opacity: 0;
            transform: translateY(30px);
            transition: opacity 1s ease, transform 1s ease;
        }

        .stats-container.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .loader-circle {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            border: 8px solid #f3f3f3;
            border-top: 8px solid var(--primary-color);
            animation: spin 1.5s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .loading-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.5s ease, visibility 0.5s ease;
        }

        .loading-container.active {
            opacity: 1;
            visibility: visible;
        }

        .progress-bar {
            height: 6px;
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color));
            border-radius: 3px;
            width: 0%;
            transition: width 0.3s ease-in-out;
        }

        .fun-fact {
            background-color: #EEF2FF;
            border-left: 4px solid var(--primary-color);
            padding: 16px;
            border-radius: 0 8px 8px 0;
            margin-top: 16px;
            font-size: 0.9rem;
        }

        .scroll-down {
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {transform: translateY(0);}
            40% {transform: translateY(-20px);}
            60% {transform: translateY(-10px);}
        }

        .icon-bg-heart { background-color: #FECDD3; color: #BE123C; }
        .icon-bg-lungs { background-color: #BFDBFE; color: #1D4ED8; }
        .icon-bg-eye { background-color: #C7D2FE; color: #4338CA; }
        .icon-bg-bed { background-color: #DDD6FE; color: #7E22CE; }
        .icon-bg-food { background-color: #FED7AA; color: #C2410C; }
        .icon-bg-earth { background-color: #A7F3D0; color: #047857; }
        .icon-bg-words { background-color: #FDE68A; color: #92400E; }
        .icon-bg-steps { background-color: #D1FAE5; color: #065F46; }
        .icon-bg-phone { background-color: #FEE2E2; color: #B91C1C; }
        .icon-bg-shower { background-color: #E0E7FF; color: #3730A3; }
        .icon-bg-birthday { background-color: #FBCFE8; color: #BE185D; }
        .icon-bg-money { background-color: #CCFBF1; color: #0F766E; }
    </style>
</head>
<body>
    <div class="loading-container" id="loadingContainer">
        <div class="loader-circle"></div>
        <h3 class="mt-8 text-xl font-semibold">Calculating Your Life Stats...</h3>
        <div class="mt-4 w-64 bg-gray-200 rounded-full overflow-hidden">
            <div class="progress-bar" id="progressBar"></div>
        </div>
        <p class="mt-2 text-gray-500" id="loadingText">Gathering heartbeats...</p>
    </div>

    <header class="bg-white py-8 shadow-sm">
        <div class="container mx-auto px-4 text-center">
            <h1 class="text-4xl md:text-5xl font-bold mb-2">My <span class="gradient-text">Life Stats</span></h1>
            <p class="text-gray-600 max-w-2xl mx-auto">Discover fascinating statistics about your life journey, from heartbeats to cosmic travels, all based on your birthday.</p>
        </div>
    </header>

    <section class="py-16 container mx-auto px-4 text-center" id="birthdaySection">
        <h2 class="text-3xl font-bold mb-8">When were you born?</h2>
        
        <div class="flex flex-col md:flex-row justify-center items-center gap-4 max-w-lg mx-auto">
            <div class="w-full md:w-1/3">
                <select id="daySelect" class="birthday-select w-full">
                    <option value="" disabled selected>Day</option>
                </select>
            </div>
            <div class="w-full md:w-1/3">
                <select id="monthSelect" class="birthday-select w-full">
                    <option value="" disabled selected>Month</option>
                    <option value="0">January</option>
                    <option value="1">February</option>
                    <option value="2">March</option>
                    <option value="3">April</option>
                    <option value="4">May</option>
                    <option value="5">June</option>
                    <option value="6">July</option>
                    <option value="7">August</option>
                    <option value="8">September</option>
                    <option value="9">October</option>
                    <option value="10">November</option>
                    <option value="11">December</option>
                </select>
            </div>
            <div class="w-full md:w-1/3">
                <select id="yearSelect" class="birthday-select w-full">
                    <option value="" disabled selected>Year</option>
                </select>
            </div>
        </div>
        
        <button id="calculateBtn" disabled class="calculate-btn mt-8 opacity-70 cursor-not-allowed">
            Calculate My Life Stats
        </button>
        
        <div class="mt-16 text-center hidden" id="scrollPrompt">
            <p class="text-gray-500 mb-2">Scroll down to see your stats</p>
            <i class="fas fa-chevron-down text-2xl text-gray-400 scroll-down"></i>
        </div>
    </section>

    <div id="statsContainer" class="stats-container">
        <!-- Stats will be inserted here -->
    </div>

    <footer class="bg-gray-800 text-white py-8">
        <div class="container mx-auto px-4 text-center">
            <p>Created with ❤️ | Inspired by <a href="https://neal.fun/life-stats/" class="text-indigo-300 hover:underline" target="_blank">Neal.fun</a></p>
            <p class="mt-2 text-gray-400 text-sm">This is for entertainment purposes only. Statistics are approximate.</p>
        </div>
    </footer>

    <script>
        // Initialize date dropdowns
        const daySelect = document.getElementById('daySelect');
        const monthSelect = document.getElementById('monthSelect');
        const yearSelect = document.getElementById('yearSelect');
        const calculateBtn = document.getElementById('calculateBtn');
        const statsContainer = document.getElementById('statsContainer');
        const scrollPrompt = document.getElementById('scrollPrompt');
        const loadingContainer = document.getElementById('loadingContainer');
        const progressBar = document.getElementById('progressBar');
        const loadingText = document.getElementById('loadingText');

        // Populate days
        for (let i = 1; i <= 31; i++) {
            const option = document.createElement('option');
            option.value = i;
            option.textContent = i;
            daySelect.appendChild(option);
        }

        // Populate years (starting from 1930 to current year)
        const currentYear = new Date().getFullYear();
        for (let i = currentYear; i >= 1930; i--) {
            const option = document.createElement('option');
            option.value = i;
            option.textContent = i;
            yearSelect.appendChild(option);
        }

        // Update days based on month and year selection
        function updateDays() {
            const month = parseInt(monthSelect.value);
            const year = parseInt(yearSelect.value);
            
            if (isNaN(month) || isNaN(year)) return;
            
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            
            // Save current day selection if possible
            const currentDay = parseInt(daySelect.value);
            
            // Clear days
            while (daySelect.firstChild) {
                daySelect.removeChild(daySelect.firstChild);
            }
            
            // Add placeholder
            const placeholder = document.createElement('option');
            placeholder.value = "";
            placeholder.textContent = "Day";
            placeholder.disabled = true;
            placeholder.selected = true;
            daySelect.appendChild(placeholder);
            
            // Populate days based on month
            for (let i = 1; i <= daysInMonth; i++) {
                const option = document.createElement('option');
                option.value = i;
                option.textContent = i;
                
                // Restore previous selection if valid
                if (i === currentDay && currentDay <= daysInMonth) {
                    option.selected = true;
                }
                
                daySelect.appendChild(option);
            }
            
            checkFormValidity();
        }

        monthSelect.addEventListener('change', updateDays);
        yearSelect.addEventListener('change', updateDays);
        daySelect.addEventListener('change', checkFormValidity);

        // Check if all fields are filled
        function checkFormValidity() {
            if (daySelect.value && monthSelect.value && yearSelect.value) {
                calculateBtn.disabled = false;
                calculateBtn.classList.remove('opacity-70', 'cursor-not-allowed');
            } else {
                calculateBtn.disabled = true;
                calculateBtn.classList.add('opacity-70', 'cursor-not-allowed');
            }
        }

        // Format large numbers with commas
        function formatNumber(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        }

        // Calculate age in milliseconds
        function getAgeInMs(birthdate) {
            return Date.now() - birthdate.getTime();
        }

        // Calculate age in years
        function getAgeInYears(birthdate) {
            const now = new Date();
            let age = now.getFullYear() - birthdate.getFullYear();
            const monthDifference = now.getMonth() - birthdate.getMonth();
            
            if (monthDifference < 0 || (monthDifference === 0 && now.getDate() < birthdate.getDate())) {
                age--;
            }
            
            return age;
        }

        // Calculate life statistics
        function calculateLifeStats() {
            const day = parseInt(daySelect.value);
            const month = parseInt(monthSelect.value);
            const year = parseInt(yearSelect.value);
            
            if (isNaN(day) || isNaN(month) || isNaN(year)) {
                alert('Please select a valid date');
                return;
            }
            
            // Show loading screen
            loadingContainer.classList.add('active');
            
            // Animation for loading screen
            let progress = 0;
            const loadingInterval = setInterval(() => {
                progress += 1;
                progressBar.style.width = `${progress}%`;
                
                if (progress <= 20) {
                    loadingText.textContent = "Counting heartbeats...";
                } else if (progress <= 40) {
                    loadingText.textContent = "Calculating breaths...";
                } else if (progress <= 60) {
                    loadingText.textContent = "Measuring Earth rotations...";
                } else if (progress <= 80) {
                    loadingText.textContent = "Tallying blinks...";
                } else {
                    loadingText.textContent = "Finalizing your life statistics...";
                }
                
                if (progress >= 100) {
                    clearInterval(loadingInterval);
                    
                    // Hide loading after a short delay to show 100%
                    setTimeout(() => {
                        loadingContainer.classList.remove('active');
                        displayStats();
                        scrollPrompt.classList.remove('hidden');
                    }, 500);
                }
            }, 30);
        }

        function displayStats() {
            const day = parseInt(daySelect.value);
            const month = parseInt(monthSelect.value);
            const year = parseInt(yearSelect.value);
            
            const birthdate = new Date(year, month, day);
            const ageInMs = getAgeInMs(birthdate);
            const ageInYears = getAgeInYears(birthdate);
            const ageInDays = Math.floor(ageInMs / (1000 * 60 * 60 * 24));
            const ageInHours = Math.floor(ageInMs / (1000 * 60 * 60));
            const ageInMinutes = Math.floor(ageInMs / (1000 * 60));
            const ageInSeconds = Math.floor(ageInMs / 1000);
            
            // Calculate various life stats
            const heartbeats = Math.floor(ageInMinutes * 80); // Average 80 beats per minute
            const breaths = Math.floor(ageInMinutes * 16); // Average 16 breaths per minute
            const blinks = Math.floor(ageInMinutes * 15); // Average 15 blinks per minute
            const sleepHours = Math.floor(ageInDays * 8); // Average 8 hours of sleep per day
            const meals = Math.floor(ageInDays * 3); // Average 3 meals per day
            const steps = Math.floor(ageInDays * 5000); // Average 5000 steps per day
            const words = Math.floor(ageInDays * 7000); // Average 7000 words spoken per day
            const earthRotations = ageInDays; // One rotation per day
            const earthOrbitsAroundSun = ageInYears; // One orbit per year
            const distanceTraveled = Math.floor(ageInDays * 24 * 1600); // Earth rotates at ~1600 km/h
            const showers = Math.floor(ageInDays * 0.8); // Average 5-6 showers per week
            const birthdayCelebrationsLeft = Math.floor(85 - ageInYears); // Average life expectancy ~85
            const moneySpent = Math.floor(ageInDays * 50); // Rough estimate of daily spending
            
            // Calculate zodiac sign
            const zodiacSigns = [
                { name: "Capricorn", icon: "fas fa-mountain", dates: [{ startMonth: 11, startDay: 22 }, { endMonth: 0, endDay: 19 }] },
                { name: "Aquarius", icon: "fas fa-water", dates: [{ startMonth: 0, startDay: 20 }, { endMonth: 1, endDay: 18 }] },
                { name: "Pisces", icon: "fas fa-fish", dates: [{ startMonth: 1, startDay: 19 }, { endMonth: 2, endDay: 20 }] },
                { name: "Aries", icon: "fas fa-fire", dates: [{ startMonth: 2, startDay: 21 }, { endMonth: 3, endDay: 19 }] },
                { name: "Taurus", icon: "fas fa-tree", dates: [{ startMonth: 3, startDay: 20 }, { endMonth: 4, endDay: 20 }] },
                { name: "Gemini", icon: "fas fa-yin-yang", dates: [{ startMonth: 4, startDay: 21 }, { endMonth: 5, endDay: 20 }] },
                { name: "Cancer", icon: "fas fa-moon", dates: [{ startMonth: 5, startDay: 21 }, { endMonth: 6, endDay: 22 }] },
                { name: "Leo", icon: "fas fa-crown", dates: [{ startMonth: 6, startDay: 23 }, { endMonth: 7, endDay: 22 }] },
                { name: "Virgo", icon: "fas fa-leaf", dates: [{ startMonth: 7, startDay: 23 }, { endMonth: 8, endDay: 22 }] },
                { name: "Libra", icon: "fas fa-balance-scale", dates: [{ startMonth: 8, startDay: 23 }, { endMonth: 9, endDay: 22 }] },
                { name: "Scorpio", icon: "fas fa-bolt", dates: [{ startMonth: 9, startDay: 23 }, { endMonth: 10, endDay: 21 }] },
                { name: "Sagittarius", icon: "fas fa-star", dates: [{ startMonth: 10, startDay: 22 }, { endMonth: 11, endDay: 21 }] }
            ];
            
            let zodiacSign = null;
            for (const sign of zodiacSigns) {
                if (
                    (month === sign.dates[0].startMonth && day >= sign.dates[0].startDay) ||
                    (month === sign.dates[1].endMonth && day <= sign.dates[1].endDay)
                ) {
                    zodiacSign = sign;
                    break;
                }
            }
            
            // Historical events
            const historicalEvents = [
                { year: 1969, event: "Moon Landing" },
                { year: 1989, event: "Fall of the Berlin Wall" },
                { year: 1991, event: "Dissolution of the Soviet Union" },
                { year: 2001, event: "9/11 Attacks" },
                { year: 2007, event: "Release of the first iPhone" },
                { year: 2008, event: "Global Financial Crisis" },
                { year: 2010, event: "Instagram was founded" },
                { year: 2011, event: "Game of Thrones TV series premiered" },
                { year: 2019, event: "COVID-19 Pandemic began" },
                { year: 2022, event: "ChatGPT was released" }
            ].filter(event => event.year > year);
            
            // Create stats HTML
            statsContainer.innerHTML = `
                <div class="pb-10">
                    <div class="text-center mb-16">
                        <h2 class="text-3xl font-bold">Your Life in Numbers</h2>
                        <p class="text-gray-600 mt-2">Based on your birthdate: ${new Date(year, month, day).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' })}</p>
                        <p class="text-xl font-medium mt-4">You have been alive for:</p>
                        <div class="flex flex-wrap justify-center gap-4 mt-4">
                            <div class="stat-card p-4 w-36">
                                <p class="text-gray-500 text-sm">Years</p>
                                <p class="text-2xl font-bold">${ageInYears}</p>
                            </div>
                            <div class="stat-card p-4 w-36">
                                <p class="text-gray-500 text-sm">Days</p>
                                <p class="text-2xl font-bold">${formatNumber(ageInDays)}</p>
                            </div>
                            <div class="stat-card p-4 w-36">
                                <p class="text-gray-500 text-sm">Hours</p>
                                <p class="text-2xl font-bold">${formatNumber(ageInHours)}</p>
                            </div>
                            <div class="stat-card p-4 w-36">
                                <p class="text-gray-500 text-sm">Minutes</p>
                                <p class="text-2xl font-bold">${formatNumber(ageInMinutes)}</p>
                            </div>
                        </div>
                    </div>
                
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                        <!-- Heartbeats -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-heart">
                                <i class="fas fa-heartbeat"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Heartbeats</h3>
                            <div class="counter" id="heartbeatsCounter">0</div>
                            <p class="text-gray-600">Your heart beats about 80 times per minute.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> Your heart will beat about 2.5 billion times in an average lifetime.
                            </div>
                        </div>
                        
                        <!-- Breaths -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-lungs">
                                <i class="fas fa-wind"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Breaths Taken</h3>
                            <div class="counter" id="breathsCounter">0</div>
                            <p class="text-gray-600">You breathe about 16 times per minute.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> The air you exhale contains about 100 times more carbon dioxide than the air you inhale.
                            </div>
                        </div>
                        
                        <!-- Blinks -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-eye">
                                <i class="fas fa-eye"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Times Blinked</h3>
                            <div class="counter" id="blinksCounter">0</div>
                            <p class="text-gray-600">You blink about 15 times per minute.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> You spend about 10% of your waking hours with your eyes closed due to blinking.
                            </div>
                        </div>
                        
                        <!-- Sleep -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-bed">
                                <i class="fas fa-bed"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Hours Slept</h3>
                            <div class="counter" id="sleepCounter">0</div>
                            <p class="text-gray-600">You sleep about 8 hours per day.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> If you live to 75, you'll spend about 25 years of your life sleeping.
                            </div>
                        </div>
                        
                        <!-- Meals -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-food">
                                <i class="fas fa-utensils"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Meals Eaten</h3>
                            <div class="counter" id="mealsCounter">0</div>
                            <p class="text-gray-600">You eat about 3 meals per day.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> The average person consumes about 35 tons of food in their lifetime.
                            </div>
                        </div>
                        
                        <!-- Earth Rotation -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-earth">
                                <i class="fas fa-globe-americas"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Earth Rotations</h3>
                            <div class="counter" id="rotationsCounter">0</div>
                            <p class="text-gray-600">You've experienced one Earth rotation per day.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> At the equator, you're moving at about 1,000 mph just standing still due to Earth's rotation.
                            </div>
                        </div>
                        
                        <!-- Words Spoken -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-words">
                                <i class="fas fa-comments"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Words Spoken</h3>
                            <div class="counter" id="wordsCounter">0</div>
                            <p class="text-gray-600">The average person speaks about 7,000 words per day.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> If all the words you've spoken were written down, they would fill about 50 books.
                            </div>
                        </div>
                        
                        <!-- Steps Taken -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-steps">
                                <i class="fas fa-shoe-prints"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Steps Taken</h3>
                            <div class="counter" id="stepsCounter">0</div>
                            <p class="text-gray-600">The average person takes about 5,000 steps per day.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> If you put all your steps in a straight line, you could have walked around the Earth multiple times.
                            </div>
                        </div>
                        
                        <!-- Distance Traveled -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-earth">
                                <i class="fas fa-route"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Distance Traveled</h3>
                            <div class="counter" id="distanceCounter">0</div>
                            <p class="text-gray-600">In kilometers due to Earth's rotation and orbit.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> Even while standing still, you travel about 1.3 million miles through space each year.
                            </div>
                        </div>
                        
                        <!-- Showers Taken -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-shower">
                                <i class="fas fa-shower"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Showers Taken</h3>
                            <div class="counter" id="showersCounter">0</div>
                            <p class="text-gray-600">Assuming an average of 5-6 showers per week.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> The average shower uses about 17 gallons of water. You've likely used thousands of gallons already!
                            </div>
                        </div>
                        
                        <!-- Birthdays Left -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-birthday">
                                <i class="fas fa-birthday-cake"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Birthdays Left</h3>
                            <div class="counter" id="birthdaysCounter">0</div>
                            <p class="text-gray-600">Based on average life expectancy of 85 years.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> As you age, your brain actually gets better at certain cognitive tasks, like vocabulary.
                            </div>
                        </div>
                        
                        <!-- Money Spent -->
                        <div class="stat-card p-8">
                            <div class="stat-icon icon-bg-money">
                                <i class="fas fa-dollar-sign"></i>
                            </div>
                            <h3 class="text-xl font-semibold">Money Spent</h3>
                            <div class="counter" id="moneyCounter">0</div>
                            <p class="text-gray-600">Rough estimate based on average daily spending.</p>
                            <div class="fun-fact">
                                <strong>Fun fact:</strong> The average person spends about $1.8 million in their lifetime.
                            </div>
                        </div>
                    </div>
                    
                    <div class="mt-20">
                        <h2 class="text-3xl font-bold text-center mb-12">Historical Context</h2>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                            <div class="stat-card p-8">
                                <h3 class="text-xl font-semibold mb-4">Zodiac Sign</h3>
                                <div class="flex items-center justify-center mb-6">
                                    <div class="w-16 h-16 rounded-full bg-indigo-100 flex items-center justify-center text-indigo-600 text-2xl">
                                        <i class="${zodiacSign?.icon || 'fas fa-question'}"></i>
                                    </div>
                                </div>
                                <p class="text-2xl font-bold text-center">${zodiacSign?.name || 'Unknown'}</p>
                            </div>
                            
                            <div class="stat-card p-8">
                                <h3 class="text-xl font-semibold mb-4">Major Events In Your Lifetime</h3>
                                <ul class="space-y-3">
                                    ${historicalEvents.slice(0, 5).map(event => `
                                        <li class="flex items-center">
                                            <span class="w-16 inline-block font-semibold">${event.year}</span>
                                            <span class="flex-1">${event.event}</span>
                                        </li>
                                    `).join('')}
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            `;
            
            // Make stats container visible
            statsContainer.classList.add('visible');
            
            // Animate counters
            animateCounter('heartbeatsCounter', heartbeats);
            animateCounter('breathsCounter', breaths);
            animateCounter('blinksCounter', blinks);
            animateCounter('sleepCounter', sleepHours);
            animateCounter('mealsCounter', meals);
            animateCounter('rotationsCounter', earthRotations);
            animateCounter('wordsCounter', words);
            animateCounter('stepsCounter', steps);
            animateCounter('distanceCounter', distanceTraveled);
            animateCounter('showersCounter', showers);
            animateCounter('birthdaysCounter', birthdayCelebrationsLeft);
            animateCounter('moneyCounter', moneySpent);
            
            // Scroll to stats
            setTimeout(() => {
                statsContainer.scrollIntoView({ behavior: 'smooth' });
            }, 500);
        }

        function animateCounter(id, target) {
            const counter = document.getElementById(id);
            if (!counter) return;
            
            const duration = 2000; // Animation duration in milliseconds
            const frameDuration = 1000 / 60; // 60fps
            const totalFrames = Math.round(duration / frameDuration);
            
            // Use exponential easing for more natural counting
            const easeOutQuart = t => 1 - (--t) * t * t * t;
            
            let frame = 0;
            const countTo = parseInt(target, 10);
            
            const animate = () => {
                frame++;
                const progress = easeOutQuart(frame / totalFrames);
                const currentCount = Math.round(countTo * progress);
                
                if (countTo > 1000000) {
                    counter.textContent = formatNumber(currentCount);
                } else {
                    counter.textContent = formatNumber(currentCount);
                }
                
                if (frame < totalFrames) {
                    requestAnimationFrame(animate);
                }
            };
            
            animate();
        }

        calculateBtn.addEventListener('click', calculateLifeStats);
    </script>
</body>
</html>
