<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SEDE COMAS - ANÁLISIS DE NOTAS</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
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
            transition: opacity 300ms ease-in-out, transform 300ms ease-in-out;
        }
    </style>
</head>
<body class="text-slate-200 min-h-screen antialiased custom-scroll flex flex-col">

    <!-- SPLASH SCREEN DE BIENVENIDA -->
    <div id="welcome-overlay" class="fixed inset-0 z-[100] bg-slate-950 flex items-center justify-center transition-opacity duration-700 opacity-100">
        <div class="text-center space-y-6 animate-pulse">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-2xl bg-gradient-to-tr from-blue-600 to-cyan-500 shadow-[0_0_50px_rgba(14,165,233,0.3)] mb-4">
                <span class="text-4xl text-white font-black" translate="no">SC</span>
            </div>
            <h2 class="text-2xl font-extrabold text-white tracking-wider uppercase">SEDE COMAS</h2>
            <p class="text-slate-400 font-medium tracking-widest text-xs uppercase">Conectando cuadro de análisis en vivo...</p>
        </div>
    </div>

    <!-- Encabezado Principal -->
    <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 h-20 flex items-center justify-between shadow-lg">
            <div class="flex items-center space-x-4">
                <div class="bg-blue-600 px-4 py-2 rounded-xl text-white font-extrabold text-lg tracking-wider" translate="no">COMAS</div>
                <div>
                    <h1 class="text-sm font-bold text-white tracking-tight uppercase">ANÁLISIS DE NOTAS</h1>
                    <p class="text-[11px] text-cyan-400 font-semibold flex items-center gap-1.5">
                        <span class="flex h-2 w-2 relative">
                            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-cyan-400 opacity-75"></span>
                            <span class="relative inline-flex rounded-full h-2 w-2 bg-cyan-500"></span>
                        </span>
                        Sincronización Directa y Exacta
                    </p>
                </div>
            </div>

            <!-- Selector Informativo del Tutor Sincronizado -->
            <div class="flex items-center space-x-3">
                <label class="text-xs font-bold text-slate-400 uppercase tracking-wider hidden md:block">Tutor Detectado:</label>
                <div class="relative">
                    <select id="tutor-display-select" class="bg-slate-850 border border-slate-750 text-cyan-400 text-xs font-bold rounded-xl px-4 py-2.5 bg-slate-900 pointer-events-none appearance-none shadow-md" translate="no">
                        <option id="active-tutor-option" value="">Cargando Tutor...</option>
                    </select>
                </div>
            </div>
        </div>
    </header>

    <!-- Caja de Alerta de Error -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 mt-8">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-6 rounded-xl text-xs font-medium">
            ⚠️ <strong>Error de Sincronización:</strong> No se pudo leer el cuadro de Análisis. Asegúrate de que el documento esté compartido de forma pública.
        </div>
    </div>

    <!-- CONTENEDOR PRINCIPAL MULTI-VISTA -->
    <main class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 py-8 flex-1 w-full space-y-8 relative">
        
        <!-- Pestañas de Navegación SPA -->
        <div class="flex justify-center sm:justify-start border-b border-slate-800/80 pb-0.5">
            <nav class="flex space-x-8" aria-label="Tabs">
                <button onclick="switchView('dashboard')" id="tab-dashboard" class="border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200">
                    <span>📊</span> Estadísticas y KPIs
                </button>
                <button onclick="switchView('reports')" id="tab-reports" class="border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200">
                    <span>📋</span> Registro Detallado (Datos Reales)
                </button>
            </nav>
        </div>

        <!-- ÁREA DINÁMICA DE VISTAS -->
        <div class="relative w-full overflow-hidden min-h-[600px]">
            
            <!-- VISTA 1: DASHBOARD -->
            <div id="view-container-dashboard" class="view-transition opacity-100 transform translate-x-0 space-y-8 w-full">
                <!-- METRICAS CONSOLIDADAS DIVIDIDAS (EXSA / EXSI) -->
                <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-blue-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📈 Promedio EXSA</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsa">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media real del bloque EXSA</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-cyan-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📉 Promedio EXSI</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsi">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media real del bloque EXSI</p>
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

                <!-- PANELES DE GRÁFICOS -->
                <section class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="lg:col-span-2 premium-card rounded-2xl p-8 shadow-xl">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Evolución Cronológica de Calificaciones</h3>
                        <div class="relative h-72">
                            <canvas id="chartEvolucion"></canvas>
                        </div>
                    </div>
                    <div class="lg:col-span-1 premium-card rounded-2xl p-8 shadow-xl flex flex-col justify-between">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Balance de Asistencia General</h3>
                        <div class="relative h-56 flex items-center justify-center">
                            <canvas id="chartAsistenciaDoughnut"></canvas>
                        </div>
                        <div class="text-center text-xs text-slate-400 mt-4" id="chart-doughnut-legend">...</div>
                    </div>
                </section>

                <!-- GRÁFICO NUEVO: ASISTENCIA POR EXAMEN (BARRAS AGRUPADAS) -->
                <section class="premium-card rounded-2xl p-8 shadow-xl w-full">
                    <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Desglose Colectivo de Asistencias y Faltas por Examen</h3>
                    <div class="relative h-80">
                        <canvas id="chartAsistenciaPorExamen"></canvas>
                    </div>
                </section>
            </div>

            <!-- VISTA 2: REGISTRO DETALLADO -->
            <div id="view-container-reports" class="view-transition opacity-0 transform translate-x-12 hidden absolute inset-x-0 top-0 space-y-8 w-full">
                <section class="premium-card rounded-2xl overflow-hidden shadow-2xl">
                    <div class="p-7 border-b border-slate-850 bg-slate-900/40 flex flex-col sm:flex-row sm:items-center justify-between gap-6">
                        <div>
                            <h3 class="text-sm font-bold text-white tracking-tight">Reporte General del Cuadro de Análisis</h3>
                            <p class="text-[11px] text-slate-400 mt-1.5">Información fidedigna mapeada celda por celda</p>
                        </div>
                        <div class="flex items-center space-x-3">
                            <button onclick="filterTable('TODOS')" id="btn-f-todos" class="px-4 py-2 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all">Todos</button>
                            <button onclick="filterTable('EXSA')" id="btn-f-exsa" class="px-4 py-2 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all">EXSA</button>
                            <button onclick="filterTable('EXSI')" id="btn-f-exsi" class="px-4 py-2 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all">EXSI</button>
                        </div>
                    </div>
                    <div class="w-full overflow-x-auto custom-scroll">
                        <table class="w-full text-left border-collapse text-xs">
                            <thead>
                                <tr class="bg-slate-900/80 text-slate-400 font-bold uppercase tracking-wider border-b border-slate-800 text-[10px]">
                                    <th class="py-4.5 px-6">Evaluación</th>
                                    <th class="py-4.5 px-5 text-center text-emerald-400 bg-emerald-500/5">Asistieron (A)</th>
                                    <th class="py-4.5 px-5 text-center text-rose-400 bg-rose-500/5">Faltaron (F)</th>
                                    <th class="py-4.5 px-6 text-right text-cyan-400">Nota</th>
                                    <th class="py-4.5 px-5 text-center">Variación %</th>
                                    <th class="py-4.5 px-5 text-center">SICA</th>
                                    <th class="py-4.5 px-6">Sección C+D</th>
                                    <th class="py-4.5 px-6">Sección CXM</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800/40 bg-slate-900/10 font-semibold text-slate-300" id="table-body-notas">
                                <!-- Filas Reales Insertadas Dinámicamente -->
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>

        </div>
    </main>

    <script>
        const SPREADSHEET_ID = '1IIUvhEyo5y1t1itBDwKbSDOVgtWXyWupyyfW6XgAg6M'; 
        const GID_ANALISIS = '540529682'; 
        const URL_API = `https://docs.google.com/spreadsheets/d/${SPREADSHEET_ID}/gviz/tq?tqx=out:json&gid=${GID_ANALISIS}`;

        let liveSheetRows = []; 
        let parsedEvaluations = []; 
        let filteredEvaluations = []; 
        let chartEvolucionInstance = null;
        let chartAsistenciaInstance = null;
        let chartBarAsistenciaInstance = null;
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

            const dashView = document.getElementById('view-container-dashboard');
            const reportsView = document.getElementById('view-container-reports');
            const tabDash = document.getElementById('tab-dashboard');
            const tabReports = document.getElementById('tab-reports');

            if (viewTarget === 'dashboard') {
                tabDash.className = "border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200";
                tabReports.className = "border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200";
                reportsView.classList.add('opacity-0', 'translate-x-12');
                setTimeout(() => {
                    reportsView.classList.add('hidden');
                    dashView.classList.remove('hidden');
                    setTimeout(() => dashView.classList.remove('opacity-0', '-translate-x-12'), 50);
                }, 300);
            } else {
                tabReports.className = "border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200";
                tabDash.className = "border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200";
                dashView.classList.add('opacity-0', '-translate-x-12');
                setTimeout(() => {
                    dashView.classList.add('hidden');
                    reportsView.classList.remove('hidden');
                    setTimeout(() => reportsView.classList.remove('opacity-0', 'translate-x-12'), 50);
                }, 300);
            }
        }

        async function loadSheetDashboard() {
            try {
                const response = await fetch(URL_API);
                const text = await response.text();
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                if (startIdx === -1 || endIdx === -1) throw new Error("Error de formato.");

                const jsonString = text.substring(startIdx, endIdx + 1);
                const dataTable = JSON.parse(jsonString).table;

                if (!dataTable || !dataTable.rows) return;
                liveSheetRows = dataTable.rows;

                // Capturar dinámicamente el tutor desde la celda combinada B1 de tu hoja
                let currentLiveTutor = cleanValue(liveSheetRows[0]?.c[1]) || "SANCHEZ CINTHYA";
                document.getElementById('active-tutor-option').innerText = currentLiveTutor;

                processAnalysisData();

                setTimeout(() => {
                    const overlay = document.getElementById('welcome-overlay');
                    if (overlay) {
                        overlay.classList.add('opacity-0');
                        setTimeout(() => overlay.remove(), 700);
                    }
                }, 400);
                document.getElementById('error-box').classList.add('hidden');
            } catch (error) {
                console.error(error);
                document.getElementById('error-box').classList.remove('hidden');
                const overlay = document.getElementById('welcome-overlay');
                if (overlay) overlay.remove();
            }
        }

        function processAnalysisData() {
            parsedEvaluations = [];
            let sumExsa = 0, countExsa = 0, sumExsi = 0, countExsi = 0;
            let totalA = 0, totalF = 0;

            liveSheetRows.forEach((row, index) => {
                if (index < 2 || !row.c || !row.c[0]) return; // Omitir cabeceras de fila 1 y 2

                let examenKey = cleanValue(row.c[0]).toUpperCase();
                if (!examenKey.startsWith("EXSA") && !examenKey.startsWith("EXSI")) return;

                // Extracción e inyección directa y exacta sin alteraciones numéricas
                let aVal = row.c[1] ? parseInt(row.c[1].v) : 0;
                let fVal = row.c[2] ? parseInt(row.c[2].v) : 0;
                let noteVal = cleanNumericValue(row.c[3]);
                let noteText = cleanValue(row.c[3]) || "-";
                let variacionText = cleanValue(row.c[4]) || "---";
                let sicaText = cleanValue(row.c[5]) || "-";
                let cdText = cleanValue(row.c[6]) || "-";
                let cxmText = cleanValue(row.c[7]) || "-";

                totalA += aVal;
                totalF += fVal;

                if (examenKey.startsWith("EXSA")) {
                    if (noteVal > 0) { sumExsa += noteVal; countExsa++; }
                    parsedEvaluations.push({
                        type: 'EXSA', name: cleanValue(row.c[0]), a: aVal, f: fVal, note: noteVal,
                        noteText: noteText, variacion: variacionText, sica: sicaText, cd: cdText, cxm: cxmText
                    });
                } else if (examenKey.startsWith("EXSI")) {
                    if (noteVal > 0) { sumExsi += noteVal; countExsi++; }
                    parsedEvaluations.push({
                        type: 'EXSI', name: cleanValue(row.c[0]), a: aVal, f: fVal, note: noteVal,
                        noteText: noteText, variacion: variacionText, sica: sicaText, cd: cdText, cxm: cxmText
                    });
                }
            });

            // Promedios e indicadores globales
            let avgExsa = countExsa > 0 ? (sumExsa / countExsa) : 0;
            let avgExsi = countExsi > 0 ? (sumExsi / countExsi) : 0;
            let totalAsistenciales = totalA + totalF;
            let pctA = totalAsistenciales > 0 ? ((totalA / totalAsistenciales) * 100).toFixed(1) : "0.0";
            let pctF = totalAsistenciales > 0 ? ((totalF / totalAsistenciales) * 100).toFixed(1) : "0.0";

            document.getElementById('kpi-exsa').innerText = avgExsa.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-exsi').innerText = avgExsi.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-asistencias').innerText = totalA.toString();
            document.getElementById('kpi-pct-asistencia').innerText = `${pctA}% Ratio global`;
            document.getElementById('kpi-faltas').innerText = totalF.toString();
            document.getElementById('kpi-pct-faltas').innerText = `${pctF}% Ratio ausentismo`;

            filterTable('TODOS');
            buildCharts(parsedEvaluations, totalA, totalF);
        }

        function renderTableRows(data) {
            const tbody = document.getElementById('table-body-notas');
            tbody.innerHTML = '';

            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-850/60";

                let varColor = "text-slate-400";
                if (row.variacion.includes('-')) varColor = "text-rose-400";
                else if (row.variacion !== '-' && row.variacion !== '0.0%' && !row.variacion.includes('#') && row.variacion !== '---') varColor = "text-emerald-400";

                tr.innerHTML = `
                    <td class="py-4 px-6 font-bold text-white tracking-wide" translate="no">${row.name}</td>
                    <td class="py-4 px-5 text-center text-emerald-400 bg-emerald-500/5 font-mono">${row.a}</td>
                    <td class="py-4 px-5 text-center text-rose-400 bg-rose-500/5 font-mono">${row.f}</td>
                    <td class="py-4 px-6 text-right text-cyan-400 font-extrabold font-mono">${row.noteText}</td>
                    <td class="py-4 px-5 text-center font-bold font-mono ${varColor}">${row.variacion}</td>
                    <td class="py-4 px-5 text-center text-slate-300 font-mono">${row.sica}</td>
                    <td class="py-4 px-6 text-slate-400 truncate max-w-[160px] font-medium text-[11px]">${row.cd}</td>
                    <td class="py-4 px-6 text-slate-400 truncate max-w-[160px] font-medium text-[11px]">${row.cxm}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function filterTable(type) {
            ['btn-f-todos', 'btn-f-exsa', 'btn-f-exsi'].forEach(id => {
                document.getElementById(id).className = "px-4 py-2 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all";
            });
            
            let activeBtnId = type === 'TODOS' ? 'btn-f-todos' : `btn-f-${type.toLowerCase()}`;
            document.getElementById(activeBtnId).className = "px-4 py-2 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all";

            if (type === 'TODOS') {
                filteredEvaluations = [...parsedEvaluations];
            } else {
                filteredEvaluations = parsedEvaluations.filter(r => r.type === type);
            }
            renderTableRows(filteredEvaluations);
        }

        function buildCharts(data, asistencias, faltas) {
            const activeData = data.filter(r => r.note > 0);

            // 1. Gráfico de Evolución de Notas
            const ctxEvolucion = document.getElementById('chartEvolucion').getContext('2d');
            if (chartEvolucionInstance) chartEvolucionInstance.destroy();
            chartEvolucionInstance = new Chart(ctxEvolucion, {
                type: 'line',
                data: {
                    labels: activeData.map(r => r.name),
                    datasets: [{
                        label: 'Puntaje Real',
                        data: activeData.map(r => r.note),
                        borderColor: '#0ea5e9',
                        backgroundColor: 'rgba(14, 165, 233, 0.06)',
                        borderWidth: 3,
                        pointBackgroundColor: '#22d3ee',
                        pointRadius: 4,
                        fill: true,
                        tension: 0.15
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        x: { grid: { color: 'rgba(255, 255, 255, 0.02)' }, ticks: { color: '#94a3b8' } },
                        y: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });

            // 2. Gráfico de Rosca de Asistencia Total
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
                    plugins: { legend: { display: false } },
                    cutout: '76%'
                }
            });

            document.getElementById('chart-doughnut-legend').innerHTML = `
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-emerald-500 mr-1.5"></span> Asistencias (${asistencias})
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-rose-500 ml-4 mr-1.5"></span> Faltas (${faltas})
            `;

            // 3. Gráfico de Barras Agrupadas: Asistencia por Examen
            const ctxBarAsistencia = document.getElementById('chartAsistenciaPorExamen').getContext('2d');
            if (chartBarAsistenciaInstance) chartBarAsistenciaInstance.destroy();
            chartBarAsistenciaInstance = new Chart(ctxBarAsistencia, {
                type: 'bar',
                data: {
                    labels: data.map(r => r.name),
                    datasets: [
                        {
                            label: 'Asistieron (A)',
                            data: data.map(r => r.a),
                            backgroundColor: '#10b981',
                            borderRadius: 6
                        },
                        {
                            label: 'Faltaron (F)',
                            data: data.map(r => r.f),
                            backgroundColor: '#f43f5e',
                            borderRadius: 6
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            labels: { color: '#94a3b8', font: { weight: 'bold', size: 11 } }
                        }
                    },
                    scales: {
                        x: { grid: { display: false }, ticks: { color: '#94a3b8' } },
                        y: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });
        }

        loadSheetDashboard();
        setInterval(loadSheetDashboard, 60000);
    </script>
</body>
</html>
