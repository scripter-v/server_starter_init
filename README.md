#Server::Starter init

##Requirements
* [Server::Starter](https://metacpan.org/pod/Server::Starter)
* Your preffered PSGI server ([Twiggy](https://metacpan.org/pod/Twiggy) for example).

##Install
At this time here is only the init-script for debian. It can be used with any PSGI-server with [Server::Starter](https://metacpan.org/pod/Server::Starter) support. Just copy 'sstarter' to init.d dir: `cp sstarter /etc/init.d/` and make them executable: `sudo chmod +x /etc/init.d/sstarter`. Then edit following variables in `/etc/init.d/sstarter`:
```
MAX_WORKERS=2
PSGI_APP='/path_to_psgi_app/app.psgi'
HTTP_SERVER="plackup --no-default-middleware -s Twiggy::Prefork --max-workers $MAX_WORKERS -a $PSGI_APP"
```
Keep in mind that not all servers support unix-sockets (i use [patched Twiggy::Prefork](https://github.com/scripter-v/Twiggy-Prefork)) therefore you may need to replace

```DAEMON_ARGS="--path $SOCKET -- $HTTP_SERVER $LOGGER"```

with

```DAEMON_ARGS="--port $PORT -- $HTTP_SERVER $LOGGER"```

##Logging
start_server's output (start/stop/reload and so on) is redirected to syslog with **'start_server'** tag and **'daemon'** facility with **'notice'** severity. Worker's access log is disabled (by `--no-default-middleware` in my case). I prefer nginx to logging requests.
