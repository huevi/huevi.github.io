# Web Scraping without DB using Github Actions


# Basic idea

To use github repositary as a storage for webscraping using Github actions(software workflow automation CI/CD tool).

1. Github actions gets triggered when a push happens to the repo or based on a corn job schedule. 
2. It checks out the repo
3. Runs the scripts 
4. Pushes the entire repo with data back to the github using [publish action](https://github.com/mikeal/publish-to-github-action)


```yaml
name: Scrape Twitter Trends Data

on:
  push:
  workflow_dispatch:
  schedule:
    - cron:  '0 */8 * * *' # Every hour. Ref https://crontab.guru/examples.html

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: get twitter data
      env:
        ACCESS_TOKEN_SECRET: ${{ secrets.ACCESS_TOKEN_SECRET }}
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        CONSUMER_KEY: ${{ secrets.CONSUMER_KEY }}
        CONSUMER_SECRET: ${{ secrets.CONSUMER_SECRET }}
      run: |
        pip3 install install pandas tweepy pytz
        python3 twitter_trends.py
    - uses: mikeal/publish-to-github-action@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # GitHub sets this for you
```


if the script uses sensitive api keys then it can be declared using [github secrets](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)

# Limitation 

* To handle large files github provides [git-lfs](https://git-lfs.github.com/)
* Github actions [usage limits](https://docs.github.com/en/actions/getting-started-with-github-actions/about-github-actions#usage-limits)


# Example repositories

* https://github.com/huevi/daily-twitter-trends-data
* https://github.com/huevi/daily_youtube_trends 

Happy data scraping/archiving!!

**This work is inspired from user [sw-yx](https://github.com/sw-yx)'s [repo](https://github.com/sw-yx/gh-action-data-scraping)**
