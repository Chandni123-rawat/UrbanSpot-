# ParkShare Lite

A standalone local college-demo project. It uses its own PostgreSQL cluster and database.

## Run on Windows

Install Node.js 20.19+ or 22.12+ and PostgreSQL 18. The backend starts the Lite database automatically. In a PowerShell terminal opened at this folder, run:

```powershell
cd backend
npm ci
npm run dev
```

In a second terminal opened at this folder:

```powershell
cd frontend
npm ci
npm run dev
```

Open http://127.0.0.1:5183. The API is on port 15053. The database is `parkshare_lite` on loopback port 55440. Its files live in this copy's ignored `.local/postgres` folder. If PowerShell blocks local scripts, run `powershell -NoProfile -ExecutionPolicy Bypass -File .\Setup-Database.ps1`. If PostgreSQL is installed elsewhere, pass `-PostgresBin 'C:/path/to/PostgreSQL/bin'`.

Register as an Owner or Seeker. To create an Admin, register one separate account, then run `node dev-db.cjs admin your-email@example.com` from `backend` and sign in again. Owners must set availability after listing a space. Admin approves it before the seeker can find it.

### Maps and payments

Copy `frontend/.env.example` to `frontend/.env.local` and configure `VITE_GOOGLE_MAPS_API_KEY` for Google Maps JavaScript, Places (New) and Routes. Restart Vite. Without a key, the map and alternate destination search remain available.

For payments, set your own `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET` and `RAZORPAY_WEBHOOK_SECRET` in `backend/.env`. Use Razorpay Test Mode and enable automatic capture. A public HTTPS webhook forwarding to `POST /api/payments/webhook` in this copy is needed for asynchronous provider notifications. Do not reuse the main project's webhook endpoint. Subscribe to `payment.captured`, `payment.failed`, `refund.created`, `refund.processed` and `refund.failed`.

### Stop PostgreSQL

```powershell
& 'C:/Program Files/PostgreSQL/18/bin/pg_ctl.exe' -D "$PWD/.local/postgres" -m fast stop
```
