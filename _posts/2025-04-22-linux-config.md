---
layout: post
title: Configurando o ambiente para contribuir com o kernel
comments: true
mathjax: true
author: Filipe Tressmann
---

Como parte da disciplina MAC0470, tive a oportunidade de enviar um patch para o kernel do Linux. Entretanto, antes disso, foi necessário configurar o ambiente em um computador seguindo os passos disponíveis no site do [flusp](https://flusp.ime.usp.br/).

Inicialmente, tentei configurar meu ambiente em uma instância EC2 com Amazon Linux 2 (AL2). Contudo, rapidamente me deparei com algumas dificuldades. Por exemplo, para compilar o kernel com a arquitetura ARM, eu precisava de um cross-compiler GCC. Infelizmente, não foi possível instalar essa dependência diretamente via yum no AL2, o que me obrigaria a compilar o cross-compiler do zero. Devido a essa e outras incompatibilidades, decidi não prosseguir com o AL2.

Mudei então para o Windows Subsystem for Linux (WSL), que se mostrou uma solução muito mais viável. Com o WSL, consegui executar todos os tutoriais do FLUSP sem problemas e, finalmente, tive sucesso no envio do meu patch para o kernel do Linux.
