---
layout: post
title: Learning GitHub.io, Jekyll, and poole
---

This is somewhat of a blog howto for [github.io](http://github.io) pages and [Jekyll](http://jekyllrb.com/) using a [theme](http://lanyon.getpoole.com/). I'll start with the tl;dr and then break it down to the what and why. I spent way too much time looking into all the capabilities of Jekyll, but here's what easy mode looks like.

##TL;DR
* Fork [poole/lanyon](https://github.com/poole/lanyon)
* Rename fork to `<username>.github.io`
* At this point `http://<username>.github.io` should look shiny with example content
* Customize and test locally
{% highlight bash %}
#Tested from OSX 10.10.1 with only default ruby gems
#Clone repo
git clone https://github.com/<username>/<username>.github.io.git
#Install bundler 
cd <username>.github.io
sudo gem install bundler
#Generate gh-pages Gemfile
printf "source 'https://rubygems.org'\ngem 'github-pages'" >> Gemfile
#Sanity check
cat Gemfile
#Install all the gems (This takes about 5 minutes)
bundle install
#Sanity check (browse to http://localhost:4000)
bundle exec jekyll serve
#Update repo
git add --all
git commit
git push
{% endhighlight %} 

<p align="center"><img src="/meme/wat.jpg"/></p>
Ok, so this adventure started when I was reading [Kura](https://twitter.com/kuramanga)'s [blog](https://kura.io/2014/12/27/santas-tor-relay-changes/) about [Tor](https://www.torproject.org/) which led to some [chuckles](https://twitter.com/narkster/status/550920992860938240) on Twitter. As I was wondering around Kura's GitHub [repos](https://github.com/kura?tab=repositories), I stumbled across [kura.io](https://github.com/kura/kura.io). Now, Kura is not doing anything close to what we are going to talk about, at least not from what I understood at the time, but the *.io* made my brain curious about why I have been seeing so much usage of the *.io* [TLD](http://en.wikipedia.org/wiki/Top-level_domain) specifically in GitHub. Searching for `github io` takes you to [GitHub Pages](https://pages.github.com/). At this stage I was very curious how GitHub was hosting user and project pages at their scale.

I followed the steps and had a *"Hello World"* page in no time. What I didn't quite grasp was the usage of [Markdown](http://daringfireball.net/projects/markdown/) to generate the webpages, but I really liked the idea of never using HTML. What webserver was GitHub using? While trying to understand this, I realized that the "web server" that GitHub is using isn't a webserver at all, but a static web site generator named [Jekyll](http://jekyllrb.com/). There is no dynamic content, and in most cases there doesn't need to be dynamic content unless your website is an application of some kind. The content of a blog doesn't really change based on who is reading it. This was my epiphany moment about "static web sites". Keep in mind that I deal mostly in distributed systems, and web development is not an area I stay for too long. Anyway, I'm late to the party and wanted to understand the real limitations of a static web site.

With the goal of starting a blog of all the weird tech adventures I go on, I wanted to see how much I could accomplish with Jekyll. This first realization was that *my content* needs to be static, and anything dynamic could be loaded from *other websites*. This is when I discovered what most bloggers have been using for quite some time to solve the *commenting* problem: [Disqus](https://disqus.com/). Once I found this, I made it my goal to start a simple and clean blog with hopefully useful anecdotes.

##Selecting a theme
Knowing that I am not a web designer by any definition, I started looking for a theme that was compatible with Jekyll which lead me to [Joshua Lande's blog](http://joshualande.com/jekyll-github-pages-poole/) about GitHub.io, Jekyll, and [poole](http://getpoole.com). It's a great article that covers almost everything. Before we go on, another great theme that I was looking into was [Gayan Virajith's Harmony](https://github.com/gayanvirajith/harmony) theme, but it has quite a bit of boiler plate variables compared to the poole themes, so I stuck with poole.

##The most confusing thing about learning Jekyll while starting with a theme: the paginator
Pagination was extremely confusing for me with Jekyll at first, because I couldn't figure out how to change the example blog post on the poole themes. If the initial page of the poole themes has blog post content then I should just be able to change `_layouts/post.html`? No. No not at all. I spent way too much time on this than I would like to admit. Jekyll pagination is accomplished by generating *duplicate* files for paginated content. This means that if I have a blog post and a paginated layout, Jekyll will generate the standalone blog post based on the `_layouts/post.html` and then generate the pagenated content based on whatever layout is referencing the pagination functions (in poole's case, this is `index.html`). I ran into this issue while I was trying to add commenting functionality to the blog posts. I ended up changing the `index.html` to use {% raw %}`{{ post.excerpt }}` instead of `{{ post.content }}`{% endraw %}. (Pro tip: If you edit your *.md* in windows, the excerpt logic will not work, because new lines in windows are not detected by the default `excerpt_separator` of `\n\n`.)

##Making sure you don't get garbage discussions in Disqus
While testing the Disqus integration, I easily accumulated 20+ discussions in my Disqus admin panel. Thankfully you can collapse all the discussions with their migration tool. Just migrate everything to the same URL. I used this code in conjunction with a `url` variable in my `_config.yml` to reduce ambiguities and clutter during testing.
{% highlight js %}
{% raw %}
var disqus_identifier = '{{ page.url }}';
var disqus_title = '{{ page.title }}';
var disqus_url = '{{ site.url }}{{ page.url }}';
{% endraw %}
{% endhighlight %}

##The End
We will see how well this works. I plan on writing more about my adventures with ElasticSearch, LogStash, Kibana 3 and 4, Xbian/Kodi, Synology NAS, RaspberryPi, InfluxDB, Grafana, and Debian. Damn you Kura.
