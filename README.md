# clps-lunar-mission-browser
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CLPS Lunar Mission Browser V5.1</title>

<style>
*{
    box-sizing:border-box;
    margin:0;
    padding:0;
}

html,body{
    width:100%;
    height:100%;
    overflow:hidden;
    background:#020305;
    color:#eef3f7;
    font-family:Arial,Helvetica,sans-serif;
}

#app{
    width:100vw;
    height:100vh;
    display:grid;
    grid-template-columns:300px 1fr 330px;
    grid-template-rows:68px 1fr 34px;
    background:
        radial-gradient(circle at center,#101820 0%,#05080b 55%,#020305 100%);
}

/* HEADER */

header{
    grid-column:1/4;
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:0 20px;
    background:rgba(4,7,10,.97);
    border-bottom:1px solid #29343e;
    z-index:100;
}

.brand{
    display:flex;
    align-items:center;
    gap:12px;
}

.nasa{
    width:42px;
    height:42px;
    border:1px solid #9aa6af;
    border-radius:50%;
    display:flex;
    align-items:center;
    justify-content:center;
    font-size:10px;
    font-weight:bold;
}

.brand h1{
    font-size:16px;
    letter-spacing:2px;
}

.brand p{
    margin-top:4px;
    font-size:8px;
    color:#687680;
    letter-spacing:1.5px;
}

.headerRight{
    display:flex;
    align-items:center;
    gap:15px;
}

.online{
    display:flex;
    align-items:center;
    gap:7px;
    color:#80909c;
    font-size:8px;
    letter-spacing:1px;
}

.dot{
    width:7px;
    height:7px;
    border-radius:50%;
    background:#54d38a;
    box-shadow:0 0 12px #54d38a;
}

button{
    font-family:inherit;
}

.headerBtn{
    width:35px;
    height:35px;
    background:#080d12;
    color:#dce4e9;
    border:1px solid #35424e;
    border-radius:4px;
    cursor:pointer;
}

/* PANELS */

.panel{
    background:rgba(6,10,14,.97);
    overflow:hidden;
}

#left{
    border-right:1px solid #29343e;
    display:flex;
    flex-direction:column;
}

#right{
    border-left:1px solid #29343e;
    display:flex;
    flex-direction:column;
}

.panelHeader{
    padding:16px;
    border-bottom:1px solid #202a33;
}

.panelHeader h2{
    font-size:10px;
    letter-spacing:2px;
}

.panelHeader p{
    margin-top:5px;
    font-size:8px;
    color:#64727d;
}

/* SEARCH */

.search{
    padding:11px;
    border-bottom:1px solid #202a33;
}

.search input{
    width:100%;
    padding:10px;
    background:#090f14;
    border:1px solid #2c3842;
    border-radius:4px;
    color:white;
    outline:none;
    font-size:10px;
}

.search input:focus{
    border-color:#74838e;
}

/* FILTERS */

.filters{
    padding:9px;
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:5px;
    border-bottom:1px solid #202a33;
}

.filter{
    padding:8px 5px;
    background:#080d12;
    border:1px solid #293640;
    border-radius:4px;
    color:#7e8b96;
    cursor:pointer;
    font-size:8px;
}

.filter.active,
.filter:hover{
    background:#17222b;
    color:#fff;
    border-color:#697782;
}

/* MISSIONS */

.missions{
    flex:1;
    overflow-y:auto;
    padding:8px;
}

.card{
    padding:11px;
    margin-bottom:6px;
    background:#080d12;
    border:1px solid transparent;
    border-radius:5px;
    cursor:pointer;
}

.card:hover{
    background:#111a21;
    border-color:#33414c;
}

.card.active{
    background:#152029;
    border-color:#73818c;
}

.cardName{
    font-size:11px;
    font-weight:bold;
}

.cardProvider{
    margin-top:4px;
    color:#6d7c87;
    font-size:8px;
}

