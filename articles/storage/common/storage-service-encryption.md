---
title: Bekleyen veri için Azure depolama hizmeti şifrelemesi | Microsoft Docs
description: Azure Blob Depolama hizmeti tarafındaki verileri depolarken şifrelemek için Azure depolama hizmeti Şifrelemesi özelliğini kullanın ve verileri alınırken bir şifre çözme.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: lakasa
ms.openlocfilehash: d469dfb5092f1269a6600ee8ee2f81778fd83b96
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450326"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Bekleyen Veri için Azure Storage Hizmeti Şifreleme

Azure depolama hizmeti şifrelemesi bekleyen veriler için Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi korumanıza yardımcı olur. Bu özellik, Azure depolama, verilerinizi otomatik olarak şifreler önce Azure depolama için kalıcı ve alma önce verilerin şifresini çözer. Şifreleme, rest, şifre çözme ve anahtar yönetimi, depolama hizmeti şifrelemesi şifreleme işlenmesini kullanıcılara saydamdır. Azure Depolama'ya yazılan tüm veriler, 256 bit şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), aşağıdakilerden birini en güçlü blok şifreleme özelliklerinden kullanılabilir.

Depolama hizmeti şifrelemesi için tüm yeni ve var olan depolama hesapları etkinleştirilir ve devre dışı bırakılamaz. Verilerinizi varsayılan olarak korumalı olduğundan, kod veya depolama hizmeti şifrelemesi yararlanmak için uygulamaları değişiklik gerekmez.

Bu özellik, verileri otomatik olarak şifreler:

- Her iki performans katmanı olduğu (standart ve Premium).
- Her iki dağıtım modeline olduğu (Azure Resource Manager ve klasik).
- Tüm Azure depolama hizmetleri (Blob Depolama, kuyruk depolama, tablo depolama ve Azure dosyaları). 

Depolama hizmeti şifrelemesi, Azure depolama performansını etkilemez.

Depolama hizmeti şifrelemesi ile kullanabileceğiniz Microsoft tarafından yönetilen bir şifreleme anahtarları veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Kendi anahtarlarınızı kullanma hakkında daha fazla bilgi için bkz. [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Azure portalında görünümü şifreleme ayarları

Depolama hizmeti şifrelemesi ayarlarını görüntülemek için oturum açın [Azure portalında](https://portal.azure.com) ve bir depolama hesabı seçin. İçinde **ayarları** bölmesinde **şifreleme** ayarı.

![Şifreleme ayarı gösteren portalı ekran görüntüsü](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Depolama hizmeti şifrelemesi hakkında SSS

**S: Klasik depolama hesabı var. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**

Y: depolama hizmeti şifrelemesi etkin tüm depolama hesapları (Klasik ve Resource Manager).

**S: ben veri Klasik depolama Hesabımı şifreleyebilir mi?**

Y: varsayılan olarak etkin şifreleme ile Azure depolama, yeni verilerinizi otomatik olarak şifreler. 

**S: bir Resource Manager depolama hesabına sahip. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**

Y: depolama hizmeti şifrelemesi, var olan tüm Resource Manager depolama hesaplarında varsayılan olarak etkindir. Bu, Blob Depolama, tablo depolama, kuyruk depolama ve Azure dosyaları için desteklenir. 

**S: nasıl bir Resource Manager depolama hesabındaki verileri şifreliyor mu?**

Y: depolama hizmeti şifrelemesi etkin tüm depolama hesapları için--Klasik ve Resource Manager, şifreleme etkinleştirilmeden önce oluşturduğunuz depolama hesabına var olan dosyalar olacak geriye dönük olarak bir arka plan şifreleme işlemi tarafından.

**Depolama hesapları ile Azure PowerShell ve Azure CLI kullanarak depolama hizmeti şifrelemesini oluşturabilirim miyim?**

Y: depolama hizmeti şifrelemesi, herhangi bir depolama hesabı oluşturma sırasında varsayılan olarak etkindir (Klasik veya Resource Manager). Hesap özellikleri, hem Azure PowerShell ve Azure CLI kullanarak doğrulayabilirsiniz.

**S: ne kadar fazla depolama hizmeti şifrelemesi etkinleştirilirse Azure depolama maliyeti?**

Y: hiçbir ek maliyet yoktur.

**Kendi şifreleme anahtarları kullanabilirim miyim?**

C: Evet, kendi şifreleme anahtarlarınızı kullanabilirsiniz. Daha fazla bilgi için [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).

**Şifreleme anahtarlarına erişimi iptal etme miyim?**

C: Evet ise, [kendi şifreleme anahtarlarınızı kullanmak](storage-service-encryption-customer-managed-keys.md) Azure anahtar Kasası'nda.

**S: Ben bir depolama hesabı oluşturduğunuzda, depolama hizmeti şifrelemesi varsayılan olarak etkin mi?**

C: Evet, depolama hizmeti şifrelemesi ve tüm Azure depolama hizmetleri için tüm depolama hesapları etkinleştirilir.

**S: nasıl bu Azure Disk Şifrelemesi ' farklıdır?**

Y: azure Disk şifrelemesi, işletim sistemi ve veri diskleri Iaas vm'lerinde şifrelemek için kullanılır. Daha fazla bilgi için [depolama Güvenlik Kılavuzu](../storage-security-guide.md).

**S: Azure Disk şifrelemesi veri disklerim etkinleştirebilirim?**

Y: Bu sorunsuz bir şekilde çalışır. Her iki yöntem de verilerinizi şifreler.

**S: depolama Hesabımı coğrafi nedenle çoğaltılacak şekilde ayarlanır. Depolama hizmeti şifrelemesi ile yedekli kopyamı de şifrelenir mi?**

C: Evet, depolama hesabını tüm kopyalarını şifrelenir. Seçenek desteklenmez--tüm yedeklilik yerel olarak yedekli depolama, bölgesel olarak yedekli depolama, coğrafi olarak yedekli depolama ve okuma erişimli coğrafi olarak yedekli depolama.

**My depolama hesabı şifreleme devre miyim?**

Y: şifreleme varsayılan olarak etkindir ve, depolama hesabı için şifrelemeyi devre dışı bırakmak için hiçbir sağlama yoktur. 

**S: depolama hizmeti şifrelemesi yalnızca belirli bölgelerde izin var mı?**

Y: depolama hizmeti şifrelemesi, tüm hizmetleri tüm bölgelerde kullanılabilir.

**S: depolama hizmeti şifrelemesi FIPS 140-2 uyumlu mu?**

C: Evet, depolama hizmeti şifrelemesi FIPS 140-2 ile uyumlu olan.

**Q: sorunlarla karşılaşıyorsanız veya geri bildirim sağlamak isterseniz nasıl birisi başvurmam gerekir?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) herhangi bir sorun ya da depolama hizmeti şifrelemesi için ilgili geri bildirim için.

## <a name="next-steps"></a>Sonraki adımlar
Azure depolama, kapsamlı bir güvenlik özellikleri, birlikte Yardım geliştiriciler güvenli uygulamalar oluşturun kümesi sağlar. Daha fazla bilgi için [depolama Güvenlik Kılavuzu](../storage-security-guide.md).
