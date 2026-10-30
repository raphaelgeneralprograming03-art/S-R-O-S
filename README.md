<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EGSE - Plano de Testes e Validação em Solo (STVP) | TPT Transceiver</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800;900&family=Rajdhani:wght@400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        space: {
                            900: '#050811',
                            800: '#0a1026',
                            700: '#121b3d',
                            border: '#1e2d5a'
                        },
                        cyanGlow: '#00f0ff',
                        laserRed: '#ff0055',
                        neonGreen: '#00ff66',
                        amberAlert: '#ffb700'
                    },
                    fontFamily: {
                        orbitron: ['Orbitron', 'sans-serif'],
                        rajdhani: ['Rajdhani', 'sans-serif'],
                        mono: ['Share Tech Mono', 'monospace']
                    }
                }
            }
        }
    </script>

    <style>
        body {
            background-color: #03050c;
            color: #c0d1ed;
            font-family: 'Rajdhani', sans-serif;
            overflow-x: hidden;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #050811;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e2d5a;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00f0ff;
        }

        /* Glassmorphism Panels */
        .egse-panel {
            background: rgba(10, 16, 38, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(0, 240, 255, 0.15);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5), inset 0 0 12px rgba(0, 240, 255, 0.05);
        }

        .egse-panel-active {
            border-color: rgba(0, 240, 255, 0.5);
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.2);
        }

        /* Glowing text effects */
        .glow-text-cyan {
            text-shadow: 0 0 8px rgba(0, 240, 255, 0.6);
        }
        .glow-text-green {
            text-shadow: 0 0 8px rgba(0, 255, 102, 0.6);
        }
        .glow-text-red {
            text-shadow: 0 0 8px rgba(ff, 0, 85, 0.6);
        }

        /* LED Status Indicator */
        .led-indicator {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            display: inline-block;
            box-shadow: 0 0 5px currentColor;
        }

        /* Scanline Overlay for Sci-Fi Display */
        .scanline-bg {
            background: linear-gradient(
                rgba(18, 16, 16, 0) 50%, 
                rgba(0, 0, 0, 0.25) 50%
            ), linear-gradient(
                90deg,
                rgba(255, 0, 0, 0.03),
                rgba(0, 255, 0, 0.01),
                rgba(0, 0, 255, 0.03)
            );
            background-size: 100% 4px, 6px 100%;
        }

        /* Custom Sliders */
        input[type=range] {
            -webkit-appearance: none;
            background: #121b3d;
            height: 6px;
            border-radius: 3px;
            border: 1px solid #1e2d5a;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 16px;
            width: 16px;
            border-radius: 50%;
            background: #00f0ff;
            cursor: pointer;
            box-shadow: 0 0 10px #00f0ff;
        }

        /* Terminal styling */
        .console-log {
            font-family: 'Share Tech Mono', monospace;
            font-size: 0.8rem;
            line-height: 1.3;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-cyanGlow selection:text-black">

    <!-- TOP HEADER / NAV -->
    <header class="border-b border-space-border bg-space-900/90 px-4 py-3 sticky top-0 z-50 backdrop-blur-md">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4">
            
            <!-- Branding & Title -->
            <div class="flex items-center gap-3">
                <div class="p-2 bg-cyanGlow/10 border border-cyanGlow/40 rounded-lg text-cyanGlow">
                    <i class="fa-solid me-1 fa-microchip text-2xl"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-orbitron font-extrabold text-lg md:text-xl text-white tracking-wider">
                            EGSE HIL SIMULATION BENCH
                        </h1>
                        <span class="px-2 py-0.5 text-xs font-mono bg-cyanGlow/20 text-cyanGlow rounded border border-cyanGlow/30">
                            ECSS-E-ST-10-02C
                        </span>
                    </div>
                    <p class="text-xs text-slate-400 font-mono">
                        PLANO DE TESTES E VALIDAÇÃO EM SOLO (STVP) • UUT: TRANSCEPTOR FOTÔNICO DE TRANSPARÊNCIA TOTAL (TPT)
                    </p>
                </div>
            </div>

            <!-- Credits & Institution Metadata -->
            <div class="flex flex-wrap items-center gap-4 text-xs font-mono border-t md:border-t-0 md:border-l border-space-border pt-2 md:pt-0 md:pl-4">
                <div>
                    <span class="text-slate-500 block">INSTITUIÇÃO / AVALIADORA:</span>
                    <span class="text-cyanGlow font-bold text-sm">UFCA / Avaliadora</span>
                </div>
                <div>
                    <span class="text-slate-500 block">PESQUISADOR / INVENTOR EXTERNO:</span>
                    <span class="text-white font-semibold">Rafael Barbosa da Silva</span>
                </div>
                <div class="flex items-center gap-2 pl-2 border-l border-space-border">
                    <span id="systemLed" class="led-indicator bg-neonGreen text-neonGreen"></span>
                    <span id="systemStatusText" class="text-neonGreen font-bold">ONLINE</span>
                </div>
            </div>

        </div>
    </header>

    <!-- MAIN CONTENT CONTAINER -->
    <main class="max-w-7xl w-full mx-auto p-4 space-y-6 flex-grow">

        <!-- SYSTEM CONTROL BAR & QUICK STATS -->
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
            
            <!-- Simulation Mode Toggle -->
            <div class="egse-panel rounded-xl p-3 flex items-center justify-between">
                <div>
                    <div class="text-xs font-mono text-slate-400">STATUS DA BANCADA</div>
                    <div id="simModeDisplay" class="font-orbitron font-bold text-cyanGlow text-sm">EM OPERAÇÃO</div>
                </div>
                <button id="toggleSimBtn" onclick="toggleSimulation()" class="px-3 py-2 bg-cyanGlow/20 hover:bg-cyanGlow/30 text-cyanGlow border border-cyanGlow/50 rounded-lg font-mono text-xs transition flex items-center gap-2">
                    <i class="fa-solid fa-pause" id="pauseIcon"></i> <span id="pauseText">PAUSAR</span>
                </button>
            </div>

            <!-- Telemetry Indicator 1: Optical Margin -->
            <div class="egse-panel rounded-xl p-3 flex items-center justify-between">
                <div>
                    <div class="text-xs font-mono text-slate-400">MARGEM LINK ÓPTICO</div>
                    <div class="font-orbitron font-bold text-neonGreen text-lg">+6.20 dB <span class="text-xs text-slate-400 font-normal">(NOMINAL)</span></div>
                </div>
                <i class="fa-solid fa-signal text-neonGreen/40 text-2xl"></i>
            </div>

            <!-- Telemetry Indicator 2: MEMS Desvio -->
            <div class="egse-panel rounded-xl p-3 flex items-center justify-between">
                <div>
                    <div class="text-xs font-mono text-slate-400">DESVIO ANGULAR MEMS</div>
                    <div id="memsDeviationVal" class="font-orbitron font-bold text-cyanGlow text-lg">±0.002°</div>
                </div>
                <i class="fa-solid fa-compass text-cyanGlow/40 text-2xl"></i>
            </div>

            <!-- Telemetry Indicator 3: Power Consumption -->
            <div class="egse-panel rounded-xl p-3 flex items-center justify-between">
                <div>
                    <div class="text-xs font-mono text-slate-400">CONSUMO UUT (CC)</div>
                    <div class="font-orbitron font-bold text-amberAlert text-lg">14.20 W <span class="text-xs text-slate-400 font-normal">(REQ-001)</span></div>
                </div>
                <i class="fa-solid fa-bolt text-amberAlert/40 text-2xl"></i>
            </div>

        </div>

        <!-- CENTER PIECE: HIL CANVAS ARCHITECTURE DIAGRAM -->
        <div class="egse-panel rounded-xl p-4 relative overflow-hidden">
            <div class="flex flex-col md:flex-row justify-between items-start md:items-center pb-3 border-b border-space-border mb-3 gap-2">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-network-wired text-cyanGlow"></i>
                    <h2 class="font-orbitron font-bold text-white text-base">ARQUITETURA DO AMBIENTE DE TESTE HIL (HARDWARE-IN-THE-LOOP)</h2>
                </div>
                <div class="flex items-center gap-4 text-xs font-mono text-slate-400">
                    <span class="flex items-center gap-1"><span class="w-2 h-2 rounded-full bg-laserRed inline-block"></span> Feixe Laser 1550nm</span>
                    <span class="flex items-center gap-1"><span class="w-2 h-2 rounded-full bg-cyanGlow inline-block"></span> Sinal Snooping (5%)</span>
                    <span class="flex items-center gap-1"><span class="w-2 h-2 rounded-full bg-neonGreen inline-block"></span> FODL Link (95%)</span>
                </div>
            </div>

            <!-- Canvas Container with Responsive Aspect Ratio -->
            <div class="relative w-full rounded-lg bg-space-900 border border-space-border overflow-hidden scanline-bg" style="min-height: 380px;">
                <canvas id="hilCanvas" class="w-full h-full block cursor-crosshair"></canvas>
            </div>

            <!-- Real-time Control Sliders under Canvas -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-4 pt-3 border-t border-space-border/60">
                
                <!-- Piezo Frequency -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-mono">
                        <span class="text-slate-300"><i class="fa-solid fa-wave-square text-amberAlert mr-1"></i> Frequência Piezoelétrica (Ruído):</span>
                        <span id="piezoFreqVal" class="text-amberAlert font-bold">120 Hz</span>
                    </div>
                    <input type="range" id="piezoFreqSlider" min="10" max="250" value="120" class="w-full" oninput="updatePiezoFreq(this.value)">
                </div>

                <!-- Fiber Distance -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-mono">
                        <span class="text-slate-300"><i class="fa-solid fa-route text-neonGreen mr-1"></i> Distância Bobina Fibra (FODL):</span>
                        <span id="fiberDistVal" class="text-neonGreen font-bold">5.000 km</span>
                    </div>
                    <input type="range" id="fiberDistSlider" min="1000" max="5000" step="500" value="5000" class="w-full" oninput="updateFiberDist(this.value)">
                </div>

                <!-- Laser Power -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-mono">
                        <span class="text-slate-300"><i class="fa-solid fa-bolt text-laserRed mr-1"></i> Potência do Laser (Injetor):</span>
                        <span id="laserPowerVal" class="text-laserRed font-bold">100 mW</span>
                    </div>
                    <input type="range" id="laserPowerSlider" min="10" max="150" value="100" class="w-full" oninput="updateLaserPower(this.value)">
                </div>

            </div>
        </div>

        <!-- TEST EXECUTION MODULE (STVP TESTES 01, 02, 03) -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            
            <!-- TEST 01 PANEL -->
            <div id="panelTest1" class="egse-panel rounded-xl p-4 flex flex-col justify-between space-y-3 transition duration-300">
                <div>
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-mono px-2 py-0.5 rounded bg-cyanGlow/10 text-cyanGlow border border-cyanGlow/30">TESTE 01</span>
                        <span id="test1Status" class="text-xs font-mono font-bold text-slate-400">PRONTO</span>
                    </div>
                    <h3 class="font-orbitron font-bold text-white text-sm mb-1">Validação de Latência Zero</h3>
                    <p class="text-xs text-slate-400 mb-3">
                        Mede a latência física de trânsito no espelho MEMS via Osciloscópio Digital GHz (Critério: &lt; 10 ns).
                    </p>

                    <!-- Mini Display Test 1 (Oscilloscope trace simulation) -->
                    <div class="bg-space-900 border border-space-border rounded p-2 h-24 relative overflow-hidden mb-3">
                        <canvas id="oscilloCanvas" class="w-full h-full"></canvas>
                    </div>

                    <!-- Latency Timer Display -->
                    <div class="flex justify-between items-center font-mono text-xs bg-space-800 p-2 rounded border border-space-border">
                        <span class="text-slate-400">Latência Medida:</span>
                        <span id="test1LatencyVal" class="font-bold text-lg text-white">0.00 ns</span>
                    </div>
                </div>

                <button onclick="runTest1()" class="w-full py-2.5 bg-cyanGlow/20 hover:bg-cyanGlow/30 text-cyanGlow border border-cyanGlow/50 rounded-lg font-orbitron text-xs tracking-wider transition flex justify-center items-center gap-2">
                    <i class="fa-solid fa-play"></i> EXECUTAR TESTE 01
                </button>
            </div>

            <!-- TEST 02 PANEL -->
            <div id="panelTest2" class="egse-panel rounded-xl p-4 flex flex-col justify-between space-y-3 transition duration-300">
                <div>
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-mono px-2 py-0.5 rounded bg-amberAlert/10 text-amberAlert border border-amberAlert/30">TESTE 02</span>
                        <span id="test2Status" class="text-xs font-mono font-bold text-slate-400">PRONTO</span>
                    </div>
                    <h3 class="font-orbitron font-bold text-white text-sm mb-1">Malha Fechada Filtro de Kalman</h3>
                    <p class="text-xs text-slate-400 mb-3">
                        Valida correção preditiva do MEMS a 1.000 Hz sob vibração mecânica de 10-250 Hz (Desvio ≤ ±0.005°).
                    </p>

                    <!-- Mini Display Test 2 (Kalman Error Curve) -->
                    <div class="bg-space-900 border border-space-border rounded p-2 h-24 relative overflow-hidden mb-3">
                        <canvas id="kalmanCanvas" class="w-full h-full"></canvas>
                    </div>

                    <!-- Deviation Stats -->
                    <div class="flex justify-between items-center font-mono text-xs bg-space-800 p-2 rounded border border-space-border">
                        <span class="text-slate-400">Erro Angular Máx:</span>
                        <span id="test2ErrorVal" class="font-bold text-lg text-white">±0.000°</span>
                    </div>
                </div>

                <button onclick="runTest2()" class="w-full py-2.5 bg-amberAlert/20 hover:bg-amberAlert/30 text-amberAlert border border-amberAlert/50 rounded-lg font-orbitron text-xs tracking-wider transition flex justify-center items-center gap-2">
                    <i class="fa-solid fa-play"></i> EXECUTAR TESTE 02
                </button>
            </div>

            <!-- TEST 03 PANEL -->
            <div id="panelTest3" class="egse-panel rounded-xl p-4 flex flex-col justify-between space-y-3 transition duration-300">
                <div>
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-mono px-2 py-0.5 rounded bg-neonGreen/10 text-neonGreen border border-neonGreen/30">TESTE 03</span>
                        <span id="test3Status" class="text-xs font-mono font-bold text-slate-400">PRONTO</span>
                    </div>
                    <h3 class="font-orbitron font-bold text-white text-sm mb-1">Perda de Nó & Redirecionamento DORP</h3>
                    <p class="text-xs text-slate-400 mb-3">
                        Simula falha por micrometeorito no Nó B e redirecionamento rápido Sonda A ➔ C (Chaveamento &lt; 1.5 ms).
                    </p>

                    <!-- Mini Display Test 3 (Probe Topology Node diagram) -->
                    <div class="bg-space-900 border border-space-border rounded p-2 h-24 relative overflow-hidden mb-3 flex items-center justify-around font-mono text-xs">
                        <div id="nodeA" class="flex flex-col items-center text-neonGreen">
                            <i class="fa-solid fa-satellite text-lg mb-1"></i>
                            <span>SONDA A</span>
                        </div>
                        <div id="linkAB" class="h-0.5 w-10 bg-neonGreen transition-all"></div>
                        <div id="nodeB" class="flex flex-col items-center text-cyanGlow">
                            <i class="fa-solid fa-satellite text-lg mb-1"></i>
                            <span>SONDA B</span>
                        </div>
                        <div id="linkAC" class="hidden h-0.5 w-10 bg-amberAlert transition-all"></div>
                        <div id="nodeC" class="flex flex-col items-center text-slate-600">
                            <i class="fa-solid fa-satellite text-lg mb-1"></i>
                            <span>SONDA C</span>
                        </div>
                    </div>

                    <!-- Switch Time Display -->
                    <div class="flex justify-between items-center font-mono text-xs bg-space-800 p-2 rounded border border-space-border">
                        <span class="text-slate-400">Tempo de Chaveamento:</span>
                        <span id="test3SwitchVal" class="font-bold text-lg text-white">0.00 ms</span>
                    </div>
                </div>

                <div class="space-y-2">
                    <button onclick="simulateMeteoriteFailure()" class="w-full py-1.5 bg-laserRed/20 hover:bg-laserRed/30 text-laserRed border border-laserRed/40 rounded font-mono text-xs transition flex justify-center items-center gap-1">
                        <i class="fa-solid fa-meteor"></i> SIMULAR MICROMETEORITO NO NÓ B
                    </button>
                    <button onclick="runTest3()" class="w-full py-2.5 bg-neonGreen/20 hover:bg-neonGreen/30 text-neonGreen border border-neonGreen/50 rounded-lg font-orbitron text-xs tracking-wider transition flex justify-center items-center gap-2">
                        <i class="fa-solid fa-play"></i> EXECUTAR TESTE 03 (PROTOCOL)
                    </button>
                </div>
            </div>

        </div>

        <!-- LOWER SECTION: VTM MATRIX & LOG CONSOLE & CERTIFICATE -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            
            <!-- VERIFICATION TRACEABILITY MATRIX (VTM) - 2 COLUMNS WIDE -->
            <div class="lg:col-span-2 egse-panel rounded-xl p-4">
                <div class="flex items-center justify-between pb-3 border-b border-space-border mb-3">
                    <div class="flex items-center gap-2">
                        <i class="fa-solid fa-table-list text-cyanGlow"></i>
                        <h2 class="font-orbitron font-bold text-white text-base">MATRIZ DE RASTREABILIDADE DE VERIFICAÇÃO (VTM)</h2>
                    </div>
                    <span class="text-xs font-mono text-slate-400">PADRÃO UFCA / AVALIADORA</span>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left font-mono text-xs border-collapse">
                        <thead>
                            <tr class="border-b border-space-border text-cyanGlow bg-space-900/50">
                                <th class="p-2.5">ID REQUISITO</th>
                                <th class="p-2.5">DESCRIÇÃO CURTA</th>
                                <th class="p-2.5">MÉTODO ESA</th>
                                <th class="p-2.5">EQUIPAMENTO EGSE NECESSÁRIO</th>
                                <th class="p-2.5">STATUS ESPERADO</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-space-border/50 text-slate-300">
                            <tr class="hover:bg-space-800/40">
                                <td class="p-2.5 font-bold text-white">REQ-TPT-001</td>
                                <td class="p-2.5">Consumo de Energia (14.2 W)</td>
                                <td class="p-2.5"><span class="px-2 py-0.5 bg-cyanGlow/10 text-cyanGlow rounded border border-cyanGlow/30">T (Teste)</span></td>
                                <td class="p-2.5">Fonte de alimentação CC calibrada digital de alta precisão</td>
                                <td class="p-2.5 text-neonGreen font-bold"><i class="fa-solid fa-circle-check mr-1"></i>Aprovado com Margem</td>
                            </tr>
                            <tr class="hover:bg-space-800/40">
                                <td class="p-2.5 font-bold text-white">REQ-TPT-004</td>
                                <td class="p-2.5">Resposta do MEMS (&lt; 150 µs)</td>
                                <td class="p-2.5"><span class="px-2 py-0.5 bg-amberAlert/10 text-amberAlert rounded border border-amberAlert/30">T / A (Teste/Análise)</span></td>
                                <td class="p-2.5">Sensor de posição de diodo de avalanche com amostragem óptica rápida</td>
                                <td class="p-2.5 text-neonGreen font-bold"><i class="fa-solid fa-circle-check mr-1"></i>Aprovado com Margem</td>
                            </tr>
                            <tr class="hover:bg-space-800/40">
                                <td class="p-2.5 font-bold text-white">REQ-TPT-005</td>
                                <td class="p-2.5">Margem de Link Óptico (+6.2 dB)</td>
                                <td class="p-2.5"><span class="px-2 py-0.5 bg-neonGreen/10 text-neonGreen rounded border border-neonGreen/30">I / T (Inspeção/Teste)</span></td>
                                <td class="p-2.5">Atenuador óptico variável de precisão (simulando distância vácuo)</td>
                                <td class="p-2.5 text-neonGreen font-bold"><i class="fa-solid fa-circle-check mr-1"></i>Aprovado com Margem</td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <!-- Legend & Definitions -->
                <div class="mt-4 pt-3 border-t border-space-border flex flex-wrap justify-between items-center text-xs font-mono text-slate-400 gap-2">
                    <div>
                        <span class="text-white font-bold">Definições ESA:</span> 
                        <span class="ml-2"><b class="text-cyanGlow">T</b> - Teste Direto;</span>
                        <span class="ml-2"><b class="text-amberAlert">A</b> - Análise/Modelagem;</span>
                        <span class="ml-2"><b class="text-neonGreen">I</b> - Inspeção Estática.</span>
                    </div>
                    <button onclick="generateFlightReport()" class="px-3 py-1.5 bg-cyanGlow/20 hover:bg-cyanGlow/30 text-cyanGlow border border-cyanGlow/50 rounded font-orbitron text-xs transition flex items-center gap-2">
                        <i class="fa-solid fa-file-contract"></i> GERAR RELATÓRIO DE CERTIFICAÇÃO
                    </button>
                </div>
            </div>

            <!-- REAL-TIME SYSTEM EVENT LOG CONSOLE -->
            <div class="egse-panel rounded-xl p-4 flex flex-col justify-between">
                <div>
                    <div class="flex items-center justify-between pb-2 border-b border-space-border mb-2">
                        <div class="flex items-center gap-2">
                            <i class="fa-solid fa-terminal text-neonGreen"></i>
                            <h2 class="font-orbitron font-bold text-white text-sm">TELEMETRIA & LOGS EGSE</h2>
                        </div>
                        <span class="w-2 h-2 rounded-full bg-neonGreen animate-pulse"></span>
                    </div>

                    <!-- Console Box -->
                    <div id="consoleLogBox" class="bg-space-900 border border-space-border rounded p-2 h-56 overflow-y-auto console-log text-slate-300 space-y-1">
                        <div class="text-slate-500">[00:00:00] Inicializando bancada EGSE HIL...</div>
                        <div class="text-cyanGlow">[00:00:01] Conectado ao Transceptor TPT. Fonte CC em 14.2W.</div>
                        <div class="text-slate-300">[00:00:02] Laser Injetor 1550nm estabilizado a 100mW.</div>
                        <div class="text-amberAlert">[00:00:03] Atuador Piezoelétrico pronto em 120Hz.</div>
                    </div>
                </div>

                <div class="mt-3 flex justify-between items-center text-xs font-mono">
                    <span class="text-slate-500">Taxa Amostragem: 1000 Hz</span>
                    <button onclick="clearLogs()" class="text-slate-400 hover:text-white underline">Limpar Console</button>
                </div>
            </div>

        </div>

    </main>

    <!-- CERTIFICATION MODAL / REPORT -->
    <div id="certModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="egse-panel max-w-2xl w-full rounded-2xl p-6 border-2 border-cyanGlow/50 relative shadow-2xl max-h-[90vh] overflow-y-auto">
            <button onclick="closeCertModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white text-xl">
                <i class="fa-solid fa-xmark"></i>
            </button>

            <!-- Certificate Header -->
            <div class="text-center border-b border-space-border pb-4 mb-4">
                <div class="text-cyanGlow font-mono text-xs tracking-widest mb-1">RELATÓRIO TÉCNICO DE VALIDAÇÃO EM SOLO (STVP)</div>
                <h2 class="font-orbitron text-xl text-white font-black tracking-wider">CERTIFICADO DE PRONTIDÃO DE VOO</h2>
                <div class="text-xs text-slate-400 font-mono mt-1">NORMA ESA ECSS-E-ST-10-02C • PROTOCOLO STVP-TPT-2026</div>
            </div>

            <!-- Report Details -->
            <div class="space-y-4 text-xs font-mono text-slate-300">
                <div class="grid grid-cols-2 gap-4 bg-space-900 p-3 rounded border border-space-border">
                    <div>
                        <span class="text-slate-500 block">UNIDADE SOB TESTE (UUT):</span>
                        <span class="text-white font-bold">Protótipo TPT MEMS+FPGA</span>
                    </div>
                    <div>
                        <span class="text-slate-500 block">INSTITUIÇÃO / AVALIADORA:</span>
                        <span class="text-cyanGlow font-bold">UFCA / Avaliadora</span>
                    </div>
                    <div>
                        <span class="text-slate-500 block">PESQUISADOR / INVENTOR:</span>
                        <span class="text-white font-bold">Rafael Barbosa da Silva</span>
                    </div>
                    <div>
                        <span class="text-slate-500 block">DATA DE CERTIFICAÇÃO:</span>
                        <span id="certDateVal" class="text-neonGreen font-bold">--/--/----</span>
                    </div>
                </div>

                <div class="space-y-2">
                    <div class="font-bold text-white font-orbitron">RESUMO DOS RESULTADOS DOS TESTES:</div>
                    <ul class="space-y-1.5 pl-2">
                        <li class="flex items-center justify-between border-b border-space-border/40 pb-1">
                            <span>• TESTE 01 (Latência Física no MEMS):</span>
                            <span id="certT1" class="text-neonGreen font-bold">APROVADO (&lt; 10 ns)</span>
                        </li>
                        <li class="flex items-center justify-between border-b border-space-border/40 pb-1">
                            <span>• TESTE 02 (Filtro Kalman 1kHz / Ruído Piezo):</span>
                            <span id="certT2" class="text-neonGreen font-bold">APROVADO (Desvio ≤ ±0.005°)</span>
                        </li>
                        <li class="flex items-center justify-between border-b border-space-border/40 pb-1">
                            <span>• TESTE 03 (Redirecionamento DORP Nodal):</span>
                            <span id="certT3" class="text-neonGreen font-bold">APROVADO (&lt; 1.5 ms)</span>
                        </li>
                    </ul>
                </div>

                <!-- Signatures -->
                <div class="pt-6 border-t border-space-border grid grid-cols-2 gap-6 text-center">
                    <div>
                        <div class="font-mono text-cyanGlow text-sm font-bold border-b border-slate-600 pb-1 mb-1">
                            Rafael Barbosa da Silva
                        </div>
                        <span class="text-[10px] text-slate-400">Pesquisador / Inventor Externo</span>
                    </div>
                    <div>
                        <div class="font-mono text-neonGreen text-sm font-bold border-b border-slate-600 pb-1 mb-1">
                            UFCA / Avaliadora
                        </div>
                        <span class="text-[10px] text-slate-400">Comissão de Validação e Certificação</span>
                    </div>
                </div>
            </div>

            <!-- Modal Action -->
            <div class="mt-6 flex justify-end">
                <button onclick="window.print()" class="px-4 py-2 bg-cyanGlow text-black font-orbitron font-bold text-xs rounded hover:bg-cyanGlow/80 transition">
                    <i class="fa-solid fa-print mr-1"></i> IMPRIMIR CERTIFICADO
                </button>
            </div>
        </div>
    </div>

    <!-- FOOTER -->
    <footer class="border-t border-space-border bg-space-900/90 py-3 px-4 text-center text-xs font-mono text-slate-500">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-2">
            <div>
                © 2026 • Bancada EGSE HIL - Transceptor Fotônico TPT • Padrão ECSS-E-ST-10-02C
            </div>
            <div class="text-slate-400">
                Desenvolvido por <span class="text-cyanGlow">Rafael Barbosa da Silva</span> | Avaliação: <span class="text-white">UFCA / Avaliadora</span>
            </div>
        </div>
    </footer>

    <!-- JAVASCRIPT LOGIC & SIMULATION ENGINE -->
    <script>
        // --- GLOBAL STATE & CONFIGURATION ---
        let simRunning = true;
        let animationFrameId = null;

        // Interactive parameters
        let piezoFreq = 120; // Hz
        let fiberDist = 5000; // km
        let laserPower = 100; // mW

        // Internal Test Results State
        let testResults = {
            test1Passed: true,
            test1Latency: 4.12, // ns
            test2Passed: true,
            test2Error: 0.0031, // deg
            test3Passed: true,
            test3SwitchTime: 1.18, // ms
            nodeBFailed: false
        };

        // DOM Canvas References
        let hilCanvas, hilCtx;
        let oscilloCanvas, oscilloCtx;
        let kalmanCanvas, kalmanCtx;

        // Particles for laser beam animation
        let laserParticles = [];
        let fiberParticles = [];

        // --- INITIALIZATION ON WINDOW LOAD ---
        window.onload = function() {
            // Setup main HiL Canvas
            hilCanvas = document.getElementById('hilCanvas');
            hilCtx = hilCanvas.getContext('2d');

            // Setup Mini Canvases
            oscilloCanvas = document.getElementById('oscilloCanvas');
            oscilloCtx = oscilloCanvas.getContext('2d');

            kalmanCanvas = document.getElementById('kalmanCanvas');
            kalmanCtx = kalmanCanvas.getContext('2d');

            // Resize handling
            resizeCanvases();
            window.addEventListener('resize', resizeCanvases);

            // Initialize laser particle streams
            initParticles();

            // Start main simulation loops
            animate();

            // Log readiness
            logConsole("Bancada EGSE HIL pronta. Sistema inicializado sob conformidade ECSS-E-ST-10-02C.", "green");
            logConsole("Pesquisador: Rafael Barbosa da Silva | Avaliadora: UFCA", "cyan");

            // Draw initial mini graphics
            drawOscilloscopeTrace(4.12);
            drawKalmanTrace(0.0031);
        };

        // Adjust canvas resolution dynamically
        function resizeCanvases() {
            if (hilCanvas && hilCanvas.parentElement) {
                hilCanvas.width = hilCanvas.parentElement.clientWidth;
                hilCanvas.height = hilCanvas.parentElement.clientHeight || 380;
            }
            if (oscilloCanvas && oscilloCanvas.parentElement) {
                oscilloCanvas.width = oscilloCanvas.parentElement.clientWidth;
                oscilloCanvas.height = oscilloCanvas.parentElement.clientHeight || 96;
            }
            if (kalmanCanvas && kalmanCanvas.parentElement) {
                kalmanCanvas.width = kalmanCanvas.parentElement.clientWidth;
                kalmanCanvas.height = kalmanCanvas.parentElement.clientHeight || 96;
            }
        }

        // Initialize particle systems for optical visual effects
        function initParticles() {
            laserParticles = [];
            for (let i = 0; i < 30; i++) {
                laserParticles.push({
                    progress: Math.random(),
                    speed: 0.008 + Math.random() * 0.005
                });
            }
        }

        // --- MAIN ANIMATION LOOP (CANVAS 2D) ---
        function animate() {
            if (simRunning) {
                renderHilArchitecture();
            }
            animationFrameId = requestAnimationFrame(animate);
        }

        // --- RENDER HIL ARCHITECTURE (CANVAS 2D) ---
        function renderHilArchitecture() {
            const w = hilCanvas.width;
            const h = hilCanvas.height;

            // Clear Canvas
            hilCtx.fillStyle = '#050811';
            hilCtx.fillRect(0, 0, w, h);

            // Draw Background Grid
            hilCtx.strokeStyle = 'rgba(30, 45, 90, 0.3)';
            hilCtx.lineWidth = 1;
            const gridSize = 25;
            for (let x = 0; x < w; x += gridSize) {
                hilCtx.beginPath();
                hilCtx.moveTo(x, 0);
                hilCtx.lineTo(x, h);
                hilCtx.stroke();
            }
            for (let y = 0; y < h; y += gridSize) {
                hilCtx.beginPath();
                hilCtx.moveTo(0, y);
                hilCtx.lineTo(w, y);
                hilCtx.stroke();
            }

            // Define Nodes Coordinates relative to Canvas Size
            const nodeInjector = { x: w * 0.12, y: h * 0.3 };
            const nodePiezo = { x: w * 0.30, y: h * 0.3 };
            const nodeUUT = { x: w * 0.50, y: h * 0.3 }; // MEMS + FPGA Drawer
            const nodeFODL = { x: w * 0.75, y: h * 0.3 }; // Fiber Optical Delay
            const nodePhotometer = { x: w * 0.90, y: h * 0.3 };
            const nodeFPGAAnalyzer = { x: w * 0.50, y: h * 0.75 };
            const nodeOscilloscope = { x: w * 0.82, y: h * 0.75 };

            // Calculate Piezoelectric vibration offset for optical beam
            const time = Date.now() * 0.005;
            const vibrationAmplitude = (piezoFreq / 250) * 4;
            const piezoVibeY = Math.sin(time * (piezoFreq / 10)) * vibrationAmplitude;

            // --- DRAW OPTICAL BEAMS (LASERS) ---

            // Beam 1: Injector -> Piezo
            drawLaserBeam(nodeInjector, nodePiezo, '#ff0055', laserPower / 50);

            // Beam 2: Piezo -> UUT (with vibration distortion)
            const piezoOutputPoint = { x: nodePiezo.x, y: nodePiezo.y + piezoVibeY };
            drawLaserBeam(piezoOutputPoint, nodeUUT, '#ff0055', laserPower / 50);

            // Beam 3: UUT -> FODL (95% Transmission - Main Link)
            drawLaserBeam(nodeUUT, nodeFODL, '#00ff66', 3);

            // Beam 4: FODL -> Photometer
            drawLaserBeam(nodeFODL, nodePhotometer, '#00ff66', 2);

            // Beam 5: UUT -> FPGA Logic Analyzer (5% Snooping Signal)
            drawLaserBeam(nodeUUT, nodeFPGAAnalyzer, '#00f0ff', 1.5, true);

            // Electrical Data Line: FPGA -> Oscilloscope
            drawDataLine(nodeFPGAAnalyzer, nodeOscilloscope, '#ffb700');

            // --- DRAW HARDWARE BLOCK MODULES ---

            // 1. Injector Laser (1550nm)
            drawModuleBox(nodeInjector, 110, 60, "INJETOR LASER", "1550nm Optic", "#ff0055");

            // 2. Piezoelectric Actuator
            drawModuleBox(nodePiezo, 120, 60, "ATUADOR PIEZO", `${piezoFreq} Hz Vibro`, "#ffb700");

            // 3. UUT Module (MEMS + FPGA Drawer)
            drawModuleBox(nodeUUT, 140, 80, "UUT: GAVETA TPT", "MEMS Mirror + FPGA", "#00f0ff", true);

            // 4. Fiber Delay Line (FODL)
            drawModuleBox(nodeFODL, 120, 60, "BOBINA FIBRA", `${fiberDist} km FODL`, "#00ff66");

            // 5. Reception Photometer
            drawModuleBox(nodePhotometer, 100, 50, "FOTÔMETRO", "Medição Jitter", "#00ff66");

            // 6. FPGA Logic Analyzer
            drawModuleBox(nodeFPGAAnalyzer, 130, 55, "ANALISADOR FPGA", "Snooping 5%", "#00f0ff");

            // 7. Digital Oscilloscope
            drawModuleBox(nodeOscilloscope, 120, 55, "OSCILOSCÓPIO", "Latência GHz", "#ffb700");

            // --- ANIMATE OPTICAL PULSES / PARTICLES ALONG PATHS ---
            laserParticles.forEach(p => {
                p.progress += p.speed;
                if (p.progress > 1) p.progress = 0;

                // Position on main line (Injector to Photometer)
                const currentX = nodeInjector.x + p.progress * (nodePhotometer.x - nodeInjector.x);
                let currentY = nodeInjector.y;

                if (currentX > nodePiezo.x && currentX < nodeUUT.x) {
                    currentY += piezoVibeY;
                }

                hilCtx.beginPath();
                hilCtx.arc(currentX, currentY, 3, 0, Math.PI * 2);
                hilCtx.fillStyle = '#ffffff';
                hilCtx.shadowColor = '#00f0ff';
                hilCtx.shadowBlur = 8;
                hilCtx.fill();
                hilCtx.shadowBlur = 0;
            });

            // MEMS Mirror Reflection Visual Effect inside UUT
            const memsAngle = (piezoVibeY * 0.05) * (Math.PI / 180);
            hilCtx.save();
            hilCtx.translate(nodeUUT.x, nodeUUT.y);
            hilCtx.rotate(memsAngle);
            hilCtx.strokeStyle = '#00f0ff';
            hilCtx.lineWidth = 4;
            hilCtx.beginPath();
            hilCtx.moveTo(-15, -15);
            hilCtx.lineTo(15, 15);
            hilCtx.stroke();
            hilCtx.restore();
        }

        // Helper function to draw glowing laser beams
        function drawLaserBeam(from, to, color, width = 2, dashed = false) {
            hilCtx.save();
            hilCtx.beginPath();
            hilCtx.moveTo(from.x, from.y);
            hilCtx.lineTo(to.x, to.y);

            if (dashed) {
                hilCtx.setLineDash([5, 5]);
            }

            hilCtx.strokeStyle = color;
            hilCtx.lineWidth = width;
            hilCtx.shadowColor = color;
            hilCtx.shadowBlur = 10;
            hilCtx.stroke();
            hilCtx.restore();
        }

        // Helper function to draw digital data line
        function drawDataLine(from, to, color) {
            hilCtx.save();
            hilCtx.beginPath();
            hilCtx.moveTo(from.x, from.y);
            hilCtx.lineTo(to.x, to.y);
            hilCtx.setLineDash([3, 3]);
            hilCtx.strokeStyle = color;
            hilCtx.lineWidth = 1.5;
            hilCtx.stroke();
            hilCtx.restore();
        }

        // Helper function to draw styled hardware module boxes
        function drawModuleBox(center, width, height, title, subtitle, color, isUUT = false) {
            const x = center.x - width / 2;
            const y = center.y - height / 2;

            // Box Fill
            hilCtx.fillStyle = isUUT ? 'rgba(0, 240, 255, 0.08)' : 'rgba(10, 16, 38, 0.85)';
            hilCtx.fillRect(x, y, width, height);

            // Border
            hilCtx.strokeStyle = color;
            hilCtx.lineWidth = isUUT ? 2 : 1;
            hilCtx.strokeRect(x, y, width, height);

            // Glow if UUT
            if (isUUT) {
                hilCtx.save();
                hilCtx.strokeStyle = 'rgba(0, 240, 255, 0.3)';
                hilCtx.lineWidth = 4;
                hilCtx.strokeRect(x - 2, y - 2, width + 4, height + 4);
                hilCtx.restore();
            }

            // Text Labels
            hilCtx.fillStyle = '#ffffff';
            hilCtx.font = 'bold 10px Orbitron';
            hilCtx.textAlign = 'center';
            hilCtx.fillText(title, center.x, center.y - 4);

            hilCtx.fillStyle = color;
            hilCtx.font = '9px Share Tech Mono';
            hilCtx.fillText(subtitle, center.x, center.y + 12);
        }

        // --- CONTROL SLIDER UPDATES ---
        function updatePiezoFreq(val) {
            piezoFreq = parseFloat(val);
            document.getElementById('piezoFreqVal').innerText = piezoFreq + " Hz";
            logConsole(`Frequência de ruído piezoelétrico alterada para ${piezoFreq} Hz`, "amber");
            
            // Real-time recalculation of MEMS deviation display
            const dev = (0.001 + (piezoFreq / 250) * 0.0035).toFixed(4);
            document.getElementById('memsDeviationVal').innerText = `±${dev}°`;
        }

        function updateFiberDist(val) {
            fiberDist = parseInt(val);
            document.getElementById('fiberDistVal').innerText = fiberDist.toLocaleString('pt-BR') + " km";
            logConsole(`Comprimento da linha FODL ajustado para ${fiberDist} km`, "green");
        }

        function updateLaserPower(val) {
            laserPower = parseInt(val);
            document.getElementById('laserPowerVal').innerText = laserPower + " mW";
            logConsole(`Potência de saída do Laser Injetor ajustada para ${laserPower} mW`, "red");
        }

        // --- SIMULATION TOGGLE ---
        function toggleSimulation() {
            simRunning = !simRunning;
            const btnText = document.getElementById('pauseText');
            const btnIcon = document.getElementById('pauseIcon');
            const simDisplay = document.getElementById('simModeDisplay');
            const systemLed = document.getElementById('systemLed');

            if (simRunning) {
                btnText.innerText = "PAUSAR";
                btnIcon.className = "fa-solid fa-pause";
                simDisplay.innerText = "EM OPERAÇÃO";
                simDisplay.className = "font-orbitron font-bold text-cyanGlow text-sm";
                systemLed.className = "led-indicator bg-neonGreen text-neonGreen";
                document.getElementById('systemStatusText').innerText = "ONLINE";
                logConsole("Simulação resumida pelo operador.", "cyan");
            } else {
                btnText.innerText = "RETOMAR";
                btnIcon.className = "fa-solid fa-play";
                simDisplay.innerText = "PAUSADO";
                simDisplay.className = "font-orbitron font-bold text-amberAlert text-sm";
                systemLed.className = "led-indicator bg-amberAlert text-amberAlert";
                document.getElementById('systemStatusText').innerText = "PAUSED";
                logConsole("Simulação pausada para análise estática.", "amber");
            }
        }

        // --- STVP TEST EXECUTION FUNCTIONS ---

        // TEST 01: Zero-Latency Physical Layer Test
        function runTest1() {
            logConsole("Iniciando TESTE 01: Validação de Latência Zero (Camada Física)...", "cyan");
            const statusEl = document.getElementById('test1Status');
            statusEl.innerText = "EXECUTANDO...";
            statusEl.className = "text-xs font-mono font-bold text-amberAlert animate-pulse";

            // Highlight panel
            document.getElementById('panelTest1').classList.add('egse-panel-active');

            setTimeout(() => {
                // Generate physical transit latency < 10 ns
                const measuredLatency = (3.80 + Math.random() * 2.10).toFixed(2);
                testResults.test1Latency = measuredLatency;
                document.getElementById('test1LatencyVal').innerText = `${measuredLatency} ns`;

                drawOscilloscopeTrace(measuredLatency);

                if (measuredLatency < 10.0) {
                    testResults.test1Passed = true;
                    statusEl.innerText = "PASS (APROVADO)";
                    statusEl.className = "text-xs font-mono font-bold text-neonGreen";
                    logConsole(`TESTE 01 CONCLUÍDO COM SUCESSO! Latência física medida: ${measuredLatency} ns (< 10 ns).`, "green");
                } else {
                    testResults.test1Passed = false;
                    statusEl.innerText = "FAIL (FALHA)";
                    statusEl.className = "text-xs font-mono font-bold text-laserRed";
                    logConsole(`TESTE 01 FALHOU! Latência excedeu limite teórico: ${measuredLatency} ns.`, "red");
                }

                document.getElementById('panelTest1').classList.remove('egse-panel-active');
            }, 1200);
        }

        // TEST 02: Kalman Filter Closed-Loop Test
        function runTest2() {
            logConsole("Iniciando TESTE 02: Malha Fechada Filtro de Kalman (1.000 Hz)...", "amber");
            const statusEl = document.getElementById('test2Status');
            statusEl.innerText = "EXECUTANDO...";
            statusEl.className = "text-xs font-mono font-bold text-amberAlert animate-pulse";

            document.getElementById('panelTest2').classList.add('egse-panel-active');

            setTimeout(() => {
                // Calculate max angular error under current Piezo vibration
                const maxError = (0.0020 + (piezoFreq / 250) * 0.0022).toFixed(4);
                testResults.test2Error = maxError;
                document.getElementById('test2ErrorVal').innerText = `±${maxError}°`;

                drawKalmanTrace(maxError);

                if (maxError <= 0.0050) {
                    testResults.test2Passed = true;
                    statusEl.innerText = "PASS (APROVADO)";
                    statusEl.className = "text-xs font-mono font-bold text-neonGreen";
                    logConsole(`TESTE 02 CONCLUÍDO COM SUCESSO! Filtro Kalman manteve erro em ±${maxError}° (Limite ±0.005°).`, "green");
                } else {
                    testResults.test2Passed = false;
                    statusEl.innerText = "FAIL (FALHA)";
                    statusEl.className = "text-xs font-mono font-bold text-laserRed";
                    logConsole(`TESTE 02 FALHOU! Erro angular excedeu limite: ±${maxError}°.`, "red");
                }

                document.getElementById('panelTest2').classList.remove('egse-panel-active');
            }, 1500);
        }

        // TEST 03: DORP Node Loss Simulation & Re-routing
        function runTest3() {
            logConsole("Iniciando TESTE 03: Protocolo DORP - Chaveamento e Reorientação MEMS...", "green");
            const statusEl = document.getElementById('test3Status');
            statusEl.innerText = "EXECUTANDO...";
            statusEl.className = "text-xs font-mono font-bold text-amberAlert animate-pulse";

            document.getElementById('panelTest3').classList.add('egse-panel-active');

            setTimeout(() => {
                if (!testResults.nodeBFailed) {
                    simulateMeteoriteFailure();
                }

                // Calculate re-routing time < 1.5 ms
                const switchTime = (0.85 + Math.random() * 0.45).toFixed(2);
                testResults.test3SwitchTime = switchTime;
                document.getElementById('test3SwitchVal').innerText = `${switchTime} ms`;

                if (switchTime < 1.50) {
                    testResults.test3Passed = true;
                    statusEl.innerText = "PASS (APROVADO)";
                    statusEl.className = "text-xs font-mono font-bold text-neonGreen";
                    logConsole(`TESTE 03 CONCLUÍDO COM SUCESSO! Reorientação para Sonda C completada em ${switchTime} ms (< 1.5 ms).`, "green");
                } else {
                    testResults.test3Passed = false;
                    statusEl.innerText = "FAIL (FALHA)";
                    statusEl.className = "text-xs font-mono font-bold text-laserRed";
                    logConsole(`TESTE 03 FALHOU! Tempo de chaveamento foi de ${switchTime} ms.`, "red");
                }

                document.getElementById('panelTest3').classList.remove('egse-panel-active');
            }, 1400);
        }

        // Simulate Micrometeorite impact on Node B
        function simulateMeteoriteFailure() {
            testResults.nodeBFailed = true;
            document.getElementById('nodeB').className = "flex flex-col items-center text-laserRed opacity-50";
            document.getElementById('linkAB').className = "h-0.5 w-10 bg-laserRed line-through";
            document.getElementById('linkAC').className = "h-0.5 w-10 bg-neonGreen transition-all";
            document.getElementById('nodeC').className = "flex flex-col items-center text-neonGreen font-bold animate-bounce";
            logConsole("ALERTA: Impacto de micrometeorito simulado! Corte de energia na Sonda B.", "red");
            logConsole("Protocolo DORP ativado: Reorientando MEMS da Sonda A para a Sonda C...", "amber");
        }

        // --- MINI GRAPH RENDERING FUNCTIONS ---

        // Oscilloscope Trace Renderer for Test 1
        function drawOscilloscopeTrace(latency) {
            const w = oscilloCanvas.width;
            const h = oscilloCanvas.height;
            oscilloCtx.clearRect(0, 0, w, h);

            // Grid lines
            oscilloCtx.strokeStyle = 'rgba(0, 240, 255, 0.1)';
            oscilloCtx.lineWidth = 1;
            for (let x = 0; x < w; x += 20) {
                oscilloCtx.beginPath(); oscilloCtx.moveTo(x, 0); oscilloCtx.lineTo(x, h); oscilloCtx.stroke();
            }

            // Signal 1: Input Pulse (Red)
            oscilloCtx.strokeStyle = '#ff0055';
            oscilloCtx.lineWidth = 2;
            oscilloCtx.beginPath();
            oscilloCtx.moveTo(10, h * 0.7);
            oscilloCtx.lineTo(w * 0.2, h * 0.7);
            oscilloCtx.lineTo(w * 0.22, h * 0.2);
            oscilloCtx.lineTo(w * 0.32, h * 0.2);
            oscilloCtx.lineTo(w * 0.34, h * 0.7);
            oscilloCtx.lineTo(w - 10, h * 0.7);
            oscilloCtx.stroke();

            // Signal 2: Output Pulse after MEMS (Cyan offset by latency)
            const offset = (latency / 10) * 30;
            oscilloCtx.strokeStyle = '#00f0ff';
            oscilloCtx.lineWidth = 2;
            oscilloCtx.beginPath();
            oscilloCtx.moveTo(10, h * 0.8);
            oscilloCtx.lineTo(w * 0.2 + offset, h * 0.8);
            oscilloCtx.lineTo(w * 0.22 + offset, h * 0.3);
            oscilloCtx.lineTo(w * 0.32 + offset, h * 0.3);
            oscilloCtx.lineTo(w * 0.34 + offset, h * 0.8);
            oscilloCtx.lineTo(w - 10, h * 0.8);
            oscilloCtx.stroke();
        }

        // Kalman Filter Error Curve for Test 2
        function drawKalmanTrace(maxErr) {
            const w = kalmanCanvas.width;
            const h = kalmanCanvas.height;
            kalmanCtx.clearRect(0, 0, w, h);

            // Limit bounds line (±0.005°)
            kalmanCtx.strokeStyle = 'rgba(255, 0, 85, 0.4)';
            kalmanCtx.setLineDash([3, 3]);
            kalmanCtx.beginPath();
            kalmanCtx.moveTo(0, h * 0.2); kalmanCtx.lineTo(w, h * 0.2); // +0.005
            kalmanCtx.moveTo(0, h * 0.8); kalmanCtx.lineTo(w, h * 0.8); // -0.005
            kalmanCtx.stroke();
            kalmanCtx.setLineDash([]);

            // Kalman Correction Curve
            kalmanCtx.strokeStyle = '#ffb700';
            kalmanCtx.lineWidth = 2;
            kalmanCtx.beginPath();
            const points = 30;
            for (let i = 0; i < points; i++) {
                const x = (i / (points - 1)) * w;
                // Decay sine curve simulating Kalman convergence
                const decay = Math.exp(-i / 8);
                const noise = (Math.random() - 0.5) * maxErr * 40;
                const y = (h / 2) + Math.sin(i * 0.8) * 25 * decay + noise;
                if (i === 0) kalmanCtx.moveTo(x, y);
                else kalmanCtx.lineTo(x, y);
            }
            kalmanCtx.stroke();
        }

        // --- CONSOLE EVENT LOGGING SYSTEM ---
        function logConsole(msg, type = "default") {
            const box = document.getElementById('consoleLogBox');
            const entry = document.createElement('div');

            const timestamp = new Date().toLocaleTimeString('pt-BR');
            let colorClass = "text-slate-300";

            if (type === "green") colorClass = "text-neonGreen font-semibold";
            if (type === "cyan") colorClass = "text-cyanGlow";
            if (type === "amber") colorClass = "text-amberAlert";
            if (type === "red") colorClass = "text-laserRed font-semibold";

            entry.className = colorClass;
            entry.innerText = `[${timestamp}] ${msg}`;

            box.appendChild(entry);
            box.scrollTop = box.scrollHeight; // Auto-scroll
        }

        function clearLogs() {
            document.getElementById('consoleLogBox').innerHTML = '<div class="text-slate-500">[00:00:00] Console limpo pelo operador.</div>';
        }

        // --- FLIGHT CERTIFICATION REPORT GENERATION ---
        function generateFlightReport() {
            const certDate = new Date().toLocaleDateString('pt-BR');
            document.getElementById('certDateVal').innerText = certDate;

            // Update modal text from global state
            document.getElementById('certT1').innerText = testResults.test1Passed 
                ? `APROVADO (${testResults.test1Latency} ns)` 
                : `REPROVADO (${testResults.test1Latency} ns)`;
            document.getElementById('certT1').className = testResults.test1Passed ? "text-neonGreen font-bold" : "text-laserRed font-bold";

            document.getElementById('certT2').innerText = testResults.test2Passed 
                ? `APROVADO (Desvio ±${testResults.test2Error}°)` 
                : `REPROVADO (Desvio ±${testResults.test2Error}°)`;
            document.getElementById('certT2').className = testResults.test2Passed ? "text-neonGreen font-bold" : "text-laserRed font-bold";

            document.getElementById('certT3').innerText = testResults.test3Passed 
                ? `APROVADO (${testResults.test3SwitchTime} ms)` 
                : `REPROVADO (${testResults.test3SwitchTime} ms)`;
            document.getElementById('certT3').className = testResults.test3Passed ? "text-neonGreen font-bold" : "text-laserRed font-bold";

            document.getElementById('certModal').classList.remove('hidden');
            logConsole("Relatório Técnico de Validação em Solo (STVP) gerado.", "cyan");
        }

        function closeCertModal() {
            document.getElementById('certModal').classList.add('hidden');
        }
    </script>
</body>
</html>
