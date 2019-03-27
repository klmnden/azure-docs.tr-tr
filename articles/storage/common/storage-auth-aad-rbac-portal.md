---
title: Azure AD RBAC - Azure depolama ile blob ve kuyruk verilere erişim haklarını yönetmek için Azure portalını kullanma | Microsoft Docs
description: Kapsayıcılar ve güvenlik sorumluları kuyruklara erişim atamak için Azure portalında rol tabanlı erişim denetimi (RBAC) kullanın. Azure depolama, Azure AD ile kimlik doğrulaması için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8214ff821bad8a46eb710c8b9506d337715db103
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58450155"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-in-the-azure-portal"></a>Azure blob ve kuyruk verilere RBAC ile Azure portalında erişim izni ver

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama, bir dizi ortak blob veya sıra verilerinize erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri tanımlar. 

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

Bu makalede RBAC rolleri atamak için Azure portalını kullanmayı açıklar. Azure portalında RBAC rolleri atayarak ve depolama kaynaklarınıza erişimi yönetmek için basit bir arabirim sağlar. RBAC rolleri için Azure komut satırı araçları veya Azure Depolama Yönetimi API'lerini kullanarak, blob ve kuyruk kaynaklarını da atayabilirsiniz. Depolama kaynakları için RBAC rolleri hakkında daha fazla bilgi için bkz. [kimlik doğrulama erişim Azure blobları ve Azure Active Directory'yi kullanarak sıralar](storage-auth-aad.md). 

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirle 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC Rolleri Ata

Bir rol ataması için uygun kapsam belirledikten sonra Azure portalında bu kaynağa gidin. Görüntü **erişim denetimi (IAM)** kaynak ayarlarını ve rol atamalarını yönetmek için bu yönergeleri izleyin:

1. Bir Azure AD güvenlik sorumlusu erişim vermek için uygun Azure depolama RBAC rolü atayın.

1. Azure Resource Manager atama [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol kapsayıcılar veya Kuyrukları, Azure AD kimlik bilgilerini kullanarak Azure portalından erişmeye ihtiyaç duyan kullanıcılar. 

Aşağıdaki bölümlerde daha ayrıntılı bir şekilde bu adımların her biri açıklanmaktadır.

### <a name="assign-a-built-in-rbac-role"></a>Yerleşik bir RBAC rolü atayın

Bir rol bir güvenlik sorumlusu atamadan önce verdiğiniz izinleri kapsamını göz önünde bulundurun emin olun. Gözden geçirme [kaynak kapsamını belirle](#determine-resource-scope) bölümü uygun kapsam karar verin.

Burada gösterilen yordam, bir kapsayıcı için kapsamlı bir rolü atar, ancak bir kuyruk için kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz: 

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin ve görüntüleme **genel bakış** hesabı.
1. Hizmetler altında **Blobları**. 
1. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
1. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.

    ![Kapsayıcı erişim denetimi ayarları gösteren ekran görüntüsü](media/storage-auth-aad-rbac-portal/portal-access-control-container.png)

1. Tıklayın **rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
1. İçinde **rol ataması Ekle** penceresinde, atamak istediğiniz Azure depolama rolü seçin. Ardından bu role atamak istediğiniz güvenlik sorumlusu bulmak üzere arama yapın.

    ![Bir RBAC rolü atayın gösteren ekran görüntüsü](media/storage-auth-aad-rbac-portal/add-rbac-role.png)

1. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcı artık adlı kapsayıcıyı verilere Okuma izinleri olmasına gösterilmektedir *örnek kapsayıcı*.

    ![Ekran gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac-portal/container-scoped-role.png)

Depolama hesabı, kaynak grubu veya abonelik kapsamında bir rol atamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Aboneliğiniz, kaynak grubu, depolama hesabı veya bir kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.
> 
> Depolama hesabınızda etkin bir hiyerarşik ad alanı varsa bir kapsayıcı veya sıra kapsamı belirlenmiş bir role atayamazsınız.

### <a name="assign-the-reader-role-for-portal-access"></a>Portal erişimi için okuyucu rolü atama

Azure depolama için yerleşik veya özel bir rol bir güvenlik sorumlusu atadığınızda, depolama hesabınızdaki veriler üzerinde işlem gerçekleştirmek için bu güvenlik sorumlusu izni vermiş olursunuz. Yerleşik **veri okuyucu** rolleri sağlamak için bir kapsayıcı veya sıra, yerleşik sırasında veri okuma izinleri **verileri katkıda bulunan** rolleri, okuma, yazma ve silme izinleri bir kapsayıcı sağlar veya Kuyruk. İzinler için belirtilen kaynak kapsamına alınır.  

Örneğin, atadığınız **depolama Blob verileri katkıda bulunan** Gamze adlı bir kapsayıcı düzeyinde kullanıcı rolüne **örnek kapsayıcı**, ardından Mary verilen okuma, yazma ve silme erişimi tüm BLOB'ları Bu kapsayıcı.

Ancak, bir blob, Azure portalında görüntülemek Mary istiyorsa, ardından **depolama Blob verileri katkıda bulunan** rolü kendisi tarafından görüntülemek için blob portalda gezinmek için yeterli izinlere sağlamaz. Ek portal üzerinden gidin ve orada görünür olan diğer kaynakları görüntülemek için Azure AD izinleri gereklidir.

Azure portalında bloblara erişmek ve bunları ek bir RBAC rolü atamak, kullanıcılarınızın gerekiyorsa [okuyucu](../../role-based-access-control/built-in-roles.md#reader) depolama hesabının veya üzeri düzeyinde bu kullanıcı rolüne. **Okuyucu** depolama hesabı kaynaklarına görüntüleyebilir, ancak bunları değiştirmek için kullanıcılara izin veren bir Azure Resource Manager rol rolüdür. Azure Depolama'daki verilere, ancak hesap yönetimi kaynakları için yalnızca Okuma izinleri sağlamaz.

Atamak için şu adımları izleyin **okuyucu** rolüne bir kullanıcı blobları Azure portalından erişebilmesi için. Bu örnekte, depolama hesabına atama kapsamı:

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin.
1. Seçin **erişim denetimi (IAM)** depolama hesabı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.
1. İçinde **rol ataması Ekle** penceresinde **okuyucu** rol. 
1. Gelen **erişim Ata** alanın, Seç **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
1. Rol atamak istediğiniz güvenlik sorumlusu bulmak üzere arama yapın.
1. Rol ataması kaydedin.

> [!NOTE]
> Okuyucu rolü atama blobları veya Azure portalını kullanarak kuyruk erişmesi gereken kullanıcılar için gereklidir. 

## <a name="next-steps"></a>Sonraki adımlar

- Depolama kaynakları için RBAC rolleri hakkında daha fazla bilgi için bkz. [kimlik doğrulama erişim Azure blobları ve Azure Active Directory'yi kullanarak sıralar](storage-auth-aad.md). 
- RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).
- Atama ve Azure PowerShell, Azure CLI veya REST API ile RBAC rolü atamalarını yönetme konusunda bilgi almak için şu makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Rol tabanlı erişim denetimi (RBAC), Azure CLI ile yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-rest.md)
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [Azure depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
