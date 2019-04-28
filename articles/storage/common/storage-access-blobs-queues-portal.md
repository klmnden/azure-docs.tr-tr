---
title: Blob erişmek veya sıra veri - Azure depolama için Azure portalını kullanma
description: Ne zaman blob erişmek veya Azure portalını kullanarak kuyruk verileri portalda istekleri Azure Depolama'ya perde yapar. Bu istekler Azure Depolama'ya doğrulanabilir ve Azure AD hesabınızın veya depolama hesabı erişim anahtarı kullanarak yetkili.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/19/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5eba650ac2a052f264d82260e9fc07bf195235da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483869"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Blob veya sıra verilerinize erişmek için Azure portalını kullanma

Blob veya kuyruk verileri kullanarak eriştiğinde [Azure portalında](https://portal.azure.com), portal istekleri Azure Depolama'ya perde gönderir. Bu istekler Azure Depolama'ya doğrulanabilir ve Azure AD hesabınızın veya depolama hesabı erişim anahtarı kullanarak yetkili. Portal ve uygun izinlere sahip değilse, iki tür arasında geçiş yapmanıza olanak tanır. hangi kimlik doğrulama yöntemini belirtir.  

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Blob veya sıra verilerinize erişmek için gereken izinler

Azure portalında blob veya sıra verilerinize erişmek için kimliğini doğrulamak istediğiniz bağlı olarak, belirli izinlere ihtiyacınız olacak. Çoğu durumda, bu izinleri rol tabanlı erişim denetimi (RBAC) sağlanır. RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).

### <a name="account-access-key"></a>Hesap erişim anahtarı

Hesap erişim anahtarı ile BLOB ve kuyruk verilere erişmek için RBAC eylemi içeren bir RBAC rolü atanmış olmalıdır **Microsoft.Storage/storageAccounts/listkeys/action**. Bu RBAC rolü, yerleşik veya özel bir rol olabilir. Destekleyen yerleşik roller **Microsoft.Storage/storageAccounts/listkeys/action** içerir:

