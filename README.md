<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>Oficina - Abogado Miguel Angel Ramirez Mogollon</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:ital,wght@0,100..900;1,100..900&family=Playfair+Display:ital,wght@0,400..900;1,400..900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        /* Modo Oscuro (default) */
        :root {
            --bg-primary: #000000;
            --bg-secondary: #0A0A0A;
            --bg-card: #111111;
            --bg-card-hover: #1A1A1A;
            --bg-modal: #1A1A1A;
            --text-primary: #FFFFFF;
            --text-secondary: #CCCCCC;
            --text-muted: #888888;
            --border-color: #333333;
            --border-light: #444444;
            --accent: #FFFFFF;
            --accent-secondary: #EAEAEA;
            --shadow: rgba(0, 0, 0, 0.5);
            --input-bg: #222222;
            --input-border: #444444;
            --success: #00FF00;
            --error: #FF3333;
            --warning: #FFA500;
            --info: #FFFFFF;
        }

        /* Modo Claro */
        body.light {
            --bg-primary: #FFFFFF;
            --bg-secondary: #F5F5F5;
            --bg-card: #FAFAFA;
            --bg-card-hover: #F0F0F0;
            --bg-modal: #FFFFFF;
            --text-primary: #000000;
            --text-secondary: #333333;
            --text-muted: #666666;
            --border-color: #E0E0E0;
            --border-light: #CCCCCC;
            --accent: #000000;
            --accent-secondary: #222222;
            --shadow: rgba(0, 0, 0, 0.1);
            --input-bg: #FFFFFF;
            --input-border: #CCCCCC;
            --success: #008000;
            --error: #FF0000;
            --warning: #FF8C00;
            --info: #000000;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg-primary);
            min-height: 100vh;
            padding: 24px;
            transition: all 0.3s ease;
            color: var(--text-primary);
        }

        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            background: var(--bg-secondary);
            border-radius: 48px;
            border: 1px solid var(--border-color);
            overflow: hidden;
            box-shadow: 0 25px 50px -12px var(--shadow);
        }

        @media (min-width: 1024px) {
            .app-wrapper { display: flex; min-height: 85vh; }
            .content-area { flex: 1; display: flex; flex-direction: column; }
            .bottom-nav { flex-direction: column !important; width: 280px; border-right: 1px solid var(--border-color); justify-content: center; gap: 12px !important; padding: 32px 20px !important; }
            .nav-item { display: flex !important; align-items: center; gap: 16px; padding: 16px 20px !important; }
            .nav-item i { margin-bottom: 0 !important; font-size: 24px; }
            .screen { padding: 32px !important; }
            #listaClientes { max-height: calc(100vh - 250px); overflow-y: auto; }
            .ia-chat-container { max-height: calc(100vh - 280px); }
        }

        @media (max-width: 600px) {
            body { padding: 12px; }
            .screen { padding: 16px; }
            #listaClientes { max-height: calc(100vh - 300px); overflow-y: auto; }
        }

        .header {
            background: var(--bg-secondary);
            padding: 28px 24px;
            text-align: center;
            border-bottom: 1px solid var(--border-color);
            position: relative;
        }

        .header h1 {
            font-size: 22px;
            font-weight: 700;
            color: var(--text-primary);
            font-family: 'Playfair Display', serif;
            letter-spacing: 2px;
        }

        .header .nombre-abogado {
            font-size: 13px;
            color: var(--text-secondary);
            font-weight: 500;
            letter-spacing: 1px;
            margin-top: 5px;
        }

        .theme-switch-wrapper {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .theme-switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 30px;
        }
        .theme-switch input { opacity: 0; width: 0; height: 0; }
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: var(--border-color);
            border-radius: 30px;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 22px;
            width: 22px;
            left: 4px;
            bottom: 4px;
            background-color: var(--accent);
            border-radius: 50%;
        }
        input:checked + .slider { background-color: var(--accent-secondary); }
        input:checked + .slider:before { transform: translateX(30px); background-color: var(--bg-primary); }

        .badge-header {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: var(--bg-card);
            padding: 6px 14px;
            border-radius: 100px;
            font-size: 10px;
            margin-top: 12px;
            color: var(--text-secondary);
            border: 1px solid var(--border-color);
        }

        .bottom-nav {
            display: flex;
            background: var(--bg-secondary);
            border-top: 1px solid var(--border-color);
            padding: 12px 20px;
            gap: 8px;
        }

        .nav-item {
            flex: 1;
            text-align: center;
            padding: 12px 8px;
            cursor: pointer;
            border-radius: 24px;
            color: var(--text-secondary);
        }
        .nav-item i { font-size: 22px; display: block; margin-bottom: 6px; }
        .nav-item span { font-size: 11px; font-weight: 500; }
        .nav-item.active {
            background: var(--bg-card);
            color: var(--text-primary);
            border: 1px solid var(--border-color);
        }

        .screen { display: none; padding: 24px; animation: fadeSlideUp 0.4s ease; min-height: 520px; }
        .screen.active { display: block; }
        @keyframes fadeSlideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .card {
            background: var(--bg-card);
            border-radius: 28px;
            padding: 20px;
            margin-bottom: 20px;
            border: 1px solid var(--border-color);
        }
        .card:hover { transform: translateY(-4px); background: var(--bg-card-hover); }

        .btn {
            background: var(--accent);
            color: var(--bg-primary);
            border: none;
            padding: 14px 24px;
            border-radius: 100px;
            font-size: 14px;
            font-weight: 700;
            cursor: pointer;
            width: 100%;
        }
        .btn:hover { transform: translateY(-2px); opacity: 0.9; }
        .btn-outline {
            background: transparent;
            border: 1.5px solid var(--border-color);
            color: var(--text-primary);
        }
        .btn-outline:hover { background: var(--bg-card-hover); }

        .form-group { margin-bottom: 20px; }
        .form-group label { display: block; margin-bottom: 8px; font-weight: 600; color: var(--text-secondary); font-size: 12px; }
        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 14px 16px;
            background: var(--input-bg);
            border: 1.5px solid var(--border-color);
            border-radius: 20px;
            font-size: 14px;
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
        }
        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
            outline: none;
            border-color: var(--text-primary);
        }

        /* IA Styles */
        .ia-container {
            background: var(--bg-card);
            border-radius: 28px;
            border: 1px solid var(--border-color);
            overflow: hidden;
        }
        .ia-header {
            background: var(--bg-secondary);
            padding: 16px 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 12px;
            flex-wrap: wrap;
        }
        .ia-header i { font-size: 28px; color: var(--text-primary); }
        .ia-header h3 { font-size: 18px; font-weight: 600; color: var(--text-primary); }
        .ia-badge {
            background: var(--bg-card-hover);
            padding: 4px 10px;
            border-radius: 100px;
            font-size: 10px;
            color: var(--text-secondary);
            border: 1px solid var(--border-color);
        }
        .ia-chat-container {
            height: 450px;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }
        .ia-message { display: flex; gap: 12px; }
        .ia-message.user { flex-direction: row-reverse; }
        .ia-avatar {
            width: 36px; height: 36px;
            border-radius: 50%;
            background: var(--bg-card-hover);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--text-primary);
        }
        .ia-bubble {
            max-width: 75%;
            padding: 12px 16px;
            border-radius: 20px;
            font-size: 14px;
        }
        .ia-message.user .ia-bubble {
            background: var(--accent);
            color: var(--bg-primary);
            border-radius: 20px 20px 4px 20px;
        }
        .ia-message.bot .ia-bubble {
            background: var(--bg-card-hover);
            border: 1px solid var(--border-color);
            color: var(--text-primary);
            border-radius: 20px 20px 20px 4px;
        }
        .ia-input-container {
            padding: 16px 20px;
            border-top: 1px solid var(--border-color);
            display: flex;
            gap: 12px;
        }
        .ia-input-container input {
            flex: 1;
            padding: 14px 16px;
            background: var(--input-bg);
            border: 1px solid var(--border-color);
            border-radius: 100px;
            color: var(--text-primary);
        }
        .ia-input-container button {
            width: auto;
            padding: 14px 24px;
            background: var(--accent);
            border: none;
            border-radius: 100px;
            color: var(--bg-primary);
            font-weight: 600;
            cursor: pointer;
        }
        .ia-suggestions {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            padding: 12px 20px;
            border-top: 1px solid var(--border-color);
        }
        .ia-suggestion {
            background: var(--bg-card-hover);
            border: 1px solid var(--border-color);
            border-radius: 100px;
            padding: 6px 14px;
            font-size: 11px;
            color: var(--text-secondary);
            cursor: pointer;
        }
        .ia-suggestion:hover { background: var(--border-color); color: var(--text-primary); }

        /* Client styles */
        .cliente-item {
            background: var(--bg-card);
            border-radius: 24px;
            padding: 16px;
            margin-bottom: 12px;
            border: 1px solid var(--border-color);
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .cliente-item:hover { transform: translateX(8px); background: var(--bg-card-hover); }
        .cliente-info h3 { font-size: 15px; font-weight: 700; color: var(--text-primary); margin-bottom: 6px; }
        .cliente-info p { font-size: 11px; color: var(--text-secondary); }
        .rama-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 100px;
            font-size: 10px;
            font-weight: 600;
            background: var(--bg-card-hover);
            color: var(--text-secondary);
            border: 1px solid var(--border-color);
        }
        .delete-btn {
            background: var(--bg-card-hover);
            border: 1px solid var(--border-color);
            width: 38px; height: 38px;
            border-radius: 19px;
            cursor: pointer;
            color: var(--error);
        }
        .delete-btn:hover { background: var(--error); color: white; }

        /* Calendar */
        .calendario-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 24px; }
        .calendario-dias { display: grid; grid-template-columns: repeat(7, 1fr); gap: 8px; margin-bottom: 12px; }
        .dia-nombre { text-align: center; font-weight: 600; padding: 8px; font-size: 11px; color: var(--text-secondary); }
        .calendario-fechas { display: grid; grid-template-columns: repeat(7, 1fr); gap: 8px; }
        .fecha {
            text-align: center; padding: 10px; border-radius: 16px; cursor: pointer; font-size: 12px;
            color: var(--text-secondary);
        }
        .fecha:hover { background: var(--bg-card-hover); }
        .fecha.selected { background: var(--accent); color: var(--bg-primary); font-weight: 700; }
        .fecha.has-event { background: var(--bg-card-hover); color: var(--text-primary); border: 1px solid var(--border-color); }

        /* Stats */
        .stats-card {
            background: var(--bg-card);
            border-radius: 28px;
            padding: 24px;
            text-align: center;
            margin-bottom: 20px;
        }
        .stats-number {
            font-size: 52px;
            font-weight: 800;
            color: var(--text-primary);
            font-family: 'Playfair Display', serif;
        }
        .barra-progreso { background: var(--border-color); border-radius: 100px; overflow: hidden; margin: 12px 0; }
        .barra { background: var(--accent); height: 32px; line-height: 32px; color: var(--bg-primary); padding: 0 12px; font-size: 12px; font-weight: 700; }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.85);
            backdrop-filter: blur(12px);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        .modal.active { display: flex; }
        .modal-content {
            background: var(--bg-modal);
            border-radius: 32px;
            padding: 28px;
            max-width: 90%;
            width: 400px;
            border: 1px solid var(--border-color);
        }
        .modal-content h3 { color: var(--text-primary); margin-bottom: 20px; }

        .sync-status {
            position: fixed;
            bottom: 20px; right: 20px;
            background: var(--bg-card);
            padding: 8px 16px;
            border-radius: 100px;
            font-size: 11px;
            color: var(--text-secondary);
            display: flex;
            align-items: center;
            gap: 8px;
            border: 1px solid var(--border-color);
        }
        .syncing { animation: spin 1s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }

        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%);
            padding: 12px 24px;
            border-radius: 100px;
            font-size: 13px;
            font-weight: 600;
            z-index: 2000;
            background: var(--accent);
            color: var(--bg-primary);
        }
    </style>
