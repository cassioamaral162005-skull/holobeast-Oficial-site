---
dg-publish: true
dg-home: true
---

```dataviewjs
---
dg-publish: true
---

```dataviewjs
// Força o Dataview a atualizar o bloco sempre que houver alteração no cofre
dv.container.className += " dataview-refresh-trigger";

// 1. Dicionário Oficial de Cores dos Tipos de Holobeast
const coresTipos = {
  "Normal": "#A8A77A", "Lutador": "#CD154C", "Voador": "#89AAE3", "Venenoso": "#B567CE",
  "Terrestre": "#D97845", "Pedra": "#C5B78C", "Inseto": "#91C12F", "Aço": "#5A8EA2",
  "Fogo": "#FF9D55", "Fantasma": "#5269AD", "Água": "#5090D6", "Planta": "#63BC5A",
  "Elétrico": "#EFC600", "Gelo": "#73CEC0", "Trevas": "#5A5465", "Luz": "#FFD700",
  "Fantasia": "#EC8FE6", "Mental": "#F7A2A0", "Cybernexo": "#00B6FF", "Sonoro": "#D6F7FF",
  "Cosmos": "#000350", "Ancestral": "#DCCCB8"
};

const nomePaginaAtual = dv.current().file.name;

// 2. Busca em TEMPO REAL os Holobeasts da sua pasta oficial da Holopédia
const holos = dv.pages('"Holobeast/⭐ Holobeast/01├──🐾Holopédia"')
    .where(p => p.id !== undefined)
    .sort(p => Number(p.id), 'asc');

if (holos.length === 0) {
    dv.el("div", "<p style='color: #64748b; font-family: sans-serif; text-align: center; margin: 20px auto;'>Nenhum Holobeast cadastrado na pasta da Holopédia ainda.</p>");
} else {
    let html = `<div style="display: flex; flex-direction: column; gap: 14px; max-width: 420px; margin: 20px auto; font-family: -apple-system, BlinkMacSystemFont, sans-serif; padding-left: 45px; position: relative;">`;

    holos.forEach((p, index) => {
        const idFormated = String(p.id || 0).padStart(3, '0');
        const nome = p.nome || p.file.name;
        const tipo1 = p.tipo1 || "Normal";
        const sprite = p.sprite ? p.sprite.trim() : "";
        const corHex = coresTipos[tipo1] || "#909296";
        
        // Verifica se este item específico é o Holobeast que está aberto na tela
        const ehOFoco = (p.file.name === nomePaginaAtual);

        // MÁGICA DA RODA INFINITA:
        // Se for o Holo aberto na tela, ele ganha o destaque máximo absoluto.
        // Se não for, o sistema calcula uma ondulação a cada 3 itens (index % 3) para fazer o desenho da roda perfeito na tela.
        let offset = "-15px";
        let zIndex = "1";
        let shadow = "0 2px 6px rgba(0,0,0,0.2)";
        let scale = "1";
        let background = "rgba(30, 41, 59, 0.45)";
        let borderNeon = "1px solid rgba(255,255,255,0.04)";

        if (ehOFoco) {
            offset = "-45px"; // Joga muito para fora
            zIndex = "30";
            shadow = `0 0 25px ${corHex}50, 0 8px 20px rgba(0,0,0,0.5)`;
            scale = "1.06"; // Dá zoom no item ativo
            background = "rgba(30, 41, 59, 0.85)";
            borderNeon = `1px solid ${corHex}aa`;
        } else if (index % 3 === 1) {
            // Cria o desenho curvado para os outros itens da lista, mantendo o balanço da roda
            offset = "-30px";
            zIndex = "10";
            shadow = "0 4px 12px rgba(0,0,0,0.3)";
            scale = "0.98";
        }

        html += `
        <a href="${p.file.name}" style="text-decoration: none; display: flex; align-items: center; background: ${background}; border-radius: 30px 4px 4px 30px; padding: 7px 16px 7px 8px; border-left: 6px solid ${corHex}; border-top: ${borderNeon}; border-right: ${borderNeon}; border-bottom: ${borderNeon}; box-shadow: ${shadow}; transform: translateX(${offset}) scale(${scale}); z-index: ${zIndex}; transition: all 0.2s ease-in-out;">
          <!-- Círculo do Sprite -->
          <div style="background: #141e30; width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: inset 0 2px 4px rgba(0,0,0,0.5); border: 1px solid ${ehOFoco ? corHex : 'rgba(255,255,255,0.08)'}; overflow: hidden;">
            <img src="sprites/${sprite}" style="width: 36px; height: 36px; object-fit: contain;" onerror="this.style.display='none'">
          </div>
          <!-- Textos informativos -->
          <div style="margin-left: 14px; flex: 1;">
            <span style="font-size: 10px; font-weight: 800; color: ${ehOFoco ? corHex : '#475569'}; display: block; letter-spacing: 0.5px;">#${idFormated}</span>
            <span style="font-size: 14.5px; font-weight: 700; color: ${ehOFoco ? '#ffffff' : '#94a3b8'};">${nome}</span>
          </div>
          <!-- Badge do Tipo Elemental -->
          <span style="background: ${corHex}${ehOFoco ? '30' : '15'}; color: ${corHex}; border: 1px solid ${corHex}60; font-size: 10.5px; font-weight: 700; padding: 3px 10px; border-radius: 10px; text-transform: uppercase; letter-spacing: 0.5px;">${tipo1}</span>
        </a>`;
    });

    html += `</div>`;
    dv.el("div", html);
}
```

```

