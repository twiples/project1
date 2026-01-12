# Product Requirements Document (PRD)
## Helen's Ballot Measures Tracker

**Version:** 1.0
**Last Updated:** January 11, 2026
**Status:** Live (2025 data)

---

## 1. Overview

### 1.1 Product Vision
A user-friendly website that helps voters understand ballot measures by answering the key question: **"What's the problem this measure is trying to solve?"** The site presents complex ballot information in an accessible, unbiased format with clear context about who benefits, who pays, and who's behind each measure.

### 1.2 Target Users
- Everyday voters seeking to understand ballot measures before voting
- Civic-minded citizens who want deeper context beyond headlines
- Researchers and journalists covering ballot initiatives

### 1.3 Core Value Proposition
Unlike other ballot tracking sites that just list pros/cons, this site:
1. Explains the **problem being solved** ("Why This Matters")
2. Shows **who benefits vs. who pays** (Beneficiaries/Funders)
3. Reveals **who's behind the measure** (Sponsored by)
4. Displays **endorsements** from both sides

---

## 2. Current Features

### 2.1 Interactive Map
| Feature | Description |
|---------|-------------|
| USA Map Background | Grayscale image of USA as visual reference |
| State Dots | Clickable circular markers for each state with data |
| Hover Effects | Dots scale up on hover with shadow enhancement |
| State Labels | Two-letter abbreviations (CA, TX, etc.) on each dot |

**States Currently Covered (2025 data):**
- California (90 measures)
- Texas (17 measures)
- Maine
- Colorado
- New York
- Washington
- Louisiana
- Ohio
- Wisconsin
- Arizona
- Illinois

### 2.2 Measure Card Structure

Each ballot measure displays the following information hierarchy:

#### Always Shown:
| Field | Description | Example |
|-------|-------------|---------|
| `jurisdiction` | Geographic scope | "Santa Clara County" or "Statewide" |
| `title` | Measure name | "Sales Tax for Health Care" |
| `id` | Proposition/Measure ID | "Measure A" or "Proposition 50" |
| `result` | Outcome | "Approved" / "Rejected" |
| `resultText` | Detailed result | "Approved 57%" |
| `pros` | Arguments in favor | Semicolon-separated list |
| `cons` | Arguments against | Semicolon-separated list |
| `source` | Data sources | "KQED, Mercury News" (clickable links) |

#### Enhanced Fields (for detailed measures):
| Field | Description | Example |
|-------|-------------|---------|
| `summary` | Plain-English explanation | "Increases county sales tax by 0.625%..." |
| `problem` | **"Why This Matters"** - The core problem | "Federal cuts threaten to close hospitals..." |
| `cost` | Total cost/revenue | "$330M/year" |
| `funding` | How it's funded | "0.625% sales tax" |
| `impactOnYou` | Personal cost to voter | "~$62/year average" |
| `currentSituation` | Background context | "County operates 4 public hospitals..." |
| `whoIsImpacted.beneficiaries` | Who benefits | ["Uninsured patients", "Low-income families"] |
| `whoIsImpacted.funders` | Who pays | ["All county residents (via sales tax)"] |
| `benefits` | Categories of benefit | "Healthcare access, Hospital operations" |
| `isNewOrUpdate` | New policy or update | "New 5-year temporary tax" |
| `initiatedBy.name` | Sponsor name | "Santa Clara County Board of Supervisors" |
| `initiatedBy.type` | Sponsor type | "Government" / "Industry" / "Grassroots" |
| `initiatedBy.motivation` | Why they sponsored it | "Prevent hospital closures..." |
| `endorsements.support` | Organizations in favor | [{name, type}] |
| `endorsements.oppose` | Organizations opposed | [{name, type}] |

### 2.3 Sponsor Types & Icons
| Type | Icon | Description |
|------|------|-------------|
| Government | :classical_building: | City/county/state agencies |
| Industry | :office: | Business/corporate sponsors |
| Grassroots | :seedling: | Citizen-led initiatives |
| Political | :balance_scale: | Political parties/groups |
| Labor | :fist: | Unions |
| Nonprofit | :green_heart: | 501(c)(3) organizations |
| Business | :briefcase: | Business associations |
| Community | :busts_in_silhouette: | Community groups |

### 2.4 Search Functionality
- Real-time search as user types (minimum 2 characters)
- Searches across all fields: title, pros, cons, summary, problem, endorsements, etc.
- Displays matching results with state context
- Shows total result count

### 2.5 State Details Panel
When a state is clicked:
- State name header
- Summary stats: Total Measures, Approved, Rejected
- Scrollable list of all measure cards
- Close button

