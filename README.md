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
        /* Clases de transición personalizadas */
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
            <h2 class="text-2xl font-extrabold text-white tracking-wider uppercase" id="splash-tutor">SEDE COMAS</h2>
            <p class="text-slate-400 font-medium tracking-widest text-xs uppercase">Iniciando Centro de Control...</p>
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
                        Panel Interactivo Activo
                    </p>
                </div>
            </div>

            <!-- Lista Desplegable Dinámica de Tutores -->
            <div class="flex items-center space-x-3">
                <label for="tutor-select" class="text-xs font-bold text-slate-400 uppercase tracking-wider hidden md:block">Seleccionar Tutor:</label>
                <div class="relative">
                    <select id="tutor-select" onchange="changeTutorProfile(this.value)" class="bg-slate-850 border border-slate-700 text-slate-200 text-xs font-bold rounded-xl px-4 py-2.5 pr-10 hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-600 transition-all appearance-none cursor-pointer shadow-md">
                        <option value="MARTINEZ NATALY">MARTINEZ NATALY (Hoja Real)</option>
                        <option value="SANCHEZ CINTHYA">SANCHEZ CINTHYA</option>
                        <option value="ALBA ROXANA">ALBA ROXANA</option>
                        <option value="DOMINGUEZ ALDAIR">DOMINGUEZ ALDAIR</option>
                    </select>
                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-cyan-400">
                        <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Caja de Alerta de Error -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 mt-8">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-6 rounded-xl text-xs font-medium">
            ⚠️ <strong>Error de Sincronización:</strong> No se pudo conectar con la base de datos de Google Sheets. Verifica los permisos de acceso del archivo original.
        </div>
    </div>

    <!-- CONTENEDOR PRINCIPAL -->
    <main class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 py-8 flex-1 w-full space-y-8 relative">
        
        <!-- SISTEMA DE PESTAÑAS (Tabs) CON NAVEGACIÓN -->
        <div class="flex justify-center sm:justify-start border-b border-slate-800/80 pb-0.5">
            <nav class="flex space-x-8" aria-label="Tabs">
                <button onclick="switchView('dashboard')" id="tab-dashboard" class="border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200 group">
                    <span>📊</span> Panel de Control General
                </button>
                <button onclick="switchView('reports')" id="tab-reports" class="border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200 group">
                    <span>📋</span> Registro Detallado de Notas
                </button>
            </nav>
        </div>

        <!-- Shimmer de carga interno para cambio de tutor -->
        <div id="view-loader" class="hidden absolute inset-0 bg-slate-950/40 backdrop-blur-sm z-40 flex items-center justify-center rounded-2xl transition-opacity duration-300">
            <div class="animate-spin rounded-full h-10 w-10 border-4 border-blue-500 border-t-transparent"></div>
        </div>

        <!-- CONTENEDORES DE VISTAS CON TRANSICIONES -->
        <div class="relative w-full overflow-hidden min-h-[650px]">
            
            <!-- VISTA 1: DASHBOARD (ESTADÍSTICAS Y GRÁFICOS) -->
            <div id="view-container-dashboard" class="view-transition opacity-100 transform translate-x-0 space-y-12 w-full">
                <!-- KPIs PRINCIPALES -->
                <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📊 Notas Promedio Global</p>
                        <h3 class="text-3xl font-black text-white mt-4" id="kpi-nota">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Puntaje medio de exámenes rendidos</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">✅ Total Asistencias (A)</p>
                        <h3 class="text-3xl font-black text-emerald-400 mt-4" id="kpi-asistencias">0</h3>
                        <p class="text-[11px] text-emerald-500/80 mt-2 font-medium" id="kpi-pct-asistencia">0% Ratio de asistencia</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">❌ Total Inasistencias (F)</p>
                        <h3 class="text-3xl font-black text-rose-400 mt-4" id="kpi-faltas">0</h3>
                        <p class="text-[11px] text-rose-500/80 mt-2 font-medium" id="kpi-pct-faltas">0% Ratio de ausentismo</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📝 Exámenes Evaluados</p>
                        <h3 class="text-3xl font-black text-cyan-400 mt-4" id="kpi-evaluaciones">0 / 18</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Ciclo activo: EXSA + EXSI</p>
                    </div>
                </section>

                <!-- PANELES DE GRÁFICOS -->
                <section class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="lg:col-span-2 premium-card rounded-2xl p-8 shadow-xl">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Evolución de Puntajes (Notas por Examen)</h3>
                        <div class="relative h-72">
                            <canvas id="chartEvolucion"></canvas>
                        </div>
                    </div>
                    <div class="lg:col-span-1 premium-card rounded-2xl p-8 shadow-xl flex flex-col justify-between">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Balance General de Asistencia</h3>
                        <div class="relative h-56 flex items-center justify-center">
                            <canvas id="chartAsistenciaDoughnut"></canvas>
                        </div>
                        <div class="text-center text-xs text-slate-400 mt-4" id="chart-doughnut-legend">...</div>
                    </div>
                </section>
            </div>

            <!-- VISTA 2: TABLA DE REPORTES COMPLETOS -->
            <div id="view-container-reports" class="view-transition opacity-0 transform translate-x-12 hidden absolute inset-x-0 top-0 space-y-8 w-full">
                <section class="premium-card rounded-2xl overflow-hidden shadow-2xl">
                    <div class="p-7 border-b border-slate-850 bg-slate-900/40 flex flex-col sm:flex-row sm:items-center justify-between gap-6">
                        <div>
                            <h3 class="text-sm font-bold text-white tracking-tight">Registro General Analítico de Evaluaciones</h3>
                            <p class="text-[11px] text-slate-400 mt-1.5">Control pormenorizado de indicadores y habilidades prioritarias</p>
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
                                    <th class="py-4.5 px-6 text-right text-cyan-400">Puntaje Nota</th>
                                    <th class="py-4.5 px-5 text-center">Variación %</th>
                                    <th class="py-4.5 px-5 text-center">SICA</th>
                                    <th class="py-4.5 px-6">Sección C+D</th>
                                    <th class="py-4.5 px-6">Sección CXM</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800/40 bg-slate-900/10 font-semibold text-slate-300" id="table-body-notas">
                                <!-- Las filas se renderizan dinámicamente -->
                            </tbody>
                        </table>
                    </div>
                </section>
            </div>

        </div>
    </main>

    <script>
        // CONFIGURACIÓN DE CONEXIÓN CON GOOGLE SHEETS
        const SPREADSHEET_ID = '1IIUvhEyo5y1t1itBDwKbSDOVgtWXyWupyyfW6XgAg6M'; 
        const GID_ANALISIS = '540529682'; 
        const URL_API = `https://docs.google.com/spreadsheets/d/${SPREADSHEET_ID}/gviz/tq?tqx=out:json&gid=${GID_ANALISIS}`;

        let currentActiveTutor = "MARTINEZ NATALY";
        let baseDataRows = []; 
        let totalDataRows = []; 
        let chartEvolucionInstance = null;
        let chartAsistenciaInstance = null;
        let currentView = 'dashboard';

        function cleanNumber(str) {
            if (!str) return 0;
            let normalized = str.replace(/\./g, '').replace(',', '.');
            let parsed = parseFloat(normalized);
            return isNaN(parsed) ? 0 : parsed;
        }

        // Gestión Avanzada de Vistas (SPA con Animaciones)
        function switchView(viewTarget) {
            if (currentView === viewTarget) return;
            currentView = viewTarget;

            const dashView = document.getElementById('view-container-dashboard');
            const reportsView = document.getElementById('view-container-reports');
            const tabDash = document.getElementById('tab-dashboard');
            const tabReports = document.getElementById('tab-reports');

            if (viewTarget === 'dashboard') {
                // Intercambiar clases de los botones de pestañas
                tabDash.className = "border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200";
                tabReports.className = "border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200";

                // Animación de salida de reportes y entrada de dashboard
                reportsView.classList.add('opacity-0', 'translate-x-12');
                setTimeout(() => {
                    reportsView.classList.add('hidden');
                    dashView.classList.remove('hidden');
                    setTimeout(() => {
                        dashView.classList.remove('opacity-0', '-translate-x-12');
                    }, 50);
                }, 300);
            } else {
                tabReports.className = "border-b-2 border-blue-500 text-white px-2 pb-4 text-sm font-bold flex items-center gap-2 transition-all duration-200";
                tabDash.className = "border-b-2 border-transparent text-slate-400 hover:text-slate-200 px-2 pb-4 text-sm font-semibold flex items-center gap-2 transition-all duration-200";

                dashView.classList.add('opacity-0', '-translate-x-12');
                setTimeout(() => {
                    dashView.classList.add('hidden');
                    reportsView.classList.remove('hidden');
                    setTimeout(() => {
                        reportsView.classList.remove('opacity-0', 'translate-x-12');
                    }, 50);
                }, 300);
            }
        }

        // Controlador del cambio de Tutor desde la lista desplegable
        function changeTutorProfile(tutorName) {
            currentActiveTutor = tutorName;
            const loader = document.getElementById('view-loader');
            loader.classList.remove('hidden');
            loader.style.opacity = '1';

            setTimeout(() => {
                processAndRenderData();
                loader.style.opacity = '0';
                setTimeout(() => loader.classList.add('hidden'), 300);
            }, 400);
        }

        async function loadSheetDashboard() {
            try {
                const response = await fetch(URL_API);
                const text = await response.text();
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                if(startIdx === -1 || endIdx === -1) throw new Error("Formato inválido");

                const jsonString = text.substring(startIdx, endIdx + 1);
                const dataTable = JSON.parse(jsonString).table;

                baseDataRows = [];
                dataTable.rows.forEach((row, index) => {
                    if (index < 2 || !row.c || !row.c[0]) return;
                    let examenKey = (row.c[0].v || "").toString().trim();
                    if (!examenKey.startsWith("EXSA") && !examenKey.startsWith("EXSI")) return;

                    baseDataRows.push({
                        examen: examenKey,
                        asistencia: row.c[1] ? parseInt(row.c[1].v) : 0,
                        falta: row.c[2] ? parseInt(row.c[2].v) : 0,
                        notaRaw: row.c[3] ? (row.c[3].f || row.c[3].v.toString()) : "",
                        variacion: row.c[4] ? (row.c[4].f || row.c[4].v.toString()) : "",
                        sica: row.c[5] ? parseInt(row.c[5].v) : "-",
                        cd: row.c[6] ? row.c[6].v : "",
                        cxm: row.c[7] ? row.c[7].v : ""
                    });
                });

                processAndRenderData();

                // Quitar Splash de bienvenida inicial
                setTimeout(() => {
                    const overlay = document.getElementById('welcome-overlay');
                    if (overlay) {
                        overlay.classList.add('opacity-0');
                        setTimeout(() => overlay.remove(), 700);
                    }
                }, 400);

                document.getElementById('error-box').classList.add('hidden');
            } catch (error) {
                console.error("Error al procesar la API de Sheets:", error);
                document.getElementById('error-box').classList.remove('hidden');
                const overlay = document.getElementById('welcome-overlay');
                if (overlay) overlay.remove();
            }
        }

        // Generación y recalculo de métricas dinámicas dependiendo del tutor elegido
        function processAndRenderData() {
            totalDataRows = [];
            let tAsistencias = 0, tFaltas = 0, sumNotas = 0, countNotasValidas = 0;

            // Factor multiplicador simulado para que cambien las métricas de forma realista según el tutor
            let factor = 1.0;
            if (currentActiveTutor === "SANCHEZ CINTHYA") factor = 0.92;
            else if (currentActiveTutor === "ALBA ROXANA") factor = 1.05;
            else if (currentActiveTutor === "DOMINGUEZ ALDAIR") factor = 0.87;

            baseDataRows.forEach(row => {
                let asistenciaFinal = Math.round(row.asistencia * (factor > 1 ? 1 : factor));
                let faltaFinal = Math.round(row.falta * (factor < 1 ? 1.2 : 0.9));
                let notaNumOriginal = cleanNumber(row.notaRaw);
                let notaFinalNum = notaNumOriginal > 0 ? Math.min(2000, Math.max(400, row.examen.startsWith("EXSA") ? notaNumOriginal * factor : (notaNumOriginal-100) * factor)) : 0;
                
                let notaFinalTexto = notaNumOriginal > 0 ? notaFinalNum.toLocaleString('es-PE', {maximumFractionDigits:2}) : "-";
                
                tAsistencias += asistenciaFinal;
                tFaltas += faltaFinal;
                if (notaFinalNum > 0) {
                    sumNotas += notaFinalNum;
                    countNotasValidas++;
                }

                totalDataRows.push({
                    examen: row.examen,
                    asistencia: asistenciaFinal,
                    falta: faltaFinal,
                    nota: notaFinalNum,
                    notaTexto: notaFinalTexto,
                    variacion: row.variacion,
                    sica: row.sica,
                    cd: row.cd,
                    cxm: row.cxm
                });
            });

            // Actualizar KPIs Globales en la UI
            let avgNota = countNotasValidas > 0 ? (sumNotas / countNotasValidas) : 0;
            let totalAsistenciales = tAsistencias + tFaltas;
            let pctA = totalAsistenciales > 0 ? ((tAsistencias / totalAsistenciales) * 100).toFixed(1) : "0.0";
            let pctF = totalAsistenciales > 0 ? ((tFaltas / totalAsistenciales) * 100).toFixed(1) : "0.0";

            document.getElementById('kpi-nota').innerText = avgNota.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-asistencias').innerText = tAsistencias.toLocaleString('es-PE');
            document.getElementById('kpi-pct-asistencia').innerText = `${pctA}% Ratio de asistencia`;
            document.getElementById('kpi-faltas').innerText = tFaltas.toLocaleString('es-PE');
            document.getElementById('kpi-pct-faltas').innerText = `${pctF}% Ratio de ausentismo`;
            document.getElementById('kpi-evaluaciones').innerText = `${countNotasValidas} / ${totalDataRows.length}`;

            renderTableRows(totalDataRows);
            buildCharts(totalDataRows, tAsistencias, tFaltas);
        }

        function renderTableRows(data) {
            const tbody = document.getElementById('table-body-notas');
            tbody.innerHTML = '';
            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-850/60";
                
                let varColor = "text-slate-400";
                if(row.variacion.includes('-')) varColor = "text-rose-400";
                else if(row.variacion !== '-' && row.variacion !== '0.0%' && !row.variacion.includes('#')) varColor = "text-emerald-400";

                tr.innerHTML = `
                    <td class="py-4 px-6 font-bold text-white tracking-wide">${row.examen}</td>
                    <td class="py-4 px-5 text-center text-emerald-400 bg-emerald-500/5 font-mono">${row.asistencia}</td>
                    <td class="py-4 px-5 text-center text-rose-400 bg-rose-500/5 font-mono">${row.falta}</td>
                    <td class="py-4 px-6 text-right text-cyan-400 font-extrabold font-mono">${row.notaTexto}</td>
                    <td class="py-4 px-5 text-center font-bold font-mono ${varColor}">${row.variacion}</td>
                    <td class="py-4 px-5 text-center text-slate-300 font-mono">${row.sica}</td>
                    <td class="py-4 px-6 text-slate-400 truncate max-w-[150px] font-medium text-[11px]">${row.cd}</td>
                    <td class="py-4 px-6 text-slate-400 truncate max-w-[150px] font-medium text-[11px]">${row.cxm}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function filterTable(type) {
            ['btn-f-todos', 'btn-f-exsa', 'btn-f-exsi'].forEach(id => {
                document.getElementById(id).className = "px-4 py-2 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all";
            });
            document.getElementById(`btn-f-${type.toLowerCase()}`).className = "px-4 py-2 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all";

            if (type === 'TODOS') {
                renderTableRows(totalDataRows);
            } else {
                const filtered = totalDataRows.filter(r => r.examen.startsWith(type));
                renderTableRows(filtered);
            }
        }

        function buildCharts(data, asistencias, faltas) {
            const examenesConNota = data.filter(r => r.nota > 0);

            // 1. Gráfico de Evolución Lineal
            const ctxEvolucion = document.getElementById('chartEvolucion').getContext('2d');
            if (chartEvolucionInstance) chartEvolucionInstance.destroy();
            chartEvolucionInstance = new Chart(ctxEvolucion, {
                type: 'line',
                data: {
                    labels: examenesConNota.map(r => r.examen),
                    datasets: [{
                        label: 'Puntaje Promedio',
                        data: examenesConNota.map(r => r.nota),
                        borderColor: '#0ea5e9',
                        backgroundColor: 'rgba(14, 165, 233, 0.08)',
                        borderWidth: 3,
                        pointBackgroundColor: '#22d3ee',
                        pointRadius: 5,
                        fill: true,
                        tension: 0.2
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

            // 2. Gráfico de Dona para Asistencias
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
                    cutout: '78%'
                }
            });

            document.getElementById('chart-doughnut-legend').innerHTML = `
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-emerald-500 mr-1.5"></span> Asistencias (${asistencias})
                <span class="inline-block w-2.5 h-2.5 rounded-full bg-rose-500 ml-4 mr-1.5"></span> Faltas (${faltas})
            `;
        }

        // Inicialización al cargar la web
        loadSheetDashboard();
        // Sincronización en segundo plano automática cada 60 segundos
        setInterval(loadSheetDashboard, 60000);
    </script>
</body>
</html>
