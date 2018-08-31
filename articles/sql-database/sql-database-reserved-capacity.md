---
title: Azure SQL veritabanı sanal çekirdek paradan tasarruf etmek ön ödeme | Microsoft Docs
description: Hesaplama maliyetlerinizden kaydetmek için Azure SQL veritabanı'nın ayrılmış kapasite satın alma konusunda bilgi edinin.
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: fcb7f2c1bb3e1653b9f8112abc0effaddc45fa54
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306478"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Azure SQL veritabanı'nın ayrılmış kapasite ile SQL veritabanı bilgi işlem kaynakları için ön ödeme

Azure SQL veritabanı ile Azure SQL veritabanı işlem kaynakları için Kullandıkça Öde fiyatlarıyla karşılaştırıldığında prepaying tarafından maliyet tasarrufu. Azure SQL veritabanı'nın ayrılmış kapasiteye sahip bir ön taahhüt SQL veritabanı'nda işlem maliyetleri önemli bir indirim almak için bir veya üç yıllık bir dönem için yapmanız gerekir. SQL veritabanı, ayrılmış bir kapasite satın almanız için bir Azure bölgesi, dağıtım türü, hizmet ve dönemi gerekir. 

SQL veritabanı örnekleri için ayırma atama gerekmez. Çalışmakta olan SQL veritabanı örnekleri veya avantajı otomatik olarak alırsınız, yeni dağıtılan vm'lere eşleşen. Rezervasyon satın alarak, bir veya üç yıllık bir dönem için SQL veritabanı örnekleri için işlem maliyetleri için ön ödeme olursunuz. Rezervasyon satın alma hemen sonra Rezervasyon öznitelikleri eşleşen işlem ücretlerini artık ödeme-olarak-ödersiniz SQL veritabanı ücretleri gidin. Ayırma, SQL veritabanı örneği ile ilişkilendirilmiş yazılım, ağ ve depolama ücretleri kapsamaz. Ayırma dönemi sonunda, fatura avantajı süresi dolar ve SQL veritabanları ödeme--, go fiyatını faturalandırılır. Rezervasyonlar otomatik yenileme değil. Fiyatlandırma bilgileri için bkz: [ayrılmış kapasite sunan SQL veritabanı](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Azure SQL veritabanı'nın ayrılmış kapasite satın alabilirsiniz [Azure portalında](https://portal.azure.com). SQL veritabanı ayrılmış kapasite satın almak için:
- En az bir kuruluş veya Kullandıkça Öde aboneliği sahip rolünün olması gerekir.
- Kurumsal abonelikler için Azure rezervasyon satın alma işlemleri içinde etkinleştirilmelidir [EA portal](https://ea.azure.com).
-  Bulut çözümü sağlayıcısı (CSP) programı için yönetim aracıları veya satış aracıları yalnızca SQL veritabanı ayrılmış kapasitesi satın alabilirsiniz.

Kurumsal müşteriler ve Kullandıkça Öde müşterileri rezervasyon satın alma işlemleri için nasıl ücretlendirilir Ayrıntılar alınmıştır [sık sorulan sorular](#frequently-asked-questions).

## <a name="determine-the-right-sql-size-before-purchase"></a>Satın almadan önce doğru SQL boyutunu belirler

Rezervasyon boyutunu temel veritabanı ve/veya belirli bir bölgedeki elastik havuzların tek işlem mevcut veya yakında-için--dağıtılabilir SQL tarafından kullanılan toplam miktarı ve aynı performans katmanına ve donanım oluşturmayı kullanarak. 

Örneğin, bir genel amaçlı, 5. nesil-16 sanal çekirdek elastik havuz ve iki iş açısından kritik, 5. nesil-4 sanal çekirdek tek veritabanının çalıştığını varsayalım. Ayrıca, ek bir genel amaçlı ve 5. nesil-16 sanal çekirdek elastik havuz, bir iş açısından kritik, 5. nesil-32 sanal çekirdek elastik havuz gelecek ay içinde dağıtmayı planladığınız şimdi gerekiyor. Ayrıca, en az 1 yıl için bu kaynakları gerekir bildiğiniz varsayalım. Bu durumda bir 32 satın almalıdır (2 x 16) sanal çekirdek, SQL veritabanı tek/elastik havuz genel amaçlı - işlem 5. nesil ve bir 40 (2 x 4 + 32) için 1 yıl ayırma için SQL veritabanı tek/elastik havuz iş açısından kritik - işlem 5. nesil sanal çekirdek 1 yıl ayırma.

## <a name="buy-sql-database-reserved-capacity"></a>SQL veritabanı ayrılmış kapasite satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Seçin **Ekle** ve ürün türü seçin bölmesinde ardından **SQL veritabanı** SQL veritabanı için yeni bir rezervasyon satın almak için.
4. Gerekli alanları doldurun. Öznitelikleri ayrılmış kapasite indirim almak için uygun eşleşen mevcut veya yeni bir tek veritabanı veya elastik havuzlara ayırır. SQL veritabanı örneklerinizi indirim almak gerçek sayısını, kapsam ve seçilen miktar bağlıdır.

   ![SQL veritabanı göndermeden önce ekran ayrılmış kapasite satın alma](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu rezervasyon adı.| 
    |Abonelik|SQL veritabanı ayrılmış kapasite ayırma için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet SQL veritabanı ayrılmış kapasite ayırma için ücretlendirilir. Abonelik, kurumsal anlaşma (teklif numarası: MS-AZR-0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR-0003P) türündedir. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|    
    |Kapsam       |Sanal çekirdek ayırma'nın kapsamı, bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Tek bir abonelik - sanal çekirdek ayırma indirimini bu Abonelikteki SQL veritabanı örneklerine uygulanır. </li><li>Paylaşılan - sanal çekirdek ayırma indirimi, herhangi bir abonelik, fatura bağlamı içinde çalışan SQL veritabanı örneklerine uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt (geliştirme ve test abonelikleri) hariç tüm aboneliklere dahildir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Bölge      |SQL veritabanı tarafından kapsanan Azure bölgesini kapasite ayırma saklıdır.|    
    |Dağıtım Türü|İçin ayırma satın almak istediğiniz SQL dağıtım türü.|
    |Hizmet Katmanı|SQL veritabanı örnekleri için hizmet katmanı.
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |SQL veritabanı içinde satın örneklerinin kapasite ayırma saklıdır. Fatura indirim almak SQL veritabanı örnekleri çalışan sayısını miktarıdır. Doğu ABD bölgesinde 10 SQL veritabanı örneğini çalıştırıyorsanız, örneğin, ardından, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtmeniz gerekir. |

5. SQL veritabanı maliyetini ayrılmış kapasite rezervasyonu gözden geçirme **maliyetleri** bölümü.
6. **Satın al**'ı seçin.
7. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

## <a name="next-steps"></a>Sonraki adımlar 
Sanal çekirdek ayırma indirimi, öznitelikleri ve veritabanına ayrılmış kapasite ayırma kapsamı eşleşen SQL veritabanı örnek sayısına otomatik olarak uygulanır. SQL veritabanı ayrılmış kapasite ayırma kapsamı güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com), PowerShell, CLI veya API üzerinden. 

SQL veritabanı'nı yönetme hakkında bilgi edinmek için kapasite ayırma ayrılmış bkz [SQL veritabanı, ayrılan kapasite](../billing/billing-manage-reserved-vm-instance.md).

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../billing/billing-save-compute-costs-reservations.md)
- [Azure Ayırmalarını yönetme](../billing/billing-manage-reserved-vm-instance.md)
- [Azure ayırmaları indirim anlama](../billing/billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

