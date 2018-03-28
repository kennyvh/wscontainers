# escape=`
FROM sixeyed/msbuild:4.7.1-webdeploy AS build-agent

WORKDIR C:\src

COPY \packages.config .
RUN nuget restore packages.config -PackagesDirectory ..\packages

COPY . C:\src
RUN msbuild WebApp1.csproj /p:OutputPath=C:\out /p:DeployOnBuild=true `
      /p:VSToolsPath=C:\MSBuild.Microsoft.VisualStudio.Web.targets.14.0.0.3\tools\VSToolsPath

FROM microsoft/windowsservercore:1709
SHELL ["powershell"]

WORKDIR C:\web-app
COPY --from=build-agent C:\out .
RUN Install-WindowsFeature Web-Server; `
    Remove-Website -Name 'Default Web Site'; `
    New-Website -Name 'web-app' -Port 80 -PhysicalPath 'c:\web-app'

