<!Anmol singh>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=2.0">
    <title>Interactive Periodic Table Atom Simulation</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
            font-family: Arial, sans-serif;
            touch-action: manipulation;
        }
        #homePage {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, #1a1a1a, #000);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 200;
            overflow-y: auto;
        }
        #homePage.hidden {
            display: none;
        }
        #homeTitle {
            color: white;
            font-size: calc(24px + 2vw);
            text-shadow: 0 0 10px #00ffff;
            animation: glow 2s ease-in-out infinite alternate;
            margin: 10px 0;
        }
        #elementGrid {
            display: grid;
            grid-template-columns: repeat(18, minmax(40px, 60px));
            gap: 3px;
            margin: 10px 0;
            max-width: 95%;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 10px;
            overflow-x: auto;
        }
        #lanthanides, #actinides {
            display: grid;
            grid-template-columns: repeat(15, minmax(40px, 60px));
            gap: 3px;
            margin: 10px 0;
            max-width: 95%;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 10px;
            overflow-x: auto;
        }
        .element {
            background: #333;
            border: 1px solid #00ffff;
            border-radius: 5px;
            padding: 5px;
            text-align: center;
            color: white;
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
            font-size: calc(10px + 1vw);
        }
        .element:hover {
            transform: scale(1.1);
            box-shadow: 0 0 10px #00ffff;
        }
        .empty {
            background: transparent;
            border: none;
        }
        #info {
            position: fixed;
            bottom: 10px;
            left: 10px;
            color: white;
            font-size: calc(12px + 0.5vw);
            z-index: 100;
            background: rgba(0, 0, 0, 0.5);
            padding: 8px;
            border-radius: 5px;
            max-width: 90%;
        }
        #elementDetails {
            position: fixed;
            top: 50px;
            right: 10px;
            color: white;
            font-size: calc(12px + 0.5vw);
            z-index: 100;
            background: rgba(0, 0, 0, 0.7);
            padding: 8px;
            border-radius: 5px;
            max-width: 40%;
        }
        #watermark {
            position: fixed;
            top: 10px;
            right: 10px;
            color: white;
            font-size: calc(12px + 0.5vw);
            font-weight: bold;
            z-index: 1000;
            background: rgba(0, 0, 0, 0.6);
            padding: 5px 10px;
            border-radius: 5px;
        }
        .label {
            position: absolute;
            color: white;
            font-size: calc(8px + 0.5vw);
            background: rgba(0, 0, 0, 0.5);
            padding: 2px 4px;
            border-radius: 3px;
            pointer-events: none;
        }
        #modelSelection {
            position: fixed;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 100;
            display: none;
        }
        #modelSelection select {
            padding: 8px;
            font-size: calc(12px + 0.5vw);
            border-radius: 5px;
            background: #00ffff;
            border: none;
            color: black;
            touch-action: auto;
        }
        #modelTitle {
            position: fixed;
            top: 50px;
            left: 50%;
            transform: translateX(-50%);
            color: white;
            font-size: calc(18px + 1vw);
            z-index: 100;
            text-shadow: 0 0 10px #00ffff;
            display: none;
        }
        #theoryDescription {
            position: fixed;
            top: 80px;
            left: 50%;
            transform: translateX(-50%);
            color: white;
            font-size: calc(12px + 0.5vw);
            z-index: 100;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 5px;
            max-width: 90%;
            text-align: center;
            display: none;
        }
        #backButton {
            position: fixed;
            top: 10px;
            left: 10px;
            padding: 8px 12px;
            background: #00ffff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: calc(12px + 0.5vw);
            z-index: 100;
            display: none;
        }
        @keyframes glow {
            from { text-shadow: 0 0 5px #00ffff; }
            to { text-shadow: 0 0 15px #00ffff; }
        }
        @media (max-width: 600px) {
            #elementGrid, #lanthanides, #actinides {
                grid-template-columns: repeat(18, minmax(30px, 40px));
            }
            .element {
                font-size: calc(8px + 1vw);
                padding: 3px;
            }
            #info, #elementDetails, #theoryDescription {
                font-size: calc(10px + 0.5vw);
            }
            #homeTitle {
                font-size: calc(20px + 2vw);
            }
            #watermark {
                font-size: calc(10px + 0.5vw);
            }
        }
    </style>
