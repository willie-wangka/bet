# Bet the Hackers — Design Documentation

## Overview

Bet the Hackers is a hackathon judging app that lets users swipe on teams (Tinder-style), bet credits on who they think will win, and see AI-generated scores for each project. The design is mobile-first with a warm, approachable aesthetic.

---

## App Flow

```
Landing Page → Swipe Teams → Pick Winner (all credits) → Leaderboard → Final Results
```

1. **Landing**: Header with branding, "Create Account" and "Dashboard" buttons, Reset option
2. **Swiping**: Users see one team card at a time. Swipe right (heart) to advance, left (X) to eliminate
3. **Pick Winner**: After swiping all teams, surviving teams are listed. User taps one — all 1,000 credits go on that team
4. **Leaderboard**: Shows "Voting Complete" with bet stats and community leaderboard
5. **Final Results**: Reveals the winning team with a celebration card and payout

---

## Layout

### Desktop (3-column grid)
- **Left Panel** (190–250px): Round status, teams left, model favorite prediction
- **Center** (320–520px): Team card deck
- **Right Panel** (210–270px): Next round info, portfolio summary, bets by team

### Mobile
- Single column, cards and panels stack vertically

```
Max content width: 1180px
Grid: minmax(190px, 250px) | minmax(320px, 520px) | minmax(210px, 270px)
Gap: 22px
Shell padding: 28px
```

---

## Color Palette

| Token        | Value                    | Usage                         |
|-------------|--------------------------|-------------------------------|
| `--ink`     | `#171717`                | Primary text                  |
| `--muted`   | `#67615d`                | Secondary text, labels        |
| `--panel`   | `#fffdf8`                | Card/panel backgrounds        |
| `--line`    | `rgba(25, 25, 25, 0.12)` | Borders, dividers             |
| `--shadow`  | `0 24px 80px rgba(20, 20, 20, 0.16)` | Card elevation    |
| Accent red  | `#b42335` / `#ff4458`    | Eyebrow text, trending badges |
| Card border | `#e8915a`                | Warm orange card frame        |
| Success     | `#12b886`                | Payout amounts, win badges    |

### Background
```css
body {
  background:
    radial-gradient(circle at top left, rgba(255, 68, 88, 0.17), transparent 30rem),
    linear-gradient(135deg, #f7f5f2 0%, #edf7f6 48%, #fff5f1 100%);
}
```
A warm, slightly pinkish gradient with a subtle radial glow in the top-left corner.

---

## Typography

- **Font Family**: Inter, ui-sans-serif, system-ui
- **H1**: `clamp(2rem, 6vw, 4.8rem)`, weight 900, line-height 0.95
- **Card Team Name**: 1.3rem, weight 800, centered
- **Card Tagline**: 0.88rem, weight 500, color `--muted`
- **Eyebrow Labels**: 0.76rem, weight 800, uppercase, color `#b42335`
- **Metric Values**: 1.8rem, bold

---

## Card Design

### Deck Container
- Aspect ratio: 0.68 (portrait)
- Max width: 500px, min height: 650px
- **Gradient border frame**: `linear-gradient(to bottom, #ffffff 0%, #f0b07a 40%, #e8915a 70%, #e8915a 100%)`
- Border radius: 14px
- Padding: 10px (creates the border frame effect)
- Overflow: hidden

