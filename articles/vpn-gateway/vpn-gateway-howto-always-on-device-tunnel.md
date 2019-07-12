---
title: Her zaman açık VPN tüneli için VPN ağ geçidi yapılandırma
description: Her zaman açık VPN tüneli için VPN ağ geçidi yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 07/09/2019
ms.author: cherylmc
ms.openlocfilehash: 81822297dcf9370fc8ce7f7ce0285689c31606ce
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695765"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Always On VPN cihazı tüneli yapılandırma

Windows 10 sanal özel ağ (VPN) istemcinin yeni özelliklerin bir VPN bağlantısı bakımını yapma olanağı biridir. Always On etkin VPN profilini otomatik olarak bağlanmasına ve Tetikleyicileri temel alan bağlı kalmasını sağlayan bir özelliktir Windows 10 — yani, kullanıcı oturum açma, ağ durumu değişikliği veya etkin cihaz ekranı.

Azure sanal ağ geçitleri Windows 10 Always On ile azure'a cihaz tüneller yanı sıra kalıcı kullanıcı tünel oluşturmak için kullanılabilir. Bu makalede her zaman açık VPN cihaz tüneli yapılandırmanıza yardımcı olur.

Her zaman açık VPN bağlantıları iki tünel türlerini içerir:

* **Cihaz tünel** kullanıcılar cihazda oturum önce belirtilen VPN sunucularına bağlanır. Oturum açma öncesi bağlantı senaryoları ve cihaz yönetim amacıyla cihaz tünel kullanın.

* **Kullanıcı tüneli** yalnızca bir kullanıcı oturum açma cihaz sonra bağlanır. Kullanıcı tüneli üzerinden VPN sunucuları kuruluş kaynaklarına erişmesine olanak tanır.

Hem cihaz tünel hem de kullanıcı tünel VPN profillerini bağımsız olarak çalışır. Bunlar aynı anda bağlanabilir ve farklı kimlik doğrulama yöntemleri ve diğer VPN yapılandırma ayarlarına uygun olarak kullanabilirsiniz.

## <a name="1-configure-the-gateway"></a>1. Ağ geçidini yapılandırma

Ikev2 ve sertifika tabanlı kimlik doğrulaması kullanan VPN ağ geçidini yapılandırma [noktadan siteye makale](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="2-configure-the-user-tunnel"></a>2. Kullanıcı tüneli yapılandırma

1. Bu konuda gösterildiği gibi Windows 10 istemci üzerinde istemci sertifikalarını yükleme [noktadan siteye VPN istemci makale](point-to-site-how-to-vpn-client-install-azure-cert.md). Sertifika geçerli kullanıcı Store olması gerekiyor
2. PowerShell, SCCM veya Intune aracılığıyla her zaman açık VPN istemcisi yapılandırma [bu yönergeleri](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections).

## <a name="3-configure-the-device-tunnel"></a>3. Cihaz tünel yapılandırma

Bir cihaz tüneli başarıyla oluşturmak için aşağıdaki gereksinimler karşılanmalıdır:

* Cihazın Windows 10 Enterprise veya eğitim sürüm 1709 veya sonraki sürümünü çalıştıran bir etki alanına katılan bilgisayarın olması gerekir.
* Tünel yalnızca Windows yerleşik VPN çözümü için yapılandırılabilir ve Ikev2 bilgisayar sertifikası kimlik doğrulamasını kullanarak kurulur. 
* Cihaz başına yalnızca bir cihaz tünel yapılandırılabilir.

1. Bu konuda gösterildiği gibi Windows 10 istemci üzerinde istemci sertifikalarını yükleme [noktadan siteye VPN istemci makale](point-to-site-how-to-vpn-client-install-azure-cert.md). Sertifika, yerel makine deposuna olması gerekiyor.
1. Kullanım [bu yönergeleri](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration) VPN profili oluşturma ve yerel sistem hesabı bağlamında cihaz tüneli yapılandırın.

### <a name="configuration-example-for-device-tunnel"></a>Cihaz tünel için yapılandırma örneği

Sanal ağ geçidi yapılandırılmış ve Windows 10 istemci üzerindeki yerel makine deposunda istemci sertifikası yüklü sonra bir istemci cihaz tünel yapılandırmak için aşağıdaki örnekleri kullanın.

1. Aşağıdaki metni kopyalayın ve kaydedileceği ***devicecert.ps1***.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Aşağıdaki metni kopyalayın ve kaydedileceği ***VPNProfile.xml*** aynı klasörde **devicecert.ps1**. Aşağıdaki metni ortamınızla eşleşecek şekilde düzenleyin.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
   </Authentication>  
   <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
    <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
   <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
     </NativeProfile> 
     <!-- use host routes(/32) to prevent routing conflicts -->  
     <Route>  
   <Address>192.168.3.5</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
     <Route>  
   <Address>192.168.3.4</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. İndirme **PsExec** gelen [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) ve dosyaları ayıklayın **C:\PSTools**.
1. Bir yönetici komut isteminden çalıştırarak PowerShell'i başlatın:

   ```
   C:\PsTools\PsExec.exe Powershell for 32-bit Windows
   C:\PsTools\PsExec64.exe Powershell for 64-bit Windows
   ```

   ![PowerShell](./media/vpn-gateway-howto-always-on-device-tunnel/powershell.png)
1. PowerShell'de, klasöre geçin burada **devicecert.ps1** ve **VPNProfile.xml** bulunur ve aşağıdaki komutu çalıştırın:

   ```powershell
   C:\> .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-device-tunnel/machinecerttest.png)
1. Çalıştırma **rasphone**.

   ![Rasphone](./media/vpn-gateway-howto-always-on-device-tunnel/rasphone.png)
1. Aranacak **MachineCertTest** girişi ve tıklatın **Connect**.

   ![Bağlan](./media/vpn-gateway-howto-always-on-device-tunnel/connect.png)
1. Bağlantı başarılı olursa, bilgisayarı yeniden başlatın. Tünel otomatik olarak bağlanır.

## <a name="cleanup"></a>Temizleme

Profilini kaldırmak için aşağıdaki komutu çalıştırın:

![Temizleme](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme için bkz: [Azure noktadan siteye bağlantı sorunları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
