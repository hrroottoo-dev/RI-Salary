# RI Salary — Netlify + Supabase starter

This is a starter foundation for Roottoo Innovation's RI Salary payroll system. It is **not yet a completed production payroll product** and is not automatically connected to your existing `risalary.netlify.app` site.

## Included
- Vite + React application
- Netlify build and SPA routing configuration
- Supabase client configuration
- Annual CTC / Gross to monthly salary calculator
- Editable Basic and HRA percentages
- PF and ESI switches, PT/TDS/LOP input
- Print-to-PDF calculation
- Supabase schema for organizations, roles, employees, salary structures, statutory rules, attendance, payroll runs, payslips and audit logs
- Row Level Security starter policies and private payslip bucket

## 1. Create Supabase project
1. Create a project at https://supabase.com/dashboard.
2. Open SQL Editor and run `supabase/schema.sql`.
3. Open Project Settings → API. Copy Project URL and the publishable key (or legacy anon key).
4. Authentication → Providers: enable Email. Configure email confirmation and redirect URLs for your Netlify domain.
5. Storage: confirm the private `ri-payslips` bucket exists.

## 2. Configure locally (Windows)
1. Install current Node.js LTS.
2. Extract this ZIP.
3. Open the extracted folder in VS Code.
4. Copy `.env.example` to `.env`.
5. Add your Supabase URL and publishable/anon key to `.env`.
6. Open Terminal in VS Code and run:
   ```powershell
   npm install
   npm run dev
   ```
7. Test the calculator and sign-in UI.

## 3. Deploy to Netlify
1. Push the project to a private GitHub repository.
2. In Netlify choose Add new project → Import an existing project.
3. Select the repository. Build command: `npm run build`; publish directory: `dist`.
4. In Netlify Project configuration → Environment variables, add:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
5. Trigger a new deploy.
6. In Supabase Auth URL Configuration, set Site URL and allowed redirect URLs to your Netlify domain.

## 4. Important security rules
- Never put a Supabase `service_role` key in browser code, `.env` variables prefixed with `VITE_`, GitHub, or Netlify client-side variables.
- Use only the publishable/anon key in the browser and enforce RLS on every exposed table.
- Review and test every RLS policy with separate owner, HR, payroll and viewer accounts before storing real employee data.
- Do not store full bank account numbers, Aadhaar numbers, passwords or other unnecessary sensitive data in this starter.
- Add server-side Edge Functions for payroll finalisation, approval, PDF generation, bulk exports and statutory filings. Never trust client-side totals for a final payroll run.

## 5. Production modules still to implement
This starter does not yet include the full employee CRUD screens, organization onboarding, invitation/role management, attendance imports, approval and lock workflow, bank transfer file, payslip template, monthly payroll register, statutory ECR exports, PT/LWF state tables, gratuity/bonus, tax regime and TDS engine, backups, or audit review UI. Those require integration and testing before production use.

## 6. Statutory configuration
Do not treat the demo PF/ESI switches as an eligibility decision. Store rule values with effective dates and source references. Validate the current Gazette notifications, scheme rules, employee membership status, statutory wage definitions, exclusions, contribution allocation, state PT/LWF slabs and rounding before each payroll period. PF employer contribution must be split correctly among EPF/EPS/EDLI/administration where applicable. ESI applicability is based on the legally defined wage and coverage conditions, not simply a user-selected yes/no.

## 7. Existing site integration
To update the existing `risalary.netlify.app` safely, provide the current website source ZIP or GitHub repository. Do not replace the live site with this starter until its existing database, authentication, records and features have been inspected and migrated.
