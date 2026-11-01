# @era-lang-official/ui

Official Declarative Reactive Widget Framework and Tailwind CSS Styling Engine for the **Era Programming Language**.

- **Pure Era Native Architecture**: Zero lines of C or C++. Compiles directly to lightweight native machine code via LLVM.
- **Flutter + Tailwind Fusion**: Declarative widget tree hierarchy styled with ergonomic, single-string utility classes.
- **60 FPS Hardware-Accelerated Rendering**: Double-buffered render loop with anti-aliased geometry, multi-layer drop shadows, and linear gradients.
- **Sub-100ms Hot Reload**: In-memory DSL template reloader (`tw_eval_dsl`) and automated dev watcher tool (`scripts/dev_watch.ps1`).

---

## 1. Quick Start

```era
import "github.com/era-lang-official/draw";
import "github.com/era-lang-official/layout";
import "github.com/era-lang-official/platform";
import "github.com/era-lang-official/ui";

fn build_user_card() -> ElementNode {
    return tw_card("card_1", "w-[380] p-4 bg-slate-900 border border-white/10 shadow-lg",
        tw_row_2("row_1", "gap-3 items-center",
            tw_avatar("av_1", "E", "w-10 h-10 bg-indigo-600"),
            tw_col_2("col_1", "gap-1",
                tw_row_2("hdr", "gap-2 items-center",
                    tw_text("name", "Era Developer", "text-sm font-bold text-white"),
                    tw_badge("badge", "PRO", "bg-emerald-500/20 text-emerald-400 border border-emerald-500/30")
                ),
                tw_text("status", "Online · 60 FPS Native", "text-xs text-slate-400")
            )
        )
    );
}
```

---

## 2. Supported Tailwind CSS Utilities

### Colors & Palettes (50 to 950)
- **Standard Scales**: `slate`, `zinc`, `indigo`, `emerald`, `green`, `rose`, `red`, `amber`, `orange`, `sky`, `cyan`, `blue`, `violet`, `purple`, `fuchsia`, `pink`.
- **Universal Opacity Modifiers**: Append `/<percent>` to any color (e.g. `bg-indigo-600/50`, `white/10`, `black/80`, `emerald-500/20`).
- **Arbitrary Hex Values**: `bg-[#0D111A]`, `text-[#CBD5E1]`, `border-[#6366F1]`.

### Dimensions & Sizing
- **Width**: `w-full` (100%), `w-1/2` (50%), `w-1/3` (33%), `w-2/3` (66%), `w-1/4` (25%), `w-3/4` (75%), `w-auto`, arbitrary `w-[320]`.
- **Height**: `h-full`, `h-1/2`, `h-10`, `h-12`, arbitrary `h-[60]`.

### Spacing (Padding, Margin, Gaps)
- **Padding**: `p-1` through `p-12`, `px-`, `py-`, `pt-`, `pb-`, `pl-`, `pr-` (4px multiples).
- **Margin**: `m-1` through `m-12`, `mx-`, `my-`, `mt-`, `mb-`, `ml-`, `mr-`.
- **Gap**: `gap-1` through `gap-12`.

### Responsive Breakpoints
Tokens prefixable with responsive window widths:
- `sm:` (≥ 640px)
- `md:` (≥ 768px)
- `lg:` (≥ 1024px)
- `xl:` (≥ 1280px)

*Example:* `"w-full md:w-[360] p-2 md:p-6 lg:p-10"`

### Typography
- **Size**: `text-xs` (10px), `text-sm` (12px), `text-base` (14px), `text-lg` (16px), `text-xl` (18px), `text-2xl` (24px), `text-3xl` (30px), `text-4xl` (36px).
- **Weight**: `font-bold`, `font-semibold`, `font-medium`, `font-normal`.
- **Alignment**: `text-center`.

### Visual Effects
- **Borders**: `border`, `border-0`, `border-2`, `border-4`, `border-white/10`, `border-indigo-500`.
- **Border Radius**: `rounded-none`, `rounded-sm` (4px), `rounded-md` (8px), `rounded-lg` (10px), `rounded-xl` (14px), `rounded-2xl` (18px), `rounded-3xl` (24px), `rounded-full` (circle/pill).
- **Drop Shadows**: `shadow-none`, `shadow-sm`, `shadow-md`, `shadow-lg`, `shadow-xl`.

