---
title: Erişim için Azure BLOB'ları ve kuyrukları (Önizleme) Azure Active Directory'yi kullanarak kimlik doğrulaması | Microsoft Docs
description: Azure BLOB'ları ve kuyrukları Azure Active Directory (Önizleme) kullanarak erişimi kimlik doğrulaması.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/13/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dcf7d237c8cfbf52a804e428d84fff0bb328c7c8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58012112"
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
    - [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [BLOB, kuyruk ve dosya](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="overview-of-azure-ad-for-storage"></a>Depolama için Azure AD genel bakış

Azure depolama ile Azure AD tümleştirmesi kullanılarak ilk adımı hizmet sorumlusu (kullanıcı, Grup veya uygulama hizmet sorumlusu) veya Azure kaynakları için yönetilen kimlikleri için RBAC rolleri için depolama veri atamaktır. RBAC rollerini ortak kapsayıcılar ve Kuyruklar için izin kümesi kapsayabilir. Azure depolama için RBAC rollerini atama hakkında daha fazla bilgi edinmek için [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).

Depolama kaynaklarını uygulamalarınıza erişim yetkisi vermek için Azure AD kullanmak için kodunuz içinden bir OAuth 2.0 erişim belirteci istemeniz gerekir. Bir erişim belirteci istemek ve Azure Depolama'ya yönelik isteklerin yetkilendirmek için kullanmak üzere öğrenmek için bkz: [Azure AD'den bir Azure depolama uygulaması (Önizleme) ile kimlik doğrulama](storage-auth-aad-app.md). Yönetilen bir kimlik kullanıyorsanız bkz [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimlikleri (Önizleme) Azure kaynakları için yönetilen](storage-auth-aad-msi.md).

Azure CLI ve PowerShell artık bir Azure AD kimlik oturum destekler. Bir Azure AD kimlik bilgilerinizle oturum sonra oturumunuz bu kimliği altında çalışır. Daha fazla bilgi için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. 

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md). 

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Azure portalında yerleşik bir rol atama hakkında bilgi edinmek için [Azure kapsayıcıları ve RBAC ile kuyrukları (Önizleme) Azure portalında erişim ver](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>RBAC rolleri tarafından verilen erişim izinleri 

Aşağıdaki tabloda, farklı kapsam düzeylerinde yerleşik rolleri tarafından verilen erişim hakları özetlenmektedir:

|Kapsam|BLOB veri sahibi|BLOB verileri katkıda bulunan|BLOB veri okuyucusu|Kuyruk verileri katkıda bulunan|Kuyruk verileri okuyucu|
|---|---|---|---|---|---|
|Abonelik düzeyi|Denetim Yönetimi tüm kapsayıcılar ve bloblar aboneliği için okuma/yazma erişimi ve POSIX erişim|Tüm kapsayıcılar ve bloblar aboneliği için okuma/yazma erişimi| Tüm kapsayıcılar ve bloblar aboneliği okuma erişimi|Abonelikteki tüm kuyruklar için okuma/yazma erişimi|Abonelikteki tüm kuyrukları okuma erişimi|
|Kaynak grubu düzeyinde|Tüm kapsayıcıları ve blobları kaynak grubunda denetim Yönetimi okuma/yazma erişimi ve POSIX erişim|Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma/yazma erişimi|Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma erişimi|Kaynak grubundaki tüm kuyruklar için okuma/yazma erişimi|Kaynak grubundaki tüm kuyrukları okuma erişimi|
|Depolama hesabı düzeyi|Tüm kapsayıcıları ve BLOB Depolama hesabında denetim Yönetimi okuma/yazma erişimi ve POSIX erişim|Tüm kapsayıcıları ve blobları depolama hesabındaki okuma/yazma erişimi|Tüm kapsayıcıları ve blobları depolama hesabındaki okuma erişimi|Depolama hesabındaki tüm kuyruklar için okuma/yazma erişimi|Depolama hesabındaki tüm kuyrukları okuma erişimi|
|Kapsayıcı/kuyruk düzeyi|Okuma/yazma erişimi ve POSIX erişim denetimi Yönetimi belirtilen kapsayıcıya ve bloblarına.|Belirtilen kapsayıcıya ve bloblarına okuma/yazma erişimi|Belirtilen kapsayıcıya ve bloblarına okuma erişimi|Belirtilen sıra okuma/yazma erişimi|Belirtilen sıra okuma erişimi|

Azure depolama işlemleri çağırmak için gereken izinler hakkında daha fazla bilgi için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="next-steps"></a>Sonraki adımlar

Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
