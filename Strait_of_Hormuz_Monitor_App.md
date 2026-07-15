Strait_of_Hormuz_Monitor

The Iran war began on 28 February 2026 after the United States and Israel conducted airstrikes on various Iranian military targets, including the
assassination of Ali Khamenei. On 4 March, Iran announced that the Strait of Hormuz was "closed," and threatened to attack any ship that attempted to pass it. 
A ceasefire attempt on April 8 was supposed to reopen the strait, but the US imposed a full naval blockade on April 13 after talks in Islamabad failed. 
On 17 June, Trump and Iranian president Masoud Pezeshkian signed a memorandum of understanding to end the war. Still, on 20 June, Iran declared that it had 
closed the strait again over Israeli strikes in Lebanon, and the blockade was reinstated on July 13 — just two days ago. Worldbank + 4 That timeline lines 
up exactly with what's in the data: outbound shipping collapses to near-zero right at the war's start, and as of the last data point (today, July 15), the 
7-day moving average for all three commodities — crude, LNG, and fertiliser — sits at exactly 0, against a prior-year baseline near 100.


In this project, I used data from the World Trade Organisation Data Lab to create a live picture of the situation.


What I built: a self-contained, interactive HTML dashboard (Strait_of_Hormuz_Monitor.html) — open it directly in any browser, no server needed:

Link to project https://strait-of-hormuz-monitor-er.netlify.app/

KPI cards were created for three commodities: LNG, Crude Oil and Fertiliser, clickable to switch the main chart as needed. 

Interactive chart toggling between daily/7-day-average views, with a dual-handle date slider to zoom into any period War timeline overlays
directly on the chart (dashed lines at each event) - plus a scrollable timeline strip below the dashboard. 


To interact with it:
Click the KPI cards (Crude Oil / LNG / Fertiliser) to switch which commodity the main chart shows.

Use the DAILY / 7D AVG toggle to switch between raw daily noise and the smoothened trend. Drag the two round handles on the slider 
below the chart to zoom into any date range — the chart updates live as you drag. Hover anywhere on the chart to see exact values 
in a tooltip The dashed vertical lines are war/ceasefire events — hover isn't wired to them individually. Still, the timeline strip 
below the chart lists them in order, left to right.


O
