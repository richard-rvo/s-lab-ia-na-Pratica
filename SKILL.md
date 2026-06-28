---
name: slab-design
description: Use ao criar páginas, landing pages, componentes ou mini-apps novos que devam seguir a identidade visual do S-LAB — paleta âmbar sobre papel quente, glassmorphism, tipografia Plus Jakarta Sans + Lora + JetBrains Mono. Carrega tokens, fundações e componentes prontos para recriar o "S-LAB look" do zero em qualquer projeto.
---

# S-LAB Design System

Identidade visual do S-LAB para aplicar em **páginas e projetos novos**. Tudo necessário está aqui — não dependa de copiar `style.css` de outro repositório.

## Quando usar

- Criar uma página, landing, dashboard, formulário ou mini-app novo que deve ter o "look S-LAB".
- Iniciar um projeto novo (mesmo fora deste repositório) que herde a identidade visual.

**Não** use para editar arquivos deste próprio repositório que já têm `style.css` (use o CSS existente), nem para lógica de backend.

## Princípios da marca (o porquê — adapte, não só copie)

- **Estética:** *warm paper + amber glassmorphism*. Fundo de papel quente (nunca branco puro), camadas de vidro fosco translúcido com `backdrop-filter`, halos de luz âmbar ambiente ao fundo dando profundidade.
- **Personalidade:** autoridade calorosa, editorial, científico-mas-humano.
- **Anti-padrão (evite o "AI look"):** roxo/azul genéricos, branco puro `#FFFFFF`, sombras duras e cinzas, cantos retos, gradientes neon. Se aparecer, está errado.

## Regras de aplicação (checklist)

1. **Eyebrows, labels, tags, badges** → sempre `--font-mono`, UPPERCASE, `letter-spacing` largo (`0.12em`–`0.18em`), cor `--orange-600`.
2. **Títulos** → `--font-display`, peso `800`, `letter-spacing: -0.04em`, line-height curto (`0.95`–`1.0`). Use `.grad` para colorir parte do título com o gradiente da marca.
3. **Texto de leitura / subtítulos / citações** → `--font-text` (Lora), itálico, `--ink-2`.
4. **Acento (laranja vivo)** → no máximo ~10% do layout. Reserve para CTAs e destaques interativos.
5. **Raios de canto** → cards `28px`, painéis `20–32px`, inputs `14px`, pills/botões `999px`. Nada de cantos retos.
6. **Fundo** → sempre `--paper` no body + o bloco `.ambient` (halos âmbar). Superfícies em vidro translúcido, nunca branco sólido.
7. **Sombras** → use `--glass-shadow` (suave, quente) e `--shadow-cta` (botões). Sem sombras pretas duras.

---

## 1. Tokens — bloco `:root`

```css
:root {
  --orange-50:  #FFF4E8;
  --orange-100: #FDE3C7;
  --orange-300: #F8A23B;
  --orange-500: #F08113;
  --orange-600: #E94D1D;
  --orange-700: #C13A12;
  --brand-grad: linear-gradient(135deg, #F8A23B 0%, #F08113 48%, #E94D1D 100%);
  --brand-grad-soft: linear-gradient(135deg, #FDB976 0%, #F39553 50%, #EE6F3A 100%);

  --paper:   #FAF7F2;
  --paper-2: #F2EEE7;
  --paper-3: #E8E3DA;
  --hairline: rgba(26,22,20,0.08);
  --hairline-strong: rgba(26,22,20,0.14);
  --ink:   #1A1614;
  --ink-2: #423B36;
  --stone: #87807A;
  --mist:  #B8B1AB;

  --font-display: "Plus Jakarta Sans", -apple-system, system-ui, sans-serif;
  --font-text:    "Lora", Georgia, serif;
  --font-mono:    "JetBrains Mono", ui-monospace, monospace;

  --glass-shadow: 0 30px 60px -20px rgba(170,90,40,0.18), 0 12px 24px -16px rgba(26,22,20,0.16);
  --shadow-brand: 0 40px 80px -20px rgba(233,77,29,0.45), inset 0 2px 0 rgba(255,255,255,0.40);
  --shadow-cta:   0 12px 24px -8px rgba(233,77,29,0.50), inset 0 1px 0 rgba(255,255,255,0.30);
}
```

---

## 2. Fundações

