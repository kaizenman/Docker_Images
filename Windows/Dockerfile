FROM mcr.microsoft.com/windows/servercore:ltsc2019

RUN powershell -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" \ 
    && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

RUN powershell -NoProfile -InputFormat None -Command \
    choco install git 7zip -y; \ 
    choco install visualcpp-build-tools --version 15.0.26228.20170424 -y; \
    choco install -y cmake --installargs 'ADD_CMAKE_TO_PATH="System"'; \
    Write-Host 'Waiting for Visual C++ Build Tools to finish'; \ 
    Wait-Process -Name vs_installer;

RUN powershell -NoProfile -InputFormat None -Command \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    (New-Object System.Net.WebClient).DownloadFile('https://github.com/kaizenman/install_opengl/archive/opengl_new.zip', 'install_opengl.zip'); \
    7z e .\install_opengl.zip;

RUN powershell -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command \
    .\install_opengl.ps1

WORKDIR C:\\projects

CMD powershell.exe