---
title: "Rest verileri için Azure Storage hizmeti şifreleme | Microsoft Docs"
description: "Veri alınırken şifresini çözmek ve Azure depolama hizmeti şifrelemesi özelliği hizmet tarafında Azure Blob Depolama veri depolarken şifrelemek için kullanın."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 6b56cbb4220ce1c8767724938dd531b8ae5c3920
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Bekleyen Veri için Azure Storage Hizmeti Şifreleme

Rest verileri için Azure depolama hizmeti şifrelemesi, kuruluş güvenlik ve uyumluluk taahhüt karşılamak için verilerinizi korumanıza yardımcı olur. Bu özellik ile Azure depolama, verilerinizi otomatik olarak şifreler önce Azure depolama alanına kalıcı yapma ve alma önce verilerin şifresini çözer. Şifreleme, rest, şifre çözme ve depolama hizmeti şifrelemesi, anahtar yönetimi şifreleme işlenmesini kullanıcılar için saydamdır. Tüm verileri Azure depolama alanına yazılır, 256 bit şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

Depolama hizmeti şifrelemesi, tüm yeni ve var olan depolama hesapları için etkin ve devre dışı bırakılamaz. Varsayılan olarak, verilerinizin güvenliği için kod veya depolama hizmeti şifrelemesi yararlanmak için uygulamaları değişiklik gerekmez.

Bu özellik, verileri otomatik olarak şifreler:

- Her iki performans katmanı olduğu (standart ve Premium).
- Her iki dağıtım modeline olduğu (Azure Resource Manager ve klasik).
- Tüm Azure Storage (Blob storage, kuyruk depolama, Table storage ve Azure dosyaları) Hizmetleri. 

Depolama hizmeti şifrelemesi, Azure depolama performansını etkilemez.

Microsoft tarafından yönetilen şifreleme anahtarları depolama hizmeti şifrelemesi ile kullanabilir veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Kendi anahtarları kullanma hakkında daha fazla bilgi için bkz: [depolama hizmeti şifrelemesi müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Azure portalında görünümü şifreleme ayarları

Depolama hizmeti şifrelemesi ayarlarını görüntülemek için oturum açın [Azure portal](https://portal.azure.com) ve bir depolama hesabı seçin. İçinde **ayarları** bölmesinde, **şifreleme** ayarı.

![Şifreleme ayarı gösteren portal ekran görüntüsü](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Depolama hizmeti şifrelemesi hakkında SSS

**S: Klasik depolama hesabı sahibim. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**

Y: depolama hizmeti şifrelemesi, tüm depolama hesapları için varsayılan olarak etkindir (Klasik ve Resource Manager).

**S: nasıl ı veri Klasik depolama Hesabımı şifreleyebilir mi?**

A: varsayılan olarak etkin şifreleme ile Azure depolama, yeni verileri otomatik olarak şifreler. 

**S: bir Resource Manager depolama hesabı sahibim. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**

Y: depolama hizmeti şifrelemesi, var olan tüm Resource Manager depolama hesapları üzerinde varsayılan olarak etkindir. Bu Blob storage, Table storage, kuyruk depolama ve Azure dosyaları için desteklenir. 

**S: nasıl Resource Manager depolama hesabı verileri şifreliyor mu?**

Y: depolama hizmeti şifrelemesi, tüm depolama hesaplarının--Klasik varsayılan olarak etkinleştirilir ve Resource Manager. Ancak, varolan veriler şifrelenmez. Var olan verileri şifrelemek için başka bir ad veya başka bir kapsayıcı kopyalayın ve ardından şifrelenmemiş sürümlerini kaldırın. 

**S: depolama hesaplarını Azure PowerShell ve Azure CLI kullanarak etkin depolama hizmeti şifrelemesi ile oluşturabilirim?**

Y: depolama hizmeti şifrelemesi, herhangi bir depolama hesabı oluşturma sırasında varsayılan olarak etkindir (Klasik veya Resource Manager). Hesap özelliklerini Azure PowerShell ve Azure CLI kullanarak doğrulayabilirsiniz.

**S: depolama hizmeti şifrelemesi etkinse, daha fazlasını nasıl Azure depolama maliyeti?**

Y: yoktur ek ücret ödemeden.

**S: şifreleme anahtarları yöneten?**

Y: Microsoft anahtarları yönetir.

**S: kendi şifreleme anahtarları kullanmak?**

Şu anda A:.

**S: şifreleme anahtarlarının erişimi iptal?**

Şu anda A:. Microsoft, tam olarak anahtarları yönetir.

**S: bir depolama hesabı oluşturduğunuzda, depolama hizmeti şifrelemesi varsayılan olarak etkin mi?**

A: Evet depolama hizmeti şifrelemesi (Microsoft tarafından yönetilen anahtarları kullanarak) tüm depolama hesaplarının--Azure Resource Manager ve klasik varsayılan olarak etkindir. Blob storage, Table storage, kuyruk depolama ve Azure dosyaları, tüm hizmetleri de--etkinleştirdi.

**S: nasıl bu Azure Disk Şifrelemesi ' farklı mı?**

Y: azure Disk şifrelemesi, işletim sistemi ve veri diskleri Iaas Vm'leri de şifrelemek için kullanılır. Daha fazla bilgi için bkz: [depolama Güvenlik Kılavuzu](../storage-security-guide.md).

**S: alırsam ne Azure Disk şifrelemesi my veri disklerde etkinleştirebilirim?**

Y: Bu sorunsuz bir şekilde çalışır. Her iki yöntem, verileri şifreler.

**S: depolama Hesabımı coğrafi nedenle çoğaltılması için ayarlanır. Depolama hizmeti şifrelemesi ile my yedek kopya de şifrelenir mi?**

A: depolama hesabı tüm kopyalarını Evet şifrelenir. Seçenek desteklenmez--tüm artıklık yerel olarak yedekli depolama, bölge olarak yedekli depolama, coğrafi olarak yedekli depolama ve coğrafi olarak yedekli depolamaya okuma erişimi.

**S: ı şifreleme depolama Hesabımı devre dışı bırakabilirim?**

Y: şifreleme varsayılan olarak etkindir ve depolama hesabınız için şifrelemeyi devre dışı bırakmak için hiçbir sağlama yok. 

**S: depolama hizmeti şifrelemesi yalnızca belirli bölgelerine izin?**

Y: depolama hizmeti şifrelemesi, tüm hizmetler için tüm bölgelerde kullanılabilir. 

**S: bir sorunla karşılaşırsanız veya geri bildirim sağlamak istiyorsanız nasıl ı biriyle iletişim?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) herhangi bir sorun ya da depolama hizmeti şifreleme ile ilgili geri bildirim için.

## <a name="next-steps"></a>Sonraki adımlar
Azure depolama kapsamlı bir güvenlik özellikleri, birlikte Yardım geliştiricileri yapı güvenli uygulamalar kümesi sağlar. Daha fazla bilgi için bkz: [depolama Güvenlik Kılavuzu](../storage-security-guide.md).
