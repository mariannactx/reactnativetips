1. Download the files inside the directory /executables
2. Copy them to /usr/local/bin
```shell
cp [filename] /usr/local/bin/[filename]
```
3. Make sure they have executable permissions
```shell
ls -g /usr/local/bin
-rw-r--r-- 1 owner      68 mai 18 20:58 [filename]
sudo chmod a+x [filename]
-rwxr-xr-x 1 owner      68 mai 18 20:58 [filename]
```
