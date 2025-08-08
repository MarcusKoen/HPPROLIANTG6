🦆 Step-by-Step: DuckDNS IP Updater via Cron
✅ 1. Get your DuckDNS token

    Go to https://www.duckdns.org

    Log in

    Copy your token and subdomain

Let’s say:

    Subdomain: marcusserver

    Token: abcdefgh-1234-abcd-5678-efghijklmnop

✅ 2. Create DuckDNS update script

Create a new script:

mkdir -p ~/duckdns
nano ~/duckdns/update.sh

Paste this:

#!/bin/bash

echo "Updating DuckDNS..."

URL="https://www.duckdns.org/update?domains=marcusserver&token=abcdefgh-1234-abcd-5678-efghijklmnop&ip="

curl -s "$URL" > ~/duckdns/duck.log

echo "Updated at $(date)" >> ~/duckdns/duck.log

    Replace marcusserver and the token with yours.

Make it executable:

chmod +x ~/duckdns/update.sh

✅ 3. Add it to Cron (updates every 5 mins)

Edit crontab:

crontab -e

Add this line at the bottom:

*/5 * * * * ~/duckdns/update.sh >/dev/null 2>&1

This will run the script every 5 minutes and update your public IP on DuckDNS.
✅ 4. Check it works

After a few minutes, check the log:

cat ~/duckdns/duck.log

It should say:

OK
Updated at 2025-08-08 09:25:00

You can also ping marcusserver.duckdns.org or access it in your browser.
✅ Next Step: Use your DuckDNS domain in Vaultwarden

Now that your DuckDNS name always points to your current IP, you can:

    Use it in Caddy or Nginx for HTTPS:

        Example: vault.marcusserver.duckdns.org

    Forward ports 80 and 443 on your router to your server.
