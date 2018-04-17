---
title: Azure bulut Kabuk sorunlarını giderme | Microsoft Docs
description: Azure bulut Kabuk sorunlarını giderme
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: damaerte
ms.openlocfilehash: 7ab344f77ef88ffdc2ff1976d97b0b9aa86aa3fc
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Sorun giderme & Azure sınırlamaları bulut Kabuğu

Azure bulut kabuğunda sorunlarını gidermek için bilinen çözümleri şunlardır:

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="early-timeouts-in-firefox"></a>FireFox erken zaman aşımlarına
- **Ayrıntılar**: Bulut Kabuk tarayıcınıza giriş/çıkış geçirmek için bir açık websocket kullanır. FireFox erken erken zaman aşımları bulut Kabuğu'nda neden websocket kapatabilirsiniz hazır ilkeleri vardır.
- **Çözümleme**: açık FireFox gidin "hakkında: yapılandırma" URL kutusuna. "Network.websocket.timeout.ping.request" için arama ve değeri 0 ile 10'a değiştirin.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Depolama iletişim kutusu - hata: 403 RequestDisallowedByPolicy
- **Ayrıntılar**: Bulut Kabuk üzerinden depolama hesabı oluştururken, yöneticiniz tarafından yerleştirilen bir Azure ilkesi nedeniyle başarısız olur Hata iletisi içerir: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Çözümleme**: kaldırın veya depolama oluşturma reddetme Azure ilke güncelleştirmek için Azure yöneticinize başvurun.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Depolama iletişim kutusu - hata: 400 DisallowedOperation
 - **Ayrıntılar**: Azure Active Directory abonelik kullanırken, depolama oluşturulamıyor.
 - **Çözümleme**: depolama kaynaklarını oluşturabileceğinden Azure aboneliği kullanın. Azure AD aboneliklerini Azure kaynaklarını oluşturmak mümkün değildir.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Çıktı - terminal hata: terminal bağlanılamadı: websocket kurulamıyor. Tuşuna `Enter` bağlanmayı.
 - **Ayrıntılar**: Bulut Kabuk bulut Kabuk altyapı websocket bağlantı olanağı gerektirir.
 - **Çözümleme**: gönderme https isteklerini ve yanıtlarını websocket etki alanlarına sağlamak için ağ ayarlarınızı yapılandırdığınız denetleyin *. console.azure.com.

## <a name="bash-troubleshooting"></a>Sorun giderme bash

### <a name="cannot-run-az-login"></a>Az oturum açma çalıştırılamıyor

- **Ayrıntılar**: çalışan `az login` bulut kabuğundan ya da Azure portalına imzalamak için kullanılan hesabı altında önceden doğrulanmış gibi çalışmayacaktır.
- **Çözümleme**: oturum açma veya oturumu kapatın ve hedeflenen Azure hesabınızla sağlamalarını kullanılan hesabınızı kullanma.

### <a name="cannot-run-the-docker-daemon"></a>Docker arka plan programı çalıştırılamıyor

