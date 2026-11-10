<img src="doc/digits-landing.png" alt="Digits landing page">

# Digits

Digits is a personal contact manager built with Next.js, React Bootstrap, NextAuth, Prisma, and PostgreSQL. Save contact details, edit your address book, and record timestamped notes about conversations. Each user sees only their own contacts; administrators can review all contacts and notes.

## Installation

1. Install Node.js 20.19 or later (Node.js 22 LTS recommended) and PostgreSQL.
2. Obtain access to the private [application source repository](https://github.com/CMDAwesome/digits), clone it, and run `npm install` in the `digits` directory. This public `digits-docs` repository contains only documentation and cannot run the application.
3. Create a PostgreSQL development database named `digits` (for example, `createdb digits`).
4. Copy `sample.env` to `.env`. Set `DATABASE_URL` to your own PostgreSQL connection string, such as `postgresql://USER:PASSWORD@localhost:5432/digits?schema=public`. URL-encode special characters in the password. Set `AUTH_SECRET` to a randomly generated secret and `AUTH_URL` to `http://localhost:3000`. Never commit `.env`.
5. Run `npx prisma generate`, then `npx prisma migrate deploy` to create the tables.
6. Run `npx prisma db seed` (or `npm run seed`). Both commands use the same awaited, repeatable seed implementation. The development accounts are `john@foo.com` and `admin@foo.com`, each with password `changeme`. John owns Philip Johnson and Henri Casanova; the administrator owns Kim Binsted. These example credentials are for local development only.
7. Run `npm run dev` and open <http://localhost:3000>.

Run `npm run lint` to check code style. Run `npm run build` followed by `npm start` for a production build. New schema changes should be developed with `npx prisma migrate dev --name NAME`.

## Walkthrough

### Landing page

The landing page introduces Digits and explains its contact management features. The navigation provides a Login menu with Sign In and Sign Up. Signed-in users receive Add Contact and List Contacts links.

### Sign In

<img src="doc/digits-signin.png" alt="Sign In page">

Enter an account email and password to open your contact list. Passwords are stored as bcrypt hashes. The user menu includes sign-out and password-change options.

### Sign Up

<img src="doc/digits-signup.png" alt="Sign Up page">

Register using an email address, password, and matching password confirmation. New accounts have the USER role and begin with an empty personal address book.

### List Contacts

<img src="doc/digits-list.png" alt="John's two seeded contacts">

Contact cards show a photograph, name, address, and description. The responsive grid displays one, two, or three columns depending on screen size. The server filters contacts and notes by the current user's email.

### Add Contact

<img src="doc/digits-add.png" alt="Add Contact form">

Provide first name, last name, address, image URL, and description. Submit saves the contact and returns to the list; Reset clears the fields. The server assigns ownership from the authenticated session rather than trusting the hidden owner field.

### Edit Contact

<img src="doc/digits-edit.png" alt="Edit Contact form">

Select Edit on a contact card to open `/edit/ID`. The form starts with that contact's saved values. Submit updates the contact; Reset restores the initial values. Pages and server actions reject access to contacts owned by someone else.

### Add Note

<img src="doc/digits-notes.png" alt="A timestamped note on Scott Robertson's contact card">

Each personal contact card includes a Note text area and Add Note button. Submission creates a separate Note record linked to that contact and returns to the list. Notes show their creation date in Hawaii time and appear only on their associated card. The contact ID and current owner are supplied automatically.

### Administrator

<img src="doc/digits-admin.png" alt="List Contacts (Admin) with owners and notes">

Administrators receive an Admin navigation link. List Contacts (Admin) displays every contact, its owner email, and its notes. Ordinary users are redirected away from this page. Administrators' personal List Contacts page still shows only their own contacts.

The screenshots above were captured from this local application. Scott Robertson and his note were created during the add/edit/note walkthrough, not by the seed script.

## Data model and source

The final data model contains User, Contact, and Note. Notes reference Contact IDs; contact and note ownership uses account email addresses. Migrations preserve the template initialization, Contact addition, Note relation, and removal of the template's sample inventory model.

The application source repository is [CMDAwesome/digits](https://github.com/CMDAwesome/digits) (private; grading requires collaborator access). This separate public repository contains only documentation and screenshots. GitHub Pages hosts the project homepage, not the Next.js application or database.
