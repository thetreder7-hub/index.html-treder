# index.html-treder
Institution-grade trading using wyckoff SMC Option chain &amp; smart money concept
import { useState, useEffect, useRef } from "react";
import {
  AreaChart, Area, BarChart, Bar,
  XAxis, YAxis, ResponsiveContainer, Tooltip,
} from "recharts";
import {
  TrendingUp, TrendingDown, Activity, Target, Shield, Brain,
  BookOpen, BarChart2, Star, Zap, Eye, Clock, Mail, Instagram,
  Youtube, Send, Linkedin, ArrowRight, Menu, X, Globe, Layers,
  AlertCircle, CheckCircle, RefreshCw, Database, ChevronRight,
} from "lucide-react";

/* ────────────────── PALETTE ────────────────── */
const C = {
  bg: "#020B18",
  surface: "rgba(5,18,32,0.9)",
  border: "rgba(0,212,255,0.14)",
  borderHov: "rgba(0,212,255,0.38)",
  primary: "#00D4FF",
  primaryDim: "rgba(0,212,255,0.08)",
  green: "#00E676",
  greenDim: "rgba(0,230,118,0.08)",
  red: "#FF3D57",
  redDim: "rgba(255,61,87,0.08)",
  amber: "#FFB300",
  amberDim: "rgba(255,179,0,0.08)",
  violet: "#A78BFA",
  pink: "#FF6B9D",
  text: "#D0E8F0",
  textDim: "#7090A0",
  textMuted: "#3E5870",
};

/* ────────────────── FAKE DATA ────────────────── */
const makeArea = (start, step, len) =>
  Array.from({ length: len }, (_, i) => {
    start += (Math.random() - 0.47) * step;
    return { t: i, v: Math.round(start) };
  });

const NIFTY_DATA = makeArea(23800, 60, 55);
const BNF_DATA   = makeArea(51200, 140, 55);

const TICKER = [
  { sym: "NIFTY", val: "23,847", chg: "+0.68%", up: true },
  { sym: "BANKNIFTY", val: "51,234", chg: "-0.23%", up: false },
  { sym: "SENSEX", val: "78,500", chg: "+0.55%", up: true },
  { sym: "SGX NIFTY", val: "23,900", chg: "+0.89%", up: true },
  { sym: "DOW", val: "39,850", chg: "-0.15%", up: false },
  { sym: "NASDAQ", val: "17,542", chg: "+0.42%", up: true },
  { sym: "GOLD", val: "₹72,450", chg: "+0.31%", up: true },
  { sym: "CRUDE OIL", val: "₹6,834", chg: "-1.20%", up: false },
  { sym: "INDIA VIX", val: "13.45", chg: "-2.30%", up: false },
  { sym: "USD/INR", val: "83.42", chg: "+0.12%", up: true },
];

const SKILLS = [
  { icon: Activity,   title: "Wyckoff Analysis",      desc: "Phase detection, composite man, spring & upthrust identification",    color: C.primary },
  { icon: Layers,     title: "Smart Money Concepts",   desc: "Order blocks, breakers, mitigation zones & CHoCH",                    color: C.green },
  { icon: BarChart2,  title: "Volume Profile",         desc: "VAH, VAL, POC, HVN/LVN — where real conviction lives",               color: C.amber },
  { icon: Eye,        title: "Option Chain Reading",   desc: "OI buildup, PCR, max pain & unwinding patterns",                     color: C.pink },
  { icon: Brain,      title: "Greeks Analysis",        desc: "Delta, Gamma, Theta, Vega — real-time option dynamics",              color: C.violet },
  { icon: Target,     title: "Market Structure",       desc: "HH, HL, LL, LH — structure breaks & continuation patterns",         color: C.primary },
  { icon: Zap,        title: "Fair Value Gaps",        desc: "FVG identification, mitigation zones & gap fills",                   color: C.green },
  { icon: Database,   title: "Liquidity Analysis",     desc: "Buyside & sellside sweeps, stop hunts & institutional traps",       color: C.amber },
  { icon: RefreshCw,  title: "Institutional Bias",     desc: "FII/DII positioning, global cues & smart money flow mapping",       color: C.pink },
  { icon: Shield,     title: "Risk Management",        desc: "Position sizing, R:R ratios, max daily loss & stop placement",      color: C.violet },
];

const PSYCH = [
  { icon: AlertCircle, title: "Fear & Greed",           desc: "Master emotional extremes that silently destroy P&L",               color: C.red },
  { icon: RefreshCw,   title: "Revenge Trading",         desc: "Break the destructive cycle of emotional retaliation",             color: C.amber },
  { icon: Shield,      title: "Discipline",              desc: "System over impulse, rules over fleeting feelings",                color: C.primary },
  { icon: Clock,       title: "Patience",                desc: "Wait for A+ setups — avoid the seduction of low-quality trades",  color: C.green },
  { icon: CheckCircle, title: "Execution Confidence",    desc: "Pull the trigger with conviction, clarity and zero hesitation",   color: C.violet },
  { icon: Brain,       title: "Emotional Control",       desc: "Detach from outcomes, anchor on process and probability",         color: C.pink },
];

const EDU = [
  { icon: BookOpen, title: "Beginner Trading",   count: 12, color: C.green,  tag: "Foundation"    },
  { icon: Layers,   title: "SMC Tutorials",      count: 18, color: C.primary,tag: "Advanced"      },
  { icon: Activity, title: "Wyckoff Method",     count: 15, color: C.amber,  tag: "Advanced"      },
  { icon: BarChart2,title: "Options Buying",     count: 20, color: C.pink,   tag: "Intermediate"  },
  { icon: Shield,   title: "Risk Management",    count: 10, color: C.violet, tag: "Essential"     },
  { icon: Brain,    title: "Market Psychology",  count: 14, color: C.green,  tag: "Mindset"       },
];

const MONTHLY = [
  { m: "Jul", w: 78 },{ m: "Aug", w: 82 },{ m: "Sep", w: 75 },
  { m: "Oct", w: 88 },{ m: "Nov", w: 85 },{ m: "Dec", w: 91 },
  { m: "Jan", w: 83 },{ m: "Feb", w: 87 },{ m: "Mar", w: 89 },
];

const NAV = [
  { href: "#home",       label: "Home"        },
  { href: "#about",      label: "About"       },
  { href: "#skills",     label: "Skills"      },
  { href: "#analysis",   label: "Analysis"    },
  { href: "#psychology", label: "Psychology"  },
  { href: "#education",  label: "Learn"       },
  { href: "#performance",label: "Performance" },
  { href: "#contact",    label: "Contact"     },
];

