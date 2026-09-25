+++
title = "Converting Blogger Website to Hugo"
date = 2026-08-07
summary = "Thank you Blogger (Google) for being by my blogging side from th initial days!"
categories = ["Blogging"]
+++
<p>Today, I am going to share what I did to transfer my olddomain.com Blogger-based website to newdomain.com static Hugo website that is hosted in Cloudflare.</p>

<p>First, I transfered all the content of olddomain.com Blogger-based website to static Hugo on my local PC quite manually. Then I deployed the static Hugo website (newdomain.com) in Cloudflare Pages.</p>

<p>Both of my domains are managed by Cloudflare. Hence, it is convenient to me to redirect old domain to new one.</p>

<h2>Step 1: Use Cloudflare IP for Old Domain</h2>
<p>First, I needed Cloudflare to have total control on my old domain. Hence, I edited the DNS Record of my old domain:</p>
<ul>
  <li>Type: A, Name: @, Content: 192.0.2.1 (Proxied)</li>
  <li>Type: A, Name: www, Content: 192.0.2.1 (Proxied)</li>
</ul>


<h2>Step 2: Use Page Rule from Old Domain</h2>
<p>Then, I used page rules segment of old domain to deploy a kind of page rule:</p>
<ul>
  
 <li>URL: https://www.olddomain.com/*</li>
  <li>SETTING: Forwarding URL (Status Code: 301 - Permanent Redirect)</li> 
  <li>Destination Url: https://www.newdomain.com/$1</li>
</ul>

<h3>OPTIONAL (For SEO): Set URL Slug Redirect in New CMS/Backend</h3>
<p>Since I deploy the static Hugo website, I needed the dynamically redirected URLs to get navigated from old Blogger styled slug (/yyyy/mm/blog-title.html or /p/page-title.html) to new one (/posts/blog-title/ or /pag-title/). For that I created _redirects file in /static folder.</p>

<p>In that file, I manually listed 301 redirect from old slugs to new slugs as:</p>
<ul>
  <li>/yyyy/mm/blog-title.html  /posts/blog-title/  301</li>
  <li>/p/page-title.html  /page-title/  301</li>
  </ul>


<p><b>Post Script</b>: My old domain is not olddomain.com and new domain is not newdomain.com. I used these just to make it easy to understand. Hopefully, this blog posts provides a tentative guide for the interested ones who want to move Blogger-based website/blog to static Hugo site hosted on Cloudflare, Netlify etc.</p>