- **Ayrıntılar**: Bulut Kabuk Kabuk ortamınızı barındırmak için bir kapsayıcı kullanır, sonuç olarak arka plan programı çalışıyor izin verilmiyor.
- **Çözümleme**: kullanan [docker makine](https://docs.docker.com/machine/overview/), docker kapsayıcıları bir uzak Docker ana bilgisayardan yönetmek için varsayılan olarak yüklü.

## <a name="powershell-troubleshooting"></a>PowerShell sorunlarını giderme

### <a name="no-home-directory-persistence"></a>No $Home dizin kalıcılığı

- **Ayrıntılar**: herhangi bir veriyi o uygulama (gibi: git, VIM ve diğerleri) Yazar `$Home` PowerShell oturumlarında kalıcı yapılmaz.
- **Çözümleme**: PowerShell profiliniz uygulama belirli bir klasöre sembolik bağlantı oluşturma `clouddrive` $Home için.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>CTRL + C dışında bir Cmdlet isteminden çıkın değil

- **Ayrıntılar**: bir Cmdlet isteminden çıkın çalışılırken `Ctrl+C` istemi mevcut değil.
- **Çözümleme**: istem'ndan çıkmak için basın `Ctrl+C` sonra `Enter`.

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

- **Ayrıntılar**: bir kullanıcı bir GUI uygulaması başlarsa, istemi döndürmez. Bir kullanıcı iki faktörlü kimlik doğrulaması etkin olan özel bir GitHub deposuna klonlar, örneğin, bir iletişim kutusu iki faktörlü kimlik doğrulamasını tamamlamak için görüntülenir.  
- **Çözümleme**: kapatıp Kabuk.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - çevrimiçi yardım sayfasına açmaz

- **Ayrıntılar**: kullanıcı yazarsa `Get-Help Find-Module -online`, bir görür bir hata iletisi gibi: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Çözümleme**: URL'yi kopyalayıp tarayıcınıza el ile açın.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure VM'lerin uzaktan yönetimi sorunlarını giderme

- **Ayrıntılar**: WinRM için varsayılan Windows Güvenlik Duvarı ayarları nedeniyle kullanıcı aşağıdaki hatayı görebilirsiniz: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
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

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` Azure sürücüsü sonucunda önbelleğe alır

- **Ayrıntılar**: sonucunu `dir` Azure sürücüde önbelleğe alınır.
- **Çözümleme**: oluşturmak veya bir kaynak Azure sürücüsü görünümünde kaldırdıktan sonra çalıştırmak `dir -force` güncelleştirmek için.

## <a name="general-limitations"></a>Genel sınırlamalar
Azure bulut Kabuk aşağıdaki bilinen sınırlamalara sahiptir:

### <a name="system-state-and-persistence"></a>Sistem durumu ve sürdürme

Bulut Kabuk oturumunuz sağlar makine geçicidir ve oturumunuz için 20 dakika etkin değil sonra geri dönüştürüldüğünde. Bulut Kabuk Azure dosya paylaşımının bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz bulut Kabuk erişmek için depolama kaynakları ayarlamak kurabilmesi gerekir. Diğer konular şunlardır:

* Takılı depolamayla yalnızca değişiklikleri içinde `clouddrive` dizin kaldı. Bash içinde `$Home` dizin de kalıcı.
* Azure dosya paylaşımları takılı yalnızca içinden, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Bash'te, çalıştırmak `env` olarak ayarlayın, bölgenizdeki bulmak için `ACC_LOCATION`.
* Azure dosyaları yalnızca yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesaplarını destekler.

### <a name="browser-support"></a>Tarayıcı desteği

Bulut Kabuğu'nu Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Apple Safari en son sürümlerini destekler. Safari özel modunda desteklenmiyor.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatma Kabuk bir tür aynı anda ya da **Bash** veya **PowerShell**. Bununla birlikte, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Hangi oturumlarına sonlandırır Bash veya PowerShell nedenleri arasında bulut Kabuğu'nu yeniden başlatmak için değiştirme.

### <a name="usage-limits"></a>Kullanım sınırları

Bulut Kabuk etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, tüm uzun süre çalışan etkileşimli olmayan oturumlar uyarmadan sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yüklemesi, `$Home` dizin kalıcı değildir.

### <a name="editing-bashrc"></a>.Bashrc düzenleme

Bunun yapılması .bashrc düzenleme beklenmeyen hatalara bulut Kabuğu'nda neden olabilir. uyarı alın.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="slow-startup-time"></a>Yavaş başlangıç zamanı

Azure bulut Kabuğu (Önizleme) PowerShell Önizleme sırasında başlatmak için 60 saniye sürebilir.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturduğunuzda varsayılan dosya konumu:

PowerShell cmdlet'lerini kullanarak, kullanıcılar dosyaları Azure sürücüsü altında oluşturulamıyor. Kullanıcıların yeni dosyaları VIM veya nano, gibi diğer araçları kullanarak oluşturduğunuzda dosyalar varsayılan olarak C:\Users klasörüne kaydedilir. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

Kullanıcı bir Windows iletişim kutusu gibi oluşturacak bir komut çalıştırırsa `Connect-AzureAD` veya `Login-AzureRMAccount`, bir görür bir hata iletisi gibi: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="gdpr-compliance-for-cloud-shell"></a>Bulut Kabuğu GDPR uyumluluk

Azure bulut Kabuk kişisel bilgileriniz ciddiye alır, yakalanan ve Azure bulut Kabuk hizmeti tarafından depolanan verileri gibi Kabuk en son kullanılan deneyiminiz için varsayılanları sağlamak için kullanılan, ayrıntıları tercih edilen yazı tipi boyutu, tercih edilen yazı tipi ve dosya paylaşımı clouddrive yedekleyin. Dışarı aktarma veya bu verileri silmek istediğiniz, aşağıdaki yönergeleri eklediniz.

### <a name="export"></a>Dışarı Aktarma
Aşağıdakileri yapmak için **verme** bulut Kabuk kaydeder, gibi kullanıcı ayarları tercih ettiği Kabuk, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın.

1. Bulut Kabuğu'nda Bash'i başlatın
2. Aşağıdaki komutları çalıştırın:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>Sil
Aşağıdakileri yapmak için **silmek** bulut Kabuk kaydeder, gibi kullanıcı ayarlarınızı tercih ettiği Kabuk, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın. Bulut Kabuk yeniden başlattığınızda, yerleşik bir dosya paylaşımı yeniden istenir. 

Kullanıcı ayarlarınız silerseniz paylaşımı silinmez gerçek Azure dosyaları bu eylemi tamamlamak için Azure dosyasına gidin.

1. Bulut Kabuğu'nda Bash'i başlatın
2. Aşağıdaki komutları çalıştırın:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
