---
title: Kapsayıcılar ve kuyrukları RBAC (Önizleme) - Azure depolama ile Azure AD erişim haklarını yönetmek için Azure portalını kullanma | Microsoft Docs
description: Kapsayıcılar ve güvenlik sorumluları kuyruklara erişim atamak için Azure portalında rol tabanlı erişim denetimi (RBAC) kullanın. Azure depolama, Azure AD ile kimlik doğrulaması için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 68511d62887cb0463fd8db01cb5c90cbc40ac4cd
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56818995"
---
# <a name="grant-access-to-azure-containers-and-queues-with-rbac-in-the-azure-portal-preview"></a>Azure kapsayıcılar ve RBAC ile kuyrukları (Önizleme) Azure portalında erişim izni ver

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. 

Bir RBAC rolü için bir Azure AD güvenlik sorumlusu atandığında, Azure verir, bir güvenlik sorumlusu için bu kaynaklara erişin. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Bir Azure AD güvenlik sorumlusu olabilir bir kullanıcı, Grup, bir uygulama hizmet sorumlusu veya [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md).

Bu makalede RBAC rolleri atamak için Azure portalını kullanmayı açıklar. Azure portalında RBAC rolleri atayarak ve depolama kaynaklarınıza erişimi yönetmek için basit bir arabirim sağlar. RBAC rolleri için Azure komut satırı araçları veya Azure Depolama Yönetimi API'lerini kullanarak, blob ve kuyruk kaynaklarını da atayabilirsiniz. Depolama kaynakları için RBAC rolleri hakkında daha fazla bilgi için bkz. [kimlik doğrulama erişim Azure blobları ve Azure Active Directory (Önizleme) kullanarak sıralar](storage-auth-aad.md). 

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC rolleri için BLOB'lar ve Kuyruklar

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirle 

Bir RBAC rolü için bir güvenlik sorumlusu atamadan önce güvenlik sorumlusunu olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, her zaman yalnızca olası en dar kapsamdan vermek en iyi olduğunu gerektirir.

Aşağıdaki listede, Azure blob ve kuyruk kaynaklarına erişimi dar kapsamlı başlatma kapsamını sınırlandırabilirsiniz düzeyleri açıklanmaktadır:

- **Tek bir kapsayıcı.** Bu kapsamda bir güvenlik sorumlusu tüm blobların kapsayıcı, hem de kapsayıcı özellikleri ve meta veri erişimi vardır.
- **Tek bir kuyruk.** Bu kapsamda bir güvenlik sorumlusu iletileri kuyruğa yanı sıra özellikleri ve meta verileri erişebilir.
- **Depolama hesabı.** Bu kapsamda bir güvenlik sorumlusu tüm kapsayıcılar ve bunların bloblar veya tüm sıralarının ve iletilerinin erişimi vardır.
- **Kaynak grubu.** Bu kapsamda bir güvenlik sorumlusu kapsayıcıları veya kaynak grubundaki tüm depolama hesaplarını kuyruklarda hepsine erişebilir.
- **Abonelik.** Bu kapsamda bir güvenlik sorumlusu kapsayıcıları veya tüm depolama hesaplarını Abonelikteki kaynak gruplarının tüm kuyruklarda tüm erişebilir.

Bir rol ataması için istenen kapsam belirlediğinizde, Azure portalında ilgili kaynağa gidin. Görüntü **erişim denetimi (IAM)** kaynak ayarlarını ve sonraki bölümleri rol atamalarını yönetmek için yönergeleri izleyin.

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC Rolleri Ata

Azure AD kimlik bilgileriyle blob ve kuyruk kaynaklara erişim izni, aşağıdaki adımları içerir: 

1. Kapsayıcılar veya sıralara erişim vermek için uygun Azure depolama RBAC rolü atayın. İçin okuma erişimi, atama **Blob verileri Okuyucu (Önizleme)** veya **kuyruk verileri Okuyucu (Önizleme)** rol. Okuma, yazma ve silme erişimi atama **Blob verileri katkıda bulunan (Önizleme)** veya **kuyruk verileri katkıda bulunan (Önizleme)** rol. Ayrıca, özel bir rol atayabilirsiniz.

