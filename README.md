# Axolotl is a crossplattform [Signal](https://www.signal.org) client

## For the Ubuntu Phone and more

Axolotl is a complete Signal client, it allows you to create a Signal account and have discussions with your contacts.
Unlike the desktop Signal client, **Axolotl is completely autonomous** and doesn't require you to have created an account with the official Signal application.

It is built upon the [Go textsecure package](https://github.com/nanu-c/textsecure/) and a vuejs app that runs in a electron/qml WebEngineView container.

To use it from your Ubuntu Touch device, simply install it from the open store:  
[![OpenStore](https://open-store.io/badges/en_US.png)](https://open-store.io/app/textsecure.nanuc)

Axolotl is also available as a snap package, to install it on Ubuntu desktop:  
[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/axolotl)

[![Snap Status](https://build.snapcraft.io/badge/nanu-c/axolotl.svg)](https://build.snapcraft.io/user/nanu-c/axolotl)

What works
-----------

 * Phone registration
 * Contact discovery
 * Direct messages
 * Group messages *mostly*
 * Photo, video, audio and contact attachments in both direct and group mode
 * Preview for photo and audio attachments
 * Storing conversations
 * Encrypted message store
 * Desktop client provisioning/syncing *partially*

What is missing
---------------

 * Push notifications
 * Most settings that are available in the Android app
 * Encrypted phone calls

There are still bugs and UI/UX quirks.

Installation of development environment
------------
* Install [Golang](https://golang.org/doc/install)
* Install node js
* Add gopath to ~/.bashrc https://github.com/golang/go/wiki/SettingGOPATH
* install dependencies `sudo apt install mercurial bzr`
* Check out this git `go get -d github.com/nanu-c/axolotl`
* `cd $(go env GOPATH)/src/github.com/nanu-c/axolotl`
* get go dependencies `go get -d ...`
* install axolotl-web dependencies: `cd axolotl-web&&npm install`

Translations
------------
Axolotl uses gettext for translations. Use the po files in `/po/` for translations.
For testing set up  development enviroment and run
* `npm run translate-extract` extracting the language strings. This updates only the pot file
* `npm run translate-update` for updating all the translation files
* `npm run translate-compile` for updating the json file used by axolotl-web. Without that you don't see any results
* or `npm run translate` for all of the 3 commands at the same time. This should be run befor commiting any changes on axolotl-web


Run development
------------
* `cd $(go env GOPATH)/src/github.com/nanu-c/axolotl`
* `go run .`
* in a new terminal `cd axolotl-web&&npm run serve`
* point a browser to the link printed in the terminal  like `http://localhost:8080`

Build Axolotl for all arches with clickable and snap
------------
requires clickable and snapcraft to be installed
```
go clean

echo "Build ut linux armhf"
clickable clean build

echo "Build linux amd64 snap"
mkdir -p build/linux-amd64-snap
snapcraft clean axolotl
snapcraft
mv *.snap build/linux-amd64-snap/

echo "Build linux amd64"
mkdir -p build/linux-amd64/axolotl-web
env GOOS=linux GOARCH=amd64 go build -o build/linux-amd64/axolotl .
cp axolotl-web/dist build/windows-amd64/axolotl-web -r

echo "Build linux arm5"
mkdir -p build/linux-arm5/axolotl-web
env GOOS=linux GOARCH=arm GOARM=5 go build -o build/linux-arm5/axolotl .
cp axolotl-web/dist build/linux-arm5/axolotl-web -r

echo "Build linux arm7"
mkdir -p build/linux-arm7/axolotl-web
env GOOS=linux GOARCH=arm GOARM=7 go build -o build/linux-arm7/axolotl .
cp axolotl-web/dist build/linux-arm7/axolotl-web -r

echo "Build linux arm64"
mkdir -p build/linux-arm64/axolotl-web
env GOOS=linux GOARCH=arm64 go build -o build/linux-arm64/axolotl .
cp axolotl-web/dist build/linux-arm64/axolotl-web -r

echo "Build windows amd64"
mkdir -p build/windows-amd64/axolotl-web
env GOOS=windows GOARCH=amd64 go build -ldflags -H=windowsgui -o build/windows-amd64/axolotl.exe .
cp axolotl-web/dist build/windows-amd64/axolotl-web -r

echo "Build windows 386"
mkdir -p build/windows-386/axolotl-web
env GOOS=windows GOARCH=386 go build -ldflags -H=windowsgui -o build/windows-386/axolotl.exe .
cp axolotl-web/dist build/windows-amd64/axolotl-web -r

echo "Build darwin amd64"
mkdir -p build/darwin-amd64/axolotl-web
env GOOS=darwin GOARCH=amd64 go build -o build/darwin-amd64/axolotl .
cp axolotl-web/dist build/darwin-amd64/axolotl-web -r
```

Installation on UT
------------

***If you want to use the current stable version, simply install it from the OpenStore***

The build-system is now integrated in the `clickable` Version 3.2.0.
* primary steps from installation
* Get [clickable](https://github.com/bhdouglass/clickable#install)
* Run `clickable`, this also transfers the click package to the Ubuntu Touch Phone
* Run `clickable launch logs` to start signal and watch the log


Run flags
-----------

* `-sys` for either `lorca`-> native chromium (has to be installed), `ut` -> runs in the ut enviroment, `me` -> qmlscene, or without that flag it runs electron
* `-eDebug` show developer console in electron mode

Contributing
-----------

Please fill issues here on github https://github.com/nanu-c/axolotl/issues

Migrating from `janimo/axolotl`
--------------------------------------
1. Download and install the app from the OpenStore; do not launch the app!
2. Copy the directory `/home/phablet/.local/share/textsecure.jani/.storage` to
   `/home/phablet/.local/share/textsecure.nanuc/.storage`
3. Copy the file `/home/phablet/.config/textsecure.jani/config.yml` to
   `/home/phablet/.config/textsecure.nanuc/config.yml`.
   Edit the copied file by changing `storageDir: /home/phablet/.local/share/textsecure.nanuc/.storage` (not strictly required: also
   update `userAgent: TextSecure 0.3.18 for Ubuntu Phone` to reflect the current version).
4. _Not strictly required._
   Copy your conversation history by copying the file `/home/phablet/.local/share/textsecure.jani/db/db.sql` to
   `/home/phablet/.local/share/textsecure.nanuc/db/db.sql`
5. _Not strictly required._
   Copy the attachments by copying the directory `/home/phablet/.local/share/textsecure.jani/attachments` to
   `/home/phablet/.local/share/textsecure.nanuc/attachments`.
   Download the `db.sql` to your computer and run `sqlite3 db.sql "UPDATE messages SET attachment = REPLACE(attachment,
   '/home/phablet/.local/share/textsecure.jani/attachments/', '/home/phablet/.local/share/textsecure.nanuc/attachments/') WHERE
   attachment LIKE '/home/phablet/.local/share/textsecure.jani/attachments/%';"`.
   Upload the now updated `db.sql` back to your phone.
6. **Remove the old app!**
   If you do not remove the old app and you send or receive new messages with the other app you, conflicts may occur.
