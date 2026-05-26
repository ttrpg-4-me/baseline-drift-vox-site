---
layout: page
title: Wages
permalink: /wages/
---

<p>Are you getting paid enough?</p>

<div class="wages-wrap">
  <table id="wages-table"></table>
</div>

<div id="wages-summary"></div>

<script>
(function () {
  var SHEET_ID = '1xnhPtCq2zggGPrTUrNewlYY8JbiMfsZK9CXn8QuSppc';
  var CSV_URL  = 'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/export?format=csv';

  var table   = document.getElementById('wages-table');
  var summary = document.getElementById('wages-summary');

  fetch(CSV_URL)
    .then(function (res) {
      if (!res.ok) throw new Error('HTTP ' + res.status);
      return res.text();
    })
    .then(function (csv) {
      var rows = parseCSV(csv);
      if (rows.length === 0) throw new Error('Empty response');

      buildTable(rows);
      buildSummary(rows);
    })
    .catch(function (err) {
      table.outerHTML = '<div class="wages-error"><strong>ERROR:</strong> Could not load wages data.</div>';
    });

  function buildTable(rows) {
    var thead = document.createElement('thead');
    var headerRow = document.createElement('tr');
    rows[0].forEach(function (cell) {
      var th = document.createElement('th');
      th.textContent = cell;
      headerRow.appendChild(th);
    });
    thead.appendChild(headerRow);
    table.appendChild(thead);

    var tbody = document.createElement('tbody');
    for (var i = 1; i < rows.length; i++) {
      var tr = document.createElement('tr');
      rows[i].forEach(function (cell) {
        var td = document.createElement('td');
        td.textContent = cell;
        tr.appendChild(td);
      });
      tbody.appendChild(tr);
    }
    table.appendChild(tbody);
  }

  function buildSummary(rows) {
    var header     = rows[0].map(function (h) { return h.toLowerCase(); });
    var factionCol = header.findIndex(function (h) { return h.includes('faction'); });
    var roleCol    = header.findIndex(function (h) { return h.includes('role'); });
    var wageCol    = header.findIndex(function (h) { return h.includes('wage') || h.includes('pay'); });

    if (factionCol < 0 || roleCol < 0 || wageCol < 0) return;

    var byRole = {};
    for (var i = 1; i < rows.length; i++) {
      var role    = rows[i][roleCol];
      var faction = rows[i][factionCol];
      var wage    = parseFloat(rows[i][wageCol]);
      if (!role || isNaN(wage)) continue;
      if (!byRole[role]) byRole[role] = [];
      byRole[role].push({ faction: faction, wage: wage });
    }

    var html = '<h2>Pay Statistics</h2>';
    Object.keys(byRole).forEach(function (role) {
      var entries = byRole[role];
      var total = 0, minW = Infinity, maxW = -Infinity;
      entries.forEach(function (e) {
        total += e.wage;
        if (e.wage < minW) minW = e.wage;
        if (e.wage > maxW) maxW = e.wage;
      });
      var avg     = Math.round(total / entries.length);
      var lowest  = entries.filter(function (e) { return e.wage === minW; });
      var highest = entries.filter(function (e) { return e.wage === maxW; });
      var fmtHigh = function (e) { return '<span class="wage-high">' + e.faction + '</span> (' + e.wage + ' CR/day)'; };
      var fmtLow  = function (e) { return '<span class="wage-low">'  + e.faction + '</span> (' + e.wage + ' CR/day)'; };

      html += '<h3>' + role + '</h3><ul>';
      html += '<li><strong>Highest:</strong> ' + highest.map(fmtHigh).join(', ') + '</li>';
      html += '<li><strong>Lowest:</strong> '  + lowest.map(fmtLow).join(', ')   + '</li>';
      html += '<li><strong>Average:</strong> ' + avg + ' CR/day</li>';
      html += '</ul>';
    });

    summary.innerHTML = html;
  }

  function parseCSV(text) {
    return text.trim().split('\n').map(function (line) {
      return line.split(',').map(function (cell) { return cell.trim(); });
    });
  }
})();
</script>
