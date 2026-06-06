Notes on the USAID Africa FY2024 → FY2025 dashboard

These notes describe what the dashboard does and doesn't show, how the numbers are computed, and the assumptions a reader should keep in mind before drawing conclusions from it.
What the dashboard shows
The dashboard compares USAID spending across African countries between fiscal years 2024 and 2025. It can display two distinct spending measures, switchable via a toggle at the top of the page: disbursements (money that actually went out the door in a given fiscal year) and obligations (money the agency committed to spend, which may or may not have been disbursed in the same year). These are different concepts that don't move together, a sharp drop in obligations in one year can show up as a drop in disbursements one or two years later, and vice versa.
All amounts are in current US dollars. No inflation adjustment has been applied between FY2024 and FY2025. For a one-year comparison this is usually fine.
The underlying data comes from foreignassistance.gov and was filtered to: managing agency equal to USAID, transaction type equal to Disbursements or Obligations, fiscal year 2024 or 2025, and country code in the set of 53 African countries (using the standard ISO-3 codes). Any spending recorded under a non-USAID managing agency is not included here. Comparisons to total US foreign assistance figures from other dashboards or news reports will therefore not match.



What each part of the dashboard responds to

The page has two filter dropdowns (Region and Country), one mode toggle (Disbursements / Obligations), and a clickable map. They interact as follows.
The bar chart and the program table respond to all three filters together. Selecting a country narrows both to that country only. Selecting a region without a country narrows them to the aggregate for that region. The map and the four summary cards do not narrow with filters — they always show the Africa-wide picture for the current mode. 
Clicking a country on the map is equivalent to picking it from the country dropdown, and it also updates the region dropdown to reflect that country's region so the rest of the filter state stays consistent. Choosing "All Africa" in the country dropdown clears the country selection while keeping the region; choosing "All regions" in the region dropdown clears both.



How the percentage change is computed

Percentage changes are computed as (FY2025 − FY2024) / FY2024 × 100, rounded to one decimal place. Two methodology choices are worth flagging.
First, the inputs to that calculation are rounded to two decimals before the percentage is computed. This is so that the displayed amounts and the displayed percentage stay internally consistent — without this step it was possible to see a row showing 0.1 → 0.1 → +100%, which read as a bug even though the underlying ungrounded values were genuinely 0.05 → 0.10. Rounding the inputs first means very small movements at the sub-$0.01M level are no longer reflected in the percentage.
Second, when the FY2024 base value is zero the percentage is reported as n/a rather than Inf or +100%. A change from zero to anything positive isn't really a percentage increase — it's a new line of spending, and it's misleading to compare it on the same scale as proportional changes.
Reading the choropleth map
The map's color scale runs from 0% to −100%, with darker red indicating sharper declines in disbursements or obligations from FY2024 to FY2025. A country whose spending grew will sit at the top of the scale and appear in the lightest color. The scale is asymmetric on purpose: declines dominated the FY2024 → FY2025 period, so a symmetric scale around zero would waste most of the color range on values that don't appear in the data.
Hover text on each country shows the two-year amounts and the percentage change. A country showing "n/a" had no recorded spending in FY2024 under the relevant transaction type, so a percentage change can't be defined.
Reading the bar chart
The bar chart shows USAID spending by sector for whatever scope is currently selected (All Africa, a region, or a country). Each bar's height is the sector total for that scope and year — and hovering shows that exact total. Sectors are sorted with the largest at the top and ordered by combined FY2024 + FY2025 total within the current scope.
The bar chart applies a $5M floor: any sector whose Africa-wide total across both years and both modes is under $5M is dropped from the chart entirely. This is to keep the chart readable when small categories would otherwise sit at the bottom as near-invisible slivers. The floor is not based on the per-selection total. So if you filter to a small country that has $0.5M in a sector whose Africa-wide total is $200M, that sector will still appear in the bar with a small value. Conversely, a sector that's under $5M continent-wide is hidden from the bar even when looking at countries where it represents the bulk of spending.
That floor never affects the program table, the map, or the summary cards. Small sectors are always visible in the table.



Reading the program table

The table lists every USAID program ("Activity Name" field in the source data) for the current filter scope, grouped first by country, then by sector, sorted by FY2024 amount descending within each sector. Amounts are in current millions of US dollars, rounded to two decimals. The table scrolls vertically inside its panel for long lists.



Region classification

The five regions used in the dashboard follow the UN subregion classification: Northern (Algeria, Egypt, Libya, Morocco, Sudan, Tunisia), Western (Benin, Burkina Faso, Cabo Verde, Côte d'Ivoire, Gambia, Ghana, Guinea, Guinea-Bissau, Liberia, Mali, Mauritania, Niger, Nigeria, Senegal, Sierra Leone, Togo), Eastern (Burundi, Comoros, Djibouti, Eritrea, Ethiopia, Kenya, Madagascar, Malawi, Mauritius, Mozambique, Rwanda, Somalia, South Sudan, Tanzania, Uganda, Zambia, Zimbabwe), Central (Angola, Cameroon, Central African Republic, Chad, Democratic Republic of the Congo, Republic of the Congo, Equatorial Guinea, Gabon, São Tomé and Príncipe), and Southern (Botswana, Eswatini, Lesotho, Namibia, South Africa).
Two assignments worth noting because they sometimes go the other way in different classifications: Sudan is in Northern (not Eastern), and South Sudan is in Eastern (not Northern). This follows the UN's M.49 standard.



Caveats and limitations

The dashboard is a static HTML file. It reflects the data as it existed when the R script was run and embedded into the file. It does not refresh on its own and is not connected to any live data source. If foreignassistance.gov publishes corrections or backfills the data, the dashboard will not pick those up until the script is re-run. 
The data was downloaded on Thursday, ‎May ‎7, ‎2026, ‏‎12:51:25 PM Chicago Time, roughly seven months after FY2025 closed on September 30, 2025. By that point USAID's reporting on foreignassistance.gov should be substantially complete for FY2025, so the dashboard reflects the settled picture of the fiscal year rather than a mid-year snapshot. Minor revisions can still appear in the source after that but the headline patterns shown here should be stable. Comparisons between FY2024 and FY2025 are reliable in both relative and absolute terms within the limits of that residual reporting lag.
The summary cards at the top of the page show Africa-wide totals for the active mode regardless of which country or region is selected. They're a fixed reference for the overall picture, not a filtered readout. If you want the totals for a specific country or region, those can be read off the bar chart or the table.
Programs in the table are aggregated at the activity level. A single program is a row, summed across whatever sub-activities or transactions appear under that activity name in the source data.



A note on the file

The dashboard's HTML file ships alongside a lib/ (or lib_dashboard/) directory containing the Plotly and htmlwidgets assets needed to render the interactive charts. When sharing the dashboard, both must travel together — the HTML file alone won't render.
The R script that builds the dashboard is parameterized only by the working directory and the input CSV path; rerunning it on an updated source CSV will produce an updated dashboard with the same structure and methodology.

