# HydroShield NWA — Affiliate Customer Intake Form

A mobile-first single-page app used by sales reps at affiliate stone fabrication shops to submit new customer referrals into the HydroShield CRM.

## Live URL

> `https://intake.hydroshieldnwa.com`

Append `?am_id=YOUR_ID` to track the referring affiliate:
```
https://intake.hydroshieldnwa.com?am_id=abc123
```

## How It Works

1. **Step 1 — Customer Info:** Rep enters customer contact details and selects the referring affiliate shop.
2. **Step 2 — Rooms & Surfaces:** Rep adds each room and the surfaces to be treated (type, material, square footage).
3. **Step 3 — Review & Submit:** Rep reviews a clean summary, then submits.

On submit, a single POST payload is sent to the n8n webhook, which handles all CRM record creation in GoHighLevel (contact upsert, room/surface custom objects, opportunity, and email notification to Mike).

## Affiliate Tracking

The `am_id` query parameter is read silently on page load and included in the submission payload. It is never shown in the UI.

## Tech

- Single file: `affiliate-customer-intake.html`
- Vanilla JS, no frameworks, no dependencies
- All CSS and JS inline — upload anywhere and it works

## Webhook

| | |
|---|---|
| **Endpoint** | `https://snowflakeventures.app.n8n.cloud/webhook/hydroshield-affiliate-new-customer` |
| **Method** | `POST` |
| **Auth** | `Bearer hydroshield-affiliate-intake-2026` |

## Payload Shape

```json
{
  "submitted_at": "2026-03-29T12:00:00.000Z",
  "contact": {
    "first_name": "",
    "last_name": "",
    "email": "",
    "phone": "",
    "address1": "",
    "city": "",
    "state": "",
    "postal_code": "",
    "affiliate_shop": "",
    "am_id": ""
  },
  "rooms": [
    { "room_name": "", "room_type": "", "room_notes": "" }
  ],
  "surfaces": [
    {
      "room_index": 0,
      "surface_type": "",
      "material_type": "",
      "square_footage": 0,
      "surface_notes": ""
    }
  ]
}
```

## Deployment

Hosted on GitHub Pages at `intake.hydroshieldnwa.com` via a GoDaddy CNAME record pointing `intake` → `pnutmarketeer.github.io`.

To update: edit `affiliate-customer-intake.html`, commit, and push to `master`. Changes are live within ~60 seconds.
