<!DOCTYPE html>

<html lang="en" data-theme="dark">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Poly — ENM Relationship Toolkit</title>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=Syne:wght@400;500;600&display=swap" rel="stylesheet">
	<style>
		/* ── THEMES: both fully declared, equal specificity, predictable ── */
		html[data-theme="dark"] {
			--bg: #0c0810;
			--surface: #15101e;
			--surface2: #1e1830;
			--surface3: #271f3d;
			--border: rgba(180, 150, 220, 0.15);
			--accent: #c9956e;
			--accent2: #9b7fd4;
			--accent3: #6dbfb8;
			--text: #f0e8d8;
			--muted: #9d8faa;
			--danger: #e07070;
			--success: #7ecb9a;
			--tab-active: #c9956e;
			--input-bg: #1e1830;
			--input-border: rgba(180, 150, 220, 0.2);
			--ph: rgba(157, 143, 170, 0.55);
			--progress: linear-gradient(90deg, #9b7fd4, #c9956e, #6dbfb8);
			--amb1: rgba(155, 127, 212, 0.07);
			--amb2: rgba(201, 149, 110, 0.06);
		}

		html[data-theme="light"] {
			--bg: #f5efe8;
			--surface: #fff;
			--surface2: #f0e9f5;
			--surface3: #e4d9f0;
			--border: rgba(100, 70, 140, 0.18);
			--accent: #8b4513;
			--accent2: #5e3d9e;
			--accent3: #1a7a72;
			--text: #1a0f2a;
			--muted: #6b5a7e;
			--danger: #b94040;
			--success: #2e7d52;
			--tab-active: #8b4513;
			--input-bg: #fff;
			--input-border: rgba(100, 70, 140, 0.22);
			--ph: rgba(107, 90, 126, 0.5);
			--progress: linear-gradient(90deg, #5e3d9e, #8b4513, #1a7a72);
			--amb1: rgba(120, 90, 180, 0.06);
			--amb2: rgba(150, 100, 60, 0.04);
		}

		*,
		*::before,
		*::after {
			box-sizing: border-box;
			margin: 0;
			padding: 0
		}

		body {
			font-family: ‘Syne’, sans-serif;
			background: var(–bg);
			color: var(–text);
			min-height: 100vh;
			font-size: 14px;
			line-height: 1.6;
			transition: background .3s, color .3s
		}

		body::before {
			content: ’’;
			position: fixed;
			inset: 0;
			background: radial-gradient(ellipse 60% 50% at 20% 10%, var(–amb1) 0%, transparent 60%), radial-gradient(ellipse 50% 40% at 80% 80%, var(–amb2) 0%, transparent 60%);
			pointer-events: none;
			z-index: 0
		}

		.wrap {
			position: relative;
			z-index: 1;
			max-width: 860px;
			margin: 0 auto;
			padding: 0 16px 80px
		}

		/* header */
		header {
			padding: 28px 0 0;
			display: flex;
			align-items: flex-start;
			justify-content: space-between;
			gap: 16px
		}

		h1 {
			font-family: ‘Playfair Display’, serif;
			font-size: clamp(22px, 4vw, 32px);
			font-weight: 600;
			letter-spacing: -.5px;
			line-height: 1.1;
			color: var(–text)
		}

		h1 em {
			font-style: italic;
			color: var(–accent)
		}

		.subtitle {
			font-size: 12px;
			color: var(–muted);
			margin-top: 4px;
			letter-spacing: .04em
		}

		/* theme toggle */
		.theme-btn {
			background: var(–surface2);
			border: 1.5px solid var(–border);
			border-radius: 20px;
			padding: 6px 14px;
			cursor: pointer;
			font-family: ‘Syne’, sans-serif;
			font-size: 12px;
			font-weight: 500;
			color: var(–muted);
			display: flex;
			align-items: center;
			gap: 6px;
			transition: all .2s;
			white-space: nowrap;
			flex-shrink: 0
		}

		.theme-btn:hover {
			color: var(–text);
			border-color: var(–accent2);
			background: var(–surface3)
		}

		/* progress */
		.prog-wrap {
			margin: 20px 0 0;
			background: var(–surface2);
			border: 1px solid var(–border);
			border-radius: 12px;
			padding: 12px 16px;
			display: flex;
			align-items: center;
			gap: 12px
		}

		.prog-lbl {
			font-size: 11px;
			color: var(–muted);
			white-space: nowrap;
			letter-spacing: .06em;
			text-transform: uppercase
		}

		.prog-bg {
			flex: 1;
			height: 6px;
			background: var(–surface3);
			border-radius: 10px;
			overflow: hidden
		}

		.prog-fill {
			height: 100%;
			background: var(–progress);
			border-radius: 10px;
			width: 0%;
			transition: width .5s cubic-bezier(.34, 1.56, .64, 1)
		}

		.prog-pct {
			font-size: 11px;
			color: var(–accent);
			font-weight: 700;
			min-width: 32px;
			text-align: right
		}

		/* tabs */
		.tabs {
			display: flex;
			gap: 4px;
			margin: 20px 0 0;
			overflow-x: auto;
			padding-bottom: 2px;
			scrollbar-width: none
		}

		.tabs::-webkit-scrollbar {
			display: none
		}

		.tab {
			background: var(–surface2);
			border: 1px solid var(–border);
			border-radius: 10px 10px 0 0;
			padding: 9px 16px;
			cursor: pointer;
			font-family: ‘Syne’, sans-serif;
			font-size: 12px;
			font-weight: 500;
			color: var(–muted);
			white-space: nowrap;
			transition: all .2s;
			display: flex;
			align-items: center;
			gap: 6px;
			flex-shrink: 0
		}

		.tab:hover {
			color: var(–text)
		}

		.tab.active {
			background: var(–surface);
			border-bottom-color: var(–surface);
			color: var(–tab-active);
			font-weight: 600
		}

		.dot {
			width: 6px;
			height: 6px;
			border-radius: 50%;
			background: var(–border);
			transition: background .2s;
			flex-shrink: 0;
			pointer-events: none
		}

		.tab.active .dot,
		.tab.has-data .dot {
			background: var(–accent2)
		}

		/* panels */
		.panel {
			display: none;
			background: var(–surface);
			border: 1px solid var(–border);
			border-top: none;
			border-radius: 0 12px 12px 12px;
			padding: 24px;
			animation: slide .22s ease
		}

		.panel.active {
			display: block
		}

		@keyframes slide {
			from {
				opacity: 0;
				transform: translateY(6px)
			}

			to {
				opacity: 1;
				transform: translateY(0)
			}
		}

		/* layout */
		.sec {
			margin-bottom: 28px
		}

		.sec:last-child {
			margin-bottom: 0
		}

		.sec-title {
			font-family: ‘Playfair Display’, serif;
			font-size: 16px;
			font-weight: 600;
			color: var(–text);
			margin-bottom: 4px
		}

		.sec-desc {
			font-size: 12px;
			color: var(–muted);
			margin-bottom: 14px;
			line-height: 1.55
		}

		.hr {
			height: 1px;
			background: var(–border);
			margin: 24px 0
		}

		.g2 {
			display: grid;
			grid-template-columns: 1fr 1fr;
			gap: 12px
		}

		.g3 {
			display: grid;
			grid-template-columns: 1fr 1fr 1fr;
			gap: 10px
		}

		@media(max-width:560px) {

			.g2,
			.g3 {
				grid-template-columns: 1fr
			}
		}

		.fld {
			margin-bottom: 14px
		}

		.fld:last-child {
			margin-bottom: 0
		}

		.mt8 {
			margin-top: 8px
		}

		.mt12 {
			margin-top: 12px
		}

		.mt16 {
			margin-top: 16px
		}

		.w100 {
			width: 100%
		}

		.muted {
			color: var(–muted);
			font-size: 12px
		}

		.hide {
			display: none !important
		}

		/* form elements — explicit colors in both themes */
		.lbl {
			display: block;
			font-size: 12px;
			color: var(–muted);
			margin-bottom: 5px;
			font-weight: 500;
			letter-spacing: .03em
		}

		input[type=text],
		input[type=date],
		textarea,
		select {
			width: 100%;
			background: var(–input-bg);
			border: 1px solid var(–input-border);
			border-radius: 8px;
			padding: 10px 12px;
			font-family: ‘Syne’, sans-serif;
			font-size: 13px;
			color: var(–text);
			outline: none;
			transition: border-color .2s, box-shadow .2s, background .3s, color .3s;
			-webkit-appearance: none;
			appearance: none
		}

		textarea {
			resize: vertical;
			min-height: 80px
		}

		select {
			cursor: pointer;
			background-image: url(“data:image/svg+xml,%3Csvg xmlns=‘http://www.w3.org/2000/svg’ width=‘12’ height=‘8’ viewBox=‘0 0 12 8’%3E%3Cpath fill=’%239d8faa’ d=‘M6 8L0 0h12z’/%3E%3C/svg%3E”);
			background-repeat: no-repeat;
			background-position: right 12px center;
			padding-right: 32px
		}

		html[data-theme=light] select {
			background-image: url(“data:image/svg+xml,%3Csvg xmlns=‘http://www.w3.org/2000/svg’ width=‘12’ height=‘8’ viewBox=‘0 0 12 8’%3E%3Cpath fill=’%236b5a7e’ d=‘M6 8L0 0h12z’/%3E%3C/svg%3E”)
		}

		input[type=text]:focus,
		input[type=date]:focus,
		textarea:focus,
		select:focus {
			border-color: var(–accent2);
			box-shadow: 0 0 0 3px rgba(155, 127, 212, .15)
		}

		input::placeholder,
		textarea::placeholder {
			color: var(–ph)
		}

		input[type=date]::-webkit-calendar-picker-indicator {
			filter: invert(.7);
			cursor: pointer
		}

		html[data-theme=light] input[type=date]::-webkit-calendar-picker-indicator {
			filter: invert(.3)
		}

		/* sliders */
		.sl-wrap {
			display: flex;
			align-items: center;
			gap: 10px
		}

		.sl-min,
		.sl-max {
			font-size: 10px;
			color: var(–muted);
			white-space: nowrap;
			min-width: 60px
		}

		.sl-max {
			text-align: right
		}

		input[type=range] {
			-webkit-appearance: none;
			appearance: none;
			flex: 1;
			height: 4px;
			border-radius: 10px;
			background: var(–surface3);
			outline: none;
			cursor: pointer
		}

		input[type=range]::-webkit-slider-thumb {
			-webkit-appearance: none;
			width: 18px;
			height: 18px;
			border-radius: 50%;
			background: var(–accent2);
			border: 2px solid var(–surface);
			box-shadow: 0 2px 8px rgba(0, 0, 0, .25);
			cursor: pointer;
			transition: transform .15s
		}

		input[type=range]::-webkit-slider-thumb:hover {
			transform: scale(1.18)
		}

		input[type=range]::-moz-range-thumb {
			width: 18px;
			height: 18px;
			border-radius: 50%;
			background: var(–accent2);
			border: 2px solid var(–surface);
			cursor: pointer
		}

		.val {
			font-size: 12px;
			font-weight: 700;
			color: var(–accent2);
			min-width: 24px;
			text-align: center
		}

		.sl-row {
			margin-bottom: 12px
		}

		.sl-hd {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin-bottom: 5px
		}

		.sl-hd span:first-child {
			font-size: 12px;
			color: var(–text)
		}

		/* checklists — div not label, no hidden checkbox, no double-fire */
		.checklist {
			display: flex;
			flex-direction: column;
			gap: 6px
		}

		.ci {
			display: flex;
			align-items: center;
			gap: 10px;
			padding: 8px 10px;
			border-radius: 8px;
			background: var(–surface2);
			border: 1px solid transparent;
			cursor: pointer;
			user-select: none;
			transition: all .15s;
			font-size: 13px;
			color: var(–text)
		}

		.ci:hover {
			border-color: var(–border)
		}

		.ci.on {
			border-color: var(–accent2);
			background: rgba(155, 127, 212, .08)
		}

		.cb {
			width: 18px;
			height: 18px;
			flex-shrink: 0;
			border-radius: 4px;
			border: 1.5px solid var(–border);
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: 11px;
			color: transparent;
			transition: all .15s;
			pointer-events: none
		}

		.ci.on .cb {
			background: var(–accent2);
			border-color: var(–accent2);
			color: #fff
		}

		/* pills */
		.pills {
			display: flex;
			flex-wrap: wrap;
			gap: 6px
		}

		.pill {
			padding: 5px 12px;
			border-radius: 20px;
			border: 1.5px solid var(–border);
			background: var(–surface2);
			font-size: 12px;
			cursor: pointer;
			transition: all .15s;
			color: var(–muted);
			user-select: none
		}

		.pill:hover {
			border-color: var(–accent2);
			color: var(–text)
		}

		.pill.on {
			background: rgba(155, 127, 212, .15);
			border-color: var(–accent2);
			color: var(–accent2);
			font-weight: 600
		}

		/* cards */
		.card {
			background: var(–surface2);
			border: 1px solid var(–border);
			border-radius: 10px;
			padding: 14px 16px;
			margin-bottom: 10px
		}

		.card-hd {
			font-size: 13px;
			font-weight: 600;
			color: var(–text);
			margin-bottom: 10px;
			display: flex;
			align-items: center;
			justify-content: space-between
		}

		.rm-btn {
			background: none;
			border: none;
			cursor: pointer;
			color: var(–muted);
			font-size: 18px;
			line-height: 1;
			padding: 0 2px;
			transition: color .2s
		}

		.rm-btn:hover {
			color: var(–danger)
		}

		/* partner cards */
		.pc {
			background: var(–surface2);
			border: 1px solid var(–border);
			border-radius: 12px;
			padding: 16px;
			margin-bottom: 14px;
			transition: border-color .2s
		}

		.pc:hover {
			border-color: rgba(155, 127, 212, .3)
		}

		.pc-hd {
			display: flex;
			align-items: center;
			justify-content: space-between;
			margin-bottom: 16px
		}

		.pname {
			font-size: 15px !important;
			font-weight: 700 !important;
			background: transparent !important;
			border: none !important;
			border-bottom: 1.5px solid var(–border) !important;
			border-radius: 0 !important;
			padding: 4px 0 !important;
			color: var(–accent) !important;
			width: 200px
		}

		.pname:focus {
			box-shadow: none !important;
			border-bottom-color: var(–accent2) !important;
			outline: none !important
		}

		.badge {
			display: inline-flex;
			align-items: center;
			justify-content: center;
			width: 30px;
			height: 30px;
			border-radius: 50%;
			font-size: 11px;
			font-weight: 700;
			border: 2px solid transparent
		}

		.b-lo {
			background: rgba(224, 112, 112, .15);
			color: var(–danger);
			border-color: rgba(224, 112, 112, .3)
		}

		.b-mi {
			background: rgba(201, 149, 110, .15);
			color: var(–accent);
			border-color: rgba(201, 149, 110, .3)
		}

		.b-hi {
			background: rgba(126, 203, 154, .15);
			color: var(–success);
			border-color: rgba(126, 203, 154, .3)
		}

		/* buttons — always type=button to prevent submit */
		.btn {
			display: inline-flex;
			align-items: center;
			gap: 7px;
			padding: 9px 18px;
			border-radius: 8px;
			border: 1.5px solid transparent;
			font-family: ‘Syne’, sans-serif;
			font-size: 13px;
			font-weight: 600;
			cursor: pointer;
			transition: all .2s;
			letter-spacing: .02em
		}

		.btn-pri {
			background: var(–accent2);
			color: #fff;
			border-color: var(–accent2)
		}

		.btn-pri:hover {
			opacity: .85;
			transform: translateY(-1px)
		}

		.btn-out {
			background: transparent;
			border-color: var(–border);
			color: var(–muted)
		}

		.btn-out:hover {
			border-color: var(–accent2);
			color: var(–accent2)
		}

		.btn-acc {
			background: var(–accent);
			color: #fff;
			border-color: var(–accent)
		}

		.btn-acc:hover {
			opacity: .85;
			transform: translateY(-1px)
		}

		/* insight */
		.insight {
			background: linear-gradient(135deg, rgba(155, 127, 212, .08), rgba(201, 149, 110, .06));
			border: 1px solid rgba(155, 127, 212, .2);
			border-radius: 10px;
			padding: 14px 16px;
			font-size: 13px;
			color: var(–text);
			line-height: 1.6;
			margin-top: 14px
		}

		.insight::before {
			content: ’✦ ’;
			color: var(–accent2);
			font-size: 10px
		}

		.i-lbl {
			font-size: 10px;
			text-transform: uppercase;
			letter-spacing: .1em;
			color: var(–accent2);
			font-weight: 700;
			margin-bottom: 6px
		}

		/* feelings grid — pointer-events:none on children prevents click confusion */
		.f-grid {
			display: grid;
			grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
			gap: 6px;
			margin-top: 10px
		}

		.f-btn {
			padding: 8px 6px;
			border-radius: 8px;
			border: 1.5px solid var(–border);
			background: var(–surface2);
			font-family: ‘Syne’, sans-serif;
			font-size: 12px;
			cursor: pointer;
			color: var(–muted);
			text-align: center;
			transition: all .15s;
			display: flex;
			flex-direction: column;
			align-items: center;
			gap: 4px;
			line-height: 1.3
		}

		.f-btn * {
			pointer-events: none
		}

		/* fix: child spans were intercepting clicks */
		.f-btn:hover {
			border-color: var(–accent2);
			color: var(–text)
		}

		.f-btn.on {
			border-color: var(–accent2);
			background: rgba(155, 127, 212, .12);
			color: var(–text);
			font-weight: 600
		}

		.emoji {
			font-size: 20px
		}

		/* stats */
		.stat {
			background: var(–surface2);
			border: 1px solid var(–border);
			border-radius: 10px;
			padding: 12px 14px;
			text-align: center
		}

		.stat-n {
			font-family: ‘Playfair Display’, serif;
			font-size: 28px;
			font-weight: 600;
			color: var(–accent2);
			line-height: 1
		}

		.stat-l {
			font-size: 11px;
			color: var(–muted);
			margin-top: 4px
		}

		/* pattern bars */
		.pbar {
			margin-bottom: 10px
		}

		.pbar-lbl {
			display: flex;
			justify-content: space-between;
			font-size: 12px;
			color: var(–muted);
			margin-bottom: 4px
		}

		.pbar-bg {
			height: 8px;
			background: var(–surface3);
			border-radius: 10px;
			overflow: hidden
		}

		.pbar-fill {
			height: 100%;
			border-radius: 10px;
			transition: width .6s cubic-bezier(.34, 1.56, .64, 1)
		}

		/* window of tolerance */
		.wot-spec {
			height: 10px;
			border-radius: 10px;
			background: linear-gradient(90deg, #e07070 0%, #c9956e 30%, #7ecb9a 55%, #9b7fd4 78%, #6dbfb8 100%);
			margin-top: 8px;
			position: relative
		}

		.wot-cur {
			position: absolute;
			top: -3px;
			width: 16px;
			height: 16px;
			border-radius: 50%;
			background: var(–surface);
			border: 2px solid var(–accent2);
			transform: translateX(-50%);
			transition: left .3s cubic-bezier(.34, 1.56, .64, 1);
			box-shadow: 0 2px 8px rgba(0, 0, 0, .35);
			pointer-events: none
		}

		/* export bar */
		.bar {
			position: fixed;
			bottom: 0;
			left: 0;
			right: 0;
			background: var(–surface);
			border-top: 1px solid var(–border);
			padding: 10px 20px;
			display: flex;
			align-items: center;
			justify-content: space-between;
			gap: 10px;
			z-index: 100;
			backdrop-filter: blur(14px);
			transition: background .3s, border-color .3s
		}

		.bar-info {
			font-size: 11px;
			color: var(–muted)
		}

		.bar-info span {
			color: var(–accent2);
			font-weight: 600
		}

		.bar-btns {
			display: flex;
			gap: 8px
		}

		/* toast */
		.toast {
			position: fixed;
			bottom: 68px;
			right: 16px;
			background: var(–surface3);
			border: 1px solid var(–accent2);
			color: var(–text);
			padding: 10px 16px;
			border-radius: 8px;
			font-size: 13px;
			z-index: 200;
			opacity: 0;
			transform: translateY(8px);
			transition: all .28s;
			pointer-events: none
		}

		.toast.on {
			opacity: 1;
			transform: translateY(0)
		}

		.sub {
			font-size: 11px;
			color: var(–muted);
			text-transform: uppercase;
			letter-spacing: .06em;
			font-weight: 600;
			margin-bottom: 8px
		}
	</style>

</head>

<body>
	<div class="wrap">

		<header>
			<div>
				<h1>Poly <em>Compass</em></h1>
				<p class="subtitle">ENM Relationship Toolkit · nervous system · connection · agreements</p>
			</div>
			<button type="button" class="theme-btn" id="themeBtn">
				<span id="themeIco">☀️</span><span id="themeTxt">Light</span>
			</button>
		</header>

		<div class="prog-wrap">
			<div class="prog-lbl">Completion</div>
			<div class="prog-bg">
				<div class="prog-fill" id="pFill"></div>
			</div>
			<div class="prog-pct" id="pPct">0%</div>
		</div>

		<div class="tabs" role="tablist">
			<button type="button" class="tab active" data-tab="you"><span class="dot"></span> 🧠 You</button>
			<button type="button" class="tab" data-tab="people"><span class="dot"></span> 💞 Your People</button>
			<button type="button" class="tab" data-tab="agreements"><span class="dot"></span> 📜 Agreements</button>
			<button type="button" class="tab" data-tab="rightnow"><span class="dot"></span> ⚡ Right Now</button>
			<button type="button" class="tab" data-tab="patterns"><span class="dot"></span> 🌀 Patterns</button>
		</div>

		<!-- ═══ YOU ═══ -->

		<div class="panel active" id="tab-you">

			```
			<div class="sec">
				<div class="sec-title">🌊 Nervous System State</div>
				<div class="sec-desc">Where are you right now? Hyperarousal = flooded, reactive. Hypoarousal = shut down, numb. The regulated middle is where real connection is possible.</div>
				<div class="sl-row">
					<div class="sl-hd"><span>Current state</span><span class="val" id="wotVal">5</span></div>
					<div class="sl-wrap">
						<div class="sl-min">⬆ Hyper</div>
						<input type="range" id="wotSlider" min="1" max="10" value="5" oninput="updateWot(this)">
						<div class="sl-max">Hypo ⬇</div>
					</div>
					<div class="wot-spec mt8">
						<div class="wot-cur" id="wotCur" style="left:50%"></div>
					</div>
				</div>
				<div class="insight">
					<div class="i-lbl">What this means</div><span id="wotTxt">You're in a regulated state — a good window for honest conversations and genuine connection.</span>
				</div>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">🪢 Attachment Tendencies</div>
				<div class="sec-desc">Most of us carry relational patterns shaped by early experiences. They're not fixed — naming them creates room to choose differently.</div>
				<div class="sl-row">
					<div class="sl-hd"><span>Anxious pull</span><span class="val" id="anxVal">3</span></div>
					<div class="sl-wrap">
						<div class="sl-min">Barely</div>
						<input type="range" id="anxSlider" min="1" max="10" value="3" oninput="sv('anxSlider','anxVal');attInsight()">
						<div class="sl-max">Strongly</div>
					</div>
				</div>
				<div class="sl-row">
					<div class="sl-hd"><span>Avoidant pull</span><span class="val" id="avoVal">3</span></div>
					<div class="sl-wrap">
						<div class="sl-min">Barely</div>
						<input type="range" id="avoSlider" min="1" max="10" value="3" oninput="sv('avoSlider','avoVal');attInsight()">
						<div class="sl-max">Strongly</div>
					</div>
				</div>
				<div class="sl-row">
					<div class="sl-hd"><span>Disorganized pull</span><span class="val" id="disVal">2</span></div>
					<div class="sl-wrap">
						<div class="sl-min">Barely</div>
						<input type="range" id="disSlider" min="1" max="10" value="2" oninput="sv('disSlider','disVal');attInsight()">
						<div class="sl-max">Strongly</div>
					</div>
				</div>
				<div class="insight">
					<div class="i-lbl">Pattern reflection</div><span id="attTxt">Adjust the sliders to see a reflection of your current attachment pattern.</span>
				</div>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">🌱 Core Needs Right Now</div>
				<div class="sec-desc">Select all that apply. Most relationship friction starts with an unmet need that hasn't been named yet.</div>
				<div class="pills" id="needPills">
					<span class="pill" data-v="connection">Connection</span>
					<span class="pill" data-v="reassurance">Reassurance</span>
					<span class="pill" data-v="space">Space</span>
					<span class="pill" data-v="clarity">Clarity</span>
					<span class="pill" data-v="support">Support</span>
					<span class="pill" data-v="fun">Fun / Levity</span>
					<span class="pill" data-v="rest">Rest</span>
					<span class="pill" data-v="validation">Validation</span>
					<span class="pill" data-v="autonomy">Autonomy</span>
					<span class="pill" data-v="intimacy">Intimacy</span>
					<span class="pill" data-v="predictability">Predictability</span>
					<span class="pill" data-v="novelty">Novelty</span>
					<span class="pill" data-v="safety">Safety</span>
					<span class="pill" data-v="seen">To be seen</span>
				</div>
				<div class="fld mt12">
					<label class="lbl">Anything else weighing on you today?</label>
					<textarea id="weighTxt" placeholder="Just a few words is enough — this is your space..." oninput="prog()"></textarea>
				</div>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">⚓ Self-Foundation Check</div>
				<div class="sec-desc">A grounded sense of self makes every relationship more sustainable. These aren't boxes to perform — they're practices that make real connection more possible.</div>
				<div class="checklist" id="sfList">
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I have regular time that is genuinely just mine</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I have a therapist, coach, or trusted support person</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I practice nervous system regulation (breath, movement, rest)</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I know my core values and try to act from them</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I can name my emotions with some accuracy</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I have meaningful friendships outside my romantic relationships</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>I pursue creative or physical outlets regularly</span>
					</div>
				</div>
			</div>
			```

		</div>

		<!-- ═══ PEOPLE ═══ -->

		<div class="panel" id="tab-people">
			<div class="sec">
				<div class="sec-title">💞 Connection Dimensions</div>
				<div class="sec-desc">Six dimensions of felt connection — rate each for every partner. The lowest score is where the most growth lives. These aren't grades; they're a map.</div>
			</div>
			<div id="pCon"></div>
			<button type="button" class="btn btn-out w100 mt8" onclick="addPartner()">＋ Add Partner</button>

			```
			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">🗺️ Relationship Architecture</div>
				<div class="sec-desc">Naming your structure removes unspoken assumptions. There's no wrong answer — just more or less clarity.</div>
				<div class="g2">
					<div class="fld">
						<label class="lbl">Your primary structure</label>
						<select id="structSel" onchange="prog()">
							<option value="">— Choose —</option>
							<option>Kitchen table poly</option>
							<option>Parallel poly</option>
							<option>Garden party poly</option>
							<option>Solo poly</option>
							<option>Relationship anarchy</option>
							<option>Hierarchical poly</option>
							<option>Non-hierarchical poly</option>
							<option>Still figuring it out</option>
						</select>
					</div>
					<div class="fld">
						<label class="lbl">Metamour contact style</label>
						<select id="metaSel" onchange="prog()">
							<option value="">— Choose —</option>
							<option>We're all friends</option>
							<option>Cordial but separate</option>
							<option>No direct contact preferred</option>
							<option>Varies by relationship</option>
						</select>
					</div>
				</div>
				<div class="fld">
					<label class="lbl">Polycule notes (ongoing dynamics, recent shifts)</label>
					<textarea id="polyNotes" placeholder="Optional — just for you..." oninput="prog()"></textarea>
				</div>
			</div>
			```

		</div>

		<!-- ═══ AGREEMENTS ═══ -->

		<div class="panel" id="tab-agreements">
			<div class="sec">
				<div class="sec-title">📜 Living Agreements</div>
				<div class="sec-desc">Agreements aren't rules — they're shared understandings that evolve as people do. Document yours so they can be revisited intentionally rather than assumed indefinitely.</div>
				<div class="g2">
					<div class="fld"><label class="lbl">Last reviewed</label><input type="date" id="lastRev" onchange="prog()"></div>
					<div class="fld"><label class="lbl">Next review scheduled</label><input type="date" id="nextRev" onchange="prog()"></div>
				</div>
			</div>

			```
			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">✅ Agreements Inventory</div>
				<div class="sec-desc">Check what you have in place. Unchecked items aren't failures — they're conversations waiting to happen.</div>
				<div class="sub" style="margin-top:4px">Sexual Health</div>
				<div class="checklist" style="margin-bottom:16px" id="agSex">
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>STI testing frequency agreed upon</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Barrier method agreements defined per partner</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Disclosure process for new partners is clear</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Sexual health updates shared promptly</span>
					</div>
				</div>
				<div class="sub">Time &amp; Scheduling</div>
				<div class="checklist" style="margin-bottom:16px" id="agTime">
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>How far in advance dates are typically planned</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Shared calendar or visibility method established</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Sleepover expectations are clear</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Holiday and big-event planning discussed</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Solo downtime is protected for everyone</span>
					</div>
				</div>
				<div class="sub">Communication</div>
				<div class="checklist" style="margin-bottom:16px" id="agComm">
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>How to flag a hard conversation is agreed on</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Check-in cadence established per relationship</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Social media and disclosure preferences set</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>What (if anything) crosses relationships is clear</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Conflict resolution approach discussed</span>
					</div>
				</div>
				<div class="sub">Finances &amp; Living</div>
				<div class="checklist" id="agFin">
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Financial entanglement (if any) is intentional</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>Cohabitation intentions or limits are clear</span>
					</div>
					<div class="ci" onclick="tick(this)">
						<div class="cb">✓</div><span>How new partners are integrated into shared spaces</span>
					</div>
				</div>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">✏️ Custom Agreements</div>
				<div class="sec-desc">Your relationships are unique. Add anything that matters to you here.</div>
				<div id="custAg"></div>
				<button type="button" class="btn btn-out mt8" onclick="addAg()">＋ Add Agreement</button>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title">🔄 What Needs Revisiting?</div>
				<div class="fld">
					<textarea id="revTxt" placeholder="Which agreements feel stale, unclear, or unresolved? List them — they don't have to be solved yet, just named." style="min-height:100px" oninput="prog()"></textarea>
				</div>
			</div>
			```

		</div>

		<!-- ═══ RIGHT NOW ═══ -->

		<div class="panel" id="tab-rightnow">
			<div class="sec">
				<div class="sec-title">⚡ What's Happening</div>
				<div class="sec-desc">Quick triage. You don't have to solve anything yet — just name it accurately and get a first step.</div>
			</div>

			```
			<div class="sec">
				<div class="sec-title" style="font-family:'Syne',sans-serif;font-size:14px;font-weight:600">I'm feeling...</div>
				<div class="f-grid">
					<button type="button" class="f-btn" data-f="flooded" onclick="feel(this)"><span class="emoji">🌊</span>Flooded</button>
					<button type="button" class="f-btn" data-f="anxious" onclick="feel(this)"><span class="emoji">😬</span>Anxious</button>
					<button type="button" class="f-btn" data-f="disconnected" onclick="feel(this)"><span class="emoji">🌫️</span>Disconnected</button>
					<button type="button" class="f-btn" data-f="hurt" onclick="feel(this)"><span class="emoji">💔</span>Hurt</button>
					<button type="button" class="f-btn" data-f="jealous" onclick="feel(this)"><span class="emoji">🟢</span>Jealous</button>
					<button type="button" class="f-btn" data-f="confused" onclick="feel(this)"><span class="emoji">🌀</span>Confused</button>
					<button type="button" class="f-btn" data-f="grateful" onclick="feel(this)"><span class="emoji">🧡</span>Grateful</button>
					<button type="button" class="f-btn" data-f="secure" onclick="feel(this)"><span class="emoji">⚓</span>Secure</button>
					<button type="button" class="f-btn" data-f="excited" onclick="feel(this)"><span class="emoji">✨</span>Excited</button>
					<button type="button" class="f-btn" data-f="overwhelmed" onclick="feel(this)"><span class="emoji">🔥</span>Overwhelmed</button>
					<button type="button" class="f-btn" data-f="lonely" onclick="feel(this)"><span class="emoji">🫙</span>Lonely</button>
					<button type="button" class="f-btn" data-f="numb" onclick="feel(this)"><span class="emoji">🪨</span>Shut Down</button>
				</div>
			</div>

			<div class="hr"></div>

			<div class="sec">
				<div class="sec-title" style="font-family:'Syne',sans-serif;font-size:14px;font-weight:600">The situation involves...</div>
				<div class="g2 mt8">
					<div class="fld">
						<label class="lbl">Category</label>
						<select id="sitSel" onchange="triage()">
							<option value="">— Choose —</option>
							<option value="conflict">Active conflict</option>
							<option value="jealousy">Jealousy / comparison</option>
							<option value="scheduling">Scheduling stress</option>
							<option value="comms">Communication breakdown</option>
							<option value="nre">NRE disruption</option>
							<option value="metamour">Metamour tension</option>
							<option value="unmet">Unmet need</option>
							<option value="breach">Agreement breach</option>
							<option value="positive">Positive check-in</option>
						</select>
					</div>
					<div class="fld">
						<label class="lbl">Which relationship(s)?</label>
						<input type="text" id="sitWho" placeholder="Name(s) or just 'all'" oninput="prog()">
					</div>
				</div>
				<div class="fld">
					<label class="lbl">Intensity right now (1 = mild · 10 = crisis)</label>
					<div class="sl-hd" style="margin-bottom:5px"><span></span><span class="val" id="intVal">5</span></div>
					<input type="range" id="intSlider" min="1" max="10" value="5" style="width:100%" oninput="sv('intSlider','intVal');triage()">
				</div>
				<div class="fld mt8">
					<label class="lbl">Describe briefly (for your clarity, not for solving)</label>
					<textarea id="sitDesc" placeholder="What happened? What's the stuck point? What do you actually want?" style="min-height:100px" oninput="prog()"></textarea>
				</div>
			</div>

			<div id="triageOut" class="hide">
				<div class="hr"></div>
				<div class="sec">
					<div class="sec-title">🧭 First Step</div>
					<div class="insight">
						<div class="i-lbl">Suggested approach</div><span id="triTxt"></span>
					</div>
					<div class="insight mt12" style="border-color:rgba(109,191,184,.25)">
						<div class="i-lbl" style="color:var(--accent3)">Something to sit with</div>
						<span id="triQ"></span>
					</div>
				</div>
			</div>
			```

		</div>

		<!-- ═══ PATTERNS ═══ -->

		<div class="panel" id="tab-patterns">
			<div class="sec">
				<div class="sec-title">🌀 Your Snapshot</div>
				<div class="sec-desc">A reflection of what you've filled in. Come back over time — patterns emerge with repetition.</div>
				<div class="g3 mt12">
					<div class="stat">
						<div class="stat-n" id="sP">—</div>
						<div class="stat-l">Partners tracked</div>
					</div>
					<div class="stat">
						<div class="stat-n" id="sA">—</div>
						<div class="stat-l">Agreements in place</div>
					</div>
					<div class="stat">
						<div class="stat-n" id="sN">—</div>
						<div class="stat-l">Needs named today</div>
					</div>
				</div>
			</div>
			<div class="hr"></div>
			<div class="sec">
				<div class="sec-title">📊 Attachment Balance</div>
				<div id="attBars"></div>
			</div>
			<div class="hr"></div>
			<div class="sec">
				<div class="sec-title">🪻 Wins &amp; Gratitude</div>
				<div class="sec-desc">Naming what's working isn't toxic positivity — it's information that helps you protect what matters.</div>
				<div class="fld">
					<label class="lbl">Recent wins in your relationships</label>
					<textarea id="winsTxt" placeholder="Even small ones count. 'We talked about the hard thing.' That counts." oninput="prog()"></textarea>
				</div>
				<div class="fld mt8">
					<label class="lbl">What I'm grateful for in each relationship</label>
					<textarea id="gratTxt" placeholder="Name names. Be specific." oninput="prog()"></textarea>
				</div>
			</div>
			<div class="hr"></div>
			<div class="sec">
				<div class="sec-title">📝 Pattern Notes</div>
				<div class="sec-desc">What loops do you keep ending up in? What are you just starting to notice?</div>
				<div class="fld">
					<textarea id="patNotes" placeholder="I notice that when _______, I tend to _______. I want to try _______." style="min-height:100px" oninput="prog()"></textarea>
				</div>
			</div>
			<div class="mt16">
				<button type="button" class="btn btn-pri" onclick="snap()">🔄 Refresh Snapshot</button>
			</div>
		</div>

	</div><!-- /wrap -->

	<div class="bar">
		<div class="bar-info">Last saved: <span id="savedAt">unsaved</span></div>
		<div class="bar-btns">
			<button type="button" class="btn btn-out" onclick="save()">💾 Save</button>
			<button type="button" class="btn btn-acc" onclick="exp()">📦 Export JSON</button>
		</div>
	</div>

	<div class="toast" id="toast"></div>

	<script>
		// ── STATE ────────────────────────────────────────────────────────
		let partners = [];
		let agreements = []; // custom agreements
		let feeling = null;
		let needs = new Set();
		let dark = true;

		// ── THEME ────────────────────────────────────────────────────────
		document.getElementById('themeBtn').addEventListener('click', () => {
			dark = !dark;
			document.documentElement.setAttribute('data-theme', dark ? 'dark' : 'light');
			document.getElementById('themeIco').textContent = dark ? '☀️' : '🌙';
			document.getElementById('themeTxt').textContent = dark ? 'Light' : 'Dark';
		});

		// ── TABS ─────────────────────────────────────────────────────────
		document.querySelectorAll('.tab').forEach(btn => {
			btn.addEventListener('click', () => {
				document.querySelectorAll('.tab').forEach(b => b.classList.remove('active'));
				document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
				btn.classList.add('active');
				document.getElementById('tab-' + btn.dataset.tab).classList.add('active');
				if (btn.dataset.tab === 'patterns') snap();
			});
		});

		// ── SLIDERS ──────────────────────────────────────────────────────
		function sv(sid, vid) {
			document.getElementById(vid).textContent = document.getElementById(sid).value;
			prog();
		}

		function updateWot(el) {
			const v = +el.value;
			document.getElementById('wotVal').textContent = v;
			document.getElementById('wotCur').style.left = (((v - 1) / 9) * 100) + '%';
			const T = {
				1: 'High activation — your threat response is fully engaged. Pause before any important conversations. Soothe your nervous system first: slow breath, cold water, movement.',
				2: 'Significantly activated. Reactivity is high and clarity is low. Movement, breath, or a short walk can help bring you back.',
				3: 'Leaning activated. You may be able to talk, but watch for reactivity. Try naming what\'s happening in your body before diving in.',
				4: 'Slightly above baseline — close to your window. One grounding exercise might bring you all the way in.',
				5: 'In your regulated zone. You\'re available for genuine connection — a good time for honest conversations.',
				6: 'Well regulated and present. Lean in — this is where real relating happens.',
				7: 'Calm and grounded. Use this state for repair, planning, and expressing appreciation.',
				8: 'Spacious and settled. Great for reflection, future-planning, and naming what you\'re grateful for.',
				9: 'Moving toward shutdown. Hypoarousal can look like calm but is actually withdrawal. Check in gently with yourself.',
				10: 'Shut down or dissociated. Your system is protecting you. Gentle movement, warmth, and sensory grounding can help. No major decisions right now.'
			};
			document.getElementById('wotTxt').textContent = T[v];
			prog();
		}

		function attInsight() {
			const a = +g('anxSlider').value,
				av = +g('avoSlider').value,
				d = +g('disSlider').value;
			let t;
			if (Math.max(a, av, d) <= 3) t = 'Your attachment system feels relatively settled right now. A good baseline to note — return here to compare during harder moments.';
			else if (a > av && a > d && a >= 6) t = 'Anxious activation is high. This pattern tends to scan for signs of rejection and reach for closeness as relief. What would reassurance feel like right now — and can you offer some of it to yourself first?';
			else if (av > a && av > d && av >= 6) t = 'Avoidant activation is high. This pattern protects itself through distance and self-sufficiency. Ask honestly: is withdrawal giving you real rest, or creating the disconnection you\'re afraid of?';
			else if (d > 5) t = 'Disorganized activation is present. This often involves a push-pull — wanting closeness and being frightened of it simultaneously. Slow everything down. Safety before vulnerability.';
			else if (a >= 5 && av >= 5) t = 'You\'re holding both anxious and avoidant pulls — common in complex relational histories. Notice which one takes over in the moment. Neither is wrong; both are information.';
			else t = 'A mixed picture. Attachment patterns are adaptive, not broken — they formed to protect you. Noticing them is the first step toward choosing differently.';
			document.getElementById('attTxt').textContent = t;
		}

		// ── NEEDS PILLS ──────────────────────────────────────────────────
		document.querySelectorAll('#needPills .pill').forEach(p => {
			p.addEventListener('click', () => {
				p.classList.toggle('on');
				p.classList.contains('on') ? needs.add(p.dataset.v) : needs.delete(p.dataset.v);
				prog();
			});
		});

		// ── CHECKLISTS ───────────────────────────────────────────────────
		// FIX: single handler, div elements only, no hidden checkbox
		function tick(el) {
			el.classList.toggle('on');
			prog();
		}

		// ── PARTNERS ─────────────────────────────────────────────────────
		const DIMS = [
			{ k: 'presence', l: 'Presence', d: 'Physically & emotionally available' },
			{ k: 'delight', l: 'Delight', d: 'Genuine enjoyment of who you are' },
			{ k: 'attunement', l: 'Attunement', d: 'Tuned into your inner world' },
			{ k: 'consistency', l: 'Consistency', d: 'Reliable rhythms & follow-through' },
			{ k: 'trust', l: 'Trust', d: 'Safe to depend on' },
			{ k: 'acknowledgment', l: 'Acknowledgment', d: 'Truly seen and known' }
		];
		const HINTS = {
			presence: 'Even short, fully undistracted time together shifts this more than long time spent half-present.',
			delight: 'People need to feel genuinely liked, not just loved. Express something specific you enjoy about exactly who they are.',
			attunement: 'Attunement means noticing and responding to the other person\'s inner world — not just their words. Ask more. Reflect back what you hear.',
			consistency: 'Reliability is built through small kept promises more than grand gestures. Find the smallest recurring act you can sustain.',
			trust: 'Trust erodes slowly and rebuilds slowly. Identify one small follow-through you can do this week.',
			acknowledgment: 'Being truly seen requires vulnerability from both sides. Share more of yourself, and ask to know more of them.'
		};

		function addPartner() {
			const id = Date.now();
			const scores = {};
			DIMS.forEach(d => scores[d.k] = 5);
			partners.push({ id, name: 'Partner', scores, style: '' });
			renderPartners();
			prog();
		}

		function removePartner(id) {
			partners = partners.filter(p => p.id !== id);
			renderPartners();
			prog();
		}

		function renderPartners() {
			const c = g('pCon');
			if (!partners.length) {
				c.innerHTML = '<p class="muted" style="padding:10px 0">No partners added yet. Add one to begin a Connection Dimensions check-in.</p>';
				return;
			}
			c.innerHTML = partners.map(pCard).join('');
		}

		function pCard(p) {
			const vals = Object.values(p.scores);
			const avg = vals.reduce((a, b) => a + b, 0) / vals.length;
			const low = DIMS.reduce((l, d) => p.scores[d.k] < p.scores[l.k] ? d : l, DIMS[0]);
			const cls = avg >= 7 ? 'b-hi' : avg >= 5 ? 'b-mi' : 'b-lo';
			const opts = ['', 'Secure', 'Anxious', 'Avoidant', 'Disorganized', 'Unknown / exploring']
				.map(s => `<option value="${s}"${p.style===s?' selected':''}>${s||'— Attachment style —'}</option>`).join('');

			return `<div class="pc" id="pc${p.id}">
    <div class="pc-hd">
      <input class="pname" type="text" value="${esc(p.name)}"
        oninput="pName(${p.id},this.value)" onchange="pName(${p.id},this.value)">
      <div style="display:flex;align-items:center;gap:8px">
        <div class="badge ${cls}" id="bdg${p.id}">${avg.toFixed(1)}</div>
        <button type="button" class="rm-btn" onclick="removePartner(${p.id})">✕</button>
      </div>
    </div>
    ${DIMS.map(d=>`
      <div class="sl-row" style="margin-bottom:8px">
        <div class="sl-hd" style="margin-bottom:3px">
          <span style="font-size:12px;color:var(--text)"><strong>${d.l}</strong> <span style="color:var(--muted);font-weight:400">· ${d.d}</span></span>
          <span class="val" id="dv${p.id}${d.k}">${p.scores[d.k]}</span>
        </div>
        <input type="range" min="1" max="10" value="${p.scores[d.k]}" style="width:100%"
          oninput="dimChg(${p.id},'${d.k}',this.value)">
      </div>`).join('')}
    <div id="ins${p.id}" class="insight" style="margin-top:10px">${insHTML(p,avg,low)}</div>
    <div class="fld mt8">
      <label class="lbl" id="slbl${p.id}">Attachment style for ${esc(p.name)}</label>
      <select onchange="pField(${p.id},'style',this.value)">${opts}</select>
    </div>
  </div>`;
		}

		function insHTML(p, avg, low) {
			if (avg >= 8) return `<div class="i-lbl" style="color:var(--success)">Strong connection</div>All six dimensions are scoring well. Keep tending what you've built.`;
			return `<div class="i-lbl">Focus area</div><strong>${low.l}</strong> is the lowest dimension right now. ${HINTS[low.k]}`;
		}

		// FIX: surgical DOM update — never re-render the whole card on slider move
		function dimChg(id, key, raw) {
			const val = +raw;
			const p = partners.find(p => p.id === id);
			if (!p) return;
			p.scores[key] = val;

			const vEl = g(`dv${id}${key}`);
			if (vEl) vEl.textContent = val;

			const vals = Object.values(p.scores);
			const avg = vals.reduce((a, b) => a + b, 0) / vals.length;
			const low = DIMS.reduce((l, d) => p.scores[d.k] < p.scores[l.k] ? d : l, DIMS[0]);

			const bdg = g(`bdg${id}`);
			if (bdg) { bdg.textContent = avg.toFixed(1);
				bdg.className = 'badge ' + (avg >= 7 ? 'b-hi' : avg >= 5 ? 'b-mi' : 'b-lo'); }

			const ins = g(`ins${id}`);
			if (ins) ins.innerHTML = insHTML(p, avg, low);

			prog();
		}

		function pName(id, name) {
			const p = partners.find(p => p.id === id);
			if (p) p.name = name;
			const lbl = g(`slbl${id}`);
			if (lbl) lbl.textContent = `Attachment style for ${name}`;
			prog();
		}

		function pField(id, f, v) {
			const p = partners.find(p => p.id === id);
			if (p) p[f] = v;
			prog();
		}

		// ── CUSTOM AGREEMENTS ─────────────────────────────────────────────
		function addAg() {
			agreements.push({ id: Date.now(), type: '', text: '' });
			renderAg();
		}

		function removeAg(id) {
			agreements = agreements.filter(a => a.id !== id);
			renderAg();
			prog();
		}

		function renderAg() {
			const c = g('custAg');
			if (!agreements.length) { c.innerHTML = ''; return; }
			// FIX: build DOM manually so textarea.value is set via property (not innerHTML)
			// This avoids HTML entity corruption on re-render
			c.innerHTML = agreements.map(a => `
    <div class="card" id="ag${a.id}">
      <div class="card-hd">
        <select style="width:auto;min-width:160px;font-size:12px;padding:6px 32px 6px 10px"
          onchange="updAg(${a.id},'type',this.value)">
          <option value="">— Type —</option>
          ${['Communication','Sexual health','Time / Scheduling','Metamour','Finances','Other']
            .map(t=>`<option${a.type===t?' selected':''}>${t}</option>`).join('')}
        </select>
        <button type="button" class="rm-btn" onclick="removeAg(${a.id})">✕</button>
      </div>
      <textarea id="agTa${a.id}" style="min-height:70px"
        placeholder="Describe the agreement clearly, in plain language..."
        oninput="updAg(${a.id},'text',this.value)"></textarea>

`
				`` <
				/div>`).join('');
				``
				`

// FIX: set textarea values via .value property — prevents HTML entity display bugs
agreements.forEach(a => {
const ta = g(`
				agTa$ { a.id }
				`);
if (ta) ta.value = a.text;
});
}

function updAg(id, f, v) {
const a = agreements.find(a=>a.id===id);
if (a) a[f] = v;
prog();
}

// ── TRIAGE ───────────────────────────────────────────────────────
function feel(btn) {
document.querySelectorAll(’.f-btn’).forEach(b=>b.classList.remove(‘on’));
btn.classList.add(‘on’);
feeling = btn.dataset.f;
triage();
prog();
}

function triage() {
const sit = g(‘sitSel’).value;
const lv  = +g(‘intSlider’).value;
if (!feeling && !sit) return;
g(‘triageOut’).classList.remove(‘hide’);

const steps = {
flooded:‘Your nervous system is overwhelmed. Before anything else: stop. Five slow breaths, cold water on your face, a short walk. No important conversations until you're back in your regulated window.’,
anxious:‘Anxiety often signals an unmet need or a familiar fear getting louder. Try naming the specific worry: 'I'm afraid that ___.'  Then ask — is this new information, or an old story replaying?’,
disconnected:‘Check whether this is restorative withdrawal or protective distancing. If connection feels unsafe right now, a low-stakes reach — a short message, a simple check-in — can gently re-open the door.’,
hurt:‘Hurt needs to be witnessed before it can be worked through. Write down or tell someone exactly what happened and what you needed. You don't have to solve it yet — just name it clearly.’,
jealous:‘Jealousy is almost always a fear wearing a different mask. Ask: what do I actually believe this moment means about me, my worth, or my place in this relationship?’,
confused:‘Confusion means something needs more clarity. Write down what you think is happening, what you want, and what you'd need to feel okay. Bring this to a conversation — not a reaction.’,
grateful:‘Let yourself feel this fully — then express it. Telling someone specifically what you appreciate about them is one of the most reliable ways to strengthen connection.’,
secure:‘Security is something to notice and actively tend. What's creating it? This is a great moment for a proactive conversation rather than a reactive one.’,
excited:‘Excitement is enlivening — and can also dysregulate. Enjoy it. And: try not to make major structural relationship decisions from a state of peak excitement.’,
overwhelmed:‘When everything feels like too much, simplify ruthlessly. What is the one most important thing right now? Just that. Everything else can wait.’,
lonely:‘Loneliness in a multi-partner structure can be especially confusing. Quantity doesn't equal felt connection. Get specific: what kind of presence are you missing, and with whom?’,
numb:‘Shutdown is a protective response, not a character flaw. Your system is doing its job. Gentle re-engagement: warmth, slow movement, something sensory. Let yourself come back at your own pace.’
};
const refs = {
conflict:‘Conflict between people with different inner worlds is expected — not a sign of failure. The goal isn't winning; it's understanding what's happening for both people well enough to find a way through.’,
jealousy:‘Jealousy usually points to a fear about belonging or worth. Getting curious about the fear underneath tends to be more useful than trying to manage the jealousy directly.’,
scheduling:‘Time scarcity is one of the most common stressors in multi-partner structures. Try naming not just what time you need, but what quality of presence and connection you're actually after.’,
comms:‘When communication breaks down, more words rarely help. Slowing down and getting genuinely curious about the other person's experience — before making your case — tends to open more doors.’,
nre:‘New Relationship Energy affects everyone involved, including partners watching from the outside. Being transparent about the experience — without over-sharing the details — is a skill worth practicing.’,
metamour:‘Metamour tension often contains real feelings worth taking seriously. Using a shared partner as a go-between usually makes things worse. Direct, careful communication tends to be better.’,
unmet:‘Unmet needs don't disappear — they go underground and resurface as resentment or acting out. Name the need in a full sentence: 'I need ___.'  Not a hint. The actual thing.’,
breach:‘An agreement breach is a relationship event, not a verdict. Useful questions: What happened for each person? What was the impact? What does repair look like? Start there.’,
positive:‘When things are going well, pay attention to what's working. Intentionally protecting the conditions for a healthy relationship matters just as much as repairing when things go wrong.’
};
const note = lv>=8?’ Given how intense this feels, stabilizing comes before resolving — don't push for closure right now.’
:lv>=5?’ This is worth addressing soon, while there's still enough space to be thoughtful.’
:’ The lower intensity gives you room to be deliberate. Use it.’;

g(‘triTxt’).textContent = (steps[feeling]||‘Name what you're feeling as precisely as possible. The more specific the label, the clearer the message underneath it.’) + note;
g(‘triQ’).textContent   = refs[sit]||‘Whatever is happening: understanding comes before fixing. Repair comes before growth. Take it in that order.’;
}

// ── PATTERNS SNAPSHOT ────────────────────────────────────────────
function snap() {
g(‘sP’).textContent = partners.length||‘0’;
g(‘sA’).textContent = document.querySelectorAll(’.ci.on’).length;
g(‘sN’).textContent = needs.size||‘0’;
const bars=[
{l:‘Anxious pull’,    id:‘anxSlider’,c:’#e07070’},
{l:‘Avoidant pull’,   id:‘avoSlider’,c:’#9b7fd4’},
{l:‘Disorganized pull’,id:‘disSlider’,c:’#c9956e’}
];
g(‘attBars’).innerHTML = bars.map(b=>{
const v=+g(b.id).value;
return ` < div class = "pbar" > < div class = "pbar-lbl" > < span > $ { b.l } < /span><span style="color:${b.c};font-weight:700">${v}/
				10 < /span></div > < div class = "pbar-bg" > < div class = "pbar-fill"
				style = "width:${v*10}%;background:${b.c}" > < /div></div > < /div>`;
			}).join(’’);
		}

		// ── PROGRESS ─────────────────────────────────────────────────────
		function prog() {
			const ids = [‘weighTxt’, ‘polyNotes’, ‘lastRev’, ‘nextRev’, ‘revTxt’, ‘sitWho’, ‘sitDesc’, ‘winsTxt’, ‘gratTxt’, ‘patNotes’];
			const total = ids.length + 5;
			let filled = 0;
			ids.forEach(id => { const e = g(id); if (e && e.value.trim()) filled++; });
			if (needs.size > 0) filled++;
			if (feeling) filled++;
			if (partners.length > 0) filled++;
			if (g(‘structSel’).value) filled++;
			if (+g(‘anxSlider’).value !== 3 || +g(‘avoSlider’).value !== 3) filled++;
			const pct = Math.min(100, Math.round((filled / total) * 100));
			g(‘pFill’).style.width = pct + ’ % ’;
			g(‘pPct’).textContent = pct + ’ % ’;
			const marks = {
				you: needs.size > 0 || !!(g(‘weighTxt’).value.trim()),
				people: partners.length > 0,
				agreements: document.querySelectorAll(’.ci.on’).length > 0,
				rightnow: !!(feeling || g(‘sitDesc’).value.trim()),
				patterns: !!(g(‘winsTxt’).value.trim())
			};
			Object.entries(marks).forEach(([t, has]) => document.querySelector(`[data-tab="${t}"]`)?.classList.toggle(‘has - data’, has));
		}

		// ── SAVE / LOAD ───────────────────────────────────────────────────
		function gather() {
			// Save checklist state by ID so position-changes don’t corrupt data
			const checks = {};
			document.querySelectorAll(’.ci’).forEach(el => {
				const key = el.querySelector(‘span’)?.textContent?.trim();
				if (key) checks[key] = el.classList.contains(‘on’);
			});
			return {
				v: ‘2’,
				at: new Date().toISOString(),
				sliders: { wot: g(‘wotSlider’).value, anx: g(‘anxSlider’).value, avo: g(‘avoSlider’).value, dis: g(‘disSlider’).value, int: g(‘intSlider’).value },
				needs: Array.from(needs),
				feeling,
				texts: { weigh: g(‘weighTxt’).value, polyN: g(‘polyNotes’).value, rev: g(‘revTxt’).value, sitWho: g(‘sitWho’).value, sitDesc: g(‘sitDesc’).value, wins: g(‘winsTxt’).value, grat: g(‘gratTxt’).value, pat: g(‘patNotes’).value },
				dates: { last: g(‘lastRev’).value, next: g(‘nextRev’).value },
				selects: { struct: g(‘structSel’).value, meta: g(‘metaSel’).value, sit: g(‘sitSel’).value },
				checks,
				partners,
				agreements
			};
		}

		function save() {
			try {
				localStorage.setItem(‘pc2’, JSON.stringify(gather()));
				g(‘savedAt’).textContent = new Date().toLocaleTimeString([], { hour: ‘2 - digit’, minute: ‘2 - digit’ });
				toast(‘💾Saved!’);
			} catch (e) { toast(‘⚠️Could not save.’); }
		}

		function load() {
			let raw;
			try { raw = localStorage.getItem(‘pc2’); } catch (e) { return; }
			if (!raw) return;
			try {
				const d = JSON.parse(raw);
				// sliders
				if (d.sliders) {
					setSlider(‘wotSlider’, ‘wotVal’, d.sliders.wot);
					updateWot(g(‘wotSlider’));
					setSlider(‘anxSlider’, ‘anxVal’, d.sliders.anx);
					setSlider(‘avoSlider’, ‘avoVal’, d.sliders.avo);
					setSlider(‘disSlider’, ‘disVal’, d.sliders.dis);
					setSlider(‘intSlider’, ‘intVal’, d.sliders.int);
					attInsight();
				}
				// needs pills
				if (d.needs) d.needs.forEach(n => { needs.add(n);
					document.querySelector(`[data-v="${n}"]`)?.classList.add(‘on’); });
				// feeling
				if (d.feeling) { feeling = d.feeling;
					document.querySelector(`[data-f="${d.feeling}"]`)?.classList.add(‘on’); }
				// text fields
				if (d.texts) {
					setVal(‘weighTxt’, d.texts.weigh);
					setVal(‘polyNotes’, d.texts.polyN);
					setVal(‘revTxt’, d.texts.rev);
					setVal(‘sitWho’, d.texts.sitWho);
					setVal(‘sitDesc’, d.texts.sitDesc);
					setVal(‘winsTxt’, d.texts.wins);
					setVal(‘gratTxt’, d.texts.grat);
					setVal(‘patNotes’, d.texts.pat);
				}
				// dates & selects
				if (d.dates) { setVal(‘lastRev’, d.dates.last);
					setVal(‘nextRev’, d.dates.next); }
				if (d.selects) { setVal(‘structSel’, d.selects.struct);
					setVal(‘metaSel’, d.selects.meta);
					setVal(‘sitSel’, d.selects.sit); }
				// partners & agreements
				if (d.partners) { partners = d.partners;
					renderPartners(); }
				if (d.agreements) { agreements = d.agreements;
					renderAg(); }
				// FIX: restore checks by label text (not by position) — robust to layout changes
				if (d.checks) {
					document.querySelectorAll(’.ci’).forEach(el => {
						const key = el.querySelector(‘span’)?.textContent?.trim();
						if (key && d.checks[key] === true) el.classList.add(‘on’);
					});
				}
				// restore triage if applicable
				if (feeling || g(‘sitSel’).value) triage();
				prog();
			} catch (e) { console.warn(‘Restore failed: ’, e); }
		}

		// ── EXPORT ───────────────────────────────────────────────────────
		function exp() {
			const blob = new Blob([JSON.stringify(gather(), null, 2)], { type: ‘application / json’ });
			const url = URL.createObjectURL(blob);
			const a = document.createElement(‘a’);
			a.href = url;
			a.download = `poly-compass-${new Date().toISOString().split('T')[0]}.json`;
			a.style.display = ‘none’;
			document.body.appendChild(a);
			a.click();
			setTimeout(() => { URL.revokeObjectURL(url);
				document.body.removeChild(a); }, 1000);
			toast(‘📦Exported!Check your downloads folder.’);
		}

		// ── TOAST ────────────────────────────────────────────────────────
		function toast(msg) {
			const t = g(‘toast’);
			t.textContent = msg;
			t.classList.add(‘on’);
			setTimeout(() => t.classList.remove(‘on’), 2800);
		}

		// ── HELPERS ──────────────────────────────────────────────────────
		const g = id => document.getElementById(id);

		function esc(s) {
			return String(s).replace(/&/g, ’ & ’).replace(/</g, ’ < ’).replace(/>/g, ’ > ’).replace(/”/g, ’"’);}function setSlider(sid, vid, v) { if (!v) return; const e = g(sid); if (e) { e.value = v;
							g(vid).textContent = v; } }

					function setVal(id, v) { if (v === undefined || v === null || v === ’’) return; const e = g(id); if (e) e.value = v; }

					// ── INIT ─────────────────────────────────────────────────────────
					document.addEventListener(‘DOMContentLoaded’, () => {
						renderPartners();
						renderAg();
						updateWot(g(‘wotSlider’));
						attInsight();
						load();
						prog();
					});
	</script>

</body>

</html>
