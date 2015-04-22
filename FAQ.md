**How does Greasefire impact my privacy?  Do you track what sites I visit?**

Greasefire does **not** record or send any data to a server while you browse the web.  The process that matches scripts to the sites you visit is done by querying a local index of all the scripts from userscripts.org (see the files include.dat, exclude.dat, and scripts.db in the extension directory).  Updated versions of these index files are downloaded periodically to ensure you see the latest scripts available.

**Does Greasefire search all scripts on userscripts.org?**

No -- The goal of Greasefire is to match scripts to specific web sites, therefore scripts that are designed to work on all sites are explicitly excluded from the index.  I maintain a list of @include lines that I feel are too general, and these scripts are filtered out when the indexes are generated.  See the `badIncludesList` in [Generate.java](http://code.google.com/p/greasefire/source/browse/java/greasefire-scraper/src/com/skrul/greasefire/Generate.java).

**How are the scripts ranked?**

There are currently three signals used to rank the scripts (see the `rankMatches` method in [gfGreasefireService.js](http://code.google.com/p/greasefire/source/browse/src/gfGreasefireService.js)):

  * Number of installs (`installs` in the source) - This is simply the number of times the script has been installed according to userscripts.org.
  * Freshness (`updated` in the source) - The last time the script was updated.
  * Specificity (`matches` in the source) - How precisely the script's @include declarations match the URL of the web page you have visited.  For example, if you are on `http://reader.google.com`, a script with `@include http://reader.google.com/*` will have a higher specificity than a script with `@include http://*.google.com/*`.

A script's ranking is made up of these three signals, weighted in the following proportions:
  * Number of installs - 25%
  * Freshness - 50%
  * Specificity - 25%

Note that these are the only useful signals Greasefire currently has access to -- the index generator simply scrapes scripts from [http://userscripts.org/scripts](http://userscripts.org/scripts) and these are the only number that are available.  I have been working with the owner of the site to see if Greasefire can get access to more data, such as script reviews and fans.

**Why is the extension download so big?  It is over 2MB!**

Greasefire includes several index files that are used to match scripts with the web sites you visit.  The indexes are included with the extension download so Greasefire can begin to work without having to download an index update.  There are currently over 17,000 user scripts, so these indexes can be quite large.  The indexes that are distributed with the extension are refreshed before each new version is released.