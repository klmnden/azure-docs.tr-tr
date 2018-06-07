---
title: Azure Storage erişimi yetkilendirme | Microsoft Docs
description: Azure Storage, Azure Active Directory, paylaşılan anahtar kimlik doğrulaması veya paylaşılan erişim imzaları gibi erişim yetkisi vermek için farklı yollar hakkında bilgi edinin.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 46bb332e28d7503e543ca3c99fa1099ef17f34c3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660994"
---
# <a name="authorizing-access-to-azure-storage"></a>Azure Storage erişimi yetkilendirme

Depolama hesabınızdaki verilere her zaman istemci HTTP/HTTPS üzerinden Azure depolama birimine istekte bulunur. Hizmet istemci verilere erişmek için gerekli izinlere sahip olmasını sağlar, böylece her istek için güvenli bir kaynak yetkilendirilmelidir. Azure Storage kaynakları güvenli hale getirmek için erişimi yetkilendirmek için bu seçenekleri sunar:

- **Azure Active Directory (Azure AD) Tümleştirmesi (Önizleme)** bloblar ve kuyruklarda olduğu için. Azure AD, bir istemcinin bir depolama hesabındaki kaynaklara erişim üzerinde ayrıntılı denetim için rol tabanlı erişim denetimi (RBAC) sağlar. Daha fazla bilgi için bkz: [Azure Active Directory (Önizleme) kullanarak Azure depolama isteklerine kimlik doğrulaması](storage-auth-aad.md).
- **Anahtar yetkilendirme paylaşılan** BLOB'lar, dosyalar, kuyruklar ve tablolar. Paylaşılan anahtar kullanan bir istemci, depolama hesabının erişim anahtarı ile imzalanmış her istek sahip bir üstbilgi geçirir. Daha fazla bilgi için bkz: [Authorize paylaşılan anahtar ile](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key/).
- **Paylaşılan erişim imzası** BLOB'lar, dosyalar, kuyruklar ve tablolar. Paylaşılan erişim imzaları (SAS) depolama hesabındaki kaynaklara Kısıtlı temsilci erişim sağlar. İmza geçerli olduğu zaman aralığını ya da erişimi yönetme esneklik sağlar verir izinleri kısıtlamaları ekleniyor. Daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Anonim ortak okuma erişiminin** kapsayıcılar ve bloblar için. Yetkilendirme gerekli değildir. Daha fazla bilgi için bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).  

Varsayılan olarak, tüm Azure Storage kaynakları güvenli hale getirilir ve yalnızca hesap sahibi için kullanılabilir. İstemcileri depolama hesabınızdaki kaynaklara erişim vermek için yukarıda özetlenen yetkilendirme stratejiler birini kullanmanız mümkün olmakla birlikte, Microsoft önerir Azure AD kullanarak mümkün olduğunda en büyük güvenlik ve kullanım kolaylığı. 



