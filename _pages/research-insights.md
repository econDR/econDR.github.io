---
layout: page
title: "research insights"
description: "What I have learned from my research: an interactive map and plain-language summaries of the main findings on institutions, democracy, cooperation, and behavior, with links to the underlying papers."
permalink: /research-insights/
nav: false
nav_order: 3
sitemap: false
---

<style>
  #research-insights-page { scroll-behavior: smooth; }

  .insights-intro {
    margin-bottom: 1.5rem;
    max-width: 60rem;
  }

  .insights-stats {
    display: flex;
    flex-wrap: wrap;
    gap: 1.25rem;
    margin-bottom: 2rem;
  }

  .insights-stat {
    flex: 1 1 140px;
    text-align: center;
    padding: 0.85rem 0.5rem;
    border-radius: 10px;
    background-color: var(--global-card-bg-color);
    border: 1px solid rgba(128, 128, 128, 0.25);
  }

  .insights-stat .stat-number {
    display: block;
    font-size: 1.6rem;
    font-weight: 700;
    color: var(--global-theme-color);
    line-height: 1.2;
  }

  .insights-stat .stat-label {
    font-size: 0.8rem;
    color: var(--global-text-color);
    opacity: 0.75;
  }

  /* --- Concept map --- */
  .insights-map-wrap {
    margin-bottom: 2.5rem;
    border: 1px solid rgba(128, 128, 128, 0.25);
    border-radius: 12px;
    background-color: var(--global-card-bg-color);
    padding: 0.5rem;
  }

  .insights-map-wrap svg {
    width: 100%;
    height: auto;
    display: block;
  }

  .map-node-circle {
    fill: var(--global-bg-color);
    stroke: var(--global-theme-color);
    stroke-width: 2px;
    transition: transform 0.15s ease, fill 0.15s ease;
    transform-box: fill-box;
    transform-origin: center;
  }

  .map-node-center .map-node-circle {
    fill: var(--global-theme-color);
  }

  .map-node-theme .map-node-circle {
    stroke-width: 2.5px;
  }

  .map-link:hover .map-node-circle,
  .map-link:focus .map-node-circle {
    transform: scale(1.12);
    fill: var(--global-theme-color);
  }

  .map-link:hover .map-node-label,
  .map-link:focus .map-node-label {
    font-weight: 700;
  }

  .map-node-label {
    fill: var(--global-text-color);
    font-size: 12.5px;
    text-anchor: middle;
    pointer-events: none;
  }

  .map-node-center .map-node-label {
    fill: var(--global-bg-color);
    font-size: 13px;
    font-weight: 700;
  }

  .map-edge {
    stroke: var(--global-text-color);
    stroke-opacity: 0.25;
    stroke-width: 1.5px;
    fill: none;
  }

  .map-theme-group {
    transition: opacity 0.25s ease;
  }

  .map-theme-group.dimmed {
    opacity: 0.18;
  }

  .insights-hint {
    font-size: 0.85rem;
    opacity: 0.7;
    margin: 0.35rem 0 0 0.25rem;
  }

  /* --- Filters --- */
  .insights-filters {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 1.75rem;
  }

  .filter-chip {
    border: 1px solid var(--global-theme-color);
    color: var(--global-theme-color);
    background: transparent;
    border-radius: 999px;
    padding: 0.35rem 0.9rem;
    font-size: 0.9rem;
    cursor: pointer;
    transition: background-color 0.15s ease, color 0.15s ease;
  }

  .filter-chip.active,
  .filter-chip:hover {
    background-color: var(--global-theme-color);
    color: var(--global-bg-color);
  }

  /* --- Card grid --- */
  .insights-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 1.1rem;
  }

  .insight-card {
    border: 1px solid rgba(128, 128, 128, 0.25);
    border-radius: 10px;
    background-color: var(--global-card-bg-color);
    padding: 1rem 1.1rem;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .insight-card .card-icon {
    font-size: 1.3rem;
  }

  .insight-card .insight-claim {
    font-weight: 600;
    line-height: 1.35;
  }

  .insight-card .insight-detail {
    font-size: 0.92rem;
    opacity: 0.9;
    line-height: 1.55;
  }

  .insight-card .insight-links a {
    font-size: 0.85rem;
    margin-right: 0.6rem;
  }

  .insights-theme-heading {
    margin-top: 2.25rem;
    margin-bottom: 0.9rem;
    scroll-margin-top: 90px;
  }
</style>

<div id="research-insights-page">

<div class="insights-intro">
  <em>What I have learned.</em> This page distills the main findings of my research in plain
  language. Explore the map below, or filter the findings by theme — each card links to the
  paper where the full evidence is presented.
</div>

<div class="insights-stats">
  <div class="insights-stat"><span class="stat-number">12</span><span class="stat-label">findings</span></div>
  <div class="insights-stat"><span class="stat-number">4</span><span class="stat-label">research themes</span></div>
  <div class="insights-stat"><span class="stat-number">6</span><span class="stat-label">centuries of history</span></div>
  <div class="insights-stat"><span class="stat-number">3</span><span class="stat-label">continents</span></div>
</div>

<div class="insights-map-wrap">
  <svg viewBox="0 0 900 620" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Concept map of research themes and papers">

    <!-- edges: center to themes -->
    <path class="map-edge" d="M450,310 L230,140"></path>
    <path class="map-edge" d="M450,310 L670,140"></path>
    <path class="map-edge" d="M450,310 L230,480"></path>
    <path class="map-edge" d="M450,310 L670,480"></path>

    <!-- theme A: institutions -->
    <g class="map-theme-group institutions">
      <path class="map-edge" d="M230,140 L70,70"></path>
      <path class="map-edge" d="M230,140 L70,140"></path>
      <path class="map-edge" d="M230,140 L70,210"></path>

      <a class="map-link" href="#theme-institutions">
        <g class="map-node-theme">
          <circle class="map-node-circle" cx="230" cy="140" r="27"></circle>
          <text class="map-node-label" x="230" y="136"><tspan x="230" dy="0">🏛️</tspan><tspan x="230" dy="14">Institutions</tspan></text>
        </g>
      </a>

      <a class="map-link" href="https://doi.org/10.1017/S0022050724000330" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="70" r="20"></circle>
          <text class="map-node-label" x="70" y="67"><tspan x="70" dy="0">Ora et</tspan><tspan x="70" dy="13">Guberna</tspan></text>
        </g>
      </a>
      <a class="map-link" href="{{ '/papers/monti-di-pieta/' | relative_url }}">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="140" r="20"></circle>
          <text class="map-node-label" x="70" y="137"><tspan x="70" dy="0">Monti di</tspan><tspan x="70" dy="13">Pietà</tspan></text>
        </g>
      </a>
      <a class="map-link" href="{{ '/papers/feudal-origins/' | relative_url }}">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="210" r="20"></circle>
          <text class="map-node-label" x="70" y="207"><tspan x="70" dy="0">Feudal</tspan><tspan x="70" dy="13">Sicily</tspan></text>
        </g>
      </a>
    </g>

    <!-- theme B: democracy -->
    <g class="map-theme-group democracy">
      <path class="map-edge" d="M670,140 L830,70"></path>
      <path class="map-edge" d="M670,140 L830,140"></path>
      <path class="map-edge" d="M670,140 L830,210"></path>

      <a class="map-link" href="#theme-democracy">
        <g class="map-node-theme">
          <circle class="map-node-circle" cx="670" cy="140" r="27"></circle>
          <text class="map-node-label" x="670" y="136"><tspan x="670" dy="0">🗳️</tspan><tspan x="670" dy="14">Democracy</tspan></text>
        </g>
      </a>

      <a class="map-link" href="https://doi.org/10.1016/j.ejpoleco.2019.101824" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="70" r="20"></circle>
          <text class="map-node-label" x="830" y="67"><tspan x="830" dy="0">Democracy</tspan><tspan x="830" dy="13">&amp; growth</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1080/02255189.2017.1382335" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="140" r="20"></circle>
          <text class="map-node-label" x="830" y="137"><tspan x="830" dy="0">Food</tspan><tspan x="830" dy="13">security</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1016/j.ejpoleco.2020.101939" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="210" r="20"></circle>
          <text class="map-node-label" x="830" y="207"><tspan x="830" dy="0">Media</tspan><tspan x="830" dy="13">capture</tspan></text>
        </g>
      </a>
    </g>

    <!-- theme C: cooperation -->
    <g class="map-theme-group cooperation">
      <path class="map-edge" d="M230,480 L70,410"></path>
      <path class="map-edge" d="M230,480 L70,480"></path>
      <path class="map-edge" d="M230,480 L70,550"></path>

      <a class="map-link" href="#theme-cooperation">
        <g class="map-node-theme">
          <circle class="map-node-circle" cx="230" cy="480" r="27"></circle>
          <text class="map-node-label" x="230" y="476"><tspan x="230" dy="0">🤝</tspan><tspan x="230" dy="14">Cooperation</tspan></text>
        </g>
      </a>

      <a class="map-link" href="{{ '/papers/noise-and-cooperation/' | relative_url }}">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="410" r="20"></circle>
          <text class="map-node-label" x="70" y="407"><tspan x="70" dy="0">Noise &amp;</tspan><tspan x="70" dy="13">cooperation</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1016/j.socec.2023.102011" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="480" r="20"></circle>
          <text class="map-node-label" x="70" y="477"><tspan x="70" dy="0">Human–robot</tspan><tspan x="70" dy="13">cooperation</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1007/s40797-026-00388-z" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="70" cy="550" r="20"></circle>
          <text class="map-node-label" x="70" y="547"><tspan x="70" dy="0">Ultimatum</tspan><tspan x="70" dy="13">game order</tspan></text>
        </g>
      </a>
    </g>

    <!-- theme D: field interventions -->
    <g class="map-theme-group field">
      <path class="map-edge" d="M670,480 L830,410"></path>
      <path class="map-edge" d="M670,480 L830,480"></path>
      <path class="map-edge" d="M670,480 L830,550"></path>

      <a class="map-link" href="#theme-field">
        <g class="map-node-theme">
          <circle class="map-node-circle" cx="670" cy="480" r="27"></circle>
          <text class="map-node-label" x="670" y="476"><tspan x="670" dy="0">🌍</tspan><tspan x="670" dy="14">Field work</tspan></text>
        </g>
      </a>

      <a class="map-link" href="https://doi.org/10.1016/j.joep.2017.12.003" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="410" r="20"></circle>
          <text class="map-node-label" x="830" y="407"><tspan x="830" dy="0">Trust</tspan><tspan x="830" dy="13">behind bars</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1016/j.econedurev.2023.102383" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="480" r="20"></circle>
          <text class="map-node-label" x="830" y="477"><tspan x="830" dy="0">High-dosage</tspan><tspan x="830" dy="13">tutoring</tspan></text>
        </g>
      </a>
      <a class="map-link" href="https://doi.org/10.1093/jae/ejab007" target="_blank" rel="noopener">
        <g class="map-node-leaf">
          <circle class="map-node-circle" cx="830" cy="550" r="20"></circle>
          <text class="map-node-label" x="830" y="547"><tspan x="830" dy="0">Child</tspan><tspan x="830" dy="13">sponsorship</tspan></text>
        </g>
      </a>
    </g>

    <!-- center -->
    <g class="map-node-center">
      <circle class="map-node-circle" cx="450" cy="310" r="36"></circle>
      <text class="map-node-label" x="450" y="306"><tspan x="450" dy="0">My</tspan><tspan x="450" dy="14">research</tspan></text>
    </g>
  </svg>
</div>
<p class="insights-hint">Click a theme to jump to its section below, or a paper to open it directly.</p>

<div class="insights-filters" role="group" aria-label="Filter findings by theme">
  <button class="filter-chip active" data-filter="all">All</button>
  <button class="filter-chip" data-filter="institutions">🏛️ Institutions</button>
  <button class="filter-chip" data-filter="democracy">🗳️ Democracy</button>
  <button class="filter-chip" data-filter="cooperation">🤝 Cooperation</button>
  <button class="filter-chip" data-filter="field">🌍 Field work</button>
</div>

<div class="insights-grid" id="insights-grid">

  <h2 class="insights-theme-heading" id="theme-institutions" data-theme="institutions">🏛️ Institutions and long-run development</h2>

  <div class="insight-card" data-theme="institutions">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">Medieval religious institutions left an economic footprint that is measurable centuries later.</div>
    <div class="insight-detail">In medieval England, monasteries following the Rule of St Benedict — which prescribed work, discipline, and careful administration — fostered better economic outcomes in the areas they managed, compared to otherwise similar monastic lands.</div>
    <div class="insight-links"><a href="https://doi.org/10.1017/S0022050724000330" target="_blank" rel="noopener">The Journal of Economic History, 2024 →</a></div>
  </div>

  <div class="insight-card" data-theme="institutions">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">Religious persuasion can build financial institutions: Franciscan preaching drove the birth of the Monti di Pietà.</div>
    <div class="insight-detail">In 15th-century Italy, itinerant Franciscan preachers legitimized lending to the poor and triggered the foundation of Monti di Pietà — early charitable credit institutions — especially in communities hit by natural disasters, with lasting effects on social capital.</div>
    <div class="insight-links"><a href="{{ '/papers/monti-di-pieta/' | relative_url }}">Working paper →</a></div>
  </div>

  <div class="insight-card" data-theme="institutions">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">How feudal lords were monitored mattered for development.</div>
    <div class="insight-detail">In medieval Sicily, territories temporarily repossessed by the Crown suffered less rent extraction and fewer agency problems — and display persistently better local economic outcomes today.</div>
    <div class="insight-links"><a href="{{ '/papers/feudal-origins/' | relative_url }}">Work in progress →</a></div>
  </div>

  <h2 class="insights-theme-heading" id="theme-democracy" data-theme="democracy">🗳️ Democracy, institutions, and accountability</h2>

  <div class="insight-card" data-theme="democracy">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Democracy does cause growth — modestly and mostly through indirect channels.</div>
    <div class="insight-detail">Pooling the evidence from more than 2,000 published regressions in a meta-analysis, the effect of democracy on economic growth is positive, but much of it operates indirectly, through channels such as human capital and economic freedom.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.ejpoleco.2019.101824" target="_blank" rel="noopener">European Journal of Political Economy, 2020 →</a></div>
  </div>

  <div class="insight-card" data-theme="democracy">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Inclusive institutions improve food security.</div>
    <div class="insight-detail">Across developing countries, democratic and inclusive institutions are associated with better food security outcomes — political inclusion has very concrete material consequences.</div>
    <div class="insight-links"><a href="https://doi.org/10.1080/02255189.2017.1382335" target="_blank" rel="noopener">Canadian Journal of Development Studies, 2018 →</a></div>
  </div>

  <div class="insight-card" data-theme="democracy">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Media competition shapes whether voters can hold politicians accountable.</div>
    <div class="insight-detail">When incumbents can "buy the silence" of news outlets, the structure of the media market — how many outlets compete and at what price they can be captured — determines how much information reaches voters and how effective elections are as an accountability device.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.ejpoleco.2020.101939" target="_blank" rel="noopener">European Journal of Political Economy, 2021 →</a></div>
  </div>

  <h2 class="insights-theme-heading" id="theme-cooperation" data-theme="cooperation">🤝 Cooperation, behavior, and human–machine interaction</h2>

  <div class="insight-card" data-theme="cooperation">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">Noise erodes cooperation, and talking about it afterwards does not fix it.</div>
    <div class="insight-detail">In indefinitely repeated Prisoner's Dilemma experiments, when intended actions are sometimes reversed by chance ("noise"), mutual cooperation weakens — and ex-post communication helps only when noise is absent or low.</div>
    <div class="insight-links"><a href="{{ '/papers/noise-and-cooperation/' | relative_url }}">Work in progress →</a></div>
  </div>

  <div class="insight-card" data-theme="cooperation">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">People cooperate with robots — more so when robots look and speak like humans.</div>
    <div class="insight-detail">In strategic interactions, communication and human-like features of a robotic counterpart increase human cooperation, and people attribute mental states to robots that behave consistently over time.</div>
    <div class="insight-links">
      <a href="https://doi.org/10.1016/j.socec.2023.102011" target="_blank" rel="noopener">JBEE, 2023 →</a>
      <a href="https://doi.org/10.1089/cyber.2023.0353" target="_blank" rel="noopener">Cyberpsychology, 2024 →</a>
    </div>
  </div>

  <div class="insight-card" data-theme="cooperation">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">Playing first changes how fair you are: order of play matters in the Ultimatum Game.</div>
    <div class="insight-detail">Experiencing both roles in the Ultimatum Game, and the order in which subjects experience them, affects the consistency of their behavior — evidence that perspective-taking shapes strategic fairness.</div>
    <div class="insight-links"><a href="https://doi.org/10.1007/s40797-026-00388-z" target="_blank" rel="noopener">Italian Economic Journal, 2026 →</a></div>
  </div>

  <h2 class="insights-theme-heading" id="theme-field" data-theme="field">🌍 Behavioral interventions in the field</h2>

  <div class="insight-card" data-theme="field">
    <div class="card-icon">🌍</div>
    <div class="insight-claim">Prosocial preferences are not fixed: targeted programs can rebuild trust, even behind bars.</div>
    <div class="insight-detail">Lab-in-the-field experiments with inmates show that rehabilitation programs can measurably increase trust and prosocial behavior, and incarcerated participants in the GRIP program (Guiding Rage Into Power) report meaningful socio-emotional change.</div>
    <div class="insight-links">
      <a href="https://doi.org/10.1016/j.joep.2017.12.003" target="_blank" rel="noopener">JEP, 2018 →</a>
      <a href="https://doi.org/10.1002/casp.70013" target="_blank" rel="noopener">JCASP, 2025 →</a>
    </div>
  </div>

  <div class="insight-card" data-theme="field">
    <div class="card-icon">🌍</div>
    <div class="insight-claim">Intensive tutoring can narrow the income-achievement gap.</div>
    <div class="insight-detail">Experimental evidence from high-dosage tutoring in Dutch primary schools shows that well-designed, intensive educational support significantly improves outcomes for disadvantaged pupils.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.econedurev.2023.102383" target="_blank" rel="noopener">Economics of Education Review, 2023 →</a></div>
  </div>

  <div class="insight-card" data-theme="field">
    <div class="card-icon">🌍</div>
    <div class="insight-claim">Child sponsorship works: sponsored children in Goma (DRC) perform better at school.</div>
    <div class="insight-detail">Evidence from an international child sponsorship program in the Democratic Republic of Congo shows significant improvements in school performance among sponsored children.</div>
    <div class="insight-links"><a href="https://doi.org/10.1093/jae/ejab007" target="_blank" rel="noopener">Journal of African Economies, 2022 →</a></div>
  </div>

</div>

<p style="margin-top: 2rem;">
  For the full list of publications, see the <a href="{{ '/publications/' | relative_url }}">research</a> page;
  for ongoing projects, see <a href="{{ '/work-in-progress/' | relative_url }}">work in progress</a>.
</p>

</div>

<script>
  (function () {
    var chips = document.querySelectorAll(".filter-chip");
    var cards = document.querySelectorAll(".insight-card");
    var headings = document.querySelectorAll(".insights-theme-heading");
    var mapGroups = document.querySelectorAll(".map-theme-group");

    chips.forEach(function (chip) {
      chip.addEventListener("click", function () {
        chips.forEach(function (c) { c.classList.remove("active"); });
        chip.classList.add("active");
        var theme = chip.dataset.filter;

        cards.forEach(function (card) {
          card.style.display = (theme === "all" || card.dataset.theme === theme) ? "" : "none";
        });
        headings.forEach(function (h) {
          h.style.display = (theme === "all" || h.dataset.theme === theme) ? "" : "none";
        });
        mapGroups.forEach(function (g) {
          if (theme === "all" || g.classList.contains(theme)) {
            g.classList.remove("dimmed");
          } else {
            g.classList.add("dimmed");
          }
        });
      });
    });
  })();
</script>
