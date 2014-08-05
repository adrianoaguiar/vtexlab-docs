---
layout: docs
title: Expiração do Totem
application: totem
docType: lib
version: latest
---

# Script de expiração para Totem 

## totem-expiration.js

Utilitário de expiração de sessão por tempo.  
Ao ser iniciado, um contador de 10 minutos é configurado.  
Qualquer interação do usuário com a página reinicia o contador.  
Após 10 minutos sem interação, o cookie do carrinho é removido e o navegador é redirecionado para uma página default.

## Versão 0.1.1

Testes unitários: http://io.vtex.com.br/totem/0.1.1/index.html  
Script para desenvolvimento: http://io.vtex.com.br/totem/0.1.1/script/totem-expiration.js  
Script para produção: http://io.vtex.com.br/totem/0.1.1/script/totem-expiration.min.js  

### Funções públicas

#### vtex.totem.startExpiration(url, millis, events)

Inicia timer.

Parâmetros e defaults:

- url = '/'
- millis = 10 * 60 * 1000 (10 minutes)
- events = ["mousemove", "keyup", "click", "scroll"]

#### vtex.totem.stopExpiration()

Pára contador em execução.