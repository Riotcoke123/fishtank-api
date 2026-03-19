<!DOCTYPE html>
<html lang="en">
<head>

<div class="wrapper"  ![logo-stripe](https://github.com/user-attachments/assets/208cca27-9850-44e6-9f19-1512fa060edc)
  <!-- ── HEADER ── -->
  <header>
    <div class="logo-line">
      <h1>fishtank.live</h1>
    </div>
    <p class="tagline">API Reference &amp; Integration Guide</p>
    <div class="badges">
      <span class="badge live"><span class="dot"></span> Live Season Active</span>
      <span class="badge api">REST API v1</span>
      <span class="badge season">Season 3</span>
    </div>
  </header>

  <!-- ── TOC ── -->
  <nav class="toc">
    <div class="toc-title">// Table of Contents</div>
    <ol>
      <li><a href="#overview">Overview</a></li>
      <li><a href="#auth">Authentication — <code>/v1/auth</code></a></li>
      <li><a href="#chat">Chat Settings — <code>/v1/settings/chat</code></a></li>
      <li><a href="#contestants">Contestant Cams — <code>/v1/contestants/cams</code></a></li>
      <li><a href="#challenges">Challenges — <code>/v1/challenges</code></a></li>
      <li><a href="#usage">Usage Notes</a></li>
    </ol>
  </nav>

  <!-- ── OVERVIEW ── -->
  <section id="overview">
    <div class="section-label">// 01 — Overview</div>
    <h2>What is fishtank.live?</h2>
    <p>
      <strong>fishtank.live</strong> is a 24/7 interactive live-stream reality show where contestants live together under constant camera surveillance. Viewers can interact in real time, vote on challenges, and influence the house via the web app.
    </p>
    <p>
      The platform exposes a REST API at <code>https://api.fishtank.live/v1</code> that powers the web and mobile clients. This document describes the endpoints observed from live traffic.
    </p>
    <div class="callout warn">
      <span class="callout-icon">⚠️</span>
      <span>This is an <strong>unofficial</strong> API reference derived from observed network traffic. Endpoints are undocumented and may change without notice.</span>
    </div>
    <h3>Base URL</h3>
    <pre><code>https://api.fishtank.live/v1</code></pre>
    <h3>Auth Scheme</h3>
    <p>All authenticated endpoints require a <code>Bearer</code> token in the <code>Authorization</code> header, obtained from <code>/v1/auth</code>.</p>
    <pre><code>Authorization: Bearer &lt;access_token&gt;</code></pre>
  </section>

  <hr />

  <!-- ── AUTH ── -->
  <section id="auth">
    <div class="section-label">// 02 — Authentication</div>
    <h2>Auth Endpoint</h2>
    <div class="endpoint fade-in">
      <div class="endpoint-header">
        <span class="method post">POST</span>
        <span class="endpoint-path">/v1/auth</span>
        <span class="endpoint-desc">Authenticate and obtain session tokens</span>
      </div>
      <div class="endpoint-body">
        <p>Returns a full session object including JWT access token, refresh token, live stream token, and user profile metadata.</p>
        <h3>Response Fields</h3>
        <table class="field-table">
          <thead>
            <tr><th>Field</th><th>Type</th><th>Description</th></tr>
          </thead>
          <tbody>
            <tr><td>session.access_token</td><td>string</td><td>JWT — short-lived bearer token for API calls (expires ~15 min)</td></tr>
            <tr><td>session.refresh_token</td><td>string</td><td>JWT — long-lived refresh token (~30 days)</td></tr>
            <tr><td>session.expires_in</td><td>number</td><td>Token lifetime in seconds</td></tr>
            <tr><td>session.live_stream_token</td><td>string</td><td>JWT — scoped token for live stream access (~24h)</td></tr>
            <tr><td>session.token_type</td><td>string</td><td>Always <code>"Bearer"</code></td></tr>
            <tr><td>session.user.id</td><td>uuid</td><td>Unique user identifier</td></tr>
            <tr><td>session.user.email</td><td>string</td><td>Account email address</td></tr>
            <tr><td>session.user.user_metadata.displayName</td><td>string</td><td>Public chat display name</td></tr>
            <tr><td>session.user.user_metadata.color</td><td>hex</td><td>Chat username color</td></tr>
            <tr><td>session.user.user_metadata.photo</td><td>url</td><td>Profile picture URL (CDN hosted)</td></tr>
            <tr><td>session.user.user_metadata.seasonPass</td><td>boolean</td><td>Whether user holds an active season pass</td></tr>
          </tbody>
        </table>
        <h3>Example Response</h3>
        <pre><code>{
  <span class="json-key">"session"</span>: {
    <span class="json-key">"access_token"</span>: <span class="json-str">"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."</span>,
    <span class="json-key">"refresh_token"</span>: <span class="json-str">"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."</span>,
    <span class="json-key">"expires_in"</span>: <span class="json-num">2592000</span>,
    <span class="json-key">"token_type"</span>: <span class="json-str">"Bearer"</span>,
    <span class="json-key">"live_stream_token"</span>: <span class="json-str">"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."</span>,
    <span class="json-key">"user"</span>: {
      <span class="json-key">"id"</span>: <span class="json-str">"ad6a3a2e-9b24-4918-8b03-44026b36bc24"</span>,
      <span class="json-key">"email"</span>: <span class="json-str">"user@example.com"</span>,
      <span class="json-key">"role"</span>: <span class="json-str">"authenticated"</span>,
      <span class="json-key">"user_metadata"</span>: {
        <span class="json-key">"displayName"</span>: <span class="json-str">"WeakCove"</span>,
        <span class="json-key">"color"</span>: <span class="json-str">"#ffffff"</span>,
        <span class="json-key">"photo"</span>: <span class="json-str">"https://cdn.fishtank.live/images/profile-small.gif"</span>,
        <span class="json-key">"seasonPass"</span>: <span class="json-bool">false</span>
      }
    }
  }
}</code></pre>
      </div>
    </div>
  </section>

  <hr />
  <!-- ── CHAT ── -->
  <section id="chat">
    <div class="section-label">// 03 — Chat Settings</div>
    <h2>Chat Settings</h2>
    <div class="endpoint fade-in">
      <div class="endpoint-header">
        <span class="method get">GET</span>
        <span class="endpoint-path">/v1/settings/chat</span>
        <span class="endpoint-desc">Fetch current user chat preferences</span>
      </div>
      <div class="endpoint-body">
        <p>Returns the authenticated user's chat moderation preferences including blocked users, word filters, and mute status.</p>
        <table class="field-table">
          <thead>
            <tr><th>Field</th><th>Type</th><th>Description</th></tr>
          </thead>
          <tbody>
            <tr><td>blockedUsers</td><td>array&lt;string&gt;</td><td>List of blocked user IDs</td></tr>
            <tr><td>wordFilters</td><td>array&lt;string&gt;</td><td>Custom filtered words/phrases</td></tr>
            <tr><td>censored</td><td>boolean</td><td>Global profanity filter toggle</td></tr>
            <tr><td>muted</td><td>string | null</td><td>Muted-until timestamp, or <code>null</code> if not muted</td></tr>
          </tbody>
        </table>
        <h3>Example Response</h3>
        <pre><code>{
  <span class="json-key">"blockedUsers"</span>: <span class="json-str">[]</span>,
  <span class="json-key">"wordFilters"</span>:  <span class="json-str">[]</span>,
  <span class="json-key">"censored"</span>:    <span class="json-bool">false</span>,
  <span class="json-key">"muted"</span>:       <span class="json-null">null</span>
}</code></pre>
      </div>
    </div>
  </section>

  <hr />

  <!-- ── CONTESTANTS ── -->
  <section id="contestants">
    <div class="section-label">// 04 — Contestant Cams</div>
    <h2>Contestant Cams</h2>
    <div class="endpoint fade-in">
      <div class="endpoint-header">
        <span class="method get">GET</span>
        <span class="endpoint-path">/v1/contestants/cams</span>
        <span class="endpoint-desc">Live scan of all contestant positions &amp; states</span>
      </div>
      <div class="endpoint-body">
        <p>Returns a real-time scan of all current-season contestants with their activity, mood, and camera position. Data is keyed by contestant ID.</p>
        <div class="callout info">
          <span class="callout-icon">ℹ️</span>
          <span>Contestants with <code>null</code> action/mood/position are currently off-camera or not in an active area.</span>
        </div>
        <table class="field-table">
          <thead>
            <tr><th>Field</th><th>Type</th><th>Description</th></tr>
          </thead>
          <tbody>
            <tr><td>scan.timestamp</td><td>ISO 8601</td><td>Server-side time of the snapshot</td></tr>
            <tr><td>scan.data[id].name</td><td>string</td><td>Contestant display name</td></tr>
            <tr><td>scan.data[id].action</td><td>string | null</td><td>Current activity (e.g. <code>"chatting"</code>, <code>"sitting"</code>, <code>"standing"</code>)</td></tr>
            <tr><td>scan.data[id].mood</td><td>string | null</td><td>Detected emotional state (e.g. <code>"happy"</code>, <code>"calm"</code>)</td></tr>
            <tr><td>scan.data[id].position</td><td>array | null</td><td>Camera room ID(s) the contestant is in</td></tr>
            <tr><td>scan.data[id].avatar</td><td>string</td><td>Avatar filename (hosted on CDN)</td></tr>
          </tbody>
        </table>
        <h3>Known Camera Positions</h3>
        <pre><code><span class="json-str">"brrr-5"</span>  <span class="json-comment">// — Main house room (Season 3 active zone)</span></code></pre>
        <h3>Live Snapshot — Season 3 Cast</h3>
        <p style="font-size:13px;color:var(--muted);">Data captured: <code>2026-03-18T00:09:14Z</code></p>
        <div class="contestant-grid">
          <div class="contestant-card fade-in">
            <div class="c-id">#105</div>
            <div class="c-name">Victoria</div>
            <span class="c-tag action">standing</span>
            <span class="c-tag mood">calm</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#106</div>
            <div class="c-name">Bashir</div>
            <span class="c-tag action">chatting</span>
            <span class="c-tag mood">happy</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#107</div>
            <div class="c-name">JD</div>
            <span class="c-tag action">chatting</span>
            <span class="c-tag mood">happy</span>
          </div>
          <div class="contestant-card fade-in offline">
            <div class="c-id">#108</div>
            <div class="c-name">Bezo</div>
            <span class="c-tag offline-tag">off-cam</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#109</div>
            <div class="c-name">James Drake</div>
            <span class="c-tag action">chatting</span>
            <span class="c-tag mood">happy</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#110</div>
            <div class="c-name">Landon</div>
            <span class="c-tag action">chatting</span>
            <span class="c-tag mood">happy</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#111</div>
            <div class="c-name">The Twins</div>
            <span class="c-tag action">sitting</span>
            <span class="c-tag mood">calm</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#112</div>
            <div class="c-name">Emma</div>
            <span class="c-tag action">standing</span>
            <span class="c-tag mood">calm</span>
          </div>
          <div class="contestant-card fade-in">
            <div class="c-id">#113</div>
            <div class="c-name">Anissa</div>
            <span class="c-tag action">sitting</span>
            <span class="c-tag mood">calm</span>
          </div>
          <div class="contestant-card fade-in offline">
            <div class="c-id">#114</div>
            <div class="c-name">Charity</div>
            <span class="c-tag offline-tag">off-cam</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <hr />

  <!-- ── CHALLENGES ── -->
  <section id="challenges">
    <div class="section-label">// 05 — Challenges</div>
    <h2>Challenges</h2>
    <div class="endpoint fade-in">
      <div class="endpoint-header">
        <span class="method get">GET</span>
        <span class="endpoint-path">/v1/challenges</span>
        <span class="endpoint-desc">Daily and global challenge status</span>
      </div>
      <div class="endpoint-body">
        <p>Returns the current state of all active challenges, including daily challenges with completion status and expiry time, plus any active global challenge.</p>
        <table class="field-table">
          <thead>
            <tr><th>Field</th><th>Type</th><th>Description</th></tr>
          </thead>
          <tbody>
            <tr><td>dailyChallenges.inProgress</td><td>array&lt;string&gt;</td><td>Slugs of currently active daily challenges</td></tr>
            <tr><td>dailyChallenges.complete</td><td>array&lt;string&gt;</td><td>Slugs of completed daily challenges</td></tr>
            <tr><td>dailyChallenges.expiresAt</td><td>unix ms</td><td>Expiry timestamp for the current daily challenge set</td></tr>
            <tr><td>globalChallenge.globalChallenge</td><td>object | null</td><td>Active global challenge data, or <code>null</code></td></tr>
            <tr><td>globalChallenge.status</td><td>string | null</td><td>Status of the global challenge</td></tr>
          </tbody>
        </table>
        <h3>Live Challenge State</h3>
        <div class="challenge-list">
          <div class="challenge-item">
            <span class="challenge-icon">🎤</span>
            <span class="challenge-name">speaker-spammer</span>
            <span class="challenge-status status-progress">IN PROGRESS</span>
          </div>
          <div class="challenge-item">
            <span class="challenge-icon">🔤</span>
            <span class="challenge-name">JJJJJJJJJJ</span>
            <span class="challenge-status status-progress">IN PROGRESS</span>
          </div>
          <div class="challenge-item">
            <span class="challenge-icon">🌍</span>
            <span class="challenge-name">global challenge</span>
            <span class="challenge-status status-none">NONE ACTIVE</span>
          </div>
        </div>
        <h3>Example Response</h3>
        <pre><code>{
  <span class="json-key">"dailyChallenges"</span>: {
    <span class="json-key">"inProgress"</span>: [<span class="json-str">"speaker-spammer"</span>, <span class="json-str">"JJJJJJJJJJ"</span>],
    <span class="json-key">"complete"</span>:    [],
    <span class="json-key">"expiresAt"</span>:   <span class="json-num">1773858738144</span>
  },
  <span class="json-key">"globalChallenge"</span>: {
    <span class="json-key">"globalChallenge"</span>: <span class="json-null">null</span>,
    <span class="json-key">"status"</span>:         <span class="json-null">null</span>
  }
}</code></pre>
      </div>
    </div>
  </section>

  <hr />

  <!-- ── USAGE NOTES ── -->
  <section id="usage">
    <div class="section-label">// 06 — Usage Notes</div>
    <h2>Usage Notes</h2>
    <h3>Token Lifecycle</h3>
    <p>The <code>access_token</code> is short-lived. Use the <code>refresh_token</code> to obtain a new session before expiry. The <code>live_stream_token</code> is scoped exclusively to the video stream and has a ~24-hour TTL.</p>
    <div class="callout danger">
      <span class="callout-icon">🔒</span>
      <span>Never commit access tokens or refresh tokens to source control. Treat them like passwords — they identify your account and grant full chat/stream access.</span>
    </div>
    <h3>CDN Assets</h3>
    <p>Profile images and contestant avatars are served from <code>https://cdn.fishtank.live/</code>. Avatar filenames follow the pattern <code>{id}-{mood}.png</code> (e.g. <code>105-happy.png</code>), or <code>{id}.png</code> for off-cam contestants.</p>
    <h3>Polling the Cam Endpoint</h3>
    <p>The <code>/v1/contestants/cams</code> endpoint returns a timestamped snapshot. To track live position changes, poll at a reasonable interval (suggested: every 5–10 seconds) to avoid rate limits.</p>
    <div class="info-grid">
      <div class="info-card">
        <div class="info-card-label">Base API URL</div>
        <div class="info-card-value">https://api.fishtank.live/v1</div>
      </div>
      <div class="info-card">
        <div class="info-card-label">CDN Base URL</div>
        <div class="info-card-value">https://cdn.fishtank.live</div>
      </div>
      <div class="info-card">
        <div class="info-card-label">Auth Type</div>
        <div class="info-card-value">JWT Bearer (HS256)</div>
      </div>
      <div class="info-card">
        <div class="info-card-label">Season</div>
        <div class="info-card-value">Season 3 — Active</div>
      </div>
    </div>
  </section>
  <!-- ── FOOTER ── -->
  <footer>
    <span>🐟 fishtank.live API reference — unofficial</span>
    <span>generated from live traffic · march 2026 · <a href="https://fishtank.live" target="_blank">fishtank.live ↗</a></span>
  </footer>

</div>
</body>
</html>
