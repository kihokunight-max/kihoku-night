![Uploading guest.png…]()
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>きほくナイト — 自分と地域をちょっと良くする、ゆるい持ち寄り会議。</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@400;500;600;700&family=Noto+Sans+JP:wght@400;500;700&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            navy: { DEFAULT: '#1e293b', dark: '#0f172a' },
            gold: { DEFAULT: '#fbbf24', deep: '#f59e0b' },
            ink: { DEFAULT: '#4a4a4a', dark: '#2d2d2d' },
            cream: { DEFAULT: '#fffbeb', light: '#f8fafc' },
          },
          fontFamily: {
            serif: ['"Noto Serif JP"', 'serif'],
            sans: ['"Noto Sans JP"', 'sans-serif'],
          },
        }
      }
    }
  </script>
  <style>
    body { font-family: 'Noto Serif JP', serif; background-color: #fffbeb; }

    /* paper texture overlay */
    .paper-texture {
      position: relative;
    }
    .paper-texture::before {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 0;
    }

    /* hand-drawn underline */
    .hand-underline {
      text-decoration: none;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='8'%3E%3Cpath d='M0 5 Q20 2 40 5 Q60 8 80 4 Q100 1 120 5 Q140 9 160 4 Q180 1 200 5' stroke='%23fbbf24' stroke-width='2.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
      background-repeat: repeat-x;
      background-position: bottom;
      background-size: 200px 8px;
      padding-bottom: 8px;
    }

    /* organic blob shape */
    .blob-bg {
      clip-path: polygon(5% 0%, 95% 2%, 100% 90%, 80% 100%, 20% 98%, 0% 88%);
    }

    /* star shimmer animation */
    @keyframes shimmer {
      0%, 100% { opacity: 0.4; transform: scale(1); }
      50% { opacity: 1; transform: scale(1.15); }
    }
    .star-shimmer { animation: shimmer 2.5s ease-in-out infinite; }
    .star-shimmer-2 { animation: shimmer 3.2s ease-in-out infinite 0.8s; }
    .star-shimmer-3 { animation: shimmer 2.8s ease-in-out infinite 1.5s; }

    @keyframes float {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-8px); }
    }
    .float-anim { animation: float 4s ease-in-out infinite; }

    .section-divider {
      width: 60px;
      height: 3px;
      background: linear-gradient(90deg, #fbbf24, #f59e0b);
      border-radius: 4px;
      margin: 0 auto 1.5rem;
    }

    /* card hover */
    .rule-card {
      transition: transform 0.25s ease, box-shadow 0.25s ease;
    }
    .rule-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 12px 32px rgba(30, 41, 59, 0.15);
    }

    /* CTA button */
    .cta-btn {
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      box-shadow: 0 4px 18px rgba(251, 191, 36, 0.45);
    }
    .cta-btn:hover {
      transform: translateY(-2px) scale(1.03);
      box-shadow: 0 8px 28px rgba(251, 191, 36, 0.6);
    }
    .cta-btn:active {
      transform: translateY(0) scale(0.99);
    }
  </style>
