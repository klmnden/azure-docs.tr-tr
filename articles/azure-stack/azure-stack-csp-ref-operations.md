---
title: Kiracılar kullanımı Azure Stack'te izleme için kaydolun | Microsoft Docs
description: Kiracı kayıtları ve Azure Stack'te Kiracı kullanımının nasıl izleneceğini yönetmek için kullanılan işlem hakkında ayrıntılar.
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
ms.date: 06/08/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: bb46881425398618df54288a9d2e6d65bb03dad4
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059751"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Azure Stack Kiracı kaydı yönetme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede operations kayıtlarınızı Kiracı ve Kiracı kullanımının nasıl izleneceğini yönetmek için kullanabileceğiniz ilgili ayrıntıları içerir. Ekleme, liste, ya da Kiracı eşlemeleri kaldırmak hakkında ayrıntılı bilgi bulabilirsiniz. İzleme kullanımınızı yönetmek için PowerShell veya faturalandırma API'si uç noktaları'nı kullanabilirsiniz.

## <a name="add-tenant-to-registration"></a>Kiracı kayıt ekleme

Böylece kendi Azure Active Directory (Azure AD) kiracısı ile bağlı bir Azure aboneliği altında bildirilen kullanımları kaydınızı için yeni bir kiracı eklemek istediğiniz zaman bu işlemi kullanın.

Bir kiracı ile ilişkili aboneliği değiştirmek istiyorsanız bu işlemi de kullanabilirsiniz, PUT/New-AzureRMResource yeniden çağırabilirsiniz. Eski eşleme üzerine yazılır.

Yalnızca bir Azure aboneliği bir kiracı ile ilişkilendirilmiş olabileceğini unutmayın. İkinci bir abonelik için mevcut bir kiracınız eklemeyi denerseniz, ilk aşırı yazılı aboneliktir. 

### <a name="use-api-profiles"></a>API profillerini kullanma

Bu makalede yer alan cmdlet'ler, PowerShell çalıştırırken bir API profili belirtmenizi gerektirir. API profilleri, bir Azure kaynak sağlayıcıları ve API sürümlerini kümesini temsil eder. Doğru sürüme sahip API ile birden çok Azure Bulutu, örneğin kullanılırken kullanmanıza yardımcı genel Azure ve Azure Stack ile çalışırken. Profilleri, yayın tarihi ile eşleşen bir ada göre belirtilir. Bu makalede ile kullanmanız gerekecektir **2017-09-03** profili.

Azure Stack ve API profilleri hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](user/azure-stack-version-profiles.md). PowerShell ile API profili ile çalışır hakkında yönergeler için bkz: [PowerShell'de Azure Stack için kullanım API sürümü profillerini](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama |
|---                         | --- |
| registrationSubscriptionID | İlk kayıt için kullanılan Azure aboneliği. |
| customerSubscriptionID     | Kaydedilecek müşteriye ait Azure aboneliği (Azure Stack değil). Oluşturulmalıdır bulut hizmeti sağlayıcısı (CSP) teklifte. Uygulamada, bu iş ortağı merkezi üzerinden anlamına gelir. Bir müşteri birden fazla Kiracı varsa, bu abonelik Azure Stack açarken kullanılacak kiracıdaki oluşturulması gerekir. |
| Kaynak grubu              | Kaydınızı depolandığı Azure kaynak grubunda. |
| registrationName           | Azure Stack kayıt adı. Bu, Azure'da depolanan bir nesnedir. Form azurestack-Cloudıd Azure Stack dağıtımınıza bulut kimliği olduğu Cloudıd içinde genellikle adıdır. |

> [!Note]  
> Kiracılar, kullandıkları her Azure Stack ile kayıtlı olması gerekir. Bir kiracı birden fazla Azure Stack kullanıyorsa, Kiracı abonelikle ilk kayıtların her dağıtımın güncelleştirmeniz gerekiyor.

### <a name="powershell"></a>PowerShell

Kayıt kaynağı güncelleştirmek için New-AzureRmResource cmdlet'ini kullanın. Azure'da oturum açın (`Add-AzureRmAccount`) ilk kayıt için kullandığınız hesabı kullanarak. Bir kiracı ekleme konusunda bir örnek aşağıda verilmiştir:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API çağrısı

**İşlem**: YERLEŞTİRME  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 201 oluşturuldu  
**Yanıt gövdesi**: boş  

## <a name="list-all-registered-tenants"></a>Kayıtlı tüm kiracılar listesinde

Bir kaydı için eklenmiş olan tüm kiracılar listesini alın.

 > [!Note]  
 > Kiracı kayıtlı, bir yanıt almaz.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | İlk kayıt için kullanılan Azure aboneliği.   |
| Kaynak grubu              | Kaydınızı depolandığı Azure kaynak grubunda.    |
| registrationName           | Azure Stack kayıt adı. Bu, Azure'da depolanan bir nesnedir. Adı genellikle biçimindedir **azurestack**-***Cloudıd***burada ***Cloudıd*** Azure Stack dağıtımınıza bulut kimliğidir.   |

### <a name="powershell"></a>PowerShell

Kayıtlı tüm kiracılar listelemek için Get-AzureRmResource cmdlet'ini kullanın. Azure'da oturum açın (`Add-AzureRmAccount`) ilk kayıt için kullandığınız hesabı kullanarak. Bir kiracı ekleme konusunda bir örnek aşağıda verilmiştir:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

Tüm Kiracı eşlemeler GET işlemi kullanarak bir listesini alabilirsiniz.

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

## <a name="remove-a-tenant-mapping"></a>Bir kiracı eşlemesi Kaldır

Bir kaydı için eklenmiş olan bir kiracı kaldırabilirsiniz. Söz konusu kiracıyı yine de Azure Stack'te kaynakları kullanıyorsa ilk Azure Stack kaydında kullanılan abonelik kullanımlarını ücretlendirilir.

### <a name="parameters"></a>Parametreler

| Parametre                  | Açıklama          |
|---                         | ---                  |
| registrationSubscriptionId | Kayıt için abonelik kimliği.   |
| Kaynak grubu              | Kayıt için kaynak grubu.   |
| registrationName           | Kayıt adı.  |
| customerSubscriptionId     | Müşteri abonelik kimliği  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API çağrısı

Kiracı eşlemeleri silme işlemini kullanarak kaldırabilirsiniz.

**İşlem**: Sil  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Yanıt**: 204 İçerik yok  
**Yanıt gövdesi**: boş

## <a name="next-steps"></a>Sonraki adımlar

 - Azure yığını kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve faturalandırma Azure Stack'te](/azure-stack-billing-and-chargeback.md).
