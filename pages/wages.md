---
layout: page
title: Wages
permalink: /wages/
---

<div class="wages-wrap">
  <table id="wages-table"></table>
</div>

<script>
(function () {
  var SHEET_ID = '1xnhPtCq2zggGPrTUrNewlYY8JbiMfsZK9CXn8QuSppc';
  var CSV_URL  = 'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/export?format=csv';

  var table  = document.getElementById('wages-table');

  fetch(CSV_URL)
    .then(function (res) {
      if (!res.ok) throw new Error('HTTP ' + res.status);
      return res.text();
    })
    .then(function (csv) {
      var rows = parseCSV(csv);
      if (rows.length === 0) throw new Error('Empty response');

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

    })
    .catch(function (err) {
      table.outerHTML = '<div class="wages-error">'
        + '<strong>ERROR:</strong> Could not load wages data.<br>'
        + '</div>';
    });

  function parseCSV(text) {
    return text.trim().split('\n').map(function (line) {
      return line.split(',').map(function (cell) { return cell.trim(); });
    });
  }
})();
</script>
