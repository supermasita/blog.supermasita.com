---
layout: post
title:  "JQuery - Get response from another service"
date:   2019-02-01 07:04:16 -0300
tags: ajax javascript jquery computers
---
The bread and butter of most websites today.

Super basic - we get the library, create a button that will call the "[JQuery.get()](https://api.jquery.com/jQuery.get/)" function and update a DIV the results. See comments in the following code example: 
```html
<!-- Get JQuery -->
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script>
$(document).ready(function(){
  // Assign the click action to button with ID "get_country_button"
  $("#get_countrybutton").click(function(){
    // On click get the URL
    $.get("https://geoip.supermasita.com/api/v1.0/ip/", function(data, status){
      // Add the result to the DIV name "result"
      // Data should be a JSON with '{"registered_contry": {"names": {"en": "XXXX"}}}'
      // No error handling in this example
      $('#result').html(data.registered_country.names.en);
    });
  });
});
</script>
<br>
<button id="get_country_button" style="background-color: #000; color: #fff;">Click to get your country</button>
<br>
<h1><div id="result" style="background-color: #000; color: #fff;"></div></h1>
<br>
```

Remember that in order to directly request from another domain, it must have correct [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) header configuration.

That's it!

### Keep reading
* <https://api.jquery.com/jQuery.get/>
* <https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS> 