### 2.6 Expandable Details ("Learn More")
For measures with additional context:
- Collapsible section with:
  - Current Situation
  - Who Is Impacted (Beneficiaries / Funders)
  - Benefits
  - New or Update?
  - Motivation

### 2.7 Password Protection
- Simple password gate for site access
- Password stored in browser localStorage
- Configurable password in code

---

## 3. Data Model

### 3.1 State Object
```javascript
{
    name: "California",           // Display name
    total: 90,                    // Total measures
    approved: 76,                 // Approved count
    rejected: 14,                 // Rejected count
    measures: [...]               // Array of measure objects
}
```

### 3.2 Measure Object (Full Schema)
```javascript
{
    // Required fields
    id: "Measure A",
    title: "Sales Tax for Health Care",
    jurisdiction: "Santa Clara County",
    result: "approved",           // "approved" | "rejected"
    resultText: "Approved 57%",
    pros: "Semicolon-separated arguments",
    cons: "Semicolon-separated arguments",
    source: "KQED, Mercury News",

    // Enhanced fields (optional)
    summary: "Plain English explanation",
    problem: "The problem this solves",
    cost: "$330M/year",
    funding: "0.625% sales tax",
    impactOnYou: "~$62/year average",
    currentSituation: "Background context",
    whoIsImpacted: {
        beneficiaries: ["Group 1", "Group 2"],
        funders: ["Funder 1", "Funder 2"]
    },
    benefits: "Category 1, Category 2",
    isNewOrUpdate: "New 5-year temporary tax",
    initiatedBy: {
        name: "Sponsor Name",
        type: "Government",
        motivation: "Why they sponsored this"
    },
    endorsements: {
        support: [
            { name: "Org Name", type: "Labor Union" }
        ],
        oppose: [
            { name: "Org Name", type: "Taxpayer Group" }
        ]
    }
}
```

### 3.3 2026 State Voting Schedule
```javascript
{
    state: "California",
    signatureDeadline: "2026-06-25",
    signaturesRequired: { amendment: 874641, statute: 546651 },
    source: "Ballotpedia",
    notes: "Signatures verified 131 days before election"
}
```

**States with 2026 deadlines tracked:** 23 states (see `stateVotingSchedule2026` object)

---

## 4. Design System

