1. Update your packages.

       $ sudo apt update

1. Download and install chrome

       $ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

> If you get 'wget' command not found, that means you do not have wget installed on your machine. Simply install it by running: `sudo apt install wget`

1. Then you can install chrome from the downloaded file.

       $ sudo dpkg -i google-chrome-stable_current_amd64.deb

   if errors, try:

       $ sudo apt-get -y install -f

1. Check Chrome is installed correctly.

       $ google-chrome --version

This version is important, you will need it to get the chromedriver.

[font: skolo online docs](https://skolo.online/documents/webscrapping/#step-1-download-chrome)