.badge{
    display:inline-block;
    margin-top:7px;
    padding:3px 6px;
    border:1px solid #40505d;
    border-radius:3px;
    font-size:7px;
    letter-spacing:1px;
    color:#a9b6c0;
}

.badge.completed{
    color:#70d39b;
    border-color:#376d52;
}

.badge.planned{
    color:#8eb0d1;
    border-color:#455e77;
}

.badge.failed{
    color:#e47e7e;
    border-color:#713f3f;
}

/* MAP */

#map{
    position:relative;
    overflow:hidden;
    background:#020407;
}

#stars{
    position:absolute;
    inset:0;
    pointer-events:none;
}

.star{
    position:absolute;
    width:1px;
    height:1px;
    border-radius:50%;
    background:#fff;
    opacity:.7;
}

.mapTitle{
    position:absolute;
    top:18px;
    left:20px;
    z-index:20;
    pointer-events:none;
}

.mapTitle h2{
    font-size:11px;
    letter-spacing:2px;
}

.mapTitle p{
    margin-top:4px;
    font-size:8px;
    color:#687782;
    letter-spacing:1px;
}

/* VIEWPORT */

#viewport{
    position:absolute;
    inset:0;
    overflow:hidden;
    cursor:grab;
    touch-action:none;
}

#viewport.dragging{
    cursor:grabbing;
}

/* MOON */

#moonWorld{
    position:absolute;
    width:min(70vh,700px);
    height:min(70vh,700px);
    left:50%;
    top:50%;
    transform:translate(-50%,-50%) scale(1);
    transform-origin:center;
    will-change:transform;
}

#moon{
    position:absolute;
    inset:0;
    border-radius:50%;
    overflow:hidden;

    /*
       LOCAL MOON IMAGE

       Put moon.jpg beside index.html.

       If it is missing, JavaScript below automatically
       switches to NASA's Moon texture.
    */

    background-image:url("moon.jpg");

    background-size:cover;
    background-position:center;
    background-repeat:no-repeat;

    background-color:#777;

    box-shadow:
        inset -65px -15px 90px rgba(0,0,0,.72),
        inset 20px 15px 55px rgba(255,255,255,.08),
        0 0 45px rgba(255,255,255,.08);
}

/* realistic shading */

#moon::before{
    content:"";
    position:absolute;
    inset:0;
    border-radius:50%;
    background:
        radial-gradient(
            circle at 35% 30%,
            rgba(255,255,255,.12),
            transparent 32%
        );
    pointer-events:none;
}

#moon::after{
    content:"";
    position:absolute;
    inset:0;
    border-radius:50%;
    background:
        radial-gradient(
            circle at 40% 38%,
            transparent 0%,
            transparent 52%,
            rgba(0,0,0,.22) 76%,
            rgba(0,0,0,.72) 100%
        );
    pointer-events:none;
}

/* MARKERS */

#markers{
    position:absolute;
    inset:0;
    z-index:10;
}

.marker{
    position:absolute;
    width:18px;
    height:18px;
    transform:translate(-50%,-50%);
    border:2px solid white;
    border-radius:50%;
    background:rgba(255,255,255,.08);
    box-shadow:
        0 0 0 4px rgba(255,255,255,.07),
        0 0 18px rgba(255,255,255,.6);
    cursor:pointer;
}

.marker::after{
    content:"";
    position:absolute;
    left:50%;
    top:50%;
    width:5px;
    height:5px;
    transform:translate(-50%,-50%);
    background:white;
    border-radius:50%;
}

.markerLabel{
    position:absolute;
    left:25px;
    top:-4px;
    white-space:nowrap;
    padding:4px 6px;
    background:rgba(3,6,9,.9);
    border:1px solid #394650;
    border-radius:3px;
    font-size:7px;
}

/* CONTROLS */

.controls{
    position:absolute;
    right:18px;
    top:18px;
    z-index:30;
    display:flex;
    flex-direction:column;
    gap:5px;
}

