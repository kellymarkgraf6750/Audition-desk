# AuditionDesk

A self-contained static web app for audition scoring and casting notes.

## GitHub Pages

1. Create a new GitHub repository.
2. Upload `index.html` to the repository root.
3. Open **Settings -> Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select the `main` branch and `/ (root)`, then save.
6. Wait for GitHub Pages to publish the site. Open the published URL on your iPad in Safari.

## Data

This prototype stores audition data in the browser using localStorage. It does not send data to a server and has no external dependencies.

## Reset / Backup

Use the in-app backup and restore controls in Setup. For serious audition use, export a JSON backup periodically.

## Notes

This is a prototype. The production version can later replace localStorage with Supabase while preserving the same interface and data model.
