# Iron Meals — ordering site

A single-page ordering site for Iron Meals. Customers browse the menu, build an order with the 3-for-£14 deal applied automatically, and send it to the Instagram DM (@ironmeals2026) to pay by bank transfer. Also lists the two in-person stockists in Antrim.

It's one self-contained `index.html` — no build step, no dependencies (images and fonts are handled inside the file). `vercel.json` just adds standard security headers.

---

## 1. Put it live on Vercel

Use the GitHub route — it's the one that lets you edit the menu later and have it go live automatically.

1. Create a free account at https://github.com and https://vercel.com (sign into Vercel **with GitHub**).
2. On GitHub: **New repository** → name it `iron-meals` → Create.
3. On the repo page: **Add file → Upload files** → drag in `index.html` and `vercel.json` → **Commit changes**.
4. On Vercel: **Add New… → Project** → **Import** the `iron-meals` repo.
5. Framework Preset: **Other**. Leave build settings blank. Click **Deploy**.
6. You get a live URL like `iron-meals.vercel.app` — that's the link to send.

**Custom domain (optional):** Vercel project → **Settings → Domains** → add e.g. `order.ironmeals.co.uk` and follow the DNS steps.

---

## 2. Update the menu later (the part you'll do most)

1. On GitHub, open the repo and click `index.html`, then the **pencil (Edit)** icon.
2. Press **Ctrl/Cmd + F** and search for: **`MENU — EDIT HERE`**
3. You'll land on the menu. Each meal is one line, in quotes, ending with a comma:

   ```
   {cat:"Pasta", icon:"🍝", items:[
     "Spaghetti Bolognese",
     "Lean Mince Pasta Bolognese",
   ]},
   ```

   - **Add a meal:** add a new line inside that category — `"New meal name",`
   - **Remove a meal:** delete its line.
   - **Rename a meal:** change the text between the quotes.
   - **New whole category:** copy a full `{cat:"...", icon:"...", items:[ ... ]},` block, paste it, change the name/icon/items. The menu tabs and sections build themselves — nothing else to touch.

4. Keep the quotes and the commas. Don't use "curly" quotes — straight `"` only.
5. Click **Commit changes**. Vercel redeploys in ~30 seconds and the live site updates.

**Tip:** before committing big changes, download the edited `index.html` and double-click it to open in a browser. If the menu shows correctly, it's good. If the page is blank, you've dropped a quote or comma — undo and try again.

---

## 3. Other things you can change

All in `index.html` (use Ctrl/Cmd + F to find them):

- **Instagram handle** — search `INSTAGRAM_HANDLE` (near the top).
- **Stockists** — search `Fountain Street` / `High Street` to edit names, addresses, hours.
- **Prices** — search `const UNIT` for the maths (`UNIT` = per meal, `TRIO` = price for any 3). If you change a price, also update the visible text: search `£4.99` and `3 FOR` in the hero banner.
- **Images / logo** — the hero graphic and meal photo are embedded as `data:image/jpeg;base64,...`. To swap in full-res originals, replace those strings (or get your developer to do it).

---

## 4. Bank-transfer auto-reply (set up once on Instagram)

1. Make the Iron Meals Instagram a **Business** or **Creator** account.
2. On desktop: **Meta Business Suite → Inbox → Automations**.
3. Create a **custom keyword automated response**, keyword: `ORDER`.
4. Reply with the bank details, e.g.:

   > Thanks for your order! To confirm it, pay by bank transfer:
   > Account name: [NAME]
   > Sort code: [XX-XX-XX]
   > Account number: [XXXXXXXX]
   > Reference: your full name
   > Once it's in we'll get your meals out to you.

Every order from the site contains the word "ORDER", so this fires automatically. The site already tells the customer to use their name as the reference.

---

## Good to know

- **Semi-automated, not hands-off:** the bank details send automatically, but confirming payment and dispatching is a manual check of the banking app.
- **No order database or card payment.** Orders arrive as Instagram DMs. A card checkout (e.g. Stripe) would be a separate build.
- **Instagram limitation:** no website can pre-type into an Instagram DM, so the order is copied to the clipboard and the customer pastes it — the site handles this and tells them what to do.
