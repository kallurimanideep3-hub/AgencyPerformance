# Agency Performance Dashboard

## Project Introduction

To assist agency heads in comprehending persistency, product contribution, APE achievement against objectives, and agent performance, this Power BI dashboard was designed for a regional life insurance firm.

There are four pages in the report:

1. Executive Summary: APE YTD, APE vs Target %, active agents, persistency rate, monthly APE vs target, top 10 agents by APE, region, channel, and year slicers.

2. Agent Performance: Deneb based agent performance visualization, APE by agent, APE vs Target, covers all agents over 8 pages, agent selection and drill through to the Agent Profile.

3. Profile of Agent: using HTML Content, Territory, YTD APE, APE Rank, MoM Growth, Persistency Rate, and Top Product by APE to create a dynamic agent profile.

4. Persistency Cohort: Use snapshot logic to do a cohort analysis based on issue year, determine the renewal rate for subsequent years, and count the active policies.

---

## Data Modeling Decisions

A star schema technique is used to design the data model.

###Tables with Dimensions

- dim_date

- dim_agent

- dim_product

### Table of facts

- fact_sales

- Fact_Target

- durability of facts

### Links

Date_key connects fact_sales to dim_date.

The dim_agent is related to:

- Sales fact

- target_fact

- fact persistence

dim_product is related to:

- sales fact

- fact_persistence

For continuity:

- active connection: dim_date[date_key] is issue_date_key

- The relationship between dim_date[date_key] and renewal_due_date_key is dormant.

This allows for analysis by renewal date if necessary without having unclear date relationships.

Because the target table contains monthly agent targets, the targets are handled separately from the daily sales dates rather than having a fact-to-fact relationship.

The source data has a few sales records with agent IDs that aren't included in dim_agent; these records haven't been quietly deleted from the source data.

---

## Using DAX

The report contains a set of actions that must be performed in order to satisfy the demands of the firm, such as:

- APE YTD

- MoM Growth in APE percentage

- Three months of APE Rolling

- APE vs Goal %

- Persistence rate

- snapshot of the number of active policies

- Number of active agents

- APE Agent Rank

### Snapshot of Active Policies

Instead of merely adding the entries, the active policy count is considered to be a semi-additive snapshot statistic.

The measure counts the unique policies while taking into account the policy issue dates, renewal status, and renewal due dates. It also determines the snapshot date and clears the normal date filter.

This prevents policy numbers from being incorrectly aggregated across several dates.

### Rate of persistence

Persistency is computed using the renewal status population:

`(Grace + Lapsed + Renewed) / Renewed`

Policies in force are not included in the denominator.

### Rank of Agent

Agent ranking is determined by APE and computed using the RANKX function across the selected agent population.

---

## Visual and UX Method

Where possible, native Power BI visuals are used in the dashboard.

### Deneb

Using Deneb, the Agent Performance page offers a unique agent performance visualization that includes:

- Comparison of APE

- Target-performance color coding

- Rank agents.

- Detailed tooltip information

- Select an agent.

- Agent navigation with multiple pages

### HTML Content

The Agent Profile page uses an HTML Content visual instead of a static image.

The profile changes automatically depending on the agent chosen or drilled through.

### Wireframe

The initial dashboard was created using:

`/mockups/page1_wireframe.html`

The final report prioritizes analytical visuals and a tidy executive layout, similar to the wireframe.

---

## Row-Level Security (RLS)

A function called:

Territory Manager

It has been established.

The position utilizes the territory field to limit access to the dim_agent table.

The View as tool in Power BI was used to test the position.

---

## Potential Enhancements

The dashboard may be enhanced with more time for development by adding:

- More advanced agent-level benchmarking

More targeted analysis and prognosis.

- More in-depth persistency cohort analysis

- From a single source, data is refreshed automatically.

- Add drill-through sites for territory and product analysis

- Enhanced mobile-layout optimization

- Better navigation and interaction for big agent populations.