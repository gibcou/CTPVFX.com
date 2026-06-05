
import { useEffect } from 'react';

function App() {
  useEffect(() => {
    /* CURSOR */
    const cur = document.getElementById('cursor');
    const ring = document.getElementById('cursor-ring');
    let mx = 0, my = 0, rx = 0, ry = 0;
    document.addEventListener('mousemove', e => {
      mx = e.clientX;
      my = e.clientY;
      if (cur) {
        cur.style.left = mx + 'px';
        cur.style.top = my + 'px';
      }
    });

    function animRing() {
      rx += (mx - rx) * 0.12;
      ry += (my - ry) * 0.12;
      if (ring) {
        ring.style.left = rx + 'px';
        ring.style.top = ry + 'px';
      }
      requestAnimationFrame(animRing);
    }
    animRing();

    /* SCROLL PROGRESS */
    const bar = document.getElementById('progress-bar');
    const nav = document.getElementById('main-nav');
    window.addEventListener('scroll', () => {
      const pct = window.scrollY / (document.body.scrollHeight - window.innerHeight) * 100;
      if (bar) {
        bar.style.width = pct + '%';
      }
      if (nav) {
        nav.classList.toggle('scrolled', window.scrollY > 60);
      }
    });

    /* REVEAL on scroll */
    const obs = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('visible');
          obs.unobserve(e.target);
        }
      });
    }, { threshold: 0.12 });
    document.querySelectorAll('.reveal,.reveal-left,.reveal-right,.stagger').forEach(el => obs.observe(el));

    /* COUNT UP */
    const cobs = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (!e.isIntersecting) return;
        const el = e.target;
        const target = +el.dataset.target;
        const dur = 1200;
        let start = null;
        function step(ts) {
          if (!start) start = ts;
          const prog = Math.min((ts - start) / dur, 1);
          el.textContent = Math.round(prog * target);
          if (prog < 1) requestAnimationFrame(step);
        }
        requestAnimationFrame(step);
        cobs.unobserve(el);
      });
    }, { threshold: 0.5 });
    document.querySelectorAll('.count-up').forEach(el => cobs.observe(el));

    /* PARTICLES */
    const canvas = document.getElementById('particles-canvas');
    if (canvas) {
      const ctx = canvas.getContext('2d');
      let W, H, pts = [];
      function resize() { W = canvas.width = canvas.offsetWidth; H = canvas.height = canvas.offsetHeight; }
      resize();
      window.addEventListener('resize', resize);
      for (let i = 0; i < 38; i++) {
        pts.push({ x: Math.random() * 1600, y: Math.random() * 900, vx: (Math.random() - .5) * 0.3, vy: (Math.random() - .5) * 0.3, r: Math.random() * 1.5 + 0.5, o: Math.random() * 0.4 + 0.1 });
      }
      function drawPts() {
        if (!ctx) return;
        ctx.clearRect(0, 0, W, H);
        pts.forEach(p => {
          p.x += p.vx; p.y += p.vy;
          if (p.x < 0 || p.x > W) p.vx *= -1;
          if (p.y < 0 || p.y > H) p.vy *= -1;
          ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
          ctx.fillStyle = 'rgba(0,212,255,' + p.o + ')'; ctx.fill();
        });
        for (let i = 0; i < pts.length; i++) {
          for (let j = i + 1; j < pts.length; j++) {
            const dx = pts[i].x - pts[j].x, dy = pts[i].y - pts[j].y, d = Math.sqrt(dx * dx + dy * dy);
            if (d < 130) { ctx.beginPath(); ctx.moveTo(pts[i].x, pts[i].y); ctx.lineTo(pts[j].x, pts[j].y); ctx.strokeStyle = 'rgba(0,212,255,' + (0.06 * (1 - d / 130)) + ')'; ctx.lineWidth = 0.5; ctx.stroke(); }
          }
        }
        requestAnimationFrame(drawPts);
      }
      drawPts();
    }

    /* FORM */
    const form = document.getElementById('contact-form');
    if (form) {
      function validate(input) {
        const v = input.value.trim();
        const grp = input.closest('.form-group');
        grp?.querySelectorAll('.form-error').forEach(e => e.remove());
        if (input.required && !v) {
          input.classList.add('error'); input.classList.remove('valid');
          const err = document.createElement('span'); err.className = 'form-error'; err.textContent = 'Required';
          grp?.appendChild(err); return false;
        }
        if (input.type === 'email' && v && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v)) {
          input.classList.add('error'); input.classList.remove('valid');
          const err = document.createElement('span'); err.className = 'form-error'; err.textContent = 'Enter a valid email';
          grp?.appendChild(err); return false;
        }
        if (v) { input.classList.remove('error'); input.classList.add('valid'); }
        return true;
      }
      form.querySelectorAll('input,textarea').forEach(el => {
        el.addEventListener('blur', () => validate(el));
        el.addEventListener('input', () => { if (el.classList.contains('error')) validate(el); });
      });
      form.addEventListener('submit', function (e) {
        e.preventDefault();
        const fields = [document.getElementById('f-name'), document.getElementById('f-email'), document.getElementById('f-msg')];
        const valid = fields.map(validate).every(Boolean);
        if (!valid) return;
        const btn = document.getElementById('form-submit-btn');
        btn.textContent = 'Sending…';
        btn.classList.add('loading');
        btn.disabled = true;

        const templateParams = {
          from_name: document.getElementById('f-name').value,
          from_email: document.getElementById('f-email').value,
          service_request: document.getElementById('f-service').value,
          website_url: document.getElementById('f-url').value,
          message: document.getElementById('f-msg').value
        };

        window.emailjs.send('service_t443pro', 'template_llurmr2', templateParams, 'jXlHgWGwGDl8A6EOg')
          .then(() => {
            form.style.display = 'none';
            document.getElementById('form-success').classList.add('visible');
          }, (err) => {
            alert('Oops! There was a problem submitting your form. Please try again later.');
            console.log(JSON.stringify(err));
          })
          .finally(() => {
            btn.textContent = 'Send message →';
            btn.classList.remove('loading');
            btn.disabled = false;
          });
      });
    }


    /* TIMECODE */
    (function () {
      const tc = document.querySelector('.timecode');
      if (!tc) return;
      let f = 14, s = 8, m = 12, h = 1;
      const interval = setInterval(() => {
        f++; if (f >= 24) { f = 0; s++; }
        if (s >= 60) { s = 0; m++; }
        if (m >= 60) { m = 0; h++; }
        const fmt = n => String(n).padStart(2, '0');
        tc.innerHTML = fmt(h) + ':<span class="tc-colon">:</span>' + fmt(m) + ':<span class="tc-colon">:</span>' + fmt(s) + ':<span class="tc-colon">:</span>' + fmt(f);
      }, 1000 / 24);
      return () => clearInterval(interval);
    })();

    /* SHOCKWAVE on buttons */
    document.querySelectorAll('.shock-btn').forEach(btn => {
      btn.addEventListener('click', function () {
        const rings = this.querySelectorAll('.shock-ring,.shock-ring-2');
        rings.forEach(r => { r.classList.remove('fire'); void r.offsetWidth; r.classList.add('fire'); });
      });
    });

    /* DATA STREAM */
    (function () {
      const ds = document.getElementById('data-stream');
      if (!ds) return;
      const chars = '01ABCDEFあいうRGBXYZ∑∆∞◈▸░█'.split('');
      for (let i = 0; i < 10; i++) {
        const col = document.createElement('div');
        col.className = 'ds-col';
        col.style.left = (i * 10 + Math.random() * 5) + '%';
        col.style.animationDuration = (8 + Math.random() * 12) + 's';
        col.style.animationDelay = (-Math.random() * 15) + 's';
        let txt = '';
        for (let j = 0; j < 40; j++) txt += chars[Math.floor(Math.random() * chars.length)] + '\n';
        col.textContent = txt;
        ds.appendChild(col);
      }
    })();

    /* RENDER WIPE on section titles */
    const titleObs = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (!e.isIntersecting) return;
        const el = e.target;
        el.classList.add('wipe-in');
        setTimeout(() => el.classList.add('wipe-done'), 700);
        titleObs.unobserve(el);
      });
    }, { threshold: 0.3 });
    document.querySelectorAll('h2.section-title').forEach(el => titleObs.observe(el));

    /* CRT power-on for quiz */
    const quizObs = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (!e.isIntersecting) return;
        e.target.classList.add('crt-in');
        quizObs.unobserve(e.target);
      });
    }, { threshold: 0.2 });
    const qw = document.getElementById('quiz-wrap');
    if (qw) quizObs.observe(qw);

  }, []);

  return (
    <>
      <div className="film-grain"></div>
      <div id="cursor"></div>
      <div id="cursor-ring"></div>
      <div id="progress-bar"></div>

      <nav id="main-nav">
        <a href="#" className="nav-logo">CUT<span>.</span>POINT</a>
        <ul className="nav-links">
          <li><a href="#why">Approach</a></li>
          <li><a href="#services">Services</a></li>
          <li><a href="#projects">Projects</a></li>
          <li><a href="#contact" className="nav-cta">Get a quote</a></li>
        </ul>
      </nav>

      <div className="scarcity">
        <p><strong>3 client spots remaining</strong> — we cap our roster at 6 studios. Currently 3 open.</p>
      </div>

      {/* HERO */}
      <section className="hero" style={{ paddingTop: '120px' }}>
        <div className="hero-bg-grid"></div>
        <div className="hero-bg-chromatic"></div><div className="scanline"></div><canvas className="hero-particles" id="particles-canvas"></canvas><div className="lens-flare"></div><div className="lens-flare-2"></div><div className="timecode">01:<span className="tc-colon">:</span>12:08<span className="tc-colon">:</span>14</div><div className="vu-meter"><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div><div className="vu-bar"></div></div>
        <div className="hero-left">
          <div className="hero-tag">Web design for VFX studios</div>
          <h1 className="display">Your VFX work<em><span className="glitch-wrap" data-text="deserves a site">deserves a site</span></em>that closes.</h1>
          <p className="hero-sub">We rebuild and maintain websites exclusively for visual effects studios. Not generic agencies. Not freelancers who need VFX explained. Fast, cinematic sites that win clients — on retainer.</p>
          <div className="hero-actions">
            <a href="#contact" className="btn-primary shock-btn">Start the conversation<span className="shock-ring"></span><span className="shock-ring-2"></span></a>
          </div>
        </div>
        <div className="hero-right">
          <div className="hero-stats">
            <div className="stat-item">
              <div className="stat-num"><span className="count-up" data-target="14">0</span><span style={{ color: '#00d4ff', textShadow: '0 0 12px rgba(0,212,255,0.8)' }}>days</span></div>
              <div className="stat-label">From kickoff to live MVP</div>
            </div>
            <div className="stat-item">
              <div className="stat-num">$<span className="count-up" data-target="10">0</span><span style={{ color: '#7b2fff', textShadow: '0 0 12px rgba(123,47,255,0.8)' }}>k</span></div>
              <div className="stat-label">Flat build fee, no surprises</div>
            </div>
            <div className="stat-item">
              <div className="stat-num"><span className="count-up" data-target="6">0</span></div>
              <div className="stat-label">Max roster — ever</div>
            </div>
          </div>
          <div className="hero-availability"><div className="dot"></div>3 of 6 spots remaining — taking new clients now</div>
        </div>
      </section>

      {/* PROBLEM */}
      <section className="problem" id="why">
        <div className="scanline"></div><div className="problem-inner">
          <div className="problem-copy reveal-left">
            <div className="section-tag">The problem</div>
            <h2 className="section-title">Generic agencies <em>don't get it.</em></h2>
            <p>You've tried explaining what a showreel is. You've watched a developer embed your Vimeo reel wrong. You've paid $15k for a site that went stale in 90 days because nobody maintains it.</p>
            <p><strong>VFX studios need a web partner who speaks the language</strong> — shot breakdowns, credit pages, cinematic video embeds, and the hustle of production timelines.</p>
            <p>That's the gap CutPoint was built to fill.</p>
          </div>
          <ul className="problem-list stagger">
            <li><div className="icon">01</div><div><strong>Your reel is buried</strong>Generic portfolio templates hide video. We build video-first layouts where your reel is the first thing production managers see.</div></li>
            <li><div className="icon">02</div><div><strong>Your site never gets updated</strong>You land a major credit and your website still shows 2022 work. Our retainer fixes that permanently.</div></li>
            <li><div className="icon">03</div><div><strong>You're explaining VFX to developers</strong>We know what a VFX breakdown is, why before/after comps matter, and how to structure a credits page. No translation required.</div></li>
            <li><div className="icon">04</div><div><strong>Your site doesn't convert</strong>Slow loads, vague service pages, no clear contact path. We build sites that turn production managers into clients.</div></li>
          </ul>
        </div>
      </section>

      {/* WHY */}
      <section className="why">
        <div className="why-inner">
          <div className="why-header reveal">
            <div className="section-tag">Why CutPoint</div>
            <h2 className="section-title">Built different.<em>By design.</em></h2>
          </div>
          <div className="why-grid stagger">
            <div className="why-card"><div className="why-num">01</div><span className="why-icon">🎬</span><h3>We speak VFX</h3><p>Showreels, shot breakdowns, credit pages, Vimeo embeds — we know the vocabulary and the craft.</p></div>
            <div className="why-card"><div className="why-num">02</div><span className="why-icon">⚡</span><h3>We move fast</h3><p>14 days from kickoff to a live, polished MVP. VFX runs on tight deadlines. Your website build should too.</p></div>
            <div className="why-card"><div className="why-num">03</div><span className="why-icon">🔁</span><h3>We stay on</h3><p>Our retainer keeps your site alive. New reel? New credit? We handle it. You focus on production.</p></div>
            <div className="why-card"><div className="why-num">04</div><span className="why-icon">📐</span><h3>Flat pricing</h3><p>$10k build. $5k/mo retainer. You know the full cost before we start. No surprise invoices.</p></div>
          </div>
        </div>
      </section>

      {/* SERVICES */}
      <section className="services" id="services">
        <div className="services-inner">
          <div className="reveal">
            <div className="section-tag">What we build</div>
            <h2 className="section-title">Sites built for how<em>VFX studios work.</em></h2>
          </div>
          <div className="services-grid stagger">
            <div className="service-card"><div className="service-label">Core offer</div><h3>Portfolio & Showreel Sites</h3><p>Video-first layouts that load fast and look cinematic. Embedded reels, project grids, before/after breakdowns, and credit pages that showcase your work.</p><ul className="service-features"><li>Custom video player integration</li><li>Lazy-loaded showreel embeds</li><li>Project case study templates</li><li>Client credit galleries</li><li>Before/after breakdown sections</li></ul></div>
            <div className="service-card"><div className="service-label">Studio presence</div><h3>New Business Sites</h3><p>Full studio presence — about, services, team, credits, and contact. The complete package for studios pitching on major productions.</p><ul className="service-features"><li>Full CMS implementation</li><li>Team and credit management</li><li>Job listings section</li><li>Multi-language ready</li><li>SEO foundation built in</li></ul></div>
            <div className="service-card"><div className="service-label">Fast turnaround</div><h3>Campaign Landing Pages</h3><p>For a specific campaign, new service offering, or event. Quick turnaround, high production value, built to convert creative directors.</p><ul className="service-features"><li>3–5 day delivery</li><li>SEO optimized structure</li><li>Contact capture forms</li><li>Analytics integration</li><li>A/B test ready</li></ul></div>
          </div>
        </div>
      </section>

      {/* PROJECTS */}
      <section className="services" id="projects">
        <div className="services-inner">
          <div className="reveal">
            <div className="section-tag">Our Work</div>
            <h2 className="section-title">Featured<em>Projects</em></h2>
          </div>
          <div className="stagger">
            <div className="service-card featured-project">
              <div>
                <div className="service-label">Case Study</div>
                <h3>Mythical VFX</h3>
                <ul className="service-features">
                  <li>Full site build</li>
                  <li>Vimeo Integration</li>
                  <li>Custom CMS</li>
                </ul>
                <a href="https://mythicalvfx.com/private/" target="_blank" rel="noreferrer" className="btn-primary" style={{ marginTop: '1rem' }}>View Project</a>
              </div>
              <div>
                <video src="first-vfx-project.mp4" autoPlay muted loop style={{ width: '100%', height: 'auto', borderRadius: '4px' }}></video>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* PROCESS */}
      <section className="process section-burn" id="process">
        <div className="process-inner">
          <div className="reveal">
            <div className="section-tag">The process</div>
            <h2 className="section-title">Kickoff to live<em>in 14 days.</em></h2>
          </div>
          <div className="process-steps stagger">
            <div className="step"><div className="step-dot active"></div><div className="step-days">Days 1–2</div><h3>Discovery</h3><p>We learn your studio's voice, review your assets, and define site structure. A single 30-minute call is all we need.</p></div>
            <div className="step"><div className="step-dot"></div><div className="step-days">Days 3–7</div><h3>Design & Build</h3><p>We design and develop in parallel. You see real progress every 48 hours — no black holes, no surprises.</p></div>
            <div className="step"><div className="step-dot"></div><div className="step-days">Days 8–12</div><h3>Review & Polish</h3><p>You review the live staging site. We iterate on copy, spacing, and animation until it's exactly right.</p></div>
            <div className="step"><div className="step-dot"></div><div className="step-days">Days 13–14</div><h3>Launch</h3><p>DNS cutover, performance optimization, and handoff. Your site is live, fast, and ready for production pitches.</p></div>
          </div>
        </div>
      </section>



      {/* PRICING */}
      <section className="pricing" id="pricing">
        <div className="pricing-inner">
          <div className="reveal">
            <div className="section-tag">Pricing</div>
            <h2 className="section-title">Simple rates.<em>No surprises.</em></h2>
          </div>
          <div className="pricing-grid stagger">
            <div className="price-card">
              <div className="price-badge">One-time</div>
              <h3>Site Build</h3>
              <div className="price-amount"><span>$</span>10k</div>
              <div className="price-freq">One-time project fee</div>
              <div className="price-divider"></div>
              <ul className="price-features">
                <li>Full site design & development</li>
                <li>Showreel & video integration</li>
                <li>Mobile-responsive on all devices</li>
                <li>SEO foundation included</li>
                <li>2 rounds of revisions</li>
                <li>2 weeks post-launch support</li>
              </ul>
              <p className="price-note">Most clients convert to a retainer after launch.</p>
            </div>
            <div className="price-card featured">
              <div className="price-badge">Monthly retainer</div>
              <h3>Ongoing Partner</h3>
              <div className="price-amount"><span>$</span>5k</div>
              <div className="price-freq">Per month, cancel anytime</div>
              <div className="price-divider"></div>
              <ul className="price-features">
                <li>New project additions anytime</li>
                <li>Reel & showreel updates</li>
                <li>Design refreshes as you evolve</li>
                <li>Performance monitoring & uptime</li>
                <li>Slack access during business hours</li>
                <li>Priority turnaround on all updates</li>
              </ul>
              <p className="price-note">This is how your site stays an asset, not a liability.</p>
            </div>
          </div>
        </div>
      </section>

      {/* CONTACT */}
      <section className="contact-section" id="contact">
        <div className="data-stream" id="data-stream"></div>
        <div className="contact-inner">
          <div className="contact-left reveal-left">
            <div className="section-tag">Let's talk</div>
            <h2>Tired of explaining VFX<em>to your web team?</em></h2>
            <p>We work exclusively with visual effects studios, motion designers, and film industry creatives. If you're ready for a site that works as hard as your team does — let's talk.</p>
            <div className="contact-meta">
              <div className="contact-meta-item">14-day delivery on all builds</div>
              <div className="contact-meta-item">3 spots currently open</div>
              <div className="contact-meta-item">Bozeman, MT — remote-friendly</div>
              <div className="contact-meta-item">Response within 1 business day</div>
            </div>
          </div>
          <div className="reveal-right">
            <div className="form-success" id="form-success">
              <span className="check">✓</span>
              <h3>Message received.</h3>
              <p>We'll be in touch within one business day. While you wait, take a look at our process overview above.</p>
            </div>
            <form className="contact-form" id="contact-form" noValidate>
              <div className="form-row">
                <div className="form-group">
                  <label htmlFor="f-name">Studio / name *</label>
                  <input type="text" id="f-name" name="from_name" placeholder="Blur Studio" required />
                </div>
                <div className="form-group">
                  <label htmlFor="f-email">Email *</label>
                  <input type="email" id="f-email" name="from_email" placeholder="you@studio.com" required />
                </div>
              </div>
              <div className="form-group">
                <label htmlFor="f-service">What are you looking for?</label>
                <select id="f-service" name="service_request">
                  <option value="">Select a service…</option>
                  <option value="build">Full site build ($10k)</option>
                  <option value="retainer">Build + retainer</option>
                  <option value="retainer-only">Retainer only ($5k/mo)</option>
                  <option value="landing">Campaign landing page</option>
                  <option value="unsure">Not sure yet</option>
                </select>
              </div>
              <div className="form-group">
                <label htmlFor="f-url">Current site URL</label>
                <input type="url" id="f-url" name="website_url" placeholder="https://yourstudio.com" />
              </div>
              <div className="form-group">
                <label htmlFor="f-msg">Tell us about your studio *</label>
                <textarea id="f-msg" name="message" placeholder="What do you do, who are your clients, what's not working about your current web presence?" required></textarea>
              </div>
              <div style={{ display: 'flex', alignItems: 'center', gap: '1rem', flexWrap: 'wrap' }}>
                <button type="submit" className="form-submit" id="form-submit-btn">Send message →</button>
                <span style={{ fontFamily: 'var(--font-mono)', fontSize: '0.65rem', color: 'var(--gray-light)', letterSpacing: '0.08em' }}>We reply within 1 business day</span>
              </div>
            </form>
          </div>
        </div>
      </section>

      <footer>
        <div className="footer-logo">CUT<span>.</span>POINT</div>
        <div className="footer-meta">Web design for VFX studios · Bozeman, MT</div>
        <div className="footer-contact"><a href="mailto:cutpointvfx@gmail.com">cutpointvfx@gmail.com</a></div>
      </footer>
    </>
  );
}

export default App;
