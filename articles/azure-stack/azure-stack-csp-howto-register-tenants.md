---
title: Kiracılar için kullanım ekleyin ve Azure yığınına faturalama | Microsoft Docs
description: Gerekli adımlar Azure bulut hizmeti sağlayıcısı tarafından yönetilen yığın bir son kullanıcı ekleyin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: e982fa2bec3cbc4845ecebb45db76f019e2178ff
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Kiracı kullanımı için ekleyin ve Azure yığınına faturalama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Gerekli olan adımları bu makalede Azure yığın bir bulut hizmeti sağlayıcısı (CSP) tarafından yönetilen son kullanıcı ekleyin. Yeni Kiracı kaynaklarına kullandığında, Azure yığın CSP aboneliklerinin kullanım rapor eder.

CSP'ler, genellikle kendi Azure yığın dağıtımda birden fazla müşteriye (kiracılar) hizmetleri sunar. Kiracılar için Azure yığın kayıt ekleme her bir kiracının kullanım bildirilen ve karşılık gelen CSP abonelik faturalandırılır sağlar. Bu makaledeki adımları tamamlamazsanız Azure yığın ilk kaydında kullanılan aboneliği için Kiracı kullanım ücretlendirilir. Bir son müşteriye kullanımı izleme ve kendi Kiracı yönetmek için Azure yığınına eklemeden önce Azure yığın bir CSP yapılandırmanız gerekir. Adımları ve kaynaklar için bkz: [kullanım ve bir bulut hizmeti sağlayıcısı olarak Azure yığını için fatura yönetme](azure-stack-add-manage-billing-as-a-csp.md).

Aşağıdaki diyagramda bir CSP Azure yığın kullanın ve kullanımı için müşteri izleme ayarlamak için yeni bir müşteri etkinleştirmek için izlemeniz gereken adımlar gösterilmektedir. Son müşteriye ekleyerek, siz de Azure yığınında kaynakları yönetmek için. Kaynaklarını yönetmek için iki seçeneğiniz vardır:

1. Son Müşteri Kiracı korur ve son müşteriye yerel Azure yığın abone için kimlik bilgilerini sağlayın.  
2. Veya son müşteriye yerel olarak aboneliğini ile çalışır ve sahibi izinlerine sahip bir konuk olarak CSP ekleyebilirsiniz.  

**Bir son müşteriye ekleme adımları**

![Kullanımı izleme ve son müşteri hesabı yönetmek için bulut hizmeti sağlayıcısı'nı ayarlama](media\azure-stack-csp-enable-billing-usage-tracking\process-csp-enable-billing.png)

## <a name="create-a-new-customer-in-partner-center"></a>Yeni bir müşteri iş ortağı Merkezi'nde oluşturun

İş ortağı Merkezi'nde müşteri için yeni bir Azure aboneliği oluşturun. Yönergeler için bkz: [yeni bir müşteri eklemek](https://msdn.microsoft.com/partner-center/add-a-new-customer).


##  <a name="create-an-azure-subscription-for-the-end-customer"></a>Son müşteri için bir Azure aboneliği oluşturun

İş ortağı Merkezi'nde müşteri kaydını oluşturduktan sonra ürün kataloğu abonelikleri satmak. Yönergeler için bkz: [oluşturun, askıya alabilir veya aboneliklerini iptal](https://msdn.microsoft.com/partner-center/create-a-new-subscription).

## <a name="create-a-guest-user-in-the-end-customer-directory"></a>Son Müşteri dizinde Konuk kullanıcı oluşturma

Son müşteriye kendi hesabını yönetecekse, kendi dizinde Konuk kullanıcı oluşturmak ve bilgi gönderin. Son kullanıcı daha sonra konuk ekleyin ve Konuk izni yükseltmesine **sahibi** Azure yığın CSP hesabı.
 
## <a name="update-the-registration-with-the-end-customer-subscription"></a>Kayıt son müşteri aboneliğe güncelleştir

Kaydınızı yeni müşteri aboneliğe güncelleştirin. Azure ortak merkezi müşteri kimliği kullanarak Müşteri'nin kullanım raporları. Bu adım, o Müşteri'nin tek tek CSP abonelik altında her müşterinin kullanım bildirilir sağlar. Bu kullanıcı kullanımını izleme ve faturalama çok daha kolay hale getirir.

> [!Note]  
> Bu adımı taşımak için bilmeniz gereken [Azure yığın kayıtlı](azure-stack-register.md).

1. Windows PowerShell ile yükseltilmiş istemi açın ve çalıştırın:  
    `Add-AzureRmAccount`
2. Azure kimlik bilgilerinizi yazın.
3. PowerShell oturumunda çalıştırın:

```powershell
    New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```
### <a name="new-azurermresource-powershell-parameters"></a>AzureRmResource yeni PowerShell parametreleri
| Parametre | Açıklama |
| --- | --- | 
|registrationSubscriptionID | Azure yığın ilk kayıt için kullanılan Azure aboneliği. |
| customerSubscriptionID | Kaydedilecek müşteriye ait Azure aboneliği (Azure yığını değil). Olmalıdır CSP teklif; oluşturulmuş uygulamada, bu iş ortağı Merkezi'nden anlamına gelir. Bir müşteri birden fazla Azure Active Directory Kiracı varsa, bu abonelik Azure yığın halinde günlüğe kaydetmek için kullanılan Kiracı oluşturulmuş olması gerekir.
| kaynak grubu | Kaydınızı depolandığı Azure kaynak grubunda. 
| registrationName | Azure yığın kayıt adı. Azure'da depolanan nesne değildir. | 


> [!Note]  
> Kiracılar, kullandıkları her Azure yığın ile kayıtlı olması gerekir. İki Azure yığın dağıtım varsa ve her ikisi de kullanmak için bir kiracı geçiyor her dağıtımın başlangıç kayıtlar Kiracı aboneliği ile güncelleştirmeniz gerekir.

## <a name="onboard-tenant-to-azure-stack"></a>Azure yığınına yerleşik Kiracı

Azure Hizmetleri Azure yığınında kullanılmak üzere birden çok Azure AD kiracılar kullanıcılardan desteklemek için yığın yapılandırın. Yönergeler için bkz: [etkinleştirmek Azure yığınında çoklu kiracı](azure-stack-enable-multitenancy.md).


## <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Azure yığınında son müşteri Kiracı yerel bir kaynak oluşturun

Azure yığınına yeni müşteri eklemiş olmanız ya da son müşteri Kiracı, Konuk hesap sahibi ayrıcalıklarına sahip etkinleştirilmiş sonra kendi Kiracı kaynak oluşturabilirsiniz doğrulayın. Örneğin, bir yönetici şunları yapabilir [Azure yığın portalı ile Windows sanal makine oluşturmak](user\azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

 - Kayıt işleminde tetiklenen varsa hata iletilerini gözden geçirmek için bkz: [Kiracı kayıt hata iletileri](azure-stack-csp-ref-infrastructure.md#usage-and-billing-error-codes).
 - Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](/azure-stack-billing-and-chargeback.md).
 - Nasıl bir bitiş müşteri, CSP ekleyebilir gözden geçirmek için bunların Azure yığınının Yöneticisi olarak, Kiracı, bkz: [Azure yığın aboneliğinizi yönetmek bir bulut hizmeti sağlayıcısı etkinleştirmek](user\azure-stack-csp-enable-billing-usage-tracking.md).