.mapBtn{
    width:36px;
    height:36px;
    background:rgba(5,9,13,.93);
    color:#dce5eb;
    border:1px solid #35424e;
    border-radius:4px;
    cursor:pointer;
    font-size:15px;
}

.mapBtn:hover{
    background:#18232c;
}

/* RETICLE */

.reticle{
    position:absolute;
    left:50%;
    top:50%;
    width:90px;
    height:90px;
    transform:translate(-50%,-50%);
    z-index:25;
    pointer-events:none;
    opacity:.25;
}

.reticle::before,
.reticle::after{
    content:"";
    position:absolute;
    background:white;
}

.reticle::before{
    width:1px;
    height:100%;
    left:50%;
}

.reticle::after{
    width:100%;
    height:1px;
    top:50%;
}

.reticleCircle{
    position:absolute;
    inset:18px;
    border:1px solid white;
    border-radius:50%;
}

/* COORDINATES */

.coords{
    position:absolute;
    left:18px;
    bottom:18px;
    z-index:30;
    padding:8px 10px;
    background:rgba(3,6,9,.88);
    border:1px solid #293640;
    border-radius:3px;
    font-family:monospace;
    font-size:8px;
    color:#aeb9c1;
}

/* RIGHT */

.detail{
    flex:1;
    overflow-y:auto;
}

.hero{
    height:175px;
    background:#080d12;
    position:relative;
    overflow:hidden;
}

.hero img{
    width:100%;
    height:100%;
    object-fit:cover;
    display:none;
    opacity:.7;
}

.heroFallback{
    position:absolute;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    color:#65737d;
    font-size:8px;
    letter-spacing:1px;
}

.gradient{
    position:absolute;
    inset:0;
    background:linear-gradient(
        transparent 20%,
        rgba(4,7,10,.97) 100%
    );
}

.heroText{
    position:absolute;
    left:15px;
    right:15px;
    bottom:13px;
}

.heroText small{
    color:#82909b;
    font-size:7px;
    letter-spacing:1px;
}

.heroText h2{
    margin-top:4px;
    font-size:18px;
}

.info{
    padding:15px;
}

.grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:6px;
}

.box{
    padding:9px;
    background:#080e13;
    border:1px solid #202b35;
    border-radius:4px;
}

.box label{
    display:block;
    margin-bottom:5px;
    color:#697783;
    font-size:7px;
    letter-spacing:1px;
}

.box strong{
    font-size:9px;
}

.section{
    margin-top:17px;
}

.sectionTitle{
    margin-bottom:7px;
    color:#74828d;
    font-size:8px;
    letter-spacing:1.5px;
}

.text{
    color:#b5c0c8;
    font-size:9px;
    line-height:1.55;
}

.payload{
    padding:7px;
    margin-bottom:5px;
    background:#080e13;
    border-left:2px solid #657580;
    color:#aebac2;
    font-size:8px;
}

.action{
    margin-top:15px;
    width:100%;
    padding:10px;
    background:#111b23;
    color:white;
    border:1px solid #3c4b57;
    border-radius:4px;
    cursor:pointer;
    font-size:8px;
    letter-spacing:1px;
}

.action:hover{
    background:#1a2832;
}

/* FOOTER */

footer{
    grid-column:1/4;
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:0 14px;
    background:#05090d;
    border-top:1px solid #26313b;
    color:#5f6d77;
    font-size:7px;
    letter-spacing:1px;
}

/* SCROLLBAR */

::-webkit-scrollbar{
    width:5px;
}

::-webkit-scrollbar-track{
    background:#05080b;
}

::-webkit-scrollbar-thumb{
    background:#27343e;
}

/* RESPONSIVE */

@media(max-width:1000px){

    #app{
        grid-template-columns:230px 1fr 270px;
    }

}

@media(max-width:750px){

    #app{
        grid-template-columns:1fr;
        grid-template-rows:60px 1fr 32px;
    }

    #left,
    #right{
        display:none;
    }

    header{
        grid-column:1;
    }

    footer{
        grid-column:1;
    }
}

</style>
</head>

<body>

<div id="app">

