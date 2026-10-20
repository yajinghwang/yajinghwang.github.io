---
layout: page
title: research
permalink: /research/
description: Peer-Reviewed Journals, Conference Proceedings, and Work in Progress
nav: true
nav_order: 2
---


<!-- Research page -->

{% include bib_search.liquid %}

<style>
.publications {
  margin-top: 1rem;
}

.pub-tabs {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
}

.pub-tab {
  appearance: none;
  background: transparent;
  border: 1px solid var(--muted-color, #ddd);
  padding: 0.5rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  color: var(--text-color, #222);
}

.pub-tab:focus-visible {
  outline: 3px solid rgba(50, 115, 220, 0.25);
  outline-offset: 2px;
}

.pub-tab.active {
  background: var(--global-theme-color);
  color: white;
  border-color: var(--global-theme-color);
}

.pub-panels {
  margin-top: 0.5rem;
}

.pub-panel[hidden] {
  display: none !important;
}

@media (max-width: 520px) {
  .pub-tabs {
    gap: 0.25rem;
  }

  .pub-tab {
    padding: 0.4rem 0.5rem;
    font-size: 0.95rem;
  }
}
</style>

<div class="publications">

  <div class="pub-tabs" role="tablist" aria-label="Research categories">

    <button type="button"
      class="pub-tab active"
      id="tab-all"
      data-target="#panel-all"
      role="tab"
      aria-selected="true"
      aria-controls="panel-all">
      All
    </button>

    <button type="button"
      class="pub-tab"
      id="tab-journals"
      data-target="#panel-journals"
      role="tab"
      aria-selected="false"
      aria-controls="panel-journals"
      tabindex="-1">
      Journals
    </button>

    <button type="button"
      class="pub-tab"
      id="tab-conference"
      data-target="#panel-conference"
      role="tab"
      aria-selected="false"
      aria-controls="panel-conference"
      tabindex="-1">
      Conference Proceedings
    </button>

    <button type="button"
      class="pub-tab"
      id="tab-wip"
      data-target="#panel-wip"
      role="tab"
      aria-selected="false"
      aria-controls="panel-wip"
      tabindex="-1">
      WiP
    </button>

  </div>

  <div class="pub-panels">

    <!-- All research outputs -->
    <section id="panel-all"
      class="pub-panel"
      role="tabpanel"
      aria-labelledby="tab-all">
      {% bibliography %}
    </section>

    <!-- Journal articles -->
    <section id="panel-journals"
      class="pub-panel"
      role="tabpanel"
      aria-labelledby="tab-journals"
      hidden>
      {% bibliography --prefix="journals" -q @*[keywords=J] %}
    </section>

    <!-- Conference proceedings -->
    <section id="panel-conference"
      class="pub-panel"
      role="tabpanel"
      aria-labelledby="tab-conference"
      hidden>
      {% bibliography --prefix="conference" -q @*[keywords=C] %}
    </section>

    <!-- Work in progress -->
    <section id="panel-wip"
      class="pub-panel"
      role="tabpanel"
      aria-labelledby="tab-wip"
      hidden>
      {% bibliography --prefix="wip" -q @*[keywords=WIP] %}
    </section>

  </div>
</div>

<script>
(function () {
  document.querySelectorAll('.publications').forEach((container) => {
    const tabs = Array.from(container.querySelectorAll('.pub-tab'));
    const panels = Array.from(container.querySelectorAll('.pub-panel'));

    function activate(tab, moveFocus = false) {
      const targetId = tab.getAttribute('aria-controls');

      tabs.forEach((item) => {
        const active = item === tab;
        item.classList.toggle('active', active);
        item.setAttribute('aria-selected', String(active));
        item.tabIndex = active ? 0 : -1;
      });

      panels.forEach((panel) => {
        panel.hidden = panel.id !== targetId;
      });

      if (moveFocus) {
        tab.focus();
      }
    }

    tabs.forEach((tab, index) => {
      tab.addEventListener('click', () => activate(tab));

      tab.addEventListener('keydown', (event) => {
        let nextIndex;

        if (event.key === 'ArrowRight' ||
            event.key === 'ArrowDown') {
          nextIndex = (index + 1) % tabs.length;
        } else if (event.key === 'ArrowLeft' ||
                   event.key === 'ArrowUp') {
          nextIndex = (index - 1 + tabs.length) % tabs.length;
        } else if (event.key === 'Home') {
          nextIndex = 0;
        } else if (event.key === 'End') {
          nextIndex = tabs.length - 1;
        } else {
          return;
        }

        event.preventDefault();
        activate(tabs[nextIndex], true);
      });
    });

    activate(tabs[0]);
  });
})();
</script>