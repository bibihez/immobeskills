---
name: visit-scheduler
description: >
  Planification des visites immobilières avec gestion du calendrier, notifications 
  et logique de distance. Use when agent says "planifie une visite", "organise des 
  visites", "booking visite", or when a lead confirms interest in visiting a property.
  Integrates with Google Calendar/Outlook. Do NOT use for initial lead response — 
  use lead-responder first.
metadata:
  author: TreeLaunch
  version: 2.0.0
  category: real-estate
  tags: [visites, calendar, planning, rendez-vous]
---

# Visit Scheduler — Planification Visites

## Step 1: Propose time slots

Check agent's calendar. Rules:
- Respect visit time blocks (e.g., Tue/Thu 14h-18h)
- Account for travel time between properties
- Group visits by geographic zone when possible
- Never propose Sunday unless agent explicitly allows it

## Step 2: Create calendar event

Summary: "Visite — [Short address]"
Description: Lead name, phone, property address, price, reference
Location: Full address
Duration: 30 minutes default
Reminder: 30 minutes before

## Step 3: Notifications

To lead (email or WhatsApp): confirmation with date, time, exact address.
To agent: confirmation with lead info.
J-1 reminder to lead: "Confirm your attendance?"

## Step 4: Distance logic

If agent has known office address:
- Property 25km+ away: reminder 24h before
- Property under 25km: reminder 2h before

## Step 5: Group visits

For open houses or grouped showings:
- Propose single timeslot with 15-min intervals
- Manage waitlist if slot is full
- Notify waitlisted leads when slot opens

## Examples

Example 1: Multiple visits same day
Result: "3 visits tomorrow: 14h00 Rue de Flandre (Dupont), 14h45 Avenue Louise (Martin), 15h30 Chaussée de Charleroi (Janssen). All within 3km. Total: 2h30."