<!-- ======================================================
HEADER
====================================================== -->

<header>

    <div class="brand">

        <div class="nasa">NASA</div>

        <div class="brandText">
            <h1>CLPS LUNAR MISSION BROWSER</h1>
            <p>COMMERCIAL LUNAR PAYLOAD SERVICES • LUNAR OPERATIONS</p>
        </div>

    </div>

    <div class="headerRight">

        <div class="online">
            <span class="dot"></span>
            LUNAR DATA SYSTEM ONLINE
        </div>

        <button class="headerBtn" onclick="fullscreen()">⛶</button>

    </div>

</header>


<!-- ======================================================
LEFT PANEL
====================================================== -->

<aside id="left" class="panel">

    <div class="panelHeader">
        <h2>MISSION DATABASE</h2>
        <p>CLPS LANDING MISSIONS</p>
    </div>

    <div class="search">

        <input
            id="search"
            type="text"
            placeholder="Search missions..."
            oninput="renderMissions()"
        >

    </div>

    <div class="filters">

        <button class="filter active"
                onclick="setFilter('all',this)">
            ALL
        </button>

        <button class="filter"
                onclick="setFilter('completed',this)">
            COMPLETED
        </button>

        <button class="filter"
                onclick="setFilter('planned',this)">
            PLANNED
        </button>

        <button class="filter"
                onclick="setFilter('failed',this)">
            FAILED
        </button>

    </div>

    <div id="missionList" class="missions"></div>

</aside>


<!-- ======================================================
MAP
====================================================== -->

<main id="map">

    <div id="stars"></div>

    <div class="mapTitle">
        <h2>LUNAR SURFACE VIEW</h2>
        <p>LRO-STYLE VISUALIZATION • CLPS LANDING SITES</p>
    </div>

    <div id="viewport">

        <div id="moonWorld">

            <div id="moon">

                <div id="markers"></div>

            </div>

        </div>

    </div>

    <div class="reticle">
        <div class="reticleCircle"></div>
    </div>

    <div class="controls">

        <button class="mapBtn" onclick="zoomIn()">+</button>

        <button class="mapBtn" onclick="resetView()">⌂</button>

        <button class="mapBtn" onclick="zoomOut()">−</button>

        <button class="mapBtn" onclick="rotateMoon()">↻</button>

        <button class="mapBtn" onclick="randomMission()">?</button>

    </div>

    <div class="coords" id="coords">
        LAT +00.000° &nbsp; LON +000.000°
    </div>

</main>


<!-- ======================================================
RIGHT PANEL
====================================================== -->

<aside id="right" class="panel">

    <div class="detail">

        <div class="hero">

            <img id="missionImage">

            <div id="heroFallback" class="heroFallback">
                NASA ARCHIVE IMAGE
            </div>

            <div class="gradient"></div>

            <div class="heroText">

                <small id="heroProvider">
                    NASA CLPS
                </small>

                <h2 id="heroName">
                    Select a Mission
                </h2>

            </div>

        </div>


        <div class="info">

            <div class="grid">

                <div class="box">
                    <label>PROVIDER</label>
                    <strong id="provider">—</strong>
                </div>

                <div class="box">
                    <label>STATUS</label>
                    <strong id="status">—</strong>
                </div>

                <div class="box">
                    <label>YEAR</label>
                    <strong id="year">—</strong>
                </div>

                <div class="box">
                    <label>LANDING SITE</label>
                    <strong id="site">—</strong>
                </div>

                <div class="box">
                    <label>LATITUDE</label>
                    <strong id="lat">—</strong>
                </div>

                <div class="box">
                    <label>LONGITUDE</label>
                    <strong id="lon">—</strong>
                </div>

            </div>


            <div class="section">

                <div class="sectionTitle">
                    MISSION OBJECTIVE
                </div>

                <div id="objective" class="text">
                    Select a mission from the database.
                </div>

            </div>


            <div class="section">

                <div class="sectionTitle">
                    NASA PAYLOADS
                </div>

                <div id="payloads"></div>

            </div>


            <div class="section">

                <div class="sectionTitle">
                    MISSION NOTES
                </div>

                <div id="notes" class="text">
                    —
                </div>

            </div>


            <button
                class="action"
                onclick="randomMission()">
                DISCOVER ANOTHER CLPS MISSION
            </button>

        </div>

    </div>

