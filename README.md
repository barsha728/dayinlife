<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Barsha's Journey Through Kathmandu</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet-ant-path@1.3.0/dist/leaflet-ant-path.css" />
    <script src="https://unpkg.com/leaflet-ant-path@1.3.0/dist/leaflet-ant-path.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=Lato:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Lato', sans-serif;
            background: linear-gradient(135deg, #f0f4f0 0%, #e8f1e8 50%, #f5f9f5 100%);
            color: #2d3e2d;
            overflow-x: hidden;
        }
        
        .header {
            text-align: center;
            padding: 45px 20px 25px;
            background: linear-gradient(135deg, #ffffff 0%, #f8fdf8 100%);
            border-bottom: 2px solid #7ba57b;
            box-shadow: 0 4px 12px rgba(123, 165, 123, 0.1);
        }
        
        .title-main {
            font-family: 'Playfair Display', serif;
            font-size: 4em;
            font-weight: 600;
            color: #3d5a3d;
            margin-bottom: 12px;
            letter-spacing: -1px;
        }
        
        .subtitle {
            font-size: 1.3em;
            color: #7ba57b;
            font-weight: 300;
            letter-spacing: 3px;
            text-transform: uppercase;
            margin-bottom: 10px;
        }
        
        .date-info {
            font-family: 'Playfair Display', serif;
            font-style: italic;
            color: #5a7a5a;
            font-size: 1.2em;
        }
        
        #map {
            height: 70vh;
            margin: 0;
            border: none;
        }
        
        .journey-sidebar {
            position: fixed;
            top: 180px;
            right: 35px;
            width: 400px;
            background: linear-gradient(135deg, #ffffff 0%, #fcfefc 100%);
            border-radius: 12px;
            padding: 32px;
            box-shadow: 
                0 2px 8px rgba(123, 165, 123, 0.15),
                0 12px 40px rgba(123, 165, 123, 0.1);
            border: 1px solid #d4e4d4;
            border-left: 4px solid #7ba57b;
            z-index: 1000;
            max-height: 72vh;
            overflow-y: auto;
        }
        
        .current-place {
            font-family: 'Playfair Display', serif;
            font-size: 2em;
            font-weight: 600;
            color: #3d5a3d;
            margin-bottom: 14px;
            line-height: 1.3;
        }
        
        .current-time {
            font-size: 1em;
            color: #7ba57b;
            margin-bottom: 18px;
            font-weight: 400;
            letter-spacing: 1px;
            background: #f0f8f0;
            padding: 8px 16px;
            border-radius: 6px;
            display: inline-block;
        }
        
        .story-text {
            font-size: 1.05em;
            color: #3d5a3d;
            line-height: 1.8;
            margin-bottom: 22px;
            font-weight: 300;
            background: rgba(240, 248, 240, 0.5);
            padding: 18px;
            border-radius: 8px;
            border-left: 3px solid #a8c9a8;
        }
        
        .photos-container {
            margin: 25px 0;
            padding: 20px 0;
            border-top: 1px solid #d4e4d4;
            border-bottom: 1px solid #d4e4d4;
            display: none;
        }
        
        .photos-container.active {
            display: block;
            animation: fadeInUp 0.6s ease;
        }
        
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(15px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .photo-item {
            margin-bottom: 18px;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(123, 165, 123, 0.15);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .photo-item:hover {
            transform: translateY(-4px);
            box-shadow: 0 8px 24px rgba(123, 165, 123, 0.25);
        }
        
        .photo-item img {
            width: 100%;
            display: block;
            border-bottom: 3px solid #7ba57b;
        }
        
        .photo-caption {
            padding: 12px;
            background: white;
            font-family: 'Playfair Display', serif;
            font-style: italic;
            color: #5a7a5a;
            font-size: 0.95em;
            text-align: center;
        }
        
        .progress-section {
            margin: 25px 0;
            padding: 20px 0;
            border-top: 1px solid #d4e4d4;
        }
        
        .progress-label {
            font-size: 0.9em;
            color: #7ba57b;
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-weight: 400;
        }
        
        .progress-track {
            width: 100%;
            height: 6px;
            background: #e8f1e8;
            border-radius: 10px;
            overflow: hidden;
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #7ba57b, #5a8a5a);
            transition: width 1.2s ease;
            border-radius: 10px;
        }
        
        .stops-section {
            margin-top: 25px;
            padding-top: 20px;
            border-top: 1px solid #d4e4d4;
        }
        
        .stops-title {
            font-size: 0.9em;
            color: #7ba57b;
            margin-bottom: 16px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-weight: 400;
        }
        
        .stop-entry {
            padding: 13px 16px;
            margin: 8px 0;
            border-radius: 6px;
            background: rgba(240, 248, 240, 0.3);
            border-left: 3px solid #d4e4d4;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .stop-entry:hover {
            background: rgba(240, 248, 240, 0.6);
            transform: translateX(6px);
        }
        
        .stop-entry.active {
            background: linear-gradient(135deg, #e8f5e8 0%, #f0f8f0 100%);
            border-left-color: #7ba57b;
            transform: translateX(8px);
            box-shadow: 0 3px 10px rgba(123, 165, 123, 0.2);
        }
        
        .stop-entry.completed {
            opacity: 0.5;
            border-left-color: #a8c9a8;
        }
        
        .stop-name {
            font-size: 0.98em;
            color: #3d5a3d;
            font-weight: 500;
            margin-bottom: 4px;
        }
        
        .stop-time {
            font-size: 0.85em;
            color: #7ba57b;
        }
        
        .leaflet-popup-content-wrapper {
            background: linear-gradient(135deg, #ffffff 0%, #fcfefc 100%);
            color: #3d5a3d;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 8px 24px rgba(123, 165, 123, 0.2);
            border-left: 4px solid #7ba57b;
        }
        
        .leaflet-popup-content {
            margin: 0;
            font-family: 'Lato', sans-serif;
        }
        
        .popup-name {
            font-family: 'Playfair Display', serif;
            font-size: 1.5em;
            font-weight: 600;
            color: #3d5a3d;
            margin-bottom: 10px;
        }
        
        .popup-time {
            font-size: 0.9em;
            color: #7ba57b;
            margin-bottom: 14px;
            background: #f0f8f0;
            padding: 6px 12px;
            border-radius: 4px;
            display: inline-block;
        }
        
        .popup-story {
            font-size: 0.95em;
            color: #3d5a3d;
            line-height: 1.6;
            font-weight: 300;
        }
        
        ::-webkit-scrollbar {
            width: 8px;
        }
        
        ::-webkit-scrollbar-track {
            background: #e8f1e8;
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: linear-gradient(180deg, #7ba57b, #5a8a5a);
            border-radius: 10px;
        }
        
        .complete-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, #ffffff 0%, #f8fdf8 100%);
            padding: 50px 70px;
            border-radius: 12px;
            box-shadow: 0 20px 60px rgba(123, 165, 123, 0.3);
            border: 2px solid #7ba57b;
            display: none;
            z-index: 2000;
            text-align: center;
        }
        
        .complete-modal.show {
            display: block;
            animation: scaleIn 0.6s ease;
        }
        
        @keyframes scaleIn {
            from {
                opacity: 0;
                transform: translate(-50%, -50%) scale(0.8);
            }
            to {
                opacity: 1;
                transform: translate(-50%, -50%) scale(1);
            }
        }
        
        .complete-modal h2 {
            font-family: 'Playfair Display', serif;
            font-size: 2.8em;
            color: #3d5a3d;
            margin-bottom: 16px;
        }
        
        .complete-modal p {
            font-size: 1.2em;
            color: #7ba57b;
            font-weight: 300;
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="subtitle">A Day in the Life</div>
        <h1 class="title-main">Barsha's Day</h1>
        <p class="date-info">Kathmandu Valley ~ July 12, 2025</p>
    </div>
    
    <div id="map"></div>
    
    <div class="journey-sidebar">
        <div class="current-place" id="placeName">Starting the journey...</div>
        <div class="current-time" id="timeDisplay">—</div>
        <div class="story-text" id="storyDisplay">The adventure begins.</div>
        
        <div class="photos-container" id="photosContainer"></div>
        
        <div class="progress-section">
            <div class="progress-label">Journey Progress</div>
            <div class="progress-track">
                <div class="progress-bar" id="progressBar" style="width: 0%"></div>
            </div>
        </div>
        
        <div class="stops-section">
            <div class="stops-title">Today's Route</div>
            <div id="stopsContainer"></div>
        </div>
    </div>
    
    <div class="complete-modal" id="completeModal">
        <h2>Journey Complete! ✨</h2>
        <p>Another beautiful day in Kathmandu</p>
    </div>
    
    <script>
        const journeyStops = [
            {
                name: "Home, Banasthali",
                coords: [27.7224492, 85.2927936],
                time: "7:00 AM",
                story: "Morning sunlight fills my room. I can hear birds chirping outside and the neighborhood slowly waking up. After breakfast with my family, I pack my bag with essentials - water bottle, snacks, my camera. Today I'm exploring Kathmandu's Ring Road. Time to go!",
                delay: 4500
            },
            {
                name: "Machhapokhari Bus Stand",
                coords: [27.735311099999997, 85.3058395],
                time: "7:35 AM",
                story: "The bus ride through Balaju was quick. Now at Machhapokhari - one of the busiest bus stands in Kathmandu! I grab some candies from a roadside shop and watch the morning rush of people catching buses to different parts of the valley.",
                delay: 4000
            },
            {
                name: "Samakhusi Chowk",
                coords: [27.7272671, 85.317499],
                time: "7:55 AM",
                story: "Passing through Samakhusi on the Ring Road. The morning traffic is building up. School children in uniforms walk in groups, laughing and chatting. The chai stalls are already serving hot tea to early commuters.",
                delay: 3500
            },
            {
                name: "Gongabu Bus Park",
                coords: [27.7493381, 85.321376],
                time: "8:15 AM",
                story: "Gongabu! The energy here is incredible - buses of all colors heading to different districts of Nepal. Vendors selling tickets, people carrying luggage, announcements echoing. This is the gateway to the rest of Nepal.",
                delay: 4000
            },
            {
                name: "Basundhara",
                coords: [27.7386184, 85.3254075],
                time: "8:35 AM",
                story: "What a change from Gongabu! Basundhara is peaceful - wide roads, trees providing shade, joggers getting their morning exercise. I take a moment to breathe in the fresh air.",
                delay: 3500
            },
            {
                name: "Maharajgunj - Teaching Hospital",
                coords: [27.7359917, 85.3302595],
                time: "9:00 AM",
                story: "Arrived at Teaching Hospital for my appointment. The waiting area is crowded but organized. After my check-up, the doctor gives me good news - everything's fine! Feeling relieved and ready to continue exploring.",
                delay: 5500
            },
            {
                name: "Maharajgunj Ring Road",
                coords: [27.7375, 85.3320],
                time: "10:45 AM",
                story: "Back on the Ring Road near Maharajgunj. The mid-morning traffic has eased a bit. I decide to head toward Chabahil and then take a detour to Boudhanath.",
                delay: 3000
            },
            {
                name: "Sukedhara",
                coords: [27.7293612, 85.3467203],
                time: "11:05 AM",
                story: "Traveling through Sukedhara on the Ring Road. This area is quite green with nice residential neighborhoods on both sides. The sun is getting warmer now.",
                delay: 3200
            },
            {
                name: "Chabahil Ring Road Junction",
                coords: [27.7166069, 85.3485072],
                time: "11:25 AM",
                story: "At Chabahil junction - a major intersection on the Ring Road. From here, I can take a detour to Boudhanath. I've been wanting to visit the stupa for a while now. Let's go!",
                delay: 3500
            },
            {
                name: "Boudhanath Stupa",
                coords: [27.7214694, 85.36203499999999],
                time: "11:45 AM",
                story: "Boudhanath is magnificent! The massive white dome with Buddha's all-seeing eyes looking in all four directions. I join pilgrims walking clockwise, spinning prayer wheels. Prayer flags flutter everywhere creating a rainbow canopy. Tibetan monks in maroon robes, the smell of incense, butter lamps flickering. I buy some prayer beads and sit on the steps just watching, absorbing the peaceful energy.",
                delay: 7000,
                images: [
                    {
                        src: "https://www.wondersofnepal.com/wp-content/uploads/2020/02/Boudhanath-Stupa-2.jpg",
                        caption: "The sacred Boudhanath Stupa"
                    }
                ]
            },
            {
                name: "Back to Chabahil",
                coords: [27.7166069, 85.3485072],
                time: "1:15 PM",
                story: "Reluctantly leaving Boudhanath's peace behind. Back to Chabahil Ring Road to continue my journey around the valley.",
                delay: 3000
            },
            {
                name: "Gaushala Ring Road",
                coords: [27.707860999999998, 85.3433532],
                time: "1:35 PM",
                story: "Traveling south on the Ring Road through Gaushala. From here, Pashupatinath Temple is very close. I can't miss visiting Nepal's most sacred Hindu temple!",
                delay: 3200
            },
            {
                name: "Pashupatinath Temple",
                coords: [27.710511999999998, 85.3488125],
                time: "1:50 PM",
                story: "Pashupatinath - where the divine feels tangible. The golden pagoda-style temple dedicated to Lord Shiva gleams in the afternoon sun. Along the Bagmati River, I witness ancient rituals - priests performing ceremonies, devotees taking holy baths, sadhus in meditation covered in ash. The temple bells ring, creating a sacred symphony. I light a butter lamp and offer my prayers, feeling deeply connected to centuries of faith.",
                delay: 6500,
                images: [
                    {
                        src: "https://cdn.britannica.com/90/94190-050-3B4A46E6/Pashupatinath-temple-Kathmandu-Nepal.jpg",
                        caption: "Pashupatinath - Nepal's sacred heart"
                    }
                ]
            },
            {
                name: "Tinkune Junction",
                coords: [27.685365899999997, 85.3488678],
                time: "3:15 PM",
                story: "At Tinkune - the eastern junction where Ring Road meets Araniko Highway. The park here is nice, with people relaxing under trees. Traffic is constant but the greenery provides a buffer.",
                delay: 3500
            },
            {
                name: "Koteshwor Ring Road",
                coords: [27.675554899999998, 85.3459238],
                time: "3:40 PM",
                story: "Heading south through Koteshwor. This is a major commercial hub - shops, restaurants, the big Bhatbhateni supermarket. Lots of activity and energy here on the southeastern Ring Road.",
                delay: 3200
            },
            {
                name: "Balkumari",
                coords: [27.6719173, 85.3359114],
                time: "4:00 PM",
                story: "Passing through Balkumari area. The famous Balkumari Temple is nearby. The Ring Road curves westward from here.",
                delay: 3000
            },
            {
                name: "Gwarko Chowk",
                coords: [27.6663423, 85.3330053],
                time: "4:20 PM",
                story: "At Gwarko - a busy intersection on the southern Ring Road. Lots of local buses and microbuses. Street vendors selling fruits and snacks line the roadside.",
                delay: 3200
            },
            {
                name: "Satdobato",
                coords: [27.6515356, 85.327837],
                time: "4:40 PM",
                story: "Reached Satdobato - the southernmost point of my Ring Road journey! From here, the road starts curving back north. Time for some shopping at Labim Mall nearby.",
                delay: 3500
            },
            {
                name: "Labim Mall, Pulchowk",
                coords: [27.677116599999998, 85.3170766],
                time: "5:00 PM",
                story: "Shopping time at Labim Mall! I browse through some boutiques, buy a nice gift for my sister. The mall has a good vibe. I grab an iced coffee from a café and rest my feet for a bit. People-watching from the café window is always fun.",
                delay: 5000
            },
            {
                name: "Lagankhel",
                coords: [27.665994899999998, 85.32266829999999],
                time: "5:50 PM",
                story: "Passing through Lagankhel on the Ring Road. Another major bus stand serving routes to southern districts. The evening rush is starting.",
                delay: 3000
            },
            {
                name: "Kalanki Chowk",
                coords: [27.6933558, 85.2819172],
                time: "6:15 PM",
                story: "At Kalanki Chowk - the southwestern junction of Ring Road! The traffic here is always intense during rush hour. From here, I can see the road heading back north toward Swayambhunath.",
                delay: 3500
            },
            {
                name: "Swayambhunath (Monkey Temple)",
                coords: [27.7148996, 85.29039569999999],
                time: "6:45 PM",
                story: "Climbing the 365 steps to Swayambhunath as the sun begins to set! Each step brings me higher above the valley. At the top, the view is breathtaking - Kathmandu spread out below, bathed in golden evening light. The ancient stupa stands proud, prayer flags creating waves of color in the breeze. Monkeys play around the complex (hence 'Monkey Temple'!). I order chiya and samosas from a small shop, sitting on the steps watching the sunset paint the sky pink and orange. This is the perfect ending.",
                delay: 7500,
                images: [
                    {
                        src: "https://www.holidify.com/images/cmsuploads/compressed/shutterstock_248887429_20190821164548.jpg",
                        caption: "Sunset view from Swayambhunath"
                    }
                ]
            },
            {
                name: "Home, Banasthali",
                coords: [27.7224492, 85.2927936],
                time: "8:00 PM",
                story: "Home sweet home! The familiar street, the warm glow of home lights. I'm tired but my heart is full. Over dinner, I share stories with my family - the magnificent stupas, the peaceful temples, the energy of the Ring Road, the sunset view. They listen, smiling, asking questions. Tomorrow will bring new adventures, but tonight I'm grateful for this beautiful day exploring my city. Kathmandu, I love you! 💚",
                delay: 5000
            }
        ];
        
        function smoothPath(start, end, points = 100) {
            const path = [];
            for (let i = 0; i <= points; i++) {
                const t = i / points;
                const lat = start[0] + (end[0] - start[0]) * t;
                const lng = start[1] + (end[1] - start[1]) * t;
                path.push([lat, lng]);
            }
            return path;
        }
        
        const map = L.map('map', {
            zoomControl: true,
            scrollWheelZoom: true
        }).setView([27.700, 85.320], 12);
        
        L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
            attribution: '&copy; OpenStreetMap contributors',
            maxZoom: 19
        }).addTo(map);
        
        const markerIcon = L.divIcon({
            className: 'marker-dot',
            html: '<div style="width: 12px; height: 12px; background: #7ba57b; border-radius: 50%; border: 3px solid white; box-shadow: 0 2px 6px rgba(123, 165, 123, 0.4);"></div>',
            iconSize: [12, 12],
            iconAnchor: [6, 6]
        });
        
        const homeIcon = L.divIcon({
            className: 'marker-home',
            html: '<div style="width: 18px; height: 18px; background: #3d5a3d; border-radius: 50%; border: 4px solid white; box-shadow: 0 3px 10px rgba(61, 90, 61, 0.5);"></div>',
            iconSize: [18, 18],
            iconAnchor: [9, 9]
        });
        
        const templeIcon = L.divIcon({
            className: 'marker-temple',
            html: '<div style="width: 14px; height: 14px; background: #5a8a5a; border-radius: 50%; border: 3px solid white; box-shadow: 0 2px 8px rgba(90, 138, 90, 0.4);"></div>',
            iconSize: [14, 14],
            iconAnchor: [7, 7]
        });
        
        let markers = [];
        let antPath = null;
        let coords = [];
        
        const stopsContainer = document.getElementById('stopsContainer');
        journeyStops.forEach((stop, i) => {
            const div = document.createElement('div');
            div.className = 'stop-entry';
            div.innerHTML = `
                <div class="stop-name">${stop.name}</div>
                <div class="stop-time">${stop.time}</div>
            `;
            div.id = `stop-${i}`;
            stopsContainer.appendChild(div);
        });
        
        async function startJourney() {
            for (let i = 0; i < journeyStops.length; i++) {
                const stop = journeyStops[i];
                
                document.getElementById('placeName').textContent = stop.name;
                document.getElementById('timeDisplay').textContent = stop.time;
                document.getElementById('storyDisplay').textContent = stop.story;
                document.getElementById('progressBar').style.width = ((i + 1) / journeyStops.length * 100) + '%';
                
                const photos = document.getElementById('photosContainer');
                if (stop.images) {
                    photos.innerHTML = '';
                    stop.images.forEach(img => {
                        const div = document.createElement('div');
                        div.className = 'photo-item';
                        div.innerHTML = `
                            <img src="${img.src}" alt="${img.caption}">
                            <div class="photo-caption">${img.caption}</div>
                        `;
                        photos.appendChild(div);
                    });
                    photos.classList.add('active');
                } else {
                    photos.classList.remove('active');
                }
                
                document.querySelectorAll('.stop-entry').forEach((el, idx) => {
                    el.classList.remove('active');
                    if (idx < i) el.classList.add('completed');
                    if (idx === i) el.classList.add('active');
                });
                
                let icon = markerIcon;
                if (i === 0 || i === journeyStops.length - 1) icon = homeIcon;
                else if (stop.name.includes('Temple') || stop.name.includes('Stupa')) icon = templeIcon;
                
                const marker = L.marker(stop.coords, { icon }).addTo(map);
                marker.bindPopup(`
                    <div class="popup-name">${stop.name}</div>
                    <div class="popup-time">${stop.time}</div>
                    <div class="popup-story">${stop.story}</div>
                `, { maxWidth: 320 });
                marker.openPopup();
                markers.push(marker);
                
                map.flyTo(stop.coords, 14, {
                    duration: 2.2,
                    easeLinearity: 0.25
                });
                
                if (i < journeyStops.length - 1) {
                    const next = journeyStops[i + 1];
                    const smooth = smoothPath(stop.coords, next.coords, 100);
                    coords.push(...smooth);
                    
                    if (antPath) map.removeLayer(antPath);
                    
                    antPath = L.polyline.antPath(coords, {
                        delay: 1100,
                        dashArray: [15, 28],
                        weight: 5,
                        color: '#7ba57b',
                        pulseColor: '#5a8a5a',
                        opacity: 0.75,
                        hardwareAccelerated: true
                    }).addTo(map);
                }
                
                await new Promise(r => setTimeout(r, stop.delay));
            }
            
            setTimeout(() => {
                document.getElementById('completeModal').classList.add('show');
                map.flyTo([27.700, 85.320], 12, { duration: 3 });
                setTimeout(() => {
                    document.getElementById('completeModal').classList.remove('show');
                }, 4500);
            }, 2000);
        }
        
        setTimeout(() => startJourney(), 2500);
    </script>
</body>
</html>
