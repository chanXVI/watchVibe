# Rolex Watch Collection & History App Map

## Purpose

Build a watch-focused app/site that helps users:

- Track a personal Rolex collection
- Document purchase and ownership history
- Explore Rolex model history and references
- Compare watches across eras, families, and specifications
- Organize notes, photos, service records, and valuation details

This document maps the first version of the product with Rolex as the starting brand.

## Product Vision

Create a Rolex knowledge and collection platform that blends three experiences:

1. Personal collection manager
2. Rolex reference archive
3. Ownership timeline and storytelling tool

The long-term goal is for a user to move easily between:

- "What Rolex watches do I own?"
- "What is the history of this reference?"
- "How has this model changed over time?"
- "When did I buy, service, wear, or sell this watch?"

## Core User Types

### 1. Collector

Wants to catalog owned Rolex watches, track value, and store details.

### 2. Researcher / Enthusiast

Wants to browse Rolex models, references, movements, case sizes, and production eras.

### 3. Seller / Buyer

Wants quick access to ownership history, condition, accessories, and market context.

### 4. Story-Driven Owner

Wants to record memories, milestones, and personal significance for each watch.

## Initial Rolex Scope

Start with a small, high-value set of Rolex families:

- Submariner
- GMT-Master / GMT-Master II
- Datejust
- Day-Date
- Explorer
- Explorer II
- Daytona
- Oyster Perpetual
- Sea-Dweller

This gives a strong mix of sports, dress, and iconic everyday Rolex models.

## Main Product Areas

## 1. Personal Collection

Each user can create entries for watches they own, owned, want, or are researching.

### Collection entry fields

- Brand
- Model family
- Reference number
- Nickname
- Year of production
- Serial range or serial notes
- Dial details
- Bezel type
- Bracelet / strap
- Case material
- Case size
- Movement / caliber
- Condition
- Box / papers included
- Purchase date
- Purchase source
- Purchase price
- Current estimated value
- Insurance value
- Service history
- Notes
- Photos
- Status: owned, sold, wishlist, researching

### Collection features

- Add/edit/delete watches
- Upload multiple photos
- Tag condition and completeness
- Track acquisition and sale events
- Filter by model, era, material, or status
- Sort by purchase date, estimated value, or model family

## 2. Rolex Reference Archive

This is the structured knowledge layer for Rolex.

### Rolex archive content per reference

- Model family
- Full reference number
- Production era
- Case size
- Material options
- Dial variations
- Bezel variations
- Bracelet options
- Movement / caliber
- Water resistance
- Notable design traits
- Historical significance
- Successor / predecessor references
- Common collector nicknames
- Image gallery
- Notes on rarity or known variants

### Example archive questions the app should answer

- What years was Rolex reference 16610 produced?
- What are the major differences between 16710 and 126710?
- Which Rolex Submariner references used aluminum bezels?
- Which Explorer models were 36mm versus 39mm versus 40mm?

## 3. Ownership Timeline

Each watch should have a timeline view.

### Timeline event types

- Purchased
- Worn for milestone
- Serviced
- Appraised
- Photographed
- Listed for sale
- Sold
- Inherited
- Modified

### Timeline value

- Builds personal history
- Helps with provenance
- Makes the app feel more emotional and narrative-driven
- Supports resale documentation and insurance readiness

## 4. Rolex History & Learning

This section explains Rolex as a brand and maps the evolution of major families.

### Content categories

- Rolex brand history
- Model family history
- Reference evolution
- Movement evolution
- Material evolution
- Design language changes
- Important releases by decade

### Example content ideas

- The evolution of the Rolex Submariner
- How GMT-Master became GMT-Master II
- Datejust dial and bezel variations over time
- The history of the Daytona chronograph
- Rolex tool watches of the 1970s to 2000s

## Suggested Information Architecture

### Top-level site/app sections

- Home
- My Collection
- Rolex Archive
- Timelines
- History
- Wishlist
- Compare
- Profile / Settings

### Suggested Rolex archive navigation

