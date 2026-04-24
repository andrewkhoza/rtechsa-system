<div align="center">

# Rtech CRM — Relevant Technologies

[![PHP](https://img.shields.io/badge/PHP-%3E%3D8.0-8892BF.svg)](https://php.net/)
[![Yii2](https://img.shields.io/badge/Yii2-Advanced%20Template-1E6887.svg)](https://www.yiiframework.com/)
[![License](https://img.shields.io/badge/License-BSD--3--Clause-blue.svg)](LICENSE.md)
[![Status](https://img.shields.io/badge/Status-Live%20%26%20Active-brightgreen.svg)]()

A full-featured CRM and e-commerce platform built on Yii2, powering the day-to-day operations of **Relevant Technologies** — a South African tech repair and retail company. The system manages everything from device booking and repair tracking to online product sales, customer communication, and staff workflows.

> **Note:** This repository contains the README only. The source code is proprietary and actively running in production. Interested parties are welcome to [get in touch](#contact--demos) for a system walkthrough or source code demo.

</div>

---

## Table of Contents

- [Features](#features)
- [Technology Stack](#technology-stack)
- [User Roles](#user-roles)
- [Repair Workflow](#repair-workflow)
- [Integrations](#integrations)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Directory Structure](#directory-structure)
- [License](#license)
- [Contact & Demos](#contact--demos)

---

## Features

<details>
<summary><strong>Repairs & Service Management</strong></summary>

- **Device Booking** — Check-in laptops, phones, and other devices with client details, reported issues, and condition notes
- **Repair Pipeline** — Structured workflow from diagnosis through to collection (see [Repair Workflow](#repair-workflow))
- **Technician Assignment** — Auto or manual assignment of technicians to repair jobs
- **Quotations & Invoicing** — Professional PDF quotes and receipts via mPDF
- **QR Code Labels** — Print QR-coded labels for quick device lookup
- **Digital Signatures** — Capture client signatures on check-in and collection via Signature Pad
- **Device Timeline** — Full audit trail of all status changes and user actions per device
- **Write-offs & Cancellations** — Handle devices that are uneconomical to repair or cancelled jobs

</details>

<details>
<summary><strong>E-Commerce (Tech Shop)</strong></summary>

- Product catalogue with category browsing, price/condition filters, and sorting
- Session-based cart with database persistence for logged-in customers
- Per-user wishlist stored in the database
- Secure checkout powered by **Yoco** (South African payment gateway)
- Promotions page for discounted items
- Verified customer product reviews with star ratings

</details>

<details>
<summary><strong>Customer & Lead Management</strong></summary>

- Client database with contact details, alternative numbers, and email history
- Public lead repair form — prospective customers can book before creating an account
- Contact/enquiry form with follow-up tracking for sales and support
- Customer portal — clients view their orders, repair history, and device status
- Automated expiring review-request links sent after job completion

</details>

<details>
<summary><strong>Staff & Access Control</strong></summary>

- Six-role RBAC with tiered permissions (see [User Roles](#user-roles))
- Configurable session timeout and auto-logout for inactive staff
- 2FA support for enhanced admin security
- Instant account freeze/unfreeze by administrators

</details>

<details>
<summary><strong>Communication & Notifications</strong></summary>

- **SMS** — Automated client updates at key repair milestones via Twilio / Bulk SMS
- **Email** — Welcome emails, password resets, quotation alerts, status updates, and confirmations
- **In-App Notifications** — Staff notification centre for repair updates and system alerts

</details>

<details>
<summary><strong>Reporting & Exports</strong></summary>

- Dashboard analytics — daily, weekly, monthly, quarterly, and yearly repair status breakdowns
- Excel/CSV data export via `kartik-v/yii2-export`
- Thermal-friendly device label printing

</details>

---

## Technology Stack

| Layer | Technology |
|---|---|
| Backend Framework | Yii2 Advanced App Template |
| Language | PHP >= 8.0 |
| Frontend | Bootstrap 4, jQuery, AJAX |
| Database | MySQL / MariaDB |
| PDF Generation | mPDF |
| QR Codes | `Da\QrCode` / BaconQrCode |
| Maps | Google Maps JavaScript API |
| Payment Gateway | Yoco |
| SMS | Twilio / Bulk SMS |
| Email | Symfony Mailer / Gmail |
| Security | reCAPTCHA, 2FA (`2amigos/2fa-library`) |

---

## User Roles

| Role | ID | Access |
|---|---|---|
| **Administrator** | `10` | Full system access — users, settings, repairs, shop, reports |
| **Manager** | `20` | All repairs, staff oversight, quotations, approvals |
| **Technician** | `30` | Assigned repairs, diagnosis notes, parts requests |
| **Front Desk** | `40` | Device check-in/out, client interactions, booking forms |
| **Sales** | `50` | Customer orders, enquiries, product management |
| **Customer** | `100` | Shop, cart, wishlist, own orders, repair tracking |

---

## Repair Workflow

```
Walk-in / Lead Form
        │
        ▼
    ┌────────┐     ┌───────────┐     ┌────────┐     ┌──────────┐     ┌────────────────┐
    │ Booked │ ──► │ Diagnosis │ ──► │ Quoted │ ──► │ Approved │ ──► │ Under Repairs  │
    └────────┘     └───────────┘     └────────┘     └──────────┘     └───────┬────────┘
                                                                              │
                                                                              ▼
    ┌──────────┐     ┌──────────────────────┐     ┌────────────────┐
    │ Returned │ ◄── │ Ready for Collection │ ◄── │ Awaiting Parts │
    └────┬─────┘     └──────────────────────┘     └────────────────┘
         │
         ▼
  [Review Request Sent]
```

Devices that are uneconomical to repair or abandoned are routed to **Write-off** or **Cancelled** states at any point in the pipeline.

---

## Integrations

| Service | Purpose |
|---|---|
| **Yoco** | Card payments for shop orders |
| **Twilio / Bulk SMS** | Automated SMS notifications to customers |
| **Google Maps** | Location input and address autocomplete |
| **Google Analytics** | Site traffic tracking |
| **reCAPTCHA** | Form spam protection |
| **Payflex** | Buy-now-pay-later referral option |

---

## Requirements

- PHP >= 8.0 with PDO (pdo_mysql), Fileinfo, and GD or ImageMagick
- MySQL / MariaDB
- Apache or Nginx with `mod_rewrite` / equivalent
- Composer
- Valid SSL certificate (required for Yoco live payments)
- SMTP credentials or a mail server

---

## Installation

### 1. Clone and install dependencies

```bash
git clone git@github.com:MuziB-hub/RtechCrm.git
cd RtechCrm
composer install
```

### 2. Initialise the environment

```bash
php init        # Linux / macOS
init.bat        # Windows
```

Select `Development` or `Production` as appropriate.

### 3. Configure the database

Create a MySQL database (e.g., `rtechsa`), then update `common/config/main-local.php`:

```php
'db' => [
    'class'    => 'yii\db\Connection',
    'dsn'      => 'mysql:host=localhost;dbname=rtechsa',
    'username' => 'root',
    'password' => 'your_password',
    'charset'  => 'utf8mb4',
],
```

Run migrations (or import the provided schema if migrations are not available):

```bash
php yii migrate
```

### 4. Configure web server document roots

| App | Document Root | URL |
|---|---|---|
| Frontend | `frontend/web/` | `http://localhost/` |
| Backend | `backend/web/` | `http://backend.localhost/` |

Ensure these directories are writable by the web server:

```
frontend/runtime/
frontend/web/uploads/
backend/runtime/
console/runtime/
```

> **Tip:** Use `775` with correct ownership in production — avoid `777`.

---

## Configuration

### Email

In `common/config/params.php` or `params-local.php`:

```php
'adminEmail'   => 'admin@rtechsa.com',
'supportEmail' => 'info@rtechsa.com',
'websiteEmail' => 'info@rtechsa.com',
```

### Yoco Payments

Configure your public and secret keys via `params-local.php` or directly in the relevant controller. Uses the official `yoco/yoco-php` SDK.

### Twilio / Bulk SMS

Set Twilio credentials as application components or parameters for the `BulkSms` / Twilio component in `params-local.php`.

### reCAPTCHA

Add your Site and Secret keys to `params-local.php` for the relevant public forms.

### Google Maps

Provide a valid Google Maps JavaScript API key in params or directly in map widget configurations.

### 2FA (Optional)

Enable two-factor authentication for admin accounts via the `2amigos/2fa-library` integration in the admin user settings.

---

## Directory Structure

```
├── common/
│   ├── components/       # AccessRule, Notifications, BulkSms
│   ├── config/           # Shared configuration
│   ├── mail/             # Email view templates
│   └── models/           # User, LoginForm, ControlAccess
│
├── frontend/
│   ├── assets/           # Asset bundles
│   ├── components/       # Frontend components
│   ├── config/           # Frontend app config
│   ├── controllers/      # Site, Repairs, Shop, CRM, etc.
│   ├── models/           # Repairs, Shop, CRM models
│   ├── views/            # Public site + staff dashboard views
│   ├── web/              # Entry point, uploads/, images/
│   └── widgets/          # Custom frontend widgets
│
├── backend/
│   ├── controllers/
│   ├── views/
│   └── web/              # Backend entry point
│
├── console/
│   ├── controllers/      # Cron, Order, Tools
│   ├── migrations/       # Database migrations
│   └── runtime/
│
└── environments/         # Dev / prod environment overrides
```

---

## License

Licensed under the **BSD-3-Clause License**. See [LICENSE.md](LICENSE.md) for details.

---

## Contact & Demos

This repository is intentionally published as a README-only showcase — the full source code is proprietary and live in production for an active client.

If you're interested in any of the following, feel free to reach out:

- **System demo or walkthrough** — a live presentation of the platform's features and workflows
- **Source code review** — a private demo of the codebase for vetting or evaluation purposes
- **Similar build** — commissioning a comparable system for your business

📧 **[info@genesisdigital.co.za](mailto:andrew@genesisdigital.co.za)**
🌐 **[genesisdigital.co.za](https://genesisdigital.co.za)**

> Built by [Genesis Digital Solutions](https://genesisdigital.co.za)
