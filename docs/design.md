# Design System: coachciso.com

A premium, ultra-minimalist design system tailored for elite CISO coaching and advisory services. Heavily inspired by the Apple aesthetic: clean typography, deliberate negative space, human-centric copy, and an uncompromising focus on clarity.

---

## 1. Design Philosophy

* **Less, But Better:** Remove every element that doesn't serve a direct purpose. No unnecessary borders, no chaotic gradients, no generic stock photos of servers or locks.
* **The Premium Pivot:** Cybersecurity branding is usually dark, aggressive, and fear-based (neon greens, deep blues, shields). *Coach CISO* is a leadership brand. It should feel calm, sophisticated, and authoritative—closer to a premium editorial magazine than a software dashboard.
* **Asymmetry & Breathing Room:** Use generous padding ($120\text{px}+$ on desktop sections) to give ideas room to breathe.

---

## 2. Colour Palette

A restricted, high-contrast monochrome palette accented by a singular, deep luxury tone.

| Tone | Hex | Usage |
| :--- | :--- | :--- |
| **Pure White** | `#FFFFFF` | Primary background, clean canvas |
| **Off-White** | `#F5F5F7` | Subtle section backgrounds, card containers (Apple Canvas) |
| **Ink Black** | `#1D1D1F` | Primary text, headings, maximum legibility |
| **Slate Gray** | `#86868B` | Secondary text, captions, deactivated states |
| **Subtle Emerald** | `#064E3B` | *Accents only.* Hyper-muted security nod used for primary CTAs or micro-interactions. |

---

## 3. Typography

The typeface choices mimic system fonts that feel invisible yet incredibly premium.

* **Primary Typeface:** `Inter` or `SF Pro Display` (Fallback: `sans-serif`)
* **Scale & Weights:**
    * **Hero Headers:** $56\text{px}$ to $72\text{px}$ | Bold (`700`) or Semibold (`600`) | Tracking: `-0.02em`
    * **Subheaders:** $24\text{px}$ to $28\text{px}$ | Regular (`400`) | Tracking: normal
    * **Body Copy:** $17\text{px}$ | Regular (`400`) | Line Height: `1.5` | Color: `#1D1D1F`
    * **UI/Labels:** $12\text{px}$ to $14\text{px}$ | Uppercase | Bold (`700`) | Tracking: `0.1em`

---

## 4. UI Components & Elements

### Layout & Structure
* **Grid:** Standard 12-column grid for desktop, but content should rarely span the full width. Keep text columns restricted to a max-width of $680\text{px}$ for optimal reading comfort.
* **Corners:** Micro-rem (e.g., `border-radius: 12px` or `16px`) on cards and containers. Avoid sharp corners; use the "squircle" feel.

### Buttons (Call to Action)
* **Primary Button:** Solid `#1D1D1F` with `#FFFFFF` text. No border, slight corner radius ($8\text{px}$). On hover, transitions smoothly to an opacity of `0.85`.
* **Secondary Button:** Inline text link with a trailing micro-arrow (`→`). Color: `#1D1D1F`. On hover, the arrow shifts slightly to the right (`transform: translateX(4px)`).

### Imagery & Media
* **Macro Photography:** If images are used, they should be high-end, black-and-white, or deeply desaturated portraits of leadership contexts (e.g., a quiet boardroom, a fountain pen, architectural lines).
* **No Clichés:** Zero images of glowing padlocks, binary code overlays, or people wearing hoodies.

---