/* ────────────────── SHARED STYLE HELPERS ────────────────── */
const card = (ex = {}) => ({
  background: C.surface,
  border: `1px solid ${C.border}`,
  borderRadius: 14,
  backdropFilter: "blur(12px)",
  ...ex,
});

const badge = (color) => ({
  display: "inline-flex", alignItems: "center", gap: 5,
  padding: "3px 11px", borderRadius: 20,
  fontSize: 11, fontWeight: 700, letterSpacing: "0.06em",
  color, background: color + "18", border: `1px solid ${color}30`,
});

const glowNum = (color) => ({
  color,
  textShadow: `0 0 24px ${color}55`,
});

/* ════════════════ GLOBAL CSS INJECTION ════════════════ */
const GLOBAL_CSS = `
* { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body { background: #020B18; overflow-x: hidden; }
::-webkit-scrollbar { width: 3px; }
::-webkit-scrollbar-track { background: #020B18; }
::-webkit-scrollbar-thumb { background: #00D4FF35; border-radius: 2px; }

@keyframes ticker { 0%{transform:translateX(0)} 100%{transform:translateX(-50%)} }
@keyframes scanner {
  0%   { top: -2px; opacity: 0; }
  5%   { opacity: 1; }
  95%  { opacity: 0.6; }
  100% { top: 100%; opacity: 0; }
}
@keyframes floatUp  { from{opacity:0;transform:translateY(32px)} to{opacity:1;transform:translateY(0)} }
@keyframes blink    { 0%,100%{opacity:1} 50%{opacity:0.25} }
@keyframes pulseLow { 0%,100%{box-shadow:0 0 8px #00D4FF20} 50%{box-shadow:0 0 28px #00D4FF50,0 0 56px #00D4FF15} }
@keyframes gridPulse{ 0%,100%{opacity:0.04} 50%{opacity:0.08} }

.fi-0  { animation: floatUp 0.7s 0.05s both ease }
.fi-1  { animation: floatUp 0.7s 0.15s both ease }
.fi-2  { animation: floatUp 0.7s 0.25s both ease }
.fi-3  { animation: floatUp 0.7s 0.38s both ease }
.fi-4  { animation: floatUp 0.7s 0.50s both ease }
.fi-5  { animation: floatUp 0.7s 0.65s both ease }

.sc { transition: all 0.28s ease; }
.sc:hover { transform: translateY(-5px); border-color: rgba(0,212,255,0.36) !important; background: rgba(0,212,255,0.05) !important; }

.btn-primary { transition: all 0.22s ease; cursor: pointer; }
.btn-primary:hover { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(0,212,255,0.35) !important; }

.btn-ghost { transition: all 0.22s ease; cursor: pointer; }
.btn-ghost:hover { transform: translateY(-2px); background: rgba(0,212,255,0.08) !important; }

.nav-a { transition: color 0.2s; text-decoration: none; color: #5A7888;
  font-size: 12px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; }
.nav-a:hover { color: #00D4FF; }

.cc { transition: all 0.28s ease; }
.cc:hover { transform: translateY(-4px); border-color: rgba(0,212,255,0.38) !important; }

.edu-c { transition: all 0.28s ease; cursor: pointer; }
.edu-c:hover { transform: translateY(-5px); box-shadow: 0 16px 48px rgba(0,0,0,0.45) !important; }

.bias-bull { color:#00E676; background:rgba(0,230,118,0.07); border:1px solid rgba(0,230,118,0.28); }
.bias-bear { color:#FF3D57; background:rgba(255,61,87,0.07);  border:1px solid rgba(255,61,87,0.28); }
.bias-neu  { color:#FFB300; background:rgba(255,179,0,0.07);  border:1px solid rgba(255,179,0,0.28); }
`;

