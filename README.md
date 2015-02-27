# Auto-ubuntu-background-change
Unbunt System Automatic change background image fron bing server

#Create sh file in your user folder

$> vim /home/rajwebsoft/background/rajsoft.sh

$> chmod 777 /home/rajwebsoft/background/rajsoft.sh

#Add the Content in rajsoft.sh file

#!/bin/bash
newdt="$(date +'%d-%m-%Y')"
values=($(curl -s 'http://www.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=en-US' | python -c 'import sys, json; print json.load(sys.stdin)["images"][0]["url"]'))

wget "http://www.bing.com$values" -O "/home/rajwebsoft/background/$newdt.jpg"

PID=$(pgrep gnome-session)
export DBUS_SESSION_BUS_ADDRESS=$(grep -z DBUS_SESSION_BUS_ADDRESS /proc/$PID/environ|cut -d= -f2-)

sleep 5s

gsettings set org.gnome.desktop.background picture-uri "file:///home/rajwebsoft/background/$newdt.jpg"

# Set the Cron time

$> crontab -e
34 13 * * * bash /home/rajwebsoft/background/rajsoft.sh >> /home/rajwebsoft/background/logcheck.txt 2>&1

Congratulation your background is updated automaticaly per day 1:34PM because bing is updated background image 1:30PM

