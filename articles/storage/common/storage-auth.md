---
title: Azure depolama erişimi yetkilendirme | Microsoft Docs
description: Azure depolama, Azure Active Directory, paylaşılan anahtar kimlik doğrulaması veya paylaşılan erişim imzaları gibi erişim yetkisi vermek için farklı yollar hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 9e8c9755c444ca7b81891f5f83164bc51aa694eb
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147061"
---
# <a name="authorizing-access-to-azure-storage"></a>Azure depolama erişimi yetkilendirme

Depolama hesabınızdaki verilere her zaman istemci Azure Storage HTTP/HTTPS üzerinden bir istek gönderir. Hizmet istemci verilere erişmek için gerekli izinlere sahip olmasını sağlar, böylece her istek için güvenli bir kaynak yetkilendirilmelidir. Azure depolama kaynaklarını güvenli hale getirmek için erişimi yetkilendirmek için bu seçenekleri sunar:

- **Azure Active Directory (Azure AD) Tümleştirmesi** BLOB'lar ve Kuyruklar için. Azure AD rol tabanlı erişim denetimi (RBAC), bir istemcinin erişimi bir depolama hesabındaki kaynaklara üzerinde ayrıntılı denetim sağlar. Daha fazla bilgi için [Azure Active Directory'yi kullanarak Azure Depolama'ya yönelik isteklerin kimlik doğrulaması](storage-auth-aad.md).
- **Paylaşılan anahtar yetkilendirme** bloblar, dosyalar, kuyruklar ve tablolar için. Paylaşılan anahtar kullanan bir istemci, bir depolama hesabı erişim anahtarı kullanılarak imzalandığını her istek üstbilgiyle geçirir. Daha fazla bilgi için [paylaşılan anahtar ile Authorize](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Paylaşılan erişim imzaları** bloblar, dosyalar, kuyruklar ve tablolar için. Paylaşılan erişim imzaları (SAS) depolama hesabınızdaki kaynaklara temsilci erişimi sınırlı sağlar. Kısıtlamaları imzanın geçerli olduğu zaman aralığını ya da erişimi yönetme esneklik sağlar verir izinleri ekleniyor. Daha fazla bilgi için [paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).
- **Anonim genel okuma erişimi** kapsayıcılar ve bloblar için. Yetkilendirme gerekli değildir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).  

Varsayılan olarak, Azure Depolama'daki tüm kaynakları güvenli hale getirilir ve yalnızca hesap sahibi için kullanılabilir. Microsoft, tüm istemciler, depolama hesabınızdaki kaynaklara erişim vermek için yukarıda özetlenen yetkilendirme stratejiler kullanabilirsiniz, ancak önerir Azure AD kullanarak mümkün olduğunda en üst düzey güvenlik ve kullanım kolaylığı. 



