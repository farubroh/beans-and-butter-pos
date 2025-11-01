<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Beans & Butter POS — Project Plan</title>
  <style>
    body{font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial;line-height:1.6;color:#0f172a;margin:28px;max-width:980px}
    h1,h2,h3{color:#0b1220}
    header{border-bottom:1px solid #e6edf3;padding-bottom:12px;margin-bottom:18px}
    pre{background:#0b1220;color:#e6edf3;padding:12px;border-radius:8px;overflow:auto}
    code{background:#f3f6f9;padding:2px 6px;border-radius:4px}
    .badge{display:inline-block;padding:4px 8px;border-radius:999px;background:#eef2ff;color:#1e3a8a;font-weight:600;margin-right:8px;font-size:12px}
    ul{margin-left:18px}
    section{margin-top:16px}
    .toc a{display:inline-block;margin-right:10px;margin-bottom:6px}
    .meta{font-size:14px;color:#334155}
    .note{background:#fff7ed;border-left:4px solid #f97316;padding:10px;border-radius:6px}
  </style>
</head>
<body>
  <header>
    <h1>Beans &amp; Butter POS System — Full Project Plan</h1>
    <p class="meta">Cloud-hosted, lightweight POS for Beans &amp; Butter Café — orders, printing, inventory, financials, and AI insights.</p>
    <div style="margin-top:10px">
      <span class="badge">Frontend: Angular PWA</span>
      <span class="badge">Backend: Spring Boot</span>
      <span class="badge">DB: Supabase (Postgres)</span>
      <span class="badge">AI: Python FastAPI (optional)</span>
    </div>
  </header>

  <section class="toc">
    <strong>Table of contents</strong>
    <div>
      <a href="#overview">Overview</a> •
      <a href="#deployment-plan">Deployment</a> •
      <a href="#core-requirements">Core Requirements</a> •
      <a href="#architecture">Architecture</a> •
      <a href="#roles">Roles &amp; Access</a> •
      <a href="#phases">Project Phases</a> •
      <a href="#ai-automation">AI &amp; Automation</a> •
      <a href="#next-steps">Next Steps</a>
    </div>
  </section>

  <section id="overview">
    <h2>Project Overview</h2>
    <p>
      Build a lightweight, cloud-hosted Point-of-Sale (POS) system for Beans &amp; Butter Café. The application
      is browser-based (PWA) so the café laptop only needs internet and a URL. Core goals: quick order entry,
      kitchen printing, accurate inventory and cost tracking, profit/loss reporting, and AI-driven operational insights.
    </p>
  </section>

  <section id="deployment-plan">
    <h2>Deployment Plan</h2>
    <p>
      Cloud-first architecture to keep the café laptop thin. Recommended free-tier services for development and small production:
    </p>
    <ul>
      <li><strong>Frontend:</strong> Netlify (Angular PWA build output).</li>
      <li><strong>Backend:</strong> Render or Railway (Spring Boot REST API).</li>
      <li><strong>Database:</strong> Supabase PostgreSQL (free tier available).</li>
      <li><strong>AI Module:</strong> Python FastAPI (host on Render / Railway or serverless functions).</li>
    </ul>
    <p class="note"><strong>Note:</strong> For production-readiness consider paid tiers for secure printing, higher DB limits, and SLA guarantees.</p>
  </section>

  <section id="core-requirements">
    <h2>Core Requirements</h2>
    <ol>
      <li><strong>POS Order Entry &amp; Printing:</strong> Fast order creation, ticketing, and kitchen/receipt printing.</li>
      <li><strong>Admin Panel:</strong> Menu management, pricing, user management, and reports.</li>
      <li><strong>Accountant Portal:</strong> Read-only financial dashboard for sales and P&amp;L review.</li>
      <li><strong>Execution Partner Workflow:</strong> Submit operational requests (e.g., vendor orders) — admin approval required.</li>
      <li><strong>AI Analytics:</strong> Sales forecasting, reorder suggestions, combo suggestions, anomaly detection, and chat insights.</li>
      <li><strong>Inventory Tracking:</strong> Track stock, costs (COGS), usage, and generate low-stock alerts.</li>
      <li><strong>Cloud-hosted Backend &amp; DB:</strong> All state stored in Supabase/Postgres; backend stateless.
      </li>
    </ol>
  </section>

  <section id="architecture">
    <h2>Architecture Overview</h2>
    <p>High level flow:</p>
    <pre><code>Angular PWA (browser)  —>  REST API (Spring Boot)  —>  Supabase (Postgres)
                     \
                      \--&gt; Optional AI Service (Python FastAPI) —&gt; Worker / Batch Jobs</code></pre>
    <h3>Components</h3>
    <ul>
      <li><strong>Angular PWA</strong> — POS UI, offline-capable cache, responsive layout for laptop or tablet.</li>
      <li><strong>Spring Boot REST API</strong> — Auth, order management, inventory, printing endpoints, role-based access control.</li>
      <li><strong>Supabase</strong> — Auth (Supabase Auth), Postgres DB, Storage for receipts or attachments, real-time subscriptions.</li>
      <li><strong>AI Module (optional)</strong> — Forecasting and recommendations served via FastAPI endpoints (can be called by backend or admin UI).</li>
      <li><strong>Print Flow</strong> — Use browser print API for receipts or integrate cloud printing via webhook to a local print agent if offline printing is required.</li>
    </ul>
  </section>

  <section id="roles">
    <h2>Roles &amp; Access Control</h2>
    <ul>
      <li><strong>Admin</strong> — Full access, can approve execution partner requests and manage users.</li>
      <li><strong>Execution Partner</strong> — Can submit operational requests; requires Admin approval to execute.</li>
      <li><strong>Accountant</strong> — Read-only access to financials and P&amp;L.</li>
      <li><strong>Employee</strong> — Create orders, process payments, print receipts.</li>
      <li><strong>Chef</strong> — Kitchen view to monitor active tickets (no financial controls).</li>
    </ul>
    <p>Enforce role-based access via Spring Security + Supabase JWT verification; sync roles into DB.</p>
  </section>

  <section id="phases">
    <h2>Project Phases</h2>
    <ol>
      <li>
        <strong>Phase 1 — Core POS &amp; Authentication</strong>
        <ul>
          <li>Angular PWA skeleton, Supabase Auth integration, order creation, basic receipts, and user roles.</li>
        </ul>
      </li>

      <li>
        <strong>Phase 2 — Menu &amp; Inventory Management</strong>
        <ul>
          <li>Spring Boot API for menu, price, inventory items, and stock adjustments. Webhooks for printing and real-time order sync.</li>
        </ul>
      </li>

      <li>
        <strong>Phase 3 — Accountant &amp; Execution Partner Portals</strong>
        <ul>
          <li>Financial views (daily/weekly/monthly sales), approvals workflow, and accountant read-only dashboards.</li>
        </ul>
      </li>

      <li>
        <strong>Phase 4 — AI Insights</strong>
        <ul>
          <li>Sales forecasting, low-stock detection, combo suggestions, anomaly detection, and an admin chat interface that queries the AI service.</li>
        </ul>
      </li>

      <li>
        <strong>Phase 5 — Reporting Dashboard &amp; Final Polish</strong>
        <ul>
          <li>Final visual polish, performance tuning, automated backups, and documentation.</li>
        </ul>
      </li>
    </ol>
  </section>

  <section id="ai-automation">
    <h2>AI &amp; Automation Features</h2>
    <ul>
      <li>Predict top-selling items and busiest hours (time-series forecasting).</li>
      <li>Detect low stock and recommend vendor reorder quantities (use safety stock rules).</li>
      <li>Suggest combo or upsell items based on association rules or collaborative filtering.</li>
      <li>Detect unusual patterns in profit/loss (anomaly detection).</li>
      <li>Chat-based insights for admin (question-answering over recent sales &amp; inventory).</li>
    </ul>
    <p>ML can run as a separate scheduled job or on-demand via FastAPI endpoints. Start with simple models (moving averages, ARIMA, or Prophet) before moving to more complex models.</p>
  </section>

  <section id="next-steps">
    <h2>Next Steps</h2>
    <ol>
      <li>Create a GitHub repository for the full-stack code (mono-repo or multi-repo; recommended: mono-repo with `frontend/` and `backend/`).</li>
      <li>Set up free hosting accounts: Netlify (frontend), Render/Railway (backend &amp; AI), Supabase (DB &amp; Auth).</li>
      <li>Implement Phase 1 MVP: login, POS UI, order creation, and simple receipt printing.</li>
      <li>Iterate adding inventory, accountant portal, and execution partner workflows.</li>
      <li>Integrate AI service and build admin chat &amp; recurring analytics jobs.</li>
    </ol>

    <h3>Developer checklist (starter)</h3>
    <pre><code>git init
mkdir frontend backend
# frontend: Angular PWA scaffold
ng new frontend --style=scss --routing
# backend: Spring Boot starter
# configure Supabase connection &amp; Supabase Auth
</code></pre>
  </section>

  <section id="appendix">
    <h2>Appendix — Useful links &amp; tips</h2>
    <ul>
      <li>Netlify docs: deploy Angular apps</li>
      <li>Render docs: deploy Spring Boot and FastAPI services</li>
      <li>Supabase docs: Auth, Postgres, and Realtime</li>
      <li>Printing: browser.print() for receipts; for kitchen thermal printers consider a local print-agent or cloud printer bridge.</li>
    </ul>
    <p style="margin-top:8px">If you want, I can also generate a Markdown README.md version or create the initial repository structure with starter files.</p>
  </section>

  <footer style="margin-top:22px;border-top:1px solid #eef2f7;padding-top:12px;color:#475569;font-size:14px">
    Created for <strong>Beans &amp; Butter Café</strong>. — Save this file as <code>README.html</code> in your GitHub repo or convert it to <code>README.md</code> for GitHub's default rendering.
  </footer>
</body>
</html>