/* ════════════════ MAIN EXPORT ════════════════ */
export default function TrederHub() {
  const [scrolled,     setScrolled]     = useState(false);
  const [menuOpen,     setMenuOpen]     = useState(false);
  const [counts,       setCounts]       = useState({ acc: 0, risk: 0, setup: 0, disc: 0 });
  const [counted,      setCounted]      = useState(false);
  const perfRef = useRef(null);

  /* inject global styles */
  useEffect(() => {
    const el = document.createElement("style");
    el.textContent = GLOBAL_CSS;
    document.head.appendChild(el);
    return () => document.head.removeChild(el);
  }, []);

  /* navbar shadow on scroll */
  useEffect(() => {
    const fn = () => setScrolled(window.scrollY > 48);
    window.addEventListener("scroll", fn);
    return () => window.removeEventListener("scroll", fn);
  }, []);

  /* animated counters on scroll into view */
  useEffect(() => {
    const targets = { acc: 87, risk: 92, setup: 89, disc: 94 };
    const obs = new IntersectionObserver(([e]) => {
      if (e.isIntersecting && !counted) {
        setCounted(true);
        let step = 0;
        const total = 70;
        const timer = setInterval(() => {
          step++;
          const p = 1 - Math.pow(1 - step / total, 3);
          setCounts({
            acc:  Math.round(targets.acc  * p),
            risk: Math.round(targets.risk * p),
            setup:Math.round(targets.setup* p),
            disc: Math.round(targets.disc * p),
          });
          if (step >= total) clearInterval(timer);
        }, 28);
      }
    }, { threshold: 0.25 });
    if (perfRef.current) obs.observe(perfRef.current);
    return () => obs.disconnect();
  }, [counted]);

  /* ── shared section wrapper ── */
  const Wrap = ({ children, id, bg }) => (
    <section id={id} style={{ padding: "80px 20px", background: bg || "transparent" }}>
      <div style={{ maxWidth: 1180, margin: "0 auto" }}>{children}</div>
    </section>
  );

  /* ── section header ── */
  const SHeader = ({ label, title, sub, center }) => (
    <div style={{ textAlign: center ? "center" : "left", marginBottom: 48 }}>
      <span style={{ fontSize: 11, fontWeight: 700, letterSpacing: "0.18em", textTransform: "uppercase",
        color: C.primary, display: "block", marginBottom: 10 }}>{label}</span>
      <h2 style={{ fontSize: "clamp(26px,3.8vw,40px)", fontWeight: 800, color: C.text,
        letterSpacing: "-0.025em", lineHeight: 1.15 }}>{title}</h2>
      <div style={{ width: 56, height: 2, background: `linear-gradient(90deg,${C.primary},transparent)`,
        borderRadius: 1, marginTop: 14, marginBottom: sub ? 12 : 0,
        marginLeft: center ? "auto" : 0, marginRight: center ? "auto" : 0 }} />
      {sub && <p style={{ fontSize: 15, color: C.textDim, marginTop: 10, lineHeight: 1.65,
        maxWidth: center ? 520 : "none", margin: center ? "10px auto 0" : undefined }}>{sub}</p>}
    </div>
  );

  /* ════════ RENDER ════════ */
  return (
    <div style={{ background: C.bg, color: C.text,
      fontFamily: "'SF Pro Display',-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif",
      overflowX: "hidden" }}>

      {/* ══════════════ NAVBAR ══════════════ */}
      <nav style={{
        position: "fixed", top: 0, left: 0, right: 0, zIndex: 999,
        transition: "all 0.3s",
        background: scrolled ? "rgba(2,11,24,0.96)" : "transparent",
        backdropFilter: scrolled ? "blur(22px)" : "none",
        borderBottom: scrolled ? `1px solid ${C.border}` : "none",
        padding: "0 20px",
      }}>
        <div style={{ maxWidth: 1180, margin: "0 auto", height: 62,
          display: "flex", alignItems: "center", justifyContent: "space-between" }}>

          {/* logo */}
          <a href="#home" style={{ display: "flex", alignItems: "center", gap: 9, textDecoration: "none" }}>
            <div style={{ width: 32, height: 32, borderRadius: 9,
              background: `linear-gradient(135deg,${C.primary},#006688)`,
              display: "flex", alignItems: "center", justifyContent: "center",
              animation: "pulseLow 3s infinite" }}>
              <Activity size={17} color="#020B18" strokeWidth={2.8} />
            </div>
            <span style={{ fontSize: 17, fontWeight: 800, letterSpacing: "-0.02em" }}>
              <span style={{ color: C.primary }}>The</span>
              <span style={{ color: C.text }}> Treder Hub</span>
            </span>
          </a>

          {/* nav links — hidden on small screens via inline (JS-free approach) */}
          <div style={{ display: "flex", gap: 24 }}>
            {NAV.slice(0, 7).map(l => (
              <a key={l.href} href={l.href} className="nav-a">{l.label}</a>
            ))}
          </div>

          {/* CTA + hamburger */}
          <div style={{ display: "flex", gap: 10, alignItems: "center" }}>
            <a href="#analysis"
              className="btn-primary"
              style={{ padding: "8px 18px", background: C.primary, color: "#020B18",
                borderRadius: 8, fontSize: 12, fontWeight: 800,
                textDecoration: "none", letterSpacing: "0.06em", border: "none" }}>
              Daily Analysis
            </a>
            <button
              onClick={() => setMenuOpen(v => !v)}
              style={{ background: "none", border: "none", color: C.text, cursor: "pointer", padding: 4 }}>
              {menuOpen ? <X size={21} /> : <Menu size={21} />}
            </button>
          </div>
        </div>

        {menuOpen && (
          <div style={{ background: "rgba(2,11,24,0.98)", borderTop: `1px solid ${C.border}`,
            padding: "14px 20px 18px" }}>
            {NAV.map(l => (
              <a key={l.href} href={l.href} className="nav-a"
                style={{ display: "block", padding: "10px 0", fontSize: 14, color: C.textDim }}
                onClick={() => setMenuOpen(false)}>{l.label}</a>
            ))}
          </div>
        )}
      </nav>

      {/* ══════════════ HERO ══════════════ */}
      <section id="home" style={{ position: "relative", minHeight: "100vh",
        display: "flex", alignItems: "center", overflow: "hidden", paddingTop: 62 }}>

        {/* grid bg */}
        <div style={{ position: "absolute", inset: 0, zIndex: 0,
          backgroundImage: `linear-gradient(rgba(0,212,255,0.045) 1px,transparent 1px),
            linear-gradient(90deg,rgba(0,212,255,0.045) 1px,transparent 1px)`,
          backgroundSize: "60px 60px", animation: "gridPulse 6s ease-in-out infinite" }} />

        {/* radial glow */}
        <div style={{ position: "absolute", inset: 0, zIndex: 1,
          background: "radial-gradient(ellipse 65% 55% at 48% 48%, rgba(0,100,180,0.13) 0%, transparent 70%)" }} />

        {/* scanner line */}
        <div style={{ position: "absolute", left: 0, right: 0, height: 1, zIndex: 2,
          background: `linear-gradient(90deg,transparent 0%,${C.primary}45 50%,transparent 100%)`,
          animation: "scanner 7s linear infinite" }} />

        {/* bottom fade */}
        <div style={{ position: "absolute", bottom: 0, left: 0, right: 0, height: 200, zIndex: 3,
          background: `linear-gradient(to top,${C.bg},transparent)` }} />

        {/* content */}
        <div style={{ maxWidth: 1180, margin: "0 auto", padding: "0 20px",
          position: "relative", zIndex: 4, width: "100%",
          display: "grid", gridTemplateColumns: "1fr auto", gap: 32, alignItems: "center" }}>

          {/* LEFT */}
          <div style={{ maxWidth: 720 }}>
            <div className="fi-0" style={{ display: "flex", gap: 8, marginBottom: 24, flexWrap: "wrap" }}>
              <span style={{ ...badge(C.primary), gap: 7 }}>
                <span style={{ width: 6, height: 6, borderRadius: "50%", background: C.primary,
                  animation: "blink 1.4s ease infinite" }} />
                LIVE MARKET ANALYSIS
              </span>
              <span style={badge(C.green)}>NSE · BSE SPECIALIST</span>
              <span style={badge(C.amber)}>NIFTY · BANKNIFTY</span>
            </div>

            <h1 className="fi-1" style={{ fontSize: "clamp(34px,5vw,58px)", fontWeight: 900,
              lineHeight: 1.1, letterSpacing: "-0.03em", marginBottom: 18 }}>
              <span style={{ color: C.text }}>Institutional-Level</span>
              <br />
              <span style={{ background: `linear-gradient(130deg,${C.primary} 0%,#0099BB 40%,${C.green} 100%)`,
                WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent",
                backgroundClip: "text" }}>Market Analysis</span>
              <br />
              <span style={{ color: C.text }}>&amp; Smart Money Trading</span>
            </h1>

            <p className="fi-2" style={{ fontSize: 17, color: C.textDim, lineHeight: 1.75,
              marginBottom: 18, maxWidth: 600 }}>
              Master the markets the way institutions do. Wyckoff Method, Smart Money Concepts,
              Volume Profile, and Option Chain — not retail noise, institutional precision.
            </p>

            <div className="fi-2" style={{ display: "flex", gap: 7, flexWrap: "wrap", marginBottom: 36 }}>
              {["Wyckoff", "SMC", "Option Chain", "Volume Profile", "Market Psychology"].map(t => (
                <span key={t} style={{ padding: "4px 12px", borderRadius: 6,
                  background: "rgba(0,212,255,0.06)", border: `1px solid rgba(0,212,255,0.16)`,
                  color: C.primary, fontSize: 12, fontWeight: 600 }}>{t}</span>
              ))}
            </div>

            <div className="fi-3" style={{ display: "flex", gap: 11, flexWrap: "wrap" }}>
              <a href="#education" className="btn-primary"
                style={{ display: "inline-flex", alignItems: "center", gap: 8,
                  padding: "13px 26px", background: C.primary, color: "#020B18",
                  borderRadius: 10, fontSize: 14, fontWeight: 800, textDecoration: "none",
                  boxShadow: `0 0 28px ${C.primary}38` }}>
                <BookOpen size={17} /> Learn Trading
              </a>
              <a href="#analysis" className="btn-ghost"
                style={{ display: "inline-flex", alignItems: "center", gap: 8,
                  padding: "13px 26px", background: "transparent",
                  border: `1px solid ${C.primary}`, color: C.primary,
                  borderRadius: 10, fontSize: 14, fontWeight: 700, textDecoration: "none" }}>
                <Activity size={17} /> Daily Analysis
              </a>
              <a href="#contact" className="btn-ghost"
                style={{ display: "inline-flex", alignItems: "center", gap: 8,
                  padding: "13px 26px", background: "rgba(255,255,255,0.03)",
                  border: `1px solid rgba(255,255,255,0.1)`, color: C.text,
                  borderRadius: 10, fontSize: 14, fontWeight: 600, textDecoration: "none" }}>
                Contact
              </a>
            </div>

            {/* hero stats */}
            <div className="fi-4" style={{ display: "flex", gap: 32, marginTop: 52, flexWrap: "wrap" }}>
              {[
                { v: "5+",    l: "Years Experience"  },
                { v: "87%",   l: "Analysis Accuracy" },
                { v: "10+",   l: "Trading Frameworks" },
                { v: "1000+", l: "Charts Analyzed"   },
              ].map(({ v, l }) => (
                <div key={l}>
                  <div style={{ fontSize: 26, fontWeight: 900, fontFamily: "monospace", ...glowNum(C.primary) }}>{v}</div>
                  <div style={{ fontSize: 12, color: C.textMuted, marginTop: 2 }}>{l}</div>
                </div>
              ))}
            </div>
          </div>

          {/* RIGHT — Nifty chart widget */}
          <div className="fi-5" style={{ ...card({ padding: 18 }), width: 340, flexShrink: 0 }}>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 10 }}>
              <span style={{ fontSize: 12, fontWeight: 800, color: C.primary, letterSpacing: "0.08em" }}>NIFTY 50</span>
              <span style={{ fontSize: 12, color: C.green, fontFamily: "monospace" }}>▲ +0.68%</span>
            </div>
            <div style={{ fontSize: 26, fontWeight: 900, fontFamily: "monospace", color: C.text, marginBottom: 10 }}>
              23,847.50
            </div>
            <ResponsiveContainer width="100%" height={110}>
              <AreaChart data={NIFTY_DATA}>
                <defs>
                  <linearGradient id="hg" x1="0" y1="0" x2="0" y2="1">
                    <stop offset="5%"  stopColor={C.primary} stopOpacity={0.28} />
                    <stop offset="95%" stopColor={C.primary} stopOpacity={0}    />
                  </linearGradient>
                </defs>
                <Area type="monotone" dataKey="v" stroke={C.primary}
                  fill="url(#hg)" strokeWidth={1.5} dot={false} />
              </AreaChart>
            </ResponsiveContainer>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 8, marginTop: 10 }}>
              <div style={{ padding: "8px 10px", background: C.greenDim, borderRadius: 8,
                border: `1px solid rgba(0,230,118,0.18)` }}>
                <div style={{ fontSize: 9, color: C.textMuted, fontWeight: 700, marginBottom: 2 }}>SUPPORT</div>
                <div style={{ fontSize: 14, color: C.green, fontFamily: "monospace", fontWeight: 800 }}>23,700</div>
              </div>
              <div style={{ padding: "8px 10px", background: C.redDim, borderRadius: 8,
                border: `1px solid rgba(255,61,87,0.18)` }}>
                <div style={{ fontSize: 9, color: C.textMuted, fontWeight: 700, marginBottom: 2 }}>RESISTANCE</div>
                <div style={{ fontSize: 14, color: C.red, fontFamily: "monospace", fontWeight: 800 }}>24,050</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* ══════════════ TICKER ══════════════ */}
      <div style={{ background: "rgba(0,212,255,0.03)", overflow: "hidden",
        borderTop: `1px solid ${C.border}`, borderBottom: `1px solid ${C.border}`,
        padding: "9px 0" }}>
        <div style={{ display: "flex", animation: "ticker 28s linear infinite",
          width: "max-content" }}>
          {[...TICKER, ...TICKER].map((t, i) => (
            <div key={i} style={{ display: "flex", alignItems: "center", gap: 8,
              padding: "0 24px", borderRight: `1px solid ${C.border}`, whiteSpace: "nowrap" }}>
              <span style={{ fontSize: 10, fontWeight: 800, color: C.textMuted, letterSpacing: "0.1em" }}>{t.sym}</span>
              <span style={{ fontSize: 13, fontWeight: 700, color: C.text, fontFamily: "monospace" }}>{t.val}</span>
              <span style={{ fontSize: 11, color: t.up ? C.green : C.red, fontFamily: "monospace" }}>{t.chg}</span>
              {t.up
                ? <TrendingUp  size={11} color={C.green} />
                : <TrendingDown size={11} color={C.red}  />}
            </div>
          ))}
        </div>
      </div>

      {/* ══════════════ ABOUT ══════════════ */}
      <Wrap id="about">
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(290px,1fr))",
          gap: 44, alignItems: "start" }}>

          <div>
            <SHeader label="About Me" title={"Trader. Analyst.\nLegal Professional."} />
            <p style={{ fontSize: 15, color: C.textDim, lineHeight: 1.8, marginBottom: 16 }}>
              I'm a professional trader and market analyst specializing in institutional-grade
              analysis frameworks. With deep expertise in Wyckoff Method and Smart Money
              Concepts, I decode where institutional money moves before retail traders
              even notice.
            </p>
            <p style={{ fontSize: 15, color: C.textDim, lineHeight: 1.8, marginBottom: 28 }}>
              As a practicing <strong style={{ color: C.primary }}>Advocate</strong>,
              I bring analytical precision and methodical discipline to both courtrooms and
              trading terminals — because both demand reading between the lines.
            </p>
            <div style={{ display: "flex", gap: 8, flexWrap: "wrap" }}>
              {["Wyckoff Expert", "SMC Specialist", "Option Buyer", "Risk Manager", "Legal Professional"].map(t => (
                <span key={t} style={badge(C.primary)}>{t}</span>
              ))}
            </div>
          </div>

          <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 13 }}>
            {[
              { icon: Activity,   t: "Wyckoff Method",      d: "Phase-based institutional accumulation & distribution",  c: C.primary },
              { icon: Layers,     t: "Smart Money Concepts", d: "Read institutional footprints and smart order flow",     c: C.green  },
              { icon: Eye,        t: "Intraday Trading",     d: "Precision entries using multi-timeframe confluence",     c: C.violet },
              { icon: Target,     t: "Option Buying",        d: "High R:R setups with Greeks-aware execution",            c: C.amber  },
              { icon: Globe,      t: "Institutional Analysis",d: "FII/DII data, global cues & macro bias building",      c: C.pink   },
              { icon: Brain,      t: "Trading Psychology",   d: "Discipline, detachment & process-based thinking",       c: C.green  },
            ].map(({ icon: Ic, t, d, c }) => (
              <div key={t} style={{ ...card({ padding: 15 }) }} className="sc">
                <Ic size={19} color={c} style={{ marginBottom: 8 }} />
                <div style={{ fontSize: 13, fontWeight: 700, color: C.text, marginBottom: 4 }}>{t}</div>
                <div style={{ fontSize: 11, color: C.textMuted, lineHeight: 1.55 }}>{d}</div>
              </div>
            ))}
          </div>
        </div>
      </Wrap>

      {/* ══════════════ SKILLS ══════════════ */}
      <Wrap id="skills" bg="rgba(0,212,255,0.025)">
        <SHeader label="Expertise" title="Trading Skills & Frameworks" center />
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(210px,1fr))", gap: 14 }}>
          {SKILLS.map(({ icon: Ic, title, desc, color }, i) => (
            <div key={title} className="sc" style={{ ...card({ padding: 20 }),
              animationDelay: `${i * 0.04}s` }}>
              <div style={{ width: 44, height: 44, borderRadius: 10,
                background: color + "14", border: `1px solid ${color}28`,
                display: "flex", alignItems: "center", justifyContent: "center", marginBottom: 14 }}>
                <Ic size={22} color={color} />
              </div>
              <h3 style={{ fontSize: 14, fontWeight: 800, color: C.text,
                marginBottom: 6, letterSpacing: "-0.01em" }}>{title}</h3>
              <p style={{ fontSize: 11, color: C.textMuted, lineHeight: 1.6 }}>{desc}</p>
              <div style={{ marginTop: 14, height: 1.5,
                background: `linear-gradient(90deg,${color},transparent)`,
                borderRadius: 1, opacity: 0.45 }} />
            </div>
          ))}
        </div>
      </Wrap>

      {/* ══════════════ MARKET ANALYSIS ══════════════ */}
      <Wrap id="analysis">
        <SHeader label="Daily Analysis" title="Market Intelligence Dashboard"
          sub="Institutional-grade market read updated before every market open." />

        {/* main charts */}
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(340px,1fr))",
          gap: 18, marginBottom: 18 }}>
          {[
            { sym: "NIFTY 50",   val: "23,847.50", chg: "▲ +162.15 (+0.68%)", up: true,
              bias: "BULLISH", biasClass: "bias-bull", data: NIFTY_DATA, color: C.primary,
              gradId: "ng", sup: "23,700", res: "24,050" },
            { sym: "BANKNIFTY",  val: "51,234.25", chg: "▼ −119.55 (−0.23%)", up: false,
              bias: "NEUTRAL",  biasClass: "bias-neu",  data: BNF_DATA,   color: C.amber,
              gradId: "bg", sup: "51,000", res: "51,600" },
          ].map(({ sym, val, chg, up, bias, biasClass, data, color, gradId, sup, res }) => (
            <div key={sym} style={card({ padding: 20 })}>
              <div style={{ display: "flex", justifyContent: "space-between",
                alignItems: "flex-start", marginBottom: 14 }}>
                <div>
                  <div style={{ fontSize: 11, color: C.textMuted, fontWeight: 700,
                    letterSpacing: "0.12em", marginBottom: 4 }}>{sym}</div>
                  <div style={{ fontSize: 26, fontWeight: 900, color: C.text,
                    fontFamily: "monospace" }}>{val}</div>
                  <div style={{ fontSize: 12, color: up ? C.green : C.red,
                    marginTop: 3, fontFamily: "monospace" }}>{chg}</div>
                </div>
                <span className={biasClass} style={{ padding: "6px 13px", borderRadius: 8,
                  fontSize: 11, fontWeight: 900, letterSpacing: "0.1em" }}>{bias}</span>
              </div>

              <ResponsiveContainer width="100%" height={130}>
                <AreaChart data={data}>
                  <defs>
                    <linearGradient id={gradId} x1="0" y1="0" x2="0" y2="1">
                      <stop offset="5%"  stopColor={color} stopOpacity={0.22} />
                      <stop offset="95%" stopColor={color} stopOpacity={0}    />
                    </linearGradient>
                  </defs>
                  <XAxis hide /><YAxis hide domain={["auto","auto"]} />
                  <Tooltip contentStyle={{ background: "#051220",
                    border: `1px solid ${C.border}`, borderRadius: 8, fontSize: 11 }}
                    labelFormatter={() => ""} formatter={v => [v.toLocaleString(), sym]} />
                  <Area type="monotone" dataKey="v" stroke={color}
                    fill={`url(#${gradId})`} strokeWidth={1.6} dot={false} />
                </AreaChart>
              </ResponsiveContainer>

              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 8, marginTop: 12 }}>
                <div style={{ padding: "9px 10px", background: C.greenDim, borderRadius: 8,
                  border: "1px solid rgba(0,230,118,0.18)" }}>
                  <div style={{ fontSize: 9, color: C.textMuted, fontWeight: 700, marginBottom: 3 }}>SUPPORT</div>
                  <div style={{ fontSize: 14, color: C.green, fontFamily: "monospace", fontWeight: 800 }}>{sup}</div>
                </div>
                <div style={{ padding: "9px 10px", background: C.redDim, borderRadius: 8,
                  border: "1px solid rgba(255,61,87,0.18)" }}>
                  <div style={{ fontSize: 9, color: C.textMuted, fontWeight: 700, marginBottom: 3 }}>RESISTANCE</div>
                  <div style={{ fontSize: 14, color: C.red, fontFamily: "monospace", fontWeight: 800 }}>{res}</div>
                </div>
              </div>
            </div>
          ))}
        </div>

        {/* info cards */}
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(170px,1fr))",
          gap: 12, marginBottom: 18 }}>
          {[
            { l: "Global Cues",   v: "MIXED",    s: "SGX +0.8% · DOW −0.1%",      c: C.amber,   I: Globe      },
            { l: "FII Activity",  v: "BUYING",   s: "₹2,340 Cr Net Buy",           c: C.green,   I: TrendingUp },
            { l: "DII Activity",  v: "SELLING",  s: "₹1,890 Cr Net Sell",          c: C.red,     I: TrendingDown},
            { l: "Put-Call Ratio",v: "1.24",     s: "Bullish Sentiment",           c: C.primary, I: BarChart2  },
            { l: "INDIA VIX",    v: "13.45",    s: "↓ 2.3% Low Volatility",       c: C.green,   I: Activity   },
            { l: "Max Pain",     v: "23,800",   s: "Weekly Expiry",               c: C.amber,   I: Target     },
          ].map(({ l, v, s, c, I }) => (
            <div key={l} style={{ ...card({ padding: 15, borderLeft: `2px solid ${c}` }) }}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 7 }}>
                <span style={{ fontSize: 9, color: C.textMuted, fontWeight: 700,
                  letterSpacing: "0.1em", textTransform: "uppercase" }}>{l}</span>
                <I size={13} color={c} />
              </div>
              <div style={{ fontSize: 17, fontWeight: 900, color: c,
                fontFamily: "monospace", marginBottom: 3 }}>{v}</div>
              <div style={{ fontSize: 10, color: C.textMuted }}>{s}</div>
            </div>
          ))}
        </div>

        {/* scenarios */}
        <div style={card({ padding: 22 })}>
          <h3 style={{ fontSize: 15, fontWeight: 800, color: C.text, marginBottom: 18,
            display: "flex", alignItems: "center", gap: 8 }}>
            <Target size={17} color={C.primary} /> Today's Trading Scenarios
          </h3>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(260px,1fr))", gap: 13 }}>
            {[
              { t: "BULL CASE",  c: C.green,  p: "60%",
                d: "Above 23,850 → push to 24,050 likely. CE above 23,850 with SL 23,720. Target 24,000." },
              { t: "BEAR CASE",  c: C.red,    p: "25%",
                d: "Below 23,700 → bearish momentum to 23,550. PE below 23,700 with SL 23,850. Target 23,500." },
              { t: "SIDEWAYS",   c: C.amber,  p: "15%",
                d: "Range 23,700–24,000. Sell straddle at ATM. Collect theta. Avoid directional exposure." },
            ].map(({ t, c, p, d }) => (
              <div key={t} style={{ padding: 16, background: c + "07", borderRadius: 10,
                border: `1px solid ${c}1E` }}>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 9 }}>
                  <span style={{ fontSize: 11, fontWeight: 900, color: c, letterSpacing: "0.1em" }}>{t}</span>
                  <span style={{ fontSize: 11, color: c, fontFamily: "monospace", fontWeight: 800 }}>{p}</span>
                </div>
                <p style={{ fontSize: 12, color: C.textDim, lineHeight: 1.65 }}>{d}</p>
              </div>
            ))}
          </div>
        </div>
      </Wrap>

      {/* ══════════════ TRADING JOURNAL ══════════════ */}
      <Wrap id="journal" bg="rgba(0,212,255,0.025)">
        <SHeader label="Trade Journal" title="Discipline & Accountability"
          sub="Every trade logged, reviewed, and turned into a compounding learning loop." />

        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(250px,1fr))", gap: 16 }}>
          {[
            { I: Target,      t: "Discipline Tracking",    d: "Trade logged against the plan. Entry precision, setup validity, rule compliance — all on record.", stat: "94%",    sl: "Rule Compliance", c: C.primary },
            { I: Brain,       t: "Emotional Analysis",      d: "Pre & post-trade emotional state tracked. FOMO, revenge, overconfidence — documented, reviewed.", stat: "18",     sl: "Emotions Logged",  c: C.violet },
            { I: AlertCircle, t: "Mistake Review",          d: "Weekly categorized error review — premature exit, wrong timeframe, sizing error, overtrading.",    stat: "5 Types", sl: "Mistake Categories",c: C.red   },
            { I: BarChart2,   t: "Execution Screenshots",   d: "Before + after chart markup for every trade. Reinforces pattern recognition and entry precision.",  stat: "200+",   sl: "Trade Screenshots", c: C.green  },
            { I: Star,        t: "Setup Grading",           d: "A / B / C grade per setup. Only A+ setups taken. Backtested monthly against journal data.",        stat: "A+ Only", sl: "Setup Quality",    c: C.amber  },
            { I: RefreshCw,   t: "Learning Loop",           d: "Monthly review converts mistakes into rules. The trading playbook evolves with every logged lesson.",stat: "12/mo",  sl: "Rule Additions",   c: C.primary},
          ].map(({ I, t, d, stat, sl, c }) => (
            <div key={t} style={{ ...card({ padding: 20 }) }} className="sc">
              <div style={{ display: "flex", justifyContent: "space-between",
                alignItems: "flex-start", marginBottom: 12 }}>
                <div style={{ width: 40, height: 40, borderRadius: 10, background: c + "14",
                  display: "flex", alignItems: "center", justifyContent: "center" }}>
                  <I size={20} color={c} />
                </div>
                <div style={{ textAlign: "right" }}>
                  <div style={{ fontSize: 19, fontWeight: 900, color: c, fontFamily: "monospace" }}>{stat}</div>
                  <div style={{ fontSize: 9, color: C.textMuted }}>{sl}</div>
                </div>
              </div>
              <h3 style={{ fontSize: 13, fontWeight: 800, color: C.text, marginBottom: 6 }}>{t}</h3>
              <p style={{ fontSize: 11, color: C.textMuted, lineHeight: 1.6 }}>{d}</p>
            </div>
          ))}
        </div>
      </Wrap>

      {/* ══════════════ PSYCHOLOGY ══════════════ */}
      <Wrap id="psychology">
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(280px,1fr))",
          gap: 44, alignItems: "start" }}>

          <div>
            <SHeader label="Psychology" title="The Mental Game of Trading" />
            <p style={{ fontSize: 15, color: C.textDim, lineHeight: 1.8, marginBottom: 20 }}>
              90% of trading failure is psychological. The best setup means nothing if fear
              paralyzes entry or greed holds a losing position. Master your mind —
              that's the only sustainable edge retail traders have.
            </p>
            <div style={{ ...card({ padding: 20, borderLeft: `3px solid ${C.primary}` }) }}>
              <p style={{ fontSize: 14, color: C.primary, fontStyle: "italic", lineHeight: 1.75 }}>
                "The market doesn't care about your feelings. Discipline isn't optional —
                it's the only real edge retail traders hold over their own psychology."
              </p>
            </div>
            <div style={{ marginTop: 20, ...card({ padding: 18 }) }}>
              <h4 style={{ fontSize: 13, fontWeight: 700, color: C.text, marginBottom: 12 }}>
                Psychology Audit — Weekly Checklist
              </h4>
              {["Did I follow my trading plan 100%?",
                "Did FOMO influence any entry?",
                "Did I revenge-trade after a loss?",
                "Was every exit rule-based?",
                "Did I stay within daily loss limit?"].map((q, i) => (
                <div key={i} style={{ display: "flex", gap: 10, alignItems: "center",
                  padding: "6px 0", borderBottom: i < 4 ? `1px solid ${C.border}` : "none" }}>
                  <div style={{ width: 16, height: 16, borderRadius: 4,
                    border: `1px solid ${C.primary}40`, flexShrink: 0 }} />
                  <span style={{ fontSize: 12, color: C.textDim }}>{q}</span>
                </div>
              ))}
            </div>
          </div>

          <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 13 }}>
            {PSYCH.map(({ icon: Ic, title, desc, color }) => (
              <div key={title} style={{ ...card({ padding: 16 }) }} className="sc">
                <Ic size={20} color={color} style={{ marginBottom: 10 }} />
                <h3 style={{ fontSize: 13, fontWeight: 800, color: C.text, marginBottom: 5 }}>{title}</h3>
                <p style={{ fontSize: 11, color: C.textMuted, lineHeight: 1.55 }}>{desc}</p>
                <div style={{ marginTop: 12, height: 1.5,
                  background: `linear-gradient(90deg,${color},transparent)`,
                  borderRadius: 1, opacity: 0.5 }} />
              </div>
            ))}
          </div>
        </div>
      </Wrap>

      {/* ══════════════ EDUCATION ══════════════ */}
      <Wrap id="education" bg="rgba(0,212,255,0.025)">
        <SHeader label="Learn" title="Educational Content"
          sub="From absolute beginner to institutional-level trader — a structured path for every stage."
          center />

        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(270px,1fr))",
          gap: 16, marginBottom: 22 }}>
          {EDU.map(({ icon: Ic, title, count, color, tag }) => (
            <div key={title} className="edu-c" style={card({ padding: 24 })}>
              <div style={{ display: "flex", justifyContent: "space-between",
                alignItems: "flex-start", marginBottom: 16 }}>
                <div style={{ width: 50, height: 50, borderRadius: 12,
                  background: color + "14", border: `1px solid ${color}22`,
                  display: "flex", alignItems: "center", justifyContent: "center" }}>
                  <Ic size={24} color={color} />
                </div>
                <span style={{ ...badge(color), fontSize: 10 }}>{tag}</span>
              </div>
              <h3 style={{ fontSize: 15, fontWeight: 800, color: C.text, marginBottom: 5 }}>{title}</h3>
              <p style={{ fontSize: 12, color: C.textMuted, marginBottom: 16 }}>{count} modules available</p>
              <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
                <div style={{ flex: 1, height: 2.5, background: "rgba(255,255,255,0.05)",
                  borderRadius: 2, marginRight: 12 }}>
                  <div style={{ width: "58%", height: "100%", background: color, borderRadius: 2 }} />
                </div>
                <ArrowRight size={15} color={color} />
              </div>
            </div>
          ))}
        </div>

        {/* PDF notes CTA */}
        <div style={{ ...card({ padding: 24, background: "rgba(0,212,255,0.04)" }) }}>
          <div style={{ display: "flex", justifyContent: "space-between",
            alignItems: "center", flexWrap: "wrap", gap: 16 }}>
            <div>
              <h3 style={{ fontSize: 17, fontWeight: 800, color: C.text, marginBottom: 4 }}>
                📥 Free PDF Study Notes
              </h3>
              <p style={{ fontSize: 13, color: C.textMuted }}>
                Wyckoff Cheat Sheet · SMC Glossary · Option Chain Guide · Risk Calculator
              </p>
            </div>
            <button className="btn-primary"
              style={{ padding: "11px 22px", background: C.primary, color: "#020B18",
                border: "none", borderRadius: 10, fontSize: 13, fontWeight: 800,
                display: "flex", alignItems: "center", gap: 8,
                boxShadow: `0 0 22px ${C.primary}30` }}>
              Download Free Notes <ArrowRight size={15} />
            </button>
          </div>
        </div>
      </Wrap>

      {/* ══════════════ PERFORMANCE DASHBOARD ══════════════ */}
      <Wrap id="performance">
        <div ref={perfRef}>
          <SHeader label="Performance" title="Analytics Dashboard"
            sub="Real-time tracking of analysis quality, risk adherence, and trading discipline."
            center />

          {/* animated counters */}
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(200px,1fr))",
            gap: 15, marginBottom: 22 }}>
            {[
              { k: "acc",  l: "Analysis Accuracy",  s: "Directional calls correct",   c: C.primary },
              { k: "risk", l: "Risk Management",     s: "Trades within risk params",   c: C.green   },
              { k: "setup",l: "Setup Quality",       s: "A+ grade setups only",        c: C.amber   },
              { k: "disc", l: "Discipline Score",    s: "Rule adherence rate",         c: C.violet  },
            ].map(({ k, l, s, c }) => (
              <div key={k} style={{ ...card({ padding: 24, textAlign: "center",
                borderTop: `2px solid ${c}` }) }}>
                <div style={{ fontSize: 50, fontWeight: 900, fontFamily: "monospace",
                  lineHeight: 1, ...glowNum(c) }}>{counts[k]}%</div>
                <div style={{ fontSize: 13, fontWeight: 800, color: C.text, marginTop: 8, marginBottom: 4 }}>{l}</div>
                <div style={{ fontSize: 10, color: C.textMuted }}>{s}</div>
              </div>
            ))}
          </div>

          {/* monthly bar chart */}
          <div style={card({ padding: 24 })}>
            <h3 style={{ fontSize: 14, fontWeight: 800, color: C.text, marginBottom: 20,
              display: "flex", alignItems: "center", gap: 8 }}>
              <BarChart2 size={17} color={C.primary} /> Monthly Win Rate (%)
            </h3>
            <ResponsiveContainer width="100%" height={190}>
              <BarChart data={MONTHLY} barCategoryGap="36%">
                <XAxis dataKey="m" tick={{ fill: C.textMuted, fontSize: 11 }}
                  axisLine={false} tickLine={false} />
                <YAxis domain={[60, 100]} unit="%" tick={{ fill: C.textMuted, fontSize: 11 }}
                  axisLine={false} tickLine={false} />
                <Tooltip contentStyle={{ background: "#051220",
                  border: `1px solid ${C.border}`, borderRadius: 8, fontSize: 11 }}
                  cursor={{ fill: "rgba(0,212,255,0.05)" }}
                  formatter={v => [`${v}%`, "Win Rate"]} />
                <Bar dataKey="w" fill={C.primary} radius={[4, 4, 0, 0]} fillOpacity={0.85} />
              </BarChart>
            </ResponsiveContainer>
          </div>

          {/* mini stats row */}
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(150px,1fr))",
            gap: 12, marginTop: 15 }}>
            {[
              { l: "Avg R:R",           v: "1 : 2.8",  c: C.green   },
              { l: "Win Rate",          v: "68%",       c: C.primary },
              { l: "Max Drawdown",      v: "4.2%",      c: C.amber   },
              { l: "Avg Daily Trades",  v: "2.4",       c: C.violet  },
              { l: "Best Month",        v: "+18.4%",    c: C.green   },
              { l: "Sharpe Ratio",      v: "2.1",       c: C.primary },
            ].map(({ l, v, c }) => (
              <div key={l} style={{ ...card({ padding: 14, textAlign: "center" }) }}>
                <div style={{ fontSize: 10, color: C.textMuted, marginBottom: 5,
                  fontWeight: 600, letterSpacing: "0.08em", textTransform: "uppercase" }}>{l}</div>
                <div style={{ fontSize: 19, fontWeight: 900, color: c,
                  fontFamily: "monospace" }}>{v}</div>
              </div>
            ))}
          </div>
        </div>
      </Wrap>

      {/* ══════════════ CONTACT ══════════════ */}
      <Wrap id="contact" bg="rgba(0,212,255,0.025)">
        <div style={{ maxWidth: 760, margin: "0 auto", textAlign: "center" }}>
          <SHeader label="Connect" title="Let's Talk Markets" center />

          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit,minmax(130px,1fr))",
            gap: 13, marginBottom: 28 }}>
            {[
              { I: Mail,      l: "Email",    h: "thetreder@gmail.com", c: C.primary,  href: "mailto:thetreder@gmail.com" },
              { I: Instagram, l: "Instagram",h: "@the_treder",          c: "#E1306C",  href: "#"   },
              { I: Youtube,   l: "YouTube",  h: "The Treder Hub",       c: "#FF0000",  href: "#"   },
              { I: Send,      l: "Telegram", h: "@thetreder",           c: "#0088CC",  href: "#"   },
              { I: Linkedin,  l: "LinkedIn", h: "The Treder",           c: "#0A66C2",  href: "#"   },
            ].map(({ I, l, h, c, href }) => (
              <a key={l} href={href} className="cc"
                style={{ ...card({ padding: 20, textDecoration: "none", display: "block", textAlign: "center" }) }}>
                <div style={{ width: 46, height: 46, borderRadius: 12, background: c + "15",
                  border: `1px solid ${c}22`, display: "flex", alignItems: "center",
                  justifyContent: "center", margin: "0 auto 11px" }}>
                  <I size={21} color={c} />
                </div>
                <div style={{ fontSize: 13, fontWeight: 800, color: C.text, marginBottom: 3 }}>{l}</div>
                <div style={{ fontSize: 10, color: C.textMuted }}>{h}</div>
              </a>
            ))}
          </div>

          {/* quick contact note */}
          <div style={{ ...card({ padding: 22 }) }}>
            <p style={{ fontSize: 14, color: C.textDim, lineHeight: 1.75 }}>
              For <strong style={{ color: C.primary }}>collaborations</strong>,
              {" "}<strong style={{ color: C.green }}>mentorship enquiries</strong>,
              {" "}<strong style={{ color: C.amber }}>institutional analysis partnerships</strong>
              {" "} or daily analysis access — reach out via any of the channels above.
              Serious traders and learners only. 🙏
            </p>
          </div>
        </div>
      </Wrap>

      {/* ══════════════ FOOTER ══════════════ */}
      <footer style={{ borderTop: `1px solid ${C.border}`, padding: "30px 20px" }}>
        <div style={{ maxWidth: 1180, margin: "0 auto" }}>
          <div style={{ display: "flex", justifyContent: "space-between",
            alignItems: "center", flexWrap: "wrap", gap: 14, marginBottom: 20 }}>

            <a href="#home" style={{ display: "flex", alignItems: "center",
              gap: 8, textDecoration: "none" }}>
              <div style={{ width: 28, height: 28, borderRadius: 7,
                background: `linear-gradient(135deg,${C.primary},#006688)`,
                display: "flex", alignItems: "center", justifyContent: "center" }}>
                <Activity size={15} color="#020B18" strokeWidth={2.8} />
              </div>
              <span style={{ fontSize: 15, fontWeight: 900, color: C.text }}>
                <span style={{ color: C.primary }}>The</span> Treder Hub
              </span>
            </a>

            <div style={{ display: "flex", gap: 20, flexWrap: "wrap" }}>
              {NAV.map(l => (
                <a key={l.href} href={l.href} className="nav-a">{l.label}</a>
              ))}
            </div>
          </div>

          <div style={{ borderTop: `1px solid ${C.border}`, paddingTop: 18,
            display: "flex", justifyContent: "space-between",
            alignItems: "flex-start", flexWrap: "wrap", gap: 12 }}>
            <p style={{ fontSize: 11, color: C.textMuted }}>
              © 2025 The Treder Hub · All rights reserved.
            </p>
            <p style={{ fontSize: 10, color: C.textMuted, maxWidth: 560,
              textAlign: "right", lineHeight: 1.6 }}>
              <strong style={{ color: C.amber }}>⚠ DISCLAIMER:</strong>{" "}
              Trading in financial instruments involves substantial risk of loss and may
              not be suitable for all investors. Past performance is not indicative of
              future results. This website is strictly for <strong>educational purposes only</strong>
              {" "}and does not constitute financial, investment, or trading advice.
              Always consult a SEBI-registered advisor before investing.
            </p>
          </div>
        </div>
      </footer>
    </div>
  );
}
