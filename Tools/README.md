# NTFY 
It is a notification service that can send notifications to your phone and chrome.
https://ntfy.sh/
 ### Use Case
 - Get notified eg. "Release Successful" or "Release Failed" after a build is done.
 - Get status of your server as a daily notification.<br><br>
 ![Ntfy Eg.](../assets/stats.jpeg)
 - Get you recurring activity status.<br><br>
 ![Ntfy Eg.](../assets/ntfy.jpeg)<br><br>
 ### Docker Deployment
change the network name and ip address as per your network.
```bash
sudo docker run -d --name ntfy --network deploy --ip 11.0.0.2 binwiederhier/ntfy
```

# UptimeKuma
It is a self-hosted monitoring tool that provides a simple interface to monitor your services.It can notify you about your service status via.
- Email
- Slack
- Discord
- Telegram
- Ntfy
<br><br>
![Ntfy Eg.](../assets/uptimekuma.png)

 ### Docker Deployment
change the network name and ip address as per your network.
```bash
docker volume create uptime-kuma
docker run -d -v uptime-kuma:/app/data --name uptime-kuma --network deploy --ip 11.0.0.2 louislam/uptime-kuma
```

# Metabase 
It is a self-hosted business intelligence tool that provides a simple interface to visualize your data and create dashboards. It can connect to various databases and provide insights into your data.
<br><br>
[Metabase](https://www.metabase.com/) 
