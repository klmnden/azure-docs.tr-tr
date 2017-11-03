---
title: "Azure yığın kaydetme | Microsoft Docs"
description: "Azure yığını, Azure aboneliğiniz ile kaydedin."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: erikje
ms.openlocfilehash: 24cde66a132ae2e1ba0eb9b1564915746e5ca448
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Azure yığını, Azure aboneliğiniz ile kaydetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Kayıt yaptırabilirsiniz [Azure yığın](azure-stack-poc.md) Azure Market öğesi Azure'dan karşıdan yüklemek ve ticari veri geri Microsoft'a raporlama ayarlamak için ile. 

> [!NOTE]
>Market dağıtım ve kullanım raporlama gibi önemli Azure yığın işlevselliğini sınamak için sağladığından kayıt önerilir. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Azure yığın Geliştirme Seti kullanıcıların bunlar rapor herhangi kullanım için ücretlendirilmezsiniz.
>


## <a name="get-azure-subscription"></a>Azure aboneliği edinme

Azure ile Azure yığın kaydetmeden önce şunlara sahip olmalısınız:

- Bir Azure aboneliği için abonelik kimliği. Kimliği almak için Azure oturumu açın, **daha fazla hizmet** > **abonelikleri**, kullanmak istediğiniz aboneliği tıklatın ve altında **Essentials** bulabilirsiniz **Abonelik kimliği**. Çin, Almanya ve US government abonelikler şu anda desteklenmiyor.
- Kullanıcı adı ve parola abonelik sahibi olan bir hesap için (MSA/2FA hesapları desteklenir).
- Azure Active Directory Azure aboneliği için. Azure portalının sağ üst köşedeki adresindeki, avatar üzerine gelerek, bu dizin Azure içinde bulabilirsiniz. 
- [Azure yığın kaynak sağlayıcısı kayıtlı](#register-azure-stack-resource-provider-in-azure).

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/en-us/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Azure'a Azure yığın kaynak sağlayıcısı kaydetme
> [!NOTE] 
> Bu adım, yalnızca Azure yığın ortamında tamamlandıktan sonra gerekir.
>

1. PowerShell ISE yönetici olarak başlatın.
2. -EnvironmentName parametresi "AzureCloud" ayarlanmış Azure abonelik sahibi Azure hesabı için oturum açın.
3. Azure kaynak sağlayıcısı "Microsoft.AzureStack" kaydedin.

Örnek: 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```


## <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin

> [!NOTE]
>Tüm adımları ana bilgisayara tamamlanması gerekir.
>

1. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md). 
2. Kopya [RegisterWithAzure.psm1 betik](https://go.microsoft.com/fwlink/?linkid=842959) bir klasöre (örneğin, C:\Temp gibi).
3. PowerShell ISE yönetici olarak başlatın ve RegisterWithAzure modülünü içeri aktarın.    
4. Add-AzsRegistration modülü RegisterWithAzure.psm1 komut dosyasından çalıştırın. Aşağıdaki yer tutucularını değiştirin: 
    - *YourCloudAdminCredential* domain\cloudadmin yerel etki alanı kimlik bilgilerini içeren bir PowerShell nesnesi (Geliştirme Seti için azurestack\cloudadmin budur).
    - *YourAzureSubscriptionID* Azure yığın kaydetmek için kullanmak istediğiniz Azure abonelik Kimliğini gösterir.
    - *YourAzureDirectoryTenantName* Azure aboneliğinizle ilişkili Azure Kiracı dizinini adıdır. Kayıt kaynak bu directory kiracısında oluşturulur. 
    - *YourPrivilegedEndpoint* adı [ayrıcalıklı uç noktası](azure-stack-privileged-endpoint.md).

    ```powershell
    Add-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -AzureDirectoryTenantName $YourAzureDirectoryTenantName  -AzureSubscriptionId $YourAzureSubscriptionId -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Development 
    ```
5. Açılır oturum açma penceresine Azure aboneliği kimlik bilgilerinizi girin.

## <a name="verify-the-registration"></a>Kayıt doğrulayın

1. Yönetici portalı'na (https://adminportal.local.azurestack.external) oturum açın.
2. Tıklatın **daha fazla hizmet** > **Market Yönetim** > **Azure'dan eklemek**.
3. Bir öğe (örneğin, WordPress) azure'dan kullanılabilir listesi görürseniz, etkinleştirme başarılı oldu.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydettirmek için etkin uyarı artık görünür.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