<div style="position: relative; width: 100%; max-width: 480px; margin: 0 auto; overflow: hidden; border-radius: 16px; box-shadow: 0 10px 25px rgba(0,0,0,0.1);">
  
  <!-- O Design de Fundo vindo do Behance -->
  <iframe 
    src="https://behance.net" 
    height="750" 
    width="100%" 
    frameborder="0" 
    allowfullscreen 
    lazyload
    style="display: block; pointer-events: none; z-index: 1;">
  </iframe>

  <!-- AREA CLICÁVEL (Botão invisível por cima do monstrinho da tela) -->
  <!-- Altere o href para o caminho exato da nota do seu Holobeast -->
  <a href="Grizerus" style="position: absolute; top: 180px; left: 40px; width: 160px; height: 160px; background: rgba(255, 0, 0, 0); z-index: 10; border-radius: 50%; cursor: pointer;" title="Ver Grizerus"></a>

  <!-- Você pode copiar a linha de cima e criar outros botões mudando o 'top' e o 'left' para posicionar em outros monstros da imagem -->

</div>

<div style="display: flex; flex-direction: column; gap: 14px; max-width: 420px; margin: 20px auto; font-family: -apple-system, BlinkMacSystemFont, sans-serif; padding-left: 40px;">

  <!-- Item Superior (Entrando na tela) -->
  <a href="Holo-1" style="text-decoration: none; display: flex; align-items: center; background: #fff5f5; border-radius: 30px 4px 4px 30px; padding: 6px 16px 6px 8px; border-left: 6px solid #fa5252; box-shadow: 0 2px 5px rgba(0,0,0,0.05); transform: translateX(-15px);">
    <div style="background: #ffffff; width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
      <img src="sprites/fogo.png" style="width: 36px; height: 36px; object-fit: contain;" onerror="this.style.display='none'">
    </div>
    <div style="margin-left: 14px; flex: 1;">
      <span style="font-size: 11px; font-weight: 700; color: #ffc9c9; display: block;">#001</span>
      <span style="font-size: 15px; font-weight: 700; color: #495057;">Pyroshell</span>
    </div>
    <span style="background: #fa5252; color: #ffffff; font-size: 11px; font-weight: 700; padding: 4px 12px; border-radius: 12px;">Fogo</span>
  </a>

  <!-- Item do MEIO (Projetado PARA FORA - Máximo destaque para a esquerda) -->
  <a href="Grizerus" style="text-decoration: none; display: flex; align-items: center; background: #f1f3f5; border-radius: 30px 4px 4px 30px; padding: 6px 16px 6px 8px; border-left: 6px solid #909296; box-shadow: 0 4px 10px rgba(0,0,0,0.08); transform: translateX(-35px); z-index: 10;">
    <div style="background: #ffffff; width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);">
      <img src="sprites/Grizerus.png" style="width: 36px; height: 36px; object-fit: contain;" alt="Grizerus">
    </div>
    <div style="margin-left: 14px; flex: 1;">
      <span style="font-size: 11px; font-weight: 700; color: #adb5bd; display: block;">#000</span>
      <span style="font-size: 15px; font-weight: 700; color: #212529;">Grizerus</span>
    </div>
    <span style="background: #909296; color: #ffffff; font-size: 11px; font-weight: 700; padding: 4px 12px; border-radius: 12px;">Normal</span>
  </a>

  <!-- Item Inferior (Entrando na tela novamente) -->
  <a href="Holo-2" style="text-decoration: none; display: flex; align-items: center; background: #e7f5ff; border-radius: 30px 4px 4px 30px; padding: 6px 16px 6px 8px; border-left: 6px solid #228be6; box-shadow: 0 2px 5px rgba(0,0,0,0.05); transform: translateX(-15px);">
    <div style="background: #ffffff; width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
      <img src="sprites/agua.png" style="width: 36px; height: 36px; object-fit: contain;" onerror="this.style.display='none'">
    </div>
    <div style="margin-left: 14px; flex: 1;">
      <span style="font-size: 11px; font-weight: 700; color: #a5d8ff; display: block;">#002</span>
      <span style="font-size: 15px; font-weight: 700; color: #495057;">Aquaflow</span>
    </div>
    <span style="background: #228be6; color: #ffffff; font-size: 11px; font-weight: 700; padding: 4px 12px; border-radius: 12px;">Água</span>
  </a>

</div>
|