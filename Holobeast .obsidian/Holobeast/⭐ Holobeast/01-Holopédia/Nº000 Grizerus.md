---
dg-publish: true
id: "000"
nome: Grizerus
tipo1: Normal
tipo2:
raridade: Exótico
hp: 77
atk: 120
defense: 90
mag_atk: 60
mag_def: 90
speed: 48
sprite: Grizerus.png
categoria: Holo Divino
altura: ???
peso: ???
habilidade: Multitype
gender: ???
evolucoes: Este Holo não evolui.
omega_evolucoes: Este Holo não Possui.
descricao: De acordo com as lendas, este holo é o primeiro de um sér. Um ser poderoso que emergiu das energias primordiais. Antes do tempo, antes do espaço, antes da própria luz... existia apenas o Vazio Consciente. Grizerus não foi criado — ele SEMPRE EXISTIU. Sua aparição tanto Biologica e bioholográfica é a matriz original da qual todos os outros Holos foram moldados. Ele não habita Brasidia; ele SONHOU Brasidia em existência.stória e os detalhes do seu Holobeast...
---

```dataviewjs
// 1. Dicionário de Cores baseado no seu arquivo de Tipagens
const coresTipos = {
  "Normal": "#A8A77A", "Lutador": "#CD154C", "Voador": "#89AAE3", "Veneno": "#B567CE",
  "Terra": "#D97845", "Pedra": "#C5B78C", "Inseto": "#91C12F", "Aço": "#5A8EA2",
  "Fogo": "#FF9D55", "Fantasma": "#5269AD", "Água": "#5090D6", "Planta": "#63BC5A",
  "Elétrico": "#EFC600", "Gelo": "#73CEC0", "Trevas": "#5A5465", "Luz": "#FFD700",
  "Fantasia": "#EC8FE6", "Mental": "#F7A2A0", "Cybernexo": "#00B6FF", "Sonoro": "#D6F7FF",
  "Cosmos": "#000350", "Ancestral": "#DCCCB8"
};

const configsRaridade = {
  "Comum": { cor: "#94a3b8", icone: "⚪" },
  "Incomum": { cor: "#3b82f6", icone: "🔵" },
  "Raro": { cor: "#eab308", icone: "🟡" },
  "Exótico": { cor: "#a855f7", icone: "✨" },
  "Pseudo-Lendário": { cor: "#f97316", icone: "🟠" },
  "Lendário": { cor: "#ef4444", icone: "🔴" },
  "Mítico": { cor: "#ec4899", icone: "🟣" }
};

const listaRaridades = ["Comum", "Incomum", "Raro", "Exótico", "Pseudo-Lendário", "Lendário", "Mítico"];

const current = dv.current();
const id = current.id || "000";
const nome = current.nome || current.file.name;
const tipo1 = current.tipo1 || "Normal";
const tipo2 = current.tipo2 ? current.tipo2.trim() : "";
const sprite = current.sprite ? current.sprite.trim() : "";
const raridade = current.raridade || "Comum";

const corNeon = coresTipos[tipo1] || "#00E5FF";
const configRaridade = configsRaridade[raridade] || { cor: "#94a3b8", icone: "⚪" };

let backgroundCirculo = "";
const cor1 = coresTipos[tipo1] || "#1e293b";

if (tipo2 && tipo2 !== "") {
  const cor2 = coresTipos[tipo2] || "#0f172a";
  backgroundCirculo = `linear-gradient(135deg, ${cor1} 50%, ${cor2} 50%)`;
} else {
  backgroundCirculo = cor1;
}

const hp = Number(current.hp) || 0;
const atk = Number(current.atk) || 0;
const defense = Number(current.defense) || 0;
const mag_atk = Number(current.mag_atk) || 0;
const mag_def = Number(current.mag_def) || 0;
const speed = Number(current.speed) || 0;
const total = hp + atk + defense + mag_atk + mag_def + speed;

const pct = (stat) => Math.min((stat / 255) * 100, 100).toFixed(2) + "%";

let finalHTML = "";

// A. Menu de Raridades
finalHTML += `<div style="display: flex; justify-content: center; gap: 6px; flex-wrap: wrap; max-width: 480px; margin: 10px auto 25px auto; font-family: sans-serif;">`;
listaRaridades.forEach(r => {
  const conf = configsRaridade[r];
  const estaAtiva = (r === raridade);
  const estiloAba = estaAtiva 
    ? `background: ${conf.cor}; color: #ffffff; border: 1px solid ${conf.cor}; font-weight: bold; box-shadow: 0 0 10px ${conf.cor}60;`
    : `background: #1e293b; color: #94a3b8; border: 1px solid rgba(255,255,255,0.05); opacity: 0.5;`;
  finalHTML += `<div style="${estiloAba} padding: 6px 12px; border-radius: 8px; font-size: 10px; text-transform: uppercase; letter-spacing: 0.5px; display: flex; align-items: center; gap: 4px;">${conf.icone} ${r}</div>`;
});
finalHTML += `</div>`;

