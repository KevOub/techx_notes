1) Install docker
2) Install docker-compose (easier with `pip3 install`)
3) `git clone` the CTFd repository
4) Realize the mariadb docker image does not support Rasberry Pi's :(
5) Use this in the docker-compose to get a bootable CTFd instance https://hub.docker.com/r/tobi312/rpi-mariadb/

---
- remove `pybluemonday` (this is a golang library which hates me and my life. The arm binaries do not work with Docker or with manual install, so gut the whole thing)
- modify `setup.sh` so that the script uses `pip3` not `pip`	
- `setup.sh` uses `apt` to install the backend code and `pip` for all the python libraries
- Then, use `gunicorn -w 4  -b 0.0.0.0:8080 wsgi:app -k gevent` if everything installed to give the web-browser something to look at :)
	- (Turns out, there is a weird edge glitch using `sync` instead of `gevent`. Explaining it is *not* fun and I leave exploring it to your own peril)

https://nopresearcher.github.io/Deploying-CTFd/


`sudo vim /etc/systemd/system/ctfd.service`

```
[Unit]
Description=Gunicorn instance to serve ctfd
After=network.target

[Service]
User=pi
Group=www-data
WorkingDirectory=/var/www/CTFd
ExecStart=gunicorn --bind unix:app.sock --keep-alive 2 --workers 3 --worker-class gevent 'CTFd:create_app()' --access-logfile '/var/log/CTFd/CTFd/logs/access.log' --error-logfile '/var/log/CTFd/CTFd/logs/error.log'


[Install]
WantedBy=multi-user.target
```