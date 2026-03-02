---
name: market-watcher
description: >
  Veille immobilière sur les portails belges (Immoweb, Zimmo, Logic-Immo). 
  Monitore nouvelles annonces, baisses de prix, et activité concurrentielle.
  Use when agent asks "quoi de neuf sur le marché", "surveille les annonces", 
  "nouvelles propriétés", or wants competitive intelligence in their area.
  This is a V2 skill requiring web scraping setup. Do NOT use for lead management.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [veille, marche, immoweb, zimmo, concurrence]
  compatibility: Requires web scraping infrastructure (Playwright/Puppeteer)
---

# Market Watcher — Veille Immobilière

NOTE: This skill requires web scraping infrastructure to be fully operational. For MVP, it operates in manual mode triggered by agent requests.

## Capabilities

1. Monitor new listings in watched areas (communes, price ranges, property types)
2. Detect price drops on competing listings
3. Track listings removed from market (sold or withdrawn)
4. Match new listings to buyer profiles in memory

## Daily digest format

"Market [Commune] this week: X new listings (Y apartments, Z houses). Average apartment price: Xk EUR (trend vs last month). X price drops detected. X properties similar to yours sold in Y days."

## Buyer matching

When a new listing matches a registered buyer's criteria:
"New property matching [Buyer name]'s profile: [Address] — [Price]EUR — [Bedrooms]bd — [Surface]m2. Contact them?"

## Implementation status

This skill is roadmap V2. Current mode: manual queries only. Full automation requires scheduled scraping jobs.
