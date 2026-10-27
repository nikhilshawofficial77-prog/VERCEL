# Clinic Feedback Portal — Vercel version

This project is prepared for Vercel. Existing rows in the earlier local SQLite database are not automatically migrated; this version starts with the hosted PostgreSQL database. The static portal is served from `index.html`; Python Functions in `api/` provide the API; feedback records live in hosted PostgreSQL. It no longer writes data to a local SQLite file.

## Deploy it

1. Extract this ZIP. Create a private GitHub repository and upload the **contents** of this folder so `index.html`, `api/`, `db.py`, `requirements.txt`, and `vercel.json` are at the repository root.
2. In Vercel, select **Add New → Project**, import the repository, and deploy. The project uses Vercel's Python Functions runtime.
3. In the Vercel dashboard, open **Storage / Marketplace** and add a PostgreSQL provider such as Neon. Connect the database to this Vercel project. Confirm the project has an environment variable named `DATABASE_URL` containing the database connection string (with SSL enabled). If the provider creates it under a different name, add `DATABASE_URL` as an alias with that same connection string.
4. In **Project Settings → Environment Variables**, add `ADMIN_TOKEN` with a long, random secret value. Apply both variables to Production and Preview as appropriate. Do not commit these values to GitHub.
5. Redeploy after adding or changing environment variables. Vercel creates the `feedback` table automatically on the first API request.
6. Open the deployed site. Patient feedback can be submitted without a login. Click **Dashboard** and enter the `ADMIN_TOKEN` value to see, update, or delete entries. The token is kept in that browser tab's session storage; closing the tab clears it.

Vercel's Python runtime is currently documented as Beta. See [Python Functions](https://vercel.com/docs/functions/runtimes/python), [Marketplace storage](https://vercel.com/docs/marketplace-storage), and [environment variables](https://vercel.com/docs/environment-variables).

## Local preview

Install Python 3 and the package in `requirements.txt`, then set `DATABASE_URL` and `ADMIN_TOKEN` in your shell. Vercel's `vercel dev` command is the closest local preview of Vercel routing. The original long-running `server.py` has been removed because Vercel invokes request handlers as Functions.

## Privacy and production use

The admin token is a simple shared-secret gate, not per-user accounts or a full authentication system. Anyone with the token can view and delete feedback. Use a long random value, limit who receives it, and add an appropriate authentication provider, backups, retention rules, and privacy/security controls before collecting real patient information. The form does not collect medical records, but names and comments may still be sensitive.