// B. Card Principal
finalHTML += `<div style="background: rgba(15, 23, 42, 0.8); border: 1px solid ${corNeon}40; border-radius: 20px; padding: 40px 20px; text-align: center; margin-bottom: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); backdrop-filter: blur(10px); font-family: sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; width: 100%; max-width: 480px; margin: 20px auto;">`;
finalHTML += `  <div style="width: 220px; height: 220px; background: ${backgroundCirculo}; border-radius: 50%; display: flex; justify-content: center; align-items: center; border: 3px solid ${corNeon}; box-shadow: 0 0 30px ${corNeon}40, inset 0 0 20px rgba(0,0,0,0.3); position: relative; margin-bottom: 20px; overflow: hidden;">`;
finalHTML += `    <img src="sprites/${sprite}" alt="${nome}" style="width: 180px; height: 180px; object-fit: contain; filter: drop-shadow(0 10px 15px rgba(0,0,0,0.5)); position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 5;" onerror="this.style.display='none'">`;
finalHTML += `  </div>`;
finalHTML += `  <span style="font-size: 0.85rem; color: #ffffff; font-weight: bold; background: ${configRaridade.cor}25; padding: 4px 14px; border-radius: 20px; border: 1px solid ${configRaridade.cor}60; display: inline-block; margin-bottom: 12px; letter-spacing: 0.5px;">${configRaridade.icone} ${raridade}</span>`;
finalHTML += `  <br><span style="font-size: 1rem; color: ${corNeon}; font-family: monospace; font-weight: bold; background: ${corNeon}20; padding: 4px 16px; border-radius: 12px; border: 1px solid ${corNeon}50; display: inline-block; margin-bottom: 10px;">#${id}</span>`;
finalHTML += `  <h1 style="font-size: 3rem; font-weight: 900; margin: 10px 0; color: white; text-shadow: 0 0 20px ${corNeon}30; letter-spacing: 1px; line-height: 1;">${nome}</h1>`;
finalHTML += `  <div style="display: flex; justify-content: center; align-items: center; gap: 15px; margin-top: 15px;">`;
finalHTML += `    <img src="Tipagens/${tipo1}.png" alt="${tipo1}" style="width: 100px; height: auto; object-fit: contain;" onerror="this.style.display='none'">`;
if (tipo2 && tipo2 !== "") {
  finalHTML += `    <img src="Tipagens/${tipo2}.png" alt="${tipo2}" style="width: 100px; height: auto; object-fit: contain;" onerror="this.style.display='none'">`;
}
finalHTML += `  </div>`;
finalHTML += `</div>`;

