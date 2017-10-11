---
title: "Azure Resource Manager istek sınırlarını | Microsoft Docs"
description: "Abonelik sınırları erişildiğinde Azure Resource Manager istekleriyle azaltma kullanmayı açıklar."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2017
ms.author: tomfitz
ms.openlocfilehash: 6d7eeaf460674c3ab98425a5412ffa465b9ffd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="throttling-resource-manager-requests"></a>Resource Manager istekleri azaltma
Her abonelik ve Kiracı, Resource Manager sınırları saatte 15.000 isteklerine okuma ve yazma isteklerinin saatte 1.200 için. Bu sınırlar her Azure Resource Manager örnek için geçerli olur; Her Azure bölgesi birden çok örneği vardır ve Azure Resource Manager için tüm Azure bölgeleri dağıtılır.  Sınırları pratikte, yukarıda listelenen daha etkili bir şekilde çok daha yüksek olacak şekilde bir kullanıcı olarak istekleri genellikle birçok farklı örnekleri tarafından sunulur.

Uygulama veya betik bu sınırları ulaşırsa, istek kısıtlama gerekir. Bu konuda, sınır ulaşmadan sahip kalan istekleri belirleme ve nasıl sınıra ulaştığınızda yanıt gösterir.

Sınırına ulaştığında, HTTP durum kodunu alma **429 çok fazla istek**.

İstek sayısı aboneliğiniz ya da Kiracı kapsamlıdır. Birden çok varsa, aboneliğinizde istekleri yapan eşzamanlı uygulamalar bu uygulamalardan istekler eklenir birlikte kalan isteklerinin sayısını belirlemek için.

Abonelik kapsamlı olanları aboneliğinizde kaynak alma gibi abonelik kimliğinizi geçirme involve gruplar isteklerdir. Kapsamlı Kiracı isteklerini alma geçerli Azure konumları gibi abonelik kimliğinizi dahil etmeyin.

## <a name="remaining-requests"></a>Kalan istekleri
Yanıt Üstbilgileri inceleyerek kalan istek sayısını belirleyebilirsiniz. Her istek için kalan okuma ve yazma isteklerinin sayısı değerlerini içerir. Aşağıdaki tabloda, bu değerleri inceleyebilirsiniz yanıt üstbilgilerini açıklanmaktadır:

| Yanıt üst bilgisi | Açıklama |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-Reads |Kalan abonelik kapsamlı okur |
| x-MS-ratelimit-Remaining-Subscription-Writes |Kalan abonelik kapsamlı Yazar |
| x-MS-ratelimit-Remaining-tenant-Reads |Kalan kapsamlı kiracı okur |
| x-MS-ratelimit-Remaining-tenant-Writes |Kalan kapsamlı Kiracı Yazar |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests |Abonelik, kaynak türü istekleri kalan kapsamlı.<br /><br />Bu üstbilgi değerini yalnızca bir hizmet varsayılan sınır kılınmış döndürülür. Resource Manager abonelik okuma veya yazmaları yerine bu değer ekler. |
| x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read |Abonelik, kaynak türü koleksiyonu isteklerini kalan kapsamlı.<br /><br />Bu üstbilgi değerini yalnızca bir hizmet varsayılan sınır kılınmış döndürülür. Bu değer kalan koleksiyonu isteklerini (liste kaynakları) sayısını sağlar. |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests |Kiracı kaynak türü istekleri kalan kapsamlı.<br /><br />Bu üst yalnızca Kiracı düzeyinde istekleri için eklenir ve yalnızca bir hizmet varsayılan sınır geçersiz kılınmış. Resource Manager Kiracı okuma veya yazmaları yerine bu değer ekler. |
| x-MS-ratelimit-Remaining-tenant-Resource-entities-Read |Kiracı kaynak türü koleksiyonu isteklerini kalan kapsamlı.<br /><br />Bu üst yalnızca Kiracı düzeyinde istekleri için eklenir ve yalnızca bir hizmet varsayılan sınır geçersiz kılınmış. |

## <a name="retrieving-the-header-values"></a>Üstbilgi değerlerini alma
Kod ya da komut dosyasında bu üstbilgi değerlerini alma herhangi bir üstbilgi değeri almaktan farklı değildir. 

Örneğin, **C#**, üstbilgi değeri almak bir **HttpWebResponse** adlı nesne **yanıt** aşağıdaki kod ile:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

İçinde **PowerShell**, bir Invoke-WebRequest işlemden üstbilgi değeri alır.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Veya, size sağlayabilir hata ayıklama, kalan isteklerini görmek istiyorsanız **-Debug** parametresi, **PowerShell** cmdlet'i.

```powershell
Get-AzureRmResourceGroup -Debug
```

Aşağıdaki yanıt değeri de dahil olmak üzere birden fazla değer döndüren:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

İçinde **Azure CLI**, daha ayrıntılı seçeneğini kullanarak üstbilgi değerini alır.

```azurecli
azure group list -vv --json
```

Aşağıdaki nesne dahil olmak üzere birden fazla değer döndüren:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Sonraki istek gönderilmeden önce bekleniyor
İstek sınırına ulaştığında, Resource Manager döndürür **429** HTTP durum kodu ve **yeniden deneme sonrasında** üstbilgi değeri. **Yeniden deneme sonrasında** değeri sonraki istek göndermeden önce uygulamanızın beklemesi gereken saniye (veya uyku) sayısını belirtir. Yeniden deneme değeri geçmeden önce bir isteği gönderirseniz, isteğiniz işlenmedi ve yeni bir yeniden deneme değeri döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* Sınırları ve kotalar hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
* Zaman uyumsuz REST istekleri işleme hakkında bilgi edinmek için [izlemek zaman uyumsuz Azure işlemleri](resource-manager-async-operations.md).
