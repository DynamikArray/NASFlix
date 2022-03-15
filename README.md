# NASflix
A Docker Stack for Media Management.  Includes Tv(sonarr), Movies(radarr), Music(lidarr) tools to search, download, track new and existing content.  Files are sent to rTorrent seedbox client for downloading, and synced back to local volumes using Resillo Sync.

## Used Resources:
-  **Guides**
    - https://trash-guides.info/How-to-setup-for/Synology/


- **Apps**
    - Sonarr for Tv
    - Radarr for Movies
    - Lidarr for Music
    - Jackett for Indexers
    - Resillo Sync for folder syncing
    - Pullio for container updates
    - NZBGet / RTorrent /Resillo (BTSync) on Seedbox
  
- **Subscriptions / Services**
    - Newshosting (Monthly) Paid yrly $7
    - NBZPlanet.net
   
## Portainer Install 
```
docker run -d --name=portainer -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer:/data --restart=always portainer/portainer-ce
```

# Permissions / Users 
``` 
sudo chown -R docker:users /folder 
sudo chmod -R a=,a+rX,u+w,g+w /folder 
```

# Pullio 
Pullio handles the automatic updates 

