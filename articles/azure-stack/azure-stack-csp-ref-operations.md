---
title: Kiracılar kullanımı Azure yığınında izleme için Kaydet | Microsoft Docs
description: Kiracı kayıtlar ve Kiracı kullanım Azure yığınında nasıl izleneceğini yönetmek için kullanılan işlem hakkında ayrıntılar.
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
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: ef7ca59647a1f8c15d85c809609060a5945bedde
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Kiracı kayıt Azure yığınında yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makale, Kiracı kayıtlar ve Kiracı kullanım nasıl izleneceğini yönetmek için kullanabileceğiniz işlemleri hakkında ayrıntılar içerir. Listesinde, eklemek veya Kiracı eşleştirmelerini kaldırmak hakkında ayrıntılı bilgi bulabilirsiniz. İzleme kullanımınız yönetmek için PowerShell veya faturalama API uç noktaları'nı kullanabilirsiniz.

## <a name="add-tenant-to-registration"></a>Kiracı kayıt ekleme

Böylece kullanıcıların Azure Active Directory (Azure AD) Kiracı ile bağlı bir Azure aboneliği altında kullanımları bildirilen yeni bir kiracı kaydınızı için eklemek istediğiniz zaman bu işlemi kullanın.

Bir kiracı ile ilişkilendirilen abonelik değiştirmek isterseniz bu işlemi de kullanabilirsiniz, PUT/New-AzureRMResource tekrar çağırabilirsiniz. Eski eşleme üzerine yazılır.

Yalnızca bir Azure aboneliği bir kiracı ile ilişkili olabileceğini unutmayın. Mevcut bir kiracı için ikinci bir abonelik eklemeyi denediğinizde, ilk abonelik aşırı yazılı olur. 


| Parametre                  | Açıklama |
|---                         | --- |
| registrationSubscriptionID | İlk kaydı için kullanılan Azure aboneliği. |
| customerSubscriptionID     | Kaydedilecek müşteriye ait Azure aboneliği (Azure yığını değil). Oluşturulmalıdır bulut hizmeti sağlayıcısı (CSP) teklifte. Uygulamada, bu iş ortağı Merkezi'nden anlamına gelir. Bir müşteri birden fazla Kiracı varsa, bu abonelik Azure yığın halinde günlüğe kaydetmek için kullanılan Kiracı oluşturulmuş olması gerekir. |
| kaynak grubu              | Kaydınızı depolandığı Azure kaynak grubunda. |
| registrationName           | Azure yığın kayıt adı. Azure'da depolanan nesne değildir. Genellikle form azurestack-Cloudıd Azure yığın dağıtımınızın bulut kimliği olduğu Cloudıd içinde addır. |

> [!Note]  
> Kiracılar, kullandıkları her Azure yığın ile kayıtlı olması gerekir. Bir kiracı birden fazla Azure yığın kullanıyorsa, ilk kayıtlar her dağıtımın Kiracı aboneliği ile güncelleştirmeniz gerekir.

### <a name="powershell"></a>PowerShell

Kayıt kaynağı güncelleştirmek için yeni AzureRmResource cmdlet'ini kullanın. Azure için oturum açma (`Add-AzureRmAccount`) ilk kaydı için kullanılan hesabı kullanarak. Bir kiracı ekleme konusunda bir örneği burada verilmiştir:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API çağrısı

**İşlem**: YERLEŞTİRME  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 201 oluşturuldu  
**Yanıt gövdesi**: boş  

## <a name="list-all-registered-tenants"></a>Tüm kayıtlı kiracılar listesi

Kayıt için eklenene tüm kiracılar listesini alın.

 > [!Note]  
 > Bir kiracı kayıtlı, bir yanıt almazsınız.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | İlk kaydı için kullanılan Azure aboneliği.   |
| kaynak grubu              | Kaydınızı depolandığı Azure kaynak grubunda.    |
| registrationName           | Azure yığın kayıt adı. Azure'da depolanan nesne değildir. Genellikle biçiminde adıdır **azurestack**-***Cloudıd***, burada ***Cloudıd*** Azure yığın dağıtımınızın bulut kimliğidir.   |

### <a name="powershell"></a>PowerShell

Tüm kayıtlı kiracılar listelemek için Get-AzureRmResovurce cmdlet'ini kullanın. Azure için oturum açma (`Add-AzureRmAccount`) ilk kaydı için kullanılan hesabı kullanarak. Bir kiracı ekleme konusunda bir örneği burada verilmiştir:

```powershell
  Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

ALMA işlemi kullanarak tüm Kiracı eşlemelerini listesini al

**İşlem**: Al  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 200  
**Yanıt gövdesi**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Eşleme Kiracı Kaldır

Bir kayıt eklenmiş olan bir kiracı kaldırabilirsiniz. Kiracı hala kaynakları Azure yığında kullanıyorsa, ilk Azure yığın kaydında kullanılan aboneliğine bunların kullanım ücretlendirilir.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | Kayıt için abonelik kimliği.   |
| kaynak grubu              | Kayıt için kaynak grubu.   |
| registrationName           | Kayıt adı.  |
| customerSubscriptionId     | Müşteri abonelik kimliği  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

Kiracı eşlemeleri silme işlemi kullanarak kaldırabilirsiniz.

**İşlem**: Sil  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 204 İçerik yok  
**Yanıt gövdesi**: boş

## <a name="next-steps"></a>Sonraki adımlar

 - Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](/azure-stack-billing-and-chargeback.md).
