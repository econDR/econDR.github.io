---
layout: page
title: "research insights"
description: "What I have learned from my research: an interactive map and plain-language summaries of the main findings on institutions, political regimes, cooperation, and behavior, with links to the underlying papers."
permalink: /research-insights/
nav: false
nav_order: 3
sitemap: false
---

<style>
  #research-insights-page {
    --rn-institutions: #8b3a3a;
    --rn-politics: #2f6b63;
    --rn-cooperation: #b07f26;
    --rn-field: #35577a;
    --rn-gold: #c8963f;
    --rn-gray: #8a94a8;
    --rn-band-opacity: 0.12;
    --rn-line: rgba(128, 128, 128, 0.25);
  }

  html[data-theme="dark"] #research-insights-page {
    --rn-institutions: #d98b8b;
    --rn-politics: #62b8aa;
    --rn-cooperation: #e3b75e;
    --rn-field: #82a9d6;
    --rn-gold: #e2b864;
    --rn-gray: #7c869a;
    --rn-band-opacity: 0.2;
  }

  #research-insights-page .t-institutions { --c: var(--rn-institutions); }
  #research-insights-page .t-politics { --c: var(--rn-politics); }
  #research-insights-page .t-cooperation { --c: var(--rn-cooperation); }
  #research-insights-page .t-field { --c: var(--rn-field); }

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
    border: 1px solid var(--rn-line);
  }

  .insights-stat .stat-number {
    display: block;
    font-size: 1.6rem;
    font-weight: 700;
    color: var(--global-theme-color);
    line-height: 1.2;
    font-variant-numeric: tabular-nums;
  }

  .insights-stat .stat-label {
    font-size: 0.8rem;
    color: var(--global-text-color);
    opacity: 0.75;
  }

  /* --- Research network: map + detail panel --- */
  .rn-shell {
    display: grid;
    grid-template-columns: minmax(0, 1.5fr) minmax(250px, 1fr);
    gap: 0;
    border: 1px solid var(--rn-line);
    border-radius: 12px;
    background-color: var(--global-card-bg-color);
    overflow: hidden;
  }

  .rn-canvas {
    position: relative;
    padding: 0.75rem 0.5rem 0.25rem;
  }

  .rn-canvas svg {
    width: 100%;
    height: auto;
    max-height: 580px;
    display: block;
    margin: 0 auto;
    touch-action: manipulation;
  }

  .rn-band { fill: var(--c); opacity: var(--rn-band-opacity); transition: opacity 0.2s ease; }
  .rn-band.cold { opacity: 0.03; }

  .rn-chord {
    fill: none;
    stroke: var(--rn-gold);
    stroke-width: 2.2px;
    stroke-opacity: 0.6;
    stroke-linecap: round;
    cursor: pointer;
    transition: stroke-opacity 0.18s ease, stroke-width 0.18s ease;
  }

  .rn-chord.same { stroke: var(--rn-gray); stroke-width: 1.5px; stroke-opacity: 0.45; }
  .rn-chord.hot { stroke-opacity: 1; stroke-width: 3.4px; }
  .rn-chord.same.hot { stroke-width: 2.6px; }
  .rn-chord.cold { stroke-opacity: 0.06; }

  .rn-node { cursor: pointer; transition: opacity 0.18s ease; outline: none; }
  .rn-node.cold { opacity: 0.22; }

  .rn-dot {
    fill: var(--global-card-bg-color);
    stroke: var(--c);
    stroke-width: 3px;
    transition: r 0.15s ease, fill 0.15s ease;
  }

  .rn-node.draft .rn-dot { stroke-dasharray: 3.5 2.6; }
  .rn-node.selected .rn-dot { fill: var(--c); }
  .rn-node:focus-visible .rn-dot { stroke-width: 4.5px; }

  .rn-label {
    fill: var(--global-text-color);
    font-size: 15px;
    pointer-events: none;
  }

  .rn-node:hover .rn-label,
  .rn-node.selected .rn-label,
  .rn-node:focus-visible .rn-label { font-weight: 700; }

  .rn-center-title {
    fill: var(--global-text-color);
    font-size: 23px;
    font-weight: 700;
    text-anchor: middle;
    font-family: Georgia, "Times New Roman", serif;
  }

  .rn-center-sub {
    fill: var(--global-text-color);
    opacity: 0.6;
    font-size: 11.5px;
    letter-spacing: 0.07em;
    text-anchor: middle;
  }

  .rn-tooltip {
    position: absolute;
    pointer-events: none;
    background: #16233d;
    color: #fff;
    font-size: 0.78rem;
    line-height: 1.4;
    padding: 0.5rem 0.7rem;
    border-radius: 8px;
    max-width: 240px;
    opacity: 0;
    transition: opacity 0.15s ease;
    transform: translate(-50%, calc(-100% - 12px));
    box-shadow: 0 8px 22px rgba(0, 0, 0, 0.35);
    z-index: 5;
  }

  .rn-tooltip .tt-title { font-weight: 700; color: #e2c284; display: block; margin-bottom: 0.1rem; }
  .rn-tooltip.visible { opacity: 1; }

  .rn-panel {
    border-left: 1px solid var(--rn-line);
    padding: 1.4rem 1.4rem 1.25rem;
    display: flex;
    flex-direction: column;
    gap: 0.65rem;
    min-height: 100%;
  }

  .rn-eyebrow {
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--c, var(--global-theme-color));
    margin: 0;
  }

  .rn-p-title {
    font-size: 1.12rem;
    line-height: 1.35;
    margin: 0;
    text-wrap: balance;
  }

  .rn-p-meta { font-size: 0.82rem; opacity: 0.7; margin: 0; }
  .rn-p-text { font-size: 0.93rem; line-height: 1.55; margin: 0; }

  .rn-pill {
    display: inline-block;
    font-size: 0.68rem;
    font-weight: 700;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    padding: 0.18rem 0.55rem;
    border-radius: 999px;
    border: 1px solid var(--c);
    color: var(--c);
    margin-left: 0.4rem;
    vertical-align: 1px;
  }

  .rn-pill.draft { border-style: dashed; }

  .rn-p-cta {
    align-self: flex-start;
    font-size: 0.88rem;
    font-weight: 600;
    padding: 0.4rem 0.9rem;
    border-radius: 999px;
    border: 1px solid var(--global-theme-color);
    color: var(--global-theme-color);
    text-decoration: none;
    transition: background-color 0.15s ease, color 0.15s ease;
  }

  .rn-p-cta:hover,
  .rn-p-cta:focus-visible {
    background-color: var(--global-theme-color);
    color: var(--global-bg-color);
    text-decoration: none;
  }

  .rn-p-sub {
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    opacity: 0.6;
    margin: 0.5rem 0 0;
  }

  .rn-list { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: 0.15rem; }

  .rn-list button {
    width: 100%;
    display: flex;
    align-items: baseline;
    gap: 0.55rem;
    text-align: left;
    background: transparent;
    border: none;
    border-radius: 6px;
    padding: 0.4rem 0.5rem;
    color: var(--global-text-color);
    font-size: 0.86rem;
    line-height: 1.35;
    cursor: pointer;
  }

  .rn-list button:hover,
  .rn-list button:focus-visible { background-color: var(--rn-line); outline: none; }
  .rn-list button.active { background-color: var(--rn-line); font-weight: 600; }

  .rn-sw { flex: none; width: 10px; height: 10px; border-radius: 3px; background: var(--c); transform: translateY(1px); }
  .rn-count { margin-left: auto; font-weight: 700; opacity: 0.7; font-variant-numeric: tabular-nums; }
  .rn-concept { flex: none; font-size: 0.72rem; font-weight: 700; color: var(--rn-gold); min-width: 0; }
  .rn-list .rn-desc { display: block; font-size: 0.78rem; opacity: 0.7; font-weight: 400; }

  .rn-back {
    align-self: flex-start;
    margin-top: auto;
    background: transparent;
    border: none;
    padding: 0.5rem 0 0;
    font-size: 0.82rem;
    color: var(--global-theme-color);
    cursor: pointer;
  }

  .rn-legend {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem 1.2rem;
    justify-content: center;
    padding: 0.8rem 0 0.2rem;
    font-size: 0.8rem;
    color: var(--global-text-color);
  }

  .rn-legend .lg { display: inline-flex; align-items: center; gap: 0.4rem; opacity: 0.85; }
  .rn-legend .sep { width: 1px; height: 14px; background: var(--rn-line); }
  .rn-legend svg { width: 14px; height: 14px; }
  .rn-legend circle { fill: none; stroke: var(--global-text-color); stroke-width: 2; }

  .insights-hint {
    font-size: 0.85rem;
    opacity: 0.7;
    margin: 0.2rem 0 2rem;
    text-align: center;
    font-style: italic;
  }

  @media (max-width: 760px) {
    .rn-shell { grid-template-columns: 1fr; }
    .rn-panel { border-left: none; border-top: 1px solid var(--rn-line); }
    .rn-canvas { padding: 0.75rem 1.75rem 0.25rem; }
    .rn-canvas svg { overflow: visible; }
    .rn-label { font-size: 19px; }
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
    border: 1px solid var(--rn-line);
    border-radius: 10px;
    background-color: var(--global-card-bg-color);
    padding: 1rem 1.1rem;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    scroll-margin-top: 90px;
  }

  .insight-card .card-icon { font-size: 1.3rem; }
  .insight-card .insight-claim { font-weight: 600; line-height: 1.35; }
  .insight-card .insight-detail { font-size: 0.92rem; opacity: 0.9; line-height: 1.55; }
  .insight-card .insight-links { margin-top: auto; }
  .insight-card .insight-links a { font-size: 0.85rem; margin-right: 0.6rem; }

  .insights-theme-heading {
    grid-column: 1 / -1;
    margin-top: 2.25rem;
    margin-bottom: 0;
    scroll-margin-top: 90px;
  }

  .insights-theme-heading:first-child { margin-top: 0; }
