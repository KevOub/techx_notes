The *system* hates me. Why?
- The Ubuntu docker image does not work well. It runs into a DNS issue *ewwww*
	- Cannot `apt-get update`
- So what is the next step? Use a non-Ubuntu docker image. Use an ARM one! Turns out that causes issues with -- GPG keys?
- Mmm what happens when I use a older and non-broken Docker image?
 ```
opt/cross/bin/armv7l-linux-musleabihf-ar: 1: /opt/cross/bin/armv7l-linux-musleabihf-ar: Syntax error: "(" unexpected
make: *** [libz.a] Error 2
```