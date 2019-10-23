# NordVPN

sudo -s

apt-get install gcc rng-tools make automake autoconf dh-autoreconf file patch perl dh-make debhelper devscripts gnupg lintian quilt libtool pkg-config libssl-dev liblzo2-dev libpam0g-dev libpkcs11-helper1-dev openssl liblz4-dev liblz4-tool net-tools iproute2

wget http://swupdate.openvpn.org/community/releases/openvpn-2.4.7.zip

unzip openvpn-2.4.7.zip

cd openvpn-2.4.7

wget https://raw.githubusercontent.com/Tunnelblick/Tunnelblick/master/third_party/sources/openvpn/openvpn-2.4.7/patches/02-tunnelblick-openvpn_xorpatch-a.diff 

wget https://raw.githubusercontent.com/Tunnelblick/Tunnelblick/master/third_party/sources/openvpn/openvpn-2.4.7/patches/03-tunnelblick-openvpn_xorpatch-b.diff 

wget https://raw.githubusercontent.com/Tunnelblick/Tunnelblick/master/third_party/sources/openvpn/openvpn-2.4.7/patches/04-tunnelblick-openvpn_xorpatch-c.diff 

wget https://raw.githubusercontent.com/Tunnelblick/Tunnelblick/master/third_party/sources/openvpn/openvpn-2.4.7/patches/05-tunnelblick-openvpn_xorpatch-d.diff 

wget https://raw.githubusercontent.com/Tunnelblick/Tunnelblick/master/third_party/sources/openvpn/openvpn-2.4.7/patches/06-tunnelblick-openvpn_xorpatch-e.diff 

git apply 02-tunnelblick-openvpn_xorpatch-a.diff

git apply 03-tunnelblick-openvpn_xorpatch-b.diff

git apply 04-tunnelblick-openvpn_xorpatch-c.diff

git apply 05-tunnelblick-openvpn_xorpatch-d.diff

git apply 06-tunnelblick-openvpn_xorpatch-e.diff 

Try this:
apt install autoconf dh-autoreconf

https://segmentfault.com/a/1190000009414135

autoreconf -i -v -f

./configure --prefix=/usr

make

make install

Change your DNS servers on Linux    
Share this answer
Follow these steps to change your DNS servers on linux:

1. Open the terminal (Ctrl + T)

2. Enter this command to become root:

su

3. After entering your root password run these commands:

rm -r /etc/resolv.conf
nano /etc/resolv.conf

4. When the text editor opens, type in these lines:

nameserver 103.86.96.100
nameserver 103.86.99.100

5. Now you have to close and save the file, you can do that by clicking Ctrl + X and pressing Y. Then continue typing in the terminal:

chattr +i /etc/resolv.conf
reboot now

You will now be using NordVPN DNS servers.
