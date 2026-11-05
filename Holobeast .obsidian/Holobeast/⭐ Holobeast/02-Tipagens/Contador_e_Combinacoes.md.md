---
dg-publish: true
---

# 📊 Painel Geral de Tipagens e Combinações

```dataviewjs
// 1. Lista Oficial dos seus 22 Tipos (Nomes exatos das suas pastas/imagens)
const todosTipos = [
  "Normal", "Lutador", "Voador", "Veneno", "Terra", "Pedra", "Inseto", "Aço", 
  "Fogo", "Fantasma", "Água", "Planta", "Elétrico", "Gelo", "Trevas", "Luz", 
  "Fantasia", "Mental", "Cybernexo", "Sonoro", "Cosmos", "Ancestral"
];

// Busca todas as notas de monstros válidas (que possuem a propriedade id)
const holos = dv.pages().where(p => p.id !== undefined);

// Objetos para guardar as contagens
let tipoUnicoContagem = {};
let tipoDuploContagem = {};
let combinacoesExistentes = new Set();

// Inicializa o contador de tipos únicos zerado para todos
todosTipos.forEach(t => tipoUnicoContagem[t] = 0);

// 2. PROCESSAMENTO E LEITURA DOS MONSTROS DO SEU COFRE
holos.forEach(p => {
  const t1 = p.tipo1 ? p.tipo1.trim() : "";
  const t2 = p.tipo2 ? p.tipo2.trim() : "";

  if (t1 && (!t2 || t2 === "")) {
    // Se só tem tipo 1, conta como Tipo Único
    if (tipoUnicoContagem[t1] !== undefined) {
      tipoUnicoContagem[t1]++;
    }
  } else if (t1 && t2) {
    // Se tem os dois tipos, ordena em ordem alfabética para evitar duplicatas (ex: "Fogo+Água" e "Água+Fogo" contarem como o mesmo)
    const comb = [t1, t2].sort().join(" + ");
    combinacoesExistentes.add(comb);
    tipoDuploContagem[comb] = (tipoDuploContagem[comb] || 0) + 1;
  }
});

// 3. ENCONTRAR COMBINAÇÕES QUE ESTÃO FALTANDO
let combinacoesFaltando = [];
for (let i = 0; i < todosTipos.length; i++) {
  for (let j = i + 1; j < todosTipos.length; j++) {
    const combPossivel = [todosTipos[i], todosTipos[j]].sort().join(" + ");
    if (!combinacoesExistentes.has(combPossivel)) {
      combinacoesFaltando.push(combPossivel);
    }
  }
}

// === RENDERIZAÇÃO DOS PAINÉIS COMPLETO EM TELA ===
let html = `<div style="font-family: system-ui, sans-serif; color: #e2e8f0; max-width: 600px; margin: 0 auto; display: flex; flex-direction: column; gap: 25px;">`;

// Bloco 1: Contagem de Tipos Puros
html += `  <div style="background: rgba(30, 41, 59, 0.4); border: 1px solid rgba(148, 163, 184, 0.15); border-radius: 12px; padding: 20px;">
    <h3 style="margin: 0 0 15px 0; color: #00B6FF; font-size: 16px;">⚪ Contagem de Holobeasts Puro (Tipo Único)</h3>
    <div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(130px, 1fr)); gap: 8px;">`;
todosTipos.forEach(t => {
  html += `    <div style="background: #1e293b; padding: 6px 10px; border-radius: 6px; display: flex; justify-content: space-between; font-size: 13px; border: 1px solid rgba(255,255,255,0.03);">
      <span style="font-weight: 500;">${t}</span>
      <span style="color: #00B6FF; font-weight: bold;">${tipoUnicoContagem[t]}</span>
    </div>`;
});
html += `    </div>
  </div>`;

// Bloco 2: Combinações Duplas Cadastradas
html += `  <div style="background: rgba(30, 41, 59, 0.4); border: 1px solid rgba(148, 163, 184, 0.15); border-radius: 12px; padding: 20px;">
    <h3 style="margin: 0 0 15px 0; color: #63BC5A; font-size: 16px;">⚔️ Combinações Duplas Existentes (${combinacoesExistentes.size})</h3>`;
if (combinacoesExistentes.size === 0) {
  html += `  <p style="font-size: 13px; color: #64748b; margin: 0;">Nenhuma criatura com tipo duplo cadastrada ainda.</p>`;
} else {
  html += `  <div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 8px;">`;
  Object.keys(tipoDuploContagem).sort().forEach(comb => {
    html += `    <div style="background: #1e293b; padding: 6px 10px; border-radius: 6px; display: flex; justify-content: space-between; font-size: 12px; border: 1px solid rgba(255,255,255,0.03);">
        <span style="color: #cbd5e1;">${comb}</span>
        <span style="color: #63BC5A; font-weight: bold;">${tipoDuploContagem[comb]}</span>
      </div>`;
  });
  html += `  </div>`;
}
html += `  </div>`;

// Bloco 3: Combinações que Faltam (Sugestões de Criação)
html += `  <div style="background: rgba(30, 41, 59, 0.4); border: 1px solid rgba(148, 163, 184, 0.15); border-radius: 12px; padding: 20px;">
    <h3 style="margin: 0 0 10px 0; color: #FF9D55; font-size: 16px;">🧩 Combinações de Tipo Duplo que Faltam (${combinacoesFaltando.length})</h3>
    <p style="font-size: 12px; color: #94a3b8; margin: 0 0 15px 0;">Estes tipos duplos ainda não possuem nenhum Holobeast criado no seu banco de dados:</p>
    <div style="max-height: 200px; overflow-y: auto; border: 1px solid rgba(255,255,255,0.05); padding: 10px; border-radius: 6px; background: #0f172a; display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 6px;">`;
combinacoesFaltando.forEach(comb => {
  html += `    <div style="font-size: 12px; color: #94a3b8; padding: 3px 6px; background: rgba(255,255,255,0.02); border-radius: 4px;">❌ ${comb}</div>`;
});
html += `    </div>
  </div>`;

html += `</div>`;

// Entrega os blocos interativos gerados
dv.el("div", html);
```
