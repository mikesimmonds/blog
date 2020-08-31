---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: search
---


<!-- Html Elements for Search -->
<div id="search-container">
  <div class="search-input-wrapper">
    <input type="text" id="search-input" placeholder="search...">
  </div>
  <div id="results-container"></div>
</div>
  
  <!-- Script pointing to search-script.js -->
  <script src="./assets/js/search-script.js" type="text/javascript"></script>
  
  <!-- Configuration -->
  <script>
  SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('results-container'),
    json: './search.json',
    searchResultTemplate: `
    <a href="{url}" class="result-link-container" >
    <span class="post-link">{title}</span>
    <span class="post-meta">{date}</span>
    <span class="post-meta" style="display: block">{tags}</span>
    </a>`,
    // searchResultTemplate: '<li><a href="{{ site.url }}{url}">{title}</a></li>',
    noResultsText: ("No result found!")
  })
  </script>
  <!-- End of search -->
