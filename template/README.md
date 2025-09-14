# template.yt-dlp.conf

This file lays out the basics required to include the base yt-dlp.conf file and guidance one where to override the base options.

## How To Use It

Assuming you're in the project's root directory:

```bash
mkdir <some folder name>
cd <some folder name>
cp ../template/template.yt-dlp.conf yt-dlp.conf
```

For the folder name, using the name of the service or website I am downloading from makes it simple to find the appropriate yt-dlp.conf file.

## Example

Suppose I want to download the video at the URL `vodservice.com/?videoid=abc123`, then my commands would look like the following:

```bash
mkdir vodservice
cd vodservice
cp ../template/template.yt-dlp.conf yt-dlp.conf

#edit the file accordingly
vim yt-dlp.conf  

#run
yt-dlp --config-locations ./vodservice vodservice.com/?videoid=abc123
```