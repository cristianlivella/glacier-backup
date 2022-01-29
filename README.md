# Glacier Backup

### Work in progress 🚧
This repository is empty at the moment, don't expect to find anything useful here.

## The issue and the idea
In 2021, after the [fire at the OVH data center](https://twitter.com/olesovhcom/status/1369478732247932929), I started taking the backup question seriusly.

I configured [rclone](https://rclone.org/) on every server and computer I own, and I started making (at least) an automatic backup per day on [Wasabi](https://wasabi.com/).

I was pretty happy about it, but after some months I figured out I was spending a lot of money, and I would have been able to save about the 80% by using [AWS S3 Glacier Deep Archive](https://aws.amazon.com/s3/storage-classes/glacier/) instead.

However, the transition is not so simple, rclone out of the box probably doesn't work well with Glacier Deep; due to the fact I'm basically making a 1:1 copy of my disks, I'll end up uploading a lot of small file and making a lot of API requests. Not a big issue with Wasabi (API requests are free, and the minimum billable file size is 4 KB), but a bigger issue for Glacier Deep, when you pay for each API request and the minium billable file size is 128 KB.

The simple idea I have to solve both issues is to create zip archives with more files, to minimize the number of files uploaded and so the API requests.

Obviusly I cannot zip and upload everything every day, because I'll end up with a lot of bandwith wasted, and I'll incounter another issue (common to Wasabi and Glacier Deep): the minimum life of each file is 6 months (3 months on Wasabi); if I delete a file before the 6 months is past, I'll keep paying for it until it reach 6 months since the upload date.

The idea so is to zip and upload only new or modified files, and keep track of deleted files. Then when it makes sense (need to do some calc and simulation) the zip archives that contains files no longer needed will be removed from AWS, and the still needed files in those archives will be re-packed in new archives.

This is just the basic idea, it sounds doable and cool to me, but I still need to figure out and define exactly how everything will happen.

Of course all files will also be encrypted, as I already do now with rclone.

Initially I want to focus only on how to make backups, and then move on to how to explore them (probably with a nice web interface) and how to restore them.
