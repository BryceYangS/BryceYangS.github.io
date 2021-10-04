---
layout: search
title: Search
menu: true
order: 10
---

<div class="container">
  <div class="row">
    <div class="col-12">
      <div id="search-bar">
        <i class="fa fa-search" aria-hidden="true"></i>
        <input id="search-input" type="text" placeholder="Search..." />
      </div>
      <ul id="results-container"></ul>
    </div>
  </div>
</div>

<!-- Script pointing to jekyll-search.js -->
<script src="/assets/js/jekyll-search.js" type="text/javascript"></script>

<script type="text/javascript">
  SimpleJekyllSearch({
    searchInput: document.getElementById("search-input"),
    resultsContainer: document.getElementById("results-container"),
    json: "/search.json",
    searchResultTemplate:
      '<li><a href="{url}" title="{desc}">{title}</a></li>',
    noResultsText: "No results found",
    limit: 10000,
    fuzzy: false,
    exclude: ["Welcome"],
  });
</script>

<style>
  #search-bar {
    margin: 32px auto;
    border: 1px solid #ccc;
    border-radius: 20px;
    padding: 0 20px;
  }

  #search-bar #search-input {
    width: calc(100% - 30px);
    border: none;
    line-height: 44px;
    outline: none;
    border-style: none;
  }
</style>