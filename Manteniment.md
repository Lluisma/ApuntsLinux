
# Linux : Manteniment


## 8 Simple Ways To Free Up Space On Ubuntu

1. Get rid of packages that are no longer required [Recommended]
    
    `sudo apt-get autoremove`


2. Clean up APT cache in Ubuntu

    `sudo du -sh /var/cache/apt`
    `sudo apt-get autoclean (o sencer: sudo apt-get clean)`

3. Clean the thumbnail cache

    `sudo du -sh ~/.cache/thumbnails`
    `rm -rf ~/.cache/thumbnails/*`

4. Remove old Linux kernels that were manually installed [For Experts]

5. Remove orphaned packages [For Experts]

    `https://itsfoss.com/free-up-space-ubuntu-linux/`

6. Uninstalling unnecessary applications [Recommended]

7. Find and remove duplicate files