### Import das fontes (no `<head>`)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=Lora:ital,wght@0,400;1,400&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
```

### Reset + body

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font-display);
  background: var(--paper);
  color: var(--ink);
  min-height: 100vh;
  overflow-x: hidden;
}
```

### Ambient — halos de luz âmbar (sempre incluir)

Coloque `<div class="ambient"></div>` como primeiro filho do `<body>`. Dá a profundidade característica.

```css
.ambient { position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden; }
.ambient::before {
  content: ""; position: absolute;
  width: 900px; height: 900px;
  background: radial-gradient(circle, rgba(240,129,19,0.28), transparent 70%);
  filter: blur(140px); border-radius: 50%;
  top: -300px; right: -260px;
  animation: driftA 12s ease-in-out infinite alternate;
}
.ambient::after {
  content: ""; position: absolute;
  width: 800px; height: 800px;
  background: radial-gradient(circle, rgba(233,77,29,0.18), transparent 70%);
  filter: blur(140px); border-radius: 50%;
  top: 60%; left: -300px;
  animation: driftB 16s ease-in-out infinite alternate;
}
@keyframes driftA { from { transform: translate(0,0) scale(1); } to { transform: translate(-80px,60px) scale(1.1); } }
@keyframes driftB { from { transform: translate(0,0) scale(1); } to { transform: translate(60px,-80px) scale(1.08); } }
```

Conteúdo da página deve vir em containers com `position: relative; z-index: 1;` para ficar acima do ambient.

---

## 3. Componentes-chave

### Nav pill flutuante

```html
<nav>
  <div class="nav-logo"><img src="logo.png" alt="S-LAB"></div>
  <ul class="nav-tabs">
    <li><button class="nav-tab active">Início</button></li>
    <li><button class="nav-tab">Sobre</button></li>
  </ul>
  <span class="nav-badge">v1.0</span>
</nav>
```

```css
nav {
  position: sticky; top: 16px; z-index: 100;
  display: grid; grid-template-columns: 1fr auto 1fr; align-items: center;
  max-width: 1200px; margin: 16px auto 0; padding: 12px 22px;
  background: rgba(250,247,242,0.72);
  backdrop-filter: blur(28px) saturate(140%);
  -webkit-backdrop-filter: blur(28px) saturate(140%);
  border: 1px solid rgba(255,255,255,0.85);
  border-radius: 999px; box-shadow: var(--glass-shadow);
}
.nav-logo { justify-self: start; }
.nav-logo img { height: 32px; width: auto; display: block; }
.nav-tabs { display: flex; gap: 4px; list-style: none; justify-self: center; }
.nav-tab {
  padding: 8px 16px; border-radius: 999px;
  font-size: 13px; font-weight: 600; letter-spacing: -0.01em;
  color: var(--stone); cursor: pointer; border: none; background: transparent;
  transition: all 200ms ease;
}
.nav-tab:hover { color: var(--ink); background: var(--paper-2); }
.nav-tab.active { background: var(--ink); color: var(--paper); }
.nav-badge {
  font-family: var(--font-mono);
  font-size: 11px; letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--orange-600); padding: 6px 14px; border-radius: 999px;
  background: var(--orange-50); border: 1px solid var(--orange-100); justify-self: end;
}
```

### Hero

```html
<section class="hero">
  <div>
    <div class="hero-eyebrow">Sua chamada curta</div>
    <h1>Título forte com <span class="grad">destaque</span> em gradiente</h1>
    <p class="hero-sub">Subtítulo editorial em itálico que explica a proposta com calma.</p>
    <div class="hero-actions">
      <a class="btn-primary" href="#">Ação principal</a>
      <a class="btn-ghost" href="#">Ação secundária</a>
    </div>
  </div>
</section>
```

```css
.hero {
  max-width: 1200px; margin: 0 auto; padding: 100px 56px 80px;
  position: relative; z-index: 1;
}
.hero-eyebrow {
  font-family: var(--font-mono);
  font-size: 11px; letter-spacing: 0.18em; text-transform: uppercase;
  color: var(--orange-600);
  display: inline-flex; align-items: center; gap: 10px; margin-bottom: 24px;
}
.hero-eyebrow::before { content: ""; width: 18px; height: 1px; background: var(--orange-500); }
.hero h1 {
  font-family: var(--font-display);
  font-size: clamp(28px, 4vw, 52px);
  font-weight: 800; letter-spacing: -0.04em; line-height: 0.95; margin-bottom: 24px;
}
.grad {
  background: var(--brand-grad);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.hero-sub {
  font-family: var(--font-text);
  font-size: 16px; line-height: 1.55; color: var(--ink-2);
  max-width: 480px; margin-bottom: 40px; font-style: italic;
}
.hero-actions { display: flex; gap: 12px; flex-wrap: wrap; }
```

