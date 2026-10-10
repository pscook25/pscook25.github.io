---
title: Recipes
---

<p><a href="/">&larr; Back to CookiesWiki</a></p>

{%- assign recipes = site.pages | where_exp: "p", "p.path contains 'recipes/'" | where_exp: "p", "p.name != 'index.md'" | sort: "title" -%}

<p><input type="search" id="recipe-filter" placeholder="Search {{ recipes.size }} recipes by name or tag (e.g. chicken, breakfast, high-protein)" style="width:100%;padding:10px;font-size:1rem;box-sizing:border-box;"></p>

<table id="recipe-table" style="width:100%;">
<thead><tr><th>Recipe</th><th>kcal</th><th>Protein (g)</th><th>Carbs (g)</th><th>Fat (g)</th><th>Time</th><th>Tags</th></tr></thead>
<tbody>
{%- for r in recipes %}
<tr><td><a href="{{ r.url | relative_url }}">{{ r.title }}</a></td><td>{{ r.calories }}</td><td>{{ r.protein_g }}</td><td>{{ r.carbs_g }}</td><td>{{ r.fat_g }}</td><td>{{ r.total_time }}</td><td>{{ r.tags | join: ", " }}</td></tr>
{%- endfor %}
</tbody>
</table>

<p><small>Macros are per serve. Click a column heading to sort.</small></p>

<script>
(function () {
  var input = document.getElementById('recipe-filter');
  var table = document.getElementById('recipe-table');
  var rows = table.tBodies[0].rows;
  input.addEventListener('input', function () {
    var q = input.value.toLowerCase();
    for (var i = 0; i < rows.length; i++) {
      rows[i].style.display = rows[i].textContent.toLowerCase().indexOf(q) > -1 ? '' : 'none';
    }
  });
  var heads = table.tHead.rows[0].cells;
  Array.prototype.forEach.call(heads, function (th, col) {
    th.style.cursor = 'pointer';
    var asc = true;
    th.addEventListener('click', function () {
      var arr = Array.prototype.slice.call(rows);
      arr.sort(function (a, b) {
        var x = a.cells[col].textContent.trim(), y = b.cells[col].textContent.trim();
        var nx = parseFloat(x), ny = parseFloat(y);
        var r = (!isNaN(nx) && !isNaN(ny)) ? nx - ny : x.localeCompare(y);
        return asc ? r : -r;
      });
      asc = !asc;
      arr.forEach(function (row) { table.tBodies[0].appendChild(row); });
    });
  });
})();
</script>
