# TourFleet Desk

A web app for tourism vehicle rental companies.

- **Calendar**: a monthly calendar of every tour, coloured by payment status (paid, partly paid, pending, overdue). Click a day to add a tour.
- **Sign up with Google**: anyone can sign in and create their own company. Each company sees only its own vehicles, tours and payments.
- **Team**: the company owner invites staff by Gmail address; they sign in and click *Join company*.
- **Vehicles**: cars, vans and buses with number plate, seats, daily rate and driver.
- **Tours & payments**: customer or group, tour dates, vehicles, total price, due date and every payment received. It warns you when a vehicle is double-booked.
- **Availability**: pick a date range to see which vehicles are free, plus a day-by-day chart.
- **Itinerary**: one entry per day (route, places, hotel, km). "Copy as text" copies it for WhatsApp or email.
- **Invoices**: open a tour and click *Make invoice*. Invoice numbers (INV-0001, INV-0002…) are given automatically, items are filled in from the tour (edit, add a discount line or VAT %), and payments already received are shown with the balance due. *Print / Save PDF* prints it or saves a PDF to send by WhatsApp or email. Add your logo, address and bank details under Companies > Edit so they appear on every invoice (the logo also shows at the top of the app).

It is a single HTML page that uses Firebase for Google sign-in (Firebase Auth) and the database (Cloud Firestore).

## Files

| File | What it is |
| --- | --- |
| `public/index.html` | The whole app |
| `public/firebase-config.js` | Your Firebase project settings (you fill this in) |
| `firestore.rules` | Database security: each company's data is visible only to its members |
| `firebase.json` | Firebase Hosting and rules setup |

## Setup

1. **Create a Firebase project** at <https://console.firebase.google.com>.
2. **Add a web app**: Project settings > General > Your apps > Web (`</>`). Copy the `firebaseConfig` values into `public/firebase-config.js`.
3. **Turn on Google sign-in**: Build > Authentication > Get started > Sign-in method > Google > Enable.
4. **Create the database**: Build > Firestore Database > Create database (production mode, pick a region such as `asia-south1`).
5. **Publish the security rules**: copy `firestore.rules` into Firestore > Rules and click Publish (or use `firebase deploy` below).
6. **Deploy** (needs Node.js):
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase deploy             # uploads the site and the security rules
   ```
   Your site is then live at `https://<your-project>.web.app`.

## How accounts work

- Anyone who signs in with Google can create a company. They become its **owner**.
- Data is stored per company: `companies/{id}`, `companies/{id}/vehicles`, `companies/{id}/tours`.
- The owner opens **Companies > Edit** and invites staff by email. Staff sign in with that Google account and click **Join company**. The owner can remove members.
- Members can add and edit vehicles and tours. Only the owner can invite, remove people or delete the company.

If you use your own domain, add it under Authentication > Settings > Authorized domains so Google sign-in works there.

## Run it on your computer

```bash
firebase serve       # or: npx serve public
```
Open <http://localhost:5000>. `localhost` is already an authorized domain for sign-in.

If `firebase-config.js` is not filled in, the app still opens and saves data in that browser only, which is handy for trying it out.
