---
title: 3HFood API

language_tabs: # must be one of https://prismjs.com/#supported-languages
  - javascript

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - common.md
  - wholesaler.md
  - supermarket.md
  - errors.md

search: true
code_clipboard: true
---

# Introduction

L'API 3Hfood est une api unique pour tous les endpoints de toutes les applications 3HFood.

Le tout est hébergé sur des cloud function, les fonctions sont alors à appeler par le biais du package firebase dans l'application

# Authentication

Toutes les routes (sauf login) ont besoin d'une authentification pour foncionner. Le package firebase se charge tout seul du renew de token, et de passer le token dans la requête.