</aside>


<!-- ======================================================
FOOTER
====================================================== -->

<footer>

    <span>
        CLPS LUNAR MISSION BROWSER V5.1
    </span>

    <span id="clock">
        SYSTEM TIME
    </span>

    <span>
        NASA LRO DATA • VISUALIZATION
    </span>

</footer>

</div>


<script>

/* =========================================================
MISSION DATABASE
========================================================= */

const missions = [

{
    name:"Peregrine Mission One",
    provider:"Astrobotic",
    status:"failed",
    year:"2024",
    site:"Sinus Viscositatis",
    lat:"34.1° N",
    lon:"25.3° W",
    objective:
        "Commercial lunar landing mission carrying NASA and commercial payloads to demonstrate lunar surface delivery capabilities.",
    payloads:[
        "NASA science instruments",
        "Lunar surface technology demonstrations",
        "Commercial payloads"
    ],
    notes:
        "Peregrine launched in January 2024 but did not complete a lunar landing."
},

{
    name:"IM-1",
    provider:"Intuitive Machines",
    status:"completed",
    year:"2024",
    site:"Malapert A",
    lat:"80.3° S",
    lon:"1.2° E",
    objective:
        "Demonstrate precision lunar landing and deliver NASA payloads for science and technology investigations.",
    payloads:[
        "LRA navigation experiment",
        "ROLSES",
        "NSS",
        "SCALPSS"
    ],
    notes:
        "IM-1 became the first U.S. commercial spacecraft to achieve a soft lunar landing in decades."
},

{
    name:"Blue Ghost Mission 1",
    provider:"Firefly Aerospace",
    status:"completed",
    year:"2025",
    site:"Mare Crisium",
    lat:"18.6° N",
    lon:"61.8° E",
    objective:
        "Deliver NASA science and technology payloads to the lunar surface and demonstrate precision landing operations.",
    payloads:[
        "Lunar PlanetVac",
        "LISTER",
        "RAC",
        "LuGRE",
        "NGLR"
    ],
    notes:
        "Blue Ghost Mission 1 successfully landed in Mare Crisium."
},

{
    name:"IM-2",
    provider:"Intuitive Machines",
    status:"completed",
    year:"2025",
    site:"Mons Mouton",
    lat:"85.0° S",
    lon:"31.0° E",
    objective:
        "Investigate the lunar south polar environment and demonstrate technologies relevant to future exploration.",
    payloads:[
        "PRIME-1",
        "TRN technology",
        "Lunar surface instruments"
    ],
    notes:
        "IM-2 targeted the lunar south polar region."
},

{
    name:"IM-3",
    provider:"Intuitive Machines",
    status:"planned",
    year:"2026",
    site:"Reiner Gamma",
    lat:"7.5° N",
    lon:"59.0° W",
    objective:
        "Investigate lunar surface processes and demonstrate new technologies at the Reiner Gamma region.",
    payloads:[
        "Lunar science instruments",
        "Technology demonstrations",
        "Surface sensors"
    ],
    notes:
        "Mission schedule and payload details can change as the program develops."
},

{
    name:"Blue Ghost Mission 2",
    provider:"Firefly Aerospace",
    status:"planned",
    year:"2026",
    site:"Lunar Far Side",
    lat:"45.0° S",
    lon:"175.0° E",
    objective:
        "Deliver scientific and technology payloads to the lunar far side.",
    payloads:[
        "Science instruments",
        "Communications technology",
        "Technology demonstrations"
    ],
    notes:
        "Planned far-side operations require specialized communications architecture."
},

{
    name:"Draper CLPS Mission",
    provider:"Team Draper",
    status:"planned",
    year:"2026",
    site:"Schrödinger Basin",
    lat:"75.0° S",
    lon:"135.0° E",
    objective:
        "Investigate the lunar far-side environment and conduct geological and geophysical investigations.",
    payloads:[
        "Geophysical instruments",
        "Lunar science payloads",
        "Technology demonstrations"
    ],
    notes:
        "Schrödinger Basin is located on the lunar far side."
},

{
    name:"IM-4",
    provider:"Intuitive Machines",
    status:"planned",
    year:"2027",
    site:"Mons Mouton",
    lat:"85.0° S",
    lon:"31.0° E",
    objective:
        "Continue lunar south-polar science and technology investigations.",
    payloads:[
        "NASA science instruments",
        "Navigation systems",
        "Surface technology"
    ],
    notes:
        "Mission planning and landing locations may evolve."
},

{
    name:"Blue Ghost Mission 3",
    provider:"Firefly Aerospace",
    status:"planned",
    year:"2028",
    site:"Gruithuisen Domes",
    lat:"36.0° N",
    lon:"40.0° W",
    objective:
        "Study unusual lunar volcanic features and demonstrate new lunar surface technologies.",
    payloads:[
        "Geology instruments",
        "Surface science",
        "Technology demonstrations"
    ],
    notes:
        "The Gruithuisen Domes are unusual volcanic features on the Moon."
}

];


