---
title: Azure Cloud Shell sorunlarını giderme | Microsoft Docs
description: Azure Cloud Shell'i sorunlarını giderme
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
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 21bc0633a9cc607325b48998791cb12631ecd0d7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856496"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Sorun giderme & sınırlamaları Azure Cloud Shell

Azure Cloud shell'de sorunları gidermek için bilinen çözümleri şunlardır:

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="early-timeouts-in-firefox"></a>FireFox erken zaman aşımları
- **Ayrıntılar**: Cloud Shell, giriş/çıkış tarayıcınıza geçirmek için açık bir websocket kullanır. FireFox beklenenden önce Cloud Shell'de erken zaman aşımları neden websocket kapatabilirsiniz hazır ilkeleri vardır.
- **Çözüm**: açık FireFox gidin "hakkında: yapılandırma" URL kutusuna. "Network.websocket.timeout.ping.request" için arama yapın ve değeri 0 ile 10'a değiştirin.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Depolama iletişim - hata: 403 RequestDisallowedByPolicy
- **Ayrıntılar**: Cloud Shell aracılığıyla bir depolama hesabı oluştururken, yöneticiniz tarafından yerleştirilen bir Azure ilkesi nedeniyle başarısız olur Hata iletisi içerir: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Çözüm**: kaldırın veya depolama oluşturma reddetme Azure İlkesi güncelleştirmek için Azure yöneticinize başvurun.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Depolama iletişim - hata: 400 DisallowedOperation
 - **Ayrıntılar**: Azure Active Directory aboneliğinin kullanırken, depolama oluşturulamıyor.
 - **Çözüm**: depolama kaynakları oluşturma yeteneğine sahip bir Azure aboneliği kullanın. Azure AD abonelikleri Azure kaynaklarını oluşturmak mümkün değildir.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Çıkış - terminal hata: terminal bağlanılamadı: websocket bağlantısı kurulamadı. Tuşuna `Enter` bağlanmayı.
 - **Ayrıntılar**: Cloud Shell Cloud Shell altyapı websocket bağlantısı olanağı gerektirir.
 - **Çözüm**: gönderme https isteklerini ve yanıtlarını websocket konumundaki etki alanları için etkinleştirmek için ağ ayarlarını yapılandırdığınız denetleyin *. console.azure.com.

## <a name="bash-troubleshooting"></a>Bash sorunlarını giderme

### <a name="cannot-run-the-docker-daemon"></a>Docker Daemon programını çalıştıramaz

