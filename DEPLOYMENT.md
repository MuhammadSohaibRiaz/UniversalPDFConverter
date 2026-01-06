# cPanel Deployment Guide

## Issues Fixed

1. ✅ Added `express` dependency to `package.json` (required by `server.js`)
2. ✅ Updated start script to use `server.js` for production
3. ✅ Created `.htaccess` for Passenger configuration

## Deployment Steps

### 1. Upload Files to cPanel

Upload all project files to your cPanel directory (e.g., `/home/rapid/toPdf/`), **EXCEPT**:
- `node_modules/` (will be installed on server)
- `.next/` build folder (will be generated on server)

### 2. Install Dependencies

**IMPORTANT**: Use the correct command:
```bash
npm install
```
**NOT** `npm run install` (that command doesn't exist)

This will install all dependencies including `express` which was missing.

### 3. Build the Application

```bash
npm run build
```

This creates the `.next` production build folder.

### 4. Configure Passenger

The `.htaccess` file is already configured for your setup:
- Points to `server.js` as the entry point
- Uses Node.js 18 (matches your server version: v18.20.8)
- App root: `/home/rapid/toPdf`

**Note**: If your actual path is different, update the `PassengerAppRoot` in `.htaccess`.

### 5. Set Environment Variables (if needed)

If you need to set `NODE_ENV=production` or `PORT`, you can:
- Set them in cPanel's Environment Variables section, OR
- Create a `.env` file in your project root

### 6. Restart Application

After installation and build:
- Restart the application through cPanel's Passenger interface, OR
- Touch the `tmp/restart.txt` file (Passenger will auto-restart)

## Troubleshooting

### If you still get "Cannot find module" errors:

1. **Verify installation**: Check that `node_modules` exists and contains `express`:
   ```bash
   ls -la node_modules/express
   ```

2. **Check Node.js version**: Ensure Passenger is using Node.js 18:
   ```bash
   node --version
   ```

3. **Rebuild**: Delete `node_modules` and `.next`, then reinstall:
   ```bash
   rm -rf node_modules .next
   npm install
   npm run build
   ```

4. **Check file permissions**: Ensure files are readable:
   ```bash
   chmod -R 755 .
   ```

### Common Issues:

- **Wrong command**: Use `npm install` not `npm run install`
- **Missing build**: Run `npm run build` before starting
- **Wrong path**: Update `PassengerAppRoot` in `.htaccess` to match your actual path
- **Node version mismatch**: Ensure Passenger uses Node.js 18 (update `.htaccess` if needed)

## File Structure After Deployment

```
/home/rapid/toPdf/
├── .htaccess
├── server.js
├── package.json
├── package-lock.json
├── node_modules/          (installed via npm install)
├── .next/                 (generated via npm run build)
├── app/
├── components/
├── lib/
├── public/
└── ... (other project files)
```

