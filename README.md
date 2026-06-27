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
        <div class="text-center space-y-6 animate-pulse">
            <div class="inline-flex items-center justify-center w-20 h-20 rounded-2xl bg-gradient-to-tr from-blue-600 to-cyan-500 shadow-[0_0_50px_rgba(14,165,233,0.3)] mb-4">
                <span class="text-4xl text-white font-black">SC</span>
            </div>
            <h2 class="text-2xl font-extrabold text-white tracking-wider uppercase" id="splash-tutor">SEDE COMAS</h2>
            <p class="text-slate-400 font-medium tracking-widest text-xs uppercase">Cargando Analítica en Vivo...</p>
        </div>
    </div>

    <!-- Encabezado Principal -->
    <header class="border-b border-slate-800 bg-slate-900/50 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 h-20 flex items-center justify-between">
            <div class="flex items-center space-x-4">
                <div class="bg-blue-600 px-4 py-2 rounded-xl text-white font-extrabold text-lg tracking-wider">COMAS</div>
                <div>
                    <h1 class="text-sm font-bold text-white tracking-tight uppercase" id="header-title">ANÁLISIS DE NOTAS</h1>
                    <p class="text-[11px] text-cyan-400 font-medium" id="header-subtitle">Tutor: Cargando...</p>
                </div>
            </div>
            <div class="flex items-center space-x-3">
                <span class="flex h-2 w-2 relative">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                </span>
                <span class="text-[10px] font-bold text-emerald-400 bg-emerald-500/10 px-3 py-1.5 rounded-full border border-emerald-500/20 uppercase">Hojas de Cálculo Sincronizadas</span>
            </div>
        </div>
    </header>

    <!-- Caja de Alerta de Error -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 mt-8">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-6 rounded-xl text-xs font-medium">
            ⚠️ <strong>Error de Conexión:</strong> No se pudo sincronizar con la API de Google Sheets. Asegúrate de que el documento esté compartido de forma pública.
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-6 sm:px-8 lg:px-12 py-12 space-y-12">
        
        <!-- KPIs PRINCIPALES (Métricas Consolidadas de la Hoja) -->
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

        <!-- PANELES DE GRÁFICOS Y ANÁLISIS DE TENDENCIAS -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <!-- Evolución Académica -->
            <div class="lg:col-span-2 premium-card rounded-2xl p-8 shadow-xl">
                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Evolución de Puntajes (Notas por Examen)</h3>
                <div class="relative h-72">
                    <canvas id="chartEvolucion"></canvas>
                </div>
            </div>

            <!-- Distribución de Asistencia -->
            <div class="lg:col-span-1 premium-card rounded-2xl p-8 shadow-xl flex flex-col justify-between">
                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-6">Balance General de Asistencia</h3>
                <div class="relative h-56 flex items-center justify-center">
                    <canvas id="chartAsistenciaDoughnut"></canvas>
                </div>
                <div class="text-center text-xs text-slate-400 mt-4" id="chart-doughnut-legend">...</div>
            </div>
        </div>

        <!-- TABLA COMPLETA: REPORTES FILTRADOS Y EXÁMENES -->
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

    </main>

    <script>
        // ... Tu código JS existente permanece intacto aquí
    </script>
</body>
</html>
