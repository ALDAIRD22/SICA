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
    </style>
</head>
<body class="text-slate-200 min-h-screen antialiased custom-scroll">

    <!-- SPLASH SCREEN DE BIENVENIDA -->
    <div id="welcome-overlay" class="fixed inset-0 z-[100] bg-slate-950 flex items-center justify-center transition-opacity duration-700 opacity-100">
        <div class="text-center space-y-4 animate-pulse">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-2xl bg-gradient-to-tr from-blue-600 to-cyan-500 shadow-[0_0_50px_rgba(14,165,233,0.3)] mb-2">
                <span class="text-4xl text-white font-black">SC</span>
            </div>
            <h2 class="text-2xl font-extrabold text-white tracking-wider uppercase" id="splash-tutor">SEDE COMAS</h2>
            <p class="text-slate-400 font-medium tracking-widest text-xs uppercase">Cargando Analítica en Vivo...</p>
        </div>
    </div>

    <!-- Encabezado Principal -->
    <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="bg-blue-600 px-3 py-1.5 rounded-xl text-white font-extrabold text-lg tracking-wider">COMAS</div>
                <div>
                    <h1 class="text-sm font-bold text-white tracking-tight uppercase" id="header-title">ANÁLISIS DE NOTAS</h1>
                    <p class="text-[11px] text-cyan-400 font-medium" id="header-subtitle">Tutor: Cargando...</p>
                </div>
            </div>
            <div class="flex items-center space-x-2">
                <span class="flex h-2 w-2 relative">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                </span>
                <span class="text-[10px] font-bold text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-full border border-emerald-500/20 uppercase">Hojas de Cálculo Sincronizadas</span>
            </div>
        </div>
    </header>

    <!-- Caja de Alerta de Error -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-4 rounded-xl text-xs font-medium">
            ⚠️ <strong>Error de Conexión:</strong> No se pudo sincronizar con la API de Google Sheets. Asegúrate de que el documento esté compartido de forma pública.
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">
        
        <!-- KPIs PRINCIPALES (Métricas Consolidadas de la Hoja) -->
        <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-lg">
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📊 Notas Promedio Global</p>
                <h3 class="text-3xl font-black text-white mt-2" id="kpi-nota">0.00</h3>
                <p class="text-[11px] text-slate-500 mt-1">Puntaje medio de exámenes rendidos</p>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-lg">
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">✅ Total Asistencias (A)</p>
                <h3 class="text-3xl font-black text-emerald-400 mt-2" id="kpi-asistencias">0</h3>
                <p class="text-[11px] text-emerald-500/80 mt-1 font-medium" id="kpi-pct-asistencia">0% Ratio de asistencia</p>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-lg">
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">❌ Total Inasistencias (F)</p>
                <h3 class="text-3xl font-black text-rose-400 mt-2" id="kpi-faltas">0</h3>
                <p class="text-[11px] text-rose-500/80 mt-1 font-medium" id="kpi-pct-faltas">0% Ratio de ausentismo</p>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-lg">
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider">📝 Exámenes Evaluados</p>
                <h3 class="text-3xl font-black text-cyan-400 mt-2" id="kpi-evaluaciones">0 / 18</h3>
                <p class="text-[11px] text-slate-500 mt-1">Ciclo activo: EXSA + EXSI</p>
            </div>
        </section>

        <!-- PANELES DE GRÁFICOS Y ANÁLISIS DE TENDENCIAS -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <!-- Evolución Académica -->
            <div class="lg:col-span-2 premium-card rounded-2xl p-6 shadow-xl">
                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-4">Evolución de Puntajes (Notas por Examen)</h3>
                <div class="relative h-72">
                    <canvas id="chartEvolucion"></canvas>
                </div>
            </div>
            <!-- Distribución de Asistencia -->
            <div class="lg:col-span-1 premium-card rounded-2xl p-6 shadow-xl flex flex-col justify-between">
                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-4">Balance General de Asistencia</h3>
                <div class="relative h-56 flex items-center justify-center">
                    <canvas id="chartAsistenciaDoughnut"></canvas>
                </div>
                <div class="text-center text-xs text-slate-400 mt-2" id="chart-doughnut-legend">...</div>
            </div>
        </div>

        <!-- TABLA COMPLETA: REPORTES FILTRADOS Y EXÁMENES -->
        <section class="premium-card rounded-2xl overflow-hidden shadow-2xl">
            <div class="p-5 border-b border-slate-850 bg-slate-900/40 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <div>
                    <h3 class="text-sm font-bold text-white tracking-tight">Registro General Analítico de Evaluaciones</h3>
                    <p class="text-[11px] text-slate-400 mt-0.5">Control pormenorizado de indicadores y habilidades prioritarias</p>
                </div>
                <div class="flex items-center space-x-2">
                    <button onclick="filterTable('TODOS')" id="btn-f-todos" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all">Todos</button>
                    <button onclick="filterTable('EXSA')" id="btn-f-exsa" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all">EXSA</button>
                    <button onclick="filterTable('EXSI')" id="btn-f-exsi" class="px-3 py-1.5 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all">EXSI</button>
                </div>
            </div>
            <div class="w-full overflow-x-auto custom-scroll">
                <table class="w-full text-left border-collapse text-xs">
                    <thead>
                        <tr class="bg-slate-900/80 text-slate-400 font-bold uppercase tracking-wider border-b border-slate-800 text-[10px]">
                            <th class="py-3.5 px-4">Evaluación</th>
                            <th class="py-3.5 px-3 text-center text-emerald-400 bg-emerald-500/5">Asistieron (A)</th>
                            <th class="py-3.5 px-3 text-center text-rose-400 bg-rose-500/5">Faltaron (F)</th>
                            <th class="py-3.5 px-4 text-right text-cyan-400">Puntaje Nota</th>
                            <th class="py-3.5 px-3 text-center">Variación %</th>
                            <th class="py-3.5 px-3 text-center">SICA</th>
                            <th class="py-3.5 px-4">Sección C+D</th>
                            <th class="py-3.5 px-4">Sección CXM</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-slate-800/40 bg-slate-900/10 font-semibold text-slate-300" id="table-body-notas">
                        <!-- Las filas se renderizan dinámicamente -->
                    </tbody>
                </table>
            </div>
        </section>
    </main>

    <script>
        // CONFIGURACIÓN DINÁMICA DE GOOGLE SHEETS
        const SPREADSHEET_ID = '1FVmVUpD467JaxKZ23_ICqk5VLq5JFU-o';
        const GID_ANALISIS = '540529682'; // Pestaña: Análisis
        const URL_API = `https://docs.google.com/spreadsheets/d/${SPREADSHEET_ID}/gviz/tq?tqx=out:json&gid=${GID_ANALISIS}`;

        let totalDataRows = [];
        let chartEvolucionInstance = null;
        let chartAsistenciaInstance = null;

        function cleanNumber(str) {
            if (!str) return 0;
            let normalized = str.replace(/\./g, '').replace(',', '.');
            let parsed = parseFloat(normalized);
            return isNaN(parsed) ? 0 : parsed;
        }

        async function loadSheetDashboard() {
            try {
                const response = await fetch(URL_API);
                const text = await response.text();
                
                // Parseo del formato JSON seguro de la API de Google Visualization
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                const jsonString = text.substring(startIdx, endIdx + 1);
                const dataTable = JSON.parse(jsonString).table;

                // 1. Extraer Metadatos del Tutor de la Fila 1 (Celdas Combinadas)
                let tutorName = dataTable.rows[0]?.c[1]?.v || "SANCHEZ CINTHYA";
                let aulaCodigo = dataTable.rows[0]?.c[7]?.v || "SMINT0326P9A";
                
                document.getElementById('header-subtitle').innerText = `Tutor: ${tutorName} | Código Aula: ${aulaCodigo}`;
                document.getElementById('splash-tutor').innerText = tutorName;

                // 2. Procesamiento de filas de Exámenes (Filas de la 3 a la 11 y de la 12 a la 20)
                totalDataRows = [];
                let tAsistencias = 0, tFaltas = 0, sumNotas = 0, countNotasValidas = 0;

                dataTable.rows.forEach((row, index) => {
                    // Ignorar cabeceras primarias
                    if (index < 2 || !row.c || !row.c[0]) return;
                    
                    let examenKey = (row.c[0].v || "").toString().trim();
                    if (!examenKey.startsWith("EXSA") && !examenKey.startsWith("EXSI")) return;

                    let asistencia = row.c[1] ? parseInt(row.c[1].v) : null;
                    let falta = row.c[2] ? parseInt(row.c[2].v) : null;
                    let notaRaw = row.c[3] ? (row.c[3].f || row.c[3].v.toString()) : "";
                    let variacion = row.c[4] ? (row.c[4].f || row.c[4].v.toString()) : "";
                    let sica = row.c[5] ? parseInt(row.c[5].v) : null;
                    let cd = row.c[6] ? row.c[6].v : "";
                    let cxm = row.c[7] ? row.c[7].v : "";

                    let notaNum = cleanNumber(notaRaw);

                    if (asistencia !== null) tAsistencias += asistencia;
                    if (falta !== null) tFaltas += falta;
                    if (notaNum > 0) {
                        sumNotas += notaNum;
                        countNotasValidas++;
                    }

                    totalDataRows.push({
                        examen: examenKey,
                        asistencia: asistencia,
                        falta: falta,
                        nota: notaNum,
                        notaTexto: notaRaw || "-",
                        variacion: variacion || "-",
                        sica: sica !== null ? sica : "-",
                        cd: cd || "-",
                        cxm: cxm || "-"
                    });
                });

                // 3. Renderizar e inyectar KPIs Globales
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

                // 4. Pintar componentes visuales
                renderTableRows(totalDataRows);
                buildCharts(totalDataRows, tAsistencias, tFaltas);

                // Remover Splash Screen de carga de forma fluida
                setTimeout(() => {
                    const overlay = document.getElementById('welcome-overlay');
                    if (overlay) {
                        overlay.classList.add('opacity-0');
                        setTimeout(() => overlay.remove(), 700);
                    }
                }, 1000);

                document.getElementById('error-box').classList.add('hidden');
            } catch (error) {
                console.error("Error cargando los datos de la API:", error);
                document.getElementById('error-box').classList.remove('hidden');
                const overlay = document.getElementById('welcome-overlay');
                if (overlay) overlay.remove();
            }
        }

        function renderTableRows(data) {
            const tbody = document.getElementById('table-body-notas');
            tbody.innerHTML = '';

            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-850";
                
                let varColor = "text-slate-400";
                if(row.variacion.includes('-')) varColor = "text-rose-400";
                else if(row.variacion !== '-' && row.variacion !== '0.0%' && !row.variacion.includes('#')) varColor = "text-emerald-400";

                tr.innerHTML = `
                    <td class="py-3.5 px-4 font-bold text-white tracking-wide">${row.examen}</td>
                    <td class="py-3.5 px-3 text-center text-emerald-400 bg-emerald-500/5 font-mono">${row.asistencia || '-'}</td>
                    <td class="py-3.5 px-3 text-center text-rose-400 bg-rose-500/5 font-mono">${row.falta !== null ? row.falta : '-'}</td>
                    <td class="py-3.5 px-4 text-right text-cyan-400 font-extrabold font-mono">${row.notaTexto}</td>
                    <td class="py-3.5 px-3 text-center font-bold font-mono ${varColor}">${row.variacion}</td>
                    <td class="py-3.5 px-3 text-center text-slate-300 font-mono">${row.sica}</td>
                    <td class="py-3.5 px-4 text-slate-400 truncate max-w-[140px] font-medium text-[11px]">${row.cd}</td>
                    <td class="py-3.5 px-4 text-slate-400 truncate max-w-[140px] font-medium text-[11px]">${row.cxm}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function filterTable(type) {
            // Manejador estético de botones activos
            ['btn-f-todos', 'btn-f-exsa', 'btn-f-exsi'].forEach(id => {
                document.getElementById(id).className = "px-3 py-1.5 rounded-lg text-xs font-bold bg-slate-800 text-slate-400 hover:bg-slate-700 transition-all";
            });
            document.getElementById(`btn-f-${type.toLowerCase()}`).className = "px-3 py-1.5 rounded-lg text-xs font-bold bg-blue-600 text-white transition-all";

            if (type === 'TODOS') {
                renderTableRows(totalDataRows);
            } else {
                const filtered = totalDataRows.filter(r => r.examen.startsWith(type));
                renderTableRows(filtered);
            }
        }

        function buildCharts(data, asistencias, faltas) {
            // Filtrar únicamente los exámenes que ya tengan puntaje cargado para el gráfico de líneas
            const examenesConNota = data.filter(r => r.nota > 0);

            // 1. Gráfico de Evolución Lineal de Notas
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
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        x: { grid: { color: 'rgba(255, 255, 255, 0.02)' }, ticks: { color: '#94a3b8' } },
                        y: { grid: { color: 'rgba(255, 255, 255, 0.03)' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });

            // 2. Gráfico de Dona para Control Asistencial Colectivo
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
                <span class="inline-block w-2 h-2 rounded-full bg-emerald-500 mr-1"></span> Asistencias (${asistencias}) 
                <span class="inline-block w-2 h-2 rounded-full bg-rose-500 ml-3 mr-1"></span> Faltas (${faltas})
            `;
        }

        // Ejecución e inicialización al cargar
        loadSheetDashboard();
        // Sincronización automática en segundo plano cada 60 segundos
        setInterval(loadSheetDashboard, 60000);
    </script>
</body>
</html>
