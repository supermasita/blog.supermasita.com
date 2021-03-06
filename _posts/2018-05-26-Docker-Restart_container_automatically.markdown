---
layout: post
title:  "Docker - Restart container automatically"
date:   2018-05-26 07:04:16 -0300
tags: docker computers
---
Checkmate! The server has rebooted and all the containers are DOWN.

Its easy to make the containers reboot automatically. Use a **[RestartPolicy](https://docs.docker.com/config/containers/start-containers-automatically/#use-a-restart-policy)**

Edit the restart policy for an already running container:
```
$ docker update --restart=always container01
container01
```

Check the changes: 
```
$ docker inspect container01 | grep RestartPolicy -A 3
            "RestartPolicy": {
                "Name": "always",
                "MaximumRetryCount": 0
            },
```

Your containers should now reboot automatically - but check the different policies, to see which one applies best to your case.
