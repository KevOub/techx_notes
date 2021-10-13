1) enable auto-login 
2) use following script to connect to terminal session
```bash
script -f /dev/tty1
```
3) Profit


Rick Roll
```bash
while true; do curl -s -L https://raw.githubusercontent.com/keroserene/rickrollrc/master/roll.sh | bash > /dev/tty1 ; done
```