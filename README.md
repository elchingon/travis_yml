travis_yml
==========

My TravisCi yml file for Rails

This config file for TravisCi is for a standard Rails project with Rspec and Selenium testing. The export DISPLAY and sh xvfb is for Selenium browser tests. 

One must also create a config/database.travis.yml file that has the test db information. You must make username travis or root and you can skip password Here is my setup for Mysql:

---
  test:
    adapter: mysql2
    encoding: utf8
    database: my_app_test
    username: travis
    
---

This config also installs the bundle_cache gem to cache bundle file to a AWS S3 bucket. This speeds up testing by a great deal. 
To do this, you must also install the travis gem to generate your S3 secret.
I followed the instructions from this website
http://leopard.in.ua/2013/10/12/speed-testing-on-travis/

Also you'll need to add your AWS credentials to a secure section there. Adding of this credentials is simple:

1. Install the travis gem using command gem install travis
2. Log into Travis with travis login --auto (from inside of your project respository directory), for Travis Pro use command travis login --pro.
3. Encrypt your S3 credentials using command travis encrypt AWS_S3_KEY="" AWS_S3_SECRET="" --add (be sure you add your actual credentials inside the double quotes). You can get your S3 key and secret from the file you generate in you Account settings. If you don't have that file, you can regenerate your credentials.

In your .travis.yml file something like this will be added :

---
env:
  global:
  - BUNDLE_ARCHIVE="test-bundle"
  - AWS_S3_BUCKET="travisci-bundler-cache"
  - RAILS_ENV=test
  - secure: wqeqweheo3H743Iob4s8qweqwec0tcv0JGlg8JBhccCPnIiFUArqwe=
---

The first time it runs it will create the tar in your bucket and in subsequent runs it will pull the bundle file from the bucket. Works really good.

Check out the website to add further speed changes.
Enjoy