---

## 3. Advanced Features

### A. State Management & Signals (`ReactiveStore`)
Fine-grained reactive signals prevent full-tree re-evaluation:
```era
let mut store = ReactiveStore::new();
store.set_str("channel_name".to_string(), "#general".to_string());
store.set_bool("is_connected".to_string(), true);

// Automatically updates when signal value changes
let title = tw_reactive_text("title", store.clone(), "channel_name", "text-lg font-bold text-white");
let panel = tw_reactive_box("box", store.clone(), "is_connected", "p-4 bg-slate-900", title);
```

### B. CSS-like Transitions & Micro-Interactions
Animate background colors and hover states smoothly at 60 FPS:
```era
// Parses transition tokens
let spec = tw_parse_style("transition-all duration-200 hover:bg-indigo-500");

// Steps transition interpolator in render loop
let mut tm = TransitionManager::new();
tm.set_hover("btn_id", true, col_idle, col_hover, 200);
tm.step(16); // Advances 1 frame (16ms)
```

### C. Multi-line Rich Text Input (`tw_textarea`)
Full 2D cursor navigation, line-wrapping, and selection:
```era
let mut model = TextAreaModel::new("Line One\nLine Two".to_string());
model.insert_char("Hello World".to_string());
model.insert_newline();

let editor = tw_textarea("code_box", "w-[600] h-[300] p-4 bg-slate-950", model, true, true);
```

### D. Hot Reload & Live Preview
Evaluate declarative template files at runtime in **< 1 ms**:
```era
let dsl = "box \"p-4 bg-slate-900 rounded-xl\"
text \"text-lg font-bold text-white\" \"Live Preview Title\"
badge \"bg-emerald-500/20 text-emerald-400\" \"0ms Hot Reload\"";

let root = tw_eval_dsl(dsl);
```

---

## 4. Component Primitives Reference

| Widget | Purpose | Example |
| :--- | :--- | :--- |
| `tw_box` | Styled container box | `tw_box(id, "p-4 bg-slate-900", child)` |
| `tw_card` | Elevated surface card | `tw_card(id, "p-4", child)` |
| `tw_row` / `tw_row_2` | Horizontal flex layout | `tw_row_2(id, "gap-3 items-center", c1, c2)` |
| `tw_col` / `tw_col_2` | Vertical flex layout | `tw_col_2(id, "gap-2", c1, c2)` |
| `tw_grid` | Responsive CSS grid | `tw_grid(id, "gap-4", 3, items)` |
| `tw_stack` / `tw_stack_2` | Layered / absolute overlay | `tw_stack_2(id, "w-10 h-10", avatar, badge)` |
| `tw_btn` | Clickable action button | `tw_btn(id, "Send", "click_action", "bg-indigo-600")` |
| `tw_badge` | Status pill indicator | `tw_badge(id, "LIVE", "bg-emerald-500/20")` |
| `tw_chip` | Filter / room selector | `tw_chip(id, "#general", true, "sel_room", "")` |
| `tw_avatar` | User profile avatar | `tw_avatar(id, "A", "bg-indigo-600")` |
| `tw_divider` | 1px horizontal rule | `tw_divider(id, "w-full bg-white/10")` |
| `tw_icon` | Unicode vector glyph | `tw_icon(id, "search", "text-slate-400")` |
| `tw_modal` | Centered modal backdrop | `tw_modal(id, screen_w, screen_h, dialog)` |
| `tw_tooltip` | Popover hover tooltip | `tw_tooltip(id, "Copy link", "")` |
| `tw_textarea` | Multi-line text editor | `tw_textarea(id, "w-full h-40", model, true, true)` |

---

## 5. Development Watch Mode
Run the live hot-reload development server:
```powershell
.\scripts\dev_watch.ps1
```
Monitors all `.tw` templates and `.era` source files, delivering instant live updates to the running application.