</head>
<body>
    <div class="app-container">
        <div class="app-wrapper">
            <div class="bottom-nav">
                <div class="nav-item" onclick="cambiarPantalla('clientes', this)">
                    <i class="fas fa-users"></i><span>Clientes</span>
                </div>
                <div class="nav-item" onclick="cambiarPantalla('calendario', this)">
                    <i class="fas fa-calendar-alt"></i><span>Calendario</span>
                </div>
                <div class="nav-item" onclick="cambiarPantalla('estadisticas', this)">
                    <i class="fas fa-chart-line"></i><span>Analytics</span>
                </div>
                <div class="nav-item active" onclick="cambiarPantalla('ia', this)">
                    <i class="fas fa-robot"></i><span>IA Ejecutora</span>
                </div>
            </div>

            <div class="content-area">
                <div class="header">
                    <div class="theme-switch-wrapper">
                        <i class="fas fa-sun"></i>
                        <label class="theme-switch">
                            <input type="checkbox" id="themeToggle" onchange="toggleTheme()">
                            <span class="slider"></span>
                        </label>
                        <i class="fas fa-moon"></i>
                    </div>
                    <h1>OFICINA</h1>
                    <div class="nombre-abogado">ABOGADO MIGUEL ANGEL RAMIREZ MOGOLLON</div>
                    <p>Gestión Legal Profesional | IA Ejecutora Integrada</p>
                    <div class="badge-header">
                        <i class="fas fa-brain"></i> IA que EJECUTA acciones
                        <i class="fas fa-cloud-upload-alt"></i> Sincronización en Nube
                    </div>
                </div>

                <div id="screen-clientes" class="screen">
                    <button class="btn" onclick="mostrarFormulario()" id="btnMostrarForm">
                        <i class="fas fa-plus-circle"></i> Nuevo Cliente
                    </button>
                    <div id="formulario" style="display: none; margin-top: 20px;">
                        <div class="card">
                            <h3><i class="fas fa-user-astronaut"></i> Registrar Cliente</h3>
                            <div class="form-group"><label>Tipo de cliente</label><select id="tipoCliente"><option value="natural">👤 Persona Natural</option><option value="empresa">🏢 Empresa</option></select></div>
                            <div class="form-group"><label>Nombre / Razón Social *</label><input type="text" id="nombreCliente" placeholder="Ej: Juan Pérez"></div>
                            <div class="form-group"><label>Teléfono / WhatsApp</label><input type="tel" id="telefonoCliente" placeholder="Ej: +57 300 123 4567"></div>
                            <div class="form-group"><label>Email</label><input type="email" id="emailCliente" placeholder="juan@estudiolegal.com"></div>
                            <div class="form-group"><label>Dirección</label><input type="text" id="direccionCliente" placeholder="Dirección física"></div>
                            <div class="form-group"><label>Rama del derecho</label><select id="ramaCliente"><option value="Civil">⚖️ Civil</option><option value="Laboral">👔 Laboral</option><option value="Datacrédito">📊 Datacrédito</option><option value="Penal">🔒 Penal</option><option value="Familia">👨‍👩‍👧 Familia</option></select></div>
                            <button class="btn" onclick="agregarCliente(this)"><i class="fas fa-save"></i> Guardar Cliente</button>
                            <button class="btn btn-outline" onclick="cerrarFormulario()" style="margin-top: 12px;"><i class="fas fa-times"></i> Cancelar</button>
                        </div>
                    </div>
                    <div id="listaClientes" style="margin-top: 20px;"></div>
                </div>

                <div id="screen-calendario" class="screen">
                    <div class="card">
                        <div class="calendario-header">
                            <button class="btn-outline" style="width: auto; padding: 10px 18px;" onclick="cambiarMes(-1)"><i class="fas fa-chevron-left"></i></button>
                            <h3 id="mesActual">Cargando...</h3>
                            <button class="btn-outline" style="width: auto; padding: 10px 18px;" onclick="cambiarMes(1)"><i class="fas fa-chevron-right"></i></button>
                        </div>
                        <div class="calendario-dias">
                            <div class="dia-nombre">L</div><div class="dia-nombre">M</div><div class="dia-nombre">M</div>
                            <div class="dia-nombre">J</div><div class="dia-nombre">V</div><div class="dia-nombre">S</div>
                            <div class="dia-nombre">D</div>
                        </div>
                        <div id="calendarioFechas" class="calendario-fechas"></div>
                    </div>
                    <div class="card">
                        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px;">
                            <h3><i class="fas fa-calendar-day"></i> Eventos del día</h3>
                            <button class="btn-outline" style="width: auto; padding: 8px 16px;" onclick="mostrarModalEvento()"><i class="fas fa-plus"></i> Agregar</button>
                        </div>
                        <div id="listaEventos"></div>
                    </div>
                </div>

                <div id="screen-estadisticas" class="screen">
                    <div class="stats-card">
                        <div class="stats-number" id="totalClientes">0</div>
                        <div class="stats-label">Clientes Registrados</div>
                    </div>
                    <div class="card"><h3><i class="fas fa-chart-pie"></i> Distribución por Rama</h3><div id="statsRamas"></div></div>
                    <div class="card"><h3><i class="fas fa-chart-line"></i> Métricas Legales</h3>
                        <div class="barra-progreso"><div class="barra" id="barraActivos" style="width: 0%;">Casos Activos</div></div>
                        <div class="barra-progreso"><div class="barra" id="barraCerrados" style="width: 0%; background: var(--success);">Casos Cerrados</div></div>
                    </div>
                    <div class="card">
                        <button class="btn btn-outline" onclick="exportarReporte()"><i class="fas fa-file-pdf"></i> Exportar Reporte</button>
                        <button class="btn btn-outline" onclick="compartirDatos()" style="margin-top: 12px;"><i class="fas fa-share-alt"></i> Compartir Datos</button>
                    </div>
                </div>

                <div id="screen-ia" class="screen active">
                    <div class="ia-container">
                        <div class="ia-header">
                            <i class="fas fa-robot"></i>
                            <h3>LexIA Ejecutora</h3>
                            <span class="ia-badge"><i class="fas fa-bolt"></i> Puedo EJECUTAR acciones</span>
                        </div>
                        <div class="ia-chat-container" id="iaChatContainer">
                            <div class="ia-message bot">
                                <div class="ia-avatar"><i class="fas fa-robot"></i></div>
                                <div class="ia-bubble">
                                    <strong>🤖 LexIA Ejecutora:</strong><br>
                                    Buenos días, Dr. Ramirez. Soy su asistente legal con capacidad de ejecutar acciones.<br><br>
                                    <strong>✅ Puedo hacer:</strong><br>
                                    📝 Crear clientes | 📅 Crear recordatorios | 📋 Registrar seguimientos<br>
                                    🔍 Consultar clientes | 🗑️ Eliminar clientes | 📊 Generar reportes<br><br>
                                    <strong>🔊 Ejemplos:</strong><br>
                                    • "Crea un cliente llamado Juan Pérez, teléfono 3001234567, rama Civil"<br>
                                    • "Agenda una reunión para mañana a las 10am"<br>
                                    • "Muéstrame todos mis clientes"<br><br>
                                    ¿En qué puedo ayudarle hoy?
                                </div>
                            </div>
                        </div>
                        <div class="ia-input-container">
                            <input type="text" id="iaInput" placeholder="Ej: Crea un cliente llamado Ana García, teléfono 3001234567" onkeypress="if(event.key==='Enter') enviarConsulta()">
                            <button onclick="enviarConsulta()"><i class="fas fa-paper-plane"></i> Ejecutar</button>
                        </div>
                        <div class="ia-suggestions">
                            <span class="ia-suggestion" onclick="enviarSugerencia('Crea un cliente llamado Pedro Ortiz, teléfono 3112223344, rama Laboral')">📝 Crear cliente</span>
                            <span class="ia-suggestion" onclick="enviarSugerencia('Agenda un recordatorio para mañana a las 9am: Audiencia virtual')">📅 Recordatorio</span>
                            <span class="ia-suggestion" onclick="enviarSugerencia('Muéstrame todos mis clientes')">👥 Ver clientes</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="sync-status" id="syncStatus"><i class="fas fa-cloud"></i><span id="syncText">Conectado</span></div>

    <div id="modalEvento" class="modal"><div class="modal-content">
        <h3><i class="fas fa-calendar-plus"></i> Nuevo Evento</h3>
        <div class="form-group"><label>Título</label><input type="text" id="tituloEvento" placeholder="Ej: Audiencia Virtual"></div>
        <div class="form-group"><label>Descripción</label><textarea id="descripcionEvento" rows="3"></textarea></div>
        <div class="form-group"><label>Hora</label><input type="time" id="horaEvento" value="10:00"></div>
        <div class="form-group"><label>Recordatorio</label><select id="recordatorioEvento"><option value="0">Sin</option><option value="15">15 min</option><option value="30">30 min</option></select></div>
        <button class="btn" onclick="agregarEvento(this)"><i class="fas fa-check"></i> Agendar</button>
        <button class="btn btn-outline" onclick="cerrarModalEvento()" style="margin-top: 12px;">Cancelar</button>
    </div></div>

    <div id="modalDetalle" class="modal"><div class="modal-content">
        <h3 id="detalleNombre"></h3>
        <div id="detalleInfo"></div>
        <div class="form-group"><label>Seguimiento</label><select id="tipoSeguimiento"><option value="llamada">📞 Llamada</option><option value="reunion">🤝 Reunión</option><option value="nota">📝 Nota</option></select></div>
        <div class="form-group"><label>Comentario</label><textarea id="comentarioSeguimiento" rows="3"></textarea></div>
        <button class="btn" onclick="agregarSeguimiento(this)"><i class="fas fa-history"></i> Registrar</button>
        <button class="btn btn-outline" onclick="cerrarModalDetalle()" style="margin-top: 12px;">Cerrar</button>
    </div></div>

    <script>
        // Firebase Config
        const firebaseConfig = {
            apiKey: "AIzaSyAmBYTnWvoc9HSDVC0erqRfx8dS9y8lFKQ",
            authDomain: "oficina-aabb5.firebaseapp.com",
            projectId: "oficina-aabb5",
            storageBucket: "oficina-aabb5.firebasestorage.app",
            messagingSenderId: "975925470653",
            appId: "1:975925470653:web:dc216f3057fb55db95ded4"
        };

        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        
        let clientes = [];
        let eventos = [];
        let seguimientos = [];
        let fechaSeleccionada = new Date();
        let mesActual = new Date();
        let clienteSeleccionado = null;
        let primerCarga = true;
        let ultimaSincronizacion = 0;
        let iaEsperandoRespuesta = false;

        // Sincronización
        async function guardarClientesEnNube() {
            const ahora = Date.now();
            if (ahora - ultimaSincronizacion < 500) return;
            ultimaSincronizacion = ahora;
            mostrarSincronizando();
            try {
                await db.collection('clientes_abogado').doc('todos_clientes').set({ data: clientes });
                mostrarConectado();
            } catch (error) { mostrarError(error); }
        }

        async function guardarEventosEnNube() {
            try { await db.collection('eventos_abogado').doc('todos_eventos').set({ data: eventos }); } 
            catch (error) { console.error(error); }
        }

        async function guardarSeguimientosEnNube() {
            try { await db.collection('seguimientos_abogado').doc('todos_seguimientos').set({ data: seguimientos }); } 
            catch (error) { console.error(error); }
        }

        async function cargarDatosDesdeNube() {
            mostrarSincronizando();
            try {
                const clientesDoc = await db.collection('clientes_abogado').doc('todos_clientes').get();
                clientes = clientesDoc.exists ? clientesDoc.data().data || [] : [];
                const eventosDoc = await db.collection('eventos_abogado').doc('todos_eventos').get();
                eventos = eventosDoc.exists ? eventosDoc.data().data || [] : [];
                const seguimientosDoc = await db.collection('seguimientos_abogado').doc('todos_seguimientos').get();
                seguimientos = seguimientosDoc.exists ? seguimientosDoc.data().data
