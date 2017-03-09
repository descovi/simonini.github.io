---
layout: post
title: Come rimuovere porzioni di html con ruby?
date: 2017-03-09 10:16:49 +0200
categories: rails nokogiri html
---

Nel mio caso avevo dei tag \<script\> che erano stati infilati dentro i testi delle canzoni presumo da dei bot.

{% highlight html %}
  <script>codice javascript inserito da utente mal intenzionato</script>
  Testo normale.
{% endhighlight %}

Per fortuna la cosa non aveva conseguenze dirette sui visitatori del sito perchè in output veniva effettuato l'escaping dei caratteri.

Le conseguenze diciamo erano diffuse ma solo estetiche.

Per rimuovere le porzioni di html _sporco_ ho utilizzato la libreria di ruby Nokogiri che in poche righe ha risolto il problema.

{% highlight ruby %}
  def self.clean_from_script_tag
    songs = Song.all
    songs.each do |s|
      lyric = Nokogiri::HTML.fragment(s.text)
      if lyric.xpath("script").present?
        lyric.xpath("script").remove
        s.text = lyric.to_html
        s.save
      end
    end
  end
{% endhighlight %}