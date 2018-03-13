---
title: "PowerShell kullanarak Azure Cloud Services rolünde için Uzak Masaüstü Bağlantısı etkinleştir"
description: "Uzak Masaüstü bağlantılara izin vermek için PowerShell kullanarak azure bulut hizmeti uygulamanızı yapılandırma"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 84fe7ba418399562b6e36ed009c5e6e47fbe24da
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>PowerShell kullanarak Azure Cloud Services rolünde için Uzak Masaüstü Bağlantısı etkinleştir

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolü Masaüstü erişmenize olanak tanır. Uzak Masaüstü Bağlantısı çalışırken, uygulamanızın sorunları tanılamak ve gidermek için kullanabilirsiniz.

Bu makalede, Uzak Masaüstü'nü PowerShell kullanarak, bulut hizmeti rollerinizi etkinleştirmek açıklar. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) Bu makale için gereken önkoşulları için. Uygulama dağıtıldıktan sonra Uzak Masaüstü'nü etkinleştirmek için PowerShell Uzak Masaüstü uzantısı kullanır.

## <a name="configure-remote-desktop-from-powershell"></a>Uzak Masaüstü PowerShell üzerinden yapılandırmak

[Kümesi AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'i belirtilen roller veya Bulut hizmeti dağıtımınızın tüm roller üzerinde Uzak Masaüstü etkinleştirmenize olanak sağlar. Cmdlet'i Uzak Masaüstü kullanıcı için kullanıcı adı ve parola belirtmenize olanak tanır *kimlik bilgisi* bir PSCredential nesnesi kabul parametresi.

PowerShell etkileşimli olarak kullanıyorsanız, kolayca PSCredential nesnesinin çağırarak ayarlayabilirsiniz [Get-kimlik bilgilerinin](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i.

```
$remoteusercredentials = Get-Credential
```

Bu komut, güvenli bir şekilde uzak kullanıcı için kullanıcı adı ve parola girmesini sağlayan bir iletişim kutusu görüntüler.

PowerShell Otomasyon senaryolarda yardımcı olduğundan, ayrıca ayarlayabilirsiniz **PSCredential** , kullanıcı etkileşimi gerektirmeyen şekilde nesnesi. İlk olarak, güvenli bir parola ayarlamanız gerekir. Düz metinli bir parola dönüştürür kullanarak bir güvenli dize belirterek başlamadan [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Bir şifrelenmiş standart dize kullanarak bu güvenli dize dönüştürmeniz gerekir sonraki [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Bu şifreli bir standart dize kullanarak bir dosyaya kaydedebilirsiniz [kümesi içeriği](https://technet.microsoft.com/library/ee176959.aspx).

Böylece her zaman parola yazmak zorunda kalmazsınız güvenli parola dosyası da oluşturabilirsiniz. Ayrıca, güvenli parola dosyasına düz metin dosyasından daha iyidir. Güvenli parola dosyası oluşturmak için aşağıdaki PowerShell kullanın:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Parolayı ayarlarken, karşıladığından emin olun [karmaşıklık gereksinimlerini](https://technet.microsoft.com/library/cc786468.aspx).

Güvenli parola dosyasından kimlik bilgisi nesnesi oluşturmak için dosya içeriğini okumak ve bunları geri bir güvenli dize kullanmaya Dönüştür [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

[Kümesi AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'ini de kabul eder bir *sona erme* belirten parametresi bir **DateTime** hangi kullanıcı hesabının süresi sırasında. Örneğin, geçerli tarih ve saat birkaç gün süresi dolacak şekilde hesabı ayarlayabilirsiniz.

Bu PowerShell örnek bir bulut hizmeti Uzak Masaüstü uzantısı kümesi gösterilmektedir:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Dağıtım yuvası ve Uzak Masaüstü'nü etkinleştirmek istediğiniz rolleri Ayrıca isteğe bağlı olarak belirtebilirsiniz. Bu parametre belirtilmezse, Uzak Masaüstü'nü tüm rollerde cmdlet sağlar **üretim** dağıtım yuvası.

Dağıtımla ilişkili Uzak Masaüstü uzantısıdır. Hizmeti için yeni bir dağıtım oluşturursanız, bu dağıtımda, Uzak Masaüstü'nü etkinleştirin sahip. Her zaman Uzak Masaüstü etkin olmasını istiyorsanız, PowerShell komut dosyalarını iş akışınıza dağıtım tümleştirme düşünmelisiniz.

## <a name="remote-desktop-into-a-role-instance"></a>Rol örneği içine Uzak Masaüstü

[Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet'tir kullanılan Uzak Masaüstü bulut hizmetinizin belirli rol örneği başlatın. Kullanabileceğiniz *LocalPath* RDP indirmek için parametre dosyası yerel olarak. Veya kullanabilirsiniz *başlatma* doğrudan bulut Hizmeti rol örneğine erişmek için Uzak Masaüstü Bağlantısı iletişim kutusunu başlatmak için parametre.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```

## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Uzak Masaüstü uzantısı'nda bir hizmeti etkin olup olmadığını denetleyin

The [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment. Cmdlet Uzak Masaüstü kullanıcı ve Uzak Masaüstü uzantısı için etkin rolleri için kullanıcı adını döndürür. Varsayılan olarak, bu dağıtım yuvasına gerçekleşir ve hazırlama yuvası kullanmayı seçebilirsiniz.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Uzak Masaüstü uzantısı bir hizmetten kaldırın

Uzak Masaüstü uzantısı bir dağıtımda zaten etkinleştirdiyseniz ve uzak masaüstü ayarlarını güncelleştirmek ihtiyacınız varsa, ilk uzantısını kaldırın. Ve yeni ayarlarla yeniden etkinleştirin. Örneğin, uzak kullanıcı hesabı için yeni bir parola ayarlayın veya hesabın süresi istiyorsanız. Bunun yapılması, etkin Uzak Masaüstü uzantısına sahip var olan dağıtımlar üzerinde gereklidir. Yeni dağıtımlar için yalnızca uzantısı doğrudan uygulayabilirsiniz.

Uzak Masaüstü uzantısı dağıtımından kaldırmak için kullanabileceğiniz [Kaldır AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet'i. Dağıtım yuvası ve Uzak Masaüstü uzantıyı kaldırmak istediğiniz rol Ayrıca isteğe bağlı olarak belirtebilirsiniz.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Uzantı yapılandırması tamamen kaldırmak için çağırmalıdır *kaldırmak* cmdlet'iyle **UninstallConfiguration** parametresi.
>
> **UninstallConfiguration** parametresi hizmetine uygulanan herhangi bir uzantı yapılandırma kaldırır. Hizmet yapılandırmasıyla ilişkili her uzantı yapılandırmadır. Çağırma *kaldırmak* cmdlet olmadan **UninstallConfiguration** keser <mark>dağıtım</mark> uzantısı yapılandırmasından nedenle etkili bir şekilde kaldırılıyor uzantı. Ancak, uzantısı Yapılandırma hizmeti ile ilişkili olarak kalır.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
