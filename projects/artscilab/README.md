# ArtSciLab Home Page

The [ArtSciLab home page](https://artscilab.atec.io) runs on a pretty simple, if unconventional, architecture. 

It comprises of two parts:
  - A wordpress installation where content admins manage the contents
  - A frontend written in Next.js (server-side-rendered React)

We use the [Wordpress REST API](https://developer.wordpress.org/rest-api/) to expose routes that allow the front-end to query the content. This way, we can achieve a little bit of a best-of-both-worlds type situation: Our content managers can keep using the systems they know and love to update posts, projects, and lab members, and the developers can do a lot really quickly with a simple Next.js app. 

## Wordpress

Like Creative Disturbance, we use the Pods plugin to create custom content types like `Lab Members` and `Projects` and extend existing types like `Posts` with additional information. 

The installation lives in the `atec.io` network installation, at [dev.atec.io](https://dev.atec.io). The reason for the name "dev" is that it used to be an actual development site, before the decision was made to use this architecture. Because the actual name of the site that serves the API doesn't matter, it was left as-is.

### Internal Server Error (500)

Although there can be several reasons for this error, most of the times it occurs due to 100% disk quota usages in DigitalOcean server. To fix this:
- Login to server using ssh
- Delete backups in `/var/cdBackup/db/backupFiles` except last few.
- Restart the server suing `sudo service nginx restart`
- It will take few minutes to get website up and running again!

## Frontend 

The frontend, as mentioned, is written using [Next.js](https://nextjs.org/). Next.js allows us to write pages as React components. They're rendered on the server before being sent to the user, which allows for decent SEO out of the box. 


---

Contributors: Al Madireddy