/* =========================================================
STATE
========================================================= */

let currentFilter="all";
let currentMission=missions[2];

let zoom=1;
let rotation=0;

let panX=0;
let panY=0;

let dragging=false;
let lastX=0;
let lastY=0;


/* =========================================================
STAR FIELD
========================================================= */

function createStars(){

    const field=document.getElementById("stars");

    field.innerHTML="";

    for(let i=0;i<180;i++){

        const star=document.createElement("div");

        star.className="star";

        star.style.left=(Math.random()*100)+"%";
        star.style.top=(Math.random()*100)+"%";

        const size=Math.random()<.9 ? 1 : 2;

        star.style.width=size+"px";
        star.style.height=size+"px";

        star.style.opacity=(.25+Math.random()*.75);

        field.appendChild(star);
    }
}


/* =========================================================
NASA MOON FALLBACK
========================================================= */

const nasaMoon =
"https://svs.gsfc.nasa.gov/vis/a000000/a004700/a004720/lroc_color_2k.jpg";


function loadMoon(){

    const test=new Image();

    test.onload=function(){

        /*
        Local moon.jpg exists.
        Keep it.
        */

        document.getElementById("moon").style.backgroundImage=
            'url("moon.jpg")';

    };

    test.onerror=function(){

        /*
        Local file missing.
        Automatically use NASA texture.
        */

        document.getElementById("moon").style.backgroundImage=
            `url("${nasaMoon}")`;

    };

    test.src="moon.jpg";
}


/* =========================================================
MISSION FILTER
========================================================= */

function setFilter(filter,button){

    currentFilter=filter;

    document
        .querySelectorAll(".filter")
        .forEach(x=>x.classList.remove("active"));

    button.classList.add("active");

    renderMissions();
}


/* =========================================================
RENDER MISSIONS
========================================================= */

function renderMissions(){

    const list=document.getElementById("missionList");

    const search=
        document
        .getElementById("search")
        .value
        .toLowerCase();

    list.innerHTML="";

    missions
    .filter(m=>{

        const matchesFilter=
            currentFilter==="all" ||
            m.status===currentFilter;

        const matchesSearch=
            m.name.toLowerCase().includes(search) ||
            m.provider.toLowerCase().includes(search) ||
            m.site.toLowerCase().includes(search);

        return matchesFilter && matchesSearch;

    })
    .forEach(m=>{

        const card=document.createElement("div");

        card.className=
            "card "+
            (m===currentMission ? "active":"");

        card.innerHTML=`

            <div class="cardName">
                ${m.name}
            </div>

            <div class="cardProvider">
                ${m.provider}
            </div>

            <span class="badge ${m.status}">
                ${m.status.toUpperCase()}
            </span>

        `;

        card.onclick=()=>selectMission(m);

        list.appendChild(card);

    });
}