### Botões

```css
.btn-primary {
  display: inline-flex; align-items: center; gap: 10px;
  background: var(--brand-grad); color: white; border: 0;
  padding: 16px 28px; border-radius: 999px;
  font-family: var(--font-display); font-weight: 700; font-size: 15px; letter-spacing: -0.01em;
  box-shadow: var(--shadow-cta); cursor: pointer; text-decoration: none;
  transition: transform 200ms, box-shadow 200ms;
}
.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 18px 36px -10px rgba(233,77,29,0.6), inset 0 1px 0 rgba(255,255,255,0.3);
}
.btn-ghost {
  display: inline-flex; align-items: center; gap: 8px;
  background: rgba(255,255,255,0.6); backdrop-filter: blur(20px);
  color: var(--ink); border: 1px solid rgba(255,255,255,0.85);
  padding: 16px 28px; border-radius: 999px;
  font-family: var(--font-display); font-weight: 600; font-size: 15px;
  cursor: pointer; text-decoration: none; transition: all 200ms;
}
.btn-ghost:hover { background: rgba(255,255,255,0.85); }
```

### Card de vidro (padrão para qualquer superfície)

```css
.glass-card {
  background: rgba(255,255,255,0.55);
  backdrop-filter: blur(28px) saturate(140%);
  -webkit-backdrop-filter: blur(28px) saturate(140%);
  border: 1px solid rgba(255,255,255,0.85);
  border-radius: 28px; box-shadow: var(--glass-shadow);
  padding: 28px 32px; overflow: hidden;
  transition: transform 200ms, box-shadow 200ms;
}
.glass-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 40px 80px -20px rgba(170,90,40,0.22), 0 16px 32px -16px rgba(26,22,20,0.18);
}
```

### Formulário

```css
.form-label {
  font-family: var(--font-mono);
  font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase;
  color: var(--stone); margin-bottom: 8px; display: block;
}
.form-input, .form-select, .form-textarea {
  width: 100%; background: rgba(255,255,255,0.75);
  border: 1px solid var(--hairline-strong); border-radius: 14px;
  padding: 12px 16px;
  font-family: var(--font-display); font-size: 14px; color: var(--ink-2);
  outline: none; transition: border-color 200ms, box-shadow 200ms;
}
.form-input::placeholder { color: var(--mist); }
.form-input:focus, .form-select:focus, .form-textarea:focus {
  border-color: var(--orange-300);
  box-shadow: 0 0 0 3px rgba(240,129,19,0.12);
}
.form-textarea { resize: vertical; min-height: 80px; line-height: 1.5; }
.form-select { appearance: none; cursor: pointer; }
```

### Footer escuro

```css
footer {
  position: relative; z-index: 1;
  background: linear-gradient(160deg, #2A211B, #1A1614);
  padding: 64px 56px; text-align: center;
}
.footer-logo img { height: 48px; width: auto; display: block; margin: 0 auto 16px; }
.footer-sub {
  font-family: var(--font-mono);
  font-size: 11px; letter-spacing: 0.14em; text-transform: uppercase;
  color: rgba(250,247,242,0.4);
}
```

---

## 4. DNA visual para imagens geradas por IA (opcional)

Quando precisar gerar imagens que combinem com a marca, cole este bloco no prompt (em inglês — performa melhor):

```
Warm paper background (#FAF7F2), soft amber glassmorphism aesthetic.
Palette: warm amber and burnt orange (#F08113, #E94D1D) over cream paper, no pure white.
Lighting: soft diffused warm light, gentle radial glow.
Style: clean editorial, frosted glass surfaces, generous whitespace, rounded shapes.
Exclusions: no purple, no neon, no hard shadows, no text, no watermarks, no oversaturated colors.
```

---

## Padrões responsivos (referência)

- `max-width: 1080px`: hero e grids passam para 1 coluna; padding lateral `32px`.
- `max-width: 720px`: nav vira menu colapsável; padding `24px`; títulos reduzidos.
- `max-width: 480px`: grids de stats/itens em 1 coluna.

Containers principais sempre com `max-width: 1200px; margin: 0 auto;`.
