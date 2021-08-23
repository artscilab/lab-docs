# Common Erros

## Internal Server Error (500)

Although there can be several reasons for this error, most of the times it occurs due to 100% disk quota usages in DigitalOcean server. To fix this:
- Login to server using ssh
- Delete backups in `/var/cdBackup/db/backupFiles` except last few.
- Restart the server suing `sudo service nginx restart`
- It will take few minutes to get website up and running again!

## Bad Gateway (502)

The production environment of ArtSciLab website is run on digital ocena server using [PM2](https://pm2.keymetrics.io/) which is process manager for Node.js. 

To check what caused error,
```
pm2 logs
```

After fixing the issue, like outdated package, old node version, wrong configuration, etc. , restart pm2 using
```
pm2 restart all
```

---

Contributors: Omkar Ajnadkar
