<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>RET.petition - Quejas</title>
  <style>
    :root {
      --bg: #050816;
      --surface: rgba(16, 26, 52, 0.88);
      --surface-strong: rgba(255, 255, 255, 0.08);
      --accent: #36d1dc;
      --accent-strong: #0f98c7;
      --text: #f4f7fb;
      --muted: #94a6c5;
      --border: rgba(255, 255, 255, 0.13);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      background: radial-gradient(circle at top left, rgba(54, 209, 220, 0.14), transparent 30%),
                  radial-gradient(circle at bottom right, rgba(147, 89, 255, 0.12), transparent 28%),
                  linear-gradient(180deg, #050816 0%, #081022 100%);
      color: var(--text);
      font-family: "Inter", system-ui, sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem;
      overflow-x: hidden;
    }

    .app {
      width: min(100%, 960px);
    }

    .scene {
      perspective: 1400px;
    }

    .card {
      position: relative;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 32px;
      padding: 2rem;
      box-shadow: 0 40px 100px rgba(0, 0, 0, 0.35);
      transform-style: preserve-3d;
      transition: transform 0.35s ease, box-shadow 0.35s ease, border-color 0.35s ease;
      overflow: hidden;
      backdrop-filter: blur(18px);
    }

    .card:hover {
      border-color: rgba(54, 209, 220, 0.24);
      box-shadow: 0 48px 130px rgba(0, 0, 0, 0.43);
    }

    .glow-ring,
    .glow-ring--small {
      position: absolute;
      border-radius: 50%;
      filter: blur(30px);
      pointer-events: none;
      opacity: 0.8;
    }

    .glow-ring {
      width: 260px;
      height: 260px;
      top: -80px;
      right: -90px;
      background: rgba(54, 209, 220, 0.24);
    }

    .glow-ring--small {
      width: 180px;
      height: 180px;
      bottom: -70px;
      left: -80px;
      background: rgba(147, 89, 255, 0.2);
    }

    .header {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin-bottom: 1.8rem;
    }

    .logo {
      width: 58px;
      height: 58px;
      border-radius: 18px;
      display: grid;
      place-items: center;
      background: linear-gradient(135deg, #36d1dc, #1a85b5);
      box-shadow: 0 18px 40px rgba(54, 209, 220, 0.2);
      color: #04121a;
      font-size: 1.45rem;
      font-weight: 800;
    }

    .title-block {
      flex: 1;
    }

    .title-block h1 {
      font-size: clamp(2rem, 2.4vw, 2.5rem);
      line-height: 1.05;
      letter-spacing: -0.03em;
      margin-bottom: 0.4rem;
    }

    .title-block p {
      color: var(--muted);
      line-height: 1.75;
      font-size: 0.98rem;
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
      padding: 0.65rem 1rem;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.12);
      color: rgba(255,255,255,0.86);
      font-size: 0.8rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      background: rgba(255,255,255,0.04);
      margin-bottom: 1.6rem;
    }

    .form-group {
      margin-bottom: 1.45rem;
      text-align: left;
    }

    .form-group label {
      display: inline-block;
      margin-bottom: 0.75rem;
      color: rgba(255,255,255,0.82);
      font-weight: 600;
    }

    textarea {
      width: 100%;
      min-height: 180px;
      border-radius: 24px;
      border: 1px solid rgba(255,255,255,0.1);
      background: rgba(255, 255, 255, 0.05);
      padding: 1.2rem 1.3rem;
      color: #f5f9ff;
      font-size: 1rem;
      line-height: 1.7;
      resize: vertical;
      outline: none;
      transition: border-color 0.24s ease, box-shadow 0.24s ease, transform 0.24s ease;
    }

    textarea:focus {
      border-color: rgba(54, 209, 220, 0.75);
      box-shadow: 0 0 0 6px rgba(54, 209, 220, 0.12);
      transform: translateY(-1px);
    }

    .helper {
      color: var(--muted);
      font-size: 0.88rem;
      margin-top: 0.65rem;
      text-align: right;
    }

    .actions {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
      flex-wrap: wrap;
    }

    .submit-btn {
      border: none;
      border-radius: 999px;
      background: linear-gradient(135deg, var(--accent), var(--accent-strong));
      color: #07101a;
      padding: 0.95rem 1.8rem;
      cursor: pointer;
      font-weight: 700;
      font-size: 1rem;
      transition: transform 0.2s ease, filter 0.2s ease;
      box-shadow: 0 18px 30px rgba(54, 209, 220, 0.18);
    }

    .submit-btn:hover {
      transform: translateY(-2px);
      filter: brightness(1.05);
    }

    .status-card {
      margin-top: 1.6rem;
      display: none;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
      padding: 1rem 1.2rem;
      border-radius: 24px;
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(54, 209, 220, 0.12);
      color: #e9f3ff;
    }

    .status-card.active {
      display: flex;
    }

    .status-circle {
      width: 52px;
      height: 52px;
      border-radius: 50%;
      display: grid;
      place-items: center;
      background: rgba(54, 209, 220, 0.16);
      color: var(--accent);
      font-size: 1.4rem;
    }

    .status-text {
      text-align: left;
    }

    .status-text strong {
      display: block;
      font-size: 1rem;
      margin-bottom: 0.25rem;
      color: #ffffff;
    }

    .status-text span {
      color: var(--muted);
      font-size: 0.92rem;
    }

    @media (max-width: 720px) {
      .card {
        padding: 1.55rem;
      }
      .header {
        flex-direction: column;
        align-items: flex-start;
      }
      .actions {
        flex-direction: column;
        align-items: stretch;
      }
      .submit-btn {
        width: 100%;
      }
      .status-card {
        flex-direction: column;
        align-items: stretch;
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="scene" id="scene">
      <div class="card" id="card">
        <div class="glow-ring"></div>
        <div class="glow-ring--small"></div>

        <div class="header">
          <div class="logo">R</div>
          <div class="title-block">
            <div class="badge">RET.petition</div>
            <h1>Deja tu queja de forma clara y segura</h1>
            <p>Escribe lo que deseas reportar sin pedir datos personales. Tu voz importa.</p>
          </div>
        </div>

        <div class="form-group">
          <label for="complaint">Tu queja</label>
          <textarea id="complaint" placeholder="Describe el problema, qué pasó y qué esperas que se mejore..."></textarea>
          <div class="helper"><span id="count">0</span>/500 caracteres</div>
        </div>

        <div class="actions">
          <div class="helper">No se solicita nombre, número ni dirección.</div>
          <button class="submit-btn" id="submitBtn">Enviar queja</button>
        </div>

        <div class="status-card" id="statusCard">
          <div class="status-circle">✓</div>
          <div class="status-text">
            <strong>Queja enviada</strong>
            <span>Gracias por expresar tu opinión. Tu mensaje se guardó correctamente.</span>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const scene = document.getElementById('scene');
    const card = document.getElementById('card');
    const textarea = document.getElementById('complaint');
    const submitBtn = document.getElementById('submitBtn');
    const statusCard = document.getElementById('statusCard');
    const count = document.getElementById('count');

    scene.addEventListener('mousemove', (event) => {
      const rect = scene.getBoundingClientRect();
      const x = event.clientX - rect.left - rect.width / 2;
      const y = event.clientY - rect.top - rect.height / 2;
      const rotateX = (y / rect.height) * 18;
      const rotateY = (x / rect.width) * 24;
      card.style.transform = `rotateX(${ -rotateX }deg) rotateY(${ rotateY }deg)`;
    });

    scene.addEventListener('mouseleave', () => {
      card.style.transform = 'rotateX(0deg) rotateY(0deg)';
    });

    textarea.addEventListener('input', () => {
      count.textContent = textarea.value.length;
      if (textarea.value.length > 500) {
        textarea.value = textarea.value.slice(0, 500);
        count.textContent = 500;
      }
    });

    submitBtn.addEventListener('click', () => {
      const text = textarea.value.trim();
      if (!text) {
        textarea.focus();
        textarea.style.borderColor = '#ff5c83';
        setTimeout(() => textarea.style.borderColor = 'rgba(255, 255, 255, 0.1)', 1500);
        return;
      }
      statusCard.classList.add('active');
      textarea.value = '';
      count.textContent = '0';
      textarea.style.borderColor = 'rgba(255, 255, 255, 0.1)';
    });
  </script>
</body>
</html>
