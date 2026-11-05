---
dg-publish: true
---

```dataviewjs
// 1. Configurações Globais e Filtros de Criaturas
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

// Busca todos os Holobeasts válidos no cofre
const todosHolos = dv.pages().where(p => p.id !== undefined && p.raridade !== undefined).sort(p => p.id, 'asc');

// Estado interno da aba (começa na primeira que encontrar criaturas ou em "Comum")
let raridadeSelecionada = "Comum";

// Containers HTML onde o Dataview vai injetar as renderizações
const containerMenu = this.container.createEl("div");
const containerLista = this.container.createEl("div");

// 2. FUNÇÃO INTERATIVA PARA DESENHAR OS BOTÕES (ABAS)
function renderizarAbas() {
  containerMenu.innerHTML = "";
  
  let abasHTML = `<div style="display: flex; justify-content: center; gap: 6px; flex-wrap: wrap; max-width: 500px; margin: 10px auto 25px auto; font-family: sans-serif;">`;
  
  listaRaridades.forEach(r => {
    const conf = configsRaridade[r];
    const estaAtiva = (r === raridadeSelecionada);
    
    // Filtra quantos monstros têm essa raridade cadastrada no cofre
    const qtd = todosHolos.where(p => p.raridade === r).length;

    const estiloAba = estaAtiva 
      ? `background: ${conf.cor}; color: #ffffff; border: 1px solid ${conf.cor}; font-weight: bold; box-shadow: 0 0 12px ${conf.cor}60; transform: scale(1.05);`
      : `background: #1e293b; color: #94a3b8; border: 1px solid rgba(255,255,255,0.05); opacity: 0.7;`;
    
    abasHTML += `<button id="btn-${r}" style="${estiloAba} padding: 8px 14px; border-radius: 8px; font-size: 11px; text-transform: uppercase; cursor: pointer; letter-spacing: 0.5px; display: flex; align-items: center; gap: 4px; transition: all 0.2s;">
      ${conf.icone} ${r} <span style="background: rgba(0,0,0,0.2); padding: 1px 5px; border-radius: 4px; margin-left: 2px;">${qtd}</span>
    </button>`;
  });
  
  abasHTML += `</div>`;
  containerMenu.innerHTML = abasHTML;

  // Adiciona os gatilhos de clique em cada botão javascript de forma limpa
  listaRaridades.forEach(r => {
    const btn = containerMenu.querySelector(`#btn-${r}`);
    if (btn) {
      btn.addEventListener("click", () => {
        raridadeSelecionada = r;
        renderizarAbas();
        renderizarListaFiltrada();
      });
    }
  });
}

// 3. FUNÇÃO PARA DESENHAR A LISTA EM RODA (CONVEXA) FILTRADA PELA ABA
function renderizarListaFiltrada() {
  containerLista.innerHTML = "";
  
  const holosFiltrados = todosHolos.where(p => p.raridade === raridadeSelecionada);
  
  if (holosFiltrados.length === 0) {
    containerLista.innerHTML = `<div style="text-align: center; color: #64748b; font-family: sans-serif; margin: 30px auto; font-size: 14px;">Nenhum Holobeast cadastrado como ${raridadeSelecionada} ainda.</div>`;
    return;
  }

  let listaHTML = `<div style="display: flex; flex-direction: column; gap: 14px; max-width: 420px; margin: 20px auto; font-family: -apple-system, BlinkMacSystemFont, sans-serif; padding-left: 40px;">`;

  holosFiltrados.forEach((p, index) => {
    // Aplica o efeito de roda convexa (itera projeção para fora e para dentro)
    let offset = (index % 2 === 0) ? "-15px" : "-35px";
    let zIndex = (index % 2 === 0) ? "1" : "10";
    
    // Cores de fundo e bordas laterais baseadas no tipo principal do Holo
    const tipo = p.tipo1 || "Normal";
    const corTipo = coresTipos[tipo] || "#909296";
    
    // Suaviza o fundo para não brigar com o modo escuro
    let bgColor = "rgba(30, 41, 59, 0.4)"; 
    let textColor = "#e2e8f0";

    listaHTML += `
    <a href="${p.file.name}" style="text-decoration: none; display: flex; align-items: center; background: ${bgColor}; border-radius: 30px 4px 4px 30px; padding: 6px 16px 6px 8px; border-left: 6px solid ${corTipo}; border-top: 1px solid rgba(255,255,255,0.03); box-shadow: 0 4px 10px rgba(0,0,0,0.15); transform: translateX(${offset}); z-index: ${zIndex}; transition: all 0.2s;">
      <div style="background: #1e293b; width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: inset 0 2px 4px rgba(0,0,0,0.3); border: 1px solid rgba(255,255,255,0.05);">
        <img src="sprites/${p.sprite}" style="width: 36px; height: 36px; object-fit: contain;" onerror="this.style.display='none'">
      </div>
      <div style="margin-left: 14px; flex: 1;">
        <span style="font-size: 11px; font-weight: 700; color: #64748b; display: block; letter-spacing: 0.5px;">#${p.id}</span>
        <span style="font-size: 15px; font-weight: 700; color: ${textColor};">${p.nome || p.file.name}</span>
      </div>
      <span style="background: ${corTipo}25; color: ${corTipo}; border: 1px solid ${corTipo}60; font-size: 11px; font-weight: 700; padding: 3px 10px; border-radius: 8px; text-transform: uppercase; letter-spacing: 0.5px;">${tipo}</span>
    </a>`;
  });

  listaHTML += `</div>`;
  containerLista.innerHTML = listaHTML;
}

// Inicializa as duas visualizações na tela de uma vez
renderizarAbas();
renderizarListaFiltrada();
```
