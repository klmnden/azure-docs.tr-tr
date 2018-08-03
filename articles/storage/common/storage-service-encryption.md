---
title: Bekleyen veri için Azure depolama hizmeti şifrelemesi | Microsoft Docs
description: Azure yönetilen diskler, Azure Blob Depolama, Azure dosyaları, Azure kuyruk depolama ve Azure tablo depolama hizmeti tarafındaki verileri depolarken şifrelemek için Azure depolama hizmeti Şifrelemesi özelliğini kullanın ve verileri alınırken bir şifre çözme.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 08/01/2018
ms.author: lakasa
ms.openlocfilehash: 1a127f7e3dd57376ecd05d4ae7030becb33f1159
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480314"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Bekleyen veri için Azure depolama hizmeti şifrelemesi
Azure depolama hizmeti şifrelemesi bekleyen veriler için Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi korumanıza yardımcı olur. Bu özellik, Azure depolama platformu, verilerinizi otomatik olarak şifreler önce Azure yönetilen diskler, Azure Blob Depolama, Azure dosyaları veya Azure kuyruk depolama için kalıcı ve alma önce verilerin şifresini çözer. Şifreleme, rest, şifre çözme ve anahtar yönetimi, depolama hizmeti şifrelemesi şifreleme işlenmesini kullanıcılara saydamdır. Azure depolama platformu için yazılan tüm veriler, 256 bit şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), aşağıdakilerden birini en güçlü blok şifreleme özelliklerinden kullanılabilir.

Depolama hizmeti şifrelemesi için tüm yeni ve var olan depolama hesapları etkinleştirilir ve devre dışı bırakılamaz. Verilerinizi varsayılan olarak korumalı olduğundan, kod veya depolama hizmeti şifrelemesi yararlanmak için uygulamaları değişiklik gerekmez.

Bu özellik, verileri otomatik olarak şifreler:

- Azure depolama hizmetleri:
    - Azure Yönetilen Diskleri
    - Azure Blob depolama
    - Azure Dosyaları
    - Azure kuyruk depolama
    - Azure tablo depolama.  
- Her iki performans katmanı olduğu (standart ve Premium).
- Her iki dağıtım modeline olduğu (Azure Resource Manager ve klasik).

Depolama hizmeti şifrelemesi, Azure depolama hizmetleri performansını etkilemez.

