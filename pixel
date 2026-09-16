(() => {
  /* ============================================================
     POKEPIXEL HUNT PANEL - ZEN CONSOLE
     Versão standalone para DevTools
     ============================================================ */

  const APP_ID = "kira-pokepixel-hunt-panel";
  const STORAGE_KEY = "kira_pokepixel_hunt_config";

  // Remove versão anterior
  document.getElementById(APP_ID)?.remove();

  /* =========================
     CONFIGURAÇÃO
     ========================= */

  const DEFAULT_CONFIG = {
    preset: "default",

    autoCapture: {
      enabled: false,
      mode: "quality",
      capsule: "pokeball",
      minQuality: 80,
      commonEnabled: false,
      shinyEnabled: true,
      shinyCapsule: "ultraball",
      speciesFilter: ""
    },

    slots: [
      { widget: "encounters", item: "total", rarity: "all" },
      { widget: "captures", item: "total", rarity: "all" },
      { widget: "quality", item: "best", rarity: "all" },
      { widget: "items", item: "pokeball", rarity: "all" }
    ]
  };

  const PRESETS = {
    default: {
      slots: [
        { widget: "encounters", item: "total", rarity: "all" },
        { widget: "captures", item: "total", rarity: "all" },
        { widget: "quality", item: "best", rarity: "all" },
        { widget: "items", item: "pokeball", rarity: "all" }
      ]
    },

    leveling: {
      slots: [
        { widget: "encounters", item: "total", rarity: "all" },
        { widget: "xp", item: "total", rarity: "all" },
        { widget: "quality", item: "average", rarity: "all" },
        { widget: "rarity", item: "rare", rarity: "rare" }
      ]
    },

    economy: {
      slots: [
        { widget: "items", item: "pokeball", rarity: "all" },
        { widget: "items", item: "greatball", rarity: "all" },
        { widget: "items", item: "ultraball", rarity: "all" },
        { widget: "captures", item: "total", rarity: "all" }
      ]
    },

    capture: {
      slots: [
        { widget: "captures", item: "total", rarity: "all" },
        { widget: "quality", item: "best", rarity: "all" },
        { widget: "rarity", item: "rare", rarity: "rare" },
        { widget: "shiny", item: "total", rarity: "shiny" }
      ]
    }
  };

  function clone(obj) {
    return JSON.parse(JSON.stringify(obj));
  }

  function loadConfig() {
    try {
      const saved = JSON.parse(localStorage.getItem(STORAGE_KEY));

      if (saved) {
        return {
          ...clone(DEFAULT_CONFIG),
          ...saved,
          autoCapture: {
            ...clone(DEFAULT_CONFIG.autoCapture),
            ...(saved.autoCapture || {})
          },
          slots:
            Array.isArray(saved.slots) && saved.slots.length === 4
              ? saved.slots
              : clone(DEFAULT_CONFIG.slots)
        };
      }
    } catch (e) {
      console.warn("[PokePixel Panel] Erro lendo configuração:", e);
    }

    return clone(DEFAULT_CONFIG);
  }

  let config = loadConfig();

  function saveConfig() {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(config));

    console.log(
      "%c[PokePixel Hunt Panel] Configuração salva",
      "color:#8be9fd;font-weight:bold",
      config
    );
  }

  /* =========================
     ROOT + SHADOW DOM
     ========================= */

  const host = document.createElement("div");
  host.id = APP_ID;

  Object.assign(host.style, {
    position: "fixed",
    top: "80px",
    right: "25px",
    zIndex: "2147483647"
  });

  document.documentElement.appendChild(host);

  const root = host.attachShadow({ mode: "open" });

  /* =========================
     CSS
     ========================= */

  const style = document.createElement("style");

  style.textContent = `
    * {
      box-sizing: border-box;
    }

    .app {
      width: 430px;
      max-height: 82vh;

      background:
        linear-gradient(
          145deg,
          rgba(20,24,34,.98),
          rgba(10,13,20,.98)
        );

      border: 1px solid #394257;
      border-radius: 14px;

      color: #e8edf7;

      font-family:
        Inter,
        system-ui,
        -apple-system,
        BlinkMacSystemFont,
        "Segoe UI",
        sans-serif;

      box-shadow:
        0 18px 50px rgba(0,0,0,.55);

      overflow: hidden;
    }

    .header {
      height: 52px;

      display: flex;
      align-items: center;
      justify-content: space-between;

      padding: 0 14px;

      background:
        linear-gradient(
          90deg,
          #20283a,
          #171d2a
        );

      border-bottom: 1px solid #343d52;

      cursor: move;
      user-select: none;
    }

    .title {
      display: flex;
      flex-direction: column;
    }

    .title strong {
      font-size: 14px;
      color: #fff;
    }

    .title span {
      font-size: 10px;
      color: #8f9bb3;
    }

    .headerButtons {
      display: flex;
      gap: 6px;
    }

    button {
      border: 1px solid #3d475e;

      background: #252d3e;
      color: #eaf0ff;

      border-radius: 7px;

      cursor: pointer;

      transition: .15s;
    }

    button:hover {
      background: #303b52;
    }

    .header button {
      width: 30px;
      height: 30px;
      font-size: 15px;
    }

    .content {
      padding: 14px;

      overflow-y: auto;
      max-height: calc(82vh - 52px);
    }

    .section {
      margin-bottom: 14px;

      padding: 12px;

      background: rgba(255,255,255,.025);

      border: 1px solid #30394d;
      border-radius: 10px;
    }

    .sectionTitle {
      display: flex;
      align-items: center;
      justify-content: space-between;

      margin-bottom: 10px;

      font-size: 12px;
      font-weight: 700;

      color: #9eb8ff;

      text-transform: uppercase;
      letter-spacing: .5px;
    }

    label {
      display: block;

      margin: 7px 0 4px;

      font-size: 11px;
      color: #aeb7c9;
    }

    select,
    input[type="text"],
    input[type="number"] {
      width: 100%;

      padding: 7px 8px;

      border: 1px solid #3a455c;
      border-radius: 7px;

      background: #171c28;
      color: #f0f3fa;

      outline: none;
    }

    select:focus,
    input:focus {
      border-color: #688cff;
    }

    .row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }

    .toggleRow {
      display: flex;
      align-items: center;
      justify-content: space-between;

      padding: 7px 0;

      font-size: 12px;

      border-bottom:
        1px solid rgba(255,255,255,.045);
    }

    .toggleRow:last-child {
      border-bottom: 0;
    }

    input[type="checkbox"] {
      width: 17px;
      height: 17px;

      accent-color: #648cff;
    }

    .slots {
      display: grid;
      gap: 8px;
    }

    .slot {
      padding: 9px;

      border: 1px solid #343e53;
      border-radius: 8px;

      background: #151a25;
    }

    .slotTitle {
      font-size: 11px;
      font-weight: 700;

      color: #d7dfff;

      margin-bottom: 7px;
    }

    .actions {
      display: flex;
      gap: 7px;
    }

    .actions button {
      flex: 1;
      padding: 8px;
    }

    .primary {
      background: #345bd6;
      border-color: #5074e9;
    }

    .primary:hover {
      background: #4169e1;
    }

    .danger {
      color: #ffb0b0;
    }

    .status {
      margin-top: 10px;

      padding: 8px;

      border-radius: 7px;

      background: #111620;

      color: #8591a8;

      font-size: 10px;
    }

    .hud {
      display: grid;
      grid-template-columns: repeat(4,1fr);

      gap: 6px;

      margin-bottom: 12px;
    }

    .hudBox {
      padding: 8px 4px;

      background: #111620;

      border: 1px solid #30394d;
      border-radius: 7px;

      text-align: center;
    }

    .hudBox strong {
      display: block;

      color: #fff;
      font-size: 13px;
    }

    .hudBox span {
      font-size: 9px;
      color: #8995ab;
    }

    .hidden {
      display: none !important;
    }

    .mini {
      width: 55px;

      padding: 8px;

      background: #161c28;

      border: 1px solid #3b465d;
      border-radius: 10px;

      color: white;

      text-align: center;

      box-shadow:
        0 10px 30px rgba(0,0,0,.5);
    }

    .mini button {
      width: 100%;

      border: 0;
      background: transparent;

      font-size: 19px;
    }

    .version {
      margin-top: 8px;

      text-align: center;

      font-size: 9px;
      color: #58647a;
    }
  `;

  root.appendChild(style);

  /* =========================
     HTML
     ========================= */

  const wrapper = document.createElement("div");

  wrapper.innerHTML = `
    <div class="app">

      <div class="header">

        <div class="title">
          <strong>⚡ PokePixel Hunt Analyzer</strong>
          <span>Zen Console Edition</span>
        </div>

        <div class="headerButtons">
          <button id="minimize" title="Minimizar">—</button>
          <button id="close" title="Fechar">×</button>
        </div>

      </div>

      <div class="content">

        <div class="hud" id="hudPreview"></div>

        <div class="section">

          <div class="sectionTitle">
            Closed HUD
          </div>

          <label>Preset</label>

          <select id="preset">
            <option value="default">Default</option>
            <option value="leveling">Leveling</option>
            <option value="economy">Economy</option>
            <option value="capture">Capture</option>
            <option value="custom">Custom</option>
          </select>

        </div>

        <div class="section">

          <div class="sectionTitle">
            HUD Slots
          </div>

          <div class="slots" id="slots"></div>

        </div>

        <div class="section">

          <div class="sectionTitle">
            Auto Capture
          </div>

          <div class="toggleRow">
            <span>Enable Auto Capture</span>
            <input
              id="autoEnabled"
              type="checkbox"
            >
          </div>

          <label>Mode</label>

          <select id="captureMode">
            <option value="quality">
              Quality
            </option>

            <option value="all">
              Capture All
            </option>

            <option value="filter">
              Species Filter
            </option>
          </select>

          <div class="row">

            <div>
              <label>Capsule</label>

              <select id="capsule">
                <option value="pokeball">
                  Poké Ball
                </option>

                <option value="greatball">
                  Great Ball
                </option>

                <option value="ultraball">
                  Ultra Ball
                </option>

                <option value="masterball">
                  Master Ball
                </option>
              </select>
            </div>

            <div>
              <label>Minimum Quality</label>

              <input
                id="minQuality"
                type="number"
                min="0"
                max="100"
              >
            </div>

          </div>

          <div class="toggleRow">
            <span>Capture Common</span>

            <input
              id="commonEnabled"
              type="checkbox"
            >
          </div>

          <div class="toggleRow">
            <span>Always Capture Shiny</span>

            <input
              id="shinyEnabled"
              type="checkbox"
            >
          </div>

          <label>Shiny Capsule</label>

          <select id="shinyCapsule">
            <option value="pokeball">
              Poké Ball
            </option>

            <option value="greatball">
              Great Ball
            </option>

            <option value="ultraball">
              Ultra Ball
            </option>

            <option value="masterball">
              Master Ball
            </option>
          </select>

          <label>Species Filter</label>

          <input
            id="speciesFilter"
            type="text"
            placeholder="Ex.: Pikachu, Eevee, Dratini"
          >

        </div>

        <div class="section">

          <div class="sectionTitle">
            Local Tools
          </div>

          <div class="actions">

            <button
              id="save"
              class="primary"
            >
              Save Config
            </button>

            <button
              id="reset"
              class="danger"
            >
              Reset Default
            </button>

          </div>

          <div
            class="status"
            id="status"
          >
            Configuration stored locally in this browser.
          </div>

        </div>

        <div class="version">
          PokePixel Hunt Panel • Console build
        </div>

      </div>

    </div>
  `;

  root.appendChild(wrapper);

  const $ = selector =>
    root.querySelector(selector);

  /* =========================
     SLOT OPTIONS
     ========================= */

  const WIDGETS = {
    encounters: "Encounters",
    captures: "Captures",
    quality: "Quality",
    rarity: "Rarity",
    shiny: "Shiny",
    items: "Items",
    xp: "Experience"
  };

  const ITEMS = {
    total: "Total",
    best: "Best",
    average: "Average",
    pokeball: "Poké Ball",
    greatball: "Great Ball",
    ultraball: "Ultra Ball",
    masterball: "Master Ball",
    common: "Common",
    uncommon: "Uncommon",
    rare: "Rare",
    epic: "Epic",
    legendary: "Legendary"
  };

  const RARITIES = {
    all: "All",
    common: "Common",
    uncommon: "Uncommon",
    rare: "Rare",
    epic: "Epic",
    legendary: "Legendary",
    shiny: "Shiny"
  };

  function options(obj, selected) {
    return Object.entries(obj)
      .map(([value, text]) =>
        `<option value="${value}" ${
          value === selected
            ? "selected"
            : ""
        }>${text}</option>`
      )
      .join("");
  }

  /* =========================
     RENDER DOS SLOTS
     ========================= */

  function renderSlots() {

    const container = $("#slots");

    container.innerHTML = "";

    config.slots.forEach((slot, index) => {

      const element =
        document.createElement("div");

      element.className = "slot";

      element.innerHTML = `

        <div class="slotTitle">
          SLOT ${index + 1}
        </div>

        <div class="row">

          <div>

            <label>Widget</label>

            <select class="widget">
              ${options(
                WIDGETS,
                slot.widget
              )}
            </select>

          </div>

          <div>

            <label>Item / Value</label>

            <select class="item">
              ${options(
                ITEMS,
                slot.item
              )}
            </select>

          </div>

        </div>

        <label>Rarity Tracker</label>

        <select class="rarity">
          ${options(
            RARITIES,
            slot.rarity
          )}
        </select>
      `;

      const widget =
        element.querySelector(".widget");

      const item =
        element.querySelector(".item");

      const rarity =
        element.querySelector(".rarity");

      widget.addEventListener(
        "change",
        () => {

          config.slots[index].widget =
            widget.value;

          config.preset = "custom";

          $("#preset").value =
            "custom";

          saveConfig();
          renderPreview();
        }
      );

      item.addEventListener(
        "change",
        () => {

          config.slots[index].item =
            item.value;

          config.preset = "custom";

          $("#preset").value =
            "custom";

          saveConfig();
          renderPreview();
        }
      );

      rarity.addEventListener(
        "change",
        () => {

          config.slots[index].rarity =
            rarity.value;

          config.preset = "custom";

          $("#preset").value =
            "custom";

          saveConfig();
          renderPreview();
        }
      );

      container.appendChild(element);
    });
  }

  /* =========================
     HUD PREVIEW
     ========================= */

  function renderPreview() {

    const hud = $("#hudPreview");

    hud.innerHTML = "";

    config.slots.forEach(slot => {

      const box =
        document.createElement("div");

      box.className = "hudBox";

      let value = "—";

      /*
       * Esses valores são placeholders.
       * A janela fica pronta para receber
       * dados capturados do jogo.
       */

      if (slot.widget === "encounters")
        value = "0";

      if (slot.widget === "captures")
        value = "0";

      if (slot.widget === "quality")
        value = "0%";

      if (slot.widget === "items")
        value = "0";

      if (slot.widget === "shiny")
        value = "0";

      if (slot.widget === "xp")
        value = "0";

      if (slot.widget === "rarity")
        value = "0";

      box.innerHTML = `
        <strong>${value}</strong>

        <span>
          ${WIDGETS[slot.widget] || slot.widget}
        </span>
      `;

      hud.appendChild(box);
    });
  }

  /* =========================
     FORM -> CONFIG
     ========================= */

  function fillForm() {

    $("#preset").value =
      config.preset || "default";

    $("#autoEnabled").checked =
      !!config.autoCapture.enabled;

    $("#captureMode").value =
      config.autoCapture.mode;

    $("#capsule").value =
      config.autoCapture.capsule;

    $("#minQuality").value =
      config.autoCapture.minQuality;

    $("#commonEnabled").checked =
      !!config.autoCapture.commonEnabled;

    $("#shinyEnabled").checked =
      !!config.autoCapture.shinyEnabled;

    $("#shinyCapsule").value =
      config.autoCapture.shinyCapsule;

    $("#speciesFilter").value =
      config.autoCapture.speciesFilter || "";
  }

  function readForm() {

    config.autoCapture.enabled =
      $("#autoEnabled").checked;

    config.autoCapture.mode =
      $("#captureMode").value;

    config.autoCapture.capsule =
      $("#capsule").value;

    config.autoCapture.minQuality =
      Number($("#minQuality").value) || 0;

    config.autoCapture.commonEnabled =
      $("#commonEnabled").checked;

    config.autoCapture.shinyEnabled =
      $("#shinyEnabled").checked;

    config.autoCapture.shinyCapsule =
      $("#shinyCapsule").value;

    config.autoCapture.speciesFilter =
      $("#speciesFilter").value.trim();
  }

  /* =========================
     PRESETS
     ========================= */

  $("#preset").addEventListener(
    "change",
    event => {

      const preset =
        event.target.value;

      config.preset = preset;

      if (PRESETS[preset]) {
        config.slots =
          clone(PRESETS[preset].slots);
      }

      saveConfig();
      renderSlots();
      renderPreview();
    }
  );

  /* =========================
     SAVE
     ========================= */

  $("#save").addEventListener(
    "click",
    () => {

      readForm();
      saveConfig();

      const status = $("#status");

      status.textContent =
        "✓ Configuration saved successfully.";

      setTimeout(() => {
        status.textContent =
          "Configuration stored locally in this browser.";
      }, 2200);
    }
  );

  /* =========================
     RESET
     ========================= */

  $("#reset").addEventListener(
    "click",
    () => {

      config =
        clone(DEFAULT_CONFIG);

      saveConfig();

      fillForm();
      renderSlots();
      renderPreview();

      $("#status").textContent =
        "✓ Default configuration restored.";
    }
  );

  /* =========================
     AUTO SAVE
     ========================= */

  [
    "#autoEnabled",
    "#captureMode",
    "#capsule",
    "#minQuality",
    "#commonEnabled",
    "#shinyEnabled",
    "#shinyCapsule",
    "#speciesFilter"
  ].forEach(selector => {

    $(selector).addEventListener(
      "change",
      () => {

        readForm();
        saveConfig();
      }
    );
  });

  /* =========================
     MINIMIZAR
     ========================= */

  $("#minimize").addEventListener(
    "click",
    () => {

      $(".content")
        .classList
        .toggle("hidden");
    }
  );

  /* =========================
     FECHAR
     ========================= */

  $("#close").addEventListener(
    "click",
    () => {

      host.remove();

      console.log(
        "[PokePixel Hunt Panel] fechado."
      );
    }
  );

  /* =========================
     ARRASTAR JANELA
     ========================= */

  const header = $(".header");

  let dragging = false;
  let offsetX = 0;
  let offsetY = 0;

  header.addEventListener(
    "mousedown",
    event => {

      if (
        event.target.closest("button")
      ) return;

      dragging = true;

      const rect =
        host.getBoundingClientRect();

      offsetX =
        event.clientX - rect.left;

      offsetY =
        event.clientY - rect.top;

      host.style.right = "auto";
    }
  );

  document.addEventListener(
    "mousemove",
    event => {

      if (!dragging) return;

      host.style.left =
        `${event.clientX - offsetX}px`;

      host.style.top =
        `${event.clientY - offsetY}px`;
    }
  );

  document.addEventListener(
    "mouseup",
    () => {

      dragging = false;
    }
  );

  /* =========================
     API PARA O CONSOLE
     ========================= */

  window.PokePixelHuntPanel = {

    open() {
      host.style.display = "";
    },

    close() {
      host.style.display = "none";
    },

    remove() {
      host.remove();
    },

    config() {
      return clone(config);
    },

    reset() {
      config =
        clone(DEFAULT_CONFIG);

      saveConfig();
      fillForm();
      renderSlots();
      renderPreview();
    },

    setStat(slot, value) {

      const boxes =
        root.querySelectorAll(
          ".hudBox strong"
        );

      if (
        slot >= 1 &&
        slot <= boxes.length
      ) {
        boxes[slot - 1]
          .textContent = value;
      }
    }
  };

  /* =========================
     INICIALIZAÇÃO
     ========================= */

  fillForm();
  renderSlots();
  renderPreview();

  console.log(
    "%c⚡ PokePixel Hunt Analyzer carregado!",
    "color:#7aa2ff;font-size:14px;font-weight:bold"
  );

  console.log(
    "Controle pelo console:",
    {
      verConfig:
        "PokePixelHuntPanel.config()",

      esconder:
        "PokePixelHuntPanel.close()",

      mostrar:
        "PokePixelHuntPanel.open()",

      resetar:
        "PokePixelHuntPanel.reset()",

      exemploHUD:
        "PokePixelHuntPanel.setStat(1, 150)"
    }
  );

})();
