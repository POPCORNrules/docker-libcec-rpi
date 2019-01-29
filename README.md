This Docker image contains the latest version of libcec compiled with Raspberry Pi support.
For use with https://balena.io

# Usage

clone repo
``` shell
git clone https://gitea.popcornrules.com/POPCORNrules/tv-power.git
cd tv-power
```

copy to your project
``` shell
cp -r * <Your Repository>
```

edit docker-compose.yml to include your project
``` yaml
version: '2'
services:
  tv-power:
    build: ./tv-power
    privileged: true
    restart: unless-stopped
+  <your service>:
+  	build: .
+  	privilaged: true
+  	restart: always
+  	...
```

edit crontab to set on/off times
``` crontab
# Turn TV on at 7:30 am
30  7 * * 1-5 echo on 0 | /opt/libcec/bin/cec-client -s -d 1 >> /dev/null 2>&1
# Turn TV off at 5:30 pm
30  17 * * 1-5 echo standby 0 | /opt/libcec/bin/cec-client -s -d 1 >> /dev/null 2>&1
# Check TV power every 10 Min
*/10 * * * * echo pow 0 | /opt/libcec/bin/cec-client -s -d 1
```

push to balena
``` shell
git push balena master
```
