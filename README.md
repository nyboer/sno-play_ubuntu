#Sno-Play
There are two builds of snowboy in this project. One is for running under Nodejs and the other is
for Python. These are cribbed and modified from the source more or less, but designed to get you up
and running with keyword detection in these two environments.

##Snowboy and Node
Make sure you have node version 6.9.1 - `node -v` will tell you. 

If you don't, you can uninstall node either with `apt-get remove node` or
```
sudo rm -rf /usr/bin/nodejs /usr/bin/npm
```
(that is, `which node` and `which npm`, then just `sudo rm` those paths).

Now you are ready for [node version manager](https://github.com/creationix/nvm) as a nice way to manage node versions. 

Install `nvm` with:

```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install node 6
```

Navigate to the `sno-node` directory, and `npm install`. This should do most of the work.
Run `node microphone.js`.
If you get the error that there is no Play() function, as I did, there is a solution. In
theory, the install process should also install the `play` module, but this did not work
for me for whatever reason. Rather than get to the root of the problem, I solved it with
the following, performed in the `sno-node` directory:
```
git clone https://github.com/Marak/play.js.git
rm -rf node_modules/play
cp -r play.js/ node_modules/play/
```
That is, manually install the play module by cloning the current source for the node play
module and put it in the node_modules directory.

##Snowboy and Python

Current binaries in this directory are for Ubuntu 14.04. If you are running on another system, 
you'll need to rebuild per below instructions.

Navigate to the `sno-py` directory and run `python demo.py`.
If it doesn't work, you'll need to install and build a bunch of stuff.

```
sudo apt-get install swig3.0 python-pyaudio python3-pyaudio sox
pip install pyaudio
sudo apt-get install libatlas-base-dev
```
make sure you have swig 3.0.7 or above with
```
swig -v
swig3.0 -v
```
if not, you'll need to compile swig, or figure out a way to ensure apt-get installs the right one.
Here's the compile instructions:
```
wget http://prdownloads.sourceforge.net/swig/swig-3.0.10.tar.gz
tar -xvf swig-3.0.10.tar.gz
cd swig*
./configure
make
sudo make install
```
Now clone the repo and build the snowboy python module, then copy it to the sno-py directory:
```
git clone https://github.com/Kitt-AI/snowboy.git
cd snowboy/swig/Python
make
cp _snowboydetect.so snowboydetect.py ~/Documents/sno-py/
```
If make delivers an error, you may need to change the `SWIG` declaration in the Makefile.
Change `SWIG=swig` to `SWIG=swig`. You can also find out if you need to do this if `which swig`
turns up empty but `which swig3.0` returns a path.
