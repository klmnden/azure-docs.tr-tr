---
title: Azure Active Directory (Önizleme) kullanarak Azure depolama erişimi kimlik doğrulaması | Microsoft Docs
description: Azure Active Directory (Önizleme) kullanarak Azure depolama erişimi kimlik doğrulaması.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 78df10f3b6062a157e1ec5a057e9f39fc40193e5
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091774"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) kullanarak Azure depolama erişimi kimlik doğrulaması

Azure depolama, Blob ve kuyruk hizmetlerine kimlik doğrulaması ve yetkilendirme ile Azure Active Directory (AD) destekler. Azure AD ile erişim vermek için kullanıcıların, grupların veya uygulama hizmet sorumluları için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. 

Azure AD kullanarak Azure Depolama'ya erişen uygulamalar yetkilendirme üst düzey güvenlik ve kullanım kolaylığı, diğer yetkilendirme seçenekler sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Benzer şekilde, depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar.

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
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (7.1.x-Preview kullanın)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Kuyruk](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="get-started-with-azure-ad-for-storage"></a>Depolama için Azure AD ile çalışmaya başlama

Azure depolama ile Azure AD tümleştirmesi kullanılarak ilk adımı hizmet sorumlusu (kullanıcı, Grup veya uygulama hizmet sorumlusu) veya Azure kaynakları için yönetilen kimlikleri için RBAC rolleri için depolama veri atamaktır. RBAC rollerini ortak kapsayıcılar ve Kuyruklar için izin kümesi kapsayabilir. Azure depolama için RBAC rolleri hakkında daha fazla bilgi edinmek için [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).

Depolama kaynaklarını uygulamalarınıza erişim yetkisi vermek için Azure AD kullanmak için kodunuz içinden bir OAuth 2.0 erişim belirteci istemeniz gerekir. Bir erişim belirteci istemek ve Azure Depolama'ya yönelik isteklerin yetkilendirmek için kullanmak üzere öğrenmek için bkz: [Azure AD'den bir Azure depolama uygulaması (Önizleme) ile kimlik doğrulama](storage-auth-aad-app.md). Yönetilen bir kimlik kullanıyorsanız bkz [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimlikleri (Önizleme) Azure kaynakları için yönetilen](storage-auth-aad-msi.md).

Azure CLI ve PowerShell artık bir Azure AD kimlik oturum destekler. Bir Azure AD kimlik bilgilerinizle oturum sonra oturumunuz bu kimliği altında çalışır. Daha fazla bilgi için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).