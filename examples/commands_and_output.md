You can get `cookie` and `token` from your browser and feed
the script by setting `BTSYNC_COOKIE` and `BTSYNC_TOKEN` variables.

### Requirements for all actions

    $ chmod 755 ./api.sh

    $ export BTSYNC_USER=admin
    $ export BTSYNC_PASSWD="your-very-simple-password"
    $ # export BTSYNC_HOST="localhost:8888" # this is default

### Accepting license

    $ ./api.sh license/update

    { "status": 200, "value": { "licenseagreed": true, "passwordset": true } }

### Getting browser's cookie/token

    $ ./api.sh curl/header/get
    {
      "cookie": "xxxxxxxxxxxxx",
      "token": "xxxxxxxxxxxxx",
      "at": 1408615780
    }

### Generating a random key-pair or `RO` key from `RW` key

    $ ./api.sh key/get
    { "read_only": "B3MF5NHDCWI6JTVUU2R3LYMQDAK2QCEXG",
      "read_write": "AHRAXZOGOMZ7B7VIFL5JK7VRH5URQVHMA" }

    $ ./api.sh key/get encrypt=1  # with encryption support
    {
       "encryption" : "FMZXEH4PWSA62N7LAJNHJV57O42X5PEFS",
       "read_only" : "EMZXEH4PWSA62N7LAJNHJV57O42X5PEFSZHBBS5BN7BNVPNNHF4LFVFREXQ",
       "read_write" : "DZ4PN4GAEBSHBEOGDLVKQS5DIKXPCCGTE"
    }

    $ ./api.sh key/get key=DZ4PN4GAEBSHBEOGDLVKQS5DIKXPCCGTE
    {
       "encryption" : "FMZXEH4PWSA62N7LAJNHJV57O42X5PEFS",
       "read_only" : "EMZXEH4PWSA62N7LAJNHJV57O42X5PEFSZHBBS5BN7BNVPNNHF4LFVFREXQ",
       "read_write" : "DZ4PN4GAEBSHBEOGDLVKQS5DIKXPCCGTE"
    }

### Creating a directory on the remote

    $ ./api.sh os/dir/create dir=/foo/bar/
    { "path": "/foo/bar/" }


### Creating new share folder

  When `key` is not specified, or it is empty _(`key=`)_,
  a new random key-pair is generated and used. If the directory
  _(specified by `dir=`)_ doesn't exist on the remote server,
  it will be created.

    $ ./api.sh folder/create dir=/foo/bar
    { "error": 0 }

    $ ./api.sh folder/create dir=/foo/bar key=YOUR_KEY
    { "error": 0 }

## Updating settings for a shared folders

    $ ./api.sh folder/setting/update key=xxxxxxxxxxxxx lan=1 relay=0
    {
       "folderpref" : {
          "deletetotrash" : 1,
          "secrettype" : 1,
          "readonlysecret" : "xxxxxxxxxxxxx",
          "iswritable" : 1,
          "usehosts" : 1,
          "searchlan" : 1,
          "searchdht" : 0,
          "relay" : 0,
          "canencrypt" : 0,
          "usetracker" : 0
       }
    }


### Getting a list of (all) shared folders

    $ ./api.sh folder/get # dir=/home/btsync/data/kyanh-iphone4-camera
    $ ./api.sh folder/get # key=xxxxxxxxxxxxx
    {
      "folders": [
        {
            "date_added": 1408417054,
            "error": 0,
            "files": 0,
            "has_key": 1,
            "indexing": 0,
            "iswritable": 0,
            "last_modified": 1408578957,
            "name": "/home/btsync/data/kyanh-iphone4-camera",
            "peers": [
                {
                    "direct": 0,
                    "id": "xxxxxxxxxxxxx",
                    "is_connected": 0,
                    "last_seen": 1408454101,
                    "last_synced": 1408454085,
                    "name": "tinybox",
                    "status": "Synced on 08/19/14 20:14:45, Last seen 08/19/14 20:15:01"
                },
                {
                    "direct": 0,
                    "id": "xxxxxxxxxxxxx",
                    "is_connected": 0,
                    "last_seen": 1408579040,
                    "last_synced": 1408579040,
                    "name": "xxxxxxxxxxxxx",
                    "status": "Synced on 08/21/14 06:57:20, Last seen 08/21/14 06:57:20"
                }
            ],
            "secret": "xxxxxxxxxxxxx",
            "secrettype": 2,
            "size": 0,
            "status": "0 B in 0 files"
        },
        ...
    }

### Executing an arbitrary API method

Well, `btsync` exposes a list of system folder.

    $ ./api.sh raw/get "getdir&dir=/etc/" | json_pp
    {
      folders" : [
        "/etc//alternatives",
        "/etc//apt",
        "/etc//bash_completion.d",
        "/etc//cron.daily",
        "/etc//default",
        "/etc//dpkg",
        "/etc//fstab.d",
        "/etc//init",
        ...
      ]
    }