### Team Card (Compact — Round 1)
- Background: `--panel` (#fffdf8, warm off-white)
- Border radius: 8px
- Inset: 12px from deck edges
- No border (border: none)
- Structure (top to bottom):
  1. **Video/Image** — `object-fit: cover`, fills top ~55% of card
  2. **Winnability Badge** — top-right overlay, dark frosted glass (`rgba(0,0,0,0.65)` + `backdrop-filter: blur(8px)`), shows average score percentage
  3. **Team Name** — 1.3rem bold, centered
  4. **Tagline** — one-line description, muted color
  5. **Ring Meters** — 3 circular score indicators (Competitiveness, Alignment, Marketability), scaled to 0.72
  6. **View Details** — pill button to expand card
  7. **Action Buttons** — X (pass) and Heart (advance), 44px round buttons

### Team Card (Expanded — View Details)
- Full scrollable card body with:
  - Back button (X icon + "Back")
  - Team identity (name, tagline, judge fit badge)
  - Project description (Brain icon)
  - Skill tags row
  - Social proof (bettors count, popularity bar, trending badge)
  - Likelihood vs field stats
  - Nia AI insight (sparkle icon, purple-tinted)
  - Team members list (name, role, LinkedIn links)
  - X and Heart action buttons at bottom

### Behind Card
- Opacity: 0.62
- Scale: 0.94, translateY: 18px
- Creates depth/stack effect

### Swipe Animations
- Left swipe: `translateX(-120%) rotate(-18deg)`, opacity 0
- Right swipe: `translateX(120%) rotate(18deg)`, opacity 0
- Transition: 250ms ease

---

## Ring Meters

Circular SVG score indicators:
- Size: 80×80px (scaled to 0.72 in card context)
- Track: `rgba(0,0,0,0.06)` stroke
- Fill: gradient stroke (red `#ef4444` → amber `#f59e0b` → green `#22c55e`) via SVG linearGradient
- Stroke width: 4
- Center text: score percentage, 1.05rem bold
- Label below: 0.58rem, uppercase, letter-spacing 0.06em
- Values: 0–100 scale

---

## Winnability Badge

- Position: absolute, top-right of card image
- Background: `rgba(0, 0, 0, 0.65)` with `backdrop-filter: blur(8px)`
- Border: `1.5px solid rgba(255, 255, 255, 0.2)`
- Border radius: 12px
- Score: 1.15rem, weight 800, white
- Label: "Win", 0.6rem, uppercase

---

## Panels (Side Cards)

- Background: `rgba(255, 253, 248, 0.84)` (semi-transparent warm white)
- Border: `1px solid var(--line)`
- Border radius: 8px
- Box shadow: `0 10px 36px rgba(20, 20, 20, 0.08)`
- Padding: 18px

---

## Buttons

### Primary Action (Bankroll style)
- Background: `#10231f` (dark teal)
- Color: `#fbfaf6`
- Border radius: 999px (pill)
- Padding: 12px 16px
- Hover: `#203632`

### Round Action Buttons (X / Heart)
- Size: 44×44px
- Border radius: 50%
- Pass (X): border `1px solid rgba(255,68,88,0.3)`, hover bg `rgba(255,68,88,0.1)`
- Like (Heart): border `1px solid rgba(255,68,88,0.3)`, hover bg `rgba(255,68,88,0.1)`

### View Details Button
- Background: none
- Border: `1px solid var(--muted)`
- Border radius: 20px (pill)
- Font: 0.78rem, weight 600
- Hover: light gray bg, darker border

---

## Bet Screen

- Full-screen modal with dark backdrop (`rgba(0,0,0,0.6)`)
- Card: max-width 440px, centered, white background, rounded 16px
- Header: Crown icon, "Pick your winner" title
- Team list: clickable buttons with team color dot, name, win probability
- One tap = all credits on that team (no slider)
- Skip betting option at bottom

---

## Winner Card

- Full-width celebration card
- Trophy icon with gold color (`#facc15`)
- Team name, tagline
- Confetti effect / visual celebration
- Portfolio summary: credits remaining, total spent, payout

---

## Dashboard (`/dashboard`)

- Separate page, linked from header
- **Winner section**: winning team display with ring meters
- **My Bets section**: user's bet history, credits remaining, total wagered, payout
- Winner bets highlighted with gold badge
- Empty states with descriptive text and links back to main page

---

## AI Agent Scores

3 scoring agents evaluate each team:
- **Competitiveness** — criterion 1 (mapped per track)
- **Alignment** — criterion 2 (mapped per track)
- **Marketability** — criterion 3 (mapped per track)

Agents use track-specific rubrics (4 tracks with different criteria and weights). Scores are 0–100 and displayed in the ring meters. Loading state shows "Agents analyzing..." with a pulse animation.

---

## Responsive Breakpoints

- **≤ 900px**: Single column layout, panels stack below deck
- **≥ 900px**: 3-column grid layout
- **≥ 1200px**: Larger deck min-height (720px)

---

## Key CSS Variables

```css
:root {
  color-scheme: light;
  --ink: #171717;
  --panel: #fffdf8;
  --muted: #67615d;
  --line: rgba(25, 25, 25, 0.12);
  --shadow: 0 24px 80px rgba(20, 20, 20, 0.16);
}
```

---

## Iconography

All icons from **Lucide React** library. Key icons used:
- `Heart` — advance/like team
- `X` — pass/eliminate team
- `Crown` — bet screen header, winner
- `Trophy` — winner card
- `Star` — favorites
- `Users` — team members, social proof
- `Brain` — project description
- `Sparkles` — Nia AI insights
- `TrendingUp` — popularity badges
- `Award` — leaderboard
- `Coins` — credits/betting
- `BadgeCheck` — judge fit indicator
