# escape=`
FROM mcr.microsoft.com/windows/servercore:ltsc2022 as msvc2022-build

SHELL ["cmd", "/S", "/C"]

RUN curl --location --output vs_buildtools.exe https://aka.ms/vs/17/release/vs_buildtools.exe `
 && (start /w vs_buildtools.exe --quiet --wait --norestart --nocache `
     --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2022\BuildTools" `
     --add Microsoft.VisualStudio.Workload.VCTools `
     --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 `
     --add Microsoft.VisualStudio.Component.Windows11SDK.22621 `
     || IF "%ERRORLEVEL%"=="3010" EXIT 0)

FROM msvc2022-build as dependency

SHELL [ "powershell.exe" ]

RUN Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
RUN choco install -y git cmake grep

FROM dependency as config
SHELL [ "C:\\Program Files\\Git\\bin\\bash.exe" ]

ENV LC_ALL="en_US.UTF-8"

CMD [ "C:\\Program Files\\Git\\bin\\bash.exe" ]