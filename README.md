<!DOCTYPE html>
<html lang="en" dir="ltr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Amiryx - Digital Student Transport Management Platform</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Inter & Cairo for Arabic -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800&family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        amiryx: {
                            blue: '#4C95D1',
                            purple: '#9D78C6',
                            purpleDark: '#7A52AA',
                            blueDark: '#3378AF',
                            bgLight: '#F5F0FB',
                            cardBg: '#FFFFFF',
                            textDark: '#1E2338'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'Cairo', 'sans-serif'],
                        arabic: ['Cairo', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Styling & Map Grid Background */
        .map-grid-bg {
            background-color: #EBF0F7;
            background-image: 
                radial-gradient(#4C95D1 0.75px, transparent 0.75px), 
                radial-gradient(#9D78C6 0.75px, #EBF0F7 0.75px);
            background-size: 28px 28px;
            background-position: 0 0, 14px 14px;
        }
        
        .pulse-ring {
            box-shadow: 0 0 0 0 rgba(76, 149, 209, 0.7);
            animation: pulse-ring 2s infinite;
        }

        @keyframes pulse-ring {
            0% {
                transform: scale(0.95);
                box-shadow: 0 0 0 0 rgba(76, 149, 209, 0.7);
            }
            70% {
                transform: scale(1.1);
                box-shadow: 0 0 0 12px rgba(76, 149, 209, 0);
            }
            100% {
                transform: scale(0.95);
                box-shadow: 0 0 0 0 rgba(76, 149, 209, 0);
            }
        }

        /* Bus Animation */
        .bus-marker {
            transition: all 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
        }

        /* RTL Specific adjustments */
        [dir="rtl"] .rtl-flip {
            transform: scaleX(-1);
        }

        /* Glassmorphism */
        .glass-panel {
            background: rgba(255, 255, 255, 0.88);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
            border: 1px solid rgba(255, 255, 255, 0.4);
        }
    </style>
</head>
<body class="bg-amiryx-bgLight text-amiryx-textDark font-sans antialiased selection:bg-amiryx-purple selection:text-white min-h-screen flex flex-col">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed top-5 right-5 z-50 flex flex-col gap-3 pointer-events-none"></div>

    <!-- Header / Navigation -->
    <header class="sticky top-0 z-40 bg-white/90 backdrop-blur-md border-b border-purple-100 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
            
            <!-- Brand Logo & Badge -->
            <div class="flex items-center space-x-3 rtl:space-x-reverse">
                <div class="flex items-center gap-2">
                    <!-- Amiryx Stylized Logo based on visual identity -->
                    <svg class="h-12 w-auto" viewBox="0 0 320 100" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <!-- Motion Swooshes -->
                        <path d="M 30 25 C 100 10, 240 10, 290 45" stroke="#4C95D1" stroke-width="3" stroke-linecap="round" fill="none"/>
                        <path d="M 20 35 C 90 20, 230 20, 280 52" stroke="#4C95D1" stroke-width="2" stroke-linecap="round" fill="none"/>
                        <path d="M 10 55 C 60 85, 180 85, 230 75" stroke="#4C95D1" stroke-width="2" stroke-linecap="round" fill="none"/>
                        <!-- Text stylized AMYRIX -->
                        <text x="15" y="60" font-family="'Inter', sans-serif" font-weight="900" font-size="42" fill="#4C95D1" stroke="#9D78C6" stroke-width="2.5" letter-spacing="1">AMYRIX</text>
                        <!-- Star Accent -->
                        <path d="M 205 32 L 208 40 L 217 40 L 210 45 L 212 53 L 205 48 L 198 53 L 200 45 L 193 40 L 202 40 Z" fill="#9D78C6"/>
                        <!-- Crown Bus Motif -->
                        <g transform="translate(185, 45) scale(0.65)">
                            <!-- Crown -->
                            <path d="M25 -5 L30 8 L40 -2 L50 8 L55 -5 L60 15 L20 15 Z" fill="#9D78C6"/>
                            <!-- Bus Body -->
                            <rect x="0" y="15" width="80" height="35" rx="8" fill="#9D78C6" />
                            <rect x="5" y="20" width="18" height="12" rx="2" fill="#FFFFFF" opacity="0.9"/>
                            <rect x="27" y="20" width="18" height="12" rx="2" fill="#FFFFFF" opacity="0.9"/>
                            <rect x="49" y="20" width="18" height="12" rx="2" fill="#FFFFFF" opacity="0.9"/>
                            <circle cx="18" cy="50" r="7" fill="#1E2338"/>
                            <circle cx="18" cy="50" r="3" fill="#FFFFFF"/>
                            <circle cx="62" cy="50" r="7" fill="#1E2338"/>
                            <circle cx="62" cy="50" r="3" fill="#FFFFFF"/>
                        </g>
                    </svg>
                </div>
                <div class="hidden lg:block border-l rtl:border-r rtl:border-l-0 border-gray-200 pl-3 rtl:pr-3 rtl:pl-0">
                    <p class="text-xs font-bold text-amiryx-purple" id="header-slogan">"Safety and comfort in every step."</p>
                </div>
            </div>

            <!-- Header Controls: Role Switcher, Language & SOS -->
            <div class="flex items-center gap-2 sm:gap-3">
                
                <!-- Role Switcher Toggle -->
                <div class="bg-amiryx-bgLight p-1 rounded-xl border border-purple-200 flex items-center">
                    <button id="nav-student-role" onclick="switchView('student')" class="px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 bg-amiryx-purple text-white shadow-sm">
                        <i class="fa-solid fa-user-graduate"></i>
                        <span id="role-student-label">Student</span>
                    </button>
                    <button id="nav-driver-role" onclick="switchView('driver')" class="px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 text-gray-600 hover:text-amiryx-purple">
                        <i class="fa-solid fa-bus"></i>
                        <span id="role-driver-label">Driver View</span>
                    </button>
                </div>

                <!-- Language Toggle -->
                <button onclick="toggleLanguage()" class="flex items-center gap-1.5 bg-amiryx-bgLight hover:bg-purple-100 text-amiryx-purpleDark px-3 py-1.5 rounded-xl text-xs font-bold transition border border-purple-200">
                    <i class="fa-solid fa-globe"></i>
                    <span id="lang-btn-text">العربية</span>
                </button>

                <!-- SOS Emergency Trigger Button -->
                <button onclick="triggerEmergencyModal()" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded-xl text-xs font-extrabold transition shadow-sm flex items-center gap-1.5 animate-pulse">
                    <i class="fa-solid fa-triangle-exclamation"></i>
                    <span id="sos-btn-text">SOS</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-grow">
        
        <!-- Hero Section -->
        <section class="relative bg-gradient-to-b from-white via-purple-50/40 to-amiryx-bgLight pt-8 pb-10 px-4 sm:px-6 lg:px-8">
            <div class="max-w-7xl mx-auto text-center">
                <span class="inline-block px-4 py-1.5 mb-3 rounded-full bg-amiryx-purple/10 text-amiryx-purpleDark font-bold text-xs tracking-wide" id="hero-badge">
                    🇸🇦 Dedicated Student Transit Management Platform
                </span>
                <h1 class="text-3xl sm:text-5xl font-extrabold text-amiryx-textDark tracking-tight max-w-4xl mx-auto leading-tight" id="hero-title">
                    Safe, Predictable & Organized University Commutes
                </h1>
                <p class="mt-3 text-sm sm:text-base text-gray-600 max-w-2xl mx-auto" id="hero-subtitle">
                    Eliminating chaotic WhatsApp messaging with a centralized platform featuring live GPS tracking, verified driver profiles, and structured route timetables.
                </p>

                <!-- Interactive Live ETA Banner -->
                <div class="mt-6 inline-flex flex-wrap items-center justify-center gap-3 bg-white px-5 py-3 rounded-2xl border border-purple-100 shadow-sm max-w-2xl">
                    <div class="flex items-center gap-2 text-xs font-bold text-emerald-600">
                        <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping"></span>
                        <span id="hero-live-status">LIVE BUS ROUTE #104</span>
                    </div>
                    <span class="text-gray-300 hidden sm:inline">•</span>
                    <div class="text-xs font-semibold text-gray-700 flex items-center gap-1.5">
                        <i class="fa-solid fa-clock text-amiryx-purple"></i>
                        <span id="hero-eta-text">Next Pickup: <strong>Your Residence</strong> in <strong>6 Mins</strong></span>
                    </div>
                    <button onclick="simulateBusMovement()" class="text-xs font-bold text-amiryx-blue hover:underline ml-2">
                        <i class="fa-solid fa-arrows-rotate mr-1"></i> <span id="btn-refresh-map">Refresh GPS</span>
                    </button>
                </div>
            </div>
        </section>

        <!-- Interactive Dashboard Prototype Workspace -->
        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
            
            <!-- Dashboard Tab Navigation for Student View -->
            <div id="student-tabs-header" class="flex overflow-x-auto gap-2 mb-6 pb-2 border-b border-purple-200">
                <button onclick="switchStudentTab('map')" id="tab-btn-map" class="px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-amiryx-purple text-white shadow-sm flex items-center gap-2">
                    <i class="fa-solid fa-map-location-dot"></i>
                    <span id="tab-label-map">Live GPS Map</span>
                </button>
                <button onclick="switchStudentTab('driver')" id="tab-btn-driver" class="px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-white text-gray-600 hover:text-amiryx-purple border border-purple-100 flex items-center gap-2">
                    <i class="fa-solid fa-id-card"></i>
                    <span id="tab-label-driver">Verified Driver</span>
                </button>
                <button onclick="switchStudentTab('schedule')" id="tab-btn-schedule" class="px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-white text-gray-600 hover:text-amiryx-purple border border-purple-100 flex items-center gap-2">
                    <i class="fa-solid fa-calendar-days"></i>
                    <span id="tab-label-schedule">Route Schedule</span>
                </button>
                <button onclick="switchStudentTab('safety')" id="tab-btn-safety" class="px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-white text-gray-600 hover:text-amiryx-purple border border-purple-100 flex items-center gap-2">
                    <i class="fa-solid fa-shield-cat"></i>
                    <span id="tab-label-safety">Safety Center</span>
                </button>
            </div>

            <!-- VIEW 1: STUDENT DASHBOARD -->
            <div id="view-student" class="space-y-6">
                
                <!-- TAB 1: LIVE MAP TRACKING -->
                <div id="student-tab-map" class="block space-y-6">
                    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                        
                        <!-- Main Map Canvas Frame (8 Cols) -->
                        <div class="lg:col-span-8 bg-white rounded-3xl p-4 shadow-sm border border-purple-100">
                            <div class="flex items-center justify-between mb-3 px-2">
                                <div class="flex items-center gap-2">
                                    <span class="p-2 rounded-xl bg-amiryx-blue/10 text-amiryx-blue font-bold text-xs">
                                        <i class="fa-solid fa-route"></i>
                                    </span>
                                    <div>
                                        <h3 class="font-bold text-gray-900 text-sm" id="map-header-title">Live GPS Location Tracking</h3>
                                        <p class="text-xs text-gray-500" id="map-header-sub">Bus Route #104 • Princess Nourah University (PNU)</p>
                                    </div>
                                </div>
                                <div class="bg-blue-50 text-amiryx-blue px-3 py-1 rounded-full text-xs font-bold border border-blue-200">
                                    <i class="fa-solid fa-signal mr-1"></i> <span id="map-live-badge">Live Broadcast</span>
                                </div>
                            </div>

                            <!-- Map Visual Display -->
                            <div class="w-full h-80 sm:h-96 rounded-2xl map-grid-bg relative overflow-hidden border border-gray-200">
                                <svg class="absolute inset-0 w-full h-full pointer-events-none" xmlns="http://www.w3.org/2000/svg">
                                    <!-- Route Line -->
                                    <path id="route-path" d="M 50 280 C 120 280, 150 150, 260 150 C 370 150, 420 80, 580 80" fill="none" stroke="#FFFFFF" stroke-width="12" stroke-linecap="round"/>
                                    <path d="M 50 280 C 120 280, 150 150, 260 150 C 370 150, 420 80, 580 80" fill="none" stroke="#4C95D1" stroke-width="4" stroke-dasharray="6,6" stroke-linecap="round"/>
                                    
                                    <!-- University Destination Marker -->
                                    <g transform="translate(560, 55)">
                                        <circle cx="10" cy="10" r="18" fill="#9D78C6" opacity="0.2"/>
                                        <circle cx="10" cy="10" r="10" fill="#9D78C6"/>
                                        <text x="10" y="14" font-size="9" fill="white" text-anchor="middle" font-weight="bold">PNU</text>
                                    </g>
                                </svg>

                                <!-- Student Residence Pickup Marker -->
                                <div class="absolute bottom-16 left-12 sm:bottom-20 sm:left-16 flex flex-col items-center">
                                    <div class="bg-amiryx-purple text-white text-[10px] px-2 py-0.5 rounded-full font-bold shadow-md mb-1 whitespace-nowrap" id="marker-pickup-label">
                                        Your Pickup Residence
                                    </div>
                                    <div class="w-8 h-8 bg-amiryx-purple rounded-full flex items-center justify-center text-white ring-4 ring-purple-200 shadow-lg">
                                        <i class="fa-solid fa-house-user text-xs"></i>
                                    </div>
                                </div>

                                <!-- Moving Bus Marker -->
                                <div id="moving-bus" class="absolute bus-marker flex flex-col items-center z-10" style="left: 20%; top: 58%;">
                                    <div class="bg-white text-amiryx-textDark text-xs px-2.5 py-1 rounded-xl font-extrabold shadow-lg border border-purple-200 flex items-center gap-1 mb-1 animate-bounce">
                                        <i class="fa-solid fa-clock text-amiryx-purple"></i>
                                        <span id="eta-counter">6 min ETA</span>
                                    </div>
                                    <div class="w-11 h-11 bg-amiryx-blue rounded-full flex items-center justify-center text-white shadow-xl ring-4 ring-blue-200 pulse-ring">
                                        <i class="fa-solid fa-bus text-lg"></i>
                                    </div>
                                </div>

                                <!-- Floating Status Card -->
                                <div class="absolute bottom-3 right-3 left-3 sm:left-auto glass-panel p-3 rounded-2xl shadow-lg border border-white/50 max-w-sm">
                                    <div class="flex items-center gap-3">
                                        <div class="w-10 h-10 rounded-xl bg-amiryx-blue text-white flex items-center justify-center text-lg shrink-0">
                                            <i class="fa-solid fa-location-arrow"></i>
                                        </div>
                                        <div class="flex-grow">
                                            <div class="text-[10px] font-bold text-gray-500 uppercase tracking-wider" id="status-card-label">Current Route Alert</div>
                                            <div class="text-sm font-bold text-gray-900" id="status-card-title">Bus is coming to you!</div>
                                            <div class="text-xs text-amiryx-blue font-medium" id="status-card-sub">Next Stop: Your Residence • 1.2 km away</div>
                                        </div>
                                        <button onclick="simulateBusMovement()" class="p-2 rounded-xl bg-purple-100 hover:bg-purple-200 text-amiryx-purple transition text-xs font-bold shrink-0" title="Simulate Movement">
                                            <i class="fa-solid fa-play"></i>
                                        </button>
                                    </div>
                                </div>

                            </div>
                        </div>

                        <!-- Side Panel: Active Driver Quick Info (4 Cols) -->
                        <div class="lg:col-span-4 space-y-4">
                            
                            <!-- Driver Card -->
                            <div class="bg-white rounded-3xl p-5 shadow-sm border border-purple-100 space-y-4">
                                <div class="flex items-center gap-3">
                                    <div class="relative">
                                        <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=150&auto=format&fit=crop&q=80" alt="Verified Driver Profile" class="w-14 h-14 rounded-2xl object-cover ring-2 ring-amiryx-purple/30">
                                        <span class="absolute -bottom-1 -right-1 bg-emerald-500 text-white w-5 h-5 rounded-full flex items-center justify-center text-[10px]" title="Amiryx Identity Checked">
                                            <i class="fa-solid fa-check"></i>
                                        </span>
                                    </div>
                                    <div>
                                        <div class="flex items-center gap-1.5">
                                            <h4 class="font-bold text-gray-900 text-sm" id="driver-card-name">Aisha Al-Otaibi</h4>
                                            <span class="bg-emerald-100 text-emerald-800 text-[10px] px-1.5 py-0.5 rounded font-bold" id="driver-verified-badge">Verified</span>
                                        </div>
                                        <p class="text-xs text-gray-500 mt-0.5">Toyota Coaster • Plate: 4892 KSA</p>
                                        <p class="text-xs text-amiryx-purple font-bold mt-1">Bus Route #104</p>
                                    </div>
                                </div>

                                <!-- Action Buttons -->
                                <div class="grid grid-cols-2 gap-2 pt-2 border-t border-gray-100">
                                    <button onclick="triggerCallModal()" class="py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-600 text-white font-bold text-xs transition flex items-center justify-center gap-1.5">
                                        <i class="fa-solid fa-phone"></i>
                                        <span id="btn-call-text">Direct Call</span>
                                    </button>
                                    <button onclick="playDriverVoiceNote()" class="py-2.5 rounded-xl bg-amiryx-bgLight hover:bg-purple-100 text-amiryx-purpleDark font-bold text-xs transition border border-purple-200 flex items-center justify-center gap-1.5">
                                        <i class="fa-solid fa-volume-high"></i>
                                        <span id="btn-audio-text">Audio Note</span>
                                    </button>
                                </div>
                            </div>

                            <!-- Scheduled Stop Quick Timeline -->
                            <div class="bg-white rounded-3xl p-5 shadow-sm border border-purple-100">
                                <h4 class="font-bold text-gray-900 text-xs uppercase tracking-wider mb-3 text-amiryx-purple" id="upcoming-stops-title">Upcoming Stops Today</h4>
                                <div class="space-y-3 text-xs">
                                    <div class="flex items-center justify-between p-2 rounded-xl bg-emerald-50 text-emerald-800 font-medium">
                                        <span>06:45 AM • Al-Nakhil Pickup</span>
                                        <i class="fa-solid fa-circle-check text-emerald-600"></i>
                                    </div>
                                    <div class="flex items-center justify-between p-2 rounded-xl bg-blue-50 text-amiryx-blue font-bold border border-blue-200">
                                        <span>07:05 AM • Your Residence</span>
                                        <span class="animate-pulse">Next</span>
                                    </div>
                                    <div class="flex items-center justify-between p-2 rounded-xl bg-gray-50 text-gray-600">
                                        <span>07:30 AM • PNU Gate 4</span>
                                        <span>Pending</span>
                                    </div>
                                </div>
                            </div>

                        </div>

                    </div>
                </div>

                <!-- TAB 2: VERIFIED DRIVER DETAILS -->
                <div id="student-tab-driver" class="hidden space-y-6">
                    <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-100 max-w-3xl mx-auto">
                        <div class="flex flex-col sm:flex-row items-center gap-6">
                            <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=200&auto=format&fit=crop&q=80" alt="Verified Driver" class="w-24 h-24 rounded-3xl object-cover ring-4 ring-purple-100">
                            <div class="space-y-2 text-center sm:text-left rtl:sm:text-right">
                                <div class="flex flex-wrap items-center justify-center sm:justify-start gap-2">
                                    <h3 class="text-xl font-bold text-gray-900">Aisha Al-Otaibi</h3>
                                    <span class="bg-emerald-100 text-emerald-800 text-xs px-2.5 py-1 rounded-full font-bold">
                                        <i class="fa-solid fa-shield-check mr-1"></i> Verified Amiryx Driver
                                    </span>
                                </div>
                                <p class="text-xs text-gray-500">Official Transport License #KSA-883920 • Female Dedicated Bus Service</p>
                                <div class="flex items-center justify-center sm:justify-start gap-4 text-xs font-semibold text-gray-700 pt-1">
                                    <span><i class="fa-solid fa-star text-amber-500 mr-1"></i>4.9 / 5.0 Rating</span>
                                    <span><i class="fa-solid fa-bus text-amiryx-blue mr-1"></i>Bus #104 (Toyota Coaster)</span>
                                </div>
                            </div>
                        </div>

                        <!-- Direct Communication Tools -->
                        <div class="mt-6 pt-6 border-t border-gray-100 grid grid-cols-1 sm:grid-cols-3 gap-3">
                            <button onclick="triggerCallModal()" class="py-3 rounded-2xl bg-emerald-500 hover:bg-emerald-600 text-white font-bold text-xs transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-phone"></i>
                                <span id="driver-full-call">Direct Phone Call</span>
                            </button>
                            <button onclick="playDriverVoiceNote()" class="py-3 rounded-2xl bg-amiryx-purple hover:bg-amiryx-purpleDark text-white font-bold text-xs transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-microphone"></i>
                                <span id="driver-full-voice">Listen to Driver Note</span>
                            </button>
                            <button onclick="triggerInAppChat()" class="py-3 rounded-2xl bg-amiryx-blue hover:bg-amiryx-blueDark text-white font-bold text-xs transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-comments"></i>
                                <span id="driver-full-chat">In-App Quick Message</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- TAB 3: STRUCTURED ROUTE SCHEDULE -->
                <div id="student-tab-schedule" class="hidden space-y-6">
                    <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-100 max-w-4xl mx-auto">
                        <div class="flex items-center justify-between mb-6">
                            <div>
                                <h3 class="font-bold text-gray-900 text-base" id="schedule-title">Daily Timetable & Route Stops</h3>
                                <p class="text-xs text-gray-500" id="schedule-sub">Replaces chaotic WhatsApp group threads with dedicated scheduled visibility.</p>
                            </div>
                            <span class="px-3 py-1 bg-purple-100 text-amiryx-purpleDark font-bold text-xs rounded-full">Princess Nourah University</span>
                        </div>

                        <div class="divide-y divide-gray-100">
                            <!-- Stop 1 -->
                            <div class="py-3 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-8 h-8 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center font-bold text-xs">1</div>
                                    <div>
                                        <div class="font-bold text-sm text-gray-900">Al-Nakhil Residence District</div>
                                        <div class="text-xs text-gray-500">Pick up 3 Students</div>
                                    </div>
                                </div>
                                <span class="text-xs font-bold text-gray-700">06:45 AM</span>
                            </div>

                            <!-- Stop 2 (Your Stop) -->
                            <div class="py-3 flex items-center justify-between bg-purple-50/50 -mx-6 px-6 border-l-4 border-amiryx-purple">
                                <div class="flex items-center gap-3">
                                    <div class="w-8 h-8 rounded-full bg-amiryx-purple text-white flex items-center justify-center font-bold text-xs">2</div>
                                    <div>
                                        <div class="font-bold text-sm text-amiryx-purpleDark" id="schedule-your-stop">Your Residence (Pickup Stop #2)</div>
                                        <div class="text-xs text-amiryx-purple font-semibold">Scheduled Target</div>
                                    </div>
                                </div>
                                <span class="text-xs font-extrabold text-amiryx-purpleDark">07:05 AM</span>
                            </div>

                            <!-- Stop 3 -->
                            <div class="py-3 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-8 h-8 rounded-full bg-gray-100 text-gray-600 flex items-center justify-center font-bold text-xs">3</div>
                                    <div>
                                        <div class="font-bold text-sm text-gray-900">PNU Campus Main Entrance (Gate 4)</div>
                                        <div class="text-xs text-gray-500">Main Drop-off Terminal</div>
                                    </div>
                                </div>
                                <span class="text-xs font-bold text-gray-700">07:30 AM</span>
                            </div>

                            <!-- Stop 4 -->
                            <div class="py-3 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-8 h-8 rounded-full bg-gray-100 text-gray-600 flex items-center justify-center font-bold text-xs">4</div>
                                    <div>
                                        <div class="font-bold text-sm text-gray-900">College of Computer & Information Sciences</div>
                                        <div class="text-xs text-gray-500">Final Campus Drop-off</div>
                                    </div>
                                </div>
                                <span class="text-xs font-bold text-gray-700">07:45 AM</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- TAB 4: SAFETY CENTER -->
                <div id="student-tab-safety" class="hidden space-y-6">
                    <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-100 max-w-3xl mx-auto space-y-6">
                        <div class="flex items-center gap-3 border-b border-gray-100 pb-4">
                            <div class="w-12 h-12 rounded-2xl bg-red-50 text-red-500 flex items-center justify-center text-xl font-bold">
                                <i class="fa-solid fa-shield-heart"></i>
                            </div>
                            <div>
                                <h3 class="font-bold text-gray-900 text-base" id="safety-center-title">Student Safety & Emergency Protocol</h3>
                                <p class="text-xs text-gray-500" id="safety-center-sub">Multi-layer safety measures for secure female student transportation.</p>
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div class="p-4 rounded-2xl bg-purple-50/50 border border-purple-100 space-y-1">
                                <div class="text-xs font-bold text-amiryx-purple" id="safety-card1-title"><i class="fa-solid fa-id-badge mr-1"></i> Student Verification</div>
                                <div class="text-xs text-gray-600" id="safety-card1-desc">Official university ID checks required for all registered commuters.</div>
                            </div>
                            <div class="p-4 rounded-2xl bg-blue-50/50 border border-blue-100 space-y-1">
                                <div class="text-xs font-bold text-amiryx-blue" id="safety-card2-title"><i class="fa-solid fa-location-crosshairs mr-1"></i> Live Broadcast</div>
                                <div class="text-xs text-gray-600" id="safety-card2-desc">Continuous trip tracking accessible during active route hours.</div>
                            </div>
                        </div>

                        <div class="pt-2 text-center">
                            <button onclick="triggerEmergencyModal()" class="w-full sm:w-auto px-8 py-3.5 rounded-2xl bg-red-500 hover:bg-red-600 text-white font-extrabold text-xs transition shadow-lg inline-flex items-center justify-center gap-2">
                                <i class="fa-solid fa-triangle-exclamation"></i>
                                <span id="btn-trigger-sos-full">Trigger Emergency SOS Alert</span>
                            </button>
                        </div>
                    </div>
                </div>

            </div>

            <!-- VIEW 2: DRIVER DASHBOARD (Simplified Non-Tech Savvy UI) -->
            <div id="view-driver" class="hidden space-y-6">
                
                <div class="bg-amber-50 border border-amber-200 rounded-2xl p-4 text-xs text-amber-900 flex items-center gap-3">
                    <i class="fa-solid fa-triangle-exclamation text-lg text-amber-600 shrink-0"></i>
                    <div>
                        <strong id="driver-mode-title">Driver Simplified Interface:</strong> 
                        <span id="driver-mode-sub">Ultra-clear interface with large buttons for non-tech-savvy driver operation while stopped.</span>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    
                    <!-- Trip Commands -->
                    <div class="bg-white rounded-3xl p-6 shadow-sm border border-purple-100 space-y-4">
                        <h3 class="font-bold text-gray-900 text-sm mb-2" id="driver-commands-title">Main Route Controls</h3>
                        
                        <button id="btn-start-route" onclick="toggleRouteStart()" class="w-full py-5 rounded-2xl bg-emerald-500 hover:bg-emerald-600 text-white font-extrabold text-base transition shadow-md flex items-center justify-center gap-3">
                            <i class="fa-solid fa-circle-play text-xl"></i>
                            <span id="text-start-route">START PICKUP ROUTE</span>
                        </button>

                        <button onclick="triggerAnnouncementModal()" class="w-full py-4 rounded-2xl bg-amiryx-blue hover:bg-amiryx-blueDark text-white font-bold text-xs transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-bullhorn"></i>
                            <span id="text-audio-announce">Send Voice Alert to Students</span>
                        </button>

                        <button onclick="triggerCallSupport()" class="w-full py-3.5 rounded-2xl bg-gray-100 hover:bg-gray-200 text-gray-800 font-bold text-xs transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-headset"></i>
                            <span id="text-call-support">Call Dispatcher / Support</span>
                        </button>
                    </div>

                    <!-- Student Pickup Checklist -->
                    <div class="md:col-span-2 bg-white rounded-3xl p-6 shadow-sm border border-purple-100">
                        <div class="flex items-center justify-between mb-4">
                            <div>
                                <h3 class="font-bold text-gray-900 text-base" id="roster-title">Student Pickup Checklist</h3>
                                <p class="text-xs text-gray-500" id="roster-sub">Tap student name to confirm boarding</p>
                            </div>
                            <span class="bg-purple-100 text-amiryx-purpleDark font-bold text-xs px-3 py-1 rounded-full" id="roster-count">
                                2 / 4 Boarded
                            </span>
                        </div>

                        <div class="space-y-3">
                            <!-- Student Item 1 -->
                            <div class="p-4 rounded-2xl border border-gray-200 flex items-center justify-between bg-emerald-50/50">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 rounded-full bg-emerald-500 text-white flex items-center justify-center font-bold">
                                        <i class="fa-solid fa-check"></i>
                                    </div>
                                    <div>
                                        <div class="font-bold text-gray-900 text-sm">Noura Al-Dosari</div>
                                        <div class="text-xs text-gray-500">Stop #1 • Al-Nakhil</div>
                                    </div>
                                </div>
                                <span class="text-xs font-bold text-emerald-700 bg-emerald-100 px-3 py-1 rounded-lg">Boarded 06:46 AM</span>
                            </div>

                            <!-- Student Item 2 (Active Demo Student) -->
                            <div class="p-4 rounded-2xl border-2 border-amiryx-blue flex items-center justify-between bg-blue-50/30">
                                <div class="flex items-center gap-3">
                                    <button onclick="toggleStudentBoarded(this)" class="w-10 h-10 rounded-full border-2 border-amiryx-blue text-amiryx-blue flex items-center justify-center hover:bg-amiryx-blue hover:text-white transition">
                                        <i class="fa-solid fa-plus"></i>
                                    </button>
                                    <div>
                                        <div class="font-bold text-gray-900 text-sm">Reem Al-Qahtani (You)</div>
                                        <div class="text-xs text-amiryx-blue font-semibold">Stop #2 • Next Arrival</div>
                                    </div>
                                </div>
                                <button onclick="toggleStudentBoarded(this.previousElementSibling.querySelector('button'))" class="px-4 py-2 rounded-xl bg-amiryx-blue text-white text-xs font-bold hover:bg-amiryx-blueDark transition">
                                    Confirm Boarding
                                </button>
                            </div>

                            <!-- Student Item 3 -->
                            <div class="p-4 rounded-2xl border border-gray-200 flex items-center justify-between">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 rounded-full border-2 border-gray-300 text-gray-300 flex items-center justify-center font-bold">
                                        <i class="fa-solid fa-user"></i>
                                    </div>
                                    <div>
                                        <div class="font-bold text-gray-900 text-sm">Lujain Al-Harbi</div>
                                        <div class="text-xs text-gray-500">Stop #3 • Al-Yasmin</div>
                                    </div>
                                </div>
                                <span class="text-xs font-bold text-gray-400 bg-gray-100 px-3 py-1 rounded-lg">Pending Pickup</span>
                            </div>

                        </div>
                    </div>

                </div>

            </div>

        </section>

        <!-- 6 Core Features Grid (No Payment Mentioned) -->
        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12 border-t border-purple-100">
            <div class="text-center mb-10">
                <h2 class="text-2xl sm:text-3xl font-extrabold text-amiryx-textDark" id="features-title">
                    6 Core Features of Amiryx V1
                </h2>
                <p class="text-gray-600 text-xs sm:text-sm mt-2 max-w-xl mx-auto" id="features-sub">
                    Designed specifically for female university students and bus drivers across Saudi Arabia.
                </p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                
                <!-- Feature 1 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-blue-50 text-amiryx-blue flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-location-dot"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-sm mb-1" id="f1-title">1. Live GPS Location Tracking</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f1-desc">
                        Interactive map simulation displaying "Bus coming to you" status and updated arrival time predictions.
                    </p>
                </div>

                <!-- Feature 2 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-purple-50 text-amiryx-purple flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-id-card"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-base mb-1" id="f2-title">2. Verified Driver Profile</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f2-desc">
                        Verified driver cards with direct phone connectivity, voice notes, and instant communication.
                    </p>
                </div>

                <!-- Feature 3 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-red-50 text-red-500 flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-shield-halved"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-base mb-1" id="f3-title">3. Student Safety Protocol</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f3-desc">
                        Student identification verification checks, boarding logs, and instant SOS emergency triggers.
                    </p>
                </div>

                <!-- Feature 4 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-amber-50 text-amber-600 flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-hand-pointer"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-base mb-1" id="f4-title">4. Beginner-Friendly UI</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f4-desc">
                        Ultra-simple interface created for both non-tech-savvy drivers and students.
                    </p>
                </div>

                <!-- Feature 5 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-calendar-days"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-base mb-1" id="f5-title">5. Structured Route Scheduling</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f5-desc">
                        Dedicated route timetables and stops that eliminate unorganized WhatsApp messaging threads.
                    </p>
                </div>

                <!-- Feature 6 -->
                <div class="bg-white p-6 rounded-3xl border border-purple-100 shadow-sm hover:shadow-md transition">
                    <div class="w-12 h-12 rounded-2xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-xl mb-4">
                        <i class="fa-solid fa-language"></i>
                    </div>
                    <h3 class="font-bold text-gray-900 text-base mb-1" id="f6-title">6. Universal Multi-Language</h3>
                    <p class="text-xs text-gray-600 leading-relaxed" id="f6-desc">
                        Bilingual Arabic/English switcher adapting both typography and layout orientation (LTR/RTL).
                    </p>
                </div>

            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-purple-100 py-6 px-4 sm:px-6 lg:px-8 mt-auto">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-2">
                <span class="font-bold text-amiryx-blue text-sm">AMIRYX</span>
                <span class="text-xs text-gray-500">© 2026 Amiryx Platform. All rights reserved.</span>
            </div>
            <div class="text-xs text-gray-500 flex items-center gap-3">
                <span>PNU & KSU Female Commute System</span>
                <span>•</span>
                <span id="footer-slogan">"Safety and comfort in every step."</span>
            </div>
        </div>
    </footer>

    <!-- INTERACTIVE MODALS -->

    <!-- Call Modal -->
    <div id="modal-call" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-3xl max-w-sm w-full p-6 text-center space-y-4 shadow-2xl">
            <div class="w-14 h-14 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center text-xl mx-auto animate-bounce">
                <i class="fa-solid fa-phone"></i>
            </div>
            <h3 class="font-bold text-base text-gray-900">Direct Contact: Aisha Al-Otaibi</h3>
            <p class="text-xs text-gray-500">Amiryx Direct Line (Driver #104)</p>
            <div class="py-2 text-lg font-bold text-amiryx-blue">050-XXXX-892</div>
            <button onclick="closeModal('modal-call')" class="w-full py-2.5 rounded-xl bg-gray-100 text-gray-700 font-bold text-xs">Close</button>
        </div>
    </div>

    <!-- Emergency SOS Modal -->
    <div id="modal-sos" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 text-center space-y-4 shadow-2xl border-2 border-red-500">
            <div class="w-14 h-14 bg-red-100 text-red-600 rounded-full flex items-center justify-center text-xl mx-auto">
                <i class="fa-solid fa-triangle-exclamation"></i>
            </div>
            <h3 class="font-bold text-lg text-red-600">Emergency SOS Activated</h3>
            <p class="text-xs text-gray-600">Your live GPS location has been transmitted to Amiryx Safety Control and University Security dispatch.</p>
            <div class="p-3 bg-red-50 rounded-xl text-xs text-red-800 font-mono text-left rtl:text-right">
                GPS Coordinates: 24.8468° N, 46.7241° E<br>
                Route: #104 (PNU Campus Terminal)
            </div>
            <button onclick="closeModal('modal-sos')" class="w-full py-2.5 rounded-xl bg-red-600 text-white font-bold text-xs hover:bg-red-700 transition">
                Cancel Alert (False Alarm)
            </button>
        </div>
    </div>

    <!-- In-App Chat Modal -->
    <div id="modal-chat" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-3xl max-w-sm w-full p-5 space-y-4 shadow-2xl">
            <div class="flex items-center justify-between border-b border-gray-100 pb-3">
                <h3 class="font-bold text-sm text-gray-900">Message Aisha (Driver #104)</h3>
                <button onclick="closeModal('modal-chat')" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="space-y-2">
                <button onclick="sendQuickMsg('I am waiting at the front door.')" class="w-full text-left rtl:text-right p-2.5 rounded-xl bg-purple-50 text-xs font-semibold text-amiryx-purpleDark hover:bg-purple-100">"I am waiting at the main entrance."</button>
                <button onclick="sendQuickMsg('I need 2 extra minutes please.')" class="w-full text-left rtl:text-right p-2.5 rounded-xl bg-purple-50 text-xs font-semibold text-amiryx-purpleDark hover:bg-purple-100">"I need 2 extra minutes please."</button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT & INTERACTION ENGINE -->
    <script>
        // Language Translations Object
        const translations = {
            en: {
                headerSlogan: '"Safety and comfort in every step."',
                roleStudent: 'Student',
                roleDriver: 'Driver View',
                langBtn: 'العربية',
                sosBtn: 'SOS',
                heroBadge: '🇸🇦 Dedicated Student Transit Management Platform',
                heroTitle: 'Safe, Predictable & Organized University Commutes',
                heroSubtitle: 'Eliminating chaotic WhatsApp messaging with a centralized platform featuring live GPS tracking, verified driver profiles, and structured route timetables.',
                heroLiveStatus: 'LIVE BUS ROUTE #104',
                heroEtaText: 'Next Pickup: <strong>Your Residence</strong> in <strong>6 Mins</strong>',
                btnRefreshMap: 'Refresh GPS',
                tabMap: 'Live GPS Map',
                tabDriver: 'Verified Driver',
                tabSchedule: 'Route Schedule',
                tabSafety: 'Safety Center',
                mapTitle: 'Live GPS Location Tracking',
                mapSub: 'Bus Route #104 • Princess Nourah University (PNU)',
                mapLiveBadge: 'Live Broadcast',
                markerPickup: 'Your Pickup Residence',
                etaCounter: '6 min ETA',
                statusLabel: 'Current Route Alert',
                statusTitle: 'Bus is coming to you!',
                statusSub: 'Next Stop: Your Residence • 1.2 km away',
                driverCardName: 'Aisha Al-Otaibi',
                driverVerified: 'Verified',
                btnCallText: 'Direct Call',
                btnAudioText: 'Audio Note',
                                upcomingStopsTitle: 'Upcoming Stops Today',
                driverFullCall: 'Direct Phone Call',
                driverFullVoice: 'Listen to Driver Note',
                driverFullChat: 'In-App Quick Message',
                scheduleTitle: 'Daily Timetable & Route Stops',
                scheduleSub: 'Replaces chaotic WhatsApp group threads with dedicated scheduled visibility.',
                scheduleYourStop: 'Your Residence (Pickup Stop #2)',
                safetyCenterTitle: 'Student Safety & Emergency Protocol',
                safetyCenterSub: 'Multi-layer safety measures for secure female student transportation.',
                safetyCard1Title: 'Student Verification',
                safetyCard1Desc: 'Official university ID checks required for all registered commuters.',
                safetyCard2Title: 'Live Broadcast',
                safetyCard2Desc: 'Continuous trip tracking accessible during active route hours.',
                btnTriggerSosFull: 'Trigger Emergency SOS Alert',
                driverModeTitle: 'Driver Simplified Interface:',
                driverModeSub: 'Ultra-clear interface with large buttons for non-tech-savvy driver operation while stopped.',
                driverCommandsTitle: 'Main Route Controls',
                startRoute: 'START PICKUP ROUTE',
                audioAnnounce: 'Send Voice Alert to Students',
                callSupport: 'Call Dispatcher / Support',
                rosterTitle: 'Student Pickup Checklist',
                rosterSub: 'Tap student name to confirm boarding',
                rosterCount: '2 / 4 Boarded',
                featuresTitle: '6 Core Features of Amiryx V1',
                featuresSub: 'Designed specifically for female university students and bus drivers across Saudi Arabia.',
                f1Title: '1. Live GPS Location Tracking',
                f1Desc: 'Interactive map simulation displaying "Bus coming to you" status and updated arrival time predictions.',
                f2Title: '2. Verified Driver Profile',
                f2Desc: 'Verified driver cards with direct phone connectivity, voice notes, and instant communication.',
                f3Title: '3. Student Safety Protocol',
                f3Desc: 'Student identification verification checks, boarding logs, and instant SOS emergency triggers.',
                f4Title: '4. Beginner-Friendly UI',
                f4Desc: 'Ultra-simple interface created for both non-tech-savvy drivers and students.',
                f5Title: '5. Structured Route Scheduling',
                f5Desc: 'Dedicated route timetables and stops that eliminate unorganized WhatsApp messaging threads.',
                f6Title: '6. Universal Multi-Language',
                f6Desc: 'Bilingual Arabic/English switcher adapting both typography and layout orientation (LTR/RTL).',
                footerSlogan: '"Safety and comfort in every step."'
            },
            ar: {
                headerSlogan: '"الأمان والراحة في كل خطوة."',
                roleStudent: 'الطالبة',
                roleDriver: 'عرض السائق',
                langBtn: 'English',
                sosBtn: 'طوارئ',
                heroBadge: '🇸🇦 منصة لإدارة النقل الجامعي للطالبات',
                heroTitle: 'تنقل جامعي آمن، ومضمون، ومُنظّم',
                heroSubtitle: 'استبدال رسائل الواتساب غير المنظمة بمنصة مركزية للتتبع المباشر، ملفات السائقين المعتمدة، والجداول الزمنية الواضحة.',
                heroLiveStatus: 'مسار الحافلة المباشر #104',
                heroEtaText: 'المحطة القادمة: <strong>منزلك</strong> خلال <strong>6 دقائق</strong>',
                btnRefreshMap: 'تحديث GPS',
                tabMap: 'خريطة التتبع',
                tabDriver: 'السائق المعتمد',
                tabSchedule: 'جدول المسار',
                tabSafety: 'مركز الأمان',
                mapTitle: 'تتبع الموقع المباشر GPS',
                mapSub: 'حافلة رقم #104 • جامعة الأميرة نورة (PNU)',
                mapLiveBadge: 'بث مباشر',
                markerPickup: 'موقع التوقف الخاص بك',
                etaCounter: 'الوصول خلال 6 دقائق',
                statusLabel: 'تنبيه المسار الحالي',
                statusTitle: 'الحافلة قادمة إليك الآن!',
                statusSub: 'المحطة القادمة: منزلك • على بعد 1.2 كم',
                driverCardName: 'عائشة العتيبي',
                driverVerified: 'معتمدة',
                btnCallText: 'اتصال مباشر',
                btnAudioText: 'رسالة صوتية',
                upcomingStopsTitle: 'المحطات القادمة اليوم',
                driverFullCall: 'اتصال هاتف مباشر',
                driverFullVoice: 'استماع للملاحظة الصوتية',
                driverFullChat: 'إرسال رسالة سريعة',
                scheduleTitle: 'جدول الرحلة والمحطات اليومية',
                scheduleSub: 'يستبدل محادثات الواتساب العشوائية بجدول زمني واضح ومحدد.',
                scheduleYourStop: 'منزلك (المحطة رقم #2)',
                safetyCenterTitle: 'بروتوكول أمان الطالبات والطوارئ',
                safetyCenterSub: 'إجراءات أمان متعددة الشروط لنقل جامعي آمن للطالبات.',
                safetyCard1Title: 'التحقق من هوية الطالبة',
                safetyCard1Desc: 'مطابقة البطاقة الجامعية لجميع الطالبات المسجلات.',
                safetyCard2Title: 'البث المباشر للمسار',
                safetyCard2Desc: 'تتبع مباشر متاح خلال ساعات الرحلة الفعالة.',
                btnTriggerSosFull: 'تفعيل إرسال إشعار الطوارئ SOS',
                driverModeTitle: 'واجهة السائق المبسطة:',
                driverModeSub: 'تصميم بلمسات واضحة وأزرار كبيرة لتسهيل الاستخدام عند التوقف.',
                driverCommandsTitle: 'أوامر التحكم الرئيسية',
                startRoute: 'بدء مسار نقل الطالبات',
                audioAnnounce: 'إرسال تنبيه صوتي للطالبات',
                callSupport: 'الاتصال بالدعم والتوجيه',
                rosterTitle: 'قائمة حضور وصعود الطالبات',
                rosterSub: 'انقر على اسم الطالبة لتأكيد الصعود',
                rosterCount: 'تم صعود 2 من 4',
                featuresTitle: 'مميزات عاميريكس V1 الستة',
                featuresSub: 'صُممت خصيصاً لطالبات الجامعات وسائقي الحافلات في المملكة العربية السعودية.',
                f1Title: '1. تتبع الموقع المباشر GPS',
                f1Desc: 'خريطة تفاعلية تعرض حالة "الحافلة قادمة إليك" مع وقت وصول متوقع.',
                f2Title: '2. ملف السائق المعتمد',
                f2Desc: 'بطاقة سائق معتمدة تتيح الاتصال المباشر والرسائل الصوتية.',
                f3Title: '3. بروتوكول أمان الطالبات',
                f3Desc: 'تأكيد البطاقة الجامعية، سجلات الصعود، وزر طوارئ SOS فوري.',
                f4Title: '4. واجهة سهلة الاستخدام',
                f4Desc: 'تصميم بسيط ومناسب للسائقين والطالبات لسهولة التعامل.',
                f5Title: '5. جدول مسارات منظم',
                f5Desc: 'جداول ومحطات محددة تجنبك محادثات مجموعات الواتساب المربكة.',
                f6Title: '6. دعم اللغات المتعددة',
                f6Desc: 'مبدل لغوي بين العربية والإنجليزية يغير النصوص واتجاه الصفحات (RTL/LTR).',
                footerSlogan: '"الأمان والراحة في كل خطوة."'
            }
        };

        let currentLang = 'en';
        let routeActive = false;

        // View Switching Function (Role)
        function switchView(role) {
            const studentView = document.getElementById('view-student');
            const driverView = document.getElementById('view-driver');
            const studentTabsHeader = document.getElementById('student-tabs-header');
            const navStudentRole = document.getElementById('nav-student-role');
            const navDriverRole = document.getElementById('nav-driver-role');

            if (role === 'student') {
                studentView.classList.remove('hidden');
                driverView.classList.add('hidden');
                studentTabsHeader.classList.remove('hidden');
                
                navStudentRole.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 bg-amiryx-purple text-white shadow-sm";
                navDriverRole.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 text-gray-600 hover:text-amiryx-purple";
            } else {
                studentView.classList.add('hidden');
                driverView.classList.remove('hidden');
                studentTabsHeader.classList.add('hidden');

                navDriverRole.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 bg-amiryx-purple text-white shadow-sm";
                navStudentRole.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition flex items-center gap-1.5 text-gray-600 hover:text-amiryx-purple";
            }
        }

        // Student Dashboard Internal Tab Switching
        function switchStudentTab(tab) {
            const tabs = ['map', 'driver', 'schedule', 'safety'];
            tabs.forEach(t => {
                const el = document.getElementById(`student-tab-${t}`);
                const btn = document.getElementById(`tab-btn-${t}`);
                if (t === tab) {
                    el.classList.remove('hidden');
                    btn.className = "px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-amiryx-purple text-white shadow-sm flex items-center gap-2";
                } else {
                    el.classList.add('hidden');
                    btn.className = "px-4 py-2 rounded-xl text-xs font-bold transition whitespace-nowrap bg-white text-gray-600 hover:text-amiryx-purple border border-purple-100 flex items-center gap-2";
                }
            });
        }

        // Language Switcher Function (RTL/LTR Layout Engine)
        function toggleLanguage() {
            currentLang = currentLang === 'en' ? 'ar' : 'en';
            const html = document.documentElement;
            
            if (currentLang === 'ar') {
                html.setAttribute('dir', 'rtl');
                html.classList.add('font-arabic');
            } else {
                html.setAttribute('dir', 'ltr');
                html.classList.remove('font-arabic');
            }

            const langData = translations[currentLang];

            // Update UI Element Translations
            document.getElementById('header-slogan').innerText = langData.headerSlogan;
            document.getElementById('role-student-label').innerText = langData.roleStudent;
            document.getElementById('role-driver-label').innerText = langData.roleDriver;
            document.getElementById('lang-btn-text').innerText = langData.langBtn;
            document.getElementById('sos-btn-text').innerText = langData.sosBtn;
            document.getElementById('hero-badge').innerText = langData.heroBadge;
            document.getElementById('hero-title').innerText = langData.heroTitle;
            document.getElementById('hero-subtitle').innerText = langData.heroSubtitle;
            document.getElementById('hero-live-status').innerText = langData.heroLiveStatus;
            document.getElementById('hero-eta-text').innerHTML = langData.heroEtaText;
            document.getElementById('btn-refresh-map').innerText = langData.btnRefreshMap;
            document.getElementById('tab-label-map').innerText = langData.tabMap;
            document.getElementById('tab-label-driver').innerText = langData.tabDriver;
            document.getElementById('tab-label-schedule').innerText = langData.tabSchedule;
            document.getElementById('tab-label-safety').innerText = langData.tabSafety;
            document.getElementById('map-header-title').innerText = langData.mapTitle;
            document.getElementById('map-header-sub').innerText = langData.mapSub;
            document.getElementById('map-live-badge').innerText = langData.mapLiveBadge;
            document.getElementById('marker-pickup-label').innerText = langData.markerPickup;
            document.getElementById('eta-counter').innerText = langData.etaCounter;
            document.getElementById('status-card-label').innerText = langData.statusLabel;
            document.getElementById('status-card-title').innerText = langData.statusTitle;
            document.getElementById('status-card-sub').innerText = langData.statusSub;
            document.getElementById('driver-card-name').innerText = langData.driverCardName;
            document.getElementById('driver-verified-badge').innerText = langData.driverVerified;
            document.getElementById('btn-call-text').innerText = langData.btnCallText;
            document.getElementById('btn-audio-text').innerText = langData.btnAudioText;
            document.getElementById('upcoming-stops-title').innerText = langData.upcomingStopsTitle;
            document.getElementById('driver-full-call').innerText = langData.driverFullCall;
            document.getElementById('driver-full-voice').innerText = langData.driverFullVoice;
            document.getElementById('driver-full-chat').innerText = langData.driverFullChat;
            document.getElementById('schedule-title').innerText = langData.scheduleTitle;
            document.getElementById('schedule-sub').innerText = langData.scheduleSub;
            document.getElementById('schedule-your-stop').innerText = langData.scheduleYourStop;
            document.getElementById('safety-center-title').innerText = langData.safetyCenterTitle;
            document.getElementById('safety-center-sub').innerText = langData.safetyCenterSub;
            document.getElementById('safety-card1-title').innerHTML = `<i class="fa-solid fa-id-badge mr-1"></i> ${langData.safetyCard1Title}`;
            document.getElementById('safety-card1-desc').innerText = langData.safetyCard1Desc;
            document.getElementById('safety-card2-title').innerHTML = `<i class="fa-solid fa-location-crosshairs mr-1"></i> ${langData.safetyCard2Title}`;
            document.getElementById('safety-card2-desc').innerText = langData.safetyCard2Desc;
            document.getElementById('btn-trigger-sos-full').innerText = langData.btnTriggerSosFull;
            document.getElementById('driver-mode-title').innerText = langData.driverModeTitle;
            document.getElementById('driver-mode-sub').innerText = langData.driverModeSub;
            document.getElementById('driver-commands-title').innerText = langData.driverCommandsTitle;
            document.getElementById('text-start-route').innerText = routeActive ? (currentLang === 'ar' ? 'إيقاف المسار' : 'STOP ROUTE') : langData.startRoute;
            document.getElementById('text-audio-announce').innerText = langData.audioAnnounce;
            document.getElementById('text-call-support').innerText = langData.callSupport;
            document.getElementById('roster-title').innerText = langData.rosterTitle;
            document.getElementById('roster-sub').innerText = langData.rosterSub;
            document.getElementById('roster-count').innerText = langData.rosterCount;
            document.getElementById('features-title').innerText = langData.featuresTitle;
            document.getElementById('features-sub').innerText = langData.featuresSub;
            document.getElementById('f1-title').innerText = langData.f1Title;
            document.getElementById('f1-desc').innerText = langData.f1Desc;
            document.getElementById('f2-title').innerText = langData.f2Title;
            document.getElementById('f2-desc').innerText = langData.f2Desc;
            document.getElementById('f3-title').innerText = langData.f3Title;
            document.getElementById('f3-desc').innerText = langData.f3Desc;
            document.getElementById('f4-title').innerText = langData.f4Title;
            document.getElementById('f4-desc').innerText = langData.f4Desc;
            document.getElementById('f5-title').innerText = langData.f5Title;
            document.getElementById('f5-desc').innerText = langData.f5Desc;
            document.getElementById('f6-title').innerText = langData.f6Title;
            document.getElementById('f6-desc').innerText = langData.f6Desc;
            document.getElementById('footer-slogan').innerText = langData.footerSlogan;

            showToast(currentLang === 'ar' ? 'تم تحويل اللغـة إلى العربيـة' : 'Switched to English Interface', 'info');
        }

        // Live GPS Bus Simulation
        function simulateBusMovement() {
            const bus = document.getElementById('moving-bus');
            const positions = [
                { left: '20%', top: '58%' },
                { left: '38%', top: '42%' },
                { left: '58%', top: '44%' },
                { left: '72%', top: '22%' }
            ];
            
            const randomPos = positions[Math.floor(Math.random() * positions.length)];
            bus.style.left = randomPos.left;
            bus.style.top = randomPos.top;

            const newEta = Math.floor(Math.random() * 5) + 2;
            document.getElementById('eta-counter').innerText = `${newEta} min ETA`;
            showToast(`Bus Location Updated • New ETA: ~${newEta} mins`, 'info');
        }

        // Play Driver Audio Note Simulation
        function playDriverVoiceNote() {
            showToast('🔊 Aisha Voice Note: "Assalamu Alaikum, arriving in 5 minutes at main gate."', 'info');
        }

        // Driver Start/Stop Route Toggle
        function toggleRouteStart() {
            routeActive = !routeActive;
            const btnText = document.getElementById('text-start-route');
            const btn = document.getElementById('btn-start-route');

            if (routeActive) {
                btnText.innerText = currentLang === 'ar' ? 'إيقاف المسار' : 'STOP ROUTE';
                btn.className = "w-full py-5 rounded-2xl bg-amber-500 hover:bg-amber-600 text-white font-extrabold text-base transition shadow-md flex items-center justify-center gap-3";
                showToast('🚀 Route Active! GPS Signal Broadcasting to Students.', 'success');
            } else {
                btnText.innerText = translations[currentLang].startRoute;
                btn.className = "w-full py-5 rounded-2xl bg-emerald-500 hover:bg-emerald-600 text-white font-extrabold text-base transition shadow-md flex items-center justify-center gap-3";
                showToast('Route Completed.', 'info');
            }
        }

        // Confirm Student Boarding in Driver Checklist
        function toggleStudentBoarded(button) {
            const parentCard = button.closest('.p-4');
            parentCard.className = "p-4 rounded-2xl border border-gray-200 flex items-center justify-between bg-emerald-50/50";
            parentCard.innerHTML = `
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-full bg-emerald-500 text-white flex items-center justify-center font-bold">
                        <i class="fa-solid fa-check"></i>
                    </div>
                    <div>
                        <div class="font-bold text-gray-900 text-sm">Reem Al-Qahtani (You)</div>
                        <div class="text-xs text-emerald-600 font-semibold">Boarded Safely</div>
                    </div>
                </div>
                <span class="text-xs font-bold text-emerald-700 bg-emerald-100 px-3 py-1 rounded-lg">Confirmed Just Now</span>
            `;
            document.getElementById('roster-count').innerText = "3 / 4 Boarded";
            showToast('Boarding Confirmed for Reem Al-Qahtani.', 'success');
        }

        // Quick Modal Control Triggering
        function triggerCallModal() {
            document.getElementById('modal-call').classList.remove('hidden');
        }

        function triggerEmergencyModal() {
            document.getElementById('modal-sos').classList.remove('hidden');
        }

        function triggerInAppChat() {
            document.getElementById('modal-chat').classList.remove('hidden');
        }

        function closeModal(id) {
            document.getElementById(id).classList.add('hidden');
        }

        function sendQuickMsg(msg) {
            closeModal('modal-chat');
            showToast(`Sent message: "${msg}"`, 'success');
        }

        function triggerAnnouncementModal() {
            showToast('📢 Voice Announcement dispatched to student route phones.', 'info');
        }

        function triggerCallSupport() {
            showToast('📞 Dialing Amiryx Support Line...', 'info');
        }

        // Custom Toast Notification System
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            const bgColor = type === 'success' ? 'bg-emerald-600' : 'bg-amiryx-blue';
            
            toast.className = `${bgColor} text-white px-4 py-3 rounded-2xl shadow-xl text-xs font-bold flex items-center gap-2 transition-all transform duration-300 pointer-events-auto max-w-sm`;
            toast.innerHTML = `
                <i class="fa-solid ${type === 'success' ? 'fa-circle-check' : 'fa-circle-info'}"></i>
                <span>${message}</span>
            `;

            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'scale-95');
                setTimeout(() => toast.remove(), 300);
            }, 3200);
        }
    </script>
</body>
</html>
