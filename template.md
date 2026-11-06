# Email Template

**Subject:** Super quick q from an aspiring GTM / Marketing Engineer

---

Hi {{first_name}}!

I was a growth intern at Milliman MedInsight and am now working toward a {{role_category}} role after I graduate next May, and I saw {{company}} was hiring for one.

*Not asking about the opening, just wanted to ask how to proceed from where I'm at :)*

Quick context so this isn't random: I shipped an AI brand-compliance agent (Claude + Copilot Studio) that a 200-person team uses, took MedInsight's LinkedIn from 1,400 to 15,976 weekly impressions in a week, and grew a 100K+ personal brand I'm now building a UGC outbound automation for (Clay, Apify, Instantly).

Actually, this email found you through a separate outbound engine I built (Clay to LeadMagic to Claude): it pulls companies hiring for GTM/Growth roles, finds the right person, and personalizes each send. You're one of my first live tests :)

{{relationship_line}}, what project(s) or experience would make you hire someone / want to speak to them further?

Even a quick pointer would help :)

Akshay Iyer
LinkedIn | GitHub

---

## Merge fields

| Field | Source | Example |
|---|---|---|
| `first_name` | Apollo / Clay | Alex |
| `company` | Job posting | Acme |
| `role_category` | Job title, grouped | GTM Engineer, Growth Engineer, Marketing Engineer |
| `relationship_line` | Contact's relationship to the open role | See below |

## Relationship variants

| Contact | `relationship_line` |
|---|---|
| Owns or manages the open role (Head of Growth, Growth Lead, Head of Marketing, founder) | Since you're hiring for this role |
| Works on the same team (Growth Engineer, Growth Marketer, Product Marketing) | Since you're on the growth/marketing team there |
