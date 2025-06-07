## About this directory
Up to this point, I have used yt-dlp for downloading from Crunchyroll, Funimation, and YouTube.

The subdirectories contained in this root directory contain one `yt-dlp.conf` file for each streaming service. 
Each of those `yt-dlp.conf` files has some distinct purpose that separates it from the other files in the subdirectories. 
Those files also link to the root `yt-dlp.conf` file except `./dubs/yt-dlp.conf` which is a supplementary config file. 
This setup acts as a sort of inheritance of config files.

Each service's config file also archives downloaded movies into a file named after that service (e.g., `.crunchyroll_archive`). The name starts with a period (.) because I prefer hiding the file.

## Purpose of the files
NOTE: For the following relative locations below, assume the present directory is `{path to root}/conf_files/`. 

### ./yt-dlp.conf
- Contains all of the standard options that I want in every use of the `yt-dlp` command.

- All other .conf files are in separate subdirectories. If those files choose to reference this primary .conf file, they will do so with `--config-locations ../`

### ./crunchyroll/yt-dlp.conf
- Some minor configurations specific to Crunchyroll.

- Conforms MOSTLY to the Jellyfin naming standard for ["Shows" files](https://jellyfin.org/docs/general/server/media/shows)

### ./dubs/yt-dlp.conf
I would use this in conjunction with a separate `--config-locations` parameter so that you get the specific login info for that service AND use this for filtering dubs

- NOTE this config file does not reference the primary config file.

So far, I have only needed to download a series explicitly labeled as "Dubs" from Crunchyroll  (e.g., Attack on Titan vs Attack on Titan (Dubs)). 
This config file has a means of sorting out videos that don't contain ([Eng[lish] ]Dub[s]) in the title using regex. 

The laymans terms means this filters out videos that don't contain (Dub) or (Dubs) in parenetheses with (Eng ...) or (English ...) as optional prefixes. 
Any word besides English and any word that isn't specifically Dub or Dubs will be considered invalid and will not pass the regex matching. As a result, it won't be downloaded.

Some passing examples:
- (Eng Dub)
- (English Dub)
- (english dUbS)

Some failing examples: 
- (Portugese Dubs)
- (English Dubbed)
- English dubs <-note: there are no parentheses 

### ./archival/yt-dlp.conf
This configuration consists of one command: search the playlist solely for videos posted within the last 30 days.  This is helpful if you're using an automated cron job to perform recurring searches and don't want to attempt to download entire playlists all over again. 

NOTE that using an archive file to prevent re-downloading files DOES NOT prevent yt-dlp from attempting to download the file first before checking with the archive file.

### ./funimation/yt-dlp.conf
 This config file contains the explicit extractor args for funimation (currently commented out) and a reference to cookies. Otherwise, there's nothing special here.

### ./youtube/yt-dlp.conf


## Other Talking Points
- You DO NOT need to use these files. They are all customized for my personal experience. You can copy and modify them however you wish.

- You can use multiple config files in the CLI `yt-dlp` command. If you go to `cd conf_files/`, where `conf_files` is the root 
  directory for these config files, then you can run the following command:
	`yt-dlp --config-locations ./crunchyroll --config-locations ./dubs` 
  to get all of the common commands plus those applicable to Crunchyroll and filtering for English dubs.

- If using multiple config files, be careful with the use and order of the crunchyroll and funimation configs, should you choose to use them. The --username and --password flags of one may override the other. 
  Fortunately, I don't see an overlap of these services to worry about that, but it's still worth mentioning.

- To override any of the default parameters in the config files, you can specify those parameters explicitly after the `--config-locations ...` parameters. For example:
	`yt-dlp --config-locations yt-dlp-conf/funimation --username newUsername --password 12345 --concurrent-fragments 2 www.url.com/video`

- The default `yt-dlp.conf` file as well as the service-related subdirectories (Crunchyroll, Funimation) use different 
  archive files. If you want one single archive file for all videos, consider commenting out the `--archive-file` parameters from 
  those respective `yt-dlp.conf` files.
