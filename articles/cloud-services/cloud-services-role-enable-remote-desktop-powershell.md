---
title: PowerShell kullanarak Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme
description: Uzak Masaüstü bağlantılarına izin verecek şekilde PowerShell kullanarak azure bulut hizmeti uygulamanızı yapılandırma
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 43ccc8e53c30219630ad10ee66a4db38656818e6
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651014"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>PowerShell kullanarak Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme

> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolünün erişmenizi sağlar. Sorun giderme ve çalışırken uygulamanızdaki sorunları tanılamak için Uzak Masaüstü Bağlantısı'nı kullanabilirsiniz.

Bu makalede, PowerShell kullanarak, bulut hizmeti rolleri, Uzak Masaüstü'nü etkinleştirme açıklar. Bkz: [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/overview) Bu makale için gereken önkoşulları için. Uygulama dağıtıldıktan sonra Uzak Masaüstü verebilmeniz için PowerShell Uzak Masaüstü uzantısı kullanır.

## <a name="configure-remote-desktop-from-powershell"></a>Uzak Masaüstü'nden PowerShell yapılandırma
[Kümesi AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'i, belirtilen roller veya Bulut hizmeti dağıtımınızın tüm roller üzerinde Uzak Masaüstü'nü etkinleştirmenizi sağlar. Cmdlet ile Uzak Masaüstü kullanıcı için kullanıcı adı ve parola belirtmenize olanak tanır *kimlik bilgisi* bir PSCredential nesnesi kabul eden bir parametre.

Etkileşimli olarak PowerShell kullanıyorsanız, kolayca PSCredential nesnesinin çağırarak ayarlayabileceğiniz [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

```powershell
$remoteusercredentials = Get-Credential
```

Bu komut, güvenli bir şekilde uzak kullanıcı için kullanıcı adı ve parola girmesini sağlayan bir iletişim kutusu görüntüler.

Otomasyon senaryolarda PowerShell yardımcı olduğundan, ayrıca ayarlayabilirsiniz **PSCredential** kullanıcı etkileşimi gerektirmeyen bir şekilde nesne. İlk olarak, güvenli bir parola ayarlamanız gerekir. Belirten bir düz metin parola dönüştürür kullanarak bir güvenli dize ile başlayan [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Ardından bu güvenli dize bir şifrelenmiş standart dize kullanarak usercontrol'e dönüştürebilirsiniz gerekir [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx). Bu şifreli standart dize kullanarak bir dosyaya kaydedebilirsiniz [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).

Her defasında parolayı yazmanız gerekmez, güvenli parola dosyası da oluşturabilirsiniz. Ayrıca, güvenli parola dosyasına düz metin dosyası iyidir. Güvenli parola dosyası oluşturmak için aşağıdaki PowerShell kullanın:

```powershell
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Parolayı ayarlarken karşıladığınızı doğrulayın [karmaşıklık gereksinimlerini](https://technet.microsoft.com/library/cc786468.aspx).

Güvenli parola dosyasından kimlik bilgisi nesnesi oluşturmak için dosya içeriğini okumak ve bunları geri güvenli kullanarak bir dize dönüştürmek [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

[Kümesi AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet de kabul eder bir *sona erme* belirten parametresi bir **DateTime** , hangi kullanıcı hesabının süresi. Örneğin, geçerli tarih ve saat birkaç gün süresi dolacak şekilde hesabı ayarlayabilirsiniz.

Bu PowerShell örneği, bir bulut hizmetinde Uzak Masaüstü uzantısı ayarlama işlemini göstermektedir:

```powershell
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Ayrıca isteğe bağlı olarak, dağıtım yuvası ve Uzak Masaüstü'nü etkinleştirmek istediğiniz rolleri de belirtebilirsiniz. Cmdlet, Uzak Masaüstü'nü tüm rollerdeki etkinleştirir, bu parametre belirtilmezse, **üretim** dağıtım yuvası.

Dağıtımla ilgili Uzak Masaüstü uzantısıdır. Hizmet için yeni bir dağıtımını oluşturun, bu dağıtım üzerinde Uzak Masaüstü'nü etkinleştirme gerekir. Her zaman Uzak Masaüstü etkin olmasını istiyorsanız, PowerShell betikleri, dağıtım iş akışınıza tümleştirme düşünmelisiniz.

## <a name="remote-desktop-into-a-role-instance"></a>Uzak Masaüstü Bağlantısı bir rol örneği

[Get-AzureRemoteDesktopFile](/powershell/module/servicemanagement/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet'tir uzaktan kullanılan Desktop'a bulut hizmetinizin belirli rol örneği. Kullanabileceğiniz *LocalPath* RDP indirmek için parametre dosyasını yerel olarak. Ya da *başlatma* doğrudan bulut Hizmeti rol örneğine erişmek için Uzak Masaüstü Bağlantısı iletişim kutusunu başlatmak için parametre.

```powershell
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```

## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Uzak Masaüstü uzantısı bir hizmette etkin olup olmadığını denetleyin

[Get-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet'i görüntüler Uzak Masaüstü etkin veya bir hizmet dağıtımı için devre dışı. Cmdlet'i, Uzak Masaüstü kullanıcı ve Uzak Masaüstü genişletme için etkin bir rol için kullanıcı adını döndürür. Varsayılan olarak, bu dağıtım yuvasına gerçekleşir ve hazırlama yuvası kullanmayı seçebilirsiniz.

```powershell
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Bir hizmetten Uzak Masaüstü uzantısını kaldırma

İlk dağıtımı Uzak Masaüstü uzantının zaten etkinleştirdiyseniz ve Uzak Masaüstü ayarları güncelleştirmeniz gerekiyor, uzantıyı kaldırın. Ve yeni ayarlarla yeniden etkinleştirin. Örneğin, uzak kullanıcı hesabı için yeni bir parola ayarlamak istiyorsanız veya hesabın süresi doldu. Bunun yapılması, etkin Uzak Masaüstü uzantılı varolan dağıtımları gereklidir. Yeni dağıtımlar için yalnızca uzantıyı doğrudan uygulayabilirsiniz.

Uzak Masaüstü uzantısı dağıtımdan kaldırmak için kullanabileceğiniz [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/servicemanagement/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'i. Ayrıca isteğe bağlı olarak, dağıtım yuvası ve Uzak Masaüstü uzantısı kaldırmak istediğiniz rol de belirtebilirsiniz.

```powershell
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Uzantı yapılandırmasını tamamen kaldırmak için çağırmalıdır *Kaldır* cmdlet'iyle **UninstallConfiguration** parametresi.
>
> **UninstallConfiguration** hizmetine uygulanan herhangi bir uzantı yapılandırma parametresi kaldırır. Hizmet yapılandırması ile ilişkili her uzantı yapılandırmadır. Çağırma *Kaldır* olmadan cmdlet'i **UninstallConfiguration** ayırır <mark>dağıtım</mark> uzantısı yapılandırmadan okuyoruz, bu nedenle etkili bir şekilde kaldırılıyor uzantı. Ancak, uzantı yapılandırması hizmetiyle ilişkili olarak kalır.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)