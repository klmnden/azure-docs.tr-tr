---
title: Azure Cloud Shell sorunlarını giderme | Microsoft Docs
description: Azure Cloud Shell'i sorunlarını giderme
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 0056364883d5a4a350e5b35374e1fc3abd0c7bea
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42056951"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Sorun giderme & sınırlamaları Azure Cloud Shell

Azure Cloud shell'de sorunları gidermek için bilinen çözümleri şunlardır:

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="early-timeouts-in-firefox"></a>FireFox erken zaman aşımları

- **Ayrıntılar**: Cloud Shell, giriş/çıkış tarayıcınıza geçirmek için açık bir websocket kullanır. FireFox beklenenden önce Cloud Shell'de erken zaman aşımları neden websocket kapatabilirsiniz hazır ilkeleri vardır.
- **Çözüm**: açık FireFox gidin "hakkında: yapılandırma" URL kutusuna. "Network.websocket.timeout.ping.request" için arama yapın ve değeri 0 ile 10'a değiştirin.

### <a name="disabling-cloud-shell-in-a-locked-down-network-environment"></a>Cloud Shell kilitli bir ağda devre dışı ortam

- **Ayrıntılar**: Yöneticiler kullanıcıları için Cloud Shell erişimi devre dışı bırakmak istediğiniz. Cloud shell'e erişim için yararlanan `ux.console.azure.com` etki alanı durdurma portal.azure.com, shell.azure.com, Visual Studio Code Azure hesabı uzantısı ve docs.microsoft.com dahil olmak üzere Cloud Shell'inizin giriş noktaları için herhangi bir erişim engellenebilir.
- **Çözüm**: erişimi kısıtlamak `ux.console.azure.com` ortamınızdaki ağ ayarları aracılığıyla. Cloud Shell simgesi portal.azure.com içinde var olmaya devam edecek, ancak başarıyla hizmetine bağlanamaz.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Depolama iletişim - hata: 403 RequestDisallowedByPolicy

- **Ayrıntılar**: Cloud Shell aracılığıyla bir depolama hesabı oluştururken, yöneticiniz tarafından yerleştirilen bir Azure ilkesi nedeniyle başarısız olur Hata iletisi içerir: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Çözüm**: kaldırın veya depolama oluşturma reddetme Azure İlkesi güncelleştirmek için Azure yöneticinize başvurun.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Depolama iletişim - hata: 400 DisallowedOperation

- **Ayrıntılar**: Azure Active Directory aboneliğinin kullanırken, depolama oluşturulamıyor.
- **Çözüm**: depolama kaynakları oluşturma yeteneğine sahip bir Azure aboneliği kullanın. Azure AD abonelikleri Azure kaynaklarını oluşturmak mümkün değildir.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Çıkış - terminal hata: terminal bağlanılamadı: websocket bağlantısı kurulamadı. Tuşuna `Enter` bağlanmayı.
- **Ayrıntılar**: Cloud Shell Cloud Shell altyapı websocket bağlantısı olanağı gerektirir.
- **Çözüm**: gönderme https isteklerini ve yanıtlarını websocket konumundaki etki alanları için etkinleştirmek için ağ ayarlarını yapılandırdığınız denetleyin *. console.azure.com.

### <a name="set-your-cloud-shell-connection-to-support-using-tls-12"></a>TLS 1.2 kullanarak desteklemek için Cloud Shell bağlantınızı ayarlayın
 - **Ayrıntılar**: TLS sürümünü bağlantınız için Cloud shell'e tanımlamak için belirli ayarlara ayarlamanız gerekir.
 - **Çözüm**: tarayıcınızın güvenlik ayarlarına gidin ve "TLS 1.2 kullan" yanındaki onay kutusunu seçin.

## <a name="bash-troubleshooting"></a>Bash sorunlarını giderme

### <a name="cannot-run-the-docker-daemon"></a>Docker Daemon programını çalıştıramaz

