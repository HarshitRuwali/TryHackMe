Install Volatility. 
ref: https://seanthegeek.net/1172/how-to-install-volatility-2-and-volatility-3-on-debian-ubuntu-or-kali-linux/

Volatility v2 

```bash
sudo apt install -y python2 python2.7-dev libpython2-dev
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
sudo python2 get-pip.py
sudo python2 -m pip install -U setuptools wheel
```

```bash
python2 -m pip install -U distorm3 yara pycrypto pillow openpyxl ujson pytz ipython capstone
sudo python2 -m pip install yara
sudo ln -s /usr/local/lib/python2.7/dist-packages/usr/lib/libyara.so /usr/lib/libyara.so
python2 -m pip install -U git+https://github.com/volatilityfoundation/volatility.git
```

![[Pasted image 20220521002035.png]]


![[Pasted image 20220521002210.png]]

![[Pasted image 20220521002415.png]]

![[Pasted image 20220521002612.png]]

![[Pasted image 20220521125543.png]]

![[Pasted image 20220521123732.png]]

![[Pasted image 20220521123757.png]]

![[Pasted image 20220521123930.png]]

![[Pasted image 20220521124556.png]]


![[Pasted image 20220521125337.png]]