- Azure Resource Manager [sahibi](../../role-based-access-control/built-in-roles.md#owner) rolü
- Azure Resource Manager [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rolü
- [Depolama hesabı Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rolü

Azure portalında blob veya kuyruğa veri erişmeyi denediğinde, portal ilk, sahip bir role atanmış olup olmadığını denetler **Microsoft.Storage/storageAccounts/listkeys/action**. Bu eylemle bir rolü atanmışsa, portal blob ve kuyruk verilere erişmek için hesap anahtarı kullanır. Bu eylemle bir rolü atanmamış, portalı kullanarak Azure AD hesabınızın verilere erişmeye çalışır.

> [!NOTE]
> Klasik Abonelik Yöneticisi rolleri Hizmet Yöneticisi ve ortak yönetici Azure Resource Manager'ın eşdeğer dahil [sahibi](../../role-based-access-control/built-in-roles.md#owner) rol. **Sahibi** rolü dahil olmak üzere tüm eylemleri içerir **Microsoft.Storage/storageAccounts/listkeys/action**, bu yönetici rollerinden biri olan bir kullanıcı da blob ve kuyruk verilerle erişebilirsiniz Hesap anahtarı. Daha fazla bilgi için [Klasik Abonelik Yöneticisi rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="azure-ad-account"></a>Azure AD hesabı

Azure portalından Azure AD hesabınızla BLOB veya kuyruğa verilere erişmek için aşağıdaki deyimleri hem de sizin için doğru olması gerekir:

- Azure Resource Manager size atandı [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol, en azından, depolama hesabının düzeyine kapsamlı ya da daha yüksek. **Okuyucu** rolü en kısıtlı izinler verir, ancak depolama hesabı yönetim kaynaklarına erişim veren başka bir Azure Resource Manager rol de kullanılabilir.
- Ya da blob veya kuyruğa verilere erişim sağlayan yerleşik veya özel bir rolü atandı.

**Okuyucu** rol ataması veya başka bir Azure Resource Manager rol ataması gereklidir ve böylece kullanıcı görüntüleyebilir ve Azure portalında depolama hesabı yönetim kaynaklarına gidebilirsiniz. Blob veya kuyruğa verilerine erişim izni RBAC rollerini depolama hesabı yönetim kaynaklarına erişimi veremez. Portalı'nda BLOB veya kuyruğa verilerine erişmek için kullanıcının depolama hesabı kaynaklarına gitmek için izinleri olmalıdır. Bu gereksinim hakkında daha fazla bilgi için bkz. [portal erişimi için okuyucu rolü atamak](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Blob veya sıra verilerinize erişimi destekleyen yerleşik roller şunlardır:

- [Depolama Blob verileri sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Data Lake depolama Gen2 için POSIX erişim denetimi için.
- [Depolama Blob verileri katkıda bulunan](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Blobları için okuma/yazma/silme izinleri.
- [Depolama Blob verileri okuyucu](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Bloblar için salt okunur izinler.
- [Depolama kuyruk verileri katkıda bulunan](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Kuyruklar için okuma/yazma/silme izinleri.
- [Depolama kuyruk verileri okuyucu](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): Kuyruklar için yalnızca okuma izni.
    
Özel roller yerleşik rolleri tarafından sağlanan aynı izinleri farklı birleşimleri destekler. Özel bir RBAC rollerini oluşturma hakkında daha fazla bilgi için bkz. [Azure kaynakları için özel roller](../../role-based-access-control/custom-roles.md) ve [Azure kaynakları için rol tanımlarını anlamak](../../role-based-access-control/role-definitions.md).

> [!NOTE]
> Klasik Abonelik Yöneticisi rolüne sahip kuyrukları listeleme desteklenmiyor. Listelemek için kuyruklar, bir kullanıcı atamış olduğunuz bunlara Azure Resource Manager **okuyucu** rolü **depolama kuyruk verileri okuyucu** rolü veya **depolama kuyruk verileri katkıda bulunan** rolü.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>BLOB'ları veya sıralara portalında gidin

Portalda BLOB veya kuyruk verileri görüntülemek için gidin **genel bakış** için bağlantıları tıklatın ve depolama hesabı için **Blobları** veya **kuyrukları**. Alternatif olarak gidebilirsiniz **Blob hizmeti** ve **kuyruk hizmeti** menüsünde bölümler. 

![Azure portalında blob veya kuyruğa veri gidin](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Geçerli kimlik doğrulama yöntemini belirleyin

Bir kapsayıcı veya bir kuyruğa gittiğinizde, Azure portalında hesabı erişim anahtarı veya Azure AD hesabınızın kimliğini doğrulamak için kullanmakta olduğunuz olup olmadığını gösterir.

Bu bölümdeki örneklerde kapsayıcı ve bloblarını erişme gösterir ancak, bir kuyruk ve iletileri erişme veya Kuyrukları listeleme portalını aynı ileti görüntüler.

### <a name="account-access-key"></a>Hesap erişim anahtarı

Hesap erişim anahtarı kullanarak kimlik doğrulama yapıyorsunuz, göreceğiniz **erişim anahtarı** Portalı'nda kimlik doğrulama yöntemi olarak belirtilen:

![Şu anda hesap anahtarı ile kapsayıcı verilerine erişme](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Azure AD hesabı kullanmaya geçmek, görüntüde vurgulanan bağlantıya tıklayın. Size atanan RBAC rolleri aracılığıyla uygun izinlere sahip değilse, devam etmek mümkün olacaktır. Ancak, doğru izinlere sahip değilse, aşağıdakine benzer bir hata iletisi görürsünüz:

![Azure AD hesabı erişimini desteklemeyen, gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Azure AD hesabınızın bunları görüntülemek için izinlere sahip değilse BLOB listede göründüğüne dikkat edin. Tıklayarak **anahtara erişim anahtarı** erişim anahtarı yeniden kimlik doğrulaması için kullanılacak bağlantı.

### <a name="azure-ad-account"></a>Azure AD hesabı

Azure AD hesabınızla kimlik doğrulama yapıyorsunuz, göreceğiniz **Azure AD kullanıcı hesabını** Portalı'nda kimlik doğrulama yöntemi olarak belirtilen:

![Şu anda Azure AD hesabı ile kapsayıcı verilere erişme](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Hesap erişim anahtarı kullanmaya geçmek, görüntüde vurgulanan bağlantıya tıklayın. Ardından hesap anahtarı erişiminiz varsa, devam etmek mümkün olacaktır. Ancak, erişimi hesap anahtarına alınamadığından, aşağıdakine benzer bir hata iletisi görürsünüz:

![Hesap anahtarı erişiminiz yok, gösterilen hata](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Hesabı anahtarlarına erişimi yoksa BLOB listede göründüğüne dikkat edin. Tıklayarak **geçiş yapmak için Azure AD kullanıcı hesabını** Azure AD hesabınızın yeniden kimlik doğrulaması için kullanılacak bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim için Azure BLOB'ları ve kuyrukları Azure Active Directory'yi kullanarak kimlik doğrulaması](storage-auth-aad.md)
- [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim izni ver](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-cli.md)
- [PowerShell ile RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-powershell.md)
