---
dg-publish: true
id: "000"
nome: Nome_Do_Holo
tipo1:
tipo2: ""
hp: 50
atk: 50
defense: 50
mag_atk: 50
mag_def: 50
speed: 50
sprite: " Grizerus.png"
---

```dataviewjs
const coresTipos = {
  "Normal": "#A8A77A", "Lutador": "#CD154C", "Voador": "#89AAE3", "Veneno": "#B567CE",
  "Terra": "#D97845", "Pedra": "#C5B78C", "Inseto": "#91C12F", "Aço": "#5A8EA2",
  "Fogo": "#FF9D55", "Fantasma": "#5269AD", "Água": "#5090D6", "Planta": "#63BC5A",
  "Elétrico": "#EFC600", "Gelo": "#73CEC0", "Trevas": "#5A5465", "Luz": "#FFD700",
  "Fantasia": "#EC8FE6", "Mental": "#F7A2A0", "Cybernexo": "#00B6FF", "Sonoro": "#D6F7FF",
  "Cosmos": "#000350", "Ancestral": "#DCCCB8"
};

const current = dv.current();
const id = current.id || "000";
const nome = current.nome || current.file.name;
const tipo1 = current.tipo1 || "Normal";
const tipo2 = current.tipo2 || null;
const sprite = current.sprite ? current.sprite.trim() : "";

const corNeon = coresTipos[tipo1] || "#00E5FF";

const hp = Number(current.hp) || 0;
const atk = Number(current.atk) || 0;
const defense = Number(current.defense) || 0;
const mag_atk = Number(current.mag_atk) || 0;
const mag_def = Number(current.mag_def) || 0;
const speed = Number(current.speed) || 0;
const total = hp + atk + defense + mag_atk + mag_def + speed;

const pct = (stat) => Math.min((stat / 255) * 100, 100).toFixed(2) + "%";

let cardHTML = '<div style="background: rgba(15, 23, 42, 0.8); border: 1px solid ' + corNeon + '40; border-radius: 20px; padding: 40px 20px; text-align: center; margin-bottom: 30px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); backdrop-filter: blur(10px); font-family: sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; width: 100%; max-width: 480px; margin: 20px auto;">';
cardHTML += '  <div style="width: 220px; height: 220px; background: radial-gradient(circle at 30% 30%, #1e293b, #0f172a); border-radius: 50%; display: flex; justify-content: center; align-items: center; border: 3px solid ' + corNeon + 'aa; box-shadow: 0 0 30px ' + corNeon + '40, inset 0 0 20px rgba(0,0,0,0.5); position: relative; margin-bottom: 20px;">';
cardHTML += '    <img src="./sprites/' + sprite + '" alt="' + nome + '" style="width: 180px; height: 180px; object-fit: contain; filter: drop-shadow(0 10px 15px rgba(0,0,0,0.5)); position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);" onerror="this.style.display=\'none\'">';
cardHTML += '  </div>';
cardHTML += '  <span style="font-size: 1rem; color: ' + corNeon + '; font-family: monospace; font-weight: bold; background: ' + corNeon + '20; padding: 4px 16px; border-radius: 12px; border: 1px solid ' + corNeon + '50; display: inline-block; margin-bottom: 10px;">#' + id + '</span>';
cardHTML += '  <h1 style="font-size: 3rem; font-weight: 900; margin: 10px 0; color: white; text-shadow: 0 0 20px ' + corNeon + '60; letter-spacing: 1px; line-height: 1;">' + nome + '</h1>';
cardHTML += '  <div style="display: flex; justify-content: center; align-items: center; gap: 15px; margin-top: 15px;">';
cardHTML += '    <img src="./Tipagens/' + tipo1 + '.png" alt="' + tipo1 + '" style="width: 100px; height: auto; object-fit: contain;" onerror="this.style.display=\'none\'">';
if (tipo2 && tipo2.trim() !== "") {
  cardHTML += '    <img src="./Tipagens/' + tipo2 + '.png" alt="' + tipo2 + '" style="width: 100px; height: auto; object-fit: contain;" onerror="this.style.display=\'none\'">';
}
cardHTML += '  </div>';
cardHTML += '</div>';

let statsHTML = '<section style="background: #f7fafc; border: 1px solid #cbd5e0; border-radius: 12px; padding: 20px; font-family: -apple-system, BlinkMacSystemFont, \'Segoe UI\', Roboto, sans-serif; max-width: 480px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); margin: 20px auto;">';
statsHTML += '  <h2 style="display: flex; align-items: center; gap: 8px; font-size: 18px; font-weight: 600; color: #1a202c; margin: 0 0 20px 0; border: none; padding: 0;">';
statsHTML += '    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#2d3748" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"></path><rect x="7" y="11" width="3" height="6"></rect><rect x="13" y="7" width="3" height="10"></rect></svg>';
statsHTML += '    Holobeast Base Stats';
statsHTML += '  </h2>';
statsHTML += '  <div style="display: flex; flex-direction: column; gap: 12px;">';

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

statsHTML += renderRow("HP", hp, "#007a5e", (hp * 2) + 110, (hp * 2) + 204);
statsHTML += renderRow("Atk", atk, "#b81d13", (atk * 2) + 5, (atk * 2) + 50);
statsHTML += renderRow("Defense", defense, "#d06a00", (defense * 2) + 5, (defense * 2) + 50);
statsHTML += renderRow("Mag. Atk", mag_atk, "#006680", (mag_atk * 2) + 5, (mag_atk * 2) + 50);
statsHTML += renderRow("Mag. Def", mag_def, "#0f2547", (mag_def * 2) + 5, (mag_def * 2) + 50);
statsHTML += renderRow("Speed", speed, "#5a2493", (speed * 2) + 5, (speed * 2) + 50);

statsHTML += '    <div style="border-top: 1px solid #cbd5e0; margin: 8px 0 4px 0;"></div>';
statsHTML += '    <div style="display: flex; align-items: center; font-size: 14px; font-weight: bold;">';
statsHTML += '      <span style="width: 75px; color: #1a202c; text-align: left;">Total</span>';
statsHTML += '      <span style="width: 40px; color: #1a202c; text-align: center;">' + total + '</span>';
statsHTML += '      <div style="flex: 1; margin: 0 15px;"></div>';
statsHTML += '      <span style="width: 40px; text-align: center; color: #4a5568; font-size: 12px; font-weight: 700;">Min</span>';
statsHTML += '      <span style="width: 40px; text-align: center; color: #4a5568; font-size: 12px; font-weight: 700;">Max</span>';
statsHTML += '    </div>';
statsHTML += '  </div>';
statsHTML += '</section>';

dv.el("div", cardHTML + statsHTML);
```

### Como aplicar para criar um monstro novo:
1. Crie uma nota em branco para o seu monstrinho.
2. Use o comando de inserir template do Obsidian e escolha o **`Template Holobeast`**.
3. Altere os valores no topo (ID, Nome, Tipos, Stats e o nome do arquivo da imagem do Sprite). 
4. Assim que você mudar para o modo de **Visualização (Reading Mode)**, o DataviewJS vai calcular os valores e renderizar o card cyberpunk com a cor correspondente.

<FollowUp>
O template salvou e carregou perfeitamente a partir da pasta **`99 - Templates`**?
</FollowUp>
