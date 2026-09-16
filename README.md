# Offline QR and Barcode Scanner POC

## Files

- `index.html`: scanner UI, IndexedDB queue and simulated synchronization
- `service-worker.js`: offline application-shell and scanner-library cache
- `manifest.webmanifest`: installable PWA metadata

## GitHub Pages deployment

Copy all three application files to the same repository folder. The page must be served over HTTPS. Open the page online once and reload it once so the Service Worker controls the page and the external scanner library is cached. After that, test offline mode from browser developer tools or by disabling the network.

## POC synchronization behavior

Every scan is first written to IndexedDB with status `pending`. When `navigator.onLine` is true, `sendToCloud()` waits briefly and marks the scan as `synced`. Replace `sendToCloud()` with a real authenticated `fetch()` call later. Keep the generated event `id` as the idempotency key.

## Important test

After first online load and one reload:

1. Turn network offline.
2. Reload the page. It should load without the Chrome dinosaur page.
3. Scan codes. They should show `Waiting`.
4. Restore the network. Items should automatically change to `Synced`.