- **Ayrıntılar**: Cloud Shell, kabuk ortamını barındırmak için bir kapsayıcı kullanır; sonuç olarak arka plan programı çalıştırma izni.
- **Çözüm**: yazılımınız [docker-machine](https://docs.docker.com/machine/overview/), uzak Docker ana bilgisayarından docker kapsayıcıları yönetmek için varsayılan olarak yüklü.

## <a name="powershell-troubleshooting"></a>PowerShell sorunlarını giderme

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

- **Ayrıntılar**: bir kullanıcı bir GUI uygulaması başlarsa, istemi sonuç döndürmez. Örneğin, bir kullanıcının iki Etmenli kimlik doğrulamasının etkin olan özel bir GitHub deposuna kopyalar, bir iletişim kutusu iki faktörlü kimlik doğrulamasını tamamlamak için görüntülenir.  
- **Çözüm**: kapatın ve yeniden Kabuğu'nu açın.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - çevrimiçi yardım sayfasına açılmıyor

- **Ayrıntılar**: kullanıcı yazarsa `Get-Help Find-Module -online`, bir gördüğü hata iletisi gibi: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Çözüm**: el ile tarayıcınızda açın ve URL'yi kopyalayın.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure sanal makinelerini uzaktan yönetimi sorunlarını giderme

- **Ayrıntılar**: WinRM için varsayılan Windows Güvenlik Duvarı ayarları nedeniyle kullanıcı şu hatayı görebilirsiniz: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Çözüm**: çalıştırma `Enable-AzureRmVMPSRemoting` tüm yönlerini hedef makinede PowerShell uzaktan iletişimini etkinleştirmek için.
 

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` Azure sürücüsü sonucunda önbelleğe alır.

- **Ayrıntılar**: sonucunu `dir` Azure sürücüde önbelleğe alınır.
- **Çözüm**: oluşturun veya Azure sürücüsü görünümünde bir kaynağı kaldırırsanız sonra çalıştırın `dir -force` güncelleştirilecek.

## <a name="general-limitations"></a>Genel sınırlamalar
Azure Cloud Shell, aşağıdaki bilinen sınırlamalara sahiptir:

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılığı

Cloud Shell oturumunuzu sağlayan makine geçicidir ve oturumunuz için 20 dakika etkin olduktan sonra dönüştürülmeden. Cloud Shell'i Azure dosya paylaşımını bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz Cloud shell'e erişim için depolama kaynaklarını ayarlama mümkün olması gerekir. Dikkat edilecek diğer noktalar şunlardır:

* Takılı depolamayla değişikliklerini içinde `clouddrive` dizin kalıcı olur. Bash, `$Home` dizin de kalıcıdır.
* Azure dosya paylaşımlarını yalnızca içinde bağlanabilir, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Bash hizmetinde çalıştırma `env` yap bölgenizi bulmak için `ACC_LOCATION`.
* Azure dosyaları, yalnızca yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesaplarını destekler.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell'i Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Safari Apple'nın en son sürümlerini destekler. Safari özel modda desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatabilir bir kabuk türünü bir kerede ya da **Bash** veya **PowerShell**. Ancak, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Hangi mevcut oturumlarını sonlandırır Bash veya PowerShell nedenleri arasında yeniden başlatmak için Cloud Shell değiştirme.

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell'de etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, herhangi bir uzun süre çalışan etkileşimli olmayan oturum uyarı vermeden sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinleri, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yükleme, `$Home` dizin kalıcı değil.

### <a name="editing-bashrc"></a>.Bashrc düzenleme

Bunun yapılması, .bashrc düzenleme beklenmeyen hatalar Cloud Shell'de neden olabileceği zaman uyarı alın.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="azuread-module-name"></a>`AzureAD` Modül adı

`AzureAD` Modül adı şu anda `AzureAD.Standard.Preview`, modül aynı işlevselliği sağlar.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modül işlevi

`SqlServer` Cloud Shell'de dahil modülü PowerShell Core yalnızca yayın öncesi desteği içeriyor. Özellikle, `Invoke-SqlCmd` henüz kullanıma sunulmamıştır.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturduğunuzda varsayılan dosya konumu:

PowerShell cmdlet'lerini kullanarak, kullanıcılar Azure sürücüsü altındaki dosyaları oluşturulamıyor. Kullanıcıların yeni dosyaları vim veya nano gibi diğer araçları kullanarak oluşturduğunuzda, dosyalar için kaydedilir `$HOME` varsayılan olarak. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

Kullanıcı bir Windows iletişim kutusu gibi oluşturacak bir komut çalıştırıyorsa `Connect-AzureAD` veya `Connect-AzureRmAccount`, bir gördüğü hata iletisi gibi: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Sekme tamamlama PSReadline kilitleniyor

Emacs için kullanıcının EditMode PSReadline olarak ayarlanırsa, kullanıcı görüntüleme aracılığıyla sekme tamamlama, tüm olanakları dener ve pencere boyutunu görüntüleme olanakları depolayamayacak kadar PSReadline kilitlenir.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğunu görüntüleme sonra geniş boşluk

Kullanıcı bir ilerleme çubuğu, böyle bir sekme görüntüler bir eylem gerçekleştirirse içinde çalışırken Tamamlanıyor `Azure:` sürücü imleç düzgün ayarlanmamıştır ve ilerleme çubuğu daha önce olduğu boşluk görünür mümkündür.

### <a name="random-characters-appear-inline"></a>Satır içi rastgele karakterler görünür

İmleç konumu dizisi kodları, örneğin `5;13R`, kullanıcı girişi görünebilir.  Karakterler el ile kaldırılabilir.

## <a name="personal-data-in-cloud-shell"></a>Cloud shell'de kişisel verileri

Azure Cloud Shell ciddiye kişisel verilerinizi alır, yakalanan ve Azure Cloud Shell hizmeti tarafından depolanan verileri gibi en son Kabuk kullanılan deneyiminiz için varsayılanlar sağlamak üzere kullanılan, tercih edilen yazı tipi boyutu, tercih edilen yazı tipi ve dosya ayrıntıları Paylaş Bu sürücü geri bulut. Dışarı aktarma veya bu verileri silmek istemeniz durumunda, aşağıdaki yönergeleri ekledik.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Dışarı Aktarma
Şunları **dışarı** kabuğu, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın Cloud Shell kaydeder, gibi kullanıcı ayarları tercih edilir.

1. Cloud shell'de Bash'i başlatma
2. Aşağıdaki komutları çalıştırın:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>Sil
Şunları **Sil** Cloud Shell kaydeder, gibi kullanıcı ayarlarınızı kabuğu, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın tercih edilir. Cloud Shell'i yeniden başlattığınızda, yerleşik bir dosya paylaşımı yeniden istenir. 

Gerçek Azure dosyaları paylaşım silinmeyecek, kullanıcı ayarlarınızı silerseniz, bu eylemi tamamlamak için Azure dosyaları'na gidin.

1. Cloud shell'de Bash'i başlatma
2. Aşağıdaki komutları çalıştırın:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
