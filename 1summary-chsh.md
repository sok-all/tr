# dns, ssl, user logging, fs, cron, daemons, awk/grep etc

## dns, ssl cert

`record registration`
- noip.com
- cloudns.net

`ssl cert`
- apt-get install **certbot**
- certbot certonly --webroot -w [ path-to-project-root ] -d [ domain ]       
  certbot certonly --webroot -w /var/www/html -d 16hn.ddns.net
