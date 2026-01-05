---
title: "How Structured Thinking Transformed REST API Design"
datePublished: Sun Jan 04 2026 18:26:11 GMT+0000 (Coordinated Universal Time)
cuid: cmk02bwfa000602k18qa51xad
slug: how-structured-thinking-transformed-rest-api-design
tags: software-architecture, consulting, system-design, backend-development, api-design, learninginpublic

---

While building the backend for **Ace Rentals**, I encountered a design issue that didn’t surface as runtime failures but gradually reduced confidence in the system. Authorization checks were implemented directly inside individual routes. Although functionally correct, this approach made security dependent on how carefully each route was written, which does not scale as systems evolve.

As the number of endpoints increased, ownership checks had to be reimplemented repeatedly. This resulted in duplicated logic, higher maintenance effort, and the risk of missing an authorization check altogether. The underlying problem was not functional—it was structural. Authorization existed as an implementation detail rather than an architectural guarantee enforced by the system.

By applying structured thinking, I redesigned authorization as a system-level concern. This article summarizes the problem, the reasoning behind the design decisions, the implementation approach, and the impact of the change.

## Applying Structured Thinking to the Problem

To understand and resolve the issue, I applied structured thinking frameworks in sequence.

First, I used **MECE analysis** to examine the authorization logic. This revealed overlaps and gaps—authorization checks were neither mutually exclusive nor collectively exhaustive, meaning enforcement was inconsistent and incomplete.

Next, I evaluated alternative designs using a **prioritization matrix**, comparing options on maintainability and reusability. Centralized middleware clearly emerged as the strongest option, offering a single enforcement point and eliminating duplication.

Finally, I defined **SMART goals** to translate the selected design into a focused implementation plan. This helped control scope and ensured the refactor addressed the core problem without expanding unnecessarily.

## **Execution Details**

Based on this analysis, authorization was treated as a system-level concern and implemented accordingly.

* Ownership checks were consolidated into a single, reusable middleware
    
* API endpoints were reorganized around resources rather than actions
    
* Authorization was enforced early in the request pipeline, before any business logic executed
    

This implementation transformed authorization from a manual developer responsibility into an architectural guarantee enforced by the system.

## Impact of the Design Change

The effects of this redesign were clear and measurable:

* **Eliminated duplicated authorization logic:** Ownership checks now live in one place instead of being repeated across routes
    
* **Consistent enforcement across protected routes:** Every protected endpoint applies the same rules by default
    
* **Reduced maintenance and regression risk:** Authorization changes are made once and applied everywhere
    
* **Clearer resource relationships:** Resource-based routes make relationships (such as listings and their reviews) explicit
    
* **Stronger separation of concerns:** Route handlers focus only on business logic, while authorization is handled independently
    

A qualitative risk assessment showed that core business logic is stable. The remaining risk is concentrated in shared authorization infrastructure, where it is explicit, visible, and easier to reason about and manage.

## Key Insight

Repeated logic and small moments of uncertainty are often early signals of deeper design issues. Treating security as a structural concern—rather than something developers must remember to implement—significantly improves clarity, reliability, and long-term maintainability.

## Design Decisions and Learnings

This article presents a high-level summary of the key decisions and outcomes from redesigning authorization in Ace Rentals.

For a deeper look into my **learning experience**—including how I identified the problem, applied structured thinking frameworks, evaluated design alternatives, and translated those decisions into implementation—you can read the full write-up here:

[**https://ace-2504.github.io/api-design-blog/**](https://ace-2504.github.io/api-design-blog/)

The full version documents the complete reasoning process, trade-offs considered, and the lessons I learned while designing and refactoring the system.