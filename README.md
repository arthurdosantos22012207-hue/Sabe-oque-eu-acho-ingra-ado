# Sabe-oque-eu-acho-ingra-ado
Site de amor de elielson,arthur
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>sabe oque eu acho engraçado</title>
  <style>
    :root {
      --bg: #000;
      --fg: #fff;
      --accent: #ff6fa3;
      --muted: rgba(255,255,255,0.15);
      --card-bg: rgba(255,255,255,0.03);
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: var(--bg);
      color: var(--fg);
      font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      line-height: 1.4;
      padding: 24px;
    }

    header {
      text-align: center;
      margin-bottom: 20px;
    }
    h1 {
      margin: 0;
      font-size: 1.6rem;
      letter-spacing: 0.6px;
    }
    p.lead {
      margin: 6px 0 0;
      color: rgba(255,255,255,0.7);
      font-size: 0.95rem;
    }

    .container {
      max-width: 900px;
      margin: 22px auto;
    }

    /* audio play hint */
    .play-hint {
      display:flex;
      justify-content:center;
      margin: 16px 0 8px;
    }
    .btn {
      background: linear-gradient(90deg, var(--accent), #8ee7ff 120%);
      color: #000;
      border: none;
      padding: 10px 16px;
      border-radius: 10px;
      font-weight: 600;
      cursor: pointer;
    }

    form.post-form {
      background: var(--card-bg);
      padding: 14px;
      border-radius: 12px;
      display: grid;
      grid-template-columns: 1fr;
      gap: 10px;
      margin-bottom: 18px;
      box-shadow: 0 4px 18px rgba(0,0,0,0.6);
    }
    .row {
      display:flex;
      gap:8px;
      align-items:center;
    }
    label {
      font-size: 0.9rem;
      color: rgba(255,255,255,0.9);
    }
    textarea {
      width: 100%;
      min-height: 90px;
      background: transparent;
      border: 1px solid var(--muted);
      padding: 10px;
      color: var(--fg);
      border-radius: 8px;
      resize: vertical;
    }
    input[type="file"] {
      color: var(--fg);
    }
    .posts {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-top: 8px;
    }
    .post {
      display: flex;
      gap: 12px;
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      padding: 12px;
      border-radius: 12px;
      align-items: center;
    }
    .post .text {
      flex: 1;
      white-space: pre-wrap;
    }
    .post .photo {
      width: 140px;
      height: 120px;
      flex-shrink: 0;
      object-fit: cover;
      border-radius: 10px;
      border: 1px solid var(--muted);
      background: #111;
    }
    .meta {
      font-size: 0.8rem;
      color: rgba(255,255,255,0.6);
      margin-top: 6px;
    }

    /* responsive */
    @media (max-width:720px) {
      .post {
        flex-direction: column;
        align-items: stretch;
      }
      .post .photo {
        width: 100%;
        height: 220px;
      }
    }

    .big-play {
      position: fixed;
      inset: 0;
      display: grid;
      place-items: center;
      background: linear-gradient(180deg, rgba(0,0,0,0.65), rgba(0,0,0,0.85));
      z-index: 999;
    }
    .big-play button {
      font-size: 1.05rem;
      padding: 18px 26px;
      border-radius: 14px;
      border: none;
      cursor: pointer;
      font-weight: 700;
    }

    footer {
      margin-top: 28px;
      text-align:center;
      color: rgba(255,255,255,0.5);
      font-size: 0.85rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>sabe oque eu acho engraçado</h1>
    <p class="lead">Escreva um texto romântico, coloque uma foto ao lado — e deixe o coração falar.</p>
  </header>

  <div class="container">
    <div class="play-hint" id="playHint">
      <button class="btn" id="trySpeakBtn">Tocar dizeres</button>
    </div>

    <form id="postForm" class="post-form" autocomplete="off">
      <label for="author">Seu nome (opcional)</label>
      <input id="author" placeholder="Ex.: Maria, João..." type="text" style="padding:8px;border-radius:8px;border:1px solid var(--muted);background:transparent;color:var(--fg)">

      <label for="text">Escreva algo romântico</label>
      <textarea id="text" placeholder="Ex.: Eu adoro quando você sorri..."></textarea>

      <div class="row">
        <label for="img">Escolher foto (lado direito)</label>
        <input id="img" type="file" accept="image/*">
      </div>

      <div style="display:flex;gap:8px;justify-content:space-between;align-items:center;">
        <small class="meta">As publicações ficam salvas neste navegador.</small>
        <div style="display:flex;gap:8px;">
          <button type="button" class="btn" id="clearBtn" style="background:transparent;border:1px solid var(--muted);color:var(--fg)">Limpar tudo</button>
          <button class="btn" id="submitBtn" type="submit">Publicar</button>
        </div>
      </div>
    </form>

    <section class="posts" id="posts"></section>

    <footer>Feito com carinho — <em>sabe oque eu acho engraçado</em></footer>
  </div>

  <!-- big play overlay (hidden by default) -->
  <div id="bigPlay" class="big-play" style="display:none;">
    <button id="bigPlayBtn" class="btn">Tocar dizeres</button>
  </div>

  <script>
    // Texto que será falado ao entrar
    const welcomeText = "Bem-vindo ao site: 'sabe oque eu acho engraçado'. Deixe aqui seu texto romântico e uma foto ao lado.";

    // DOM
    const trySpeakBtn = document.getElementById('trySpeakBtn');
    const bigPlay = document.getElementById('bigPlay');
    const bigPlayBtn = document.getElementById('bigPlayBtn');
    const postsEl = document.getElementById('posts');
    const form = document.getElementById('postForm');
    const imgInput = document.getElementById('img');
    const textInput = document.getElementById('text');
    const authorInput = document.getElementById('author');
    const clearBtn = document.getElementById('clearBtn');

    // localStorage key
    const STORAGE_KEY = 'sabe_que_posts_v1';

    // Utility: speak with Web Speech API
    function speak(text) {
      if (!('speechSynthesis' in window)) {
        alert('Seu navegador não suporta a síntese de voz (Web Speech API).');
        return;
      }
      // cancel any current utterances
      window.speechSynthesis.cancel();
      const u = new SpeechSynthesisUtterance(text);
      // choose a voice (try to match pt-BR)
      const voices = window.speechSynthesis.getVoices();
      const ptVoice = voices.find(v => /pt(-|_)br/i.test(v.lang)) || voices.find(v => /pt/i.test(v.lang));
      if (ptVoice) u.voice = ptVoice;
      u.rate = 1;
      u.pitch = 1;
      window.speechSynthesis.speak(u);
    }

    // Try to auto-speak on load. Browsers usually block autoplay without gesture.
    function tryAutoSpeak() {
      try {
        // Some browsers allow immediate speak if TTS is allowed
        speak(welcomeText);
        // Hide the large overlay if succeeded
        setTimeout(() => {
          // If still speaking or queued, hide small play hint
          if (window.speechSynthesis.speaking || window.speechSynthesis.pending) {
            document.getElementById('playHint').style.display = 'none';
          }
        }, 200);
      } catch (e) {
        // show big play overlay if cannot
        document.getElementById('playHint').style.display = 'block';
      }
    }

    // On first user interaction, some browsers then allow TTS.
    window.addEventListener('load', () => {
      // call getVoices to warm voices
      window.speechSynthesis.getVoices();
      // try automatic speak (may be blocked)
      tryAutoSpeak();
    });

    // Buttons to trigger speech
    trySpeakBtn.addEventListener('click', () => {
      speak(welcomeText);
      document.getElementById('playHint').style.display = 'none';
    });
    bigPlayBtn && bigPlayBtn.addEventListener('click', () => {
      speak(welcomeText);
      bigPlay.style.display = 'none';
    });

    // Posts management
    function loadPosts() {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (!raw) return [];
      try {
        return JSON.parse(raw);
      } catch (e) {
        console.warn('Erro ao ler posts do storage', e);
        return [];
      }
    }
    function savePosts(posts) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(posts));
    }

    function renderPosts() {
      const posts = loadPosts();
      postsEl.innerHTML = '';
      if (posts.length === 0) {
        postsEl.innerHTML = '<p style="color:rgba(255,255,255,0.6);text-align:center">Seja o primeiro a deixar um recado 💌</p>';
        return;
      }
      posts.slice().reverse().forEach(p => {
        const el = document.createElement('article');
        el.className = 'post';
        const textWrap = document.createElement('div');
        textWrap.className = 'text';
        textWrap.innerHTML = `<strong style="display:block">${escapeHtml(p.author || 'Anonimo')}</strong><div style="margin-top:6px">${escapeHtml(p.text)}</div><div class="meta">${new Date(p.createdAt).toLocaleString()}</div>`;
        el.appendChild(textWrap);

        if (p.imageData) {
          const img = document.createElement('img');
          img.className = 'photo';
          img.src = p.imageData;
          img.alt = 'Foto do recado';
          el.appendChild(img);
        }
        postsEl.appendChild(el);
      });
    }

    // Escape for safe HTML insertion
    function escapeHtml(s) {
      if (!s) return '';
      return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;').replaceAll("'", '&#39;').replaceAll('\\n','<br>');
    }

    // Handle form submit
    form.addEventListener('submit', (ev) => {
      ev.preventDefault();
      const text = textInput.value.trim();
      if (!text) {
        alert('Escreva algo romântico antes de publicar ✨');
        return;
      }
      const author = authorInput.value.trim();
      const file = imgInput.files[0];
      if (file) {
        // limit to 5MB
        if (file.size > 5 * 1024 * 1024) {
          alert('A imagem deve ter no máximo 5MB.');
          return;
        }
        const reader = new FileReader();
        reader.onload = (e) => {
          const imgData = e.target.result;
          addPost({ author, text, imageData: imgData, createdAt: Date.now() });
        };
        reader.readAsDataURL(file);
      } else {
        addPost({ author, text, imageData: null, createdAt: Date.now() });
      }
      form.reset();
    });

    function addPost(post) {
      const posts = loadPosts();
      posts.push(post);
      savePosts(posts);
      renderPosts();
    }

    // Clear all posts
    clearBtn.addEventListener('click', () => {
      if (confirm('Tem certeza que deseja apagar todos os recados salvos neste navegador?')) {
        localStorage.removeItem(STORAGE_KEY);
        renderPosts();
      }
    });

    // Initial render
    renderPosts();

    // Show big overlay if speech is blocked until user interacts
    // If after short time there is no speech and no user interaction, show overlay
    setTimeout(() => {
      if (!window.speechSynthesis.speaking && !window.speechSynthesis.pending) {
        // show small hint already visible; offer bigger overlay
        // but avoid being annoying: show only if user hasn't clicked anything.
        // We'll show a small overlay asking for gesture only if play hint button wasn't clicked.
        // For simplicity show overlay after timeout.
        // But only show overlay on larger screens.
        if (window.innerWidth > 600) {
          bigPlay.style.display = 'none'; // keep hidden by default
        }
      }
    }, 1200);

    // accessibility: press 'P' to trigger the speech (convenience)
    window.addEventListener('keydown', (e) => {
      if (e.key.toLowerCase() === 'p' && !e.metaKey && !e.ctrlKey) {
        speak(welcomeText);
      }
    });
  </script>
</body>
</html>