</style>

<div id="research-insights-page">

<div class="insights-intro">
  <em>What I have learned.</em> This page distills the main findings of my research in plain
  language. In the map below, each point is a study and each chord a concept that links two of
  them; golden chords cross from one research theme to another. Select a paper to read its main
  finding, or browse all the findings by theme further down.
</div>

<div class="insights-stats">
  <div class="insights-stat"><span class="stat-number">14</span><span class="stat-label">studies</span></div>
  <div class="insights-stat"><span class="stat-number">4</span><span class="stat-label">research themes</span></div>
  <div class="insights-stat"><span class="stat-number">900+</span><span class="stat-label">years of history</span></div>
  <div class="insights-stat"><span class="stat-number">4</span><span class="stat-label">continents</span></div>
</div>

<div class="rn-shell" id="rn-wrap">
  <div class="rn-canvas" id="rn-canvas">
    <svg id="rn-svg" viewBox="0 0 640 580" role="group" aria-label="Network of research papers and the concepts that connect them"></svg>
    <div class="rn-tooltip" id="rn-tooltip" aria-hidden="true"></div>
  </div>
  <aside class="rn-panel" id="rn-panel" aria-live="polite"></aside>
</div>

<div class="rn-legend">
  <span class="lg t-institutions"><span class="rn-sw"></span>Institutions</span>
  <span class="lg t-politics"><span class="rn-sw"></span>Political regimes</span>
  <span class="lg t-cooperation"><span class="rn-sw"></span>Cooperation</span>
  <span class="lg t-field"><span class="rn-sw"></span>Field work</span>
  <span class="sep"></span>
  <span class="lg"><svg viewBox="0 0 14 14" aria-hidden="true"><circle cx="7" cy="7" r="5"></circle></svg>Published</span>
  <span class="lg"><svg viewBox="0 0 14 14" aria-hidden="true"><circle cx="7" cy="7" r="5" stroke-dasharray="2.6 2"></circle></svg>Working paper</span>
