<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>AVICH Costos</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
<style>
:root{--bg:#0f0e17;--surface:#1a1929;--card:#211f35;--border:#2e2c47;--accent:#f7c948;--accent2:#e06c3a;--green:#4ecb71;--red:#e05c5c;--blue:#5b9cf6;--text:#fffffe;--muted:#a7a5c0;--radius:16px;}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;max-width:480px;margin:0 auto;padding-bottom:100px;}
.header{background:linear-gradient(135deg,#1a1929 0%,#211f35 100%);padding:24px 20px 20px;border-bottom:1px solid var(--border);position:sticky;top:0;z-index:100;}
.header-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;}
.logo{font-family:'Syne',sans-serif;font-weight:800;font-size:20px;letter-spacing:-0.5px;}
.logo span{color:var(--accent);}
.project-tag{background:var(--border);padding:5px 12px;border-radius:20px;font-size:12px;color:var(--muted);cursor:pointer;display:flex;align-items:center;gap:6px;}
.project-tag:hover{background:var(--card);}
.totals-strip{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;}
.total-pill{background:var(--card);border-radius:12px;padding:10px 12px;border:1px solid var(--border);text-align:center;}
.total-pill .label{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;margin-bottom:3px;}
.total-pill .value{font-family:'Syne',sans-serif;font-weight:700;font-size:15px;}
.total-pill.main .value{color:var(--accent);font-size:17px;}
.total-pill.cost .value{color:var(--muted);}
.total-pill.profit .value{color:var(--green);}
.tabs{display:flex;gap:8px;padding:16px 20px 0;overflow-x:auto;scrollbar-width:none;}
.tabs::-webkit-scrollbar{display:none;}
.tab{padding:8px 16px;border-radius:20px;font-size:13px;font-weight:500;white-space:nowrap;cursor:pointer;border:1px solid var(--border);color:var(--muted);background:transparent;transition:all .2s;}
.tab.active{background:var(--accent);color:#000;border-color:var(--accent);font-weight:700;}
.tab:hover:not(.active){border-color:var(--accent);color:var(--accent);}
.content{padding:16px 20px;}
.section-card{background:var(--card);border-radius:var(--radius);border:1px solid var(--border);margin-bottom:14px;overflow:hidden;}
.section-header{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;cursor:pointer;user-select:none;}
.section-header:hover{background:rgba(255,255,255,.02);}
.section-left{display:flex;align-items:center;gap:10px;}
.section-icon{width:34px;height:34px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
.section-name{font-family:'Syne',sans-serif;font-weight:700;font-size:14px;}
.section-count{font-size:11px;color:var(--muted);margin-top:1px;}
.section-total{font-family:'Syne',sans-serif;font-weight:700;font-size:16px;color:var(--accent);}
.chevron{color:var(--muted);transition:transform .2s;font-size:12px;}
.section-body{display:none;}.section-body.open{display:block;}
.section-header.open .chevron{transform:rotate(90deg);}
.item-row{display:flex;align-items:center;padding:11px 16px;border-top:1px solid var(--border);gap:10px;}
.item-row:hover{background:rgba(255,255,255,.02);}
.item-dot{width:6px;height:6px;border-radius:50%;background:var(--muted);flex-shrink:0;}
.item-info{flex:1;min-width:0;}
.item-name{font-size:13px;font-weight:500;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;text-transform:capitalize;}
.item-meta{font-size:11px;color:var(--muted);margin-top:2px;}
.item-subtotal{font-family:'Syne',sans-serif;font-weight:600;font-size:14px;color:var(--text);white-space:nowrap;}
.item-actions{display:flex;gap:4px;}
.btn-icon{width:28px;height:28px;border-radius:8px;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:13px;transition:all .15s;background:var(--border);color:var(--muted);}
.btn-icon:hover{background:var(--accent);color:#000;}
.add-item-row{padding:10px 16px;border-top:1px solid var(--border);}
.add-item-btn{display:flex;align-items:center;gap:8px;background:none;border:1px dashed var(--border);border-radius:10px;padding:8px 14px;color:var(--muted);font-size:13px;cursor:pointer;width:100%;transition:all .2s;font-family:'DM Sans',sans-serif;}
.add-item-btn:hover{border-color:var(--accent);color:var(--accent);}
.inline-form{display:none;padding:12px 16px;border-top:1px solid var(--border);background:rgba(247,201,72,.04);gap:8px;flex-direction:column;}
.inline-form.show{display:flex;}
.form-row{display:flex;gap:8px;}
.form-input{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:9px 12px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:13px;outline:none;transition:border-color .2s;}
.form-input:focus{border-color:var(--accent);}
.form-input::placeholder{color:var(--muted);}
.form-input.wide{flex:2;}.form-input.narrow{flex:1;width:0;}
.form-actions{display:flex;gap:8px;justify-content:flex-end;}
.btn-save{background:var(--accent);color:#000;border:none;border-radius:10px;padding:9px 18px;font-weight:700;font-size:13px;cursor:pointer;font-family:'Syne',sans-serif;}
.btn-cancel{background:var(--border);color:var(--muted);border:none;border-radius:10px;padding:9px 14px;font-size:13px;cursor:pointer;font-family:'DM Sans',sans-serif;}
.btn-cancel:hover{background:var(--red);color:#fff;}
.utilidad-card{background:var(--card);border-radius:var(--radius);border:1px solid var(--border);padding:16px;margin-bottom:14px;}
.utilidad-title{font-family:'Syne',sans-serif;font-weight:700;font-size:14px;margin-bottom:12px;display:flex;align-items:center;gap:8px;}
.util-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px solid var(--border);}
.util-row:last-child{border-bottom:none;}
.util-label{font-size:13px;color:var(--muted);}
.util-value{font-family:'Syne',sans-serif;font-weight:600;font-size:14px;}
.util-input-row{display:flex;align-items:center;gap:10px;}
.util-slider{-webkit-appearance:none;appearance:none;width:100px;height:4px;border-radius:2px;background:var(--border);outline:none;}
.util-slider::-webkit-slider-thumb{-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:var(--accent);cursor:pointer;}
.util-pct{font-family:'Syne',sans-serif;font-weight:700;color:var(--accent);font-size:14px;min-width:40px;text-align:right;}
.summary-card{background:linear-gradient(135deg,#211f35,#1a1929);border-radius:var(--radius);border:1px solid var(--border);padding:16px;margin-bottom:14px;}
.summary-title{font-family:'Syne',sans-serif;font-weight:800;font-size:13px;text-transform:uppercase;letter-spacing:1px;color:var(--muted);margin-bottom:12px;}
.summary-row{display:flex;justify-content:space-between;align-items:center;padding:7px 0;}
.summary-row+.summary-row{border-top:1px solid var(--border);}
.summary-cat{display:flex;align-items:center;gap:8px;font-size:13px;}
.summary-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0;}
.summary-amt{font-family:'Syne',sans-serif;font-weight:600;font-size:13px;}
.summary-divider{border:none;border-top:1px solid var(--border);margin:10px 0;}
.quick-calc{background:linear-gradient(135deg,rgba(247,201,72,.1),rgba(247,201,72,.03));border:1px solid rgba(247,201,72,.2);border-radius:var(--radius);padding:16px;margin-bottom:14px;}
.qc-title{font-family:'Syne',sans-serif;font-weight:700;font-size:14px;margin-bottom:10px;color:var(--accent);}
.qc-row{display:flex;justify-content:space-between;padding:6px 0;font-size:13px;}
.qc-row .label{color:var(--muted);}.qc-row .val{font-weight:600;}
.qc-row .val.accent{color:var(--accent);}.qc-row .val.green{color:var(--green);}
.factor-row{display:flex;align-items:center;gap:10px;margin-top:10px;padding-top:10px;border-top:1px solid rgba(247,201,72,.2);}
.factor-label{font-size:12px;color:var(--muted);flex:1;}
.factor-input{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:6px 10px;color:var(--accent);font-family:'Syne',sans-serif;font-weight:700;font-size:14px;width:70px;text-align:center;outline:none;}
.factor-input:focus{border-color:var(--accent);}
.project-name-display{font-family:'Syne',sans-serif;font-weight:700;font-size:22px;margin-bottom:4px;}
.project-date{font-size:12px;color:var(--muted);}

/* COMPROBANTE */
.comp-type-selector{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px;}
.comp-type-btn{border-radius:var(--radius);padding:18px 14px;cursor:pointer;border:2px solid var(--border);background:var(--card);transition:all .2s;text-align:center;}
.comp-type-btn:hover{border-color:var(--accent);}
.comp-type-btn.selected{border-color:var(--accent);background:rgba(247,201,72,.08);}
.comp-type-btn .ct-icon{font-size:28px;margin-bottom:6px;}
.comp-type-btn .ct-name{font-family:'Syne',sans-serif;font-weight:700;font-size:15px;}
.comp-type-btn .ct-desc{font-size:11px;color:var(--muted);margin-top:3px;}
.comp-type-btn.selected .ct-name{color:var(--accent);}
.comp-section{background:var(--card);border-radius:var(--radius);border:1px solid var(--border);padding:16px;margin-bottom:14px;}
.comp-section-title{font-family:'Syne',sans-serif;font-weight:700;font-size:13px;text-transform:uppercase;letter-spacing:.8px;color:var(--muted);margin-bottom:14px;display:flex;align-items:center;gap:8px;}
.field-row{margin-bottom:12px;}
.field-row:last-child{margin-bottom:0;}
.field-label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;margin-bottom:5px;display:block;}
.field-input{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:11px 14px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:14px;width:100%;outline:none;transition:border-color .2s;}
.field-input:focus{border-color:var(--accent);}
.field-input::placeholder{color:var(--muted);}
.field-row-2{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
.comp-items-preview{background:var(--surface);border-radius:12px;border:1px solid var(--border);overflow:hidden;margin-bottom:12px;}
.prev-header{display:grid;grid-template-columns:1fr auto auto;gap:8px;padding:8px 12px;background:var(--border);font-size:11px;color:var(--muted);font-weight:600;text-transform:uppercase;letter-spacing:.5px;}
.prev-row{display:grid;grid-template-columns:1fr auto auto;gap:8px;padding:8px 12px;border-top:1px solid var(--border);font-size:12px;}
.prev-row:nth-child(even){background:rgba(255,255,255,.02);}
.prev-total-row{display:flex;justify-content:space-between;padding:10px 12px;border-top:2px solid var(--border);font-family:'Syne',sans-serif;font-weight:700;}
.doc-number-row{display:flex;align-items:center;gap:10px;margin-bottom:0;}
.doc-number-prefix{background:var(--border);border-radius:10px;padding:11px 14px;color:var(--accent);font-family:'Syne',sans-serif;font-weight:700;font-size:14px;white-space:nowrap;}
.gen-btns{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:6px;}
.gen-btn{border:none;border-radius:var(--radius);padding:16px;cursor:pointer;font-family:'Syne',sans-serif;font-weight:700;font-size:14px;display:flex;flex-direction:column;align-items:center;gap:6px;transition:all .2s;}
.gen-btn .gb-icon{font-size:22px;}
.gen-btn-boleta{background:linear-gradient(135deg,#f7c948,#e0a020);color:#000;}
.gen-btn-boleta:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(247,201,72,.3);}
.gen-btn-factura{background:linear-gradient(135deg,#5b9cf6,#3a6fd4);color:#fff;}
.gen-btn-factura:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(91,156,246,.3);}
.gen-btn-full{grid-column:1/-1;background:linear-gradient(135deg,var(--green),#2da84e);color:#000;border:none;border-radius:var(--radius);padding:16px;cursor:pointer;font-family:'Syne',sans-serif;font-weight:700;font-size:15px;display:flex;align-items:center;justify-content:center;gap:10px;transition:all .2s;}
.gen-btn-full:hover{transform:translateY(-2px);box-shadow:0 8px 20px rgba(78,203,113,.3);}

/* MODAL */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:200;align-items:flex-end;justify-content:center;}
.modal-overlay.show{display:flex;}
.modal{background:var(--card);border-radius:20px 20px 0 0;border:1px solid var(--border);padding:20px;width:100%;max-width:480px;animation:slideUp .25s ease;}
@keyframes slideUp{from{transform:translateY(40px);opacity:0;}to{transform:translateY(0);opacity:1;}}
.modal-title{font-family:'Syne',sans-serif;font-weight:800;font-size:18px;margin-bottom:16px;}
.modal-field{margin-bottom:12px;}
.modal-label{font-size:12px;color:var(--muted);margin-bottom:5px;display:block;text-transform:uppercase;letter-spacing:.5px;}
.modal-input{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:11px 14px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:14px;width:100%;outline:none;}
.modal-input:focus{border-color:var(--accent);}
.modal-row{display:flex;gap:10px;}
.modal-actions{display:flex;gap:10px;margin-top:16px;}
.modal-save{flex:1;background:var(--accent);color:#000;border:none;border-radius:12px;padding:13px;font-family:'Syne',sans-serif;font-weight:700;font-size:15px;cursor:pointer;}
.modal-delete{background:var(--red);color:#fff;border:none;border-radius:12px;padding:13px 16px;cursor:pointer;font-size:15px;}
.modal-cancel{background:var(--border);color:var(--muted);border:none;border-radius:12px;padding:13px 16px;cursor:pointer;font-size:13px;}
.toast{position:fixed;top:20px;left:50%;transform:translateX(-50%) translateY(-60px);background:var(--green);color:#000;font-family:'Syne',sans-serif;font-weight:700;font-size:13px;padding:10px 20px;border-radius:20px;z-index:999;transition:transform .3s;}
.toast.show{transform:translateX(-50%) translateY(0);}
.sc0{background:rgba(247,201,72,.12);}.sc1{background:rgba(78,203,113,.12);}.sc2{background:rgba(224,108,58,.12);}
.sc3{background:rgba(100,149,237,.12);}.sc4{background:rgba(200,100,200,.12);}.sc5{background:rgba(80,200,200,.12);}
.st0{color:var(--accent);}.st1{color:var(--green);}.st2{color:var(--accent2);}
.st3{color:cornflowerblue;}.st4{color:violet;}.st5{color:#50c8c8;}
.screen{display:none;}.screen.active{display:block;}
select.field-input{cursor:pointer;}
.prev-tab-btn{flex:1;padding:8px 6px;border-radius:10px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:11px;font-weight:600;cursor:pointer;font-family:'DM Sans',sans-serif;transition:all .2s;white-space:nowrap;}
.prev-tab-btn.active{background:var(--accent);color:#000;border-color:var(--accent);}
.prev-tab-btn:hover:not(.active){border-color:var(--accent);color:var(--accent);}
.categ-row{display:flex;align-items:center;gap:10px;padding:9px 12px;border-top:1px solid var(--border);}
.categ-bar-wrap{flex:1;background:var(--border);border-radius:4px;height:6px;overflow:hidden;}
.categ-bar{height:6px;border-radius:4px;}
.qr-box{background:linear-gradient(135deg,rgba(247,201,72,.1),rgba(247,201,72,.03));border:1px solid rgba(247,201,72,.2);border-radius:12px;overflow:hidden;}
.qr-row{display:flex;justify-content:space-between;align-items:center;padding:10px 14px;border-bottom:1px solid rgba(247,201,72,.1);font-size:13px;}
.qr-row:last-child{border-bottom:none;}
.qr-row .ql{color:var(--muted);}
.qr-row .qv{font-family:'Syne',sans-serif;font-weight:700;}
.qr-total{background:var(--accent);display:flex;justify-content:space-between;align-items:center;padding:12px 14px;}
.qr-total span{font-family:'Syne',sans-serif;font-weight:800;font-size:15px;color:#000;}
</style>
</head>
<body>

<div class="header">
  <div class="header-top">
    <div class="logo">AVICH <span>Costos</span></div>
    <div class="project-tag" onclick="promptProjectName()">
      <span id="headerProjectName">Mi Proyecto</span><span>✏️</span>
    </div>
  </div>
  <div class="totals-strip">
    <div class="total-pill cost"><div class="label">Costo</div><div class="value" id="stripCosto">S/ 0</div></div>
    <div class="total-pill main"><div class="label">Venta</div><div class="value" id="stripVenta">S/ 0</div></div>
    <div class="total-pill profit"><div class="label">Ganancia</div><div class="value" id="stripGanancia">S/ 0</div></div>
  </div>
</div>

<div class="tabs">
  <div class="tab active" onclick="switchTab('costos')">📋 Costos</div>
  <div class="tab" onclick="switchTab('resumen')">📊 Resumen</div>
  <div class="tab" onclick="switchTab('comprobante')">🧾 Comprobante</div>
</div>

<!-- COSTOS -->
<div class="screen active" id="screenCostos">
  <div class="content" id="sectionsContainer"></div>
</div>

<!-- RESUMEN -->
<div class="screen" id="screenResumen">
  <div class="content">
    <div style="padding:6px 0 16px;">
      <div class="project-name-display" id="resumeProjectName">Mi Proyecto</div>
      <div class="project-date" id="resumeDate"></div>
    </div>
    <div class="utilidad-card">
      <div class="utilidad-title">💰 Utilidad</div>
      <div class="util-row">
        <span class="util-label">Porcentaje de utilidad</span>
        <div class="util-input-row">
          <input type="range" class="util-slider" id="utilSlider" min="0" max="60" value="20" oninput="updateUtilidad(this.value)">
          <span class="util-pct" id="utilPct">20%</span>
        </div>
      </div>
      <div class="util-row"><span class="util-label">Costo base</span><span class="util-value" id="utilBase">S/ 0.00</span></div>
      <div class="util-row"><span class="util-label">Monto utilidad</span><span class="util-value" id="utilMonto" style="color:var(--green)">S/ 0.00</span></div>
      <div class="util-row"><span class="util-label">Precio de venta</span><span class="util-value" style="color:var(--accent);font-size:18px" id="utilVenta">S/ 0.00</span></div>
    </div>
    <div class="quick-calc">
      <div class="qc-title">⚡ Cálculo Rápido</div>
      <div class="qc-row"><span class="label">Gastos de compras</span><span class="val" id="qcCompras">S/ 0.00</span></div>
      <div class="qc-row"><span class="label">Venta mueble</span><span class="val accent" id="qcVenta">S/ 0.00</span></div>
      <div class="qc-row"><span class="label">Ganancia por mueble</span><span class="val green" id="qcGanancia">S/ 0.00</span></div>
      <div class="factor-row">
        <span class="factor-label">Factor cálculo rápido</span>
        <input type="number" class="factor-input" id="factorInput" value="1.8" step="0.1" oninput="onFactorChange()">
      </div>
    </div>
    <div class="summary-card" id="summaryCard"></div>
  </div>
</div>

<!-- COMPROBANTE -->
<div class="screen" id="screenComprobante">
  <div class="content">

    <div class="comp-type-selector">
      <div class="comp-type-btn selected" id="btnBoleta" onclick="selectDocType('boleta')">
        <div class="ct-icon">🧾</div>
        <div class="ct-name">Boleta</div>
        <div class="ct-desc">Consumidor final</div>
      </div>
      <div class="comp-type-btn" id="btnFactura" onclick="selectDocType('factura')">
        <div class="ct-icon">📄</div>
        <div class="ct-name">Factura</div>
        <div class="ct-desc">Empresa con RUC</div>
      </div>
    </div>

    <!-- EMISOR -->
    <div class="comp-section">
      <div class="comp-section-title">🏢 Datos del Emisor (Tú)</div>
      <div class="field-row">
        <label class="field-label">Nombre / Razón Social</label>
        <input class="field-input" id="emisorNombre" placeholder="Tu nombre o empresa" oninput="saveInvoiceState()">
      </div>
      <div class="field-row-2">
        <div>
          <label class="field-label">RUC / DNI</label>
          <input class="field-input" id="emisorRuc" placeholder="20123456789" oninput="saveInvoiceState()">
        </div>
        <div>
          <label class="field-label">Teléfono</label>
          <input class="field-input" id="emisorTelefono" placeholder="999 888 777" oninput="saveInvoiceState()">
        </div>
      </div>
      <div class="field-row">
        <label class="field-label">Dirección</label>
        <input class="field-input" id="emisorDireccion" placeholder="Av. Principal 123, Lima" oninput="saveInvoiceState()">
      </div>
      <div class="field-row">
        <label class="field-label">Correo electrónico</label>
        <input class="field-input" id="emisorEmail" placeholder="micarpinteria@gmail.com" type="email" oninput="saveInvoiceState()">
      </div>
    </div>

    <!-- CLIENTE -->
    <div class="comp-section">
      <div class="comp-section-title">👤 Datos del Cliente</div>
      <div class="field-row">
        <label class="field-label">Nombre completo</label>
        <input class="field-input" id="clienteNombre" placeholder="Nombre del cliente" oninput="saveInvoiceState()">
      </div>
      <div class="field-row-2">
        <div>
          <label class="field-label" id="clienteDocLabel">DNI</label>
          <input class="field-input" id="clienteDoc" placeholder="12345678" oninput="saveInvoiceState()">
        </div>
        <div>
          <label class="field-label">Teléfono</label>
          <input class="field-input" id="clienteTelefono" placeholder="999 000 111" oninput="saveInvoiceState()">
        </div>
      </div>
      <div class="field-row">
        <label class="field-label">Dirección</label>
        <input class="field-input" id="clienteDireccion" placeholder="Dirección del cliente" oninput="saveInvoiceState()">
      </div>
      <div class="field-row" id="clienteRazonRow" style="display:none">
        <label class="field-label">Razón Social</label>
        <input class="field-input" id="clienteRazon" placeholder="Empresa S.A.C." oninput="saveInvoiceState()">
      </div>
      <div class="field-row">
        <label class="field-label">Correo electrónico</label>
        <input class="field-input" id="clienteEmail" placeholder="cliente@email.com" type="email" oninput="saveInvoiceState()">
      </div>
    </div>

    <!-- DOC -->
    <div class="comp-section">
      <div class="comp-section-title">📋 Datos del Comprobante</div>
      <div class="field-row">
        <label class="field-label">N° de Comprobante</label>
        <div class="doc-number-row">
          <span class="doc-number-prefix" id="docPrefix">B001-</span>
          <input class="field-input" id="docNumero" placeholder="00001" style="flex:1" oninput="saveInvoiceState()">
        </div>
      </div>
      <div class="field-row-2">
        <div>
          <label class="field-label">Fecha emisión</label>
          <input class="field-input" type="date" id="docFecha" oninput="saveInvoiceState()">
        </div>
        <div>
          <label class="field-label">Fecha venc.</label>
          <input class="field-input" type="date" id="docVencimiento" oninput="saveInvoiceState()">
        </div>
      </div>
      <div class="field-row">
        <label class="field-label">Descripción del servicio</label>
        <input class="field-input" id="docDescripcion" placeholder="Fabricación e instalación de mueble" oninput="saveInvoiceState()">
      </div>
      <div class="field-row">
        <label class="field-label">Método de pago</label>
        <select class="field-input" id="docPago" onchange="saveInvoiceState()">
          <option value="Contado">Contado</option>
          <option value="Transferencia bancaria">Transferencia bancaria</option>
          <option value="Yape / Plin">Yape / Plin</option>
          <option value="Deposito en cuenta">Deposito en cuenta</option>
          <option value="Credito 30 dias">Credito 30 dias</option>
        </select>
      </div>
      <div class="field-row">
        <label class="field-label">Notas adicionales</label>
        <input class="field-input" id="docNotas" placeholder="Garantia 6 meses, incluye instalacion..." oninput="saveInvoiceState()">
      </div>
    </div>

    <!-- ITEMS PREVIEW -->
    <div class="comp-section">
      <div class="comp-section-title">📦 Items a facturar</div>
      <div style="display:flex;gap:8px;margin-bottom:14px;">
        <button class="prev-tab-btn active" id="ptb0" onclick="switchPreviewTab(0)">📋 Detalle</button>
        <button class="prev-tab-btn" id="ptb1" onclick="switchPreviewTab(1)">📂 Categorías</button>
        <button class="prev-tab-btn" id="ptb2" onclick="switchPreviewTab(2)">⚡ Cálc. Rápido</button>
      </div>
      <div id="itemsPreviewContainer"></div>
    </div>

    <!-- BOTONES -->
    <div class="gen-btns">
      <button class="gen-btn gen-btn-boleta" onclick="generarPDF('boleta')">
        <span class="gb-icon">🧾</span>Boleta PDF
      </button>
      <button class="gen-btn gen-btn-factura" onclick="generarPDF('factura')">
        <span class="gb-icon">📄</span>Factura PDF
      </button>
      <button class="gen-btn-full" onclick="generarPDF(state.docType||'boleta')">
        ⬇️ Descargar PDF del comprobante seleccionado
      </button>
    </div>

  </div>
</div>

<!-- MODAL EDIT -->
<div class="modal-overlay" id="editModal">
  <div class="modal">
    <div class="modal-title">Editar item</div>
    <div class="modal-field">
      <label class="modal-label">Descripcion</label>
      <input class="modal-input" id="editDesc" type="text" placeholder="Ej: Melamina roble">
    </div>
    <div class="modal-row">
      <div class="modal-field" style="flex:1">
        <label class="modal-label">Medida / Unidad</label>
        <input class="modal-input" id="editUnit" type="text" placeholder="unidad">
      </div>
      <div class="modal-field" style="flex:1">
        <label class="modal-label">Cantidad</label>
        <input class="modal-input" id="editQty" type="number" placeholder="1">
      </div>
    </div>
    <div class="modal-field">
      <label class="modal-label">Precio Unitario (S/)</label>
      <input class="modal-input" id="editPrice" type="number" placeholder="0.00">
    </div>
    <div class="modal-actions">
      <button class="modal-cancel" onclick="closeModal()">x</button>
      <button class="modal-delete" onclick="deleteItemModal()">Eliminar</button>
      <button class="modal-save" onclick="saveEditModal()">Guardar</button>
    </div>
  </div>
</div>

<div class="toast" id="toast">Guardado</div>

<script>
// STATE
let state = {
  projectName:'Mi Proyecto', utilidadPct:20, factor:1.8, docType:'boleta',
  sections:[
    {id:'s1',name:'MATERIALES',icon:'🪵',items:[{id:'i1',desc:'Melamina pelikano (manzano) 2440x2150',unit:'unidad',qty:1,price:270}]},
    {id:'s2',name:'ACCESORIOS',icon:'🔩',items:[{id:'i2',desc:'Tapatornillos',unit:'centenario',qty:0.3,price:7}]},
    {id:'s3',name:'TRANSPORTE',icon:'🚚',items:[{id:'i3',desc:'Movilizacion de material al taller',unit:'unidad',qty:1,price:20},{id:'i4',desc:'Movilizacion de taller al cliente',unit:'unidad',qty:1,price:20}]},
    {id:'s4',name:'GASTOS DE ACABADO',icon:'🎨',items:[{id:'i5',desc:'Thiner',unit:'GL',qty:0.3,price:23},{id:'i6',desc:'Waype',unit:'KG',qty:0.3,price:8},{id:'i7',desc:'Silicona brillo y proteccion',unit:'unidad',qty:0.3,price:18}]},
    {id:'s5',name:'MANO DE OBRA',icon:'👷',items:[{id:'i8',desc:'Maestro',unit:'unidad',qty:1,price:120},{id:'i9',desc:'Armador',unit:'unidad',qty:1,price:70},{id:'i10',desc:'Ayudante',unit:'unidad',qty:1,price:50}]},
    {id:'s6',name:'OTROS',icon:'📦',items:[{id:'i11',desc:'Desgaste de equipos',unit:'unidad',qty:0.02,price:426.8},{id:'i12',desc:'Diseno',unit:'global',qty:1,price:80}]}
  ]
};
let inv = {emisorNombre:'',emisorRuc:'',emisorTelefono:'',emisorDireccion:'',emisorEmail:'',clienteNombre:'',clienteDoc:'',clienteTelefono:'',clienteDireccion:'',clienteRazon:'',clienteEmail:'',docNumero:'00001',docFecha:'',docVencimiento:'',docDescripcion:'Fabricacion e instalacion de mueble',docPago:'Contado',docNotas:''};

let editTarget=null, openSections=new Set(['s1','s2','s3','s4','s5','s6']), idC=100;
function gid(){return 'x'+(idC++);}

function sTot(s){return s.items.reduce((a,i)=>a+(i.qty*i.price),0);}
function totCosto(){return state.sections.reduce((a,s)=>a+sTot(s),0);}
function calc(){
  const c=totCosto(), u=c*(state.utilidadPct/100), v=c+u, f=parseFloat(state.factor)||1.8;
  return{costo:c,util:u,venta:v,ganancia:v-c,qv:c*f,qg:c*f-c};
}
function fmt(n){return 'S/ '+n.toFixed(2);}
function fmtS(n){return n>=1000?'S/ '+(n/1000).toFixed(1)+'k':'S/ '+n.toFixed(0);}
function cap(s){return s?s.charAt(0).toUpperCase()+s.slice(1):s;}

function renderAll(){renderSections();updateAll();renderPreview();}

function renderSections(){
  const ct=document.getElementById('sectionsContainer'); ct.innerHTML='';
  state.sections.forEach((sec,si)=>{
    const t=sTot(sec), op=openSections.has(sec.id), ci=si%6;
    const card=document.createElement('div'); card.className='section-card';
    const hdr=document.createElement('div'); hdr.className='section-header'+(op?' open':'');
    hdr.innerHTML=`<div class="section-left"><div class="section-icon sc${ci}">${sec.icon}</div><div><div class="section-name">${sec.name}</div><div class="section-count">${sec.items.length} item${sec.items.length!==1?'s':''}</div></div></div><div style="display:flex;align-items:center;gap:10px"><div class="section-total st${ci}">${fmt(t)}</div><span class="chevron">&#9658;</span></div>`;
    hdr.onclick=()=>togSec(sec.id,card,hdr);
    const body=document.createElement('div'); body.className='section-body'+(op?' open':'');
    sec.items.forEach((item,ii)=>{
      const sub=item.qty*item.price, r=document.createElement('div'); r.className='item-row';
      r.innerHTML=`<div class="item-dot"></div><div class="item-info"><div class="item-name">${item.desc}</div><div class="item-meta">${item.qty} ${item.unit} x S/ ${item.price}</div></div><div class="item-subtotal">${fmt(sub)}</div><div class="item-actions"><button class="btn-icon" onclick="openEdit(${si},${ii})">edit</button></div>`;
      body.appendChild(r);
    });
    const fid='f_'+sec.id;
    const ar=document.createElement('div'); ar.className='add-item-row';
    ar.innerHTML=`<button class="add-item-btn" onclick="togForm('${fid}')">+ Agregar item</button>`;
    const fm=document.createElement('div'); fm.className='inline-form'; fm.id=fid;
    fm.innerHTML=`<input class="form-input wide" placeholder="Descripcion" id="${fid}_d"><div class="form-row"><input class="form-input narrow" placeholder="Unidad" id="${fid}_u"><input class="form-input narrow" type="number" placeholder="Cant." id="${fid}_q"><input class="form-input narrow" type="number" placeholder="Precio" id="${fid}_p"></div><div class="form-actions"><button class="btn-cancel" onclick="togForm('${fid}')">Cancelar</button><button class="btn-save" onclick="addIt(${si},'${fid}')">Agregar</button></div>`;
    body.appendChild(ar); body.appendChild(fm); card.appendChild(hdr); card.appendChild(body); ct.appendChild(card);
  });
  const as=document.createElement('div');
  as.innerHTML=`<button onclick="addSec()" style="display:flex;align-items:center;justify-content:center;gap:8px;background:none;border:1px dashed var(--border);border-radius:var(--radius);padding:14px;color:var(--muted);font-size:13px;cursor:pointer;width:100%;font-family:'DM Sans',sans-serif;margin-bottom:8px;transition:all .2s" onmouseover="this.style.borderColor='var(--accent)';this.style.color='var(--accent)'" onmouseout="this.style.borderColor='var(--border)';this.style.color='var(--muted)'">+ Nueva categoria</button>`;
  ct.appendChild(as);
}
function togSec(id,card,hdr){
  if(openSections.has(id)){openSections.delete(id);card.querySelector('.section-body').classList.remove('open');hdr.classList.remove('open');}
  else{openSections.add(id);card.querySelector('.section-body').classList.add('open');hdr.classList.add('open');}
}
function togForm(fid){const f=document.getElementById(fid);f.classList.toggle('show');if(f.classList.contains('show'))document.getElementById(fid+'_d').focus();}
function addIt(si,fid){
  const d=document.getElementById(fid+'_d').value.trim(), u=document.getElementById(fid+'_u').value.trim()||'unidad', q=parseFloat(document.getElementById(fid+'_q').value)||1, p=parseFloat(document.getElementById(fid+'_p').value)||0;
  if(!d){alert('Ingresa una descripcion');return;}
  state.sections[si].items.push({id:gid(),desc:d,unit:u,qty:q,price:p});
  save(); renderAll(); toast('Item agregado'); openSections.add(state.sections[si].id);
}
function addSec(){
  const n=prompt('Nombre de la nueva categoria:'); if(!n)return;
  const icons=['📁','⭐','🔧','💡','🛠️','📌'];
  const ns={id:'s'+gid(),name:n.toUpperCase(),icon:icons[state.sections.length%icons.length],items:[]};
  state.sections.push(ns); openSections.add(ns.id); save(); renderAll(); toast('Categoria creada');
}
function openEdit(si,ii){
  editTarget={si,ii}; const it=state.sections[si].items[ii];
  document.getElementById('editDesc').value=it.desc; document.getElementById('editUnit').value=it.unit;
  document.getElementById('editQty').value=it.qty; document.getElementById('editPrice').value=it.price;
  document.getElementById('editModal').classList.add('show');
}
function closeModal(){document.getElementById('editModal').classList.remove('show');editTarget=null;}
function saveEditModal(){
  if(!editTarget)return; const{si,ii}=editTarget;
  state.sections[si].items[ii]={...state.sections[si].items[ii],desc:document.getElementById('editDesc').value.trim(),unit:document.getElementById('editUnit').value.trim()||'unidad',qty:parseFloat(document.getElementById('editQty').value)||0,price:parseFloat(document.getElementById('editPrice').value)||0};
  closeModal(); save(); renderAll(); toast('Item actualizado');
}
function deleteItemModal(){
  if(!editTarget||!confirm('Eliminar este item?'))return; const{si,ii}=editTarget;
  state.sections[si].items.splice(ii,1); closeModal(); save(); renderAll(); toast('Item eliminado');
}
function updateAll(){
  const c=calc();
  document.getElementById('stripCosto').textContent=fmtS(c.costo);
  document.getElementById('stripVenta').textContent=fmtS(c.venta);
  document.getElementById('stripGanancia').textContent=fmtS(c.ganancia);
  document.getElementById('utilBase').textContent=fmt(c.costo);
  document.getElementById('utilMonto').textContent=fmt(c.util);
  document.getElementById('utilVenta').textContent=fmt(c.venta);
  document.getElementById('qcCompras').textContent=fmt(c.costo);
  document.getElementById('qcVenta').textContent=fmt(c.qv);
  document.getElementById('qcGanancia').textContent=fmt(c.qg);
  renderSum(c);
}
function updateUtilidad(v){state.utilidadPct=parseFloat(v);document.getElementById('utilPct').textContent=v+'%';save();updateAll();}
function onFactorChange(){state.factor=parseFloat(document.getElementById('factorInput').value)||1.8;save();updateAll();}
function renderSum(c){
  const card=document.getElementById('summaryCard');
  const clrs=['#f7c948','#4ecb71','#e06c3a','#6495ed','#c864c8','#50c8c8'];
  let h='<div class="summary-title">Desglose por categoria</div>';
  state.sections.forEach((s,i)=>{const t=sTot(s);h+=`<div class="summary-row"><div class="summary-cat"><div class="summary-dot" style="background:${clrs[i%clrs.length]}"></div>${s.icon} ${s.name}</div><div class="summary-amt">${fmt(t)}</div></div>`;});
  h+=`<hr class="summary-divider"><div style="display:flex;justify-content:space-between;align-items:center;padding:8px 0 0"><div style="font-family:'Syne',sans-serif;font-weight:800;font-size:15px">COSTO TOTAL</div><div style="font-family:'Syne',sans-serif;font-weight:800;font-size:22px;color:var(--accent)">${fmt(c.costo)}</div></div><div style="display:flex;justify-content:space-between;padding:4px 0;font-size:13px;color:var(--muted)"><span>+ Utilidad ${state.utilidadPct}%</span><span style="color:var(--green)">${fmt(c.util)}</span></div><div style="display:flex;justify-content:space-between;padding:8px 0 0;border-top:1px solid var(--border);margin-top:6px"><span style="font-family:'Syne',sans-serif;font-weight:800;font-size:16px">PRECIO DE VENTA</span><span style="font-family:'Syne',sans-serif;font-weight:800;font-size:20px;color:var(--accent)">${fmt(c.venta)}</span></div>`;
  card.innerHTML=h;
}

// COMPROBANTE
function selectDocType(t){
  state.docType=t;
  document.getElementById('btnBoleta').classList.toggle('selected',t==='boleta');
  document.getElementById('btnFactura').classList.toggle('selected',t==='factura');
  document.getElementById('docPrefix').textContent=t==='boleta'?'B001-':'F001-';
  document.getElementById('clienteDocLabel').textContent=t==='factura'?'RUC':'DNI';
  document.getElementById('clienteRazonRow').style.display=t==='factura'?'block':'none';
  save();
}
let previewTab=0;
function switchPreviewTab(n){
  previewTab=n;
  [0,1,2].forEach(i=>{const b=document.getElementById('ptb'+i);if(b)b.classList.toggle('active',i===n);});
  renderPreview();
}
function renderPreview(){
  const ct=document.getElementById('itemsPreviewContainer'); if(!ct)return;
  const c=calc();
  if(previewTab===0) ct.innerHTML=renderPreviewDetalle(c);
  else if(previewTab===1) ct.innerHTML=renderPreviewCategorias(c);
  else ct.innerHTML=renderPreviewRapido(c);
}
function renderPreviewDetalle(c){
  let h=`<div class="comp-items-preview"><div class="prev-header"><span>Descripcion</span><span>Cant.</span><span>Subtotal</span></div>`;
  let has=false;
  state.sections.forEach(s=>s.items.forEach(it=>{
    if(it.qty>0&&it.price>0){has=true;const sub=it.qty*it.price;h+=`<div class="prev-row"><span style="text-transform:capitalize">${it.desc}</span><span>${it.qty} ${it.unit}</span><span>${fmt(sub)}</span></div>`;}
  }));
  if(!has)h+=`<div style="padding:16px;text-align:center;color:var(--muted);font-size:13px">Agrega items en la pestana Costos</div>`;
  h+=`<div class="prev-total-row"><span>Subtotal</span><span>${fmt(c.costo)}</span></div><div class="prev-total-row" style="color:var(--green)"><span>Utilidad (${state.utilidadPct}%)</span><span>${fmt(c.util)}</span></div><div class="prev-total-row" style="color:var(--accent);font-size:16px"><span>TOTAL</span><span>${fmt(c.venta)}</span></div></div>`;
  return h;
}
function renderPreviewCategorias(c){
  const clrs=['#f7c948','#4ecb71','#e06c3a','#6495ed','#c864c8','#50c8c8'];
  let h=`<div class="comp-items-preview"><div class="prev-header" style="grid-template-columns:auto 1fr auto auto"><span>Cat.</span><span>Categoría</span><span>%</span><span>Total</span></div>`;
  state.sections.forEach((s,i)=>{
    const t=sTot(s); if(t<=0)return;
    const pct=c.costo>0?((t/c.costo)*100).toFixed(1):0;
    const clr=clrs[i%clrs.length];
    h+=`<div class="categ-row"><div style="width:8px;height:8px;border-radius:2px;background:${clr};flex-shrink:0"></div><div style="flex:1;min-width:0"><div style="font-size:12px;font-weight:600">${s.icon} ${s.name}</div><div class="categ-bar-wrap" style="margin-top:4px"><div class="categ-bar" style="width:${pct}%;background:${clr}"></div></div></div><div style="font-size:11px;color:var(--muted);min-width:34px;text-align:right">${pct}%</div><div style="font-size:13px;font-weight:700;min-width:70px;text-align:right">${fmt(t)}</div></div>`;
  });
  h+=`<div class="prev-total-row"><span>Costo base</span><span>${fmt(c.costo)}</span></div><div class="prev-total-row" style="color:var(--green)"><span>Utilidad (${state.utilidadPct}%)</span><span>${fmt(c.util)}</span></div><div class="prev-total-row" style="color:var(--accent);font-size:16px"><span>PRECIO VENTA</span><span>${fmt(c.venta)}</span></div></div>`;
  return h;
}
function renderPreviewRapido(c){
  const f=parseFloat(state.factor)||1.8;
  return `<div class="qr-box">
    <div class="qr-row"><span class="ql">Costo base (gastos compras)</span><span class="qv">${fmt(c.costo)}</span></div>
    <div class="qr-row"><span class="ql">Factor aplicado</span><span class="qv" style="color:var(--accent)">× ${f}</span></div>
    <div class="qr-row"><span class="ql">Utilidad generada</span><span class="qv" style="color:var(--green)">${fmt(c.qg)}</span></div>
    <div class="qr-row"><span class="ql">Margen de ganancia</span><span class="qv" style="color:var(--green)">${c.costo>0?((c.qg/c.costo)*100).toFixed(1):0}%</span></div>
    <div class="qr-total"><span>PRECIO RÁPIDO</span><span>${fmt(c.qv)}</span></div>
  </div>
  <div style="margin-top:10px;padding:10px 14px;background:var(--surface);border-radius:12px;border:1px solid var(--border)">
    <div style="font-size:11px;color:var(--muted);margin-bottom:6px;text-transform:uppercase;letter-spacing:.5px">Comparación de métodos</div>
    <div style="display:flex;justify-content:space-between;font-size:13px;padding:5px 0;border-bottom:1px solid var(--border)"><span style="color:var(--muted)">Con utilidad (${state.utilidadPct}%)</span><span style="font-weight:700">${fmt(c.venta)}</span></div>
    <div style="display:flex;justify-content:space-between;font-size:13px;padding:5px 0"><span style="color:var(--muted)">Cálculo rápido (×${f})</span><span style="font-weight:700;color:var(--accent)">${fmt(c.qv)}</span></div>
  </div>`;
}
function saveInvoiceState(){
  ['emisorNombre','emisorRuc','emisorTelefono','emisorDireccion','emisorEmail','clienteNombre','clienteDoc','clienteTelefono','clienteDireccion','clienteRazon','clienteEmail','docNumero','docFecha','docVencimiento','docDescripcion','docPago','docNotas'].forEach(f=>{const el=document.getElementById(f);if(el)inv[f]=el.value;});
  save();
}
function loadInvFields(){
  ['emisorNombre','emisorRuc','emisorTelefono','emisorDireccion','emisorEmail','clienteNombre','clienteDoc','clienteTelefono','clienteDireccion','clienteRazon','clienteEmail','docNumero','docDescripcion','docNotas'].forEach(f=>{const el=document.getElementById(f);if(el)el.value=inv[f]||'';});
  if(inv.docFecha)document.getElementById('docFecha').value=inv.docFecha;
  if(inv.docVencimiento)document.getElementById('docVencimiento').value=inv.docVencimiento;
  const s=document.getElementById('docPago');if(s&&inv.docPago){for(let o of s.options){if(o.value===inv.docPago){o.selected=true;break;}}}
}

// PDF
function generarPDF(tipo){
  saveInvoiceState();
  const{jsPDF}=window.jspdf;
  const doc=new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
  const W=210,pL=18,pR=192;
  const c=calc();
  const esBoleta=tipo==='boleta';
  const tipeTxt=esBoleta?'BOLETA DE VENTA':'FACTURA';
  const pref=esBoleta?'B001-':'F001-';
  const numDoc=pref+(inv.docNumero||'00001');
  const hoy=new Date().toLocaleDateString('es-PE',{day:'2-digit',month:'2-digit',year:'numeric'});
  const fEm=inv.docFecha?fmtD(inv.docFecha):hoy;
  const fVe=inv.docVencimiento?fmtD(inv.docVencimiento):'-';

  const CD=[15,14,23], CM=[33,31,53], CA=[247,201,72], CB=[91,156,246], CG=[78,203,113], CW=[255,255,255], CGR=[167,165,192];

  // Fondo oscuro header
  doc.setFillColor(...CD); doc.rect(0,0,W,62,'F');
  doc.setFillColor(...CM); doc.rect(0,62,W,235,'F');
  // Barra accent izq
  doc.setFillColor(...CA); doc.rect(0,0,5,297,'F');

  // Nombre emisor
  doc.setTextColor(...CW); doc.setFont('helvetica','bold'); doc.setFontSize(20);
  doc.text(inv.emisorNombre||'Mi Empresa',pL+2,26);
  doc.setFont('helvetica','normal'); doc.setFontSize(9); doc.setTextColor(...CGR);
  let ey=33;
  if(inv.emisorRuc){doc.text('RUC: '+inv.emisorRuc,pL+2,ey);ey+=6;}
  if(inv.emisorDireccion){doc.text(inv.emisorDireccion,pL+2,ey);ey+=6;}
  if(inv.emisorTelefono){doc.text('Tel: '+inv.emisorTelefono,pL+2,ey);ey+=6;}
  if(inv.emisorEmail){doc.text(inv.emisorEmail,pL+2,ey);}

  // Badge tipo
  const bc=esBoleta?CA:CB;
  doc.setFillColor(...bc); doc.roundedRect(136,16,56,20,3,3,'F');
  doc.setTextColor(esBoleta?0:255,esBoleta?0:255,esBoleta?0:255); doc.setFont('helvetica','bold'); doc.setFontSize(12);
  doc.text(tipeTxt,164,29,{align:'center'});
  doc.setFont('helvetica','normal'); doc.setFontSize(9); doc.setTextColor(...CGR);
  doc.text(numDoc,pR,40,{align:'right'});
  doc.text('Emitido: '+fEm,pR,47,{align:'right'});

  // Linea divisoria
  doc.setDrawColor(...CA); doc.setLineWidth(0.4); doc.line(pL,55,pR,55);

  // CLIENTE BOX
  doc.setFillColor(20,18,38); doc.roundedRect(pL,60,82,40,4,4,'F');
  doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8);
  doc.text('DATOS DEL CLIENTE',pL+4,68);
  doc.setFont('helvetica','bold'); doc.setFontSize(11); doc.setTextColor(...CW);
  doc.text(inv.clienteNombre||'Cliente',pL+4,76);
  doc.setFont('helvetica','normal'); doc.setFontSize(8.5); doc.setTextColor(...CGR);
  let cy=83;
  const dlbl=esBoleta?'DNI':'RUC';
  if(inv.clienteDoc){doc.text(dlbl+': '+inv.clienteDoc,pL+4,cy);cy+=6;}
  if(inv.clienteRazon&&!esBoleta){doc.text(inv.clienteRazon,pL+4,cy);cy+=6;}
  if(inv.clienteTelefono){const tl=doc.splitTextToSize('Tel: '+inv.clienteTelefono,74);doc.text(tl[0],pL+4,cy);cy+=6;}
  if(inv.clienteDireccion){const dl=doc.splitTextToSize(inv.clienteDireccion,74);doc.text(dl[0],pL+4,cy);}

  // DOC INFO BOX
  doc.setFillColor(20,18,38); doc.roundedRect(108,60,84,40,4,4,'F');
  doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8);
  doc.text('DETALLES',112,68);
  const rows=[['N Comprobante',numDoc],['Fecha emision',fEm],['Fecha vencimiento',fVe],['Metodo de pago',inv.docPago||'Contado'],['Proyecto',state.projectName||'-']];
  let dy=76;
  rows.forEach(([l,v])=>{
    doc.setFont('helvetica','normal'); doc.setFontSize(8); doc.setTextColor(...CGR); doc.text(l+':',112,dy);
    doc.setFont('helvetica','bold'); doc.setFontSize(8); doc.setTextColor(...CW);
    const vl=doc.splitTextToSize(v,55);
    doc.text(vl[0],pR,dy,{align:'right'}); dy+=6;
  });

  // DESCRIPCION
  let y=108;
  if(inv.docDescripcion){
    doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8.5);
    doc.text('DESCRIPCION DEL SERVICIO',pL,y); y+=5;
    doc.setFont('helvetica','italic'); doc.setFontSize(9); doc.setTextColor(...CGR);
    const dl=doc.splitTextToSize(inv.docDescripcion,170);
    doc.text(dl[0],pL,y); y+=7;
  }

  // TABLA
  const rows2=[];
  state.sections.forEach(s=>s.items.forEach(it=>{
    if(it.qty>0&&it.price>0) rows2.push([cap(it.desc),it.unit,it.qty.toString(),'S/ '+it.price.toFixed(2),'S/ '+(it.qty*it.price).toFixed(2)]);
  }));

  doc.autoTable({
    startY:y,
    head:[['Descripcion','Unidad','Cant.','P. Unit.','Subtotal']],
    body:rows2,
    theme:'plain',
    styles:{font:'helvetica',fontSize:8.5,textColor:CW,cellPadding:{top:4,right:4,bottom:4,left:4}},
    headStyles:{fillColor:CA,textColor:[0,0,0],fontStyle:'bold',fontSize:8.5},
    alternateRowStyles:{fillColor:[22,20,38]},
    columnStyles:{0:{cellWidth:72},1:{cellWidth:22,halign:'center'},2:{cellWidth:17,halign:'center'},3:{cellWidth:30,halign:'right'},4:{cellWidth:30,halign:'right'}},
    margin:{left:pL,right:W-pR},
  });

  // TOTALES
  let ty=doc.lastAutoTable.finalY+8;
  const bx=pR-82, bw=82;
  const tots=[['Subtotal',fmt(c.costo),false],['Utilidad ('+state.utilidadPct+'%)',fmt(c.util),false]];
  doc.setFillColor(20,18,38); doc.roundedRect(bx,ty-2,bw,tots.length*9+22,4,4,'F');
  let tr=ty+6;
  tots.forEach(([l,v])=>{
    doc.setFont('helvetica','normal'); doc.setFontSize(9); doc.setTextColor(...CGR); doc.text(l,bx+4,tr);
    doc.setTextColor(...CW); doc.text(v,pR,tr,{align:'right'}); tr+=9;
  });
  doc.setDrawColor(...CA); doc.setLineWidth(0.3); doc.line(bx+4,tr,pR,tr); tr+=4;
  doc.setFillColor(...CA); doc.roundedRect(bx,tr-2,bw,13,3,3,'F');
  doc.setTextColor(0,0,0); doc.setFont('helvetica','bold'); doc.setFontSize(11);
  doc.text('TOTAL',bx+4,tr+7); doc.text(fmt(c.venta),pR,tr+7,{align:'right'});

  // NOTAS
  if(inv.docNotas){
    doc.setFillColor(20,18,38); doc.roundedRect(pL,ty-2,79,30,4,4,'F');
    doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8);
    doc.text('NOTAS',pL+4,ty+6);
    doc.setFont('helvetica','normal'); doc.setFontSize(8); doc.setTextColor(...CGR);
    const nl=doc.splitTextToSize(inv.docNotas,71);
    doc.text(nl.slice(0,3),pL+4,ty+13);
  }

  // DESGLOSE POR CATEGORIA
  let secY=ty+36;
  const clrsCat=['#f7c948','#4ecb71','#e06c3a','#6495ed','#c864c8','#50c8c8'];
  const catRows=state.sections.filter(s=>sTot(s)>0);
  if(catRows.length>0){
    doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8.5);
    doc.text('DESGLOSE POR CATEGORIA',pL,secY); secY+=5;
    doc.setFillColor(20,18,38); doc.roundedRect(pL,secY,172,catRows.length*9+8,3,3,'F');
    secY+=6;
    catRows.forEach((s,i)=>{
      const t=sTot(s); const pct=c.costo>0?((t/c.costo)*100).toFixed(1):0;
      const rgb=clrsCat[i%clrsCat.length].match(/\w\w/g).map(x=>parseInt(x,16));
      doc.setFillColor(...rgb); doc.rect(pL+4,secY-3,3,5,'F');
      doc.setFont('helvetica','normal'); doc.setFontSize(8); doc.setTextColor(...CGR);
      doc.text(s.name,pL+10,secY);
      doc.setTextColor(...CW); doc.text(pct+'%',pL+95,secY,{align:'right'});
      doc.text(fmt(t),pR,secY,{align:'right'}); secY+=9;
    });
    secY+=4;
  }

  // CALCULO RAPIDO
  const f=parseFloat(state.factor)||1.8;
  doc.setTextColor(...CA); doc.setFont('helvetica','bold'); doc.setFontSize(8.5);
  doc.text('CALCULO RAPIDO (Factor x'+f+')',pL,secY); secY+=5;
  doc.setFillColor(20,18,38); doc.roundedRect(pL,secY,172,32,3,3,'F');
  secY+=7;
  const qrRows=[['Costo base',fmt(c.costo),...CGR],['Factor aplicado','x'+f,...CA],['Precio rapido',fmt(c.qv),...CA],['Ganancia rapida',fmt(c.qg),...CG]];
  qrRows.forEach(([l,v,r,g,b])=>{
    doc.setFont('helvetica','normal'); doc.setFontSize(8); doc.setTextColor(...CGR); doc.text(l+':',pL+4,secY);
    doc.setFont('helvetica','bold'); doc.setTextColor(r,g,b); doc.text(v,pR,secY,{align:'right'}); secY+=7;
  });

  // FIRMA / FOOTER
  if(inv.clienteEmail||inv.emisorEmail){
    let fy=secY+4;
    if(inv.emisorEmail){doc.setTextColor(...CGR);doc.setFont('helvetica','normal');doc.setFontSize(8);doc.text('Emisor: '+inv.emisorEmail,pL,fy);}
    if(inv.clienteEmail){doc.setTextColor(...CGR);doc.setFont('helvetica','normal');doc.setFontSize(8);doc.text('Cliente: '+inv.clienteEmail,pL,fy+6);}
  }

  // Footer bar
  doc.setFillColor(...CD); doc.rect(0,282,W,15,'F');
  doc.setDrawColor(...CA); doc.setLineWidth(0.3); doc.line(pL,282,pR,282);
  doc.setTextColor(...CGR); doc.setFont('helvetica','normal'); doc.setFontSize(7.5);
  doc.text('Generado con AVICH Costos',pL,289);
  doc.text(numDoc+' | '+fEm,pR,289,{align:'right'});

  const fn=(esBoleta?'Boleta':'Factura')+'_'+numDoc.replace(/\s/g,'_')+'_'+(inv.clienteNombre||'cliente').replace(/\s+/g,'_')+'.pdf';
  doc.save(fn);
  toast('PDF descargado: '+fn);
}

function fmtD(s){if(!s)return'-';const[y,m,d]=s.split('-');return`${d}/${m}/${y}`;}

function switchTab(tab){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  const sc=document.getElementById('screen'+tab.charAt(0).toUpperCase()+tab.slice(1));
  if(sc)sc.classList.add('active');
  document.querySelectorAll('.tab').forEach(t=>{if(t.textContent.toLowerCase().includes(tab.slice(0,3)))t.classList.add('active');});
  if(tab==='resumen'){updateAll();document.getElementById('resumeProjectName').textContent=state.projectName;document.getElementById('resumeDate').textContent='Actualizado: '+new Date().toLocaleDateString('es-PE');}
  if(tab==='comprobante'){renderPreview();loadInvFields();selectDocType(state.docType||'boleta');}
}
function promptProjectName(){
  const n=prompt('Nombre del proyecto:',state.projectName); if(n){state.projectName=n;document.getElementById('headerProjectName').textContent=n;try{document.getElementById('resumeProjectName').textContent=n;}catch(e){}save();}
}
function toast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2500);}

function save(){try{localStorage.setItem('cmv3',JSON.stringify({state,inv}));}catch(e){}}
function loadSt(){
  try{
    const p=JSON.parse(localStorage.getItem('cmv3')||'null');
    if(p){if(p.state&&p.state.sections)state={...state,...p.state};if(p.inv)inv={...inv,...p.inv};}
  }catch(e){}
  document.getElementById('utilSlider').value=state.utilidadPct;
  document.getElementById('utilPct').textContent=state.utilidadPct+'%';
  document.getElementById('factorInput').value=state.factor||1.8;
  document.getElementById('headerProjectName').textContent=state.projectName;
  const hoy=new Date().toISOString().split('T')[0];
  if(!inv.docFecha)inv.docFecha=hoy;
  if(!inv.docVencimiento){const v=new Date();v.setDate(v.getDate()+30);inv.docVencimiento=v.toISOString().split('T')[0];}
}

document.getElementById('editModal').addEventListener('click',function(e){if(e.target===this)closeModal();});
loadSt(); renderAll();
</script>
</body>
</html>
