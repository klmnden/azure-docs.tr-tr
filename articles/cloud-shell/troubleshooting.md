---
title: "Azure bulut Kabuğu (Önizleme) sorunlarını giderme | Microsoft Docs"
description: "Azure bulut Kabuk sorunlarını giderme"
services: azure
documentationcenter: 
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/2/2017
ms.author: damaerte
ms.openlocfilehash: 89d5d8df9327c6136fbd00078f6a34f78d85032e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshooting-azure-cloud-shell"></a>Azure bulut Kabuk sorunlarını giderme

Bilinen çözümler için Azure bulut Kabuğu sorunları şunları içerir:

## <a name="general-resolutions"></a>Genel çözümleri

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Depolama iletişim kutusu - hata: 403 RequestDisallowedByPolicy
- **Ayrıntılar**: Bulut Kabuk üzerinden depolama hesabı oluştururken, yöneticiniz tarafından yerleştirilen bir Azure ilkesi nedeniyle başarısız olur Hata iletisi içerir:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Çözümleme**: kaldırın veya depolama oluşturma reddetme Azure ilke güncelleştirmek için Azure yöneticinize başvurun.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Depolama iletişim kutusu - hata: 400 DisallowedOperation
 - **Ayrıntılar**: Azure Active Directory abonelik kullanırken, depolama oluşturulamıyor.
 - **Çözümleme**: depolama kaynaklarını oluşturabileceğinden Azure aboneliği kullanın. Azure AD aboneliklerini Azure kaynaklarını oluşturmak mümkün değildir.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Çıktı - terminal hata: terminal bağlanılamadı: websocket kurulamıyor. Tuşuna `Enter` bağlanmayı.
 - **Ayrıntılar**: Bulut Kabuk bulut Kabuk altyapı websocket bağlantı olanağı gerektirir.
 - **Çözümleme**: gönderme https isteklerini ve yanıtlarını websocket etki alanlarına sağlamak için ağ ayarlarınızı yapılandırdığınız denetleyin *. console.azure.com.

## <a name="bash-resolutions"></a>Bash çözümleri

### <a name="cannot-run-az-login"></a>Az oturum açma çalıştırılamıyor

- **Ayrıntılar**: çalışan `az login` bulut kabuğundan ya da Azure portalına imzalamak için kullanılan hesabı altında önceden doğrulanmış gibi çalışmayacaktır.
- **Çözümleme**: oturum açma veya oturumu kapatın ve hedeflenen Azure hesabınızla sağlamalarını kullanılan hesabınızı kullanma.

### <a name="cannot-run-the-docker-daemon"></a>Docker arka plan programı çalıştırılamıyor

- **Ayrıntılar**: Bulut Kabuk Kabuk ortamınızı barındırmak için bir kapsayıcı kullanır, sonuç olarak arka plan programı çalışıyor izin verilmiyor.
- **Çözümleme**: kullanan [docker makine](https://docs.docker.com/machine/overview/), docker kapsayıcıları bir uzak Docker ana bilgisayardan yönetmek için varsayılan olarak yüklü.

## <a name="powershell-resolutions"></a>PowerShell çözümleri

### <a name="no-home-directory-persistence"></a>No $Home dizin kalıcılığı

- **Ayrıntılar**: herhangi bir veriyi o uygulama (gibi: git, VIM ve diğerleri) Yazar `$Home` PowerShell oturumlarında kalıcı yapılmaz.
- **Çözümleme**: PowerShell profiliniz uygulama belirli bir klasöre sembolik bağlantı oluşturma `clouddrive` $Home için.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>CTRL + C dışında bir Cmdlet isteminden çıkın değil

- **Ayrıntılar**: bir Cmdlet isteminden çıkın çalışılırken `Ctrl+C` istemi mevcut değil.
- **Çözümleme**: istem'ndan çıkmak için basın `Ctrl+C` sonra `Enter`.

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

- **Ayrıntılar**: bir kullanıcı bir GUI uygulaması başlarsa, istemi döndürmez. Bir kullanıcı iki faktörlü kimlik doğrulaması etkin olan özel bir GitHub deposuna klonlar, örneğin, bir iletişim kutusu iki faktörlü kimlik doğrulamasını tamamlamak için görüntülenir.
- **Çözümleme**: `Ctrl+C` komutu çıkmak için.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - çevrimiçi yardım sayfasına açmaz

- **Ayrıntılar**: kullanıcı yazarsa `Get-Help Find-Module -online`, bir görür bir hata iletisi gibi:`Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Çözümleme**: URL'yi kopyalayıp tarayıcınıza el ile açın.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure VM'lerin uzaktan yönetimi sorunlarını giderme

- **Ayrıntılar**: WinRM için varsayılan Windows Güvenlik Duvarı ayarları nedeniyle kullanıcı aşağıdaki hatayı görebilirsiniz:`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Çözümleme**: VM'nizi çalıştığından emin olun. Çalıştırabilirsiniz `Get-AzureRmVM -Status` VM durumu bulunamadı.  Ardından, yeni bir güvenlik duvarı kuralı her alt ağdan WinRM bağlantılara izin vermek için Uzak VM'de ekleyin, örneğin

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 Kullanabileceğiniz [Azure özel betik uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) oturum açmak için yeni güvenlik duvarı kuralı eklemek için Uzak VM önlemek için.
 Önceki komut bir dosyaya deyin kaydedebileceğiniz `addfirerule.ps1`ve Azure storage kapsayıcısına yükleyin.
 Aşağıdaki komutu deneyin:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir`Azure sürücüsü sonucunda önbelleğe alır

- **Ayrıntılar**: sonucunu `dir` Azure sürücüde önbelleğe alınır.
- **Çözümleme**: oluşturmak veya bir kaynak Azure sürücüsü görünümünde kaldırdıktan sonra çalıştırmak `dir -force` güncelleştirmek için.