Depolama hizmeti şifrelemesi ile kullanabileceğiniz Microsoft tarafından yönetilen bir şifreleme anahtarları veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Kendi anahtarlarınızı kullanma hakkında daha fazla bilgi için bkz. [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Azure portalında görünümü şifreleme ayarları
Depolama hizmeti şifrelemesi ayarlarını görüntülemek için oturum açın [Azure portalında](https://portal.azure.com) ve bir depolama hesabı seçin. İçinde **ayarları** bölmesinde **şifreleme** ayarı.

![Şifreleme ayarı gösteren portalı ekran görüntüsü](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Depolama hizmeti şifrelemesi hakkında SSS
**Bir Resource Manager depolama hesabındaki verileri nasıl şifreliyor mu?**  
Depolama hizmeti şifrelemesi etkin tüm depolama hesapları için--Klasik ve Resource Manager, şifreleme etkinleştirilmeden önce oluşturduğunuz depolama hesabına var olan dosyalar olacak geriye dönük olarak bir arka plan şifreleme işlemi tarafından.

**Ben bir depolama hesabı oluşturduğunuzda, depolama hizmeti şifrelemesi varsayılan olarak etkin mi?**  
Evet, depolama hizmeti şifrelemesi ve tüm Azure depolama hizmetleri için tüm depolama hesapları etkinleştirilir.

**Resource Manager depolama hesabı var. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**  
Depolama hizmeti şifrelemesi, var olan tüm Resource Manager depolama hesaplarında varsayılan olarak etkindir. Bu tablo depolama Azure Blob Depolama, Azure dosyaları, Azure kuyruk depolama için desteklenir. 

**Ben şifreleme depolama Hesabımı devre dışı bırakabilirim?**  
Şifreleme varsayılan olarak etkindir ve, depolama hesabı için şifrelemeyi devre dışı bırakmak için hiçbir sağlama yoktur. 

**Ne kadar depolama hizmeti şifrelemesi etkinleştirilirse Azure depolama maliyeti?**  
Hiçbir ek ücret yoktur.

**Kendi şifreleme anahtarlarını kullanabilir miyim?**  
Evet, Azure Blob Depolama ve Azure dosyaları için kendi şifreleme anahtarlarınızı kullanabilirsiniz. Müşteri tarafından yönetilen anahtarlar Azure yönetilen diskler tarafından şu anda desteklenmemektedir. Daha fazla bilgi için [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md).

**Şifreleme anahtarlarına erişimi iptal etme?**  
Evet, varsa, [kendi şifreleme anahtarlarınızı kullanmak](storage-service-encryption-customer-managed-keys.md) Azure anahtar Kasası'nda.

**Depolama hizmeti şifrelemesi Azure Disk Şifrelemesi ' farklı mı?**  
Azure Disk şifrelemesi, BitLocker ve DM-Crypt gibi işletim sistemi tabanlı çözümler ve Azure anahtar kasası arasında tümleştirme sağlar. Depolama hizmeti şifrelemesi, yerel olarak katmanında Azure depolama platformu, sanal makine aşağıdaki şifreleme sağlar.

**Klasik depolama hesabı var. Depolama hizmeti şifrelemesi üzerinde etkinleştirebilirim?**  
Depolama hizmeti şifrelemesi etkin tüm depolama hesapları (Klasik ve Resource Manager).

**Nasıl miyim veri Klasik depolama Hesabımı şifreleyebilir mi?**  
Varsayılan olarak etkin şifreleme ile Azure depolama Hizmetleri'nde depolanan tüm veriler otomatik olarak şifrelenir. 

**Azure PowerShell ve Azure CLI kullanarak depolama hizmeti şifrelemesini ile depolama hesapları oluşturabilir miyim?**  
Herhangi bir depolama hesabı oluşturma sırasında depolama hizmeti şifrelemesi varsayılan olarak etkindir (Klasik veya Resource Manager). Hesap özellikleri, hem Azure PowerShell ve Azure CLI kullanarak doğrulayabilirsiniz.

**Depolama Hesabımı coğrafi nedenle çoğaltılacak şekilde ayarlanır. Depolama hizmeti şifrelemesi ile yedekli kopyamı de şifrelenir mi?**  
Evet, depolama hesabını tüm kopyalarını şifrelenir. Seçenek desteklenmez--tüm yedeklilik yerel olarak yedekli depolama, bölgesel olarak yedekli depolama, coğrafi olarak yedekli depolama ve okuma erişimli coğrafi olarak yedekli depolama.

**Depolama hizmeti şifrelemesi yalnızca belirli bölgelerde izin verilir?**  
Depolama hizmeti şifrelemesi tüm bölgelerde kullanılabilir.

**Depolama hizmeti şifrelemesi FIPS 140-2 uyumlu mu?**  
Evet, depolama hizmeti şifrelemesi FIPS 140-2 ile uyumlu olan.

**Herhangi bir sorun veya geri bildirim sağlamak isterseniz nasıl birisi başvurmam gerekir?**  
İlgili kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) herhangi bir sorun ya da depolama hizmeti şifrelemesi için ilgili geri bildirim için.

## <a name="next-steps"></a>Sonraki adımlar
Azure depolama, kapsamlı bir güvenlik özellikleri, birlikte Yardım geliştiriciler güvenli uygulamalar oluşturun kümesi sağlar. Daha fazla bilgi için [depolama Güvenlik Kılavuzu](../storage-security-guide.md).