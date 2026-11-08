# TourFleet Desk

A web app for tourism vehicle rental companies.

- **Calendar**: a monthly calendar of every tour, coloured by payment status (paid, partly paid, pending, overdue). Click a day to add a tour.
- **Companies**: run one or more rental companies from the same login.
- **Vehicles**: cars, vans and buses with number plate, seats, daily rate and driver.
- **Tours & payments**: customer or group, tour dates, vehicles, total price, due date and every payment received. It warns you when a vehicle is double-booked.
- **Availability**: pick a date range to see which vehicles are free, plus a day-by-day chart.
- **Itinerary**: one entry per day (route, places, hotel, km). "Copy as text" copies it for WhatsApp or email.

It is a single HTML page that uses Firebase for Google sign-in (Firebase Auth) and the database (Cloud Firestore).

## Files

| File | What it is |
| --- | --- |
| `public/index.html` | The whole app |
| `public/firebase-config.js` | Your Firebase project settings (you fill this in) |
| `firestore.rules` | Database security: only approved Google accounts can read or write |
| `firebase.json` | Firebase Hosting and rules setup |

## Setup

1. **Create a Firebase project** at <https://console.firebase.google.com>.
2. **Add a web app**: Project settings > General > Your apps > Web (`</>`). Copy the `firebaseConfig` values into `public/firebase-config.js`.
3. **Turn on Google sign-in**: Build > Authentication > Get started > Sign-in method > Google > Enable.
4. **Create the database**: Build > Firestore Database > Create database (production mode, pick a region such as `asia-south1`).
5. **Approve your staff**: in `firestore.rules`, put the Google email addresses that may use the system in the list.
6. **Deploy** (needs Node.js):
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase use --add          # pick your project
   firebase deploy             # uploads the site and the security rules
   ```
   Your site is then live at `https://<your-project>.web.app`.

To add another staff member later, add their email to `firestore.rules` and run `firebase deploy --only firestore:rules`.

If you use your own domain, add it under Authentication > Settings > Authorized domains so Google sign-in works there.

## Run it on your computer

```bash
firebase serve       # or: npx serve public
```
Open <http://localhost:5000>. `localhost` is already an authorized domain for sign-in.

If `firebase-config.js` is not filled in, the app still opens and saves data in that browser only, which is handy for trying it out.
