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
            <p class="text-slate-400 font-medium tracking-widest text-xs uppercase">Buscando y ordenando matriz de datos...</p>
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
                        Búsqueda Dinámica por Nombre Habilitada
                    </p>
                </div>
            </div>

            <!-- Selector Desplegable de Tutores Oficiales -->
            <div class="flex items-center space-x-3">
                <label for="tutor-select" class="text-xs font-bold text-slate-400 uppercase tracking-wider hidden md:block">Tutor Responsable:</label>
                <div class="relative">
                    <select id="tutor-select" onchange="changeTutorProfile(this.value)" class="bg-slate-850 border border-slate-700 text-slate-200 text-xs font-bold rounded-xl px-4 py-2.5 pr-10 hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-600 transition-all appearance-none cursor-pointer shadow-md" translate="no">
                        <option value="GARCIA LESLY">GARCIA LESLY</option>
                        <option value="MARTINEZ NATALY">MARTINEZ NATALY</option>
                        <option value="BOZA MARIANA">BOZA MARIANA</option>
                        <option value="SANCHEZ CINTHYA">SANCHEZ CINTHYA</option>
                        <option value="ALCARRAZ ALEXANDER">ALCARRAZ ALEXANDER</option>
                        <option value="CARRERA VIRGINIA">CARRERA VIRGINIA</option>
                        <option value="ESPADA RODRIGO 1">ESPADA RODRIGO 1</option>
                        <option value="ESPADA RODRIGO 2">ESPADA RODRIGO 2</option>
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
            ⚠️ <strong>Error de Sincronización:</strong> No pudimos encontrar los bloques de datos correspondientes en la pestaña 'matriz de datos'.
        </div>
    </div>

    <!-- CONTENEDOR PRINCIPAL MULTI-VISTA -->
    <main class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 py-8 flex-1 w-full space-y-8 relative">
        
        <!-- Pestañas de Navegación -->
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

        <!-- Cargador de transición interna -->
        <div id="view-loader" class="hidden absolute inset-0 bg-slate-950/40 backdrop-blur-sm z-40 flex items-center justify-center rounded-2xl transition-opacity duration-300">
            <div class="animate-spin rounded-full h-10 w-10 border-4 border-blue-500 border-t-transparent"></div>
        </div>

        <!-- ÁREA DINÁMICA DE VISTAS -->
        <div class="relative w-full overflow-hidden min-h-[600px]">
            
            <!-- VISTA 1: DASHBOARD -->
            <div id="view-container-dashboard" class="view-transition opacity-100 transform translate-x-0 space-y-8 w-full">
                <!-- METRICAS CONSOLIDADAS CON DIVISION EXSA Y EXSI -->
                <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-blue-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📈 Promedio EXSA</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsa">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media académica de la serie EXSA</p>
                    </div>
                    <div class="premium-card rounded-2xl p-7 flex flex-col justify-between shadow-lg border-l-4 border-l-cyan-500">
                        <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📉 Promedio EXSI</p>
                        <h3 class="text-3xl font-black text-white mt-4 font-mono" id="kpi-exsi">0.00</h3>
                        <p class="text-[11px] text-slate-500 mt-2">Media académica de la serie EXSI</p>
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
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Proporción de Asistencias Total</h3>
                        <div class="relative h-56 flex items-center justify-center">
                            <canvas id="chartAsistenciaDoughnut"></canvas>
                        </div>
                        <div class="text-center text-xs text-slate-400 mt-4" id="chart-doughnut-legend">...</div>
                    </div>
                </section>

                <!-- GRÁFICO BARRAS: ASISTENCIA POR EXAMEN -->
                <section class="premium-card rounded-2xl p-8 shadow-xl w-full">
                    <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Métrica Comparativa de Asistencias y Faltas por Examen</h3>
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
                            <h3 class="text-sm font-bold text-white tracking-tight">Reporte Detallado de Notas de la Matriz</h3>
                            <p class="text-[11px] text-slate-400 mt-1.5">Información real procesada por examen y competencias cruzadas</p>
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
                                    <th class="py-4.5 px-5 text-center text-emerald-400 bg-emerald-500/5">Asistencias (A)</th>
                                    <th class="py-4.5 px-5 text-center text-rose-400 bg-rose-500/5">Faltas (F)</th>
                                    <th class="py-4.5 px-6 text-right text-cyan-400">Nota</th>
                                    <th class="py-4.5 px-5 text-center">Avance %</th>
                                    <th class="py-4.5 px-5 text-center">SICA / SICI</th>
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
        const GID_MATRIZ = '1386929555'; 
        const URL_API = `https://docs.google.com/spreadsheets/d/${SPREADSHEET_ID}/gviz/tq?tqx=out:json&gid=${GID_MATRIZ}`;

        let rawSheetTable = null; 
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
            let str = cell.v.toString().replace(/\./g, '').replace(',', '.');
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
                if (startIdx === -1 || endIdx === -1) throw new Error("Error en formato JSON.");

                const jsonString = text.substring(startIdx, endIdx + 1);
                rawSheetTable = JSON.parse(jsonString).table;

                changeTutorProfile(document.getElementById('tutor-select').value);

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

        // CONTROLADOR INTELIGENTE: ENCUENTRA LA FILA EXACTA BUSCANDO POR NOMBRE EN CADA BLOQUE
        function changeTutorProfile(tutorName) {
            if (!rawSheetTable || !rawSheetTable.rows) return;

            const loader = document.getElementById('view-loader');
            loader.classList.remove('hidden');
            loader.style.opacity = '1';

            let rowNotes = null, rowSica = null, rowAsist = null, rowSec = null;
            let targetTutorClean = tutorName.trim().toUpperCase();

            // Escaneo dinámico vertical por bloques para emparejar al tutor real sin errores cruzados
            rawSheetTable.rows.forEach((row, idx) => {
                if (!row.c || !row.c[0]) return;
                let currentTutorInRow = cleanValue(row.c[0]).toUpperCase();

                if (currentTutorInRow.includes(targetTutorClean) || targetTutorClean.includes(currentTutorInRow)) {
                    if (idx >= 2 && idx <= 9) rowNotes = row.c;       // Bloque de Notas reales
                    if (idx >= 12 && idx <= 19) rowSica = row.c;      // Bloque de SICA/SICI
                    if (idx >= 23 && idx <= 30) rowAsist = row.c;     // Bloque de Asistencias (A/F)
                    if (idx >= 33 && idx <= 40) rowSec = row.c;       // Bloque de C+D / CXM
                }
            });

            // Si falla la búsqueda, mostrar caja de alerta
            if (!rowNotes || !rowAsist) {
                document.getElementById('error-box').classList.remove('hidden');
                loader.classList.add('hidden');
                return;
            } else {
                document.getElementById('error-box').classList.add('hidden');
            }

            parsedEvaluations = [];
            let sumExsa = 0, countExsa = 0, sumExsi = 0, countExsi = 0;
            let totalA = 0, totalF = 0;

            // Procesar bloque EXSA (1 al 9)
            for (let i = 1; i <= 9; i++) {
                let noteVal = cleanNumericValue(rowNotes[2 + i]); 
                let noteText = cleanValue(rowNotes[2 + i]);
                let avanceText = cleanValue(rowNotes[22 + i]) || "-"; 
                let sicaVal = rowSica ? (cleanValue(rowSica[2 + i]) || "-") : "-";     
                
                let aVal = parseInt(cleanValue(rowAsist[2 * i - 1])) || 0;
                let fVal = parseInt(cleanValue(rowAsist[2 * i])) || 0;

                let cdVal = rowSec ? (cleanValue(rowSec[2 * i]) || "-") : "-";
                let cxmVal = rowSec ? (cleanValue(rowSec[2 * i + 1]) || "-") : "-";

                if (noteVal > 0) { sumExsa += noteVal; countExsa++; }
                totalA += aVal; totalF += fVal;

                parsedEvaluations.push({
                    type: 'EXSA', name: `EXSA ${i}`, note: noteVal, noteText: noteText || "-",
                    avance: avanceText, sica: sicaVal, a: aVal, f: fVal, cd: cdVal, cxm: cxmVal
                });
            }

            // Procesar bloque EXSI (1 al 9)
            for (let i = 1; i <= 9; i++) {
                let noteVal = cleanNumericValue(rowNotes[12 + i]); 
                let noteText = cleanValue(rowNotes[12 + i]);
                let avanceText = cleanValue(rowNotes[31 + i]) || "-"; 
                let sicaVal = rowSica ? (cleanValue(rowSica[12 + i]) || "-") : "-";     
                
                let aVal = parseInt(cleanValue(rowAsist[17 + 2 * i])) || 0;
                let fVal = parseInt(cleanValue(rowAsist[18 + 2 * i])) || 0;

                let cdVal = rowSec ? (cleanValue(rowSec[18 + 2 * i]) || "-") : "-";
                let cxmVal = rowSec ? (cleanValue(rowSec[19 + 2 * i]) || "-") : "-";

                if (noteVal > 0) { sumExsi += noteVal; countExsi++; }
                totalA += aVal; totalF += fVal;

                parsedEvaluations.push({
                    type: 'EXSI', name: `EXSI ${i}`, note: noteVal, noteText: noteText || "-",
                    avance: avanceText, sica: sicaVal, a: aVal, f: fVal, cd: cdVal, cxm: cxmVal
                });
            }

            let avgExsa = countExsa > 0 ? (sumExsa / countExsa) : 0;
            let avgExsi = countExsi > 0 ? (sumExsi / countExsi) : 0;
            let globalAsistanceRatio = (totalA + totalF) > 0 ? ((totalA / (totalA + totalF)) * 100).toFixed(1) : "0.0";
            let globalAbsenceRatio = (totalA + totalF) > 0 ? ((totalF / (totalA + totalF)) * 100).toFixed(1) : "0.0";

            document.getElementById('kpi-exsa').innerText = avgExsa.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-exsi').innerText = avgExsi.toLocaleString('es-PE', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('kpi-asistencias').innerText = totalA.toLocaleString('es-PE');
            document.getElementById('kpi-pct-asistencia').innerText = `${globalAsistanceRatio}% Ratio global`;
            document.getElementById('kpi-faltas').innerText = totalF.toLocaleString('es-PE');
            document.getElementById('kpi-pct-faltas').innerText = `${globalAbsenceRatio}% Ratio ausentismo`;

            filterTable('TODOS');
            buildCharts(parsedEvaluations, totalA, totalF);

            setTimeout(() => {
                loader.style.opacity = '0';
                setTimeout(() => loader.classList.add('hidden'), 300);
            }, 300);
        }

        function renderTableRows(data) {
            const tbody = document.getElementById('table-body-notas');
            tbody.innerHTML = '';

            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-850/60";

                let varColor = "text-slate-400";
                if (row.avance.includes('-')) varColor = "text-rose-400";
                else if (row.avance !== '-' && row.avance !== '0.0%' && !row.avance.includes('#')) varColor = "text-emerald-400";

                tr.innerHTML = `
                    <td class="py-4 px-6 font-bold text-white tracking-wide" translate="no">${row.name}</td>
                    <td class="py-4 px-5 text-center text-emerald-400 bg-emerald-500/5 font-mono">${row.a}</td>
                    <td class="py-4 px-5 text-center text-rose-400 bg-rose-500/5 font-mono">${row.f}</td>
                    <td class="py-4 px-6 text-right text-cyan-400 font-extrabold font-mono">${row.noteText}</td>
                    <td class="py-4 px-5 text-center font-bold font-mono ${varColor}">${row.avance}</td>
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
