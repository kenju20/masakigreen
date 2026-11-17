# Masaki Green Realty — cPanel Deployment

This is a static website: HTML, CSS, JavaScript, fonts and images. It needs no PHP, no database and no Node.js on the server.

## 1. What to upload

Upload the **contents** of the `dist` folder, not the `dist` folder itself:

```
index.html
favicon.png
robots.txt
.htaccess        (hidden file, optional but recommended)
assets/          (whole folder: CSS, JS, fonts, logo, property photos)
```

Never upload `src/`, `node_modules/`, `package.json` or any other project file.

To build `dist` yourself: run `npm install` once, then `npm run build`.

## 2. Where to upload in cPanel File Manager

1. Log in to cPanel and open **File Manager**.
2. Open the domain's document root. For the main domain this is usually `public_html`.
3. Tick **Settings → Show Hidden Files (dotfiles)** so you can see `.htaccess`.
4. Click **Upload** and upload `masaki-green-realty-dist.zip`.
5. Right-click the zip, choose **Extract**, and extract into the same folder. You should see `index.html` and `assets/` directly inside `public_html`.
6. Delete the zip after extracting.

**Subfolder preview (optional):** to show the client without replacing a live site, create `public_html/preview/` and extract there. The site will be at `https://masakigreenrealty.co.tz/preview/`. All asset paths are relative, so no changes are needed.

## 3. Domain document root

- **Main domain:** the document root is `public_html`, so nothing needs configuring.
- **Addon domain or subdomain:** go to cPanel → **Domains** and check the domain's **Document Root** column. Upload to that folder.
- Make sure **SSL/TLS Status** shows a valid certificate (AutoSSL), and that **Domains → Force HTTPS Redirect** is on.

## 4. Client-side routing

The site uses hash-based addresses, for example `/#/properties` and `/#/property/family-villa-with-pool-masaki`.

- Page refreshes and direct links work on any static host.
- **No rewrite rules are needed.**
- The included `.htaccess` only adds caching and security headers. If it causes a 500 error on your host, delete it; the site works without it.

## 5. Verify after upload

Open the site in a private/incognito window and check:

- [ ] The homepage loads with the logo, fonts and styling.
- [ ] **Properties**: filters change the results, and pages 1 and 2 work.
- [ ] A property page opens; refresh it and confirm it still loads.
- [ ] The gallery arrows, full-screen view and brochure preview work.
- [ ] **Compare** and **Saved** keep their selections after a refresh.
- [ ] The contact form shows validation errors when empty, then the demonstration message.
- [ ] The WhatsApp button opens a chat with +255 657 633 698.
- [ ] Test on a phone: the menu, the filters panel and the bottom contact bar all work.
- [ ] The browser console (F12) shows no red errors.

## 6. Replacing a build safely

1. Build the new version (`npm run build`) and zip the contents of `dist`.
2. In File Manager, select everything in the document root, choose **Compress**, and save `backup-YYYY-MM-DD.zip` outside `public_html` (for example in your home folder).
3. Delete the old `index.html` and `assets/` folder. Leave other sites' folders, `.well-known/` and `cgi-bin/` alone.
4. Upload and extract the new zip.
5. Hard-refresh (Ctrl+F5) and repeat the checks in section 5.
6. **Rollback:** delete the new files and extract the backup zip.

Each build gives asset files new names, and `index.html` is not cached, so visitors get the new version straight away.

## Known limitations of this static prototype

- **Forms:** enquiry forms validate input, then hand the message to WhatsApp with the visitor's details filled in. Nothing is stored or emailed by the server. Newsletter sign-up shows a "coming soon" message. Real form delivery needs the Laravel backend.
- **Saved, compare and recently viewed:** these are stored in the visitor's own browser, on that device only.
- **Brochure:** shown as a print-friendly preview. "Print / Save as PDF" uses the browser's print dialog. Automatic server-side PDF generation comes with the backend.
- **Search engines:** the site can be indexed. Because page addresses use `#/`, Google sees mainly the homepage. Separate indexable URLs per property, a sitemap and per-page metadata come with the Laravel build.
- **Content:** 8 listings (MGR-0001 to MGR-0008) are real. The 6 listings with SMP references, their illustrated images and the Insights articles are samples, labelled on the site. The legal pages are drafts that need legal review.
- **Adding or editing listings:** edit `src/data/properties.js`, put photos in `src/assets/photos/` named `mgr-00XX-1.jpg`, `-2.jpg` and so on, then rebuild and re-upload. A self-service admin dashboard comes with the Laravel backend.
