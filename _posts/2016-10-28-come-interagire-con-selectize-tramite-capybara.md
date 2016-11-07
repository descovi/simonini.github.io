---
layout: post
title: Come interagire con selectize tramite Capybara
categories: rails tdd integration-test capybara selectize javascript
---

Interagire attraverso Capybara con la propria app e un javascript driver non è una cosa agevolissima. In particolare se si è alle prime armi come me su questo tipo di argomento.
Ho faticato non poco a trovare una soluzione all'interazione col plugin javascript selectize (https://github.com/selectize/selectize.js/).

Alla fine, come spesso accade in queste occasioni, la soluzione non era così complessa ma la ricerca per la soluzione invece è stata molto più lunga del previsto.

Ecco il metodo che ho creato all'interno del mio helper (test/helpers/form_helper.rb)

{% highlight ruby %}
def select_tags_from_selectize(parent_selector, with:)
  fail "Array expected" unless with.is_a?(Array)
  selector = "#{parent_selector} select".to_json.html_safe
  js_string = "var selector = $(#{selector})[0].selectize.setValue(#{with.to_json.html_safe})"
  page.execute_script(js_string)
end
{% endhighlight %}

Decisivo nella ricerca e nella soluzione dei miei problemi è stato reperire questo prezioso gist: [link](https://gist.github.com/jcieslar/6491810f4fd9fada0ab3).