1. Azure Resource Manager atama [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol kapsayıcılar veya Kuyrukları, Azure AD kimlik bilgilerini kullanarak Azure portalından erişmeye ihtiyaç duyan kullanıcılar. 

Aşağıdaki bölümlerde daha ayrıntılı bir şekilde bu adımların her biri açıklanmaktadır.

### <a name="assign-a-built-in-rbac-role"></a>Yerleşik bir RBAC rolü atayın

Bir rol bir güvenlik sorumlusu atamadan önce verdiğiniz izinleri kapsamını göz önünde bulundurun emin olun. Gözden geçirme [kaynak kapsamını belirle](#determine-resource-scope) bölümü uygun kapsam karar verin.

Burada gösterilen yordam, bir kapsayıcı için kapsamlı bir rolü atar, ancak bir kuyruk için kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz: 

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin ve görüntüleme **genel bakış** hesabı.
1. Hizmetler altında **Blobları**. 
1. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
1. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.

    ![Ekran görüntüsü kapsayıcı erişim denetimi ayarları](media/storage-auth-aad-rbac/portal-access-control-container.png)

1. Tıklayın **rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
1. İçinde **rol ataması Ekle** penceresinde, atamak istediğiniz Azure depolama rolü seçin. Ardından bu role atamak istediğiniz güvenlik sorumlusu bulmak üzere arama yapın.

    ![Bir RBAC rolünün nasıl atanacağını gösteren ekran görüntüsü](media/storage-auth-aad-rbac/add-rbac-role.png)

1. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcı artık adlı kapsayıcıyı verilere Okuma izinleri olmasına gösterilmektedir *örnek kapsayıcı*.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/container-scoped-role.png)

Depolama hesabı, kaynak grubu veya abonelik kapsamında bir rol atamak için benzer adımları izleyebilirsiniz.

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Aboneliğiniz, kaynak grubu, depolama hesabı veya bir kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.
> 
> Depolama hesabınızda etkin bir hiyerarşik ad alanı varsa bir kapsayıcı veya sıra kapsamı belirlenmiş bir role atayamazsınız.

### <a name="assign-the-azure-resource-manager-reader-role"></a>Azure Resource Manager okuyucu rolü atama

Azure depolama için yerleşik veya özel bir rol bir güvenlik sorumlusu atadığınızda, depolama hesabınızdaki veriler üzerinde işlem gerçekleştirmek için bu güvenlik sorumlusu izni vermiş olursunuz. Yerleşik **veri okuyucu** rollerini sağlamak için bir kapsayıcı veya sırası, verileri okuma izinleri while yerleşik olarak **verileri katkıda bulunan** rolleri, okuma, yazma ve silme izinleri bir kapsayıcı sağlar veya Kuyruk. İzinler için belirtilen kaynak kapsamına alınır.  

Örneğin, atadığınız **depolama Blob verileri katkıda bulunan (Önizleme)** Gamze adlı bir kapsayıcı düzeyinde kullanıcı rolüne **örnek kapsayıcı**, ardından Mary verilen okuma, yazma ve silme erişimi tüm kapsayıcıdaki bloblara.

Ancak, bir blob, Azure portalında görüntülemek Mary istiyorsa, ardından **depolama Blob verileri katkıda bulunan (Önizleme)** rolü kendisi tarafından görüntülemek için blob portalda gezinmek için yeterli izinlere sağlamaz. Ek portal üzerinden gidin ve orada görünür olan diğer kaynakları görüntülemek için Azure AD izinleri gereklidir.

Azure portalında bloblara erişmek ve bunları ek bir RBAC rolü atamak, kullanıcılarınızın gerekiyorsa [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolüne kullanıcılar. **Okuyucu** depolama hesabı kaynaklarına görüntüleyebilir, ancak bunları değiştirmek için kullanıcılara izin veren bir Azure Resource Manager rol rolüdür. Azure Depolama'daki verilere, ancak hesap yönetimi kaynakları için yalnızca Okuma izinleri sağlamaz.

Atamak için şu adımları izleyin **okuyucu** rol. Bu durumda, kapsayıcıya atama kapsamı:

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin ve görüntüleme **genel bakış** hesabı.
1. Hizmetler altında **Blobları**. 
1. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
1. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.
1. İçinde **rol ataması Ekle** penceresinde **okuyucu** rol. 
1. Gelen **erişim Ata** açılan listesinde, select **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
1. Rol atamak istediğiniz güvenlik sorumlusu bulmak üzere arama yapın.
1. Rol ataması kaydedin.

## <a name="use-azure-ad-credentials-with-the-portal"></a>Portal ile Azure AD kimlik bilgilerini kullanın

Blobları veya Azure portalında Azure AD kimlik bilgilerinizi kullanarak sıralara erişmek için aşağıdaki görüntüde gösterilen Önizleme bağlantıları kullanın:

![Erişim bloblarını veya Portalı'nda Azure AD kimlik bilgileriyle kuyrukları](media/storage-auth-aad-rbac/access-data-azure-ad.png)

Önizleme bağlantıları yerine üretime bağlantıları kullanarak blob veya sıra veri erişimi engelliyorsa, Azure portalında Azure AD kullanmak yerine erişim yetkisi vermek için hesap anahtarını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).
- Atama ve Azure PowerShell, Azure CLI veya REST API ile RBAC rolü atamalarını yönetme konusunda bilgi almak için şu makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Rol tabanlı erişim denetimi (RBAC), Azure CLI ile yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-rest.md)
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [Azure depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure kapsayıcılar ve sıralar için Azure AD tümleştirmesi hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