/* =========================================================
SELECT MISSION
========================================================= */

async function selectMission(mission){

    currentMission=mission;

    document.getElementById("heroName").textContent=
        mission.name;

    document.getElementById("heroProvider").textContent=
        mission.provider;

    document.getElementById("provider").textContent=
        mission.provider;

    document.getElementById("status").textContent=
        mission.status.toUpperCase();

    document.getElementById("year").textContent=
        mission.year;

    document.getElementById("site").textContent=
        mission.site;

    document.getElementById("lat").textContent=
        mission.lat;

    document.getElementById("lon").textContent=
        mission.lon;

    document.getElementById("objective").textContent=
        mission.objective;

    document.getElementById("notes").textContent=
        mission.notes;

    const payloads=
        document.getElementById("payloads");

    payloads.innerHTML="";

    mission.payloads.forEach(p=>{

        const div=document.createElement("div");

        div.className="payload";

        div.textContent=p;

        payloads.appendChild(div);

    });

    renderMissions();

    updateMarkerFocus(mission);

    loadNASAImage(mission);

}


/* =========================================================
NASA IMAGE API
========================================================= */

async function loadNASAImage(mission){

    const img=document.getElementById("missionImage");

    const fallback=
        document.getElementById("heroFallback");

    img.style.display="none";

    fallback.style.display="flex";

    fallback.textContent="SEARCHING NASA IMAGE ARCHIVE...";

    try{

        const query=
            encodeURIComponent(
                mission.name+" Moon NASA"
            );

        const response=
            await fetch(
                `https://images-api.nasa.gov/search?q=${query}&media_type=image&page_size=10`
            );

        const data=await response.json();

        if(
            data.collection &&
            data.collection.items &&
            data.collection.items.length
        ){

            const item=
                data.collection.items.find(
                    x=>
                    x.links &&
                    x.links.some(
                        l=>l.rel==="preview"
                    )
                );

            if(item){

                const link=
                    item.links.find(
                        l=>l.rel==="preview"
                    );

                img.onload=()=>{

                    fallback.style.display="none";
                    img.style.display="block";

                };

                img.onerror=()=>{

                    img.style.display="none";
                    fallback.style.display="flex";
                    fallback.textContent=
                        "NASA IMAGE UNAVAILABLE";

                };

                img.src=link.href;

                return;

            }

        }

        fallback.textContent=
            "NASA IMAGE NOT AVAILABLE";

    }catch(error){

        fallback.textContent=
            "NASA ARCHIVE OFFLINE";

    }
}


/* =========================================================
MARKERS
========================================================= */

function createMarkers(){

    const container=
        document.getElementById("markers");

    container.innerHTML="";

    const positions=[
        [35,40],
        [60,30],
        [70,58],
        [45,70],
        [27,62],
        [67,76],
        [54,49],
        [39,55],
        [76,40]
    ];

    missions.forEach((mission,i)=>{

        const marker=
            document.createElement("div");

        marker.className="marker";

        marker.style.left=
            positions[i][0]+"%";

        marker.style.top=
            positions[i][1]+"%";

        marker.title=mission.name;

        const label=
            document.createElement("div");

        label.className="markerLabel";

        label.textContent=mission.name;

        marker.appendChild(label);

        marker.onclick=(event)=>{

            event.stopPropagation();

            selectMission(mission);

        };

        container.appendChild(marker);

    });
}


/* =========================================================
FOCUS MARKER
========================================================= */

function updateMarkerFocus(mission){

    const index=
        missions.indexOf(mission);

    const markers=
        document.querySelectorAll(".marker");

    markers.forEach((marker,i)=>{

        if(i===index){

            marker.style.transform=
                "translate(-50%,-50%) scale(1.35)";

            marker.style.borderColor="#ffffff";

        }else{

            marker.style.transform=
                "translate(-50%,-50%) scale(1)";

        }

    });

}