### 4.1 Visual Style (Squero/Nest-inspired)
- **Light, airy backgrounds** - #f5f5f5 primary, #ffffff cards
- **Minimal color palette** - Cerulean accent (#00afd8)
- **Generous white space**
- **Refined typography** - Inter (sans) + Fraunces (serif)
- **Smooth animations** - cubic-bezier(0.16, 1, 0.3, 1)
- **Subtle shadows** - layered depth without heavy borders

### 4.2 Color Palette
| Token | Color | Usage |
|-------|-------|-------|
| `--bg-primary` | #f5f5f5 | Page background |
| `--bg-secondary` | #ffffff | Card backgrounds |
| `--text-primary` | #1a1a1a | Headlines |
| `--text-secondary` | #7b858e | Body text |
| `--accent` | #00afd8 | Links, highlights |
| `--success` | #22c55e | Approved badges |
| `--danger` | #ef4444 | Rejected badges |

### 4.3 Tag Colors
| Tag Type | Background | Text |
|----------|------------|------|
| Beneficiary | #dcfce7 (green) | #166534 |
| Funder | #fef3c7 (amber) | #92400e |
| Support endorsement | Light blue | Blue |
| Oppose endorsement | Light red | Red |

---

## 5. Technical Architecture

### 5.1 Stack
- **Frontend:** Single HTML file with embedded CSS and JavaScript
- **Data:** Inline JavaScript object (`ballotData`)
- **Hosting:** GitHub Pages
- **No backend required**

### 5.2 Key Functions
| Function | Purpose |
|----------|---------|
| `buildMeasureCard(measure)` | Renders a measure card HTML |
| `showStateDetails(stateKey)` | Opens state panel with measures |
| `displaySearchResults(results, query)` | Shows search results |
| `formatSourceLinks(sourceText)` | Converts sources to clickable links |
| `buildEndorsementsSection(endorsements)` | Renders support/oppose lists |
| `toggleDetails(btn)` | Expands/collapses "Learn More" |
| `hasDeadlinePassed(stateKey)` | Checks 2026 deadline status |
| `getUpcomingDeadlines()` | Returns sorted upcoming deadlines |

---

## 6. Content Guidelines

### 6.1 Writing Style
- **Neutral tone** - Present both sides fairly
- **Plain English** - Avoid jargon
- **Concise** - Semicolon-separated lists for pros/cons
- **Source everything** - Always cite sources

### 6.2 "Why This Matters" (Problem Field)
This is the **key differentiator**. Each problem statement should:
1. State the specific issue in 1-2 sentences
2. Include concrete numbers when available
3. Explain why action is needed now

**Good example:**
> "Federal cuts to Medicaid and SNAP programs threaten to close county hospitals and reduce healthcare access for 1.9 million residents."

### 6.3 Beneficiaries vs. Funders
- **Beneficiaries:** Who directly benefits from the measure passing
- **Funders:** Who pays for it (taxpayers, specific groups, etc.)

---

## 7. Future Roadmap

### 7.1 2026 Election Preparation
- [ ] Add 2026 ballot measures as they qualify
- [ ] Track signature collection progress
- [ ] Display upcoming deadlines to users
- [ ] Add "Coming Soon" states based on schedule

### 7.2 Feature Ideas for Discussion
| Feature | Description | Priority |
|---------|-------------|----------|
| Filter by result | Show only approved/rejected | Medium |
| Filter by jurisdiction | Show only local/statewide | Medium |
| Filter by sponsor type | Show only grassroots, etc. | Low |
| Print/PDF export | Voter guide format | Medium |
| Email digest | Upcoming measures | Low |
| Compare measures | Side-by-side view | Low |
| Voting history | Track user's past votes | Low |

### 7.3 Data Collection Needs
For each new measure, collect:
1. Basic info (id, title, result)
2. Problem being solved
3. Who sponsored it and why
4. Who benefits / who pays
5. Key endorsements (3-5 per side)
6. Reliable source links

---

## 8. Success Metrics

### 8.1 User Engagement
- Time on site
- Number of measures viewed per session
- Search queries used
- "Learn More" expansion rate

### 8.2 Content Quality
- All measures have problem statements
- All measures have sources
- Detailed measures have endorsements
- All sources are linked

---

## 9. Known Limitations

1. **Single-page app** - No URL routing for direct links to measures
2. **No mobile-specific optimizations** - Responsive but not mobile-first
3. **Manual data entry** - No automated data collection
4. **No user accounts** - Cannot save preferences
5. **English only** - No internationalization

---

## 10. Changelog

| Date | Version | Changes |
|------|---------|---------|
| Jan 11, 2026 | 1.0 | Added beneficiaries/funders categories |
| Jan 11, 2026 | 1.0 | Added 2026 voting schedule for 23 states |
| Jan 2026 | 0.9 | Added clickable source links |
| Jan 2026 | 0.8 | Replaced SVG map with image + dots |
| Dec 2025 | 0.7 | Added "Sponsored by" labels |
| Dec 2025 | 0.6 | Added "Why This Matters" section |
| Nov 2025 | 0.5 | Visual redesign (Squero/Nest aesthetic) |
| Nov 2025 | 0.1 | Initial launch with 2025 data |

---

## Appendix A: Source Link Mapping

| Source Name | URL |
|-------------|-----|
| Ballotpedia | https://ballotpedia.org |
| Santa Cruz Local | https://santacruzlocal.org |
| Lookout Santa Cruz | https://lookoutlocal.com |

*Add more sources to `formatSourceLinks()` as needed.*

---

## Appendix B: 2026 Signature Deadlines (Sorted)

| Deadline | State | Signatures Required |
|----------|-------|---------------------|
| Jan 20 | Alaska | 34,098 |
| Feb 1 | Florida | 880,062 |
| Feb 2 | Maine | 67,682 |
| Feb 6 | Wyoming | 40,669 |
| Feb 15 | Utah | 140,748 |
| May 1 | Idaho | 70,725 |
| May 3 | Illinois | 328,371 |
| May 3 | Missouri | 170,215 / 106,384 |
| May 5 | South Dakota | 35,017 / 17,508 |
| Jun 17 | Massachusetts | 12,429 (round 2) |
| Jun 21 | Montana | 60,241 / 30,121 |
| Jun 24 | Nevada | 148,788 |
| Jun 25 | California | 874,641 / 546,651 |
| Jul 1 | Ohio | 413,487 |
| Jul 2 | Arizona | 383,923 / 255,949 |
| Jul 2 | Arkansas | 90,704 / 72,563 |
| Jul 2 | Nebraska | 10% / 7% registered voters |
| Jul 2 | Oregon | 156,231 / 117,173 |
| Jul 2 | Washington | 308,911 |
| Jul 6 | Michigan | 446,198 / 356,958 |
| Jul 6 | North Dakota | 31,164 / 15,582 |
| Aug 3 | Colorado | 124,238 |
| Aug 25 | Oklahoma | 172,993 / 92,263 |

**Note:** Texas does not have citizen initiative process. Mississippi's process is suspended.