// C. Tabela de Stats
finalHTML += `<section style="background: #f7fafc; border: 1px solid #cbd5e0; border-radius: 12px; padding: 20px; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; max-width: 480px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); margin: 20px auto;">`;
finalHTML += `  <h2 style="display: flex; align-items: center; gap: 8px; font-size: 18px; font-weight: 600; color: #1a202c; margin: 0 0 20px 0; border: none; padding: 0;">`;
finalHTML += `    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#2d3748" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"></path><rect x="7" y="11" width="3" height="6"></rect><rect x="13" y="7" width="3" height="10"></rect></svg>`;
finalHTML += `    Holobeast Base Stats`;
finalHTML += `  </h2>`;
finalHTML += `  <div style="display: flex; flex-direction: column; gap: 12px;">`;

const renderRow = (label, val, bg, minVal, maxVal) => {
  let row = '    <div style="display: flex; align-items: center; font-size: 14px;">';
  row += '      <span style="width: 75px; font-weight: 600; color: #2d3748; text-align: left;">' + label + '</span>';
  row += '      <span style="width: 40px; font-weight: 700; color: #1a202c; text-align: center;">' + val + '</span>';
  row += '      <div style="flex: 1; background: #cbd5e0; height: 12px; border-radius: 6px; margin: 0 15px; overflow: hidden; position: relative;">';
  row += '        <div style="width: ' + pct(val) + '; background: ' + bg + '; height: 100%; border-radius: 6px;"></div>';
  row += '      </div>';
  row += '      <span style="width: 40px; text-align: center; color: #2d3748; font-weight: 600; font-size: 13px;">' + minVal + '</span>';
  row += '      <span style="width: 40px; text-align: center; color: #2d3748; font-weight: 600; font-size: 13px;">' + maxVal + '</span>';
  row += '    </div>';
  return row;
};

finalHTML += renderRow("HP", hp, "#007a5e", (hp * 2) + 110, (hp * 2) + 204);
finalHTML += renderRow("Atk", atk, "#b81d13", (atk * 2) + 5, (atk * 2) + 50);
finalHTML += renderRow("Defense", defense, "#d06a00", (defense * 2) + 5, (defense * 2) + 50);
finalHTML += renderRow("Mag. Atk", mag_atk, "#006680", (mag_atk * 2) + 5, (mag_atk * 2) + 50);
finalHTML += renderRow("Mag. Def", mag_def, "#0f2547", (mag_def * 2) + 5, (mag_def * 2) + 50);
finalHTML += renderRow("Speed", speed, "#5a2493", (speed * 2) + 5, (speed * 2) + 50);

finalHTML += '    <div style="border-top: 1px solid #cbd5e0; margin: 8px 0 4px 0;"></div>';
finalHTML += '    <div style="display: flex; align-items: center; font-size: 14px; font-weight: bold;">';
finalHTML += '      <span style="width: 75px; color: #1a202c; text-align: left;">Total</span>';
finalHTML += '      <span style="width: 40px; color: #1a202c; text-align: center;">' + total + '</span>';
finalHTML += '      <div style="flex: 1; margin: 0 15px;"></div>';
finalHTML += '      <span style="width: 40px; text-align: center; color: #4a5568; font-size: 12px; font-weight: 700;">Min</span>';
finalHTML += '      <span style="width: 40px; text-align: center; color: #4a5568; font-size: 12px; font-weight: 700;">Max</span>';
finalHTML += '    </div>';
finalHTML += '  </div>';
finalHTML += '</section>';

// Renderiza a parte gráfica do card e dos stats
dv.el("div", finalHTML);
```

```dataviewjs
// Pega as propriedades do topo da nota atual
const current = dv.current();

const descricao = current.descricao || "Nenhuma descrição disponível.";
const categoria = current.categoria || "Não informada";
const altura = current.altura || "???";
const peso = current.peso || "???";
const habilidade = current.habilidade || "Nenhuma";
const gender = current.gender || "Desconhecido";
const evolucoes = current.evolucoes || "Não possui.";
const omega_evolucoes = current.omega_evolucoes || "Não possui.";

