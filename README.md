# page-renderer
Updates the history about the page rendered without loading the page.

## Introduction
This is similar to turbolinks in rails gem, helps in showing the content of the page without reloading the page.

## How it works?
Handles the click event of all anchor tag, then call the history.pushState method to store the tracking of pages navigated. In click event handler method the anchor tag href will be used to fetch the page via $.ajax method.

## Coder view
    jQuery(document).ready(function($) {
        $(".data-link").click(function() {
          var href = $(this).attr('href');
          routeTo(href);
          return false;
        });
        
        onpopstate = function(event) {
          showPage(event.state);
        }
        
        function routeTo(href) {
          showPage(href);
          history.pushState(href, document.title + ' - ' + href, href);
        }
        
        function showPage(href) {
          if (href == null || !href)
            href = 'dashboard.html';
          $.ajax({
            url: getBaseURL() + href,
            success: function(html) {
              $("#workspace").html(html);
            }
          });
        }
        
        function getBaseURL() {
          return location.protocol + "//" + location.hostname +
              (location.port && ":" + location.port);
        }
        
        routeTo(null);
    });
