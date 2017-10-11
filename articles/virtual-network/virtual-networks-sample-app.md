---
title: "DMZ'ler ile kullanmak için örnek uygulama | Microsoft Docs"
description: "Trafik akışı senaryolarını sınamak için DMZ oluşturduktan sonra bu basit web uygulaması dağıtma"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 60340ab7-b82b-40e0-bd87-83e41fe4519c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 8506238e41c5d9dac8d76d729d4919b30a0528b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="sample-application-for-use-with-dmzs"></a>Örnek bir uygulama DMZ'ler ile kullanmak için
[Güvenlik sınırı en iyi yöntemler sayfasına dön][HOME]

Bu PowerShell komut dosyalarını yerel olarak yükleyin ve arka uç AppVM01 sunucusundan içerik ile ön uç IIS01 sunucusundan html sayfalarını görüntüleyen basit bir web uygulaması için IIS01 ve AppVM01 sunucularda çalıştırabilirsiniz.

Bu uygulama basit bir sınama ortamında pek çok DMZ örnekler sağlar ve nasıl değişiklikleri uç noktaları, Nsg'ler, UDR ve güvenlik duvarı kuralları trafik akışına etkileyebilir.

## <a name="firewall-rule-to-allow-icmp"></a>ICMP izin veren güvenlik duvarı kuralı
Bu basit bir PowerShell ifadesi ICMP (Ping) trafiğine izin vermek için bir Windows VM üzerinde çalıştırılabilir. Windows güvenlik duvarı (çoğu Linux distro'lar ICMP varsayılan olarak etkindir) üzerinden iletmek için daha kolay test ve ping Protokolü vererek sorun giderme için bu Güvenlik Duvarı'nı güncelleştirme sağlar.

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

Aşağıdaki komut dosyaları kullanırsanız, bu güvenlik duvarı kuralı ilk ifade ektir.

## <a name="iis01---web-application-installation-script"></a>IIS01 - Web uygulama yükleme betiği
Bu komut dosyası aşağıdakileri yapar:

1. Açık IMCPv4 (Ping) daha kolay test etmek için yerel sunucunun windows güvenlik duvarı
2. IIS ve .net yükleme Framework v4.5
3. Bir ASP.NET web sayfası ve Web.config dosyası oluşturma
4. Dosya erişimi kolaylaştırmak için varsayılan uygulama havuzunu Değiştir
5. Yönetici hesabınız ve parolanız için anonim kullanıcı Ayarla

RDP IIS01 alırken bu PowerShell komut dosyasını yerel olarak çalıştırmanız gerekir.

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Clasic Pipeline to remote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a>AppVM01 - dosya sunucusu yükleme betiği
Bu komut dosyası basit bu uygulama için arka uç ayarlar. Bu komut dosyası aşağıdakileri yapar:

1. Açık IMCPv4 (Ping) daha kolay test etmek için Güvenlik Duvarı'nda
2. Web sitesi için bir dizin oluşturun
3. Uzaktan Erişim tarafından web sayfasını olması için bir metin dosyası oluşturun
4. Anonim erişime izin vermek için dosya ve dizin izinlerini ayarlayın
5. IE Artırılmış Güvenlik bu sunucudan daha kolay gezinme izin vermek için devre dışı bırakma 

> [!IMPORTANT]
> **En iyi uygulaması**: hiçbir zaman bir üretim sunucuda IE Artırılmış Güvenlik devre dışı kapatın, artı, genellikle bir üretim sunucusundan Web'de gezinmek için kötü bir fikirdir. Ayrıca, dosya paylaşımları anonim erişim için açma tamamlandı ancak kötü bir fikir burada kolaylık sağlamak için geçerlidir.
> 
> 

RDP AppVM01 alırken bu PowerShell komut dosyasını yerel olarak çalıştırmanız gerekir. PowerShell aktarılmadığı sağlamak için yönetici olarak çalıştırmak için gereklidir.

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS sunucusu yükleme betiği
DNS sunucusu kurmak için bu örnek uygulama dahil betik yok. Güvenlik duvarı kuralları, NSG veya UDR sınama DNS trafiğinin eklenmesi gerekiyorsa, DNS01 sunucusunun elle ayarlanması gerekir. Ağ yapılandırma xml dosyasını ve Resource Manager şablonu hem örnekleri için birincil DNS sunucusu ve düzeyi 3'ü yedekleme DNS sunucusu tarafından barındırılan ortak DNS sunucusu olarak DNS01 içerir. Düzey 3 DNS sunucusu yerel olmayan trafik için kullanılan gerçek bir DNS sunucusunun ve DNS01 ile ayarlanmadı, DNS oluşacak yerel ağ.

## <a name="next-steps"></a>Sonraki adımlar
* Bir IIS sunucusunda IIS01 komut dosyasını çalıştır
* AppVM01 üzerinde dosya sunucusu komut dosyasını çalıştır
* IIS01 yapınızın doğrulamak için ortak IP göz atın

<!--Link References-->
[HOME]: ../best-practices-network-security.md
