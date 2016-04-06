 - Test embedded from the start
 - Test on mobile from the start
 - Test over HTTPS from the start
 - Fixed width is bad.
  - Anything wider than 300px will badly affect mobile sites on small devices.
  - If you need to compromise and use fixed width - rather make it work on mobile (300px wide) and weirdly narrow but usable on large devices.
 - Put a space in empty tags e.g. `<iframe ...> </iframe` or `<script src="..."> </script`
  - Some CMSs rewrite empty tags e.g. as `<iframe ... />` which browsers don't accept properly.
 