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
ms.date: 11/15/2017
ms.author: erikje
ms.openlocfilehash: 977630741b8424c4c6bd5f5d492e33b9981b9cb5
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
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

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/en-us/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Azure'a Azure yığın kaynak sağlayıcısı kaydetme
> [!NOTE] 
> Bu adım yalnızca bir kez Azure yığın ortamında tamamlanmalıdır.
>

1. Bir Powershell oturumu yönetici olarak başlatın.
2. (Login-AzureRmAccount cmdlet'i, oturum açtığınızda "AzureCloud" - EnvironmentName parametresi ayarladığınızdan emin olun ve oturum açma için kullanabileceğiniz) Azure abonelik sahibi Azure hesabı için oturum açın.
3. Kayıt Azure kaynak sağlayıcısı "Microsoft.AzureStack."

**Örnek:** 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```

## <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin

> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir makineden çalıştırmanız gerekir. Azure yığın Geliştirme Seti için ana bilgisayar olacaktır. Tümleşik bir sistem kullanıyorsanız, Azure yığın operatörünüze başvurun.
>

1. Bir yönetici olarak bir PowerShell konsolu açın ve [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).  

2. Azure yığın kaydetmek için kullanacağı Azure hesabı ekleyin. Bunu yapmak için çalıştırın `Add-AzureRmAccount` cmdlet parametre olmadan. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 öğeli kimlik doğrulama kullanmak zorunda kalabilirsiniz.  

3. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Varolan kayda karşılık gelen Powershell modülleri sürümlerini silecek ve [Github'dan en son sürümünü indirme](azure-stack-powershell-download.md).  

5. Önceki adımda oluşturduğunuz "AzureStack araçları yönetici" dizininden "Kayıt" klasörüne gidin ve ".\RegisterWithAzure.psm1" modülünü içeri aktarın:  

   ```powershell 
   Import-Module .\RegisterWithAzure.psm1 
   ```

6. Aynı PowerShell oturumunda, aşağıdaki komut dosyasını çalıştırın. Kimlik bilgileri istendiğinde belirtin `azurestack\cloudadmin` kullanıcıyı ve parolayı yerel yönetici için dağıtım sırasında kullandığınız ile aynıdır.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Add-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -AzureSubscriptionId $AzureContext.Subscription.Id `
       -AzureDirectoryTenantName $AzureContext.Tenant.TenantId `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development 
   ```

   | Parametre | Açıklama |
   | -------- | ------------- |
   | CloudAdminCredential | İçin kullanılan bulut etki alanı kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı biçimindedir "\<Azure yığın etki alanı\>\cloudadmin". Geliştirme Seti için kullanıcı adı "azurestack\cloudadmin" olarak ayarlanır. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun.|
   | Azuresubscriptionıd | Azure aboneliği Azure yığın kaydetmek için kullanın.|
   | AzureDirectoryTenantName | Azure aboneliğinizle ilişkili Azure Kiracı dizinin adı. Kayıt kaynak bu directory kiracısında oluşturulur. |
   | PrivilegedEndpoint | Dağıtım görevleri günlük toplama ve diğer posta gibi özellikleriyle sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Geliştirme Seti için ayrıcalıklı endpoint "AzS-ERCS01" sanal makine üzerinde barındırılır. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun. Daha fazla bilgi edinmek için bkz [kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md) konu.|
   | BillingModel | Aboneliğinizi kullanan faturalama modeli. İzin verilen değerler bu parametre are-"Kapasitesi", "PayAsYouUse" ve "Geliştirme". Geliştirme Seti için bu değer "Geliştirme" ayarlanır. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun. |

7. Komut tamamlandığında bir ileti görür "etkinleştirme Azure yığın (Bu adımı tamamlamak için 10 dakika sürebilir)." 

## <a name="verify-the-registration"></a>Kayıt doğrulayın

1. Yönetici portalı'na (https://adminportal.local.azurestack.external) oturum açın.
2. Tıklatın **daha fazla hizmet** > **Market Yönetim** > **Azure'dan eklemek**.
3. Bir öğe (örneğin, WordPress) azure'dan kullanılabilir listesi görürseniz, etkinleştirme başarılı oldu.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydettirmek için etkin uyarı artık görünür.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

