
# Install pip
brew install pip

# Install virtualenv
sudo pip install virtualenv

# Setup virtualenv for pelican
mkdir ~/.pelican
virtualenv ~/.pelican
cd ~/.pelican
source bin/activate # start virtualenv for pelican

# Install pelican and markdown (Markdown translates to html format)
pip install pelican
pip install Markdown

# Create dir for blog
mkdir ~/blog

# Setup blog with pelican
pelican-quickstart
# bunch of steps which essentially setup your pelican settings and populate
# blog directory with various files

# Add a blogpost
vim first_blog.md
-
Title: My first blogpost
Date: 2018-09-27 23:01
Category: Test

Testing out pelican for blogging.
-

mv first_blog.md content/

# Generate blogpost static content
pelican content
# you now have a website

# Start local webserver to check out your blog
make devserver
# go to localhost:8000 in your browser now
# make stopserver

# Customize theme
git clone https://github.com/getpelican/pelican-themes.git ~/.pelican
# Use any theme inside the repo. I used bootstrap
pelican-themes --install ~/.pelican/pelican-themes/bootstrap
pelican-themes -l # you should be able to see the theme you just installed

# Update pelicanconf.py and add your theme
-
THEME = 'bootstrap'
-

# regen static content
pelican content
# now start webserver to see if that worked

# Publish to s3
# in order to do this you need to set up s3cmd for setting up s3 configuration/authentication

# Create s3 bucket
# set your bucket to display static content
# optionally add a domain name redirect if you have your own domain name
# Create IAM user which either has total access to s3 or has for the specific bucket you wish to upload your site to
# get access key id and secret key for the user
brew install s3cmd
s3cmd --configure
# feed s3cmd *only* access key and secret key (optionally encryption) and leave everything else the same
s3cmd ls
# you should see the bucket you wish to upload to

# edit makefile and set the s3 bucket
-
S3_BUCKET=bharatblogtest
-
make s3_upload

# use your bucket's endpoint url or domain name to access your blog!




