---
title: Guide How To Übersicht
description: 
published: true
date: 2025-04-19T14:10:22.378Z
tags: 
editor: markdown
dateCreated: 2025-04-19T13:43:50.081Z
---

# Zielsetzung dieses Dokuments

Dieses Dokument dient als zentraler Ausgangspunkt für die Definition von Rahmenbedingungen. Es stellt zentrale Fragestellungen vor, beantwortet diese in kompakter Form (TL;DR) und verweist auf weiterführende Inhalte, in denen die Themen ausführlich behandelt werden.

Sollten im Laufe der Entwicklung weitere, thematisch ähnliche Fragen aufkommen, die in diesem Dokument noch nicht behandelt werden, sind diese nachzupflegen. Dieses Vorgehen stellt sicher, dass eine einheitliche Herangehensweise im gesamten Projekt gewährleistet ist. Dadurch wird eine bereichsübergreifende Entwicklung unterstützt, da alle Beteiligten einem gemeinsamen Muster folgen.

> **TL;DR:**  
> Dieses Dokument definiert zentrale Rahmenbedingungen durch kompakte Beantwortung häufig gestellter Fragen (TL;DR) mit Verlinkungen zu detaillierten Inhalten. Neue, ähnliche Fragestellungen sollen ergänzt werden, um eine einheitliche und bereichsübergreifende Entwicklung im Projekt sicherzustellen.


# FAQ

### Wann soll ich in Unity 2D `OnTriggerEnter2D` verwenden und wann lieber ein `Physics2D.Overlap*`-basierter Sensor?

> **TL;DR:**  
> Verwende `OnTriggerEnter2D` / `OnTriggerExit2D`, wenn du eine sofortige Reaktion beim Betreten/Verlassen eines Bereichs brauchst.  
> Nutze `Physics2D.Overlap*` mit Tickrate, wenn du regelmäßig checken willst, ob sich etwas im Bereich befindet – ideal für Zonen-Checks oder KI.  
> ➜ [Ausführliche Erklärung mit Beispielen](unity_2d_sensors_guide.md)
