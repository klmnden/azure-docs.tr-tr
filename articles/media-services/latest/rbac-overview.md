---
title: Media Services hesapları - Azure için rol tabanlı erişim denetimi | Microsoft Docs
description: Bu makalede, Azure Media Services hesapları için rol tabanlı erişim denetimi (RBAC) açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f72e98d8874a5a5dc94deb882affdf66388b13c9
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548534"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Media Services hesapları için rol tabanlı erişim denetimi (RBAC)

Şu anda, Azure Media Services tanımlı herhangi bir özel roller belirli hizmete. Müşteriler, yerleşik rolleri kullanabileceğiniz **sahibi** veya **katkıda bulunan** Media Services hesabına tam erişim elde etmek için. Bu roller arasındaki temel fark: **sahibi** bir kaynağa kimlerin erişebildiğini denetlemenize ve **katkıda bulunan** olamaz. Yerleşik okuyucu hesabı yalnızca okuma erişimi Media Services hesabına sahiptir. 

## <a name="design-principles"></a>Tasarım ilkeleri

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. V3 API'ler döndürmeyen parolaları veya kimlik üzerinde **alma** veya **listesi** operations. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Kullanıcı parolaları veya kimlik bilgilerini almak için ayrı bir eylem yöntemini çağırmak gerekir. **Okuyucu** Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets gibi işlemler çağrılamıyor şekilde rol operations çağrılamıyor. Ayrı Eylemler sahip isterseniz özel bir rol daha ayrıntılı RBAC güvenlik izinleri ayarlamanızı sağlar.

Media Services işlemlerini listelemek için destekler, yapın:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

[Yerleşik rol tanımları](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) makale belirten, tam olarak hangi rolü sağlar. 

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Klasik abonelik yönetici rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [Azure kaynakları için RBAC nedir?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)

## <a name="next-steps"></a>Sonraki adımlar

- [V3 API'ler Media Services ile geliştirme](media-services-apis-overview.md)
- [Media Services .NET kullanarak içerik anahtarı ilkesi alma](get-content-key-policy-dotnet-howto.md)
