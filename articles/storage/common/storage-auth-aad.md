---
title: Erişim için Azure BLOB'ları ve kuyrukları (Önizleme) Azure Active Directory'yi kullanarak kimlik doğrulaması | Microsoft Docs
description: Azure BLOB'ları ve kuyrukları Azure Active Directory (Önizleme) kullanarak erişimi kimlik doğrulaması.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: a918a44042df13c8b799d823656c4cd878f8f618
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426660"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>Erişim için Azure BLOB'ları ve kuyrukları (Önizleme) Azure Active Directory'yi kullanarak kimlik doğrulaması

Azure depolama, Blob ve kuyruk hizmetlerine kimlik doğrulaması ve yetkilendirme ile Azure Active Directory (AD) destekler. Azure AD ile erişim vermek için kullanıcıların, grupların veya uygulama hizmet sorumluları için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. 

Kullanıcılar veya uygulamalar Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yetkilendirme başka bir yolla üstün güvenlik ve kullanım kolaylığı sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. Microsoft Azure depolama uygulamalarınız için mümkün olduğunda Azure AD kimlik doğrulaması kullanmanızı önerir.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="about-the-preview"></a>Önizleme hakkında

Önizleme hakkında aşağıdaki noktalara dikkat edin:

- Azure AD tümleştirmesi, Blob ve kuyruk hizmetlerine yalnızca Önizleme'de kullanılabilir.
- Azure AD tümleştirmesi, tüm genel bölgelerde GPv1, GPv2 ve Blob Depolama hesapları için kullanılabilir. 
- Yalnızca Resource Manager dağıtım modeliyle oluşturulan depolama hesapları desteklenmektedir. 
- Azure depolama analizi günlük arayan kimlik bilgileri için destek yakında sunulacaktır.
- Azure AD yetkilendirme standart depolama hesaplarında kaynaklara erişim şu anda desteklenmiyor. Premium depolama hesaplarındaki sayfa blobları için erişim yetkilendirme aktarılması yakında desteklenecek.
- Azure depolama, hem yerleşik hem de özel RBAC rollerini destekler. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya sıra kapsamı roller atayabilirsiniz.
- Azure AD tümleştirme desteklemekte Azure depolama istemci kitaplıkları şunları içerir:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [BLOB, kuyruk ve dosya](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="get-started-with-azure-ad-for-storage"></a>Depolama için Azure AD ile çalışmaya başlama

Azure depolama ile Azure AD tümleştirmesi kullanılarak ilk adımı hizmet sorumlusu (kullanıcı, Grup veya uygulama hizmet sorumlusu) veya Azure kaynakları için yönetilen kimlikleri için RBAC rolleri için depolama veri atamaktır. RBAC rollerini ortak kapsayıcılar ve Kuyruklar için izin kümesi kapsayabilir. Azure depolama için RBAC rolleri hakkında daha fazla bilgi edinmek için [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).

Depolama kaynaklarını uygulamalarınıza erişim yetkisi vermek için Azure AD kullanmak için kodunuz içinden bir OAuth 2.0 erişim belirteci istemeniz gerekir. Bir erişim belirteci istemek ve Azure Depolama'ya yönelik isteklerin yetkilendirmek için kullanmak üzere öğrenmek için bkz: [Azure AD'den bir Azure depolama uygulaması (Önizleme) ile kimlik doğrulama](storage-auth-aad-app.md). Yönetilen bir kimlik kullanıyorsanız bkz [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimlikleri (Önizleme) Azure kaynakları için yönetilen](storage-auth-aad-msi.md).

Azure CLI ve PowerShell artık bir Azure AD kimlik oturum destekler. Bir Azure AD kimlik bilgilerinizle oturum sonra oturumunuz bu kimliği altında çalışır. Daha fazla bilgi için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
