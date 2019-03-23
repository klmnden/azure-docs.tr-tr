---
title: Erişim için Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması | Microsoft Docs
description: Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması yapmak.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ff543b7275ab05a83b1be1d156cbc6059a3b5430
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369906"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Erişim için Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması

Azure depolama, Blob ve kuyruk hizmetlerine kimlik doğrulaması ve yetkilendirme ile Azure Active Directory (AD) destekler. Azure AD ile erişim vermek için kullanıcıların, grupların veya uygulama hizmet sorumluları için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. 

Kullanıcılar veya uygulamalar Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yetkilendirme başka bir yolla üstün güvenlik ve kullanım kolaylığı sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. Microsoft Azure depolama uygulamalarınız için mümkün olduğunda Azure AD kimlik doğrulaması kullanmanızı önerir.

Kimlik doğrulaması ve Azure AD kimlik bilgileriyle yetkilendirme tüm genel amaçlı v2, genel amaçlı v1 ve Blob Depolama hesapları tüm ortak bölgelerde kullanılabilir. Depolama hesapları yalnızca Azure Resource Manager dağıtım modeli desteği ile Azure AD yetkilendirme oluşturuldu.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için Azure AD genel bakış

Azure depolama ile Azure AD tümleştirmesi kullanılarak ilk adımı hizmet sorumlusu (kullanıcı, Grup veya uygulama hizmet sorumlusu) veya Azure kaynakları için yönetilen kimlikleri için RBAC rolleri için depolama veri atamaktır. RBAC rollerini ortak kapsayıcılar ve Kuyruklar için izin kümesi kapsayabilir. Azure depolama için RBAC rollerini atama hakkında daha fazla bilgi edinmek için [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

Depolama kaynaklarını uygulamalarınıza erişim yetkisi vermek için Azure AD kullanmak için kodunuz içinden bir OAuth 2.0 erişim belirteci istemeniz gerekir. Bir erişim belirteci istemek ve Azure Depolama'ya yönelik isteklerin yetkilendirmek için kullanmak üzere öğrenmek için bkz: [Azure AD'den bir Azure depolama uygulaması ile kimlik doğrulama](storage-auth-aad-app.md). Yönetilen bir kimlik kullanıyorsanız bkz [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimliklerini Azure kaynakları için yönetilen](storage-auth-aad-msi.md).

Azure CLI ve PowerShell artık bir Azure AD kimlik oturum destekler. Bir Azure AD kimlik bilgilerinizle oturum sonra oturumunuz bu kimliği altında çalışır. Daha fazla bilgi için bkz. [CLI veya PowerShell ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. 

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Azure portalında yerleşik bir rol atama hakkında bilgi edinmek için [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim verin](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>RBAC rolleri tarafından verilen erişim izinleri 

Azure depolama işlemleri çağırmak için gereken izinler hakkında daha fazla bilgi için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim izni ver](storage-auth-aad-rbac.md)
- [Bloblara ve kuyruklara erişmek için bir uygulamadan Azure Active Directory ile kimlik doğrulaması yapma](storage-auth-aad-app.md)
- [Azure kaynakları için BLOB'ları ve kuyrukları yönetilen kimliklerle erişimi kimlik doğrulaması](storage-auth-aad-msi.md)
- [CLI veya PowerShell ile Azure depolamaya erişmek için bir Azure AD kimliğini kullanın.](storage-auth-aad-script.md)