/* =========================================================
MOON TRANSFORM
========================================================= */

function updateMoon(){

    const world=
        document.getElementById("moonWorld");

    world.style.transform=
        `
        translate(
            calc(-50% + ${panX}px),
            calc(-50% + ${panY}px)
        )
        scale(${zoom})
        rotate(${rotation}deg)
        `;

}


/* =========================================================
ZOOM
========================================================= */

function zoomIn(){

    zoom=Math.min(3.5,zoom+.2);

    updateMoon();

}

function zoomOut(){

    zoom=Math.max(.65,zoom-.2);

    updateMoon();

}

function resetView(){

    zoom=1;
    panX=0;
    panY=0;
    rotation=0;

    updateMoon();

}

function rotateMoon(){

    rotation+=15;

    updateMoon();

}


/* =========================================================
RANDOM MISSION
========================================================= */

function randomMission(){

    const random=
        missions[
            Math.floor(
                Math.random()*missions.length
            )
        ];

    selectMission(random);

}


/* =========================================================
DRAG / PAN
========================================================= */

const viewport=
    document.getElementById("viewport");

viewport.addEventListener(
    "pointerdown",
    e=>{

        dragging=true;

        lastX=e.clientX;
        lastY=e.clientY;

        viewport.classList.add("dragging");

        viewport.setPointerCapture(e.pointerId);

    }
);

viewport.addEventListener(
    "pointermove",
    e=>{

        if(!dragging) return;

        const dx=
            e.clientX-lastX;

        const dy=
            e.clientY-lastY;

        panX+=dx;
        panY+=dy;

        lastX=e.clientX;
        lastY=e.clientY;

        updateMoon();

    }
);

viewport.addEventListener(
    "pointerup",
    e=>{

        dragging=false;

        viewport.classList.remove("dragging");

    }
);

viewport.addEventListener(
    "pointercancel",
    ()=>{

        dragging=false;

        viewport.classList.remove("dragging");

    }
);


/* =========================================================
MOUSE WHEEL
========================================================= */

viewport.addEventListener(
    "wheel",
    e=>{

        e.preventDefault();

        if(e.deltaY<0){

            zoom=Math.min(3.5,zoom+.12);

        }else{

            zoom=Math.max(.65,zoom-.12);

        }

        updateMoon();

    },
    {passive:false}
);


/* =========================================================
FULLSCREEN
========================================================= */

function fullscreen(){

    if(!document.fullscreenElement){

        document.documentElement.requestFullscreen();

    }else{

        document.exitFullscreen();

    }

}


/* =========================================================
COORDINATES
========================================================= */

viewport.addEventListener(
    "pointermove",
    e=>{

        const rect=
            viewport.getBoundingClientRect();

        const x=
            ((e.clientX-rect.left)/rect.width-.5)*360;

        const y=
            (.5-(e.clientY-rect.top)/rect.height)*180;

        document.getElementById("coords").textContent=
            `LAT ${y>=0?"+":""}${y.toFixed(3)}°   LON ${x>=0?"+":""}${x.toFixed(3)}°`;

    }
);


/* =========================================================
CLOCK
========================================================= */

function updateClock(){

    const now=new Date();

    document.getElementById("clock").textContent=
        now.toLocaleString();

}

setInterval(updateClock,1000);


/* =========================================================
KEYBOARD
========================================================= */

document.addEventListener(
    "keydown",
    e=>{

        if(e.key==="+") zoomIn();

        if(e.key==="-") zoomOut();

        if(e.key==="0") resetView();

        if(e.key==="f" || e.key==="F")
            fullscreen();

        if(e.code==="Space"){

            e.preventDefault();

            randomMission();

        }

    }
);


/* =========================================================
START
========================================================= */

createStars();

createMarkers();

renderMissions();

selectMission(currentMission);

loadMoon();

updateMoon();

updateClock();

</script>

</body>
</html>
