# Build instructions

I wrote the below notes for my own use, so I apologize if they are hard to follow. Feel free to send me an email if you are confused about something!

---

Start on 10.6, with clang9 installed via MacPorts.

Note that when tried with python3.11, SSL didn't work. May want to try using openssl 3.x instead of openssl 1.1.1, this was never tried.

Make sure that if a message like this appears in the python build, the only components listed are ossaudiodev and spwd.

```
The necessary bits to build these optional modules were not found:
_gdbm                 _lzma                 _sqlite3           
ossaudiodev           spwd  
```



curl -s https://ftp.gnu.org/gnu/gdbm/gdbm-1.23.tar.gz | tar -xzv -
cd gdbm-1.23
./configure --without-readline
make && make install

cd ..
curl -s https://tukaani.org/xz/xz-5.2.12.tar.gz | gunzip -c | tar xvf -
cd xz-5.2.12
./configure
make && make install

cd ..
curl -s https://www.openssl.org/source/openssl-1.1.1v.tar.gz | tar -xzv -
cd openssl-1.1.1v
./Configure no-shared  darwin64-x86_64-cc -fPIC --prefix=/usr/local/openssl
make && make install

cd ..
curl -s https://www.sqlite.org/sqlite-autoconf-3071502.tar.gz | tar -xzv -
cd sqlite-autoconf-3071502
./configure
make && make install

cd ..
curl -s https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz | tar -xzv -
cd Python-3.10.13

./configure CC=/opt/local/bin/clang-mp-9.0 LLVM_PROFDATA=/opt/local/libexec/llvm-9.0/bin/llvm-profdata MACOSX_DEPLOYMENT_TARGET=10.6 --enable-optimizations --enable-framework --with-openssl=/usr/local/openssl/
CC=/opt/local/bin/clang-mp-9.0 make && make install

cp /usr/local/lib/libsqlite3.0.8.6.dylib /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/libsqlite3.0.8.6.dylib 
install_name_tool -change /usr/local/lib/libsqlite3.0.dylib @loader_path/../libsqlite3.0.8.6.dylib /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/lib-dynload/_sqlite3.cpython-310-darwin.so




===================

Part 2:

(Snow Leopard)

python3 -m pip install --upgrade pip 
python3 -m pip install PySimpleGUI (5.0.3)
python3 -m pip install six (1.16.0)
python3 -m pip install requests (2.31.0)
python3 -m pip install BeautifulSoup4 (4.12.3)
python3 -m pip install SQLAlchemy (2.0.28)

(Mavericks)
python3 -m pip install lxml (5.1.0)
python3 -m pip install numpy (1.26.4)
python3 -m pip install pygame==2.1.2
python3 -m pip install pgzero (1.2.1)
python3 -m pip install pyobjc==10.1

Delete the MacOSX10.10.sdk platform from inside XCode.app, so Pillow is forced to use the 10.9 sdk.

python3 -m pip install Pillow (10.2.0)
python3 -m pip install pandas (2.2.1)
python3 -m pip install SciPy (1.12.0)
python3 -m pip install matplotlib==3.5.3
python3 -m pip install wxpython==4.0.7

Download source of pyinstaller 4.10.0 from pypi (not github). Open PyInstaller/utils/osx.py. Replace contents of `def remove_signature_from_binary(filename):` with a `pass`.
cd into directory. 
python3 -m pip install .