</head>
<body>
  <div id="root"></div>

  <script type="text/babel">
    const { useState } = React;

    /* ─── Logo SVG (placeholder – swap out <img> when image_8.png is ready) ─── */
    const LogoSVG = ({ size = 140 }) => (
      <svg width={size} height={size} viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <radialGradient id="skyGrad" cx="50%" cy="60%" r="55%">
            <stop offset="0%" stopColor="#1e3a5f" />
            <stop offset="100%" stopColor="#0f172a" />
          </radialGradient>
          <radialGradient id="moonGrad" cx="40%" cy="40%" r="50%">
            <stop offset="0%" stopColor="#fef3c7" />
            <stop offset="100%" stopColor="#fbbf24" />
          </radialGradient>
        </defs>
        {/* outer circle – night sky */}
        <circle cx="100" cy="100" r="95" fill="url(#skyGrad)" stroke="#fbbf24" strokeWidth="2.5" />
        {/* stars */}
        <circle cx="50" cy="45" r="2" fill="#fef3c7" className="star-shimmer" />
        <circle cx="148" cy="38" r="1.5" fill="#fef3c7" className="star-shimmer-2" />
        <circle cx="165" cy="72" r="2.2" fill="#fef9c3" className="star-shimmer-3" />
        <circle cx="35" cy="85" r="1.4" fill="#fef3c7" className="star-shimmer" />
        <circle cx="170" cy="130" r="1.8" fill="#fef3c7" className="star-shimmer-2" />
        <circle cx="60" cy="150" r="1.5" fill="#fef9c3" className="star-shimmer-3" />
        {/* moon */}
        <circle cx="105" cy="78" r="30" fill="url(#moonGrad)" />
        <circle cx="118" cy="68" r="24" fill="#1e3a5f" />
        {/* lemon slice hint – oval organic shape */}
        <ellipse cx="100" cy="138" rx="32" ry="20" fill="#fbbf24" opacity="0.9" />
        <ellipse cx="100" cy="138" rx="24" ry="13" fill="#fef3c7" opacity="0.9" />
        <line x1="100" y1="125" x2="100" y2="151" stroke="#fbbf24" strokeWidth="1.5" />
        <line x1="87" y1="128" x2="113" y2="148" stroke="#fbbf24" strokeWidth="1.5" />
        <line x1="113" y1="128" x2="87" y2="148" stroke="#fbbf24" strokeWidth="1.5" />
        {/* text KIHOKU inside */}
        <text x="100" y="172" textAnchor="middle" fill="#fbbf24" fontSize="11" fontFamily="serif" letterSpacing="3" fontWeight="600">KIHOKU</text>
      </svg>
    );

    /* ─── Icons (lucide-style inline SVGs) ─── */
    const IconCalendar = () => (
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/>
      </svg>
    );
    const IconMapPin = () => (
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z"/><circle cx="12" cy="10" r="3"/>
      </svg>
    );
    const IconUsers = () => (
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 00-3-3.87"/><path d="M16 3.13a4 4 0 010 7.75"/>
      </svg>
    );
    const IconStar = () => (
      <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" stroke="none">
        <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
      </svg>
    );
    const IconArrowRight = () => (
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round">
        <line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/>
      </svg>
    );
    const IconShield = () => (
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
      </svg>
    );
    const IconHeart = () => (
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <path d="M20.84 4.61a5.5 5.5 0 00-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 00-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 000-7.78z"/>
      </svg>
    );
    const IconCompass = () => (
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
        <circle cx="12" cy="12" r="10"/><polygon points="16.24 7.76 14.12 14.12 7.76 16.24 9.88 9.88 16.24 7.76"/>
      </svg>
    );

    const GOOGLE_FORM_URL = "https://forms.gle/ji184ZXZ3kNQdju1A";

    /* ─── Decorative wavy divider ─── */
    const WaveDivider = ({ flip = false, fromColor = "#fffbeb", toColor = "#1e293b" }) => (
      <div style={{ lineHeight: 0, transform: flip ? 'scaleY(-1)' : 'none' }}>
        <svg viewBox="0 0 1440 60" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" style={{ display: 'block', width: '100%', height: '60px' }}>
          <path d="M0,30 C180,60 360,0 540,30 C720,60 900,0 1080,30 C1260,60 1380,10 1440,30 L1440,60 L0,60 Z" fill={toColor} />
        </svg>
      </div>
    );

    /* ─── Main App ─── */
    const App = () => {
      return (
        <div className="min-h-screen" style={{ backgroundColor: '#fffbeb', color: '#2d2d2d' }}>

          {/* ════════════════════════════════════════
              HERO SECTION
          ════════════════════════════════════════ */}
          <section
            className="relative overflow-hidden paper-texture"
            style={{ background: 'linear-gradient(160deg, #0f172a 0%, #1e293b 60%, #1e3a5f 100%)' }}
          >
            {/* decorative star dots */}
            <div className="absolute inset-0 overflow-hidden pointer-events-none">
              {[
                [12, 8], [25, 15], [75, 5], [88, 12], [95, 25],
                [8, 35], [18, 55], [90, 45], [82, 68], [70, 10],
                [45, 20], [55, 8], [35, 30], [60, 85], [15, 75],
              ].map(([x, y], i) => (
                <div
                  key={i}
                  className={`absolute rounded-full bg-amber-100 ${i % 3 === 0 ? 'star-shimmer' : i % 3 === 1 ? 'star-shimmer-2' : 'star-shimmer-3'}`}
                  style={{
                    left: `${x}%`, top: `${y}%`,
                    width: `${(i % 3) + 2}px`, height: `${(i % 3) + 2}px`,
                    opacity: 0.5,
                  }}
                />
              ))}
            </div>

            <div className="relative z-10 flex flex-col items-center justify-center px-6 py-20 text-center">
              {/* Logo */}
              <div className="float-anim mb-8">
                <div style={{
                  background: 'rgba(255,251,235,0.97)',
                  borderRadius: '50%',
                  padding: '12px',
                  boxShadow: '0 0 40px rgba(251,191,36,0.35), 0 4px 24px rgba(0,0,0,0.3)',
                  display: 'inline-block',
                }}>
                  <img src="logo.png" alt="きほくナイト" width="180" height="180"
                    style={{ display: 'block', borderRadius: '50%', objectFit: 'cover' }} />
                </div>
              </div>

              {/* Catchcopy */}
              <p className="font-serif text-amber-100 mb-5 max-w-lg leading-relaxed"
                style={{ fontSize: 'clamp(1rem, 3.5vw, 1.35rem)' }}>
                自分と地域をちょっと良くする、<br />
                <span className="hand-underline text-amber-300 font-semibold">ゆるい持ち寄り会議。</span>
              </p>

              {/* Lead text */}
              <p className="text-slate-300 max-w-md mb-10 leading-relaxed"
                style={{ fontSize: 'clamp(0.85rem, 2.5vw, 1rem)' }}>
                境界を越えて、このエリアを面白がるプレイヤーの<br className="hidden sm:block" />夜のたまり場。
              </p>

              {/* CTA Button */}
              <a
                href={GOOGLE_FORM_URL}
                target="_blank"
                rel="noopener noreferrer"
                className="cta-btn inline-flex items-center gap-2 px-8 py-4 rounded-full font-bold text-navy-dark"
                style={{ fontSize: 'clamp(0.95rem, 2.5vw, 1.1rem)', color: '#0f172a' }}
              >
                Vol.1 に参加申し込む
                <IconArrowRight />
              </a>

              <p className="mt-3 text-slate-400 text-sm">2026年2月27日（金）開催 ／ 定員10名</p>
            </div>

            {/* wave bottom */}
            <WaveDivider toColor="#fffbeb" />
          </section>

          {/* ════════════════════════════════════════
              INTRODUCTION
          ════════════════════════════════════════ */}
          <section className="py-20 px-6 paper-texture" style={{ backgroundColor: '#fffbeb' }}>
            <div className="max-w-2xl mx-auto text-center">
              <div className="flex items-center justify-center gap-2 mb-3">
                <span className="text-amber-400"><IconStar /></span>
                <span className="text-amber-500 text-sm font-sans tracking-widest uppercase">About</span>
                <span className="text-amber-400"><IconStar /></span>
              </div>
              <div className="section-divider"></div>
              <h2 className="font-serif font-bold text-navy mb-8"
                style={{ fontSize: 'clamp(1.5rem, 4vw, 2rem)', color: '#1e293b' }}>
                きほくナイトとは？
              </h2>

              <div className="relative">
                {/* decorative quote mark */}
                <span className="absolute -top-6 -left-2 text-amber-200 font-serif select-none"
                  style={{ fontSize: '6rem', lineHeight: 1, opacity: 0.5 }}>"</span>

                <div className="relative z-10 text-left space-y-5 leading-8"
                  style={{ color: '#4a4a4a', fontSize: 'clamp(0.9rem, 2.5vw, 1.05rem)' }}>
                  <p>
                    「きほくナイト」は、行政の枠も、仕事の肩書きも、ちょっと横に置いて、
                    月に一度集まる<strong className="text-navy" style={{color:'#1e293b'}}>ゆるい作戦会議</strong>です。
                  </p>
                  <p>
                    一人のプレイヤーとしての「できた！」という喜びや、「どうする？」という問いかけを持ち寄り、
                    対話の中で混ぜ合わせる。
                  </p>
                  <p>
                    終わる頃には、<span className="hand-underline" style={{backgroundImage: "url(\"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='8'%3E%3Cpath d='M0 5 Q20 2 40 5 Q60 8 80 4 Q100 1 120 5 Q140 9 160 4 Q180 1 200 5' stroke='%23fbbf24' stroke-width='2.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E\")"}}>明日の一歩が少しだけ軽くなっている</span>。
                    そんな夜を目指しています。
                  </p>
                </div>
              </div>
            </div>
          </section>

          {/* wave */}
          <div style={{ lineHeight: 0, backgroundColor: '#fffbeb' }}>
            <svg viewBox="0 0 1440 50" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" style={{ display: 'block', width: '100%', height: '50px' }}>
              <path d="M0,25 C200,50 400,0 600,25 C800,50 1000,0 1200,25 C1300,37 1380,15 1440,25 L1440,50 L0,50 Z" fill="#fef3c7" />
            </svg>
          </div>

          {/* ════════════════════════════════════════
              THREE RULES
          ════════════════════════════════════════ */}
          <section className="py-20 px-6" style={{ backgroundColor: '#fef3c7' }}>
            <div className="max-w-4xl mx-auto">
              <div className="text-center mb-12">
                <div className="flex items-center justify-center gap-2 mb-3">
                  <span className="text-amber-500"><IconStar /></span>
                  <span className="text-amber-600 text-sm font-sans tracking-widest uppercase">Rules</span>
                  <span className="text-amber-500"><IconStar /></span>
                </div>
                <div className="section-divider"></div>
                <h2 className="font-serif font-bold"
                  style={{ fontSize: 'clamp(1.4rem, 4vw, 1.9rem)', color: '#1e293b' }}>
                  きほくナイトの「3つの鉄則」
                </h2>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                {[
                  {
                    num: '01',
                    icon: <IconShield />,
                    title: '肩書きにとらわれない',
                    body: '専門性はリスペクトしつつ、立場の壁を越えてフラットに語る。',
                  },
                  {
                    num: '02',
                    icon: <IconHeart />,
                    title: '成功も悩みも、等身大で',
                    body: 'カッコいい「できた！」も、泥臭い「困った」も、みんなの学びに変える。',
                  },
                  {
                    num: '03',
                    icon: <IconCompass />,
                    title: '越境を面白がる',
                    body: '自分の町、自分の業種の外側にこそ、ブレイクスルーの鍵がある。境界を越える刺激を楽しむ。',
                  },
                ].map(({ num, icon, title, body }) => (
                  <div
                    key={num}
                    className="rule-card rounded-2xl p-7 relative overflow-hidden"
                    style={{ backgroundColor: '#fffbeb', border: '1.5px solid #fde68a', boxShadow: '0 4px 20px rgba(30,41,59,0.08)' }}
                  >
                    {/* number watermark */}
                    <span
                      className="absolute top-3 right-4 font-serif font-bold select-none"
                      style={{ fontSize: '4.5rem', color: '#fde68a', lineHeight: 1 }}
                    >{num}</span>

                    <div className="relative z-10">
                      <div className="mb-4" style={{ color: '#1e293b' }}>{icon}</div>
                      <h3 className="font-serif font-bold text-navy mb-3"
                        style={{ fontSize: '1.05rem', color: '#1e293b' }}>
                        {title}
                      </h3>
                      <p className="leading-7 text-sm" style={{ color: '#4a4a4a' }}>{body}</p>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </section>

          {/* wave */}
          <div style={{ lineHeight: 0, backgroundColor: '#fef3c7' }}>
            <svg viewBox="0 0 1440 50" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" style={{ display: 'block', width: '100%', height: '50px' }}>
              <path d="M0,0 C360,50 720,0 1080,50 C1260,75 1380,20 1440,0 L1440,50 L0,50 Z" fill="#fffbeb" />
            </svg>
          </div>

          {/* ════════════════════════════════════════
              EVENT INFO Vol.1
          ════════════════════════════════════════ */}
          <section className="py-20 px-6 paper-texture" style={{ backgroundColor: '#fffbeb' }}>
            <div className="max-w-3xl mx-auto">
              <div className="text-center mb-12">
                <div className="flex items-center justify-center gap-2 mb-3">
                  <span className="text-amber-400"><IconStar /></span>
                  <span className="text-amber-500 text-sm font-sans tracking-widest uppercase">Event</span>
                  <span className="text-amber-400"><IconStar /></span>
                </div>
                <div className="section-divider"></div>
                <h2 className="font-serif font-bold"
                  style={{ fontSize: 'clamp(1.4rem, 4vw, 1.9rem)', color: '#1e293b' }}>
                  最新イベント情報
                </h2>
              </div>

              {/* Event Card */}
              <div
                className="rounded-3xl overflow-hidden"
                style={{ border: '2px solid #fbbf24', boxShadow: '0 8px 40px rgba(30,41,59,0.12)' }}
              >
                {/* Card header */}
                <div
                  className="px-8 py-5 flex items-center justify-between flex-wrap gap-3"
                  style={{ background: 'linear-gradient(135deg, #1e293b, #0f172a)' }}
                >
                  <div className="flex items-center gap-3">
                    <span
                      className="px-3 py-1 rounded-full text-xs font-bold font-sans tracking-wider"
                      style={{ backgroundColor: '#fbbf24', color: '#0f172a' }}
                    >NEW</span>
                    <span
                      className="px-3 py-1 rounded-full text-xs font-bold font-sans tracking-wider"
                      style={{ backgroundColor: 'rgba(251,191,36,0.2)', color: '#fbbf24', border: '1px solid #fbbf24' }}
                    >募集中</span>
                  </div>
                  <span className="text-amber-400 font-sans text-sm tracking-widest">Vol.1</span>
                </div>

                {/* Card body */}
                <div className="p-8" style={{ backgroundColor: '#fff' }}>
                  <h3
                    className="font-serif font-bold leading-snug mb-3"
                    style={{ fontSize: 'clamp(1.1rem, 3.5vw, 1.5rem)', color: '#1e293b' }}
                  >
                    きほくナイト vol.1<br />
                    <span className="text-amber-500">「『伝える』の解像度を上げる」</span>
                  </h3>
                  <p className="text-sm mb-6 leading-relaxed" style={{ color: '#4a4a4a' }}>
                    〜広告とPRの違いを学び、自分たちの活動や地域をもっとクリアに届ける方法を語る〜
                  </p>

                  {/* Details */}
                  <div className="space-y-3 mb-8">
                    {[
                      { icon: <IconCalendar />, text: '2026年2月27日（金） 19:00 〜 21:00' },
                      { icon: <IconMapPin />, text: 'まちライブラリー＠きほく' },
                      { icon: <IconUsers />, text: '定員 10名（先着順）' },
                    ].map(({ icon, text }, i) => (
                      <div key={i} className="flex items-center gap-3 text-sm" style={{ color: '#2d2d2d' }}>
                        <span style={{ color: '#f59e0b', flexShrink: 0 }}>{icon}</span>
                        <span className="font-sans">{text}</span>
                      </div>
                    ))}
                  </div>

                  {/* Divider */}
                  <div className="my-6" style={{ height: '1px', background: 'linear-gradient(90deg, transparent, #fde68a, transparent)' }} />

                  {/* Guest */}
                  <div className="flex flex-col sm:flex-row items-start sm:items-center gap-5">
                    {/* Guest photo */}
                    <div className="flex-shrink-0">
                      <div
                        className="rounded-2xl overflow-hidden"
                        style={{ width: '100px', height: '100px', border: '3px solid #fbbf24' }}
                      >
                        <img
                          src="guest.png"
                          alt="坂木 勇介 氏"
                          style={{ width: '100%', height: '100%', objectFit: 'cover', objectPosition: 'center 15%' }}
                          onError={(e) => {
                            e.target.style.display = 'none';
                            e.target.parentNode.style.background = 'linear-gradient(135deg, #1e293b, #1e3a5f)';
                            e.target.parentNode.style.display = 'flex';
                            e.target.parentNode.style.alignItems = 'center';
                            e.target.parentNode.style.justifyContent = 'center';
                            e.target.parentNode.innerHTML = '<span style="color:#fbbf24;font-size:2rem">👤</span>';
                          }}
                        />
                      </div>
                    </div>

                    {/* Guest info */}
                    <div>
                      <span className="inline-block text-xs font-sans tracking-widest px-2 py-0.5 rounded mb-2"
                        style={{ backgroundColor: '#fef3c7', color: '#92400e', border: '1px solid #fde68a' }}>
                        ゲスト
                      </span>
                      <p className="font-serif font-bold text-lg" style={{ color: '#1e293b' }}>
                        坂木 勇介 氏
                      </p>
                      <p className="text-sm font-sans" style={{ color: '#f59e0b' }}>
                        PRプロデューサー / 紀北町地域活性化起業人
                      </p>
                      <p className="text-sm mt-2 leading-relaxed" style={{ color: '#4a4a4a' }}>
                        東京・虎ノ門を拠点に活動するPRのプロフェッショナル。紀北町の地域活性化起業人として、ローカルな取り組みを社会に「届ける技術」を磨き続けている。情報の整理と発信の解像度を上げることで、地域と都市をつなぐ架け橋を担う。
                      </p>
                    </div>
                  </div>

                  {/* CTA */}
                  <div className="mt-8 text-center">
                    <a
                      href={GOOGLE_FORM_URL}
                      target="_blank"
                      rel="noopener noreferrer"
                      className="cta-btn inline-flex items-center gap-2 px-8 py-4 rounded-full font-bold"
                      style={{ color: '#0f172a', fontSize: '1rem' }}
                    >
                      参加申し込みはこちら
                      <IconArrowRight />
                    </a>
                    <p className="mt-3 text-xs font-sans" style={{ color: '#9ca3af' }}>
                      Googleフォームに遷移します。参加費無料。
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </section>

          {/* wave to footer */}
          <div style={{ lineHeight: 0, backgroundColor: '#fffbeb' }}>
            <svg viewBox="0 0 1440 60" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" style={{ display: 'block', width: '100%', height: '60px' }}>
              <path d="M0,30 C180,60 360,0 540,30 C720,60 900,0 1080,30 C1260,60 1380,10 1440,30 L1440,60 L0,60 Z" fill="#0f172a" />
            </svg>
          </div>

          {/* ════════════════════════════════════════
              FOOTER
          ════════════════════════════════════════ */}
          <footer
            className="relative paper-texture py-16 px-6"
            style={{ background: 'linear-gradient(160deg, #0f172a 0%, #1e293b 100%)' }}
          >
            {/* stars in footer */}
            <div className="absolute inset-0 overflow-hidden pointer-events-none">
              {[[10,20],[30,40],[70,15],[85,35],[50,55],[20,70],[65,75],[90,60]].map(([x,y],i) => (
                <div key={i} className={`absolute rounded-full bg-amber-100 ${i%2===0?'star-shimmer':'star-shimmer-2'}`}
                  style={{ left:`${x}%`, top:`${y}%`, width:'2px', height:'2px', opacity:0.3 }} />
              ))}
            </div>

            <div className="relative z-10 max-w-2xl mx-auto text-center">
              {/* Logo */}
              <div className="float-anim inline-block mb-6">
                <div style={{
                  background: 'rgba(255,251,235,0.97)',
                  borderRadius: '50%',
                  padding: '8px',
                  boxShadow: '0 0 28px rgba(251,191,36,0.3), 0 4px 16px rgba(0,0,0,0.3)',
                  display: 'inline-block',
                }}>
                  <img src="logo.png" alt="きほくナイト" width="110" height="110"
                    style={{ display: 'block', borderRadius: '50%', objectFit: 'cover' }} />
                </div>
              </div>

              <p className="text-slate-400 text-sm mb-8">
                自分と地域をちょっと良くする、ゆるい持ち寄り会議。
              </p>

              {/* Final CTA */}
              <a
                href={GOOGLE_FORM_URL}
                target="_blank"
                rel="noopener noreferrer"
                className="cta-btn inline-flex items-center gap-2 px-8 py-4 rounded-full font-bold mb-10"
                style={{ color: '#0f172a', fontSize: '1rem' }}
              >
                Vol.1 に参加申し込む
                <IconArrowRight />
              </a>

              {/* Divider */}
              <div style={{ height: '1px', background: 'linear-gradient(90deg, transparent, rgba(251,191,36,0.3), transparent)', marginBottom: '1.5rem' }} />

              <p className="text-slate-500 text-xs font-sans">
                © きほくナイト実行委員会
              </p>
            </div>
          </footer>

        </div>
      );
    };

    ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
