import streamlit as st
import random
import time
import plotly.graph_objects as go
from datetime import datetime

st.set_page_config(
    page_title="Mesa de Dados ⚔",
    page_icon="🎲",
    layout="wide",
    initial_sidebar_state="expanded",
)

# ─── CSS ─────────────────────────────────────────────────────────────────────
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@400;700&family=Crimson+Pro:ital,wght@0,400;0,600;1,400&display=swap');

html, body, [class*="css"] { font-family: 'Crimson Pro', Georgia, serif; }
.stApp { background: #0d0a06; }

[data-testid="stSidebar"] {
    background: linear-gradient(180deg, #100c07 0%, #1a1208 100%);
    border-right: 1px solid #3a2d10;
}
[data-testid="stSidebar"] * { color: #e8c97a !important; }

h1 { font-family:'Cinzel Decorative',serif !important; color:#e8c97a !important; text-align:center; letter-spacing:0.08em; }
h2 { font-family:'Cinzel Decorative',serif !important; color:#c9a84c !important; font-size:1.1rem !important; }
h3 { font-family:'Cinzel Decorative',serif !important; color:#a87c2a !important; font-size:0.9rem !important; }

[data-testid="metric-container"] {
    background: rgba(201,168,76,0.07);
    border: 1px solid rgba(201,168,76,0.25);
    border-radius: 12px; padding: 0.75rem 1rem;
}
[data-testid="metric-container"] label {
    font-family:'Cinzel Decorative',serif !important;
    font-size:0.65rem !important; letter-spacing:0.12em !important;
    color:rgba(201,168,76,0.6) !important;
}
[data-testid="metric-container"] [data-testid="stMetricValue"] {
    font-family:'Cinzel Decorative',serif !important; color:#e8c97a !important; font-size:2rem !important;
}
[data-testid="metric-container"] [data-testid="stMetricDelta"] { color:rgba(201,168,76,0.55) !important; }

.stButton > button {
    font-family:'Cinzel Decorative',serif !important;
    background: linear-gradient(135deg,#6b4f15 0%,#c9a84c 50%,#6b4f15 100%) !important;
    color:#0d0a06 !important; border:1px solid #c9a84c !important;
    border-radius:8px !important; font-weight:700 !important; letter-spacing:0.08em !important;
}
.stButton > button:hover { box-shadow:0 6px 20px rgba(201,168,76,0.3) !important; }

.stSelectbox > div > div {
    background:rgba(201,168,76,0.05) !important;
    border-color:rgba(201,168,76,0.3) !important; color:#e8c97a !important;
}
.stNumberInput > div > div > input {
    background:rgba(201,168,76,0.05) !important;
    border-color:rgba(201,168,76,0.3) !important; color:#e8c97a !important;
}
.streamlit-expanderHeader {
    font-family:'Cinzel Decorative',serif !important; color:#c9a84c !important;
    background:rgba(201,168,76,0.05) !important;
    border:0.5px solid rgba(201,168,76,0.2) !important; border-radius:8px !important;
}
hr { border-color:rgba(201,168,76,0.2) !important; }
::-webkit-scrollbar { width:4px; }
::-webkit-scrollbar-thumb { background:rgba(201,168,76,0.3); border-radius:2px; }

.pip-box {
    display:inline-block; width:40px; height:40px; line-height:40px;
    text-align:center; border-radius:8px;
    font-family:'Cinzel Decorative',serif; font-size:12px; margin:3px;
    border:1px solid rgba(201,168,76,0.3); background:rgba(201,168,76,0.06); color:#ccc;
    transition: all 0.3s ease;
}
.pip-max { background:rgba(201,168,76,0.3) !important; border-color:#c9a84c !important; color:#e8c97a !important; box-shadow:0 0 10px rgba(201,168,76,0.3); }
.pip-min { background:rgba(200,80,80,0.1) !important; border-color:rgba(200,80,80,0.4) !important; color:rgba(220,100,100,0.85) !important; }
.pip-high { background:rgba(201,168,76,0.15) !important; border-color:rgba(201,168,76,0.5) !important; color:#e0bc60 !important; }

.crit-badge {
    display:block; background:rgba(201,168,76,0.2); border:1px solid #c9a84c;
    border-radius:20px; padding:6px 20px; font-family:'Cinzel Decorative',serif;
    font-size:14px; color:#e8c97a; letter-spacing:0.12em; text-align:center; margin-bottom:12px;
}
.fumble-badge {
    display:block; background:rgba(200,80,80,0.1); border:1px solid rgba(200,80,80,0.5);
    border-radius:20px; padding:6px 20px; font-family:'Cinzel Decorative',serif;
    font-size:14px; color:rgba(230,100,100,0.9); letter-spacing:0.12em; text-align:center; margin-bottom:12px;
}

/* Modifier section */
.mod-section {
    background: rgba(201,168,76,0.05);
    border: 1px solid rgba(201,168,76,0.2);
    border-radius: 10px;
    padding: 12px 14px;
    margin: 8px 0;
}
.mod-title {
    font-family:'Cinzel Decorative',serif;
    font-size:10px; letter-spacing:0.15em;
    color:rgba(201,168,76,0.5); text-transform:uppercase;
    margin-bottom:8px;
}

/* Dice animation area */
#dice-anim-area {
    min-height: 120px;
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    align-items: center;
    justify-content: center;
    padding: 16px;
    background: rgba(201,168,76,0.03);
    border: 1px solid rgba(201,168,76,0.12);
    border-radius: 12px;
    margin-bottom: 16px;
}

.die-anim {
    width: 60px; height: 60px;
    border-radius: 10px;
    border: 2px solid rgba(201,168,76,0.5);
    background: rgba(13,10,6,0.9);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Cinzel Decorative', serif;
    font-size: 20px;
    color: #e8c97a;
    position: relative;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5), inset 0 0 20px rgba(201,168,76,0.05);
    transition: all 0.15s ease;
}

.die-anim.rolling {
    animation: dieRoll 0.08s infinite alternate;
    border-color: #c9a84c;
    box-shadow: 0 0 20px rgba(201,168,76,0.4), 0 4px 15px rgba(0,0,0,0.5);
}

.die-anim.landed {
    animation: dieLand 0.4s ease forwards;
}

.die-anim.landed-max {
    animation: dieLandMax 0.5s ease forwards;
    border-color: #e8c97a;
    box-shadow: 0 0 25px rgba(201,168,76,0.6);
}

.die-anim.landed-min {
    animation: dieLandMin 0.4s ease forwards;
    border-color: rgba(200,80,80,0.6);
    box-shadow: 0 0 15px rgba(200,80,80,0.3);
    color: rgba(220,100,100,0.85) !important;
}

@keyframes dieRoll {
    0%   { transform: rotate(-8deg) scale(1.05) translateY(-2px); }
    100% { transform: rotate(8deg)  scale(1.05) translateY(2px); }
}
@keyframes dieLand {
    0%   { transform: scale(1.3) rotate(5deg); opacity:0.5; }
    60%  { transform: scale(0.92) rotate(-2deg); }
    80%  { transform: scale(1.04) rotate(1deg); }
    100% { transform: scale(1) rotate(0deg); opacity:1; }
}
@keyframes dieLandMax {
    0%   { transform: scale(1.5); opacity:0.3; }
    50%  { transform: scale(0.9); }
    75%  { transform: scale(1.12); }
    100% { transform: scale(1); opacity:1; }
}
@keyframes dieLandMin {
    0%   { transform: scale(1.2) rotate(-10deg); opacity:0.3; }
    50%  { transform: scale(0.95) rotate(5deg); }
    100% { transform: scale(1) rotate(0deg); opacity:1; }
}

.die-label {
    position: absolute;
    bottom: -18px; left: 50%; transform: translateX(-50%);
    font-size: 9px; color: rgba(201,168,76,0.45);
    font-family: 'Cinzel Decorative', serif;
    white-space: nowrap;
}
</style>
""", unsafe_allow_html=True)

# ─── State ───────────────────────────────────────────────────────────────────
for k, v in {
    "history": [], "last_rolls": {}, "last_total": None,
    "last_modifier": 0, "roll_count": 0, "session_rolls": [],
    "modifiers": [],  # list of {"type": "+"/"-"/"×"/"÷", "value": int, "desc": str}
}.items():
    if k not in st.session_state:
        st.session_state[k] = v

# ─── Data ────────────────────────────────────────────────────────────────────
DICE_OPTIONS = {
    "d2": 2, "d3": 3, "d4": 4, "d6": 6, "d8": 8,
    "d10": 10, "d12": 12, "d16": 16, "d20": 20,
    "d30": 30, "d60": 60, "d100": 100,
}
DIE_ICONS = {2:"○",3:"△",4:"◆",6:"⬡",8:"◈",10:"◉",12:"⬟",16:"◇",20:"⬢",30:"★",60:"✦",100:"◎"}

# Presets: purely dice notation, no names
PRESETS = [
    "1d20", "2d20", "4d6", "2d6", "8d6",
    "6d6",  "2d8",  "1d12","10d10","5d100",
    "3d4",  "1d4+1d6+1d8+1d12",
]

def roll_die(faces): return random.randint(1, faces)

def parse_notation(notation):
    """Parse '2d6+1d8' into list of (faces, qty)"""
    import re
    groups = re.findall(r'(\d+)d(\d+)', notation)
    return [(int(f), int(q)) for q, f in groups]

def classify_pip(val, faces):
    if val == faces: return "max"
    if val == 1:     return "min"
    if val >= faces * 0.75: return "high"
    return "normal"

def build_pip_html(rolls, faces):
    html = ""
    for r in rolls:
        cls = classify_pip(r, faces)
        extra = f"pip-{cls}" if cls != "normal" else ""
        html += f'<span class="pip-box {extra}">{r}</span>'
    return html

def detect_special(rolls_dict):
    if 20 in rolls_dict:
        if 20 in rolls_dict[20]: return "crit"
        if 1 in rolls_dict[20] and 20 not in rolls_dict[20]: return "fumble"
    return None

def format_notation(rolls_dict, mods):
    parts = [f"{len(v)}d{k}" for k, v in sorted(rolls_dict.items())]
    s = " + ".join(parts)
    for m in mods:
        op = m["type"]
        v  = m["value"]
        if op == "+": s += f" +{v}"
        elif op == "-": s += f" -{v}"
        elif op == "×": s += f" ×{v}"
        elif op == "÷": s += f" ÷{v}"
    return s

def apply_modifiers(raw_sum, mods):
    result = float(raw_sum)
    for m in mods:
        op, v = m["type"], m["value"]
        if op == "+": result += v
        elif op == "-": result -= v
        elif op == "×": result *= v
        elif op == "÷": result = result / v if v != 0 else result
    return int(result)

def do_roll(dice_entries, mods):
    rolls_dict = {}
    for faces, qty in dice_entries:
        rolls = [roll_die(faces) for _ in range(qty)]
        if faces in rolls_dict:
            rolls_dict[faces].extend(rolls)
        else:
            rolls_dict[faces] = rolls

    raw_sum = sum(v for vals in rolls_dict.values() for v in vals)
    total   = apply_modifiers(raw_sum, mods)
    notation = format_notation(rolls_dict, mods)
    ts = datetime.now().strftime("%H:%M:%S")

    st.session_state.last_rolls    = rolls_dict
    st.session_state.last_total    = total
    st.session_state.last_modifier = sum(m["value"] if m["type"] == "+" else -m["value"] for m in mods if m["type"] in ("+","-"))
    st.session_state.roll_count   += 1
    st.session_state.session_rolls.append(total)
    st.session_state.history.insert(0, {"notation": notation, "total": total, "rolls": dict(rolls_dict), "time": ts})
    if len(st.session_state.history) > 20:
        st.session_state.history.pop()
    return rolls_dict, total

# ─── Sidebar ─────────────────────────────────────────────────────────────────
with st.sidebar:
    st.markdown("## ⚔ Mesa de Dados")
    st.markdown("---")

    # Presets — apenas notação
    st.markdown("### 📜 Predefinições")
    cols_p = st.columns(3)
    chosen_preset = None
    for i, p in enumerate(PRESETS):
        if cols_p[i % 3].button(p, key=f"preset_{i}", use_container_width=True):
            chosen_preset = p

    st.markdown("---")
    st.markdown("### 🎲 Grupos de Dados")
    num_groups = st.number_input("Quantos grupos", 1, 6, 1, step=1)
    dice_entries = []
    for i in range(int(num_groups)):
        c1, c2 = st.columns([2, 1])
        with c1:
            die_key = st.selectbox("Dado", list(DICE_OPTIONS.keys()), index=5, key=f"dt_{i}", label_visibility="collapsed")
        with c2:
            qty = st.number_input("Qtd", 1, 99, 1, key=f"dq_{i}", label_visibility="collapsed")
        dice_entries.append((DICE_OPTIONS[die_key], int(qty)))

    st.markdown("---")

    # ── Modifier section ────────────────────────────────────────────────────
    st.markdown("### ➕ Modificadores")
    st.markdown('<div class="mod-section">', unsafe_allow_html=True)

    mod_op  = st.selectbox("Operação", ["+", "-", "×", "÷"], key="mod_op", label_visibility="collapsed")
    mod_val = st.number_input("Valor", 0, 9999, 0, key="mod_val", label_visibility="collapsed")
    mod_desc = st.text_input("Descrição (opcional)", key="mod_desc", label_visibility="collapsed",
                              placeholder="ex: Força, Proficiência...")

    c_add, c_clr = st.columns(2)
    if c_add.button("＋ Adicionar", use_container_width=True):
        if mod_val != 0:
            st.session_state.modifiers.append({"type": mod_op, "value": mod_val, "desc": mod_desc or mod_op + str(mod_val)})

    if c_clr.button("✕ Limpar", use_container_width=True):
        st.session_state.modifiers = []

    st.markdown('</div>', unsafe_allow_html=True)

    if st.session_state.modifiers:
        for i, m in enumerate(st.session_state.modifiers):
            colm, colr = st.columns([4, 1])
            colm.markdown(
                f"<span style='font-size:12px;color:#c9a84c'>{m['type']}{m['value']}</span>"
                f"<span style='font-size:11px;color:rgba(201,168,76,0.5)'> — {m['desc']}</span>",
                unsafe_allow_html=True
            )
            if colr.button("✕", key=f"rm_mod_{i}"):
                st.session_state.modifiers.pop(i)
                st.rerun()

    st.markdown("---")
    roll_button = st.button("🎲  ROLAR OS DADOS", use_container_width=True)
    if st.button("🗑 Limpar Histórico", use_container_width=False):
        st.session_state.history = []
        st.session_state.session_rolls = []
        st.session_state.roll_count = 0
        st.session_state.last_rolls = {}
        st.session_state.last_total = None

# ─── Handle preset click ─────────────────────────────────────────────────────
if chosen_preset:
    entries = parse_notation(chosen_preset)
    do_roll(entries, st.session_state.modifiers)

# ─── Handle roll button ───────────────────────────────────────────────────────
if roll_button:
    do_roll(dice_entries, st.session_state.modifiers)

# ─── Main ─────────────────────────────────────────────────────────────────────
st.markdown("<h1>⚔ Mesa de Dados RPG</h1>", unsafe_allow_html=True)
st.markdown("<hr>", unsafe_allow_html=True)

has_result = st.session_state.last_total is not None

# ── Dice Animation (JavaScript) ───────────────────────────────────────────────
if has_result:
    rolls      = st.session_state.last_rolls
    total      = st.session_state.last_total
    all_values = [v for vals in rolls.values() for v in vals]

    # Build dice data for JS animation
    dice_js_data = []
    for faces, face_rolls in sorted(rolls.items()):
        for r in face_rolls:
            dice_js_data.append({"faces": faces, "result": r})

    import json
    dice_json = json.dumps(dice_js_data)

    anim_html = f"""
<div id="dice-anim-area"></div>
<script>
(function() {{
    const data   = {dice_json};
    const area   = document.getElementById('dice-anim-area');
    if (!area) return;
    area.innerHTML = '';

    const FRAMES_ROLL  = 18;   // ~1.5s rolling
    const FRAME_MS     = 80;
    const STAGGER_MS   = 120;

    const icons = {{2:'○',3:'△',4:'◆',6:'⬡',8:'◈',10:'◉',12:'⬟',16:'◇',20:'⬢',30:'★',60:'✦',100:'◎'}};

    // create die elements
    const divs = data.map((d, i) => {{
        const wrap = document.createElement('div');
        wrap.style.position = 'relative';
        wrap.style.marginBottom = '22px';

        const el = document.createElement('div');
        el.className = 'die-anim rolling';
        el.textContent = icons[d.faces] || '●';

        const lbl = document.createElement('span');
        lbl.className = 'die-label';
        lbl.textContent = 'd' + d.faces;

        wrap.appendChild(el);
        wrap.appendChild(lbl);
        area.appendChild(wrap);
        return {{ el, d }};
    }});

    // stagger-start each die's roll animation
    divs.forEach(({{ el, d }}, i) => {{
        let frame = 0;
        const delay = i * STAGGER_MS;
        const totalFrames = FRAMES_ROLL + i * 2;  // later dice roll longer

        setTimeout(() => {{
            const iv = setInterval(() => {{
                frame++;
                // show random value while rolling
                el.textContent = Math.floor(Math.random() * d.faces) + 1;

                if (frame >= totalFrames) {{
                    clearInterval(iv);
                    // land
                    el.textContent = d.result;
                    el.classList.remove('rolling');

                    const isMax  = d.result === d.faces;
                    const isMin  = d.result === 1;
                    const isHigh = d.result >= Math.ceil(d.faces * 0.75);

                    if (isMax)       el.classList.add('landed-max');
                    else if (isMin)  el.classList.add('landed-min');
                    else             el.classList.add('landed');

                    if (isMax) {{
                        el.style.color = '#e8c97a';
                    }} else if (isMin) {{
                        el.style.color = 'rgba(220,100,100,0.85)';
                    }} else if (isHigh) {{
                        el.style.color = '#e0bc60';
                    }} else {{
                        el.style.color = '#ccc';
                    }}
                }}
            }}, FRAME_MS);
        }}, delay);
    }});
}})();
</script>
"""
    st.components.v1.html(anim_html, height=max(140, (len(all_values) // 8 + 1) * 100))

    # ── Special badges ────────────────────────────────────────────────────────
    special = detect_special(rolls)
    if special == "crit":
        st.markdown('<div class="crit-badge">✦ ACERTO CRÍTICO! ✦</div>', unsafe_allow_html=True)
        st.balloons()
    elif special == "fumble":
        st.markdown('<div class="fumble-badge">☠ FALHA CRÍTICA ☠</div>', unsafe_allow_html=True)

    # ── Metrics ───────────────────────────────────────────────────────────────
    raw_sum = sum(all_values)
    mod_total = total - raw_sum
    c1, c2, c3, c4, c5 = st.columns(5)
    c1.metric("TOTAL", total, delta=f"{mod_total:+d}" if mod_total else None)
    c2.metric("DADOS", len(all_values))
    c3.metric("MÍNIMO", min(all_values))
    c4.metric("MÉDIA", f"{raw_sum/len(all_values):.1f}")
    c5.metric("MÁXIMO", max(all_values))

    # Modifiers breakdown
    if st.session_state.modifiers:
        mod_parts = " | ".join(
            f"<span style='color:#c9a84c'>{m['type']}{m['value']}</span>"
            f"<span style='color:rgba(201,168,76,0.45);font-size:11px'> {m['desc']}</span>"
            for m in st.session_state.modifiers
        )
        st.markdown(
            f"<div style='text-align:center;margin:6px 0;font-size:13px'>"
            f"Modificadores: {mod_parts}"
            f" → bruto <b style='color:#e8c97a'>{raw_sum}</b>"
            f" → total <b style='color:#e8c97a'>{total}</b></div>",
            unsafe_allow_html=True
        )

    st.markdown("<hr>", unsafe_allow_html=True)

    left, right = st.columns([3, 2])

    with left:
        st.markdown("## Resultados por grupo")
        for faces, face_rolls in sorted(rolls.items()):
            icon = DIE_ICONS.get(faces, "◉")
            face_sum = sum(face_rolls)
            pct = face_sum / (faces * len(face_rolls)) * 100
            st.markdown(
                f"**{icon} d{faces}** — {len(face_rolls)} dado(s) → soma **{face_sum}** "
                f"<span style='color:rgba(201,168,76,0.5);font-size:12px'>({pct:.0f}% do máximo)</span>",
                unsafe_allow_html=True
            )
            st.markdown(build_pip_html(face_rolls, faces), unsafe_allow_html=True)
            st.progress(face_sum / (faces * len(face_rolls)))
            st.markdown("<br>", unsafe_allow_html=True)

    with right:
        st.markdown("## Distribuição")
        fig = go.Figure()
        fig.add_trace(go.Histogram(
            x=all_values,
            nbinsx=min(len(set(all_values)), 30),
            marker_color="rgba(201,168,76,0.6)",
            marker_line_color="rgba(201,168,76,0.9)",
            marker_line_width=1,
        ))
        fig.update_layout(
            paper_bgcolor="rgba(0,0,0,0)", plot_bgcolor="rgba(201,168,76,0.04)",
            font_color="#c9a84c", margin=dict(l=10,r=10,t=10,b=30), height=220,
            showlegend=False,
            xaxis=dict(gridcolor="rgba(201,168,76,0.1)"),
            yaxis=dict(gridcolor="rgba(201,168,76,0.1)"),
            bargap=0.08,
        )
        st.plotly_chart(fig, use_container_width=True, config={"displayModeBar": False})

        if len(rolls) > 1:
            labels = [f"d{k}" for k in rolls]
            values = [sum(v) for v in rolls.values()]
            fig2 = go.Figure(go.Pie(
                labels=labels, values=values, hole=0.5,
                marker=dict(colors=["#c9a84c","#8a6a1f","#e8c97a","#6b4f15","#a87c2a","#d4b060"],
                            line=dict(color="rgba(0,0,0,0.3)", width=1)),
                textinfo="label+percent", textfont_color="#0d0a06",
            ))
            fig2.update_layout(paper_bgcolor="rgba(0,0,0,0)", font_color="#c9a84c",
                               margin=dict(l=0,r=0,t=0,b=0), height=180, showlegend=False)
            st.plotly_chart(fig2, use_container_width=True, config={"displayModeBar": False})

    # ── Session trend ────────────────────────────────────────────────────────
    if len(st.session_state.session_rolls) > 1:
        st.markdown("<hr>", unsafe_allow_html=True)
        st.markdown("## Tendência da Sessão")
        seq = st.session_state.session_rolls
        avg = sum(seq) / len(seq)
        fig3 = go.Figure()
        fig3.add_trace(go.Scatter(
            y=seq, x=list(range(1, len(seq)+1)),
            mode="lines+markers",
            line=dict(color="#c9a84c", width=2),
            marker=dict(color="#e8c97a", size=7, line=dict(color="#0d0a06", width=1)),
            fill="tozeroy", fillcolor="rgba(201,168,76,0.07)",
        ))
        fig3.add_hline(y=avg, line_dash="dot", line_color="rgba(201,168,76,0.4)",
                       annotation_text=f"Média: {avg:.1f}", annotation_font_color="#c9a84c")
        fig3.update_layout(
            paper_bgcolor="rgba(0,0,0,0)", plot_bgcolor="rgba(201,168,76,0.03)",
            font_color="#c9a84c", margin=dict(l=10,r=10,t=10,b=30), height=200, showlegend=False,
            xaxis=dict(title="Rolagem #", gridcolor="rgba(201,168,76,0.08)"),
            yaxis=dict(title="Total", gridcolor="rgba(201,168,76,0.08)"),
        )
        st.plotly_chart(fig3, use_container_width=True, config={"displayModeBar": False})

else:
    st.markdown("""
    <div style='text-align:center;padding:4rem 2rem;color:rgba(201,168,76,0.35);'>
        <div style='font-size:3rem;'>🎲</div>
        <div style='font-family:Cinzel Decorative,serif;font-size:1rem;margin-top:1rem;letter-spacing:0.1em;'>
            Configure seus dados na barra lateral e role!
        </div>
    </div>""", unsafe_allow_html=True)

# ─── History ──────────────────────────────────────────────────────────────────
if st.session_state.history:
    st.markdown("<hr>", unsafe_allow_html=True)
    with st.expander("📜 Histórico de Rolagens", expanded=False):
        for i, entry in enumerate(st.session_state.history):
            ca, cb, cc = st.columns([3,1,1])
            ca.markdown(f"<span style='color:#888;font-size:12px'>#{st.session_state.roll_count-i}</span>  `{entry['notation']}`", unsafe_allow_html=True)
            cb.markdown(f"<span style='font-family:Cinzel Decorative,serif;color:#e8c97a;font-size:16px'>{entry['total']}</span>", unsafe_allow_html=True)
            cc.markdown(f"<span style='color:rgba(201,168,76,0.4);font-size:11px'>{entry['time']}</span>", unsafe_allow_html=True)

st.markdown("<hr>", unsafe_allow_html=True)
st.markdown(
    "<div style='text-align:center;font-size:11px;color:rgba(201,168,76,0.3);font-family:Cinzel Decorative,serif;letter-spacing:0.1em;'>"
    "⚔ Mesa de Dados RPG — que os dados rolem a seu favor ⚔</div>",
    unsafe_allow_html=True
)
