# Decalid

A decentralized calendar interaction and distribution project.

## Preamble

Calendars are broken on the Internet. If we have M calendars (work, home, chores, ...) and N people we want to share those calendars with, we usually end up sharing none and instead talking out-of-band due to how hard it is to share calendars while retaining privacy.

Web calendars have no notion of fine-grained permissions or minimization of information provided while being useful to all people the calendars are shared with.

This proposal wants to make a M-1-N system where you aggregate all source "authoritative" calendars to create a "user master calendar" view that can then be shared using filters.

The filtering can ensure that no exfiltration of confidential information described in the calendar is ever compromised, while providing a convenient way for users and apps to share information to your calendar. For example, apps like Calendly should not need to access your whole calendar details even if they need to be able to read-write back into the calendar with their own events. Instead, they should just be seeing free/busy status, and not event details. This is currently not possible.

This project explores the viability of such an approach.

## Project flow chart

```mermaid
flowchart LR
    subgraph PersonalCalendar
    A1[/Personal iCloud Account/] --> B
    A3[/Personal Nextcloud Calendar/] --> B
    A4[/Friend's shared Calendar/] --> B
    RSVP[Decalid RSVP Endpoint] ..-> E
    
    SK --> X1[/Your phone/]
    SK --> X2[/Your laptop/]
    SK --> X3[/Etc/]
    SSK1 -->|ro| F1[/Friend 1/]
    SSK2 -->|ro| F2[/App 1/]
    SSK3 -->|ro| PUB[/Public version\nOnly consolidated freebusy info/]


    subgraph Decalid
    R[/Rules and filters/]
    B(Decalid Source)
    E{Self-hosted or managed\nDecalid Engine}
    SK(Decalid Master Share)
    R .-> R1
    R .-> R2
    R .-> R3
    R1 .-> SSK1
    R2 .-> SSK2
    R3 .-> SSK3
    E --> SSK1
    E --> SSK2
    E --> SSK3
    SSK1(Decalid Share 1)
    SSK2(Decalid Share 2)
    SSK3(Decalid Share 3)
    R1[/Rules for Share 1/]
    R2[/Rules for Share 2/]
    R3[/Rules for Share 3/]
    B --> E
    E --> SK
    end


    end
    subgraph WorkCalendar
    direction LR
    A2[/Work Exchange O365/] --> B2(Decalid Source)
    WBox[/Mandatory filters stipulated by Enteprise/] ..-> E2
    
    B2 --> E2{Decalid Engine\nHosted by enterprise}
    
    E2 --> SK2[Work-hosted Decalid Master Share]
    SK2 --> WK[/Share to coworker that cannot\ngo through groupware system;\neg freelancer/]

    end

    SK2 --> B
```