</head>
<body>
    <div id="homePage">
        <h1 id="homeTitle">Interactive Periodic Table Atom Simulation</h1>
        <div id="elementGrid"></div>
        <div id="lanthanides"></div>
        <div id="actinides"></div>
    </div>
    <div id="modelSelection">
        <select id="modelDropdown">
            <option value="dalton">Dalton's Billiard Ball Model - John Dalton</option>
            <option value="thomson">Thomson's Plum Pudding Model - J.J. Thomson</option>
            <option value="rutherford">Rutherford's Planetary Model - Ernest Rutherford</option>
            <option value="bohr" selected>Bohr's Quantized Model - Niels Bohr</option>
        </select>
    </div>
    <h2 id="modelTitle"></h2>
    <div id="theoryDescription"></div>
    <div id="info" class="hidden">
        Touch: Drag to rotate, pinch to zoom<br>
        Tap: Zoom to selected atom<br>
        Tap/Hover: View details<br>
        Labels appear when zoomed in<br>
        Tap 'Back' to return to periodic table
    </div>
    <div id="elementDetails" class="hidden"></div>
    <div id="watermark">ANMOL SINGH / IG: rajput_anmolsingh3</div>
    <button id="backButton">Back</button>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.134.0/examples/js/controls/OrbitControls.js"></script>
    <script>
        // Check WebGL support
        if (!window.WebGLRenderingContext || !document.createElement('canvas').getContext('webgl')) {
            console.error('WebGL is not supported in this browser.');
            alert('This application requires WebGL. Please use a compatible browser.');
        }

        // Scene setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        document.body.appendChild(renderer.domElement);

        // Add a simple background grid for visibility confirmation
        const gridHelper = new THREE.GridHelper(20, 20, 0x444444, 0x444444);
        scene.add(gridHelper);

        // Controls
        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.enableZoom = true;
        controls.enablePan = true;
        controls.enableRotate = true;
        controls.maxDistance = 50;
        controls.minDistance = 1;
        controls.touchDampingFactor = 0.1;

        // Lighting
        const ambientLight = new THREE.AmbientLight(0x404040, 0.8);
        scene.add(ambientLight);
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1.0);
        directionalLight.position.set(10, 10, 5);
        scene.add(directionalLight);

        // Element data
        const elementData = [
            { name: 'Hydrogen', symbol: 'H', atomicNumber: 1, massNumber: 1.008, period: 1, group: 1, color: 0xff0000, electronConfig: [1] },
            { name: 'Helium', symbol: 'He', atomicNumber: 2, massNumber: 4.002, period: 1, group: 18, color: 0xffff00, electronConfig: [2] },
            { name: 'Lithium', symbol: 'Li', atomicNumber: 3, massNumber: 6.941, period: 2, group: 1, color: 0xff6600, electronConfig: [2,1] },
            { name: 'Beryllium', symbol: 'Be', atomicNumber: 4, massNumber: 9.012, period: 2, group: 2, color: 0xff9900, electronConfig: [2,2] },
            { name: 'Boron', symbol: 'B', atomicNumber: 5, massNumber: 10.811, period: 2, group: 13, color: 0xccff00, electronConfig: [2,3] },
            { name: 'Carbon', symbol: 'C', atomicNumber: 6, massNumber: 12.011, period: 2, group: 14, color: 0x99ff00, electronConfig: [2,4] },
            { name: 'Nitrogen', symbol: 'N', atomicNumber: 7, massNumber: 14.007, period: 2, group: 15, color: 0x66ff00, electronConfig: [2,5] },
            { name: 'Oxygen', symbol: 'O', atomicNumber: 8, massNumber: 15.999, period: 2, group: 16, color: 0x33ff00, electronConfig: [2,6] },
            { name: 'Fluorine', symbol: 'F', atomicNumber: 9, massNumber: 18.998, period: 2, group: 17, color: 0x00ff00, electronConfig: [2,7] },
            { name: 'Neon', symbol: 'Ne', atomicNumber: 10, massNumber: 20.180, period: 2, group: 18, color: 0x00ff33, electronConfig: [2,8] },
            { name: 'Sodium', symbol: 'Na', atomicNumber: 11, massNumber: 22.990, period: 3, group: 1, color: 0x00ff66, electronConfig: [2,8,1] },
            { name: 'Magnesium', symbol: 'Mg', atomicNumber: 12, massNumber: 24.305, period: 3, group: 2, color: 0x00ff99, electronConfig: [2,8,2] },
            { name: 'Aluminum', symbol: 'Al', atomicNumber: 13, massNumber: 26.982, period: 3, group: 13, color: 0x00ffcc, electronConfig: [2,8,3] },
            { name: 'Silicon', symbol: 'Si', atomicNumber: 14, massNumber: 28.085, period: 3, group: 14, color: 0x00ccff, electronConfig: [2,8,4] },
            { name: 'Phosphorus', symbol: 'P', atomicNumber: 15, massNumber: 30.974, period: 3, group: 15, color: 0x0099ff, electronConfig: [2,8,5] },
            { name: 'Sulfur', symbol: 'S', atomicNumber: 16, massNumber: 32.06, period: 3, group: 16, color: 0x0066ff, electronConfig: [2,8,6] },
            { name: 'Chlorine', symbol: 'Cl', atomicNumber: 17, massNumber: 35.45, period: 3, group: 17, color: 0x0033ff, electronConfig: [2,8,7] },
            { name: 'Argon', symbol: 'Ar', atomicNumber: 18, massNumber: 39.948, period: 3, group: 18, color: 0x0000ff, electronConfig: [2,8,8] },
            { name: 'Potassium', symbol: 'K', atomicNumber: 19, massNumber: 39.098, period: 4, group: 1, color: 0x3300ff, electronConfig: [2,8,8,1] },
            { name: 'Calcium', symbol: 'Ca', atomicNumber: 20, massNumber: 40.078, period: 4, group: 2, color: 0x6600ff, electronConfig: [2,8,8,2] },
            { name: 'Scandium', symbol: 'Sc', atomicNumber: 21, massNumber: 44.956, period: 4, group: 3, color: 0x9900ff, electronConfig: [2,8,9,2] },
            { name: 'Titanium', symbol: 'Ti', atomicNumber: 22, massNumber: 47.867, period: 4, group: 4, color: 0xcc00ff, electronConfig: [2,8,10,2] },
            { name: 'Vanadium', symbol: 'V', atomicNumber: 23, massNumber: 50.942, period: 4, group: 5, color: 0xff00cc, electronConfig: [2,8,11,2] },
            { name: 'Chromium', symbol: 'Cr', atomicNumber: 24, massNumber: 51.996, period: 4, group: 6, color: 0xff0099, electronConfig: [2,8,13,1] },
            { name: 'Manganese', symbol: 'Mn', atomicNumber: 25, massNumber: 54.938, period: 4, group: 7, color: 0xff0066, electronConfig: [2,8,13,2] },
            { name: 'Iron', symbol: 'Fe', atomicNumber: 26, massNumber: 55.845, period: 4, group: 8, color: 0xff0033, electronConfig: [2,8,14,2] },
            { name: 'Cobalt', symbol: 'Co', atomicNumber: 27, massNumber: 58.933, period: 4, group: 9, color: 0xff0000, electronConfig: [2,8,15,2] },
            { name: 'Nickel', symbol: 'Ni', atomicNumber: 28, massNumber: 58.693, period: 4, group: 10, color: 0xcc0000, electronConfig: [2,8,16,2] },
            { name: 'Copper', symbol: 'Cu', atomicNumber: 29, massNumber: 63.546, period: 4, group: 11, color: 0x990000, electronConfig: [2,8,18,1] },
            { name: 'Zinc', symbol: 'Zn', atomicNumber: 30, massNumber: 65.38, period: 4, group: 12, color: 0x660000, electronConfig: [2,8,18,2] },
            { name: 'Gallium', symbol: 'Ga', atomicNumber: 31, massNumber: 69.723, period: 4, group: 13, color: 0x330000, electronConfig: [2,8,18,3] },
            { name: 'Germanium', symbol: 'Ge', atomicNumber: 32, massNumber: 72.630, period: 4, group: 14, color: 0x000033, electronConfig: [2,8,18,4] },
            { name: 'Arsenic', symbol: 'As', atomicNumber: 33, massNumber: 74.922, period: 4, group: 15, color: 0x000066, electronConfig: [2,8,18,5] },
            { name: 'Selenium', symbol: 'Se', atomicNumber: 34, massNumber: 78.971, period: 4, group: 16, color: 0x000099, electronConfig: [2,8,18,6] },
            { name: 'Bromine', symbol: 'Br', atomicNumber: 35, massNumber: 79.904, period: 4, group: 17, color: 0x0000cc, electronConfig: [2,8,18,7] },
            { name: 'Krypton', symbol: 'Kr', atomicNumber: 36, massNumber: 83.798, period: 4, group: 18, color: 0x0000ff, electronConfig: [2,8,18,8] },
            { name: 'Rubidium', symbol: 'Rb', atomicNumber: 37, massNumber: 85.468, period: 5, group: 1, color: 0x3300ff, electronConfig: [2,8,18,8,1] },
            { name: 'Strontium', symbol: 'Sr', atomicNumber: 38, massNumber: 87.62, period: 5, group: 2, color: 0x6600ff, electronConfig: [2,8,18,8,2] },
            { name: 'Yttrium', symbol: 'Y', atomicNumber: 39, massNumber: 88.906, period: 5, group: 3, color: 0x9900ff, electronConfig: [2,8,18,9,2] },
            { name: 'Zirconium', symbol: 'Zr', atomicNumber: 40, massNumber: 91.224, period: 5, group: 4, color: 0xcc00ff, electronConfig: [2,8,18,10,2] },
            { name: 'Niobium', symbol: 'Nb', atomicNumber: 41, massNumber: 92.906, period: 5, group: 5, color: 0xff00cc, electronConfig: [2,8,18,12,1] },
            { name: 'Molybdenum', symbol: 'Mo', atomicNumber: 42, massNumber: 95.95, period: 5, group: 6, color: 0xff0099, electronConfig: [2,8,18,13,1] },
            { name: 'Technetium', symbol: 'Tc', atomicNumber: 43, massNumber: 98, period: 5, group: 7, color: 0xff0066, electronConfig: [2,8,18,13,2] },
            { name: 'Ruthenium', symbol: 'Ru', atomicNumber: 44, massNumber: 101.07, period: 5, group: 8, color: 0xff0033, electronConfig: [2,8,18,15,1] },
            { name: 'Rhodium', symbol: 'Rh', atomicNumber: 45, massNumber: 102.906, period: 5, group: 9, color: 0xff0000, electronConfig: [2,8,18,16,1] },
            { name: 'Palladium', symbol: 'Pd', atomicNumber: 46, massNumber: 106.42, period: 5, group: 10, color: 0xcc0000, electronConfig: [2,8,18,18] },
            { name: 'Silver', symbol: 'Ag', atomicNumber: 47, massNumber: 107.868, period: 5, group: 11, color: 0x990000, electronConfig: [2,8,18,18,1] },
            { name: 'Cadmium', symbol: 'Cd', atomicNumber: 48, massNumber: 112.414, period: 5, group: 12, color: 0x660000, electronConfig: [2,8,18,18,2] },
            { name: 'Indium', symbol: 'In', atomicNumber: 49, massNumber: 114.818, period: 5, group: 13, color: 0x330000, electronConfig: [2,8,18,18,3] },
            { name: 'Tin', symbol: 'Sn', atomicNumber: 50, massNumber: 118.710, period: 5, group: 14, color: 0x000033, electronConfig: [2,8,18,18,4] },
            { name: 'Antimony', symbol: 'Sb', atomicNumber: 51, massNumber: 121.760, period: 5, group: 15, color: 0x000066, electronConfig: [2,8,18,18,5] },
            { name: 'Tellurium', symbol: 'Te', atomicNumber: 52, massNumber: 127.60, period: 5, group: 16, color: 0x000099, electronConfig: [2,8,18,18,6] },
            { name: 'Iodine', symbol: 'I', atomicNumber: 53, massNumber: 126.904, period: 5, group: 17, color: 0x0000cc, electronConfig: [2,8,18,18,7] },
            { name: 'Xenon', symbol: 'Xe', atomicNumber: 54, massNumber: 131.293, period: 5, group: 18, color: 0x0000ff, electronConfig: [2,8,18,18,8] },
            { name: 'Cesium', symbol: 'Cs', atomicNumber: 55, massNumber: 132.905, period: 6, group: 1, color: 0x3300ff, electronConfig: [2,8,18,18,8,1] },
            { name: 'Barium', symbol: 'Ba', atomicNumber: 56, massNumber: 137.327, period: 6, group: 2, color: 0x6600ff, electronConfig: [2,8,18,18,8,2] },
            { name: 'Lanthanum', symbol: 'La', atomicNumber: 57, massNumber: 138.905, period: 8, group: 1, color: 0x9900ff, electronConfig: [2,8,18,18,9,2] },
            { name: 'Cerium', symbol: 'Ce', atomicNumber: 58, massNumber: 140.116, period: 8, group: 2, color: 0xcc00ff, electronConfig: [2,8,18,19,9,2] },
            { name: 'Praseodymium', symbol: 'Pr', atomicNumber: 59, massNumber: 140.908, period: 8, group: 3, color: 0xff00cc, electronConfig: [2,8,18,21,8,2] },
            { name: 'Neodymium', symbol: 'Nd', atomicNumber: 60, massNumber: 144.242, period: 8, group: 4, color: 0xff0099, electronConfig: [2,8,18,22,8,2] },
            { name: 'Promethium', symbol: 'Pm', atomicNumber: 61, massNumber: 145, period: 8, group: 5, color: 0xff0066, electronConfig: [2,8,18,23,8,2] },
            { name: 'Samarium', symbol: 'Sm', atomicNumber: 62, massNumber: 150.36, period: 8, group: 6, color: 0xff0033, electronConfig: [2,8,18,24,8,2] },
            { name: 'Europium', symbol: 'Eu', atomicNumber: 63, massNumber: 151.964, period: 8, group: 7, color: 0xff0000, electronConfig: [2,8,18,25,8,2] },
            { name: 'Gadolinium', symbol: 'Gd', atomicNumber: 64, massNumber: 157.25, period: 8, group: 8, color: 0xcc0000, electronConfig: [2,8,18,25,9,2] },
            { name: 'Terbium', symbol: 'Tb', atomicNumber: 65, massNumber: 158.925, period: 8, group: 9, color: 0x990000, electronConfig: [2,8,18,27,8,2] },
            { name: 'Dysprosium', symbol: 'Dy', atomicNumber: 66, massNumber: 162.500, period: 8, group: 10, color: 0x660000, electronConfig: [2,8,18,28,8,2] },
            { name: 'Holmium', symbol: 'Ho', atomicNumber: 67, massNumber: 164.930, period: 8, group: 11, color: 0x330000, electronConfig: [2,8,18,29,8,2] },
            { name: 'Erbium', symbol: 'Er', atomicNumber: 68, massNumber: 167.259, period: 8, group: 12, color: 0x000033, electronConfig: [2,8,18,30,8,2] },
            { name: 'Thulium', symbol: 'Tm', atomicNumber: 69, massNumber: 168.934, period: 8, group: 13, color: 0x000066, electronConfig: [2,8,18,31,8,2] },
            { name: 'Ytterbium', symbol: 'Yb', atomicNumber: 70, massNumber: 173.045, period: 8, group: 14, color: 0x000099, electronConfig: [2,8,18,32,8,2] },
            { name: 'Lutetium', symbol: 'Lu', atomicNumber: 71, massNumber: 174.967, period: 8, group: 15, color: 0x0000cc, electronConfig: [2,8,18,32,9,2] },
            { name: 'Hafnium', symbol: 'Hf', atomicNumber: 72, massNumber: 178.49, period: 6, group: 4, color: 0x0000ff, electronConfig: [2,8,18,32,10,2] },
            { name: 'Tantalum', symbol: 'Ta', atomicNumber: 73, massNumber: 180.948, period: 6, group: 5, color: 0x3300ff, electronConfig: [2,8,18,32,11,2] },
            { name: 'Tungsten', symbol: 'W', atomicNumber: 74, massNumber: 183.84, period: 6, group: 6, color: 0x6600ff, electronConfig: [2,8,18,32,12,2] },
            { name: 'Rhenium', symbol: 'Re', atomicNumber: 75, massNumber: 186.207, period: 6, group: 7, color: 0x9900ff, electronConfig: [2,8,18,32,13,2] },
            { name: 'Osmium', symbol: 'Os', atomicNumber: 76, massNumber: 190.23, period: 6, group: 8, color: 0xcc00ff, electronConfig: [2,8,18,32,14,2] },
            { name: 'Iridium', symbol: 'Ir', atomicNumber: 77, massNumber: 192.217, period: 6, group: 9, color: 0xff00cc, electronConfig: [2,8,18,32,15,2] },
            { name: 'Platinum', symbol: 'Pt', atomicNumber: 78, massNumber: 195.084, period: 6, group: 10, color: 0xff0099, electronConfig: [2,8,18,32,17,1] },
            { name: 'Gold', symbol: 'Au', atomicNumber: 79, massNumber: 196.967, period: 6, group: 11, color: 0xff0066, electronConfig: [2,8,18,32,18,1] },
            { name: 'Mercury', symbol: 'Hg', atomicNumber: 80, massNumber: 200.592, period: 6, group: 12, color: 0xff0033, electronConfig: [2,8,18,32,18,2] },
            { name: 'Thallium', symbol: 'Tl', atomicNumber: 81, massNumber: 204.38, period: 6, group: 13, color: 0xff0000, electronConfig: [2,8,18,32,18,3] },
            { name: 'Lead', symbol: 'Pb', atomicNumber: 82, massNumber: 207.2, period: 6, group: 14, color: 0xcc0000, electronConfig: [2,8,18,32,18,4] },
            { name: 'Bismuth', symbol: 'Bi', atomicNumber: 83, massNumber: 208.980, period: 6, group: 15, color: 0x990000, electronConfig: [2,8,18,32,18,5] },
            { name: 'Polonium', symbol: 'Po', atomicNumber: 84, massNumber: 209, period: 6, group: 16, color: 0x660000, electronConfig: [2,8,18,32,18,6] },
            { name: 'Astatine', symbol: 'At', atomicNumber: 85, massNumber: 210, period: 6, group: 17, color: 0x330000, electronConfig: [2,8,18,32,18,7] },
            { name: 'Radon', symbol: 'Rn', atomicNumber: 86, massNumber: 222, period: 6, group: 18, color: 0x000033, electronConfig: [2,8,18,32,18,8] },
            { name: 'Francium', symbol: 'Fr', atomicNumber: 87, massNumber: 223, period: 7, group: 1, color: 0x000066, electronConfig: [2,8,18,32,18,8,1] },
            { name: 'Radium', symbol: 'Ra', atomicNumber: 88, massNumber: 226, period: 7, group: 2, color: 0x000099, electronConfig: [2,8,18,32,18,8,2] },
            { name: 'Actinium', symbol: 'Ac', atomicNumber: 89, massNumber: 227, period: 9, group: 1, color: 0x0000cc, electronConfig: [2,8,18,32,18,9,2] },
            { name: 'Thorium', symbol: 'Th', atomicNumber: 90, massNumber: 232.038, period: 9, group: 2, color: 0x0000ff, electronConfig: [2,8,18,32,18,10,2] },
            { name: 'Protactinium', symbol: 'Pa', atomicNumber: 91, massNumber: 231.036, period: 9, group: 3, color: 0x3300ff, electronConfig: [2,8,18,32,20,9,2] },
            { name: 'Uranium', symbol: 'U', atomicNumber: 92, massNumber: 238.029, period: 9, group: 4, color: 0x6600ff, electronConfig: [2,8,18,32,21,9,2] },
            { name: 'Neptunium', symbol: 'Np', atomicNumber: 93, massNumber: 237, period: 9, group: 5, color: 0x9900ff, electronConfig: [2,8,18,32,22,9,2] },
            { name: 'Plutonium', symbol: 'Pu', atomicNumber: 94, massNumber: 244, period: 9, group: 6, color: 0xcc00ff, electronConfig: [2,8,18,32,24,8,2] },
            { name: 'Americium', symbol: 'Am', atomicNumber: 95, massNumber: 243, period: 9, group: 7, color: 0xff00cc, electronConfig: [2,8,18,32,25,8,2] },
            { name: 'Curium', symbol: 'Cm', atomicNumber: 96, massNumber: 247, period: 9, group: 8, color: 0xff0099, electronConfig: [2,8,18,32,25,9,2] },
            { name: 'Berkelium', symbol: 'Bk', atomicNumber: 97, massNumber: 247, period: 9, group: 9, color: 0xff0066, electronConfig: [2,8,18,32,27,8,2] },
            { name: 'Californium', symbol: 'Cf', atomicNumber: 98, massNumber: 251, period: 9, group: 10, color: 0xff0033, electronConfig: [2,8,18,32,28,8,2] },
            { name: 'Einsteinium', symbol: 'Es', atomicNumber: 99, massNumber: 252, period: 9, group: 11, color: 0xff0000, electronConfig: [2,8,18,32,29,8,2] },
            { name: 'Fermium', symbol: 'Fm', atomicNumber: 100, massNumber: 257, period: 9, group: 12, color: 0xcc0000, electronConfig: [2,8,18,32,30,8,2] },
            { name: 'Mendelevium', symbol: 'Md', atomicNumber: 101, massNumber: 258, period: 9, group: 13, color: 0x990000, electronConfig: [2,8,18,32,31,8,2] },
            { name: 'Nobelium', symbol: 'No', atomicNumber: 102, massNumber: 259, period: 9, group: 14, color: 0x660000, electronConfig: [2,8,18,32,32,8,2] },
            { name: 'Lawrencium', symbol: 'Lr', atomicNumber: 103, massNumber: 262, period: 9, group: 15, color: 0x330000, electronConfig: [2,8,18,32,32,8,3] },
            { name: 'Rutherfordium', symbol: 'Rf', atomicNumber: 104, massNumber: 267, period: 7, group: 4, color: 0x000033, electronConfig: [2,8,18,32,32,10,2] },
            { name: 'Dubnium', symbol: 'Db', atomicNumber: 105, massNumber: 268, period: 7, group: 5, color: 0x000066, electronConfig: [2,8,18,32,32,11,2] },
            { name: 'Seaborgium', symbol: 'Sg', atomicNumber: 106, massNumber: 271, period: 7, group: 6, color: 0x000099, electronConfig: [2,8,18,32,32,12,2] },
            { name: 'Bohrium', symbol: 'Bh', atomicNumber: 107, massNumber: 270, period: 7, group: 7, color: 0x0000cc, electronConfig: [2,8,18,32,32,13,2] },
            { name: 'Hassium', symbol: 'Hs', atomicNumber: 108, massNumber: 277, period: 7, group: 8, color: 0x0000ff, electronConfig: [2,8,18,32,32,14,2] },
            { name: 'Meitnerium', symbol: 'Mt', atomicNumber: 109, massNumber: 278, period: 7, group: 9, color: 0x3300ff, electronConfig: [2,8,18,32,32,15,2] },
            { name: 'Darmstadtium', symbol: 'Ds', atomicNumber: 110, massNumber: 281, period: 7, group: 10, color: 0x6600ff, electronConfig: [2,8,18,32,32,17,1] },
            { name: 'Roentgenium', symbol: 'Rg', atomicNumber: 111, massNumber: 282, period: 7, group: 11, color: 0x9900ff, electronConfig: [2,8,18,32,32,18,1] },
            { name: 'Copernicium', symbol: 'Cn', atomicNumber: 112, massNumber: 285, period: 7, group: 12, color: 0xcc00ff, electronConfig: [2,8,18,32,32,18,2] },
            { name: 'Nihonium', symbol: 'Nh', atomicNumber: 113, massNumber: 286, period: 7, group: 13, color: 0xff00cc, electronConfig: [2,8,18,32,32,18,3] },
            { name: 'Flerovium', symbol: 'Fl', atomicNumber: 114, massNumber: 289, period: 7, group: 14, color: 0xff0099, electronConfig: [2,8,18,32,32,18,4] },
            { name: 'Moscovium', symbol: 'Mc', atomicNumber: 115, massNumber: 290, period: 7, group: 15, color: 0xff0066, electronConfig: [2,8,18,32,32,18,5] },
            { name: 'Livermorium', symbol: 'Lv', atomicNumber: 116, massNumber: 293, period: 7, group: 16, color: 0xff0033, electronConfig: [2,8,18,32,32,18,6] },
            { name: 'Tennessine', symbol: 'Ts', atomicNumber: 117, massNumber: 294, period: 7, group: 17, color: 0xff0000, electronConfig: [2,8,18,32,32,18,7] },
            { name: 'Oganesson', symbol: 'Og', atomicNumber: 118, massNumber: 294, period: 7, group: 18, color: 0xcc0000, electronConfig: [2,8,18,32,32,18,8] }
        ];

        // Theories descriptions
        const theories = {
            dalton: "John Dalton's Atomic Theory (1808): All matter is composed of extremely small, indivisible particles called atoms. Atoms of the same element are identical in size, mass, and properties, while atoms of different elements differ. Atoms cannot be created, destroyed, or subdivided. Atoms combine in simple whole-number ratios to form compounds, and chemical reactions involve rearranging atoms.",
            thomson: "J.J. Thomson's Plum Pudding Model (1904): The atom is a sphere of positive charge with negatively charged electrons embedded within it, like plums in a pudding, to maintain electrical neutrality. This model followed the discovery of the electron and explained why atoms are neutral overall.",
            rutherford: "Ernest Rutherford's Nuclear Model (1911): The atom consists of a tiny, dense, positively charged nucleus containing most of the mass, surrounded by mostly empty space with electrons orbiting around it. This was deduced from the gold foil experiment where most alpha particles passed through but some were deflected.",
            bohr: "Niels Bohr's Atomic Model (1913): Electrons orbit the nucleus in fixed, quantized energy levels or shells. Electrons can jump between levels, absorbing or emitting energy as photons. This model explained the hydrogen atom's emission spectrum and introduced the idea of quantization in atomic structure."
        };

        let atomGroups = {};
        let labels = {};
        let currentModel = 'bohr';

        // Home page
        const homePage = document.getElementById('homePage');
        const elementGrid = document.getElementById('elementGrid');
        const lanthanidesGrid = document.getElementById('lanthanides');
        const actinidesGrid = document.getElementById('actinides');
        const maxPeriod = 7;
        const maxGroup = 18;

        try {
            for (let period = 1; period <= maxPeriod; period++) {
                for (let group = 1; group <= maxGroup; group++) {
                    const element = elementData.find(e => e.period === period && e.group === group);
                    const div = document.createElement('div');
                    if (element) {
                        div.className = 'element';
                        div.innerHTML = `
                            ${element.symbol}<br>
                            <small>${element.name}<br>
                            ${element.atomicNumber}<br>
                            ${element.massNumber.toFixed(3)}</small>
                        `;
                        div.addEventListener('click', () => showAtomSimulation(element.symbol));
                    } else {
                        div.className = 'empty';
                    }
                    elementGrid.appendChild(div);
                }
            }

            const lanthanides = elementData.filter(e => e.period === 8);
            for (let group = 1; group <= 15; group++) {
                const element = lanthanides.find(e => e.group === group);
                const div = document.createElement('div');
                if (element) {
                    div.className = 'element';
                    div.innerHTML = `
                        ${element.symbol}<br>
                        <small>${element.name}<br>
                        ${element.atomicNumber}<br>
                        ${element.massNumber.toFixed(3)}</small>
                    `;
                    div.addEventListener('click', () => showAtomSimulation(element.symbol));
                } else {
                    div.className = 'empty';
                }
                lanthanidesGrid.appendChild(div);
            }

            const actinides = elementData.filter(e => e.period === 9);
            for (let group = 1; group <= 15; group++) {
                const element = actinides.find(e => e.group === group);
                const div = document.createElement('div');
                if (element) {
                    div.className = 'element';
                    div.innerHTML = `
                        ${element.symbol}<br>
                        <small>${element.name}<br>
                        ${element.atomicNumber}<br>
                        ${element.massNumber.toFixed(3)}</small>
                    `;
                    div.addEventListener('click', () => showAtomSimulation(element.symbol));
                } else {
                    div.className = 'empty';
                }
                actinidesGrid.appendChild(div);
            }
        } catch (error) {
            console.error('Error building periodic table:', error);
        }

        // Back button
        const backButton = document.getElementById('backButton');
        backButton.addEventListener('click', showHomePage);

        // Model selection
        const modelDropdown = document.getElementById('modelDropdown');
        const theoryDescription = document.getElementById('theoryDescription');
        modelDropdown.addEventListener('change', (event) => {
            currentModel = event.target.value;
            const selectedOption = modelDropdown.options[modelDropdown.selectedIndex].text;
            document.getElementById('modelTitle').textContent = selectedOption;
            theoryDescription.innerHTML = theories[currentModel];
            const currentSymbol = Object.keys(atomGroups)[0];
            if (currentSymbol) {
                showAtomSimulation(currentSymbol);
            }
        });

        function showHomePage() {
            homePage.className = '';
            document.getElementById('info').className = 'hidden';
            document.getElementById('elementDetails').className = 'hidden';
            document.getElementById('modelSelection').style.display = 'none';
            document.getElementById('modelTitle').style.display = 'none';
            theoryDescription.style.display = 'none';
            backButton.style.display = 'none';
            Object.values(atomGroups).forEach(({ group }) => scene.remove(group));
            Object.values(labels).forEach(({ elementLabel, internalLabels = [] }) => {
                elementLabel.remove();
                internalLabels.forEach(l => l.remove());
            });
            atomGroups = {};
            labels = {};
            camera.position.set(0, 5, 10);
            controls.target.set(0, 0, 0);
            controls.update();
            console.log('Returned to home page');
        }

        function showAtomSimulation(symbol) {
            const data = elementData.find(e => e.symbol === symbol);
            if (!data) {
                console.error(`Element ${symbol} not found`);
                return;
            }

            try {
                homePage.className = 'hidden';
                document.getElementById('info').className = '';
                document.getElementById('elementDetails').className = '';
                document.getElementById('modelSelection').style.display = 'block';
                document.getElementById('modelTitle').style.display = 'block';
                theoryDescription.style.display = 'block';
                backButton.style.display = 'block';

                // Clear previous atoms and labels
                Object.values(atomGroups).forEach(({ group }) => scene.remove(group));
                Object.values(labels).forEach(({ elementLabel, internalLabels = [] }) => {
                    elementLabel.remove();
                    internalLabels.forEach(l => l.remove());
                });
                atomGroups = {};
                labels = {};

                const group = new THREE.Group();
                group.name = symbol;
                scene.add(group);
                atomGroups[symbol] = { group, data };

                const nucleusRadius = Math.max(0.5, data.atomicNumber * 0.01);
                const internalLabels = [];

                if (currentModel === 'dalton') {
                    const atomGeo = new THREE.SphereGeometry(nucleusRadius * 2, 12, 12);
                    const atomMat = new THREE.MeshPhongMaterial({ color: data.color });
                    const atom = new THREE.Mesh(atomGeo, atomMat);
                    atom.userData = { type: 'atom', name: 'Atom' };
                    group.add(atom);
                } else if (currentModel === 'thomson') {
                    const atomGeo = new THREE.SphereGeometry(nucleusRadius * 2, 12, 12);
                    const atomMat = new THREE.MeshPhongMaterial({ color: 0x8B4513, transparent: true, opacity: 0.7 });
                    const atom = new THREE.Mesh(atomGeo, atomMat);
                    atom.userData = { type: 'atom', name: 'Positive Sphere' };
                    group.add(atom);

                    const electrons = Math.min(data.atomicNumber, 10);
                    for (let i = 0; i < electrons; i++) {
                        const eGeo = new THREE.SphereGeometry(0.05, 8, 8);
                        const eMat = new THREE.MeshBasicMaterial({ color: 0x00ffff });
                        const electron = new THREE.Mesh(eGeo, eMat);
                        electron.userData = { type: 'electron', name: 'Electron', angle: Math.random() * 2 * Math.PI, speed: 0.02, radius: nucleusRadius * 1.5 };
                        electron.position.set(
                            electron.userData.radius * Math.cos(electron.userData.angle),
                            electron.userData.radius * Math.sin(electron.userData.angle),
                            (Math.random() - 0.5) * nucleusRadius
                        );
                        atom.add(electron);
                    }
                } else if (currentModel === 'rutherford') {
                    const nucleusGeo = new THREE.SphereGeometry(nucleusRadius, 12, 12);
                    const nucleusMat = new THREE.MeshPhongMaterial({ color: 0x8B4513 });
                    const nucleus = new THREE.Mesh(nucleusGeo, nucleusMat);
                    nucleus.userData = { type: 'nucleus', name: 'Nucleus' };
                    group.add(nucleus);

                    const protons = Math.min(data.atomicNumber, 10);
                    const neutrons = Math.min(Math.round(data.massNumber) - data.atomicNumber, 10);
                    if (protons + neutrons < 10) {
                        for (let i = 0; i < protons; i++) {
                            const pGeo = new THREE.SphereGeometry(0.1, 8, 8);
                            const pMat = new THREE.MeshBasicMaterial({ color: 0xff0000 });
                            const p = new THREE.Mesh(pGeo, pMat);
                            p.userData = { type: 'proton', name: 'Proton' };
                            p.position.set(
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8
                            );
                            nucleus.add(p);
                        }
                        for (let i = 0; i < neutrons; i++) {
                            const nGeo = new THREE.SphereGeometry(0.1, 8, 8);
                            const nMat = new THREE.MeshBasicMaterial({ color: 0x0000ff });
                            const n = new THREE.Mesh(nGeo, nMat);
                            n.userData = { type: 'neutron', name: 'Neutron' };
                            n.position.set(
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8
                            );
                            nucleus.add(n);
                        }
                    }

                    const electrons = Math.min(data.atomicNumber, 10);
                    for (let i = 0; i < electrons; i++) {
                        const shellGroup = new THREE.Group();
                        group.add(shellGroup);
                        const radius = nucleusRadius + 1 + i * 0.5;
                        const electronGeo = new THREE.SphereGeometry(0.05, 8, 8);
                        const electronMat = new THREE.MeshBasicMaterial({ color: 0x00ffff });
                        const electron = new THREE.Mesh(electronGeo, electronMat);
                        electron.userData = { type: 'electron', name: 'Electron', angle: Math.random() * 2 * Math.PI, speed: 0.02, radius };
                        electron.position.set(radius * Math.cos(electron.userData.angle), 0, radius * Math.sin(electron.userData.angle));
                        shellGroup.add(electron);
                    }
                } else {
                    const nucleusGeo = new THREE.SphereGeometry(nucleusRadius, 12, 12);
                    const nucleusMat = new THREE.MeshPhongMaterial({ color: 0x8B4513 });
                    const nucleus = new THREE.Mesh(nucleusGeo, nucleusMat);
                    nucleus.userData = { type: 'nucleus', name: 'Nucleus' };
                    group.add(nucleus);

                    const protons = Math.min(data.atomicNumber, 10);
                    const neutrons = Math.min(Math.round(data.massNumber) - data.atomicNumber, 10);
                    if (protons + neutrons < 10) {
                        for (let i = 0; i < protons; i++) {
                            const pGeo = new THREE.SphereGeometry(0.1, 8, 8);
                            const pMat = new THREE.MeshBasicMaterial({ color: 0xff0000 });
                            const p = new THREE.Mesh(pGeo, pMat);
                            p.userData = { type: 'proton', name: 'Proton' };
                            p.position.set(
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Mathandom() - 0.5) * nucleusRadius * 0.8
                            );
                            nucleus.add(p);
                        }
                        for (let i = 0; i < neutrons; i++) {
                            const nGeo = new THREE.SphereGeometry(0.1, 8, 8);
                            const nMat = new THREE.MeshBasicMaterial({ color: 0x0000ff });
                            const n = new THREE.Mesh(nGeo, nMat);
                            n.userData = { type: 'neutron', name: 'Neutron' };
                            n.position.set(
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8,
                                (Math.random() - 0.5) * nucleusRadius * 0.8
                            );
                            nucleus.add(n);
                        }
                    }

                    let shellRadius = nucleusRadius + 1;
                    const maxShells = Math.min(data.electronConfig.length, 3);
                    data.electronConfig.slice(0, maxShells).forEach((electronsInShell, shellIndex) => {
                        const shellGroup = new THREE.Group();
                        group.add(shellGroup);

                        const ringGeo = new THREE.RingGeometry(shellRadius - 0.1, shellRadius + 0.1, 32);
                        const ringMat = new THREE.MeshBasicMaterial({ color: 0x444444, transparent: true, opacity: 0.3 });
                        const ring = new THREE.Mesh(ringGeo, ringMat);
                        ring.rotation.x = Math.PI / 2;
                        shellGroup.add(ring);

                        const electrons = Math.min(electronsInShell, 8);
                        for (let i = 0; i < electrons; i++) {
                            const angle = (i / electrons) * Math.PI * 2;
                            const electronGeo = new THREE.SphereGeometry(0.05, 8, 8);
                            const electronMat = new THREE.MeshBasicMaterial({ color: 0x00ffff });
                            const electron = new THREE.Mesh(electronGeo, electronMat);
                            electron.userData = { type: 'electron', name: 'Electron', angle, speed: 0.03 / (shellIndex + 1), radius: shellRadius };
                            electron.position.set(Math.cos(angle) * shellRadius, 0, Math.sin(angle) * shellRadius);
                            shellGroup.add(electron);
                        }

                        shellRadius += 1.5;
                    });
                }

                const elementLabel = document.createElement('div');
                elementLabel.className = 'label';
                elementLabel.textContent = `${data.symbol} - ${data.name}`;
                document.body.appendChild(elementLabel);
                labels[symbol] = { elementLabel, internalLabels };

                camera.position.set(0, 5, 10);
                controls.target.set(0, 0, 0);
                controls.update();

                console.log(`Loaded ${symbol} in ${currentModel} model`);
            } catch (error) {
                console.error(`Error rendering atom simulation for ${symbol}:`, error);
            }
        }

        function updateLabels() {
            try {
                Object.values(atomGroups).forEach(({ group, data }) => {
                    const symbol = data.symbol;
                    const distance = camera.position.distanceTo(group.position);
                    const elementLabel = labels[symbol].elementLabel;
                    const internalLabels = labels[symbol].internalLabels;

                    if (distance < 20) {
                        const screenPos = group.position.clone().project(camera);
                        const x = (screenPos.x * 0.5 + 0.5) * window.innerWidth;
                        const y = (-screenPos.y * 0.5 + 0.5) * window.innerHeight;
                        elementLabel.style.display = 'block';
                        elementLabel.style.left = `${x}px`;
                        elementLabel.style.top = `${y - 20}px`;
                    } else {
                        elementLabel.style.display = 'none';
                    }

                    if (distance < 5 && currentModel !== 'dalton') {
                        const nucleus = group.children.find(c => c.userData.type === 'nucleus' || c.userData.type === 'atom');
                        if (nucleus) {
                            let nucleusLabel = internalLabels.find(l => l.id === `nucleusLabel_${symbol}`);
                            if (!nucleusLabel) {
                                nucleusLabel = document.createElement('div');
                                nucleusLabel.className = 'label';
                                nucleusLabel.id = `nucleusLabel_${symbol}`;
                                nucleusLabel.textContent = currentModel === 'thomson' ? 
                                    `Positive Sphere` : 
                                    `Nucleus (P: ${data.atomicNumber}, N: ${Math.round(data.massNumber) - data.atomicNumber})`;
                                document.body.appendChild(nucleusLabel);
                                internalLabels.push(nucleusLabel);
                            }
                            const screenPos = nucleus.position.clone().applyMatrix4(group.matrixWorld).project(camera);
                            const x = (screenPos.x * 0.5 + 0.5) * window.innerWidth;
                            const y = (-screenPos.y * 0.5 + 0.5) * window.innerHeight;
                            nucleusLabel.style.display = 'block';
                            nucleusLabel.style.left = `${x}px`;
                            nucleusLabel.style.top = `${y - 10}px`;
                        }

                        group.traverse(child => {
                            if (child.userData.type === 'proton' || child.userData.type === 'neutron' || child.userData.type === 'electron') {
                                let label = internalLabels.find(l => l.id === `${child.userData.type}_${child.uuid}`);
                                if (!label) {
                                    label = document.createElement('div');
                                    label.className = 'label';
                                    label.id = `${child.userData.type}_${child.uuid}`;
                                    label.textContent = child.userData.name;
                                    document.body.appendChild(label);
                                    internalLabels.push(label);
                                }
                                const worldPos = child.position.clone().applyMatrix4(child.parent.matrixWorld).applyMatrix4(group.matrixWorld);
                                const screenPos = worldPos.project(camera);
                                const x = (screenPos.x * 0.5 + 0.5) * window.innerWidth;
                                const y = (-screenPos.y * 0.5 + 0.5) * window.innerHeight;
                                label.style.display = 'block';
                                label.style.left = `${x}px`;
                                label.style.top = `${y}px`;
                            }
                        });
                    } else {
                        internalLabels.forEach(l => l.style.display = 'none');
                    }
                });
            } catch (error) {
                console.error('Error updating labels:', error);
            }
        }

        // Mouse and touch events
        const raycaster = new THREE.Raycaster();
        const mouse = new THREE.Vector2();
        let lastIntersected = null;
        let lastTapTime = 0;

        function handleInteraction(event) {
            if (homePage.className === 'hidden') {
                const now = Date.now();
                if (event.type === 'touchstart' && now - lastTapTime < 300) return;
                lastTapTime = now;

                const clientX = event.type === 'touchstart' ? event.touches[0].clientX : event.clientX;
                const clientY = event.type === 'touchstart' ? event.touches[0].clientY : event.clientY;
                mouse.x = (clientX / window.innerWidth) * 2 - 1;
                mouse.y = -(clientY / window.innerHeight) * 2 + 1;

                raycaster.setFromCamera(mouse, camera);
                const intersects = raycaster.intersectObjects(Object.values(atomGroups).map(a => a.group), true);

                const detailsDiv = document.getElementById('elementDetails');
                if (intersects.length > 0) {
                    const intersected = intersects[0].object;
                    const group = intersected.parent.name ? intersected.parent : intersected.parent.parent;
                    const data = atomGroups[group.name]?.data;
                    if (data && lastIntersected !== group) {
                        lastIntersected = group;
                        detailsDiv.innerHTML = `<strong>${data.name} (${data.symbol})</strong><br>Atomic Number: ${data.atomicNumber}<br>Atomic Mass: ${data.massNumber.toFixed(3)}<br>Protons: ${data.atomicNumber}<br>Neutrons: ${Math.round(data.massNumber) - data.atomicNumber}<br>Electrons: ${data.atomicNumber}`;
                    }
                    if (event.type === 'touchstart' || event.type === 'click') {
                        const pos = group.position.clone();
                        camera.position.set(pos.x, pos.y + 5, pos.z + 5);
                        camera.lookAt(pos);
                        controls.target.copy(pos);
                        controls.update();
                    }
                } else {
                    lastIntersected = null;
                    detailsDiv.innerHTML = '';
                }
            }
        }

        window.addEventListener('mousemove', handleInteraction);
        window.addEventListener('click', handleInteraction);
        window.addEventListener('touchstart', handleInteraction);

        // Animation loop
        function animate() {
            requestAnimationFrame(animate);

            if (homePage.className === 'hidden') {
                try {
                    Object.values(atomGroups).forEach(({ group }) => {
                        group.children.forEach(child => {
                            if (child.userData.type === 'atom' && currentModel === 'thomson') {
                                child.children.forEach(electron => {
                                    if (electron.userData.angle !== undefined) {
                                        electron.userData.angle += electron.userData.speed;
                                        electron.position.set(
                                            electron.userData.radius * Math.cos(electron.userData.angle),
                                            electron.userData.radius * Math.sin(electron.userData.angle),
                                            electron.position.z
                                        );
                                    }
                                });
                            } else if (currentModel === 'rutherford') {
                                child.children.forEach(electron => {
                                    if (electron.userData.angle !== undefined) {
                                        electron.userData.angle += electron.userData.speed;
                                        electron.position.set(
                                            electron.userData.radius * Math.cos(electron.userData.angle),
                                            0,
                                            electron.userData.radius * Math.sin(electron.userData.angle)
                                        );
                                    }
                                });
                            } else if (currentModel === 'bohr') {
                                if (child.children.length === 0) {
                                    child.rotation.y += 0.02 / (child.children.length + 1);
                                }
                                child.children.forEach(electron => {
                                    if (electron.userData.angle !== undefined) {
                                        electron.userData.angle += electron.userData.speed;
                                        electron.position.set(
                                            electron.userData.radius * Math.cos(electron.userData.angle),
                                            0,
                                            electron.userData.radius * Math.sin(electron.userData.angle)
                                        );
                                    }
                                });
                            }
                        });
                    });

                    updateLabels();
                    controls.update();
                } catch (error) {
                    console.error('Error in animation loop:', error);
                }
            }

            renderer.render(scene, camera);
        }

        camera.position.set(0, 5, 10);
        controls.target.set(0, 0, 0);
        animate();

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            updateLabels();
        });
    </script>
</body>
</html>
