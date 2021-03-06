# Web Checklist

- Does it work on mobile?
- Google Analytics
- Social Sharing / Open Graph
  - specific values for important/interesting pages
- Submitted to Google/Bing/Yandex (for duckduckgo)?
  - sitemap
- HTTPS by default
  - We usually use letsencrypt
  - For server-side apps, we use letsencrypt dokku plugin if possible if it's running on dokku, otherwise via certbot
    - This should be set up for automated renewal
  - HTTP should redirect to HTTPS
- Do colours/contrasts look ok on cheap old devices?
  - Looking good on a top-end mac or iphone isn't a sufficient test.
- Does www. work?
  - Even if it redirects to non-www, it needs to work because people will add it even when you ask them not to!
- global javascript error logging
  - e.g. `window.addEventListener('error', trackJavaScriptError, false);`
- AJAX error handlers
  - This is so important
  - This can be as simple as adding `.fail(function( jqXHR, textStatus, errorThrown ) { throw textStatus })` to your jQuery ajax request.
- Uptime monitor (we usualy use uptimedoctor.com)
  - 5-minute interval is generally fine - 1-minute interval is useful for sites where a couple of minutes are more serious
  - Use the HTTPS URL for the site
  - Expand Advanced Options menu
    - Add a couple of required keywords. Ideally words that aren't likely to be shown on error pages and aren't likely to be modified if the page is managed via CMS - this check helps if something breaks but doesn't result in an HTTP error code
    - Add certificate validation so we get notified quickly if a certificate expires
    - This is mainly for server-side apps. For really important static sites, this is still useful for detecting things like botched deploys or misconfigured DNS