</div>
<p class="insights-hint">Select a paper to see its finding and connections · hover a chord to read the concept it stands for.</p>

<div class="insights-filters" role="group" aria-label="Filter findings by theme">
  <button class="filter-chip active" data-filter="all">All</button>
  <button class="filter-chip" data-filter="institutions">🏛️ Institutions</button>
  <button class="filter-chip" data-filter="politics">🗳️ Political regimes</button>
  <button class="filter-chip" data-filter="cooperation">🤝 Cooperation</button>
  <button class="filter-chip" data-filter="field">🌍 Field work</button>
</div>

<div class="insights-grid" id="insights-grid">

  <h2 class="insights-theme-heading" id="theme-institutions" data-theme="institutions">🏛️ Institutions and long-run development</h2>

  <div class="insight-card" data-theme="institutions" id="card-ora">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">Medieval religious institutions left an economic footprint that is measurable centuries later.</div>
    <div class="insight-detail">In medieval England, monasteries following the Rule of St Benedict — which prescribed work, discipline, and careful administration — fostered better economic outcomes in the areas they managed, compared to otherwise similar monastic lands.</div>
    <div class="insight-links"><a href="https://doi.org/10.1017/S0022050724000330" target="_blank" rel="noopener">The Journal of Economic History, 2024 →</a></div>
  </div>

  <div class="insight-card" data-theme="institutions" id="card-cross">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">Monastic lordship came with conflict: when the monasteries were dissolved, local violence fell.</div>
    <div class="insight-detail">Linking more than 18,000 Domesday manors to geocoded conflict events (1300–1700), conflict near Benedictine manors fell relative to secular manors after the Dissolution of the Monasteries (1536–38) — most of all around high-value and fortified manors.</div>
    <div class="insight-links"><a href="{{ '/papers/cross-and-sword/' | relative_url }}">Work in progress →</a></div>
  </div>

  <div class="insight-card" data-theme="institutions" id="card-monti">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">Religious persuasion can build financial institutions: Franciscan preaching drove the birth of the Monti di Pietà.</div>
    <div class="insight-detail">In 15th-century Italy, itinerant Franciscan preachers legitimized lending to the poor and triggered the foundation of Monti di Pietà — early charitable credit institutions — especially in communities hit by natural disasters, with lasting effects on social capital.</div>
    <div class="insight-links"><a href="{{ '/papers/monti-di-pieta/' | relative_url }}">Working paper →</a></div>
  </div>

  <div class="insight-card" data-theme="institutions" id="card-feudal">
    <div class="card-icon">🏛️</div>
    <div class="insight-claim">How feudal lords were monitored mattered for development.</div>
    <div class="insight-detail">In medieval Sicily, territories temporarily repossessed by the Crown suffered less rent extraction and fewer agency problems — and display persistently better local economic outcomes today.</div>
    <div class="insight-links"><a href="{{ '/papers/feudal-origins/' | relative_url }}">Working paper →</a></div>
  </div>

  <h2 class="insights-theme-heading" id="theme-politics" data-theme="politics">🗳️ Political regimes and accountability</h2>

  <div class="insight-card" data-theme="politics" id="card-demgrow">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Democracy does cause growth — modestly and mostly through indirect channels.</div>
    <div class="insight-detail">Pooling the evidence from more than 2,000 published regressions in a meta-analysis, the effect of democracy on economic growth is positive, but much of it operates indirectly, through channels such as human capital and economic freedom.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.ejpoleco.2019.101824" target="_blank" rel="noopener">European Journal of Political Economy, 2020 →</a></div>
  </div>

  <div class="insight-card" data-theme="politics" id="card-food">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Inclusive institutions improve food security.</div>
    <div class="insight-detail">Across developing countries, democratic and inclusive institutions are associated with better food security outcomes — political inclusion has very concrete material consequences.</div>
    <div class="insight-links"><a href="https://doi.org/10.1080/02255189.2017.1382335" target="_blank" rel="noopener">Canadian Journal of Development Studies, 2018 →</a></div>
  </div>

  <div class="insight-card" data-theme="politics" id="card-media">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Media competition shapes whether voters can hold politicians accountable.</div>
    <div class="insight-detail">When incumbents can "buy the silence" of news outlets, the structure of the media market — how many outlets compete and at what price they can be captured — determines how much information reaches voters and how effective elections are as an accountability device.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.ejpoleco.2020.101939" target="_blank" rel="noopener">European Journal of Political Economy, 2021 →</a></div>
  </div>

  <div class="insight-card" data-theme="politics" id="card-taste">
    <div class="card-icon">🗳️</div>
    <div class="insight-claim">Can economic liberalization buy political quiet?</div>
    <div class="insight-detail">Ongoing work studies China's Free Trade Zones to ask whether commercial reform helps an autocratic state contain social contention.</div>
    <div class="insight-links"><a href="{{ '/papers/taste-of-freedom/' | relative_url }}">Work in progress →</a></div>
  </div>

  <h2 class="insights-theme-heading" id="theme-cooperation" data-theme="cooperation">🤝 Cooperation, trust, and human–machine interaction</h2>

  <div class="insight-card" data-theme="cooperation" id="card-anchoring">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">Trust is anchored: people copy the trust they have just seen — even when it costs them.</div>
    <div class="insight-detail">In two consecutive Trust Games with reversed roles, subjects who play trustor second send more, anchoring on the amount their partner sent them: one in five copies it exactly, twice what chance would predict. Anchoring, not learning, drives the effect — and these subjects earn lower payoffs.</div>
    <div class="insight-links"><a href="https://doi.org/10.1007/s40888-026-00420-6" target="_blank" rel="noopener">Economia Politica, 2026 →</a></div>
  </div>

  <div class="insight-card" data-theme="cooperation" id="card-ultimatum">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">Playing first changes how fair you are: order of play matters in the Ultimatum Game.</div>
    <div class="insight-detail">Experiencing both roles in the Ultimatum Game, and the order in which subjects experience them, affects the consistency of their behavior — evidence that perspective-taking shapes strategic fairness.</div>
    <div class="insight-links"><a href="https://doi.org/10.1007/s40797-026-00388-z" target="_blank" rel="noopener">Italian Economic Journal, 2026 →</a></div>
  </div>

  <div class="insight-card" data-theme="cooperation" id="card-robot">
    <div class="card-icon">🤝</div>
    <div class="insight-claim">People cooperate with robots — more so when robots look and speak like humans.</div>
    <div class="insight-detail">In strategic interactions, communication and human-like features of a robotic counterpart increase human cooperation, and people attribute mental states to robots that behave consistently over time.</div>
    <div class="insight-links">
      <a href="https://doi.org/10.1016/j.socec.2023.102011" target="_blank" rel="noopener">JBEE, 2023 →</a>
      <a href="https://doi.org/10.1089/cyber.2023.0353" target="_blank" rel="noopener">Cyberpsychology, 2024 →</a>
    </div>
  </div>

  <h2 class="insights-theme-heading" id="theme-field" data-theme="field">🌍 Behavioral interventions in the field</h2>

  <div class="insight-card" data-theme="field" id="card-prison">
    <div class="card-icon">🌍</div>
    <div class="insight-claim">Prosocial preferences are not fixed: targeted programs can rebuild trust, even behind bars.</div>
    <div class="insight-detail">Lab-in-the-field experiments with inmates show that rehabilitation programs can measurably increase trust and prosocial behavior, and incarcerated participants in the GRIP program (Guiding Rage Into Power) report meaningful socio-emotional change.</div>
    <div class="insight-links">
      <a href="https://doi.org/10.1016/j.joep.2017.12.003" target="_blank" rel="noopener">JEP, 2018 →</a>
      <a href="https://doi.org/10.1002/casp.70013" target="_blank" rel="noopener">JCASP, 2025 →</a>
    </div>
  </div>

  <div class="insight-card" data-theme="field" id="card-tutoring">
    <div class="card-icon">🌍</div>
    <div class="insight-claim">Intensive tutoring can narrow the income-achievement gap.</div>
    <div class="insight-detail">Experimental evidence from high-dosage tutoring in Dutch primary schools shows that well-designed, intensive educational support significantly improves outcomes for disadvantaged pupils.</div>
    <div class="insight-links"><a href="https://doi.org/10.1016/j.econedurev.2023.102383" target="_blank" rel="noopener">Economics of Education Review, 2023 →</a></div>
  </div>

  <div class="insight-card" data-theme="field" id="card-sponsor">
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
    var THEMES = {
      institutions: { name: "Institutions", desc: "Medieval institutions and their long-run economic legacy." },
      politics:     { name: "Political regimes", desc: "Democracy, autocracy, and how citizens hold power to account." },
      cooperation:  { name: "Cooperation", desc: "Lab experiments on trust, fairness, and cooperation, with humans and robots." },
      field:        { name: "Field work", desc: "Programs that change behavior in schools, prisons, and communities." }
    };

    var STATUS = { pub: "Published", wp: "Working paper", wip: "Work in progress" };

    var PAPERS = [
      { id: "monti", theme: "institutions", status: "wp", label: ["Monti di", "Pietà"],
        title: "Crediting the Poor: The Effect of Italian Monti di Pietà on Social Capital",
        venue: "Working paper · with C. Marconi",
        finding: "Franciscan preaching legitimized lending to the poor and drove the founding of the Monti di Pietà in 15th-century Italy, especially after natural disasters, with lasting effects on social capital.",
        url: "{{ '/papers/monti-di-pieta/' | relative_url }}", ext: false },
      { id: "feudal", theme: "institutions", status: "wp", label: ["Feudal", "Sicily"],
        title: "The Feudal Origins of Economic Prosperity: Evidence from Medieval Sicily",
        venue: "Working paper (SSRN) · with L. Cherici, G. Merzoni & F. Trombetta",
        finding: "Territories temporarily repossessed by the Crown suffered less rent extraction and fewer agency problems, and are more prosperous today.",
        url: "{{ '/papers/feudal-origins/' | relative_url }}", ext: false },
      { id: "ora", theme: "institutions", status: "pub", label: ["Ora et", "Guberna"],
        title: "Ora et Guberna. The Economic Impact of the Rule of St Benedict in Medieval England",
        venue: "The Journal of Economic History, 2024 · with F. Trombetta",
        finding: "Monasteries following the Rule of St Benedict, with its emphasis on work, discipline, and careful administration, fostered better economic outcomes on the lands they managed.",
        url: "https://doi.org/10.1017/S0022050724000330", ext: true },
      { id: "cross", theme: "institutions", status: "wip", label: ["Cross &", "the Sword"],
        title: "The Cross and the Sword: Monastic Land, Jurisdictional Power, and Conflict in Medieval England",
        venue: "Work in progress · with F. Trombetta",
        finding: "After the Dissolution of the Monasteries (1536–38), conflict near former Benedictine manors fell relative to secular manors, most of all around high-value and fortified ones.",
        url: "{{ '/papers/cross-and-sword/' | relative_url }}", ext: false },

      { id: "taste", theme: "politics", status: "wip", label: ["Taste of", "Freedom?"],
        title: "A Taste of Freedom? Commercial Reform and the Containment of Contention in China's Free Trade Zones",
        venue: "Work in progress",
        finding: "Ongoing work asks whether commercial liberalization in China's Free Trade Zones helps an autocratic state contain social contention.",
        url: "{{ '/papers/taste-of-freedom/' | relative_url }}", ext: false },
      { id: "demgrow", theme: "politics", status: "pub", label: ["Democracy", "& growth"],
        title: "Does democracy cause growth? A meta-analysis (of 2000 regressions)",
        venue: "European Journal of Political Economy, 2020 · with M. Colagrossi & M. A. Maggioni",
        finding: "Pooling more than 2,000 estimates, democracy has a positive but modest effect on growth, operating mostly through indirect channels such as human capital and economic freedom.",
        url: "https://doi.org/10.1016/j.ejpoleco.2019.101824", ext: true },
      { id: "media", theme: "politics", status: "pub", label: ["Media", "capture"],
        title: "The Price of Silence: Media Competition, Capture, and Electoral Accountability",
        venue: "European Journal of Political Economy, 2021 · with F. Trombetta",
        finding: "When incumbents can buy the silence of news outlets, the structure of the media market decides how much information reaches voters, and how well elections discipline politicians.",
        url: "https://doi.org/10.1016/j.ejpoleco.2020.101939", ext: true },
      { id: "food", theme: "politics", status: "pub", label: ["Food", "security"],
        title: "Food security and democracy: do inclusive institutions matter?",
        venue: "Canadian Journal of Development Studies, 2018",
        finding: "Across developing countries, democratic and inclusive institutions go hand in hand with better food security.",
        url: "https://doi.org/10.1080/02255189.2017.1382335", ext: true },

      { id: "sponsor", theme: "field", status: "pub", label: ["Child", "sponsorship"],
        title: "International Child Sponsorship Improves School Performance: Evidence from Goma (DRC)",
        venue: "Journal of African Economies, 2022",
        finding: "Children sponsored through an international program in Goma perform significantly better at school.",
        url: "https://doi.org/10.1093/jae/ejab007", ext: true },
      { id: "tutoring", theme: "field", status: "pub", label: ["High-dosage", "tutoring"],
        title: "Closing the income-achievement gap? Experimental evidence from high-dosage tutoring in Dutch primary education",
        venue: "Economics of Education Review, 2023",
        finding: "Intensive, well-designed tutoring significantly improves the outcomes of disadvantaged pupils.",
        url: "https://doi.org/10.1016/j.econedurev.2023.102383", ext: true },
      { id: "prison", theme: "field", status: "pub", label: ["Trust", "behind bars"],
        title: "Trust behind bars: Measuring change in inmates' prosocial preferences",
        venue: "Journal of Economic Psychology, 2018 · follow-up in J. Community & Applied Social Psychology, 2025",
        finding: "Rehabilitation programs measurably raise inmates' trust and prosocial behavior; participants in the GRIP program report meaningful socio-emotional change.",
        url: "https://doi.org/10.1016/j.joep.2017.12.003", ext: true },

      { id: "anchoring", theme: "cooperation", status: "pub", label: ["Anchoring", "trust"],
        title: "Anchoring Trust. Order of Moves and Agent Behavior in Two Trust Games",
        venue: "Economia Politica, 2026 · with M. A. Maggioni",
        finding: "Subjects who play trustor second anchor on the amount their partner just sent them: one in five copies it exactly. They trust more, and earn less.",
        url: "https://doi.org/10.1007/s40888-026-00420-6", ext: true },
      { id: "ultimatum", theme: "cooperation", status: "pub", label: ["Ultimatum", "game order"],
        title: "Putting Yourself in Someone Else's Shoes. Order of Play and Consistent Behavior in the Ultimatum Game",
        venue: "Italian Economic Journal, 2026 · with M. A. Maggioni",
        finding: "Playing both roles of the Ultimatum Game, and the order in which they are played, shapes how consistent and fair people are.",
        url: "https://doi.org/10.1007/s40797-026-00388-z", ext: true },
      { id: "robot", theme: "cooperation", status: "pub", label: ["Human–robot", "cooperation"],
        title: "If it looks like a human and speaks like a human... Communication and cooperation in strategic Human-Robot Interactions",
        venue: "J. of Behavioral and Experimental Economics, 2023 · follow-up in Cyberpsychology, 2024",
        finding: "People cooperate more with robots that communicate and look human, and attribute mental states to robots that behave consistently.",
        url: "https://doi.org/10.1016/j.socec.2023.102011", ext: true }
    ];

    var CHORDS = [
      { a: "ora", b: "monti", label: "Religion & economy" },
      { a: "ora", b: "feudal", label: "Historical persistence" },
      { a: "monti", b: "feudal", label: "Medieval Italy" },
      { a: "ora", b: "cross", label: "Benedictine monasteries" },
      { a: "cross", b: "feudal", label: "Lords & jurisdiction" },
      { a: "cross", b: "taste", label: "Conflict & contention" },
      { a: "ora", b: "demgrow", label: "Institutions & growth" },
      { a: "taste", b: "demgrow", label: "Economic vs political freedom" },
      { a: "taste", b: "media", label: "Containing dissent" },
      { a: "demgrow", b: "food", label: "Inclusive institutions" },
      { a: "demgrow", b: "media", label: "Accountability" },
      { a: "monti", b: "prison", label: "Social capital & trust" },
      { a: "food", b: "sponsor", label: "Development outcomes" },
      { a: "tutoring", b: "sponsor", label: "Education" },
      { a: "prison", b: "robot", label: "Trust" },
      { a: "prison", b: "ultimatum", label: "Fairness & prosociality" },
      { a: "prison", b: "anchoring", label: "Measuring trust" },
      { a: "anchoring", b: "ultimatum", label: "Order of play" }
    ];

    var SVGNS = "http://www.w3.org/2000/svg";
    var svg = document.getElementById("rn-svg");
    var canvas = document.getElementById("rn-canvas");
    var tooltip = document.getElementById("rn-tooltip");
    var panel = document.getElementById("rn-panel");
    if (!svg || !canvas || !tooltip || !panel) return;

    var CX = 320, CY = 290, R = 188, LABEL_R = 226;
    var themeOrder = ["institutions", "politics", "field", "cooperation"];
    var sectorGap = 0.2;
    var perPaper = (2 * Math.PI - sectorGap * themeOrder.length) / PAPERS.length;

    var state = { selected: null, filter: "all" };
    var byId = {};
    var nodes = {};
    var angle = -Math.PI / 2 + sectorGap / 2;

    PAPERS.forEach(function (p) { byId[p.id] = p; });
    themeOrder.forEach(function (t) {
      PAPERS.filter(function (p) { return p.theme === t; }).forEach(function (p) {
        var a = angle + perPaper / 2;
        nodes[p.id] = {
          paper: p, angle: a,
          x: CX + R * Math.cos(a), y: CY + R * Math.sin(a),
          lx: CX + LABEL_R * Math.cos(a), ly: CY + LABEL_R * Math.sin(a)
        };
        angle += perPaper;
      });
      angle += sectorGap;
    });

    function esc(s) {
      return String(s).replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;");
    }

    function el(name, attrs, parent) {
      var e = document.createElementNS(SVGNS, name);
      for (var k in attrs) e.setAttribute(k, attrs[k]);
      (parent || svg).appendChild(e);
      return e;
    }

    function shortName(p) { return p.label.join(" "); }

    function neighbours(id) {
      return CHORDS.filter(function (c) { return c.a === id || c.b === id; }).map(function (c) {
        return { id: c.a === id ? c.b : c.a, label: c.label };
      });
    }

    /* ---------- draw ---------- */
    var hit = el("rect", { x: 0, y: 0, width: 640, height: 580, fill: "transparent" });
    hit.addEventListener("click", function () { select(null); });

    var bandEls = [];
    themeOrder.forEach(function (t) {
      var members = PAPERS.filter(function (p) { return p.theme === t; }).map(function (p) { return nodes[p.id]; });
      var a0 = members[0].angle - perPaper / 2 + 0.03;
      var a1 = members[members.length - 1].angle + perPaper / 2 - 0.03;
      var r1 = R + 14, r0 = R - 14;
      var large = (a1 - a0) > Math.PI ? 1 : 0;
      var band = el("path", {
        d: ["M", CX + r1 * Math.cos(a0), CY + r1 * Math.sin(a0),
            "A", r1, r1, 0, large, 1, CX + r1 * Math.cos(a1), CY + r1 * Math.sin(a1),
            "L", CX + r0 * Math.cos(a1), CY + r0 * Math.sin(a1),
            "A", r0, r0, 0, large, 0, CX + r0 * Math.cos(a0), CY + r0 * Math.sin(a0), "Z"].join(" "),
        "class": "rn-band t-" + t
      });
      band.dataset.theme = t;
      bandEls.push(band);
    });

    var chordEls = [];
    CHORDS.forEach(function (c) {
      var n1 = nodes[c.a], n2 = nodes[c.b];
      var same = n1.paper.theme === n2.paper.theme;
      var dAng = Math.abs(n1.angle - n2.angle);
      if (dAng > Math.PI) dAng = 2 * Math.PI - dAng;
      var pull = 1 - (dAng / Math.PI) * 0.85;
      var mx = (n1.x + n2.x) / 2, my = (n1.y + n2.y) / 2;
      var qx = CX + (mx - CX) * pull, qy = CY + (my - CY) * pull;
      var path = el("path", {
        d: "M" + n1.x + "," + n1.y + " Q" + qx + "," + qy + " " + n2.x + "," + n2.y,
        "class": "rn-chord" + (same ? " same" : "")
      });
      path.addEventListener("mousemove", function (ev) {
        showTip(ev, "<span class='tt-title'>" + esc(c.label) + "</span>" + esc(shortName(n1.paper)) + " ↔ " + esc(shortName(n2.paper)));
        path.classList.add("hot");
      });
      path.addEventListener("mouseleave", function () { hideTip(); render(); });
      path.addEventListener("click", function () { select(c.a); });
      chordEls.push({ el: path, c: c });
    });

    var t1 = el("text", { x: CX, y: CY - 6, "class": "rn-center-title" });
    t1.textContent = "My Research";
    var t2 = el("text", { x: CX, y: CY + 16, "class": "rn-center-sub" });
    t2.textContent = PAPERS.length + " STUDIES · " + CHORDS.length + " LINKS";

    var nodeEls = {};
    PAPERS.forEach(function (p) {
      var n = nodes[p.id];
      var g = el("g", {
        "class": "rn-node t-" + p.theme + (p.status === "pub" ? "" : " draft"),
        tabindex: "0",
        role: "button",
        "aria-label": p.title + " (" + STATUS[p.status] + ")"
      });
      el("circle", { cx: n.x, cy: n.y, r: 10, "class": "rn-dot" }, g);

      var anchor = "middle";
      var cosA = Math.cos(n.angle);
      if (cosA > 0.3) anchor = "start";
      else if (cosA < -0.3) anchor = "end";
      var text = el("text", { x: n.lx, y: n.ly - 4, "text-anchor": anchor, "class": "rn-label" }, g);
      p.label.forEach(function (line, i) {
        var ts = document.createElementNS(SVGNS, "tspan");
        ts.setAttribute("x", n.lx);
        ts.setAttribute("dy", i === 0 ? 0 : 16);
        ts.textContent = line;
        text.appendChild(ts);
      });

      g.addEventListener("click", function (ev) { ev.stopPropagation(); hideTip(); select(p.id); });
      g.addEventListener("keydown", function (ev) {
        if (ev.key === "Enter" || ev.key === " ") { ev.preventDefault(); select(p.id); }
      });
      g.addEventListener("mouseenter", function () { render(p.id); });
      g.addEventListener("mouseleave", function () { render(); });
      g.addEventListener("focus", function () { render(p.id); });
      g.addEventListener("blur", function () { render(); });
      nodeEls[p.id] = g;
    });

    /* ---------- state ---------- */
    function render(previewId) {
      var focus = previewId || state.selected;
      var lit = null;
      if (focus) {
        lit = {};
        lit[focus] = true;
        neighbours(focus).forEach(function (nb) { lit[nb.id] = true; });
      }
      var f = state.filter;

      PAPERS.forEach(function (p) {
        var g = nodeEls[p.id];
        var cold = lit ? !lit[p.id] : (f !== "all" && p.theme !== f);
        g.classList.toggle("cold", cold);
        g.classList.toggle("selected", p.id === state.selected);
        g.querySelector(".rn-dot").setAttribute("r", p.id === focus ? 13 : 10);
      });

      chordEls.forEach(function (ch) {
        var on, cold;
        if (focus) {
          on = ch.c.a === focus || ch.c.b === focus;
          cold = !on;
        } else {
          on = false;
          cold = f !== "all" && byId[ch.c.a].theme !== f && byId[ch.c.b].theme !== f;
        }
        ch.el.classList.toggle("hot", on);
        ch.el.classList.toggle("cold", cold);
      });

      bandEls.forEach(function (b) {
        b.classList.toggle("cold", !focus && f !== "all" && b.dataset.theme !== f);
      });
    }

    function renderPanel() {
      var p = state.selected && byId[state.selected];
      var html;
      if (!p) {
        html = "<p class='rn-eyebrow'>Explore the map</p>" +
          "<h3 class='rn-p-title'>" + PAPERS.length + " studies, one research agenda</h3>" +
          "<p class='rn-p-text'>Select a paper to read its main finding and see how it connects to the rest of my work. Or pick a theme:</p>" +
          "<ul class='rn-list'>" + themeOrder.map(function (t) {
            var count = PAPERS.filter(function (x) { return x.theme === t; }).length;
            return "<li><button type='button' class='t-" + t + (state.filter === t ? " active" : "") + "' data-filter-theme='" + t + "'>" +
              "<span class='rn-sw'></span><span>" + esc(THEMES[t].name) +
              "<span class='rn-desc'>" + esc(THEMES[t].desc) + "</span></span>" +
              "<span class='rn-count'>" + count + "</span></button></li>";
          }).join("") + "</ul>";
        panel.className = "rn-panel";
      } else {
        var nbs = neighbours(p.id);
        var draft = p.status !== "pub";
        html = "<p class='rn-eyebrow'>" + esc(THEMES[p.theme].name) +
          "<span class='rn-pill" + (draft ? " draft" : "") + "'>" + esc(STATUS[p.status]) + "</span></p>" +
          "<h3 class='rn-p-title'>" + esc(p.title) + "</h3>" +
          "<p class='rn-p-meta'>" + esc(p.venue) + "</p>" +
          "<p class='rn-p-text'>" + esc(p.finding) + "</p>" +
          "<a class='rn-p-cta' href='" + esc(p.url) + "'" + (p.ext ? " target='_blank' rel='noopener'" : "") + ">Read the paper →</a>" +
          "<p class='rn-p-sub'>Connected to</p>" +
          "<ul class='rn-list'>" + nbs.map(function (nb) {
            var q = byId[nb.id];
            return "<li><button type='button' class='t-" + q.theme + "' data-select='" + q.id + "'>" +
              "<span class='rn-sw'></span><span>" + esc(shortName(q)) +
              "<span class='rn-desc'>" + esc(nb.label) + "</span></span></button></li>";
          }).join("") + "</ul>" +
          "<button type='button' class='rn-back'>← All themes</button>";
        panel.className = "rn-panel t-" + p.theme;
      }
      panel.innerHTML = html;
    }

    function select(id) {
      state.selected = id;
      if (id && state.filter !== "all" && byId[id].theme !== state.filter) setFilter("all", true);
      render();
      renderPanel();
    }

    panel.addEventListener("click", function (ev) {
      var btn = ev.target.closest("button");
      if (!btn) return;
      if (btn.dataset.select) {
        select(btn.dataset.select);
        var target = nodeEls[btn.dataset.select];
        if (target) target.focus({ preventScroll: true });
      } else if (btn.dataset.filterTheme) {
        setFilter(state.filter === btn.dataset.filterTheme ? "all" : btn.dataset.filterTheme);
      } else if (btn.classList.contains("rn-back")) {
        select(null);
      }
    });

    document.addEventListener("keydown", function (ev) {
      if (ev.key === "Escape" && state.selected) select(null);
    });

    function showTip(ev, html) {
      var rect = canvas.getBoundingClientRect();
      tooltip.innerHTML = html;
      tooltip.style.left = (ev.clientX - rect.left) + "px";
      tooltip.style.top = (ev.clientY - rect.top) + "px";
      tooltip.classList.add("visible");
    }

    function hideTip() { tooltip.classList.remove("visible"); }

    /* ---------- theme filters (cards + map) ---------- */
    var chips = document.querySelectorAll(".filter-chip");
    var cards = document.querySelectorAll(".insight-card");
    var headings = document.querySelectorAll(".insights-theme-heading");

    function setFilter(theme, silent) {
      state.filter = theme;
      chips.forEach(function (c) { c.classList.toggle("active", c.dataset.filter === theme); });
      cards.forEach(function (card) {
        card.style.display = (theme === "all" || card.dataset.theme === theme) ? "" : "none";
      });
      headings.forEach(function (h) {
        h.style.display = (theme === "all" || h.dataset.theme === theme) ? "" : "none";
      });
      if (!silent) {
        state.selected = null;
        render();
        renderPanel();
      }
    }

    chips.forEach(function (chip) {
      chip.addEventListener("click", function () { setFilter(chip.dataset.filter); });
    });

    render();
    renderPanel();
  })();
</script>
