**User Guide: Importing GPS Coordinates into Dawarich**

Introduction:
This user guide provides step-by-step instructions on how to extract GPS coordinates from photos and import them into the Dawarich service. This process is useful for adding points of interest from memorable locations into Dawarich, especially when Google Location History is unavailable or was not enabled.

Requirements:
- Mac OS operating system
- exiftool software installed
- Template file provided in the PR

Steps to Import GPS Coordinates into Dawarich:

1. Download and install exiftool from the official website.
2. Download the template file attached to the PR.
3. Create a separate directory for the photos from which you want to extract coordinates.
4. Move the necessary photos to this directory.
5. Open Terminal and navigate to the directory containing the photos.

Command to Execute:
```
exiftool -if '$gpsdatetime' -fileOrder gpsdatetime -p ./gpx.fmt -d %Y-%m-%dT%H:%M:%SZ *JPG > output.gpx
```

Note: Ensure that exiftool is properly installed on your system, and the file 'gpx.fmt' is located in the same directory as the photos.

