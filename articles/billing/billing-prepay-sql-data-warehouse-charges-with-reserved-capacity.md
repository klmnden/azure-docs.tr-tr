---
title: Azure ayrılmış kapasite ile SQL veri ambarı ücretleri için ön ödeme
description: Nasıl, SQL veri ambarı ücretleri paradan tasarruf etmek için ayrılmış kapasite ön ödeme öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: banders
ms.openlocfilehash: cea2c8e6d476c3ea2799337ab2da1f9406731814
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565354"
---
# <a name="prepay-for-sql-data-warehouse-charges-with-reserved-capacity"></a>SQL veri ambarı ücretleri için ön ödeme ile ayrılmış kapasite

CDWU kullanımınız için bir veya üç yıl boyunca prepaying tarafından Azure SQL veri ambarı ile paradan tasarruf edebilirsiniz. Azure'ı seçmeniz gerekebilir SQL veri ambarı ayrılmış kapasite satın almak için bölge ve dönem. Ardından, SQL veri ambarı SKU sepetinize ekleyin ve satın almak istediğiniz cDWU birim miktarını seçin.

SQL veri ambarı ayırma öznitelikleri eşleşen kullanımı artık ödeme-olarak-sizin hakkınızda ücretlendirilir bir ayırma satın aldığınızda, ücretler gidin.

Ayırma, depolama veya SQL veri ambarı kullanım ile ilişkili ağ ücretleri ele alınmamıştır.

Ayrılmış kapasite süresi dolduğunda, SQL veri ambarı örneği çalışmaya devam eder ancak ödeme--, go fiyatı üzerinden faturalandırılır. Rezervasyonlar otomatik olarak yenileme yoktur.

Fiyatlandırma bilgileri için bkz: [SQL veri ambarı ayrılmış kapasite sunan](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

Azure SQL veri ambarı ayrılmış kapasite satın alabilirsiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade). Ayrılmış kapasite satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolüne sahip olmalıdır.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** seçeneği etkinleştirilmelidir [EA portal](https://ea.azure.com/). Ayarı devre dışıysa, bir EA yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programın, yalnızca yönetim aracıları veya satış aracılarının SQL veri ambarı ayrılmış kapasite satın alabilirsiniz.

Kurumsal müşteriler ve Kullandıkça Öde müşterileri rezervasyon satın alma işlemleri için nasıl ücretlendirilir hakkında daha fazla bilgi için bkz. [Kurumsal kayıt için Azure ayırma kullanımını anlamak](billing-understand-reserved-instance-usage-ea.md) ve [Azure anlama ayırma kullanımı için Kullandıkça Öde aboneliğinizi](billing-understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Satın almadan önce doğru boyutu seçin

Rezervasyon boyutu toplam dayanmalıdır SQL veri ambarı işlem tükettiğiniz veri ambarı birimi (cDWU). 100 cDWU artışlarla satın alma işlemleri gerçekleştirilir.

Örneğin, SQL veri ambarı'nın toplam tüketiminiz DW3000c olduğu varsayılır. Tümünün için ayrılmış bir kapasite satın almanız istiyorsunuz. Bu nedenle, 30 cDWU ayrılmış kapasite birimleri satın almalıdır.

## <a name="buy-sql-data-warehouse-reserved-capacity"></a>SQL veri ambarı ayrılmış kapasite satın alın

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Bir abonelik seçin. Abonelik listesi ayrılmış kapasitesi için ödeme için kullanılan aboneliği seçmek için kullanın. Abonelik ödeme yöntemini, ön maliyet ayrılmış kapasite için ücretlendirilir. Kurumsal Anlaşma abonelik türü olmalıdır (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P) ya da Kullandıkça Öde (sayılar sunar: MS-AZR-0003P veya MS-AZR-0023P).
  - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
  - Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.
4. Bir kapsam seçin. Kapsam listesi abonelik kapsamında seçmek için kullanın.
  - **Tek bir kaynak grup kapsamı** — ayırma indirimi, eşleşen kaynakları yalnızca seçilen kaynak grubunda uygular.
  - **Tek abonelik kapsamında** — ayırma indirimi, eşleşen kaynaklara seçili Abonelikteki geçerlidir.
  - **Paylaşılan kapsam** — fatura bağlamında uygun aboneliklerin kaynaklarında eşleşen ayırma indirimi geçerlidir. Kurumsal Anlaşma müşterileri için fatura bağlamı kaydı değil. Kullandıkça Öde tarifesine göre ile tek tek abonelikleri için faturalama Hesap Yöneticisi tarafından oluşturulan tüm uygun abonelikleri kapsamıdır.
    - Kurumsal müşteriler için fatura bağlamı EA kayıt ' dir.
    - Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.
5. Ayrılmış kapasite tarafından kapsanan bir Azure bölgesi seçmek için bir bölge seçin.
6. Miktar seçin. Satın almak istediğiniz 100 veri ambarı birimi (cDWU) miktarını girin.    
  Örneğin, bir miktar 30 saatte ayrılmış kapasite 3.000 cDWU verirsiniz.
7. SQL veri ambarı ayrılmış kapasite ayırma maliyeti gözden geçirme **maliyetleri** bölümü.
8. **Satın al**'ı seçin.
9. Seçin **bu rezervasyonu görüntüle** , satın alma durumunu görmek için.

## <a name="cancellations-and-exchanges"></a>İptalleri ve değişimler

İhtiyacınız varsa SQL veri ambarınızın iptal etmek için kapasite ayrılmış, % 12 erken sonlandırma ücreti olabilir. Para iadeleri satın aldığınız fiyattan veya geçerli rezervasyon fiyatından düşük olana göre hesaplanır. Para iadesi 50,000.00 yılda sınırlıdır. Para iadesi alırsınız % 12 erken sonlandırma ücreti saatlere eşit olarak dağıtılmış kalan Bakiye ' dir. Azure portal ve select ayırma Git bir iptal isteğinde bulunmak **para iadesi** bir destek isteği oluşturmak için.

SQL veri ambarı ayrılmış kapasite başka bir bölge ya da terim değiştirmeniz gerekiyorsa, eşit veya daha fazla değeri için başka bir ayırma gönderip alabilir. Yeni ayırma işleminin başlangıç tarihi değiştirilen ayırma işleminin başlangıç tarihiyle aynı olmaz. Bir veya üç yıllık süre yeni ayırma oluşturduğunuzda başlatır. Bir exchange istemek için Azure portalında ayırma açın ve seçin **Exchange** bir destek isteği oluşturmak için.

Exchange ya da para iadesinin ayırmaları hakkında daha fazla bilgi için bkz. [ayırma değişimleri ve para iadesi](billing-azure-reservations-self-service-exchange-and-refund.md).

Ayırma indirimi, bölge ve SQL veri ambarı ayrılmış kapasite kapsamını eşleşen SQL veri ambarı örnek sayısına otomatik olarak uygulanır. SQL veri ambarı ayrılmış kapasite ile kapsamını güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com/), PowerShell, CLI veya API üzerinden.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

- Ayırma indirimleri Azure SQL veri ambarı'na nasıl uygulandığı hakkında daha fazla bilgi için bkz: [ayırma indirimleri Azure SQL veri ambarı'na nasıl geçerli](billing-prepay-sql-data-warehouse-charges-with-reserved-capacity.md).

- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
  - [Azure ayırmaları indirim anlama](billing-understand-reservation-charges.md)
  - [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
  - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
