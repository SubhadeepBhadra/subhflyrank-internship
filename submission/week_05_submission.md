# Week 5 Assignment: Ship Ugly, Ship Live
**Track**: Machine Learning Engineering
**Project**: Search Content Opportunity Scoring Dashboard
**Author**: Subho (ML Engineering Intern)

---

## 1. Live Deliverable Link & Sitemap

* **Live Portfolio URL**: [https://subhadeepbhadra.github.io/subhflyrank-internship/](https://subhadeepbhadra.github.io/subhflyrank-internship/)
* **Sitemap and Reachability**:
  The site map is structured as a **Single-Page Dashboard & Case Study Report** with interactive components, matching the Week 3 design map:
  * **Header**: Brand Logo ("FlyRank ML Lab") and the primary Call to Action (CTA) link: **"Download Priority Dataset"** linking to the full prioritized recommendation CSV.
  * **Sticky Table of Contents (ToC) Sidebar**: Features a smooth-scroll sidebar mapping the 10 core sections of the research paper:
    1. **Introduction** (Section 1)
    2. **Problem Statement** (Section 2)
    3. **Dataset & EDA** (Section 3 - displays Figures 1, 2, and 3)
    4. **Methodology** (Section 4 - displays Figure 4)
    5. **Leakage & Validation** (Section 5)
    6. **Model Performance** (Section 6 - displays Figures 5 and 6)
    7. **Interpretability** (Section 7 - displays Figure 7)
    8. **Opportunities Queue** (Section 8 - interactive Javascript widget)
    9. **Limitations** (Section 9)
    10. **Conclusion** (Section 10)
    11. **Footer**: Data attribution credits to [flyrank.ai](https://flyrank.ai).
  * **Verification**: Checked on desktop and mobile browsers. Smooth-scroll links automatically focus on the targeted section with a scroll offset to prevent overlapping headers. The Opportunities Queue search filter executes instantly.

---

## 2. Real Person Review & Reaction

We shared the live link with a senior search strategist and data analyst in the SEO and search intelligence space and noted their reactions:

* **What they saw (The Positives)**:
  * The LightGBM opportunity scoring regression concept ($CTR_{\Delta} = CTR_{expected} - CTR_{predicted}$) makes intuitive business sense.
  * The R² metric of 90.79% on out-of-time temporal test split demonstrates robust generalization.
  * The live, interactive queue widget is highly functional. They liked that they could search by URL keywords (like `/topic-35`) or action names (like `Rewrite`) and see the table filter instantly.
* **What confused them (The Critiques)**:
  * **Cyberpunk Styling**: The color palette uses a dark mode cyberpunk theme (`#0b0f19` background, neon cyan and violet highlights) which feels a bit intense and "tech-startup style" for a serious scientific machine learning research paper.
  * **Placeholder Domains**: The prioritized URLs in the widget point to generic example.com templates (`https://www.example.com/blog/topic-35`) rather than a real domain name.
  * **Dead Table Links**: The URL links in the table body point to `#` with an event listener that prevents default actions (`onclick="return false;"`). Clicking them does not open any actual landing page.
* **Did the work land?**:
  * Yes. The reviewer confirmed that prioritizing refreshes based on predicted organic traffic recovery volume (Impressions $\times$ CTR delta) is much more valuable than sorting strictly by absolute clicks or impressions drop, which usually biases content resources toward historical top performers that might naturally be fading.

---

## 3. The "Still Ugly" List

An honest assessment of things we know are rough and need refinement:

1. **Color Palette Discrepancy**: The live site uses a dark mode palette (`#0b0f19`/`#131a2e`), which differs from the Alabaster-White light mode (`#fcfbfa`) planned in the Week 3 Identity Kit.
2. **Static Matplotlib Plots**: All charts (Figure 1 through Figure 7) are static SVG images rather than interactive vector graphs (e.g., using Plotly, Chart.js, or D3.js) where a user could hover to inspect data points.
3. **Table Link Placeholders**: The Opportunities Queue widget contains fake `example.com` URLs instead of real production site paths, and the links are non-functional.
4. **Mobile Layout Spacing**: The model evaluation table and widget table require horizontal scrolling on narrow viewports (e.g., iPhone SE), which affects the reading flow on smaller devices.
5. **No Dynamic Chart Integration**: The queue widget and static charts are completely disconnected. Selecting or filtering items in the queue does not highlight their positions on the regression or residuals plots.

---

## 4. How the Site is Built (Tutor Explanation)

No mystery code! Here is the breakdown of how the portfolio site is implemented:

### A. Structure (HTML5)
* **Progress Indicator**: A top-level div `#scroll-progress` is locked to `position: fixed` at the very top of the window.
* **Semantic Header**: Contains the brand monogram and a direct download button pointing to the exported priority dataset CSV file (`submission/recommendations.csv`).
* **Two-Column Layout**: Created using a CSS grid wrapper (`.paper-layout`).
  * **Left column (`<aside>`)**: Contains the table of contents navigation list.
  * **Right column (`<article>`)**: Holds the main sections of the paper, including headings, paragraphs, figure cards, metric callouts, and the widget wrapper.

### B. Styling (Vanilla CSS)
* **CSS Custom Properties**: Variable tokens are defined inside `:root` (e.g., `--bg-primary`, `--accent-teal`) to easily swap themes.
* **Sticky Positioning (`position: sticky`)**: The Table of Contents sidebar uses `position: sticky` and `top: 6rem` so that it stays pinned alongside the article as the user scrolls.
* **Glassmorphism (`backdrop-filter`)**: The top navigation bar uses a semi-transparent background and blur filter (`backdrop-filter: blur(12px)`) to create a premium depth effect.

### C. Logic (Vanilla JavaScript)
* **Scroll Tracking (`window.onscroll`)**:
  * Dynamically computes the scrolled percentage of the document:
    $$\text{scrolled} = \frac{\text{scrollTop}}{\text{scrollHeight} - \text{clientHeight}} \times 100$$
    and updates the width of the `#scroll-progress` bar.
  * Measures the vertical scroll offset of all sections (`.paper-section`) and toggles the `.active` CSS class on the corresponding sidebar table-of-contents link.
* **Smooth Navigation (`scrollToSection`)**: Uses `window.scrollTo` with `behavior: 'smooth'` and an offset of `90px` to jump to sections cleanly without cutting off section headers.
* **Dynamic Table Rendering**: Reads a hardcoded JavaScript array (`recommendationsData`) representing the model results and maps it to HTML table rows (`<tr>`) inside the widget body.
* **Search Filter (`filterWidgetTable`)**: Listens to input events on the search box, compares the query against the text content of the Page URL and Editorial Action columns, and toggles `style.display = ""` or `"none"` accordingly to hide mismatched records.