- By model family
- By decade
- By reference number
- By case size
- By complication
- By material

## Suggested Data Model

## Core entities

### Brand

- id
- name
- country
- founded_year
- summary

### ModelFamily

- id
- brand_id
- name
- category
- launch_year
- discontinued_year
- summary

### Reference

- id
- model_family_id
- reference_number
- nickname
- production_start_year
- production_end_year
- case_size_mm
- material
- movement
- water_resistance_m
- summary

### WatchInstance

- id
- user_id
- reference_id
- serial_notes
- production_year
- dial_details
- bezel_details
- bracelet_details
- condition
- box_papers_status
- purchase_price
- estimated_value
- insurance_value
- status
- notes

### Event

- id
- watch_instance_id
- event_type
- event_date
- title
- details
- amount

### Photo

- id
- watch_instance_id
- url_or_path
- caption
- taken_date

### ServiceRecord

- id
- watch_instance_id
- service_date
- provider
- description
- cost

## Rolex-Specific Starter Taxonomy

### Model families to seed first

- Submariner
- GMT-Master
- GMT-Master II
- Datejust
- Day-Date
- Explorer
- Explorer II
- Daytona
- Oyster Perpetual
- Sea-Dweller

### Helpful filters

- Sports
- Dress
- Professional
- Vintage
- Neo-vintage
- Modern
- Steel
- Two-tone
- Gold
- Platinum
- Time-only
- Date
- GMT
- Chronograph

## Key Screens for MVP

### 1. Dashboard

Shows:

- Total watches
- Total estimated collection value
- Recent timeline events
- Watches recently added
- Wishlist items

### 2. My Collection List

Shows:

- Card/list view of owned watches
- Filters for family, year, status, material
- Quick stats and thumbnail photos

### 3. Watch Detail Page

Shows:

- Hero photos
- Core watch specs
- Ownership timeline
- Service history
- Notes
- Related Rolex archive entry

### 4. Rolex Reference Detail Page

Shows:

- Historical overview
- Technical specs
- Known variants
- Related references
- Timeline of production

### 5. Compare Page

Shows:

- Side-by-side specs for two or more Rolex references
- Differences in movement, era, size, and materials

## MVP Recommendation

Start with a focused first release:

### MVP goals

- User can add Rolex watches to a collection
- User can browse seeded Rolex references
- User can view a watch detail page linked to a Rolex reference page
- User can log timeline events and service history
- User can filter collection entries by family and status

### Leave for later

- Market pricing automation
- Community/social features
- Dealer listings
- Authentication complexity beyond basic login
- Multi-brand support
- Advanced analytics

## Content Strategy for Rolex

To keep the project manageable, use three layers of content:

### Layer 1. Structured data

Facts such as reference number, years, size, movement, and materials.

### Layer 2. Editorial summaries

Short explanations of why a reference matters.

### Layer 3. Personal ownership data

Purchase history, notes, photos, and events tied to a user's actual watch.

This separation helps distinguish objective reference data from user-generated content.

## Future Enhancements

- Expand to Tudor, Omega, Seiko, Cartier, and Patek Philippe
- Add market value history
- Add import from spreadsheets or CSV
- Add watch wear log
- Add collection analytics by era, family, and value
- Add public/private collection sharing
- Add favorite references and saved searches
- Add article and media library

## Open Questions

- Will the app focus more on personal collection management or editorial watch history?
- Will reference data be manually curated first, or imported from a seed dataset?
- Should users track only owned watches, or also sold/wishlist/research entries from day one?
- Will each watch instance support highly detailed variant data, such as dial text and clasp codes?
- Should the first version be web-first, mobile-first, or responsive from the start?

## Recommended Next Step

Turn this Rolex map into three follow-up documents:

1. Feature requirements for MVP
2. Database schema draft
3. Seed data plan for initial Rolex references

If we keep going, the next best move is to define:

- 10 to 20 starter Rolex references
- the database tables
- the first user flows for add watch, browse reference, and log event
