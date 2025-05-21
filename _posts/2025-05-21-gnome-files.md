---
layout: post
title: Contribuição ao GNOME Files
comments: true
mathjax: true
author: Filipe Tressmann
---

Como parte da disciplina MAC0470, tive a oportunidade de contribuir com o projeto *Files* do GNOME.

O primeiro passo foi encontrar uma issue aberta que não fosse muito complexa, permitindo que eu me familiarizasse com o ambiente do projeto — como testá-lo, como contribuir, entre outros aspectos.

Nesse sentido, o GNOME é muito bem estruturado. As issues são detalhadamente documentadas e há uma pessoa responsável por organizá-las, atribuindo labels e descrições bastante claras.

Após algum tempo de pesquisa, encontrei uma issue interessante para uma primeira contribuição: [#3822](https://gitlab.gnome.org/GNOME/nautilus/-/issues/3823). O problema descrito era que, ao criar uma pasta a partir de uma seleção de arquivos usando a ação "New folder with selection", a funcionalidade de desfazer (Ctrl+Z) não se comportava corretamente. Os arquivos eram movidos para fora da nova pasta, mas a pasta não era excluída como esperado.

Para resolver esse problema, precisei entender como o sistema de *undos* funcionava internamente — o que exigiu certo tempo. O projeto utiliza *callbacks* para lidar com ações assíncronas, o que dificultou um pouco a compreensão inicial do fluxo de execução.

Depois de entender como as *callbacks* se relacionavam, parti para a implementação. Enfrentei mais alguns desafios, principalmente por precisar aprender um pouco sobre a `glib`, a biblioteca utilizada no projeto.

Com a implementação finalizada, o processo de envio da *Merge Request* (MR) foi relativamente simples: criei um *fork* do projeto, subi minhas alterações e segui os passos da interface do GitLab.

Você pode conferir minha MR [aqui](https://gitlab.gnome.org/GNOME/nautilus/-/merge_requests/1752)