// Renderiza o painel aplicando os valores de forma automática e com texto escuro nítido
dv.el("div", `
<div style="max-width: 480px; margin: 30px auto; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; line-height: 1.6; background: rgba(240, 244, 248, 0.9); border: 2px solid #cbd5e1; border-radius: 20px; padding: 25px; box-shadow: 0 10px 25px rgba(0,0,0,0.15);">

  <!-- SEÇÃO: DESCRIÇÃO -->
  <h2 style="margin: 0 0 10px 0; font-size: 19px; font-weight: 800; color: #1e293b; display: flex; align-items: center; gap: 8px; border: none; padding: 0;">
    <span>📜</span> Descrição
  </h2>
  <p style="margin: 0; color: #0f172a; font-weight: 700; font-size: 14.5px;">${descricao}</p>

  <hr style="border: 0; border-top: 2px solid #cbd5e1; margin: 20px 0;">

  <!-- SEÇÃO: INFORMAÇÕES -->
  <h2 style="margin: 0 0 14px 0; font-size: 19px; font-weight: 800; color: #1e293b; display: flex; align-items: center; gap: 8px; border: none; padding: 0;">
    <span>📊</span> Informações
  </h2>
  <ul style="padding: 0; margin: 0; list-style: none; display: flex; flex-direction: column; gap: 10px; font-size: 14.5px;">
    <li style="display: flex; justify-content: space-between; border-bottom: 1px dashed #cbd5e1; padding-bottom: 6px;"><strong style="color: #1e293b; font-weight: 800;">Categoria:</strong> <span style="color: #0f172a; font-weight: 700;">${categoria}</span></li>
    <li style="display: flex; justify-content: space-between; border-bottom: 1px dashed #cbd5e1; padding-bottom: 6px;"><strong style="color: #1e293b; font-weight: 800;">Altura:</strong> <span style="color: #0f172a; font-weight: 700;">${altura}</span></li>
    <li style="display: flex; justify-content: space-between; border-bottom: 1px dashed #cbd5e1; padding-bottom: 6px;"><strong style="color: #1e293b; font-weight: 800;">Peso:</strong> <span style="color: #0f172a; font-weight: 700;">${peso}</span></li>
    <li style="display: flex; justify-content: space-between; border-bottom: 1px dashed #cbd5e1; padding-bottom: 6px;"><strong style="color: #1e293b; font-weight: 800;">Habilidade:</strong> <span style="color: #0f172a; font-weight: 700;">${habilidade}</span></li>
    <li style="display: flex; justify-content: space-between; padding-bottom: 2px;"><strong style="color: #1e293b; font-weight: 800;">Gênero (Gender):</strong> <span style="color: #0f172a; font-weight: 700;">${gender}</span></li>
  </ul>

  <hr style="border: 0; border-top: 2px solid #cbd5e1; margin: 20px 0;">

  <!-- SEÇÃO: EVOLUÇÕES -->
  <h2 style="margin: 0 0 10px 0; font-size: 19px; font-weight: 800; color: #1e293b; display: flex; align-items: center; gap: 8px; border: none; padding: 0;">
    <span>🔄</span> Evoluções
  </h2>
  <p style="margin: 0; color: #0f172a; font-weight: 700; font-size: 14.5px;">${evolucoes}</p>

  <hr style="border: 0; border-top: 2px solid #cbd5e1; margin: 20px 0;">

  <!-- SEÇÃO: ÔMEGA EVOLUÇÕES -->
  <h2 style="margin: 0 0 10px 0; font-size: 19px; font-weight: 800; color: #1e293b; display: flex; align-items: center; gap: 8px; border: none; padding: 0;">
    <span>Ω</span> Ômega Evoluções
  </h2>
  <p style="margin: 0; color: #0f172a; font-weight: 700; font-size: 14.5px;">${omega_evolucoes}</p>

</div>
`);
```





