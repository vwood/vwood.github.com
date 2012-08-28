---
layout: post
title: Webscraping
tags: [python, frameworks suck, scraping]
published: false
---

{{ page.title }}
================
<p class="meta">17 August 2012</p>

So I've wanted to do some webscraping. So I took a look a scrapy. It's a framework, so it's crap and tries to take over any project it's in[^1]. This is thanks to Twisted being shit.

So let's use mechanize and libxml2.


[^1]: All frameworks suck. They're heavyweight, and limiting. Libraries are much better.


Python language mistakes:
lambda
statement/expression
map, and, or
conflats let with set (no warning for incorrect names)
shitty scoping rules
explicit return
packages are crap
default values are broken
self.x = x
preference for frameworks rather than libraries


~~~~
#!/usr/bin/env python2

import re
import mechanize
import lxml.html

class Scraper(object):
    start_urls = []
    follow_urls = []
    reject_urls = []
    rules = []
    allowed_domains = []
    useragent = 'Mozilla/5.0 compatible (Commodore 64; 6502)'
    max_visits = 5

    def __init__(self):
        self.visited = set()
        self.to_visit = set(self.start_urls)

    def config_browser(self, browser):
        pass

    def parse(self, response):
        pass

    def scrape(self):
        br = mechanize.Browser()
        br.addheaders = [('User-agent', self.useragent)]
        self.config_browser(br)

        while len(self.to_visit) > 0 and len(self.visited) < self.max_visits:
            url = self.to_visit.pop()
            response = br.open(url)
            self.visited.add(url)
            if not br.viewing_html():
                continue

            self.parse(Response(title=br.title(),
                                url=response.geturl(),
                                header=response.info(),
                                contents=lxml.html.parse(response)))
            for regex in self.follow_urls:
                for link in br.links(url_regex=regex):
                    self.add_link(link)

    def add_link(self, link):
        if link.url in self.visited or link.url in self.to_visit:
            return
        if link.url[0] == '#':
            return
        for reject in self.reject_urls:
            if re.search(reject, link.url):
                return
        for domain in self.allowed_domains:
            if re.match('^http://(www\.)?' + domain, link.url):
                self.to_visit.add(link.url)
                return

class Response():
    def __init__(self, header='', title='', contents='', url=''):
        self.header = header
        self.title = title
        self.contents = contents
        self.url = url

def get_path_of_element(elem):
    """Given an element, return the absolute Xpath of that element."""
    path = []
    while elem is not None:
        parent = elem.getparent()
        if elem.attrib.has_key('id'):
            path.insert(0, "%s[@id='%s']" % (elem.tag, elem.attrib['id']))
        elif parent is not None:
            children = list(parent.iterchildren(tag=elem.tag))
            if len(children) < 2:
                path.insert(0, elem.tag)
            else:
                path.insert(0, "%s[%d]" % (elem.tag, children.index(elem)))
        else:
            path.insert(0, elem.tag)
        elem = parent
    return '/' + '/'.join(path)

def get_tree_of_element(elem, depth=None):
    """Given an element, return a tree representation of just the elements."""
    if depth > 0:
        depth -= 1
    elif depth == 0:
        return str(elem.tag)
    children = [get_tree_of_element(child, depth=depth) for child in elem]
    if len(children) > 0:
        return '%s[%s]' % (elem.tag, ', '.join(children))
    return str(elem.tag)

class RedditScraper(Scraper):
    start_urls = ['http://www.reddit.com']
    follow_urls = [r'/r/[^/]+/$']
    reject_urls = [r'/user/', r'/comments/', r'/\?', r'/top/']
    rules = []
    allowed_domains = ['reddit.com']
    max_visits = 50

    def parse(self, response):
        print response.url,
        print get_tree_of_element(response.contents.getroot(), depth = 3)

def google(query, n = 10):
    query = 'https://www.google.co.nz/#hl=en&safe=active&output=search&q=%s&oq=%s&start=0' % (query, query)
    return query

class GoogleScraper(Scraper):
    start_urls = ['https://www.google.co.nz/#hl=en&safe=active&output=search&q=cats&oq=cats&start=0&gbv=1']
    allowed_domains = ['google.co.nz']

    def parse(self, response):
        for i, result in enumerate(response.contents.xpath("//a")):
            print i, result.attrib


#        for i, result in enumerate(response.contents.xpath("//ol")):
#            print i, result.xpath("//*/text()"), result.attrib
#        for i, result in enumerate(response.contents.xpath("//*[contains(text(), 'cat')]")):
#            print i, result.tag, result.attrib, get_path_of_element(result)
#        for i, result in enumerate(response.contents.xpath("//li[@class='gbt']/*/text()")):
#            print i, result
#        for i, result in enumerate(response.contents.xpath("//li[@class='gbmtc']/*/text()")):
#            print i, result

class FreelancerScraper(Scraper):
    start_urls = ['http://www.freelancer.co.nz/jobs/Web-Scraping/']
    allowed_domains = ['freelancer.co.nz']

    def parse(self, response):
#       elem.attrib, elem.tag, elem.text
        def summarise(item, n=30):
            if item is None:
                return ""
            item = str(item).strip().replace("\n", " ")
            if len(item) > n:
                item = item[:n]
            return '"' + item + '"'

        for elem in response.contents.xpath("//table[@id='project_table_static']/tbody/tr"):
            print ', '.join(summarise(td) for td in elem.xpath("td/text()") if td is not None)

ss = GoogleScraper()
ss.scrape()
~~~~
