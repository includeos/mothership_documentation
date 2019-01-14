Installing GUI client
-------------------------
::

    $ git clone git@github.com:includeos/mothership_client.git # f.ex. in your HOME directory

MacOS::

    $ brew install node
    $ brew install npm
    $ npm install -g webpack@2.6.1
    $ npm install
    $ ./copyfiles.sh

Ubuntu::

    $ sudo apt install npm
    $ sudo npm install -g n # webpack needs the node command as opposed to nodejs. The npm n tool should fix that.
    $ sudo n stable
    $ sudo npm install -g webpack@2.6.1
    $ sudo npm install
    $ ./copyfiles.sh
