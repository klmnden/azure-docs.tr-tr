---
title: Azure portal - Azure depolama içinde depolama hesabı ayarlarını yönetme | Microsoft Docs
description: Erişim katmanını değiştirme veya hesap tarafından kullanılan çoğaltma türünü değiştirme hesap erişim anahtarlarını yeniden erişim denetimi ayarlarını yapılandırma dahil olmak üzere Azure portalında depolama hesabı ayarlarını yönetmeyi öğrenin. Ayrıca Portalı'nda bir depolama hesabını silmek nasıl öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/05/2019
ms.author: tamram
ms.openlocfilehash: fa574558afeec5a7706482a142c0187e6a34bdb3
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370406"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Azure portalında depolama hesabı ayarlarını yönetme

Çeşitli ayarları depolama hesabınız için kullanılabilir [Azure portalında](https://portal.azure.com). Bu makalede bu ayarlar ve bunları nasıl kullanacağınızı bazılarını açıklar.

## <a name="access-control"></a>Erişim denetimi

Azure depolama, Blob Depolama ve kuyruk depolama ile rol tabanlı erişim denetimi (RBAC) için Azure Active Directory ile kimlik doğrulaması destekler. Azure AD ile kimlik doğrulaması hakkında daha fazla bilgi için bkz. [kimlik doğrulama erişim Azure blobları ve Azure Active Directory'yi kullanarak sıralar](storage-auth-aad.md).

**Erişim denetimi** ayarları Azure portalında kullanıcıları, grupları, hizmet sorumluları ve yönetilen kimlikleri için RBAC rolleri atamak için basit bir yol sunar. RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ile blob ve kuyruk verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

> [!NOTE]
> Kullanıcılar veya uygulamalar Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yetkilendirme başka bir yolla üstün güvenlik ve kullanım kolaylığı sağlar. Paylaşılan anahtar yetkilendirme uygulamalarınızı kullanmaya devam ederken, Azure AD kullanarak kodunuzu ile hesap erişim anahtarını depolamak için gereken bozar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim vermek için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD'ye SAS belirteçlerini yönetin veya güvenliği aşılmış bir SAS iptal etme hakkında endişelenmenize gerek kalmadan benzer özellikleri sunar. 

## <a name="tags"></a>Etiketler

Azure depolama, özelleştirilmiş bir taksonomi kullanarak Azure kaynaklarınızı düzenlemek için Azure Resource Manager etiketleri destekler. Böylece bunları mantıksal bir şekilde aboneliğinizde gruplandırabilirsiniz, depolama hesaplarınıza etiketler ekleyebilirsiniz. 

Depolama hesapları için bir etiket adı 128 karakterle sınırlıdır ve etiket değeri 256 karakterle sınırlıdır.

Daha fazla bilgi için [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Erişim tuşları

Bir depolama hesabı oluşturduğunuzda, Azure, iki adet 512 bit depolama hesabı erişim anahtarlarını oluşturur. Bu anahtarları, depolama hesabınızın paylaşılan anahtar aracılığıyla erişim yetkisi vermek için kullanılabilir. Döndürme ve uygulamalarınızı kesinti olmadan anahtarları yeniden ve Microsoft bunu düzenli olarak yapmak önerir.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="view-and-copy-access-keys"></a>Erişim anahtarlarını görüntüleme ve kopyalama

Depolama hesabınızın kimlik bilgilerini görüntülemek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Depolama hesabınızı bulun.
3. Depolama hesabına genel bakışın **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Hesap erişim anahtarlarınız ve her bir anahtar için tam bağlantı dizesi görüntülenir.
4. **key1** bölümünde **Anahtar** değerini bulun ve **Kopyala** düğmesine tıklayarak hesap anahtarını kopyalayın.
5. Alternatif olarak, tüm bağlantı dizesini kopyalayabilirsiniz. **key1** bölümünde **Bağlantı dizesi** değerini bulun ve **Kopyala** düğmesine tıklayarak bağlantı dizesini kopyalayın.

    ![Azure portalında erişim anahtarlarını görüntüleme gösteren ekran görüntüsü](media/storage-manage-account/portal-connection-string.png)

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
