#Install Docker-Sync on Windows 10 for Symfony 4

## Setup on Windows 10

- Update Windows to latest version < 18.03
- Install Docker for Windows
- Open the Docker for Windows settings and check Expose daemon on tcp://localhost:2375 without TLS
- Enable WSL Open the Windows Control Panel, Programs and Features, click on the left on Turn Windows features on or off and check Windows Subsystem for Linux near the bottom.
- Install a distro Open the Microsoft Store and search for ‘linux’. Choose Debian
- Launch and update The distro you choose is now an ‘app’ on your system.
- Open the start menu and launch it, then follow the on screen instructions in order to complete the installation.
- When you have a fully working shell, update the system.

```
sudo apt update
sudo apt upgrade
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
sudo ln -s "/mnt/c/Program Files/Docker/Docker/resources/bin/docker.exe" /usr/local/bin/docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s "/mnt/c/Program Files/Docker/Docker/resources/bin/docker-compose.exe" /usr/local/bin/docker-compose
sudo usermod -aG docker $USER
sudo nano /etc/wsl.conf
```

```text
[automount]
root = /
options = "metadata"
```

```
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
```

- Restart Machine


```shell script
sudo apt-get install ruby ruby-dev
sudo gem install docker-sync
sudo apt-get install build-essential
sudo apt-get install make
wget http://caml.inria.fr/pub/distrib/ocaml-4.08/ocaml-4.08.1.tar.gz
tar xvf ocaml-4.08.1.tar.gz
cd ocaml-4.08.1
./configure
make world
make opt
umask 022
sudo make install
sudo make clean
sudp apt-get install wget
cd ..
wget https://github.com/bcpierce00/unison/archive/v2.51.2.tar.gz
tar xvf v2.51.2.tar.gz
cd unison-2.51.2
# The implementation src/system.ml does not match the interface system.cmi:curl and needs to be patched
curl https://github.com/bcpierce00/unison/commit/23fa1292.diff?full_index=1 -o patch.diff
git apply patch.diff
make UISTYLE=text
sudo cp src/unison /usr/local/bin/unison
sudo cp src/unison-fsmonitor /usr/local/bin/unison-fsmonitor
```

To use go into Debian and run

```shell script
docker-sync-stack clean
docker-sync-stack start
docker-compose -f docker-compose.yml -f docker-compose-dev.yml up
```

This will allow you to see the initial build, if it is OK then you can run to following to run in a detatched state

```shell script
docker-sync clean
docker-sync start
```

### Useful Commands

```shell script
docker-compose exec php composer install

docker-compose -f docker-compose.yml -f docker-compose-dev.yml up

```

### ToDo and Troubleshooting

If multiple projects then change the sync names in `docker-sync.yml` and `docker-compose.yml`