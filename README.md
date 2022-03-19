# NASflix
A Docker Stack for Media Management.  Includes Tv(sonarr), Movies(radarr), Music(lidarr) tools to search, download, track new and existing content.  Files are sent to rTorrent seedbox client for downloading, and synced back to local volumes using Resillo Sync.

## Used Resources:
-  **Guides**
    - https://trash-guides.info/How-to-setup-for/Synology/
-  **Synology DSM Free Port 80/443**
    - https://www.reddit.com/r/synology/comments/ahs3xh/prevent_dsm_listening_on_port_80443/   

- **Apps in this Stack**
    - Sonarr for Tv
    - Radarr for Movies
    - Lidarr for Music
    - Navidrome for Listening to Music
    - Jackett for Indexers
    - Resillo Sync for folder syncing
    - Pullio for container updates
    - Organizr for a dashboard of sorts
    - Tautulli for Plex Stats    
    - Bliss for Music Tag/Cover Art Mgmt
  
- **Apps on SeedBox**
    - NZBGet 
    - qBittorent 
    - Resillo Sync (BTSync) - Syncs completd torrent/nzb folder with local download folder.
   
- **Subscriptions / Services**
    - Newshosting.com (Monthly) Paid yrly $7
    - NBZPlanet.net
    - Bliss - 79$ one time to get unlimited fixes

- **Reverse Proxy**
  - Resillo is the only app not included in the reverse proxy, but the rest are added and controled through there enviroment variables in the compose file
    - VIRTUAL_HOST=xyz.nasflix 
    - VIRTUAL_PORT=1234
    - List of domains included
      - tv.nasflix - Sonarr
      - movies.nasflix - Radarr
      - music.nasflix - Lidarr
      - listen.nasflix - Navidrome
      - bliss.nasflix - Bliss 
      - jackett.nasflix - Jackett
      - home.nasflix - Tautulli    

- **Reverse Proxy on Rasberry Pi**
  - We route traffic to this container via our raspberry pi, and it handles picking up the domain names  via DNSMasq and send thems to our container. 
  -  **sync.nasflix** 
       - is handled on the pie seperatlly using its own nginx conf to route the traffic directly to its port of 8888 and 55555 ?   IDK it ends up using a relay with our nginx container setup, so we handle its DNS out side of this stack.  IT STILL avaiable at the ``` hostIP:8888 ```      


---


## **.env File config**
We can control the user information, timezone, and then all our config, media and download paths.  There is also an entry for a discord webhook address for Pullio update notifications.  And Bliss uses the Spotify information to fetch album/artist art.

```
NASFLIX_PUID=1028
NASFLIX_PGID=100
NASFLIX_UMASK-002
NASFLIX_TZ=America/New_York

NASFLIX_CONFIGS=/volume1/nasflix/appdata
NASFLIX_MEDIA=/volume1/nasflix/media
NASFLIX_DOWNLOADS=/volume1/nasflix/downloads
REMOTE_DOWNLOAD_PATH=/home/jekyll1886/files/completed/

NASFLIX_UPDATE_NOTIFY_WEBHOOK=

NASFLIX_SPOTIFY_SECRET=
NASFLIX_SPOTIFY_ID=
```  


---
## Folders that need creating
List out which folders are used, needed, then outline that.
- **Folder Structures**
  - In the .env we fake the seedbox path that the download clients report.  This allows us to not have to use Path Mapping and has worked great.  

---

## Portainer Install
```
docker run -d --name=portainer -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer:/data --restart=always portainer/portainer-ce
```
---
## DSM Free up port 80 / 443 
  This is set up as a one time task on our box, so if/when DSM updates, we can just run it again.  Otherwise SSH in and run these commands to move 80 to 81 and 443 to 444.   Freeing them up for Docker useage.
 ``` 
sed -i -e 's/80/81/' -e 's/443/444/' /usr/syno/share/nginx/server.mustache /usr/syno/share/nginx/DSM.mustache /usr/syno/share/nginx/WWWService.mustache

 sudo systemctl restart nginx
```
---
## Permissions / Users 
Set up permissions and user per Trash's Resource Guide and then these commands can be very hand to set folders to correct owner and permissions
```
sudo chown -R docker:users /folder

sudo chmod -R a=,a+rX,u+w,g+w /folder  (775)
```
---




## Bliss / Spotify Intergration for Artist Images
- https://www.navidrome.org/docs/usage/external-integrations/#spotify


## Pullio 
TODO ---> Pullio handles the automatic updates , this task needs to be added to the docker tasks still.

