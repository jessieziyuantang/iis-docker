# escape=`

# Base image
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8 AS runtime

# Set the shell to PowerShell
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; Set-ExecutionPolicy Unrestricted -Force;"]

# Disable IIS auto start
RUN ["cmd", "/S", "/C", "sc", "config", "w3svc", "start=demand"]

# Install LogMonitor.exe
RUN md c:\LogMonitor
COPY Tools/LogMonitor/. c:/LogMonitor

# Update IIS config
RUN \
    # Enable ETW logging for Default Web Site on IIS    
    c:/windows/system32/inetsrv/appcmd.exe set config -section:system.applicationHost/sites /\"[name='Default Web Site'].logFile.logTargetW3C:\"File,ETW\"\" /commit:apphost ; \

# .... copy in your application ....

ENTRYPOINT ["C:\\LogMonitor\\LogMonitor.exe", "C:\\ServiceMonitor.exe", "w3svc"]