<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vaccine Counseling Tool</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: #fff;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      padding: 50px 30px;
    }

    .app {
      display: flex;
      gap: 44px;
      align-items: flex-start;
      width: 100%;
      max-width: 980px;
    }

    /* ── Left Panel ── */
    .left-panel { width: 310px; flex-shrink: 0; }

    .tool-title {
      font-size: 17px;
      font-weight: 900;
      color: #5c021d;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 18px;
    }

    .birthdate-input {
      width: 100%;
      background: #5c021d;
      color: white;
      border: none;
      border-radius: 8px;
      padding: 12px 16px;
      font-size: 14px;
      font-weight: 700;
      text-align: center;
      outline: none;
      margin-bottom: 10px;
      font-family: inherit;
    }
    .birthdate-input::placeholder { color: rgba(255,255,255,0.85); }

    /* Dropdown */
    .dropdown-wrap { position: relative; margin-bottom: 4px; }

    .dropdown-header {
      width: 100%;
      background: #5c021d;
      color: white;
      border: none;
      border-radius: 8px;
      padding: 12px 16px;
      font-size: 14px;
      font-weight: 700;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      user-select: none;
      font-family: inherit;
    }
    .dropdown-header.open { border-radius: 8px 8px 0 0; }
    .dropdown-arrow { font-size: 11px; }

    .dropdown-menu {
      display: none;
      position: absolute;
      top: 100%;
      left: 0; right: 0;
      background: #ea9ab2;
      border-radius: 0 0 8px 8px;
      overflow: hidden;
      z-index: 20;
    }
    .dropdown-menu.open { display: block; }

    .dropdown-item {
      padding: 10px 16px;
      cursor: pointer;
      color: #3e0213;
      font-size: 14px;
      font-weight: 500;
    }
    .dropdown-item:hover { background: rgba(255,255,255,0.35); }
    .dropdown-item.selected { font-weight: 700; background: rgba(255,255,255,0.2); }

    .clear-exclusion {
      display: none;
      font-size: 11px;
      color: #5c021d;
      cursor: pointer;
      text-align: right;
      margin-bottom: 8px;
      text-decoration: underline;
    }
    .clear-exclusion.visible { display: block; }

    /* Conditions */
    .conditions-header {
      background: #5c021d;
      color: white;
      font-weight: 700;
      font-size: 14px;
      text-align: center;
      padding: 10px 16px;
      border-radius: 8px;
      margin-top: 6px;
      margin-bottom: 12px;
    }

    .conditions-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
      margin-bottom: 18px;
      transition: opacity 0.25s;
    }
    .conditions-grid.faded { opacity: 0.38; pointer-events: none; }

    .condition-btn {
      background: #e9e6d6;
      color: #3e0213;
      border: 2px solid transparent;
      border-radius: 50px;
      padding: 9px 8px;
      font-size: 12.5px;
      font-weight: 500;
      cursor: pointer;
      text-align: center;
      min-height: 40px;
      transition: background 0.15s, border-color 0.15s;
      font-family: inherit;
      line-height: 1.3;
    }
    .condition-btn.selected {
      background: white;
      border-color: #5c021d;
      font-weight: 600;
    }

    /* GO */
    .go-wrapper { display: flex; justify-content: flex-end; margin-top: 2px; }

    .go-btn {
      background: white;
      color: #3e0213;
      border: 2.5px solid #3e0213;
      border-radius: 50px;
      padding: 9px 22px;
      font-size: 14px;
      font-weight: 700;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-family: inherit;
      transition: background 0.15s, color 0.15s;
    }
    .go-btn:hover { background: #3e0213; color: white; }
    .go-arrow { font-size: 11px; }

    /* ── Right Panel ── */
    .right-panel { flex: 1; min-width: 0; }

    .tabs-row {
      display: flex;
      gap: 5px;
      padding-left: 2px;
    }

    .tab {
      background: #3e0213;
      color: white;
      border: none;
      border-radius: 10px 10px 0 0;
      padding: 10px 22px 12px;
      font-size: 13px;
      font-weight: 700;
      letter-spacing: 0.8px;
      cursor: pointer;
      font-family: inherit;
      position: relative;
      z-index: 2;
    }
    .tab.active {
      background: #690323;
      padding-top: 12px;
    }

    .content-box {
      background: #690323;
      border-radius: 16px;
      border-top-left-radius: 4px;
      min-height: 460px;
      padding: 30px;
      color: white;
      position: relative;
      z-index: 1;
      margin-top: -1px;
    }

    /* Content styles */
    .placeholder-text {
      font-size: 16px;
      color: rgba(255,255,255,0.65);
      font-style: italic;
      line-height: 1.6;
    }

    .rejection-text {
      font-size: 15px;
      line-height: 1.7;
      color: white;
    }

    .burden-list {
      list-style: disc;
      padding-left: 22px;
      margin-bottom: 22px;
    }
    .burden-list li {
      font-size: 15px;
      line-height: 1.65;
      margin-bottom: 10px;
      color: white;
    }

    .efficacy-text p {
      font-size: 15px;
      line-height: 1.65;
      margin-bottom: 8px;
      color: white;
    }

    .error-text {
      font-size: 14px;
      color: #ea9ab2;
      font-weight: 500;
    }
  </style>
</head>
<body>
<div class="app">

  <!-- LEFT PANEL -->
  <div class="left-panel">
    <h1 class="tool-title">Vaccine Counseling Tool</h1>

    <input type="text" id="birthdate" class="birthdate-input"
           placeholder="Birthdate (MM/DD/YYYY)" maxlength="10">

    <div class="dropdown-wrap" id="dropdown-wrap">
      <div class="dropdown-header" id="dropdown-header">
        <span id="dropdown-label">Exclusion Criteria</span>
        <span class="dropdown-arrow">▼</span>
      </div>
      <div class="dropdown-menu" id="dropdown-menu">
        <div class="dropdown-item" data-value="immunocompromised">Immunocompromised</div>
        <div class="dropdown-item" data-value="pregnant">Pregnant</div>
        <div class="dropdown-item" data-value="under18">Under 18</div>
      </div>
    </div>
    <span class="clear-exclusion" id="clear-exclusion">Clear selection ×</span>

    <div class="conditions-header">Select all conditions that apply:</div>
    <div class="conditions-grid" id="conditions-grid">
      <button class="condition-btn" data-condition="Cardiovascular Disease">Cardiovascular Disease</button>
      <button class="condition-btn" data-condition="Hypertension">Hypertension</button>
      <button class="condition-btn" data-condition="Heart Failure">Heart Failure</button>
      <button class="condition-btn" data-condition="Diabetes">Diabetes</button>
      <button class="condition-btn" data-condition="CKD">CKD</button>
      <button class="condition-btn" data-condition="Asthma">Asthma</button>
      <button class="condition-btn" data-condition="COPD">COPD</button>
      <button class="condition-btn" data-condition="Obesity">Obesity</button>
    </div>

    <div class="go-wrapper">
      <button class="go-btn" id="go-btn">GO <span class="go-arrow">▶</span></button>
    </div>
  </div>

  <!-- RIGHT PANEL -->
  <div class="right-panel">
    <div class="tabs-row">
      <button class="tab active" data-tab="abrysvo">ABRYSVO</button>
      <button class="tab" data-tab="prevnar20">PREVNAR 20</button>
      <button class="tab" data-tab="shingrix">SHINGRIX</button>
    </div>
    <div class="content-box" id="content-box">
      <p class="placeholder-text">Enter the patient's birthdate and select any applicable conditions, then click GO to view personalized vaccine recommendations.</p>
    </div>
  </div>

</div>

<script>
// ── Vaccine Data ────────────────────────────────────────────────────
const vaccineData = {
  abrysvo: {
    name: 'ABRYSVO',
    minAge: 60,
    rejection: 'The RSV vaccine (e.g., Arexvy, Abrysvo) is currently recommended for adults 60 years and older using shared clinical decision-making, and for pregnant individuals to protect their infants. This tool is limited to data for adults 60 years and older and does not include data for pregnant individuals or immunocompromised individuals.',
    ageBurden: {
      '50-54': '14% of patients with RSV associated hospitalizations were in the age range of 50–54',
      '55-59': '14% of patients with RSV associated hospitalizations were in the age range of 55–59',
      '60-64': '10% of patients with RSV associated hospitalizations were in the age range of 60–64',
      '65-69': '22.9% of patients with RSV associated hospitalizations were in the age range of 65–69',
      '70-74': '22.9% of patients with RSV associated hospitalizations were in the age range of 70–74',
      '75-79': '39.4% of patients with RSV associated hospitalizations were in the age range of 75–79',
      '80-84': '39.4% of patients with RSV associated hospitalizations were in the age range of 80–84',
      '85+':   '39.4% of patients with RSV associated hospitalizations were in the age range of 85+',
    },
    conditions: {
      'Cardiovascular Disease': '57.4% of patients with RSV associated hospitalizations had cardiovascular disease',
      'Obesity':                '39% of patients with RSV associated hospitalizations had obesity',
      'Diabetes':               '34.1% of patients with RSV associated hospitalizations had diabetes',
      'COPD':                   '31.4% of patients with RSV associated hospitalizations had COPD',
      'Heart Failure':          '28% of patients with RSV associated hospitalizations had heart failure',
      'CKD':                    '27% of patients with RSV associated hospitalizations had chronic kidney disease',
      'Asthma':                 '24% of patients with RSV associated hospitalizations had asthma',
    },
    efficacy: [
      'Vaccine effectiveness of ABRYSVO against RSV related hospitalization or ED visits ranges from 73%–89%',
      'Vaccine effectiveness of ABRYSVO against RSV Lower Respiratory Tract Disease ranges from 78%–82%',
    ],
  },

  prevnar20: {
    name: 'PREVNAR 20',
    minAge: 50,
    rejection: 'The Pneumonia vaccine (PREVNAR 20) is only recommended for all PCV-naïve adults aged ≥50 years. Adults aged 19–49 years with an immunocompromising condition, a CSF leak, or a cochlear implant may also be eligible. This tool does not include data for individuals or immunocompromised adults.',
    ageBurden: {
      '50-64': 'Adults aged 50–64 years have a rate of pneumococcal disease nearly 3 times higher than those aged 18–49',
      '65+':   'Adults aged 65+ years have a rate of pneumococcal disease over 7 times higher than those aged 18–49',
    },
    conditions: {
      'Cardiovascular Disease': '35% of patients that were hospitalized for invasive pneumococcal disease had cardiovascular disease',
      'Diabetes':               '32% of patients that were hospitalized for invasive pneumococcal disease had diabetes',
      'COPD':                   '53% of patients that were hospitalized for invasive pneumococcal disease had COPD',
      'Heart Failure':          '32% of patients that were hospitalized for invasive pneumococcal disease had heart failure',
      'CKD':                    '23% of patients that were hospitalized for invasive pneumococcal disease had chronic kidney disease',
    },
    efficacy: [
      'Vaccine efficacy against a first episode of either vaccine-type nonbacteremic/noninvasive or vaccine-type bacteremic/invasive pneumococcal CAP was decreased by 46–73%',
      'Vaccine efficacy against vaccine-type CAP hospitalization was decreased by 75%',
      'Disclaimer: Built on established efficacy of Prevnar 13®',
    ],
  },

  shingrix: {
    name: 'SHINGRIX',
    minAge: 50,
    rejection: 'The Shingrix vaccine is only recommended for healthy adults 50 years and older or immunocompromised adults 19 years and older: people who are or will be at increased risk of shingles due to disease or therapy. This tool does not include data for pregnant individuals or immunocompromised adults.',
    ageBurden: {
      '50-54': '38.77% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 50–54',
      '55-59': '38.77% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 55–59',
      '60-64': '38.77% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 60–64',
      '65-69': '23.02% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 65–69',
      '70-74': '23.02% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 70–74',
      '75-79': '21.48% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 75–79',
      '80-84': '21.48% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 80–84',
      '85+':   '21.48% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection were in the age range of 85+',
    },
    conditions: {
      'Cardiovascular Disease': '63.80% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had cardiovascular disease',
      'Hypertension':           '57.40% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had hypertension',
      'Obesity':                '14.60% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had obesity',
      'Diabetes':               '15.80% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had diabetes',
      'COPD':                   '6.60% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had COPD',
      'Heart Failure':          '7.60% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had heart failure',
      'CKD':                    '13% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had chronic kidney disease',
      'Asthma':                 '9.80% of patients that developed irreversible postherpetic neuralgia (PHN) after their shingles infection had asthma',
    },
    efficacy: {
      '50-69': [
        'Initial efficacy: 97.2% against herpes zoster.',
        'Long-term efficacy: Maintained a VE of 87.7% against herpes zoster up to 11 years post-vaccination.',
        'Vaccine efficacy against postherpetic neuralgia was 87.5%',
      ],
      '70+': [
        'Initial efficacy: 97.2% against herpes zoster.',
        'Long-term efficacy: VE against HZ was sustained at 82.0% in the eleventh year post-vaccination, while a pooled analysis reported a long-term VE of 73.2%.',
      ],
    },
  },
};

// ── State ────────────────────────────────────────────────────────────
let activeTab = 'abrysvo';
let selectedExclusion = null;
let computedResults = null;

// ── Helpers ──────────────────────────────────────────────────────────
function calculateAge(str) {
  if (!str) return null;
  const parts = str.split('/');
  if (parts.length !== 3) return null;
  const [m, d, y] = parts.map(Number);
  if (!m || !d || !y || y < 1900 || y > 2100) return null;
  const bd = new Date(y, m - 1, d);
  if (isNaN(bd.getTime()) || bd.getMonth() !== m - 1) return null;
  const today = new Date();
  let age = today.getFullYear() - bd.getFullYear();
  const mDiff = today.getMonth() - bd.getMonth();
  if (mDiff < 0 || (mDiff === 0 && today.getDate() < bd.getDate())) age--;
  return age;
}

function getAgeBucket(age, key) {
  if (key === 'shingrix' || key === 'abrysvo') {
    if (age >= 85) return '85+';
    if (age >= 80) return '80-84';
    if (age >= 75) return '75-79';
    if (age >= 70) return '70-74';
    if (age >= 65) return '65-69';
    if (age >= 60) return '60-64';
    if (age >= 55) return '55-59';
    if (age >= 50) return '50-54';
  }
  if (key === 'prevnar20') {
    if (age >= 65) return '65+';
    if (age >= 50) return '50-64';
  }
  return null;
}

// ── Render ────────────────────────────────────────────────────────────
function renderContent() {
  const box = document.getElementById('content-box');

  if (!computedResults) {
    box.innerHTML = '<p class="placeholder-text">Enter the patient\'s birthdate and select any applicable conditions, then click GO to view personalized vaccine recommendations.</p>';
    return;
  }

  const { age, exclusion, conditions } = computedResults;
  const data = vaccineData[activeTab];

  if (exclusion || age < data.minAge) {
    box.innerHTML = `<p class="rejection-text">${data.rejection}</p>`;
    return;
  }

  const bullets = [];
  const bucket = getAgeBucket(age, activeTab);
  if (bucket && data.ageBurden[bucket]) bullets.push(data.ageBurden[bucket]);
  conditions.forEach(c => { if (data.conditions[c]) bullets.push(data.conditions[c]); });

  let efficacyLines = [];
  if (activeTab === 'shingrix') {
    efficacyLines = age >= 70 ? data.efficacy['70+'] : data.efficacy['50-69'];
  } else {
    efficacyLines = data.efficacy;
  }

  let html = '';
  if (bullets.length) {
    html += '<ul class="burden-list">' + bullets.map(b => `<li>${b}</li>`).join('') + '</ul>';
  }
  if (efficacyLines && efficacyLines.length) {
    html += '<div class="efficacy-text">' + efficacyLines.map(l => `<p>${l}</p>`).join('') + '</div>';
  }

  box.innerHTML = html || '<p class="placeholder-text">No data available for this patient profile.</p>';
}

// ── Tabs ──────────────────────────────────────────────────────────────
document.querySelectorAll('.tab').forEach(tab => {
  tab.addEventListener('click', () => {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    tab.classList.add('active');
    activeTab = tab.dataset.tab;
    renderContent();
  });
});

// ── Condition Buttons ─────────────────────────────────────────────────
document.querySelectorAll('.condition-btn').forEach(btn => {
  btn.addEventListener('click', () => btn.classList.toggle('selected'));
});

// ── Dropdown ──────────────────────────────────────────────────────────
const dropdownHeader = document.getElementById('dropdown-header');
const dropdownMenu   = document.getElementById('dropdown-menu');
const dropdownLabel  = document.getElementById('dropdown-label');
const clearBtn       = document.getElementById('clear-exclusion');
const condGrid       = document.getElementById('conditions-grid');

dropdownHeader.addEventListener('click', () => {
  dropdownMenu.classList.toggle('open');
  dropdownHeader.classList.toggle('open');
});

document.querySelectorAll('.dropdown-item').forEach(item => {
  item.addEventListener('click', () => {
    document.querySelectorAll('.dropdown-item').forEach(i => i.classList.remove('selected'));
    selectedExclusion = item.dataset.value;
    dropdownLabel.textContent = item.textContent;
    item.classList.add('selected');
    dropdownMenu.classList.remove('open');
    dropdownHeader.classList.remove('open');
    clearBtn.classList.add('visible');
    condGrid.classList.add('faded');
  });
});

clearBtn.addEventListener('click', () => {
  selectedExclusion = null;
  dropdownLabel.textContent = 'Exclusion Criteria';
  document.querySelectorAll('.dropdown-item').forEach(i => i.classList.remove('selected'));
  clearBtn.classList.remove('visible');
  condGrid.classList.remove('faded');
});

document.addEventListener('click', e => {
  if (!document.getElementById('dropdown-wrap').contains(e.target)) {
    dropdownMenu.classList.remove('open');
    dropdownHeader.classList.remove('open');
  }
});

// ── GO Button ─────────────────────────────────────────────────────────
document.getElementById('go-btn').addEventListener('click', () => {
  const age = calculateAge(document.getElementById('birthdate').value.trim());
  if (age === null) {
    computedResults = null;
    document.getElementById('content-box').innerHTML =
      '<p class="error-text">Please enter a valid birthdate in MM/DD/YYYY format.</p>';
    return;
  }
  const conditions = Array.from(document.querySelectorAll('.condition-btn.selected'))
    .map(b => b.dataset.condition);
  computedResults = { age, exclusion: selectedExclusion, conditions };
  renderContent();
});

// ── Birthdate auto-format ─────────────────────────────────────────────
document.getElementById('birthdate').addEventListener('input', function () {
  let v = this.value.replace(/\D/g, '');
  if (v.length > 2) v = v.slice(0, 2) + '/' + v.slice(2);
  if (v.length > 5) v = v.slice(0, 5) + '/' + v.slice(5);
  this.value = v.slice(0, 10);
});
</script>
</body>
</html>
