curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt -y install nodejs make gcc g++
sudo npm install -g npm@latest
npm -v
npm install
sudo npm run build:js
