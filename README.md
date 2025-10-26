# Phyubox blog

## Getting up and running
The following commands will allow you to get up and running if deploying the blog post in a new VM - these are more notes than instructions so next time I have to do this I can just copy and paste the commands :)


```bash
sudo apt install libpq-dev rbenv npm
rbenv install 3.2.6
rbenv global 3.2.6
sudo apt install ruby-full
sudo gem install bundler
sudo bundle install
bundle exec jekyll serve --host 0.0.0.0
```

this should get you up and running with the blog as is 

Much love 

Past Paul