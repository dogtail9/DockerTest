FROM microsoft/windowsservercore:latest
ENV VERSION 1.13.8

SHELL ["powershell", "-command"]
RUN Invoke-WebRequest -Proxy "http://devproxy.shbmain.shb.biz:8088" -Uri http://nginx.org/download/nginx-$ENV:VERSION.zip -OutFile c:\nginx-$ENV:VERSION.zip; \
	Expand-Archive -Path C:\nginx-$ENV:VERSION.zip -DestinationPath C:\ -Force; \
	Remove-Item -Path c:\nginx-$ENV:VERSION.zip -Confirm:$False; \
	Rename-Item -Path nginx-$ENV:VERSION -NewName nginx

# Make sure that Docker always uses default DNS servers which hosted by Dockerd.exe
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenDefaultServers -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenUnreachableServers -Value 0 -Type DWord
	
# Shorten DNS cache times
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxCacheTtl -Value 30 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxNegativeCacheTtl -Value 30 -Type DWord

WORKDIR /nginx
EXPOSE 80
CMD ["nginx", "-g", "\"daemon off;\""]