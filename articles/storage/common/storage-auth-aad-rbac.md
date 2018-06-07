---
title: Azure Storage kapsayıcıları ve Kuyruklar (Önizleme) erişim haklarını yönetmek için RBAC kullanma | Microsoft Docs
description: Kullanıcılar, gruplar, uygulama hizmet asıl adı veya yönetilen hizmet kimlikleri için Azure Storage verilere erişim için rolleri atamak için rol tabanlı erişim denetimi (RBA) kullanın. Azure depolama kapsayıcıları ve Kuyruklar erişim hakları yerleşik ve özel rol destekler.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: tamram
ms.openlocfilehash: cb77bd4418e105c877202f0f1725350380ea2308
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661012"
---
# <a name="manage-access-rights-to-azure-storage-data-with-rbac-preview"></a>RBAC (Önizleme) ile Azure depolama verilere erişim haklarını yönetme

Azure Active Directory (Azure AD) güvenlikli kaynaklara erişim hakkı yetkilendirir [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure depolama kapsayıcıları veya sıralara erişmek için kullanılan izinler ortak kümelerini kapsayan yerleşik RBAC roller kümesini tanımlar. Ne zaman bir RBAC rolü atanmış bir Azure AD kimlik için kimlik bu kaynaklara erişimi verilir göre belirtilen kapsam. Abonelik, kaynak grubu, depolama hesabı ya da bir bireysel kapsayıcı veya sıra düzeyine erişimi kısıtlanabilir. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanarak Azure Storage kaynakları için erişim hakları atayabilirsiniz. 

Bir Azure AD kimlik kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir veya bu olabilir bir *yönetilen hizmet kimliği*. Bir güvenlik sorumlusu bir kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir. A [yönetilen hizmet kimliği](../../active-directory/managed-service-identity/overview.md) olan Azure sanal makineler, işlev uygulamalarının, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan kimliğini doğrulamak için kullanılan otomatik olarak yönetilen bir kimlik. Azure AD kimlik genel bakış için bkz: [anlamak Azure kimlik çözümleri](https://docs.microsoft.com/en-us/azure/active-directory/understand-azure-identity-solutions).

## <a name="rbac-roles-for-azure-storage"></a>Azure Storage için RBAC rolleri

Azure depolama yerleşik ve özel RBAC rollerini destekler. Bu yerleşik RBAC rolleri Azure AD ile kullanmak için Azure Storage sunar:

- [Depolama Blob veri katkıda bulunan (Önizleme)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Depolama Blob veri Okuyucu (Önizleme)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Depolama sırası veri katkıda bulunan (Önizleme)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Depolama sırası veri Okuyucu (Önizleme)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Hakkında daha fazla bilgi için yerleşik roller Azure Storage için tanımlanan bkz [rol tanımları anlayın](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Kapsayıcılar ve Kuyruklar ile kullanılmak üzere özel roller tanımlayabilir. Daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](https://docs.microsoft.com/azure/role-based-access-control/custom-roles.md). 

> [!IMPORTANT]
> Bu önizleme yalnızca üretim dışı kullanılması amaçlanmıştır. Azure AD tümleştirmesi için Azure Storage genel kullanıma bildirilmiş kadar üretim hizmet düzeyi sözleşmelerine (SLA) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyorsa paylaşılan anahtar yetkilendirme veya SAS belirteci uygulamalarınızı kullanmaya devam. ' Nin önizlemesi hakkında ek bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması beş dakika kadar sürebilir.

## <a name="assign-a-role-to-a-security-principal"></a>Bir güvenlik sorumlusu rol atama

Kapsayıcılar veya sıralara depolama hesabınızdaki izinleri vermek için Azure kimlik RBAC rolü atayın. Depolama hesabı veya belirli bir kapsayıcıya veya kuyruğa rol ataması kapsamını belirleyebilirsiniz. Aşağıdaki tabloda, kapsam bağlı olarak yerleşik rolleri tarafından verilen erişim hakları özetlenmektedir: 

|                                 |     BLOB veri katkıda bulunan                                                 |     BLOB veri okuyucusu                                                |     Sıra veri katkıda bulunan                                  |     Sıra veri okuyucusu                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    Aboneliği kapsamlıdır       |    Tüm kapsayıcılar ve bloblar aboneliğindeki okuma/yazma erişimi       |    Tüm kapsayıcılar ve bloblar aboneliğindeki okuma erişimi       |    Abonelikteki tüm sıraları okuma/yazma erişimi       |    Abonelikteki tüm sıraları okuma erişimi         |
|    Kaynak grubu için kapsamlı     |    Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma/yazma erişimi     |    Tüm kapsayıcıları ve blobları kaynak grubunda okuma erişimi     |    Kaynak grubundaki tüm sıraları okuma/yazma erişimi     |    Kaynak grubundaki tüm sıraları okuma erişimi     |
|    Depolama hesabı için kapsamlı    |    Tüm kapsayıcılar ve bloblar depolama hesabındaki okuma/yazma erişimi    |    Tüm kapsayıcılar ve bloblar depolama hesabındaki okuma erişimi    |    Depolama hesabındaki tüm sıraları okuma/yazma erişimi    |    Depolama hesabındaki tüm sıraları okuma erişimi    |
|    Kapsayıcı/kuyruğuna kapsamı    |    Belirtilen kapsayıcı ve bloblarını okuma/yazma erişimi              |    Belirtilen kapsayıcı ve bloblarını okuma erişimi              |    Belirtilen sıraya okuma/yazma erişimi                  |    Belirtilen sıra okuma erişimi                    |

> [!NOTE]
> Azure Storage hesabınızı sahibi olarak, otomatik olarak verilere erişim izni atanmaz. Açıkça kendiniz RBAC rolü için Azure Storage atamanız gerekir. Aboneliğiniz, kaynak grubu, depolama hesabı veya bir kapsayıcı veya sıra düzeyinde atayabilirsiniz.

Azure depolama işlemleri çağırmak için gereken izinler hakkında daha fazla bilgi için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

Aşağıdaki bölümlerde, depolama hesabına kapsamlı veya tek bir kapsayıcıya kapsamlı bir rol atamasını gösterilmektedir.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Azure Portal'da depolama hesabı için kapsamlı bir rol atayın

Tüm kapsayıcıları veya Azure Portal'da depolama hesabı kuyruklarda erişim verilmesi yerleşik bir rol atamak için:

1. İçinde [Azure portal](https://azure.portal.com/), depolama hesabınıza gidin.
2. Depolama hesabınızı seçin ve ardından **erişim denetimi (IAM)** hesabı için erişim denetimi ayarlarını görüntülemek için. Tıklatın **Ekle** yeni bir rolü eklemek için düğmeyi.

    ![Depolama erişim denetimi ayarlarını gösteren ekran görüntüsü](media/storage-auth-aad-rbac/portal-access-control.png)

3. İçinde **izinleri eklemek** penceresinde, bir Azure AD kimlik atamak için rol seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın. Örneğin, aşağıdaki gösterir görüntü **depolama Blob veri Okuyucu (Önizleme)** rol bir kullanıcıya atanmış.

    ![RBAC rolü atama gösteren ekran görüntüsü](media/storage-auth-aad-rbac/add-rbac-role.png)

4. **Kaydet**’e tıklayın. Rol atanmış kimlik bu rolü altında listelenen görüntülenir. Örneğin, aşağıdaki resimde eklenen kullanıcıları artık depolama hesabındaki tüm blob verilerine okuma izinlerinin olduğunu gösterir.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Bir kapsayıcı veya Azure portalında sıra kapsamına rol atama

Bir kapsayıcı veya kuyruğa kapsamlı yerleşik bir rol atama için adımları benzerdir. Burada gösterilen yordamı bir kapsayıcıya kapsamlı bir rol atar ancak bir sıraya kapsamlı bir rol atamak için aynı adımları izleyin: 

1. İçinde [Azure portal](https://azure.portal.com/), depolama hesabınıza gidin ve görüntülemek **genel bakış** hesabı.
2. BLOB hizmeti altında seçin **Gözat BLOB'lar**. 
3. Bir rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
4. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için.
5. İçinde **izinleri eklemek** penceresinde, bir Azure AD kimlik atamak istediğiniz rolü seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın.
6. **Kaydet**’e tıklayın. Rol atanmış kimlik bu rolü altında listelenen görüntülenir. Örneğin, aşağıdaki resimde eklenen kullanıcı artık adlı kapsayıcı verileri için Okuma izinlerine sahip olduğunu gösterir *örnek kapsayıcı*.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Sonraki Adımlar

- RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi ile çalışmaya başlama](../../role-based-access-control/overview.md).
- Atayın ve RBAC rolü atamalarını Azure PowerShell, Azure CLI veya REST API ile yönetme hakkında bilgi almak için aşağıdaki makalelere bakın:
    - [Rol tabanlı erişim denetimi (RBAC) Azure PowerShell ile yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Rol tabanlı erişim denetimi (RBAC) Azure CLI ile yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [Rol tabanlı erişim denetimi (RBAC) REST API ile yönetme](../../role-based-access-control/role-assignments-rest.md)
- Kapsayıcılar ve depolama uygulamalarınızdaki sıralarından erişim yetkisi vermek öğrenmek için bkz: [Azure depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure depolama ekibi blog gönderisi, Azure AD tümleştirmesi için Azure kapsayıcıları ve Kuyruklar hakkında ek bilgi için bkz: [Azure depolama için Azure Önizleme AD Authentication Duyurusu](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 