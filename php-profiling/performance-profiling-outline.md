3:15pm - 4:00pm
45 minutes

Performance Troubleshooting and Pitfalls

What is Application Performance?

- The speed at which Drupal can return a non-cached HTTP response
- That includes the initial page load
- Processing of Forms and AJAX requests

When You Should Profile

- Before you deploy changes
- Before you launch your site
- When notice it's slow
- When you get an email telling you its slow

- Always if Possible


Profile your Application

 - XHProf (on-demand profiling, totally free)
 - Tideways (free extension for on-demand profiling, pricing for historical tracking)
 - BlackFire (on-demand profiling)
 - New Relic (Historical, comprehensive profiling on every page)

Demonstrate Profiling
 - XHProf

Demonstrate Profiling
 - New Relic

Common Pitfalls

Use of $_SESSION unnecessarily
  - $_SESSION['header_images'] = ['foo.jpg', 'bar.jpg']
  - Sessions disable the most critical cache, the page cache.

Limit use of $_SESSION if possible
  - Don't allow logins, use external services for polling, mailing lists.

More inoccuous things that can cause sessions:
  - drupal_set_message()
  - Any form, including the search form and polls
  - ??

Web Services HTTP Requests
  - Mailing lists (MailChimp)
  - Search Services
  - OEmbed (Twitter/Facebook/Instagram/YouTube)
  - 

Excessive Logging
  - Happens most frequently with dblog
  - DON'T just switch to syslog! Fix the problems in your code to decrease logging.

Debugging Modes Turned on
  - Cache rebuild
  - Twig debug

Not Using PHP Op-Code Cache
  - PHP 7 pretty much required, on by default. But double-check.

Long-running MySQL queries
  - Easy to have with Views that use lots of fields
  - Consider de-normalization or simplifying the view

Aggregating CSS and JS files
  - Doing on-demand slows the rendering of the page.
  - Module like Advanced Agg can reduce duplicate aggregation and pre-generate them.

Refactoring Difficult or Slow?
  - Cover up the problem!
  - "DevOps that problem" away.

Varnish
  - Ultimate tool in covering up back-end problems
  - Specialized cookie handling code to ignore sessions or other cookies
  - However, not a solution for long-term problems like PHP code performance

Other "Good ideas"
  - Use PHP-FPM instead of Apache's mod_php. This keeps memory for PHP separate from Apache.
  - Use Imagemagick instead of PHP's GD. This keeps image manipulation from being limited by PHP and frees up memory after usage.


Back-end Caching Tools
  - Redis, Memcache?
  - Not as necessary as it had been previously. And if network is involved, better off sticking with MySQL for caching in most cases.
  - Can be benificial on single-server setups.
  - Can be benificial to reduce load on MySQL, but that's about it.
