# TotalSistemasEx
Here I've created with the help of community a program who transfers the File Transfer Protocol To a specific Directory on Windows.
# Import Module
import ftplib
import os

# Fill Required Information
HOSTNAME =     input('Insira o Host:')
USERNAME =     input('Insira o Usuário:')
PASSWORD =     input('Insira a senha:')
FtpDirectory = input ('Insira o Local do Diretório:')
DowDirectory = input('Insira a Pasta Desejada:')

# Connect FTP Server
ftp = ftplib.FTP(HOSTNAME, USERNAME, PASSWORD)
ftp.cwd(FtpDirectory)
# force UTF-8 encoding
ftp.encoding = "utf-8"

# Enter File Extension
filematch = '*.Total'
import time

downloaded = []

while True:  # runs forever
    skipped = 0

    for filename in ftp.nlst(filematch):
        if  filename not in downloaded:
            fhandle = open(os.path.join(DowDirectory,filename), 'wb')
            print('Getting ' + filename)
            ftp.retrbinary('RETR ' + filename, fhandle.write)
            fhandle.close()
            downloaded.append(filename)
        else:
            skipped += 1

    print('Downloaded %s, skipped %d files' % (downloaded[-1], skipped))
    time.sleep(24 * 60 * 60)  # sleep 24 hours after finishing last download

ftp.quit()
