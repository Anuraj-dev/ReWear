<p align="center">
  <img src="app/static/WhatsApp%20Image%202025-07-12%20at%204.32.38%20PM.jpeg" alt="ReWear logo" width="88" height="88" />
</p>

<h1 align="center">ReWear</h1>

<p align="center">
  A clothing swap site for items you no longer wear.<br />
  Offer one of yours for another member's item, or spend points to take it home.
</p>

<p align="center">
  <a href="#getting-started">Getting started</a>
  ·
  <a href="#how-swaps-work">How swaps work</a>
  ·
  <a href="#configuration">Configuration</a>
  ·
  <a href="https://github.com/Anuraj-dev/ReWear/issues">Issues</a>
</p>

ReWear is a clothing swap site. Members list clothes they no longer wear, and an admin approves each listing before other members can see it. You can then offer one of your own items for a direct swap, or spend points on an item and offer nothing back.

It is a Flask app that runs on your own machine. It stores data in SQLite and renders pages with Jinja2 and Bootstrap 5. You don't need API keys or third-party accounts. Bootstrap and Font Awesome load from a CDN, so pages need internet access to look right.

## What you get

- **Two ways to get an item.** A direct swap offers one of your approved listings in return. A point redemption spends the item's point value and needs nothing from you in return.
- **Points that move with each trade.** Every account starts with 100 points. When a direct swap is accepted, both members get 10 points. When a point redemption is accepted, the points move from the requester to the owner.
- **Approval before anything goes live.** New listings stay hidden until an admin approves them. Rejecting a listing deletes it and cancels its pending swap requests.
- **Structured listings.** Each listing has a category of Men, Women or Kids, and a type of Shirt, Pants, Dress or Others. Sizes run from S to XL, conditions from New to Acceptable. You add comma-separated tags and set the point value yourself. Search matches titles and descriptions, and filters by category, type, size and point range.
- **Images by link.** A listing takes an image URL. There is no upload, so host your photos elsewhere and paste the link.
- **Server-side checks.** Passwords are stored as Werkzeug hashes, and session cookies are signed with `SECRET_KEY`. Admin pages refuse any session without the admin flag.
- **A profile with your numbers.** Your profile shows total listings, approved listings, completed swaps and your current points.

## Getting started

You need Python 3.9 or newer. Run these commands from the project root.

```sh
git clone https://github.com/Anuraj-dev/ReWear.git
cd ReWear
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

On Windows, activate the environment with `venv\Scripts\activate`.

Next, create the database. Git ignores the `migrations/` folder, so `flask db init` creates it on your machine.

```sh
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
python run.py
```

Open http://localhost:5000 to see the site. `run.py` starts Flask in debug mode, so keep it to local use. The SQLite database lives at `instance/site.db`. Create your own account at `/auth/register`.

### Load sample data

```sh
python sampledata.py
```

The script deletes all existing users, listings and swaps first. Then it creates six members who all use the password `password123`, such as `alice@example.com`. It also creates an admin at `admin@rewear.com` with the password `admin123`. Sign in as the admin to reach the admin pages and approve listings.

## How swaps work

Every swap request has one of two types and moves through four states: Pending, Accepted, Rejected and Cancelled. Only a Pending request can change state. The requester can cancel it, and the item's owner can accept or reject it.

- A direct swap offers one of your approved, available listings. When the owner accepts, both listings are marked Swapped and each member gets 10 points.
- A point redemption needs your balance to cover the item's point value when you send the request. When the owner accepts, the points move from you to the owner and the item is marked Redeemed.
- A listing page can also redeem an item straight away, with no request and no owner step. The points move and the item is marked Redeemed.

## Configuration

| Variable | Default | Purpose |
| --- | --- | --- |
| `SECRET_KEY` | A development key in `app/__init__.py` | Signs session cookies. Set a long random value before you run the app anywhere other people can reach. |

`SECRET_KEY` is the only setting read from the environment. The database path is fixed in `app/__init__.py`.

## Contributing

Fork the repository, create a branch for your change, and open a pull request against `main` on [GitHub](https://github.com/Anuraj-dev/ReWear).
