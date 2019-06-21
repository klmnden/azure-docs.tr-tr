---
title: Azure portal - Azure depolama içinde depolama hesabı ayarlarını yönetme | Microsoft Docs
description: Erişim katmanını değiştirme veya hesap tarafından kullanılan çoğaltma türünü değiştirme hesap erişim anahtarlarını yeniden erişim denetimi ayarlarını yapılandırma dahil olmak üzere Azure portalında depolama hesabı ayarlarını yönetmeyi öğrenin. Ayrıca Portalı'nda bir depolama hesabını silmek nasıl öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 66bdc4bd1e17347419a6eccd7c9532db17b33001
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303488"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Azure portalında depolama hesabı ayarlarını yönetme

Çeşitli ayarları depolama hesabınız için kullanılabilir [Azure portalında](https://portal.azure.com). Bu makalede bu ayarlar ve bunları nasıl kullanacağınızı bazılarını açıklar.

## <a name="access-control"></a>Erişim denetimi

Azure depolama, Blob Depolama ve kuyruk depolama ile rol tabanlı erişim denetimi (RBAC) için Azure Active Directory ile yetkilendirme destekler. Azure AD ile yetkilendirme hakkında daha fazla bilgi için bkz: [erişimi yetkilendirin Azure blobları ve Azure Active Directory'yi kullanarak sıralar](storage-auth-aad.md).

**Erişim denetimi** ayarları Azure portalında kullanıcıları, grupları, hizmet sorumluları ve yönetilen kimlikleri için RBAC rolleri atamak için basit bir yol sunar. RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ile blob ve kuyruk verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

## <a name="tags"></a>Tags

Azure depolama, özelleştirilmiş bir taksonomi kullanarak Azure kaynaklarınızı düzenlemek için Azure Resource Manager etiketleri destekler. Böylece bunları mantıksal bir şekilde aboneliğinizde gruplandırabilirsiniz, depolama hesaplarınıza etiketler ekleyebilirsiniz.

Depolama hesapları için bir etiket adı 128 karakterle sınırlıdır ve etiket değeri 256 karakterle sınırlıdır.

Daha fazla bilgi için [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Erişim anahtarları

Bir depolama hesabı oluşturduğunuzda, Azure, iki adet 512 bit depolama hesabı erişim anahtarlarını oluşturur. Bu anahtarları, depolama hesabınızın paylaşılan anahtar aracılığıyla erişim yetkisi vermek için kullanılabilir. Döndürme ve uygulamalarınızı kesinti olmadan anahtarları yeniden ve Microsoft bunu düzenli olarak yapmak önerir.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

### <a name="view-account-keys-and-connection-string"></a>Hesap anahtarları ve bağlantı dizesini görüntüle

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

Microsoft, düzenli aralıklarla depolama hesabınızın korunmasına yardımcı olmak için erişim anahtarlarınızı yeniden önerir. Böylece, anahtarlarınızın döndürebilirsiniz iki erişim tuşu atanır. Anahtarlarınızı döndürürken, uygulamanızın Azure depolama erişimi süreç boyunca tutar emin olun. 

> [!WARNING]
> Erişim tuşlarınızı yeniden oluşturmak, herhangi bir uygulama veya depolama hesabı anahtarı bağımlı olan Azure hizmetleri etkileyebilir. Depolama hesabına erişmek için hesap anahtarı kullanan tüm istemciler, media services, bulut, masaüstü ve mobil uygulamalar ve Azure depolama için grafik kullanıcı arabirimi uygulamalar aşağıdaki gibi yeni anahtarı kullanacak şekilde güncelleştirilmelidir [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

Bu işlem, depolama hesabı anahtarlarını döndürmek için izleyin:

1. İkincil anahtarı kullanmak için uygulama kodunuzdaki bağlantı dizelerini güncelleştirin.
2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. Üzerinde **erişim anahtarlarını** Azure portalındaki dikey **anahtarı yeniden oluştur1**ve ardından **Evet** yeni bir anahtar oluşturmak istediğinizi onaylamak için.
3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.
4. İkincil erişim tuşunu da aynı şekilde yeniden oluşturun.

## <a name="account-configuration"></a>Hesabı yapılandırma

Bir depolama hesabı oluşturduktan sonra yapılandırmasını değiştirebilirsiniz. Örneğin, nasıl verilerinizi çoğaltılır veya hesabın erişim katmanı sık erişimli'den seyrek erişimliye değiştirmek değiştirebilirsiniz. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin, ardından bulun ve tıklatın **yapılandırma** altında **ayarları** görüntülemek ve/veya hesap yapılandırmasını değiştirmek için.

Depolama hesap yapılandırmasını değiştirme eklenen maliyetlerini neden olabilir. Daha fazla ayrıntı için [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası.

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Artık kullanmadığınız bir depolama hesabını kaldırmak için [Azure portal](https://portal.azure.com)’da depolama hesabına gidin ve **Sil**’e tıklayın. Depolama hesabı silindiğinde, hesaptaki tüm veriler dahil olmak üzere tüm hesap silinir.

> [!WARNING]
> Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.
> 

Bir Azure sanal makinesiyle ilişkili bir depolama hesabını silmeye çalışırsanız, depolama hesabının kullanımda olduğu hakkında bir hata alabilirsiniz. Bu hatayı giderme hakkında yardım için lütfen bkz. [Depolama hesaplarını sildiğinizde sorun giderme](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure depolama hesabına genel bakış](storage-account-overview.md)
- [Depolama hesabı oluşturma](storage-quickstart-create-account.md)
