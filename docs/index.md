<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Vista de archivos del repositorio</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #0f172a;
      --bg-card: #111827;
      --bg-card-hover: #1f2937;
      --accent: #38bdf8;
      --accent-soft: rgba(56, 189, 248, 0.1);
      --text-main: #e5e7eb;
      --text-muted: #9ca3af;
      --border-soft: #1f2937;
      --radius: 14px;
      --shadow-soft: 0 18px 45px rgba(15, 23, 42, 0.8);
      --transition-fast: 0.18s ease-out;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #1f2937 0, #020617 55%, #000 100%);
      color: var(--text-main);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      padding: 24px;
    }

    .page {
      width: 100%;
      max-width: 1200px;
    }

    header {
      margin-bottom: 24px;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .title-row {
      display: flex;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    h1 {
      font-size: clamp(1.6rem, 3vw, 2rem);
      font-weight: 650;
      letter-spacing: 0.03em;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .badge {
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.12em;
      padding: 4px 10px;
      border-radius: 999px;
      border: 1px solid var(--accent-soft);
      background: rgba(15, 23, 42, 0.7);
      color: var(--accent);
    }

    p.subtitle {
      font-size: 0.9rem;
      color: var(--text-muted);
      max-width: 540px;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
      gap: 18px;
    }

    .card {
      position: relative;
      background: linear-gradient(145deg, #020617, #020617);
      border-radius: var(--radius);
      border: 1px solid var(--border-soft);
      padding: 16px 15px 14px;
      box-shadow: 0 22px 50px rgba(0, 0, 0, 0.6);
      overflow: hidden;
      display: flex;
      flex-direction: column;
      gap: 8px;
      transition: transform var(--transition-fast),
        box-shadow var(--transition-fast),
        border-color var(--transition-fast),
        background var(--transition-fast);
    }

    .card::before {
      content: "";
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at top right, rgba(56, 189, 248, 0.18), transparent 55%);
      opacity: 0;
      transition: opacity var(--transition-fast);
      pointer-events: none;
    }

    .card:hover {
      transform: translateY(-4px) scale(1.01);
      border-color: rgba(56, 189, 248, 0.4);
      background: linear-gradient(145deg, #020617, #020617);
      box-shadow: var(--shadow-soft);
    }

    .card:hover::before {
      opacity: 1;
    }

    .card-header {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .file-icon {
      width: 32px;
      height: 32px;
      border-radius: 12px;
      background: radial-gradient(circle at 30% 0, #38bdf8, #0ea5e9 35%, #020617 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      flex-shrink: 0;
    }

    .file-meta {
      display: flex;
      flex-direction: column;
      gap: 2px;
      min-width: 0;
    }

    .file-name {
      font-size: 0.96rem;
      font-weight: 600;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .file-tag {
      font-size: 0.75rem;
      color: var(--accent);
      opacity: 0.9;
    }

    .card-body {
      font-size: 0.82rem;
      color: var(--text-muted);
      margin-top: 4px;
      min-height: 40px;
    }

    .card-footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
      font-size: 0.78rem;
      color: var(--text-muted);
      gap: 8px;
    }

    .pill {
      padding: 3px 8px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(148, 163, 184, 0.25);
      font-size: 0.72rem;
    }

    .btn-link {
      margin-left: auto;
      text-decoration: none;
      padding: 6px 11px;
      border-radius: 999px;
      font-size: 0.78rem;
      border: 1px solid rgba(56, 189, 248, 0.7);
      color: var(--accent);
      background: radial-gradient(circle at top, rgba(56, 189, 248, 0.1), transparent 60%);
      display: inline-flex;
      align-items: center;
      gap: 5px;
      transition: background var(--transition-fast),
        color var(--transition-fast),
        border-color var(--transition-fast),
        transform 0.12s ease-out;
    }

    .btn-link span {
      font-size: 0.86em;
    }

    .btn-link:hover {
      background: rgba(56, 189, 248, 0.16);
      color: #e0f2fe;
      border-color: var(--accent);
      transform: translateY(-1px);
    }

    .btn-link:active {
      transform: translateY(0);
    }

    @media (max-width: 600px) {
      body {
        padding: 16px;
      }

      header {
        margin-bottom: 18px;
      }

      .card {
        padding: 14px 13px 12px;
      }
    }
  </style>
</head>
<body>
  <main class="page">
    <header>
      <div class="title-row">
        <h1>📂 Archivos del repositorio</h1>
        <span class="badge">Card View</span>
      </div>
      <p class="subtitle">
        Selecciona un archivo para verlo en GitHub. Edita este HTML para apuntar cada tarjeta
        al archivo correspondiente de tu repositorio.
      </p>
    </header>

    <section class="grid">
      <!-- CARD 1 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📄</div>
          <div class="file-meta">
            <div class="file-name">Introducción</div>
            <div class="file-tag">Documentación</div>
          </div>
        </div>
        <div class="card-body">
          Descripción corta del archivo 1. Explica brevemente qué contiene este archivo.
        </div>
        <div class="card-footer">
          <span class="pill">Markdown</span>
          <a class="btn-link" href="https://github.com/ArchimageFenix/AdGuardHomeSecurity/blob/main/docs/00-introduccion.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 2 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">⚙️</div>
          <div class="file-meta">
            <div class="file-name">Público Objetivo</div>
            <div class="file-tag">Configuración</div>
          </div>
        </div>
        <div class="card-body">
          Archivo de configuración principal. Contiene parámetros del proyecto.
        </div>
        <div class="card-footer">
          <span class="pill">YAML</span>
          <a class="btn-link" href="https://github.com/ArchimageFenix/AdGuardHomeSecurity/blob/main/docs/01-publico-objetivo-y-alcance.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 3 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">💻</div>
          <div class="file-meta">
            <div class="file-name">Modelo de Amenazas Analizado</div>
            <div class="file-tag">Threat Model</div>
          </div>
        </div>
        <div class="card-body">
          Script de utilidad para automatizar tareas relacionadas con el proyecto.
        </div>
        <div class="card-footer">
          <span class="pill">Shell</span>
          <a class="btn-link" href="https://github.com/ArchimageFenix/AdGuardHomeSecurity/blob/main/docs/02-threat-model.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 4 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🧪</div>
          <div class="file-meta">
            <div class="file-name">tests.md</div>
            <div class="file-tag">Notas</div>
          </div>
        </div>
        <div class="card-body">
          Notas de pruebas, resultados y observaciones técnicas relevantes.
        </div>
        <div class="card-footer">
          <span class="pill">Notas</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/tests.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 5 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📦</div>
          <div class="file-meta">
            <div class="file-name">Dockerfile</div>
            <div class="file-tag">Contenedor</div>
          </div>
        </div>
        <div class="card-body">
          Definición de la imagen Docker necesaria para ejecutar el proyecto.
        </div>
        <div class="card-footer">
          <span class="pill">Docker</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/Dockerfile" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 6 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📘</div>
          <div class="file-meta">
            <div class="file-name">README.md</div>
            <div class="file-tag">Principal</div>
          </div>
        </div>
        <div class="card-body">
          Información general del proyecto, instrucciones de uso e instalación.
        </div>
        <div class="card-footer">
          <span class="pill">Markdown</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO#readme" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 7 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🧩</div>
          <div class="file-meta">
            <div class="file-name">adguard.conf</div>
            <div class="file-tag">AdGuard</div>
          </div>
        </div>
        <div class="card-body">
          Configuración específica para AdGuard Home en este entorno.
        </div>
        <div class="card-footer">
          <span class="pill">Config</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/adguard.conf" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 8 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🛡️</div>
          <div class="file-meta">
            <div class="file-name">fail2ban.conf</div>
            <div class="file-tag">Seguridad</div>
          </div>
        </div>
        <div class="card-body">
          Reglas personalizadas de Fail2Ban para proteger tu servidor DNS cifrado.
        </div>
        <div class="card-footer">
          <span class="pill">Fail2Ban</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/fail2ban.conf" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 9 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📄</div>
          <div class="file-meta">
            <div class="file-name">ipset-rules.txt</div>
            <div class="file-tag">Listas</div>
          </div>
        </div>
        <div class="card-body">
          Conjunto de reglas IPSet utilizadas para bloqueo y filtrado avanzado.
        </div>
        <div class="card-footer">
          <span class="pill">IPSet</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/ipset-rules.txt" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 10 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📑</div>
          <div class="file-meta">
            <div class="file-name">changelog.md</div>
            <div class="file-tag">Historial</div>
          </div>
        </div>
        <div class="card-body">
          Registro de cambios, versiones y ajustes realizados en el tiempo.
        </div>
        <div class="card-footer">
          <span class="pill">Log</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/changelog.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 11 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📊</div>
          <div class="file-meta">
            <div class="file-name">metrics.md</div>
            <div class="file-tag">Métricas</div>
          </div>
        </div>
        <div class="card-body">
          Resumen de métricas, estadísticas o resultados relevantes del sistema.
        </div>
        <div class="card-footer">
          <span class="pill">Info</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/metrics.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 12 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🧾</div>
          <div class="file-meta">
            <div class="file-name">notes.md</div>
            <div class="file-tag">Notas</div>
          </div>
        </div>
        <div class="card-body">
          Notas personales o de mantenimiento relacionadas con este repositorio.
        </div>
        <div class="card-footer">
          <span class="pill">Notas</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/notes.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 13 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🧰</div>
          <div class="file-meta">
            <div class="file-name">tools.md</div>
            <div class="file-tag">Herramientas</div>
          </div>
        </div>
        <div class="card-body">
          Listado de herramientas, comandos y utilidades usadas en el entorno.
        </div>
        <div class="card-footer">
          <span class="pill">Docs</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/tools.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 14 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">📝</div>
          <div class="file-meta">
            <div class="file-name">todo.md</div>
            <div class="file-tag">Pendientes</div>
          </div>
        </div>
        <div class="card-body">
          Lista de tareas pendientes, ideas y mejoras planificadas.
        </div>
        <div class="card-footer">
          <span class="pill">Tareas</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/todo.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>

      <!-- CARD 15 -->
      <article class="card">
        <div class="card-header">
          <div class="file-icon">🧱</div>
          <div class="file-meta">
            <div class="file-name">extra.md</div>
            <div class="file-tag">Extra</div>
          </div>
        </div>
        <div class="card-body">
          Archivo adicional para cualquier contenido extra que quieras exponer.
        </div>
        <div class="card-footer">
          <span class="pill">Extra</span>
          <a class="btn-link" href="https://github.com/TU_USUARIO/TU_REPO/blob/main/extra.md" target="_blank" rel="noopener noreferrer">
            <span>Ver archivo</span> ↗
          </a>
        </div>
      </article>
    </section>
  </main>
</body>
</html>
