---
layout: post
title: Mostra nella sidebar
categories: editor
---

# Scorciatoie per la tastiera

Scorciatoia per mostrare  nella sidebar

    { "keys": ["alt+."], "command": "reveal_in_side_bar"}

Scorciatoia alternativa a cmd+k +b per nascondere e mostrare la sidebar.
Ho creato questa scorciatoia perchè mi trovo spesso a mostrare/nascondere la sidebar e la scorciatoia di default obbligava ad un numero eccessivo di passaggi.

    {"keys":["ctrl+s"], "command": "toggle_side_bar"},


# Setting di sublime

Questi sono i miei attuali settaggi di Sublime Text.

    {
      "color_scheme": "Packages/Seti_UX/Seti.tmTheme",
      "font_face": "Monaco",
      "font_size": 14,
      "highlight_line": true,
      "ignored_packages":
      [
        "Vintage"
      ],
      "save_on_focus_lost": true,
      "tab_size": 2,
      "translate_tabs_to_spaces": true
    }

Trovo che

    "save_on_focus_lost": true

Sia il miglior dei recenti settaggi.
In questo modo passando da browser a sublime text il salvataggio è automatico.

    "tab_size": 2,
    "translate_tabs_to_spaces": true

Questi ultimi settaggi sono invece più che altri dettati dalle convenzioni di ruby/rails che è il mio principale ambiente di lavoro.
