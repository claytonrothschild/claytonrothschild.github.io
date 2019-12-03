---
title: 'Create a local, static archive of Wordpress'
date: 2015-05-27
permalink: /posts/2012/08/blog-post-1/
tags:
  - cool posts
  - category1
  - category2
---
[![Creating a Local Archive of WordPress Installations](https://web.archive.org/web/20170709033425im_/http://www.specificgeneralist.com/wp-content/uploads/2015/05/gnu-e14327646148311.png)](https://web.archive.org/web/20170709033425/http://www.specificgeneralist.com/creating-a-local-archive-of-wordpress-installations/)

In college, I spent many evenings working on personal projects. These included “The Bear Trade,” a craigslist alternative for Baylor students; “NaturalStudyAids.com” which dropshipped natural ADHD supplements and was my first exposure to split-testing; “onlyatwalmart.net,” a site that automatically ripped content off of PeopleofWalmart, republished it, and made money off of ad clicks and good SEO; and various other personal blogs, among other sites. Trust me, there were a lot of projects. Ask me about “Sperrysocks.com” some day.

Anyway, after a number of failed projects and the realization that a lot of my information was still public on the internet, I decided it was time to purge my webserver. Doing so would reduce the number of exploits exposed from outdated software and allow me to tailor my personal web footprint and therefore my brand. Before deleting, I wanted to make a local replica of my websites, a number of which ran on WordPress.

WordPress is tricky, of course, as pages are built dynamically on the server side before being rendered in the DOM for the user. Thus, the only solution to have a WordPress installation available to view locally would be to install a server environment ([MAMP](https://www.mamp.info/en/) or [WAMP](http://www.wampserver.com/en/)), copy your PHP files, DB, and scripts to it, and then deal with the hassle of renaming the WordPress installation paths (Note: A serialized find and replace script on your SQL database makes this part a lot easier). Not only that, you would have to spin up the server instance anytime you wanted to view your sites, and worry about keeping your software up-to-date.

Instead of going through this arduous process, I opted to use a mirroring utility. Such a utility starts at a root URL, and recursively mirrors each hyperlink as a static local HTML file. The benefits are obvious of course: viewing your local copy of your WordPress site later is instant, and requires no server side compiling. The downside is space is increased, as every potential dynamic page must be rendered and saved. Then again, [storage cost hasn’t been the controlling variable in decision-making for years](http://www.mkomo.com/cost-per-gigabyte-update).

[Wget](http://www.gnu.org/software/wget/) is the obvious choice for recursive web downloads. This utility is part of the GNU project, is linux based, and can traverse over FTP or HTTP. After downloading your files, all links are changed to their relative paths on your local machine. Thus, clicking around the website never makes a remote HTTP request, and all resources remain local as well. I tried WinHTTrak, but could not get the utility to change resource paths from remote to relative/local; otherwise this would have been the utility of choice for someone unfamiliar or uncomfortable with a command line interface. I eventually used WinWGet, a GUI’d windows port of the utility.

[Wget is pretty straightforward to use](Wget is pretty straightforward to use), but here are the hic-ups I ran into:

Conversion of links to relative paths (-k paramater) is the final step of the wget job
------
If your job fails or is cancelled for any reason, the URIs within a page remain pointed to their remote locations. Resources, however, appeared to be rendered using their new local, relative paths (if I remember correctly). If you use WinWGet, as I did, a failed job will sometimes show as complete. So don’t be quick to keep the local mirror and delete the remote! Consult the log no matter what to ensure -k actually performed. The log will tell you how many files were “converted.”

404s Lead to Failed jobs
------
If the utility encounters too many 404 URLs, it will stop running without any indication of success or failure. A 404 typically, though not always, will cause the job to stop and fail. Once again, WinWGet does not make this clear. A failed job due to 404s will still show the Green checkmark next to the job name, misleading you to believe the job was successful.

**Personal note: This post was written in 2015. I imagine there are great plugins that exist for Wordpress to "static"-fy your site for offline use.**