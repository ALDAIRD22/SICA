<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SICA - ALDAIR RIVERA</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Chart.js DataLabels Plugin -->
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.0.0"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: radial-gradient(circle at top right, #111827 0%, #0f172a 60%, #020617 100%);
        }
        .premium-card {
            background: rgba(15, 23, 42, 0.65);
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(20px);
        }
        .custom-scroll::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scroll::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.5);
            border-radius: 8px;
        }
        .custom-scroll::-webkit-scrollbar-thumb {
            background: rgba(14, 165, 233, 0.4);
            border-radius: 8px;
        }
        .custom-scroll::-webkit-scrollbar-thumb:hover {
            background: rgba(14, 165, 233, 0.7);
        }
        .view-transition {
            transition: opacity 250ms ease-in-out, transform 250ms ease-in-out;
        }
    </style>
</head>
<body class="text-slate-200 min-h-screen antialiased custom-scroll flex flex-col">

    <!-- SPLASH SCREEN CON FIRMA ALDAIR RIVERA Y PROMEDIOS GENERALES GIGANTES -->
    <div id="welcome-overlay" class="fixed inset-0 z-[100] bg-slate-950 flex flex-col items-center justify-center transition-opacity duration-1000 opacity-100 p-6">
        <div class="text-center space-y-6 max-w-2xl w-full">
            <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl bg-gradient-to-tr from-blue-600 to-cyan-500 shadow-[0_0_40px_rgba(14,165,233,0.3)]">
                <span class="text-3xl text-white font-black" translate="no">AR</span>
            </div>
            <h2 class="text-xl font-bold text-slate-400 tracking-widest uppercase">ALDAIR RIVERA • REPORTES EN VIVO</h2>
            
            <!-- Bloque de visualización masiva de promedios -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-6 my-8">
                <div class="bg-slate-900/80 border border-blue-500/20 rounded-2xl p-6 shadow-2xl backdrop-blur-xl">
                    <p class="text-xs font-bold text-blue-400 uppercase tracking-wider mb-2">PROMEDIO GENERAL EXSA</p>
                    <h3 class="text-5xl font-black text-white font-mono" id="splash-avg-exsa">...</h3>
                </div>
                <div class="bg-slate-900/80 border border-cyan-500/20 rounded-2xl p-6 shadow-2xl backdrop-blur-xl">
                    <p class="text-xs font-bold text-cyan-400 uppercase tracking-wider mb-2">PROMEDIO GENERAL EXSI</p>
                    <h3 class="text-5xl font-black text-white font-mono" id="splash-avg-exsi">...</h3>
                </div>
            </div>
            
            <p class="text-slate-500 font-semibold tracking-wider text-xs uppercase animate-pulse">Sincronizando base de datos central de la sede...</p>
        </div>
    </div>

    <!-- Encabezado Principal -->
    <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 h-20 flex items-center justify-between shadow-lg">
            <div class="flex items-center space-x-4">
                <div class="bg-blue-600 px-4 py-2 rounded-xl text-white font-extrabold text-lg tracking-wider" translate="no">COMAS</div>
                <div>
                    <h1 class="text-sm font-bold text-white tracking-tight uppercase">ANÁLISIS DE NOTAS • <span class="text-cyan-400">ALDAIR RIVERA</span></h1>
                    <p class="text-[11px] text-slate-400 font-semibold flex items-center gap-1.5">
                        <span class="flex h-2 w-2 relative">
                            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-cyan-400 opacity-75"></span>
                            <span class="relative inline-flex rounded-full h-2 w-2 bg-cyan-500"></span>
                        </span>
                        Consolidado Absoluto desde Examen 1
                    </p>
                </div>
            </div>

            <!-- Selector Desplegable de Tutores -->
            <div class="flex items-center space-x-3">
                <label for="tutor-select" class="text-xs font-bold text-slate-400 uppercase tracking-wider hidden md:block">Tutor Responsable:</label>
                <div class="relative">
                    <select id="tutor-select" onchange="filterByTutor(this.value)" class="bg-slate-850 border border-slate-700 text-slate-200 text-xs font-bold rounded-xl px-4 py-2.5 pr-10 hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-600 transition-all appearance-none cursor-pointer shadow-md" translate="no">
                        <!-- Automatizado -->
                    </select>
                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-cyan-400">
                        <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <div id="error-box" class="hidden max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 mt-8">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-6 rounded-xl text-xs font-medium">
            ⚠️ <strong>Error de Sincronización:</strong> Verifica que la pestaña del documento se llame estrictamente 'Datos_web'.
        </div>
    </div>

    <!-- CONTENEDOR PRINCIPAL MULTI-VISTA -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-1 w-full space-y-8 relative">
        
        <!-- Pestañas SPA -->
        <div class="flex justify-center sm:justify-start border-b border-slate-800/80 pb-0.5">
            <nav class="flex space-x-8" aria-label="Tabs">
                <button onclick="switchView('dashboard')" id="tab-dashboard" class="border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200">
                    <span>📊</span> Resumen General
                </button>
                <button onclick="switchView('advanced')" id="tab-advanced" class="border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200">
                    <span>🧠</span> Analítica Avanzada y Estándares
                </button>
                <button onclick="switchView('reports')" id="tab-reports" class="border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200">
                    <span>📋</span> Reporte Detallado Completo
                </button>
            </nav>
        </div>

        <!-- Cargador interno -->
        <div id="view-loader" class="hidden absolute inset-0 bg-slate-950/40 backdrop-blur-sm z-40 flex items-center justify-center rounded-2xl transition-opacity duration-300">
            <div class="animate-spin rounded-full h-10 w-10 border-4 border-blue-500 border-t-transparent"></div>
        </div>

        <!-- ÁREA DINÁMICA -->
        <div class="relative w-full overflow-hidden min-h-[600px]">
            
            <!-- VISTA 1: DASHBOARD RESUMEN -->
            <div id="view-container-dashboard" class="view-transition opacity-100 transform translate-x-0 space-y-8 w-full">
                <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-blue-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📈 Promedio EXSA</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsa">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media académica real de la serie EXSA</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-cyan-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📉 Promedio EXSI</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsi">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media académica real de la serie EXSI</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-emerald-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">✅ Asistencias Totales</p>
                        <h3 class="text-3xl font-black text-emerald-400 mt-4 font-mono" id="kpi-asistencias">0</h3>
                        <p class="text-[11px] text-emerald-500/80 mt-2 font-medium" id="kpi-pct-asistencia">0% Ratio global</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-rose-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">❌ Inasistencias Totales</p>
                        <h3 class="text-3xl font-black text-rose-400 mt-4 font-mono" id="kpi-faltas">0</h3>
                        <p class="text-[11px] text-rose-500/80 mt-2 font-medium" id="kpi-pct-faltas">0% Ratio global</p>
                    </div>
                </section>

                <section class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <!-- Evolución académica -->
                    <div class="lg:col-span-2 space-y-6">
                        <div class="premium-card rounded-2xl p-8 shadow-xl">
                            <div class="flex items-center justify-between mb-6">
                                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400">Evolución Cronológica de Calificaciones</h3>
                                <span class="text-[10px] bg-blue-500/10 text-blue-400 font-bold px-2 py-1 rounded-md uppercase tracking-tight animate-pulse">👉 Haz clic en un punto para inspeccionar</span>
                            </div>
                            <div class="relative h-72">
                                <canvas id="chartEvolucion"></canvas>
                            </div>
                        </div>

                        <!-- PANEL DE DIAGNÓSTICO DETALLADO POR EXAMEN -->
                        <div id="interactive-diagnostic-panel" class="premium-card rounded-2xl p-6 shadow-xl hidden transition-all duration-300 transform translate-y-4 opacity-0 border border-blue-500/20 bg-slate-900/80">
                            <div class="flex items-center justify-between border-b border-slate-800 pb-4 mb-4">
                                <div class="flex items-center space-x-3">
                                    <div class="w-2.5 h-2.5 rounded-full bg-blue-500 shadow-[0_0_12px_#3b82f6]"></div>
                                    <h4 class="text-sm font-extrabold text-white uppercase tracking-wider" id="diag-title">EXAMEN SELECCIONADO</h4>
                                </div>
                                <span class="text-[10px] font-bold uppercase px-2 py-1 rounded-md" id="diag-badge-std">ESTÁNDAR</span>
                            </div>
                            <div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-xs">
                                <div class="bg-slate-950/40 p-3.5 rounded-xl border border-slate-800/60">
                                    <p class="text-slate-400 font-medium mb-1">🎯 Calificación Puntaje</p>
                                    <p class="text-lg font-black text-cyan-400 font-mono" id="diag-note">0.00</p>
                                </div>
                                <div class="bg-slate-950/40 p-3.5 rounded-xl border border-slate-800/60">
                                    <p class="text-slate-400 font-medium mb-1">📈 Desempeño SICA</p>
                                    <p class="text-lg font-black text-blue-400 font-mono" id="diag-sica">0</p>
                                </div>
                                <div class="bg-slate-950/40 p-3.5 rounded-xl border border-slate-800/60">
                                    <p class="text-slate-400 font-medium mb-1">👥 Ratio Presentes</p>
                                    <p class="text-lg font-black text-emerald-400 font-mono" id="diag-asist">0 alumnos</p>
                                </div>
                                <div class="bg-slate-950/40 p-3.5 rounded-xl border border-slate-800/60">
                                    <p class="text-slate-400 font-medium mb-1">🚨 Ausentismo Crítico</p>
                                    <p class="text-lg font-black text-rose-400 font-mono" id="diag-faltas">0 faltas</p>
                                </div>
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-[11px] mt-4 pt-3 border-t border-slate-850">
                                <div class="flex items-start space-x-2">
                                    <span class="text-slate-400 font-bold uppercase tracking-wider block mt-0.5">📚 Bloque C+D:</span>
                                    <span class="text-slate-300 font-semibold bg-slate-950/30 px-2 py-0.5 rounded border border-slate-800" id="diag-cd">-</span>
                                </div>
                                <div class="flex items-start space-x-2">
                                    <span class="text-slate-400 font-bold uppercase tracking-wider block mt-0.5">🧮 Bloque CXM:</span>
                                    <span class="text-slate-300 font-semibold bg-slate-950/30 px-2 py-0.5 rounded border border-slate-800" id="diag-cxm">-</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="lg:col-span-1 premium-card rounded-2xl p-8 shadow-xl flex flex-col justify-between">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Proporción de Asistencias Total</h3>
                        <div class="relative h-56 flex items-center justify-center">
                            <canvas id="chartAsistenciaDoughnut"></canvas>
                        </div>
                        <div class="text-center text-xs text-slate-400 mt-4" id="chart-doughnut-legend">...</div>
                    </div>
                </section>
            </div>

            <!-- VISTA 2: ANALÍTICA AVANZADA Y ESTÁNDARES -->
            <div id="view-container-advanced" class="view-transition opacity-0 transform translate-x-12 hidden absolute inset-x-0 top-0 space-y-8 w-full">
                <section class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="premium-card rounded-2xl p-6 text-center shadow-md border-t-2 border-cyan-500">
                        <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">🎯 Probabilidad de Asistencia</p>
                        <h4 class="text-4xl font-extrabold text-cyan-400 mt-3 font-mono" id="advanced-prob">0.0%</h4>
                    </div>
                    <div class="premium-card rounded-2xl p-6 text-center shadow-md border-t-2 border-blue-500">
                        <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">💎 Volumen Acumulado SICA</p>
                        <h4 class="text-4xl font-extrabold text-blue-400 mt-3 font-mono" id="advanced-sica">0</h4>
                    </div>
                    <div class="premium-card rounded-2xl p-6 text-center shadow-md border-t-2 border-purple-500">
                        <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">📈 Estándar de Competencias</p>
                        <h4 class="text-2xl font-black text-purple-400 mt-4 uppercase tracking-tight">SUBIR ↑ (Aciertos)</h4>
                    </div>
                </section>

                <!-- GRÁFICO CON BARRAS ROJAS Y VERDES PEGADAS AL COSTADO -->
                <section class="premium-card rounded-2xl p-8 shadow-xl w-full">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 mb-6">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400">Métrica Comparativa de Asistencias y Faltas por Examen (Números en Vivo)</h3>
                        <div class="text-[10px] font-bold text-slate-400 bg-slate-900 px-3 py-1 rounded-md border border-slate-800">
                            ESTÁNDAR GENERAL ASISTENCIA: <span class="text-emerald-400">SUBIR ↑</span> | FALTAS: <span class="text-rose-400">BAJAR ↓</span>
                        </div>
                    </div>
                    <div class="relative h-96">
                        <canvas id="chartAsistenciaPorExamen"></canvas>
                    </div>
                </section>

                <section class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="premium-card rounded-2xl p-6 shadow-md">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-4">Frecuencia de Cursos en Bloque C+D (Estándar de Dominio: SUBIR ↑)</h3>
                        <div class="relative h-64">
                            <canvas id="chartCursosCD"></canvas>
                        </div>
                    </div>
                    <div class="premium-card rounded-2xl p-6 shadow-md">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-4">Frecuencia de Cursos en Bloque CXM (Estándar de Dominio: SUBIR ↑)</h3>
                        <div class="relative h-64">
                            <canvas id="chartCursosCXM"></canvas>
                        </div>
                    </div>
                </section>
            </div>

            <!-- VISTA 3: REPORTE DETALLADO COMPLETO (OPTIMIZADO SIN BARRA DE DESPLAZAMIENTO) -->
            <div id="view-container-reports" class="view-transition opacity-0 transform translate-x-12 hidden absolute inset-x-0 top-0 w-full">
                <section class="rounded-2xl overflow-hidden shadow-2xl bg-black border border-slate-800/80">
                    <div class="p-5 border-b border-slate-850 bg-black flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                        <div>
                            <h3 class="text-sm font-bold text-white tracking-tight">Reporte Detallado Completo</h3>
                            <p class="text-[11px] text-slate-400 mt-1">Ajuste de pantalla fluido en vivo sin barras de scroll lateral</p>
                        </div>
                        <div class="flex items-center space-x-2">
                            <button onclick="filterTable('TODOS')" id="btn-f-todos" class="px-3.5 py-1.5 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all">Todos</button>
                            <button onclick="filterTable('EXSA')" id="btn-f-exsa" class="px-3.5 py-1.5 rounded-lg text-xs font-bold bg-slate-900 text-slate-400 hover:bg-slate-800 transition-all">EXSA</button>
                            <button onclick="filterTable('EXSI')" id="btn-f-exsi" class="px-3.5 py-1.5 rounded-lg text-xs font-bold bg-slate-900 text-slate-400 hover:bg-slate-800 transition-all">EXSI</button>
                        </div>
                    </div>
                    <div class="w-full text-[11px] bg-black">
                        <table class="w-full text-left border-collapse table-auto bg-black">
                            <thead>
                                <tr class="bg-black text-slate-400 font-bold uppercase tracking-wider border-b border-slate-800 text-[10px]">
                                    <th class="py-3 px-3 bg-black">Evaluación</th>
                                    <th class="py-3 px-2 text-center text-emerald-400 bg-emerald-950/30">Asist. (A)</th>
                                    <th class="py-3 px-2 text-center text-rose-400 bg-rose-950/30">Faltas (F)</th>
                                    <th class="py-3 px-3 text-right text-cyan-400 bg-black">Nota</th>
                                    <th class="py-3 px-2 text-center bg-black">Var %</th>
                                    <th class="py-3 px-2 text-center bg-black">SICA</th>
                                    <th class="py-3 px-3 bg-black">Sección C+D</th>
                                    <th class="py-3 px-3 bg-black">Sección CXM</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800/60 bg-black font-semibold text-slate-300" id="table-body-notas">
                                <!-- Generado Dinámicamente sin scrollbar -->
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>

        </div>
    </main>

    <!-- FOOTER MARCA REGISTRADA -->
    <footer class="border-t border-slate-850 py-4 text-center text-[11px] text-slate-500 font-medium">
        Analítica de Control Desarrollada por <span class="text-slate-400 font-bold">Aldair Rivera</span> &copy; 2026
    </footer>

    <script>
        Chart.register(ChartDataLabels);

        const SPREADSHEET_ID = '1IIUvhEyo5y1t1itBDwKbSDOVgtWXyWupyyfW6XgAg6M'; 
        const GID_DATOS_WEB = '1216544172'; 
        const URL_API = `https://docs.google.com/spreadsheets/d/${SPREADSHEET_ID}/gviz/tq?tqx=out:json&gid=${GID_DATOS_WEB}`;

        let totalDatabase = []; 
        let currentFilteredRows = [];
        let chartEvolucionInstance = null;
        let chartAsistenciaInstance = null;
        let chartBarAsistenciaInstance = null;
        let chartCDInstance = null;
        let chartCXMInstance = null;
        let currentView = 'dashboard';

        function cleanValue(cell) {
            if (!cell || cell.v === null || cell.v === undefined) return "";
            return cell.f || cell.v.toString().trim();
        }

        function cleanNumericValue(cell) {
            if (!cell || cell.v === null || cell.v === undefined) return 0;
            if (typeof cell.v === 'number') return cell.v;
            let str = cell.v.toString().replace(/\s/g, '').replace(/\./g, '').replace(',', '.');
            let val = parseFloat(str);
            return isNaN(val) ? 0 : val;
        }

        function switchView(viewTarget) {
            if (currentView === viewTarget) return;
            currentView = viewTarget;

            const views = {
                dashboard: document.getElementById('view-container-dashboard'),
                advanced: document.getElementById('view-container-advanced'),
                reports: document.getElementById('view-container-reports')
            };

            const tabs = {
                dashboard: document.getElementById('tab-dashboard'),
                advanced: document.getElementById('tab-advanced'),
                reports: document.getElementById('tab-reports')
            };

            Object.keys(views).forEach(v => {
                if(v === viewTarget) {
                    tabs[v].className = "border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200";
                    views[v].classList.remove('hidden');
                    setTimeout(() => views[v].classList.remove('opacity-0', 'translate-x-12', '-translate-x-12'), 40);
                } else {
                    tabs[v].className = "border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200";
                    views[v].classList.add('opacity-0', v === 'reports' ? 'translate-x-12' : '-translate-x-12');
                    views[v].classList.add('hidden');
                }
            });
        }

        async function loadSheetDashboard() {
            try {
                const response = await fetch(URL_API);
                const text = await response.text();
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                if (startIdx === -1 || endIdx === -1) throw new Error("JSON Corrupto");

                const jsonString = text.substring(startIdx, endIdx + 1);
                const dataTable = JSON.parse(jsonString).table;

                if (!dataTable || !dataTable.rows) return;

                totalDatabase = [];
                let uniqueTutors = new Set();
                
                let globalExsaSum = 0, globalExsaCount = 0;
                let globalExsiSum = 0, globalExsiCount = 0;

                dataTable.rows.forEach((row) => {
                    if (!row || !row.c || row.c.length < 3) return;

                    let tutorRaw = cleanValue(row.c[0]);
                    let examenName = cleanValue(row.c[2]);

                    if (!tutorRaw || !examenName || tutorRaw.toUpperCase() === "TUTOR" || examenName.toUpperCase() === "EXAMEN") return;

                    let tutorName = tutorRaw.trim();
                    uniqueTutors.add(tutorName);
                    
                    let noteVal = cleanNumericValue(row.c[5]);
                    let isExsa = examenName.toUpperCase().includes("EXSA");
                    
                    if(noteVal > 0) {
                        if(isExsa) { globalExsaSum += noteVal; globalExsaCount++; }
                        else { globalExsiSum += noteVal; globalExsiCount++; }
                    }

                    totalDatabase.push({
                        tutor: tutorName,
                        name: examenName,
                        type: isExsa ? "EXSA" : "EXSI",
                        a: row.c[3] ? (row.c[3].v !== null ? parseInt(row.c[3].v) : 0) : 0, 
                        f: row.c[4] ? (row.c[4].v !== null ? parseInt(row.c[4].v) : 0) : 0, 
                        note: noteVal, 
                        noteText: cleanValue(row.c[5]) || "-",
                        variacion: cleanValue(row.c[6]) || "---", 
                        sica: cleanValue(row.c[7]) || "-", 
                        cd: cleanValue(row.c[8]) || "-", 
                        cxm: cleanValue(row.c[9]) || "-" 
                    });
                });

                let generalExsaAvg = globalExsaCount > 0 ? (globalExsaSum / globalExsaCount) : 0;
                let generalExsiAvg = globalExsiCount > 0 ? (globalExsiSum / globalExsiCount) : 0;
                
                document.getElementById('splash-avg-exsa').innerText = generalExsaAvg.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
                document.getElementById('splash-avg-exsi').innerText = generalExsiAvg.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });

                const selectElement = document.getElementById('tutor-select');
                let previousValue = selectElement.value;
                selectElement.innerHTML = '';
                
                uniqueTutors.forEach(tutor => {
                    let option = document.createElement('option');
                    option.value = tutor;
                    option.innerText = tutor;
                    selectElement.appendChild(option);
                });

                if (previousValue && uniqueTutors.has(previousValue)) { selectElement.value = previousValue; }

                filterByTutor(selectElement.value);

                setTimeout(() => {
                    const overlay = document.getElementById('welcome-overlay');
                    if (overlay) {
                        overlay.classList.add('opacity-0');
                        setTimeout(() => overlay.remove(), 1000);
                    }
                }, 1800);
                
                document.getElementById('error-box').classList.add('hidden');
            } catch (error) {
                console.error(error);
                document.getElementById('error-box').classList.remove('hidden');
                const overlay = document.getElementById('welcome-overlay');
                if (overlay) overlay.remove();
            }
        }

        function filterByTutor(selectedTutor) {
            if (!selectedTutor) return;
            
            const loader = document.getElementById('view-loader');
            loader.classList.remove('hidden');
            loader.style.opacity = '1';

            document.getElementById('interactive-diagnostic-panel').classList.add('hidden', 'opacity-0');

            currentFilteredRows = totalDatabase.filter(r => r.tutor === selectedTutor);

            let sumExsa = 0, countExsa = 0, sumExsi = 0, countExsi = 0, totalSicaSum = 0;
            let totalA = 0, totalF = 0;
            let cursosCDMap = {}, cursosCXMMap = {};

            currentFilteredRows.forEach(row => {
                totalA += row.a;
                totalF += row.f;
                let sicaParsed = parseInt(row.sica);
                if(!isNaN(sicaParsed)) totalSicaSum += sicaParsed;

                if (row.type === 'EXSA' && row.note > 0) { sumExsa += row.note; countExsa++; }
                else if (row.type === 'EXSI' && row.note > 0) { sumExsi += row.note; countExsi++; }

                if(row.cd && row.cd !== "-") {
                    row.cd.split(/[-–,]/).forEach(c => {
                        let name = c.trim().toUpperCase();
                        if(name && name !== "CD") cursosCDMap[name] = (cursosCDMap[name] || 0) + 1;
                    });
                }
                if(row.cxm && row.cxm !== "-") {
                    row.cxm.split(/[-–,]/).forEach(c => {
                        let name = c.trim().toUpperCase();
                        if(name && name !== "CXM") cursosCXMMap[name] = (cursosCXMMap[name] || 0) + 1;
                    });
                }
            });

            let avgExsa = countExsa > 0 ? (sumExsa / countExsa) : 0;
            let avgExsi = countExsi > 0 ? (sumExsi / countExsi) : 0;
            let totalAsistenciales = totalA + totalF;
            let pctA = totalAsistenciales > 0 ? ((totalA / totalAsistenciales) * 100).toFixed(1) : "0.0";

            document.getElementById('kpi-exsa').innerText = avgExsa.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-exsi').innerText = avgExsi.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-asistencias').innerText = totalA.toString();
            document.getElementById('kpi-pct-asistencia').innerText = `${pctA}% Ratio global`;
            document.getElementById('kpi-faltas').innerText = totalF.toString();
            document.getElementById('kpi-pct-faltas').innerText = `${totalAsistenciales > 0 ? ((totalF / totalAsistenciales) * 100).toFixed(1) : "0.0"}% Ratio ausentismo`;

            document.getElementById('advanced-prob').innerText = `${pctA}%`;
            document.getElementById('advanced-sica').innerText = totalSicaSum.toString();

            filterTable('TODOS');
            buildCharts(currentFilteredRows, totalA, totalF, cursosCDMap, cursosCXMMap);

            setTimeout(() => {
                loader.style.opacity = '0';
                setTimeout(() => loader.classList.add('hidden'), 200);
            }, 200);
        }

        function renderTableRows(data) {
            const tbody = document.getElementById('table-body-notas');
            tbody.innerHTML = '';

            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-900 transition-colors border-b border-slate-850/60 text-[11px] bg-black";

                let varColor = "text-slate-400";
                if (row.variacion.includes('-')) varColor = "text-rose-400";
                else if (row.variacion !== '-' && row.variacion !== '0.0%' && !row.variacion.includes('#') && row.variacion !== '---') varColor = "text-emerald-400";

                tr.innerHTML = `
                    <td class="py-3 px-3 font-bold text-white tracking-wide" translate="no">${row.name}</td>
                    <td class="py-3 px-2 text-center text-emerald-400 bg-emerald-950/25 font-mono">${row.a}</td>
                    <td class="py-3 px-2 text-center text-rose-400 bg-rose-950/25 font-mono">${row.f}</td>
                    <td class="py-3 px-3 text-right text-cyan-400 font-extrabold font-mono">${row.noteText}</td>
                    <td class="py-3 px-2 text-center font-bold font-mono ${varColor}">${row.variacion}</td>
                    <td class="py-3 px-2 text-center text-slate-300 font-mono">${row.sica}</td>
                    <td class="py-3 px-3 text-slate-400 truncate max-w-[150px] font-medium">${row.cd}</td>
                    <td class="py-3 px-3 text-slate-400 truncate max-w-[150px] font-medium">${row.cxm}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function filterTable(type) {
            ['btn-f-todos', 'btn-f-exsa', 'btn-f-exsi'].forEach(id => {
                let el = document.getElementById(id);
                if(el) el.className = "px-4 py-2 rounded-lg text-xs font-bold bg-slate-900 text-slate-400 hover:bg-slate-800 transition-all";
            });
            
            let activeBtn = document.getElementById(type === 'TODOS' ? 'btn-f-todos' : `btn-f-${type.toLowerCase()}`);
            if(activeBtn) activeBtn.className = "px-4 py-2 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all";

            if (type === 'TODOS') { filteredEvaluations = [...currentFilteredRows]; } 
            else { filteredEvaluations = currentFilteredRows.filter(r => r.type === type); }
            renderTableRows(filteredEvaluations);
        }

        function buildCharts(data, asistencias, faltas, cdMap, cxmMap) {
            const activeData = data.filter(r => r.note > 0);

            // 1. Gráfico de Notas
            const ctxEvolucion = document.getElementById('chartEvolucion').getContext('2d');
            if (chartEvolucionInstance) chartEvolucionInstance.destroy();
            chartEvolucionInstance = new Chart(ctxEvolucion, {
                type: 'line',
                data: {
                    labels: activeData.map(r => r.name),
                    datasets: [{
                        data: activeData.map(r => r.note),
                        borderColor: '#0ea5e9',
                        backgroundColor: 'rgba(14, 165, 233, 0.06)',
                        borderWidth: 3,
                        pointBackgroundColor: '#22d3ee',
                        pointHoverRadius: 8,
                        pointRadius: 4,
                        fill: true,
                        tension: 0.15
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false }, datalabels: { display: false } },
                    onClick: (event, elements) => {
                        if (elements.length > 0) {
                            const index = elements[0].index;
                            const examName = chartEvolucionInstance.data.labels[index];
                            displayExamDiagnostic(examName);
                        }
                    },
                    scales: {
                        x: { grid: { color: 'rgba(255, 255, 255, 0.02)' }, ticks: { color: '#94a3b8' } },
                        y: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });

            // 2. Gráfico de Rosca
            const ctxDoughnut = document.getElementById('chartAsistenciaDoughnut').getContext('2d');
            if (chartAsistenciaInstance) chartAsistenciaInstance.destroy();
            chartAsistenciaInstance = new Chart(ctxDoughnut, {
                type: 'doughnut',
                data: {
                    labels: ['Asistencias', 'Faltas'],
                    datasets: [{
                        data: [asistencias, faltas],
                        backgroundColor: ['#10b981', '#f43f5e'],
                        borderColor: '#0f172a',
                        borderWidth: 4
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false }, datalabels: { display: false } },
                    cutout: '76%'
                }
            });

            document.getElementById('chart-doughnut-legend').innerHTML = `
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-emerald-500 mr-1.5"></span> Asistencias (${asistencias})
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-rose-500 ml-4 mr-1.5"></span> Faltas (${faltas})
            `;

            // PERFECCIONADO: 3. Gráfico de Barras Agrupadas con barras pegadas al costado
            const ctxBarAsistencia = document.getElementById('chartAsistenciaPorExamen').getContext('2d');
            if (chartBarAsistenciaInstance) chartBarAsistenciaInstance.destroy();
            chartBarAsistenciaInstance = new Chart(ctxBarAsistencia, {
                type: 'bar',
                data: {
                    labels: data.map(r => r.name),
                    datasets: [
                        { label: 'Asistieron (A)', data: data.map(r => r.a), backgroundColor: '#10b981', borderRadius: 4 },
                        { label: 'Faltaron (F)', data: data.map(r => r.f), backgroundColor: '#f43f5e', borderRadius: 4 }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    layout: { padding: { top: 25 } }, 
                    // Configuración para pegar las barras juntas una al lado de la otra
                    barPercentage: 0.85,
                    categoryPercentage: 0.65,
                    plugins: {
                        legend: { labels: { color: '#94a3b8', font: { weight: 'bold' } } },
                        datalabels: {
                            display: true,
                            anchor: 'end',
                            align: 'top',
                            offset: 2,
                            color: '#cbd5e1',
                            font: { weight: '800', size: 10, family: 'monospace' },
                            formatter: function(val) { return val; } 
                        }
                    },
                    scales: {
                        x: { grid: { display: false }, ticks: { color: '#94a3b8' } },
                        y: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });

            renderFrecuenciaChart('chartCursosCD', chartCDInstance, Object.keys(cdMap), Object.values(cdMap), '#3b82f6');
            renderFrecuenciaChart('chartCursosCXM', chartCXMInstance, Object.keys(cxmMap), Object.values(cxmMap), '#06b6d4');
        }

        function displayExamDiagnostic(examName) {
            const examMatch = currentFilteredRows.find(r => r.name === examName);
            if (!examMatch) return;

            const panel = document.getElementById('interactive-diagnostic-panel');
            panel.classList.add('opacity-0', 'translate-y-4');
            
            setTimeout(() => {
                panel.classList.remove('hidden');
                document.getElementById('diag-title').innerText = `DIAGNÓSTICO INTERACTIVO: ${examMatch.name}`;
                document.getElementById('diag-note').innerText = examMatch.noteText;
                document.getElementById('diag-sica').innerText = examMatch.sica;
                document.getElementById('diag-asist').innerText = `${examMatch.a} Alumnos`;
                document.getElementById('diag-faltas').innerText = `${examMatch.f} Faltas`;
                document.getElementById('diag-cd').innerText = examMatch.cd;
                document.getElementById('diag-cxm').innerText = examMatch.cxm;

                const badgeStd = document.getElementById('diag-badge-std');
                if(examMatch.note >= 1100) {
                    badgeStd.innerText = "ESTÁNDAR: SUBIENDO ↑ (Excelente)";
                    badgeStd.className = "text-[10px] font-bold uppercase px-2 py-1 rounded-md bg-emerald-500/10 text-emerald-400 border border-emerald-500/20";
                } else {
                    badgeStd.innerText = "ESTÁNDAR: BAJO CONTROL";
                    badgeStd.className = "text-[10px] font-bold uppercase px-2 py-1 rounded-md bg-blue-500/10 text-blue-400 border border-blue-500/20";
                }

                panel.classList.remove('opacity-0', 'translate-y-4');
            }, 150);
        }

        function renderFrecuenciaChart(canvasId, instanceVar, labels, values, color) {
            const ctx = document.getElementById(canvasId).getContext('2d');
            if (window[canvasId + 'Instance']) window[canvasId + 'Instance'].destroy();
            
            window[canvasId + 'Instance'] = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{ data: values, backgroundColor: color, borderRadius: 4 }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y', 
                    plugins: { 
                        legend: { display: false },
                        datalabels: { display: true, anchor: 'end', align: 'right', color: '#94a3b8', font: { weight: 'bold' } }
                    },
                    scales: {
                        x: { grid: { display: false }, ticks: { color: '#94a3b8' } },
                        y: { grid: { display: false }, ticks: { color: '#94a3b8' } }
                    }
                }
            });
        }

        loadSheetDashboard();
        setInterval(loadSheetDashboard, 60000);
    </script>
</body>
</html>
