---
layout: default
title: Projects
permalink: /projects
---

<section class="projects">
  <h2>{{ page.title }}</h2>
  <input type="text" id="search-input" placeholder="Search…" autocomplete="off">

  <div id="projects-list">
  {% assign projectsByYear = site.projects | sort: "date" | reverse | group_by_exp: "project", "project.date | date: '%Y'" %}
  {% for year in projectsByYear %}
  <h3 class="year" data-year>{{ year.name }}</h3>
  <ul data-year-list>
    {% for project in year.items %}
    <li><a href="{{ site.baseurl }}{{ project.url }}">{{ project.title }}</a><time datetime="{{ project.date | date_to_xmlschema }}">{{ project.date | date: "%b %-d" }}</time></li>
    {% endfor %}
  </ul>
  {% endfor %}
  </div>

  <div id="pagination"></div>
</section>

<script>
(function() {
  var ITEMS_PER_PAGE = 10;
  var currentPage = 1;
  var projects = null;
  var filteredItems = null;

  var input = document.getElementById('search-input');
  var allItems = Array.from(document.querySelectorAll('#projects-list li'));

  fetch('/projects.json')
    .then(function(r) { return r.json(); })
    .then(function(data) { projects = data; });

  function updateYearHeaders() {
    document.querySelectorAll('[data-year]').forEach(function(yearEl) {
      var ul = yearEl.nextElementSibling;
      var anyVisible = Array.from(ul.querySelectorAll('li')).some(function(li) {
        return li.style.display !== 'none';
      });
      yearEl.style.display = anyVisible ? '' : 'none';
      ul.style.display = anyVisible ? '' : 'none';
    });
  }

  function renderControls(items) {
    var total = Math.ceil(items.length / ITEMS_PER_PAGE);
    var container = document.getElementById('pagination');
    container.innerHTML = '';
    if (total <= 1) return;

    if (currentPage > 1) {
      var prev = document.createElement('button');
      prev.textContent = '←';
      prev.onclick = function() { renderPage(items, currentPage - 1); };
      container.appendChild(prev);
    }

    var info = document.createElement('span');
    info.textContent = currentPage + ' / ' + total;
    container.appendChild(info);

    if (currentPage < total) {
      var next = document.createElement('button');
      next.textContent = '→';
      next.onclick = function() { renderPage(items, currentPage + 1); };
      container.appendChild(next);
    }
  }

  function renderPage(items, page) {
    currentPage = page;
    var start = (page - 1) * ITEMS_PER_PAGE;
    var end = start + ITEMS_PER_PAGE;
    allItems.forEach(function(li) { li.style.display = 'none'; });
    items.slice(start, end).forEach(function(li) { li.style.display = ''; });
    updateYearHeaders();
    renderControls(items);
  }

  function applyFilter(q) {
    currentPage = 1;
    if (!q) {
      filteredItems = allItems;
    } else {
      filteredItems = allItems.filter(function(li) {
        var href = li.querySelector('a').getAttribute('href');
        return projects && projects.some(function(p) {
          return p.url === href &&
            (p.title.toLowerCase().includes(q) ||
             p.tags.toLowerCase().includes(q) ||
             p.content.toLowerCase().includes(q));
        });
      });
    }
    renderPage(filteredItems, 1);
  }

  input.addEventListener('input', function() {
    applyFilter(this.value.trim().toLowerCase());
  });

  filteredItems = allItems;
  renderPage(filteredItems, 1);
}());
</script>
