# Hiring Signal Outbound

Signal-based outbound I ran on my own job search. I found companies hiring GTM and Growth Engineers, found the person closest to that role at each one, checked the data, and sent a short, targeted email asking what would make them want to talk to a candidate.

42 templated emails got 5 replies (11.9%). One fully researched email got an in-person invite within 3 days. The replies told me exactly what to build next, so this is v0 of a bigger system.

## The problem

I want a GTM or Marketing Engineer role. The usual path (apply on a job board, wait) is slow and gets ignored. Outbound works for selling software, so I treated my job search like a sales motion:

- **The signal:** a company posting a GTM or Growth Engineer role is actively investing in the function I want to work in.
- **The buyer:** the person who owns or works next to that role, not a recruiter.
- **The ask:** low pressure. Not "hire me," but "what would make you want to talk to someone?"

## How it works

```
Find companies hiring          Find the right person         Validate              Send
GTM / Growth Engineers   →     closest to the role      →    current role    →    personalized email
(Clay + Apollo)                (Apollo + Clay)               (Apify)               (Mailmeteor, Google Sheets)
                                                                                         ↓
                                                                                  Track replies
                                                                                  (by hand, Gmail)
```

1. **Find companies.** Pulled job postings for GTM Engineer and Growth Engineer roles across the US and London into a Clay table: company, domain, job title, role category, location, and post date.
2. **Find the right person.** Used Apollo and Clay to find 1 to 3 people per company on the growth or marketing team, ranked toward the person who owns or works next to the open role (Head of Growth, Growth Lead, Growth Engineer, Head of Marketing).
3. **Validate.** Used Apify to scrape each contact's LinkedIn profile and check they were still in that role, since enrichment data goes stale.
4. **Personalize.** Built the email from merge fields (first name, company, open role) plus a variant chosen by the contact's relationship to the role: "since you're hiring for this role" for owners, "since you're on the growth team there" for peers.
5. **Send.** Scheduled all 42 sends through Mailmeteor from Google Sheets, with no tracking pixels, to keep it looking like a normal email from a person.
6. **Track.** Logged replies by hand from Gmail.

## Stack

Clay · Apollo · Apify · Google Sheets · Mailmeteor · Gmail

## The email

The full template, with merge fields, is in [`template.md`](template.md). The design choices that mattered:

- **Lead with a non-ask.** "Not asking about the opening, just wanted to ask how to proceed from where I'm at." This lowered the bar to reply.
- **Earn the read in one paragraph.** Three concrete proof points with numbers, nothing else.
- **Be honest about the method.** The email told each person they were found by an outbound engine and were one of its first live tests. That turned the email itself into a proof point.
- **Ask one specific question** they could answer in two lines.

## Results

| Metric | Result |
|---|---|
| Emails sent | 42 (Sep 14, 2026, sent over ~9 minutes) |
| Replies | 5 (11.9%) |
| Detailed, actionable replies | 3 |
| Ongoing conversations | 1, which turned into an invitation to pitch an idea |
| Time to first replies | 4 replies within ~4 hours, 1 more 7 weeks later |
| Companies reached | 42, from Series A AI startups to companies like Clay |

### What the replies said

Paraphrased, since these were private emails:

- **A Senior Growth Engineer at an AI search startup:** they hire people who ship end-to-end GTM systems themselves (enrichment, outbound, lifecycle, scoring), work in real code and APIs rather than only configuring tools, and measure impact on pipeline and revenue, not impressions.
- **A Growth Strategist at a major GTM platform:** shift from "here's what I've built" to "here's what I'd build for you." Pick companies, find a specific problem each one has, reach out to the person who owns it, and for the best ones, build a small v1 and walk through it in a 2-minute Loom.
- **A Founding Growth Marketer at an NYC startup:** show business impact in hard ROI and pipeline, beyond impressions.
- **A Growth Lead at a GTM software company:** asked to see the 100K creator brand mentioned in the email. That started a thread over six weeks. When I pitched a project idea, they said it wouldn't help, and invited me to come back with something on the social or education side of their business instead.

The last one is the reply I learned the most from. A specific idea, even a wrong one, got a specific "no" and a clear direction, which a general ask never would have.

## From template to one-to-one

After the batch send, I tested the opposite approach: one email, fully researched, to one person.

The target was an advisor helping a fast-growing startup build its Growth Engineering function, someone who had also written publicly about the rise of GTM Engineering. Instead of a template, the email:

- **Opened with their work.** It referenced a piece they'd written about GTM Engineering, so the first line could only have been written to them.
- **Named the gap honestly.** The open roles asked for 5+ years. The email said so, and asked about an intern or contract path instead.
- **Matched proof to their team.** Each proof point mapped to what a Growth Engineering team does: outbound systems, an AI agent in production, marketing context, and a growth system pointed at my own business.
- **Ended with a small ask:** any pointer on how to grow toward the role.

| Metric | Result |
|---|---|
| Emails sent | 1 |
| Reply | Yes, in 3 days |
| Outcome | Invited to an in-person event the same week |

**The comparison:** 42 templated sends got 5 replies and useful advice. One researched email got an invitation. Both have a place. Volume finds the people who will answer, and research earns the conversation. That's the balance `outbound-agent` is built around: score many, research the best few deeply.

## What broke

| Problem | What happened | Fix in v1 |
|---|---|---|
| Wrong person | Some contacts weren't close to the role (a PR coordinator, a junior e-commerce specialist, an affiliate marketer). | Score every contact on seniority and closeness to the role before sending. |
| Mismatched data | One row paired a person's name with someone else's LinkedIn URL. | Cross-check the name on the scraped profile against the enriched name. |
| Stale data | One "Head of Growth" profile described a job at a different company. | Compare the scraped current employer to the target company and drop mismatches. |
| Coverage gaps | About 6 companies returned no matching profile. | Add a fallback: try the founder or Head of Marketing at small companies. |
| No tracking | Tracking was off, so there was no open data, and replies were logged by hand. | Log every send and reply in a database, and read replies from Gmail automatically. |
| Generic ask | The email asked for advice, not a conversation about their problem. | Lead with a specific problem at their company and a v1 I'd build for it. |

## What I'm building next

Every reply pointed the same direction: real code, scoring, a specific take on their business, and outcomes measured in pipeline. That's what v1 does.

[`outbound-agent`](https://github.com/akshayiyer23/outbound-agent) (in progress):

- Pulls fresh job postings from company job boards every day
- Scores each company and contact in Python before anything is sent
- Researches each company and proposes a specific v1 project for that team, which I approve
- Drafts the email in Gmail, follows up, and logs everything in a database
- Measures replies and interviews booked, compared against this v0

## What I learned

- **Targeting beat writing.** The right person at the right company answered a fairly simple email within hours.
- **Research beat volume for the best targets.** One email built around the reader's own work did more than 42 templated ones.
- **The data is never clean.** Enrichment returns stale and mismatched records, so validation has to be a real step, not an afterthought.
- **Outbound is research.** Three replies from people doing the job I want gave me a clearer roadmap than any course.
