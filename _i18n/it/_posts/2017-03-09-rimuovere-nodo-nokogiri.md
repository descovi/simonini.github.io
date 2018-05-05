---
layout: post
title: Come rimuovere porzioni di html con ruby?
date: 2017-03-09 10:16:49 +0200
categories: rails html
---

Nel mio caso avevo dei tag \<script\> che erano stati infilati dentro i testi delle canzoni presumo da dei bot.

```html
  <script>codice javascript inserito da utente mal intenzionato</script>
  Testo normale.
```

Per fortuna la cosa non aveva conseguenze dirette sui visitatori del sito perché in output veniva effettuato l'escaping dei caratteri.

Le conseguenze diciamo erano diffuse ma solo estetiche.

Per rimuovere le porzioni di html _sporco_ ho utilizzato la libreria di ruby Nokogiri che in poche righe ha risolto il problema.

Ciao sono Alessandro e scrivo delle cose.

```ruby
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
```
