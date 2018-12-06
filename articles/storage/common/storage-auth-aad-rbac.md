---
title: Kapsayıcılar ve kuyrukları (Önizleme) - Azure depolama için erişim haklarını yönetmek için RBAC kullanma | Microsoft Docs
description: Kullanıcılar, gruplar, uygulama hizmet sorumlularını veya yönetilen hizmet kimlikleri için blob ve kuyruk verilere erişim için rolleri atamak için rol tabanlı erişim denetimi (RBAC) kullanın. Azure Storage kapsayıcıları ve Kuyruklar erişim hakları için yerleşik ve özel rollerin destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: ac62800e81cece61e9f51c496ace2868629a49a1
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960252"
---
# <a name="manage-access-rights-to-azure-blob-and-queue-data-with-rbac-preview"></a>Azure Blob ve kuyruk verisi ile RBAC (Önizleme) için erişim haklarını yönetme

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. Ne zaman bir RBAC rolü atanmış bir Azure AD kimlik için kimlik bu kaynaklara erişimi verilir göre belirtilen kapsam. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanarak Azure Storage kaynakları için erişim hakları atayabilirsiniz. 

Bir kullanıcı, Grup veya uygulama hizmet sorumlusunun bir Azure AD kimlik olabilir veya Azure kaynakları için bir yönetilen kimlik olabilir. Bir güvenlik sorumlusu, kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir. A [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md) olan Azure sanal makineleri, işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan kimliğini doğrulamak için kullanılan otomatik olarak yönetilen bir kimlik. Azure AD'de kimlik genel bakış için bkz. [anlamak Azure kimlik çözümleri](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

Azure depolama, hem yerleşik hem de özel RBAC rollerini destekler. Azure depolama, Azure AD ile kullanmak için bu yerleşik RBAC rolleri sunar:

- [Depolama Blob verileri katkıda bulunan (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Depolama Blob verileri Okuyucu (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Depolama kuyruk verileri katkıda bulunan (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Depolama kuyruk verileri Okuyucu (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Kapsayıcılar ve Kuyruklar ile kullanmak için özel roller de tanımlayabilirsiniz. Daha fazla bilgi için [Azure rol tabanlı erişim denetimi için özel roller oluşturma](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-a-role-to-a-security-principal"></a>Bir güvenlik sorumlusu için rol atama

Kapsayıcıları ve kuyruk depolama hesabınızda izinler vermek için Azure kimlik bir RBAC rolü atayın. Depolama hesabı veya belirli bir kapsayıcı veya kuyruğa rol ataması kapsamını belirleyebilirsiniz. Kapsam bağlı olarak, yerleşik rolleri tarafından verilen erişim hakları aşağıdaki tabloda özetlenmiştir:

|Kapsam|BLOB veri sahibi|BLOB verileri katkıda bulunan|BLOB veri okuyucusu|Kuyruk verileri katkıda bulunan|Kuyruk verileri okuyucu|
|---|---|---|---|---|---|
|Subscrition düzeyi|Tüm kapsayıcılar ve bloblar aboneliği için okuma/yazma erişimi|Tüm kapsayıcılar ve bloblar aboneliği için okuma/yazma erişimi| Tüm kapsayıcılar ve bloblar aboneliği okuma erişimi|Abonelikteki tüm kuyruklar için okuma/yazma erişimi|Abonelikteki tüm kuyrukları okuma erişimi|
|Kaynak grubu düzeyinde|Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma/yazma erişimi|Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma/yazma erişimi|Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma erişimi|Kaynak grubundaki tüm kuyruklar için okuma/yazma erişimi|Kaynak grubundaki tüm kuyrukları okuma erişimi|
|Depolama hesabı düzeyi|Tüm kapsayıcıları ve blobları depolama hesabındaki okuma/yazma erişimi|Tüm kapsayıcıları ve blobları depolama hesabındaki okuma/yazma erişimi|Tüm kapsayıcıları ve blobları depolama hesabındaki okuma erişimi|Depolama hesabındaki tüm kuyruklar için okuma/yazma erişimi|Depolama hesabındaki tüm kuyrukları okuma erişimi|
|Kapsayıcı/kuyruk düzeyi|Belirtilen kapsayıcıya ve bloblarına okuma/yazma erişimi|Belirtilen kapsayıcıya ve bloblarına okuma/yazma erişimi|Belirtilen kapsayıcıya ve bloblarına okuma erişimi|Belirtilen sıra okuma/yazma erişimi|Belirtilen sıra okuma erişimi|

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Aboneliğiniz, kaynak grubu, depolama hesabı veya bir kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.

Azure depolama işlemleri çağırmak için gereken izinler hakkında daha fazla bilgi için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

Aşağıdaki bölümlerde, depolama hesabına kapsamlandırılan veya tek bir kapsayıcıya kapsamlı bir rol atamak gösterilmektedir.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Azure portalında depolama hesabı için kapsamlı bir rol atayın

Tüm kapsayıcıları veya Azure portalında depolama hesabındaki sıralara erişim verme yerleşik bir rol atamak için:

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin.
1. Depolama hesabınızı seçin ve ardından **erişim denetimi (IAM)** hesabı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.

    ![Depolama erişim denetimi ayarları gösteren ekran görüntüsü](media/storage-auth-aad-rbac/portal-access-control.png)

1. Tıklayın **rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
1. İçinde **rol ataması Ekle** penceresinde bir Azure AD kimliğine nasıl atamak için rolü seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın. Örneğin, aşağıdaki gösterir görüntüde **depolama Blob verileri Okuyucu (Önizleme)** bir kullanıcıya atanmış rol.

    ![Bir RBAC rolünün nasıl atanacağını gösteren ekran görüntüsü](media/storage-auth-aad-rbac/add-rbac-role.png)

1. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcı artık depolama hesabındaki tüm blob verilerine Okuma izinlerine sahip olduğunu gösterir.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Bir kapsayıcı veya Azure portalında sıra kapsamı rol atama

> [!IMPORTANT]
> Henüz etkin hiyerarşik ad alanı ile bir hesabı kullanıyorsanız bu işlemi yapamazsınız.

Bir kapsayıcı veya bir sıra kapsamı belirlenmiş, yerleşik bir rol atamak için adımları da buradakilere benzer. Burada gösterilen yordam, bir kapsayıcı için kapsamlı bir rolü atar, ancak bir kuyruk için kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz: 

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin ve görüntüleme **genel bakış** hesabı.
1. Hizmetler altında **Blobları**. 
1. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
1. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.

    ![Ekran görüntüsü kapsayıcı erişim denetimi ayarları](media/storage-auth-aad-rbac/portal-access-control-container.png)
1. Tıklayın **rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
1. İçinde **rol ataması Ekle** penceresinde bir Azure AD kimliğine nasıl atamak istediğiniz rolü seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın.
1. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcı artık adlı kapsayıcıyı verilere Okuma izinleri olmasına gösterilmektedir *örnek kapsayıcı*.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Sonraki adımlar

- RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).
- Atama ve Azure PowerShell, Azure CLI veya REST API ile RBAC rolü atamalarını yönetme konusunda bilgi almak için şu makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Rol tabanlı erişim denetimi (RBAC), Azure CLI ile yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-rest.md)
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [Azure depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure kapsayıcılar ve sıralar için Azure AD tümleştirmesi hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