- **Ayrıntılar**: Cloud Shell, kabuk ortamını barındırmak için bir kapsayıcı kullanır; sonuç olarak arka plan programı çalıştırma izni.
- **Çözüm**: yazılımınız [docker-machine](https://docs.docker.com/machine/overview/), uzak Docker ana bilgisayarından docker kapsayıcıları yönetmek için varsayılan olarak yüklü.

## <a name="powershell-troubleshooting"></a>PowerShell sorunlarını giderme

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez.

- **Ayrıntılar**: bir kullanıcı bir GUI uygulama başlarsa, istemi sonuç döndürmez. Örneğin, bir iki faktörlü kimlik doğrulaması etkin olan bir özel GitHub deposunu kopyaladığınızda, bir iletişim kutusu iki faktörlü kimlik doğrulamasını tamamlamak için görüntülenir.
- **Çözüm**: kapatın ve yeniden Kabuğu'nu açın.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure sanal makinelerini uzaktan yönetimi sorunlarını giderme

- **Ayrıntılar**: WinRM için varsayılan Windows Güvenlik Duvarı ayarları nedeniyle kullanıcı şu hatayı görebilirsiniz: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Çözüm**: çalıştırma `Enable-AzureRmVMPSRemoting` tüm yönlerini hedef makinede PowerShell uzaktan iletişimini etkinleştirmek için.

### <a name="dir-does-not-update-the-result-in-azure-drive"></a>`dir` Azure sürücüsü sonucunda güncelleştirmez.

- **Ayrıntılar**: kullanıcı deneyimini sonuçlarını iyileştirmek için varsayılan olarak `dir` Azure sürücüde önbelleğe alınır.
- **Çözüm**: oluşturmak, güncelleştirmek veya bir Azure kaynağı kaldırmak sonra çalıştırın `dir -force` Azure sürücüsü sonuçları güncelleştirilecek.

## <a name="general-limitations"></a>Genel sınırlamalar

Azure Cloud Shell, aşağıdaki bilinen sınırlamalara sahiptir:

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılığı

Cloud Shell oturumunuzu sağlayan makine geçicidir ve oturumunuz için 20 dakika etkin olduktan sonra dönüştürülmeden. Cloud Shell'i Azure dosya paylaşımını bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz Cloud shell'e erişim için depolama kaynaklarını ayarlama mümkün olması gerekir. Dikkat edilecek diğer noktalar şunlardır:

- Takılı depolamayla değişikliklerini içinde `clouddrive` dizin kalıcı olur. Bash, `$HOME` dizin de kalıcıdır.
- Azure dosya paylaşımlarını yalnızca içinde bağlanabilir, [bölgeye atanan](persisting-shell-storage.md#mount-a-new-clouddrive).
  - Bash hizmetinde çalıştırma `env` yap bölgenizi bulmak için `ACC_LOCATION`.
- Azure dosyaları, yalnızca yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesaplarını destekler.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell'de aşağıdaki tarayıcılardan en son sürümlerini destekler:

- Microsoft Edge
- Microsoft Internet Explorer
- Google Chrome
- Mozilla Firefox
- Apple Safari
  - Safari özel modda desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve Yapıştır

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar yalnızca başlatabilir bir kabuk türünü bir kerede ya da **Bash** veya **PowerShell**. Ancak, Bash veya PowerShell aynı anda çalışan birden çok örneği olabilir. Hangi mevcut oturumlarını sonlandırır Bash veya PowerShell nedenleri arasında yeniden başlatmak için Cloud Shell değiştirme.

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell'de etkileşimli kullanım durumları için tasarlanmıştır. Sonuç olarak, herhangi bir uzun süre çalışan etkileşimli olmayan oturum uyarı vermeden sonlandırılır.

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinleri, sudo erişimi olmadan normal kullanıcı olarak ayarlanır. Dışında herhangi bir yükleme, `$Home` dizin kalıcı değil.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="editing-bashrc"></a>.Bashrc düzenleme

Bunun yapılması, .bashrc düzenleme beklenmeyen hatalar Cloud Shell'de neden olabileceği zaman uyarı alın.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="preview-version-of-azuread-module"></a>AzureAD modülüne Önizleme sürümü

Şu anda `AzureAD.Standard.Preview`, .NET Standard tabanlı, modül Önizleme sürümü kullanılabilir. Bu modül aynı işlevselliği sağlar `AzureAD`.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modül işlevi

`SqlServer` Cloud Shell'de dahil modülü PowerShell Core yalnızca yayın öncesi desteği içeriyor. Özellikle, `Invoke-SqlCmd` henüz kullanıma sunulmamıştır.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturduğunuzda varsayılan dosya konumu

PowerShell cmdlet'lerini kullanarak, kullanıcılar Azure sürücüsü altındaki dosyalar oluşturamaz. Kullanıcıların yeni dosyaları vim veya nano gibi diğer araçları kullanarak oluşturduğunuzda, dosyalar için kaydedilir `$HOME` varsayılan olarak.

### <a name="commands-that-create-gui-pop-ups-are-not-supported"></a>GUI açılır pencereleri oluşturma komutları desteklenmiyor

Kullanıcı bir Windows iletişim kutusu gibi oluşturacak bir komut çalıştırıyorsa `Connect-AzureAD` veya `Connect-AzureRmAccount`, bir gördüğü hata iletisi gibi: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-can-throw-psreadline-exception"></a>Sekme tamamlama PSReadline durum oluşturabilir

Emacs için kullanıcının PSReadline EditMode ayarlarsanız, tüm olasılıkları aracılığıyla sekme tamamlama, görüntülenecek kullanıcı çalışır ve pencere boyutunu görüntüleme olanakları depolayamayacak kadar PSReadline işlenmeyen özel durum oluşturur.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğunu görüntüleme sonra geniş boşluk

Bir komut veya kullanıcı eyleminin bir ilerleme çubuğu, böyle bir sekme görüntüler içinde çalışırken Tamamlanıyor `Azure:` sürücü imleç düzgün ayarlanmamıştır ve ilerleme çubuğu daha önce olduğu boşluk görünür mümkündür.

### <a name="random-characters-appear-inline"></a>Satır içi rastgele karakterler görünür

İmleç konumu dizisi kodları, örneğin `5;13R`, kullanıcı girişi görünebilir. Karakterler el ile kaldırılabilir.

## <a name="personal-data-in-cloud-shell"></a>Cloud shell'de kişisel verileri

Azure Cloud Shell ciddiye kişisel verilerinizi alır, yakalanan ve Azure Cloud Shell hizmeti tarafından depolanan verileri gibi en son Kabuk kullanılan deneyiminiz için varsayılanlar sağlamak üzere kullanılan, tercih edilen yazı tipi boyutu, tercih edilen yazı tipi ve dosya ayrıntıları Paylaş Bu sürücü geri bulut. Dışarı aktarma veya bu verileri silmek istemeniz durumunda, aşağıdaki yönergeleri kullanın.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Dışarı Aktarma
Şunları **dışarı** kabuğu, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın Cloud Shell kaydeder, gibi kullanıcı ayarları tercih edilir.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)
2. Bash veya PowerShell içinde aşağıdaki komutları çalıştırın:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  ((Invoke-WebRequest -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}).Content | ConvertFrom-Json).properties | Format-List
```

### <a name="delete"></a>Sil
Şunları **Sil** Cloud Shell kaydeder, gibi kullanıcı ayarlarınızı kabuğu, yazı tipi boyutunu ve yazı tipi aşağıdaki komutları çalıştırın tercih edilir. Cloud Shell'i yeniden başlattığınızda, yerleşik bir dosya paylaşımı yeniden istenir. 

>[!Note]
> Kullanıcı ayarlarınızı silerseniz, gerçek Azure dosyaları paylaşım silinmez. Bu eylemi tamamlamak için Azure dosyalarınıza gidin.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i Başlat")](https://shell.azure.com)
2. Bash veya PowerShell içinde aşağıdaki komutları çalıştırın:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  Invoke-WebRequest -Method Delete -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}
  ```
