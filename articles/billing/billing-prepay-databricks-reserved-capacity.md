---
title: Bir ön satın alma ile Azure Databricks maliyetlerini iyileştirme
description: Nasıl, Azure Databricks ücretleri paradan tasarruf etmek için ayrılmış kapasite ön ödeme öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 99eb4de86aa227d558bec54d011a0b1548d27cf0
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811264"
---
# <a name="optimize-azure-databricks-costs-with-a-pre-purchase"></a>Bir ön satın alma ile Azure Databricks maliyetlerini iyileştirme

Kaydedebileceğiniz, Azure Databricks üzerinde bir veya üç yıl için Azure Databricks önceden satın aldığınızda birim (DBU) maliyetlerini işleme birimleri (DBCU). Satın alma dönem boyunca herhangi bir zamanda önceden satın alınan DBCUs kullanabilirsiniz. VM'ler, farklı önceden satın alınan birimlerin saatlik olarak dolmasın ve dönemi satın alma sırasında herhangi bir zamanda bunları kullanın.

Azure Databricks kullanım önceden satın alınan DBUs otomatik olarak çıkarır. Dağıtmanız veya önceden satın alınan bir plan DBU kullanımının ön satın alma indirimleri almak için Azure Databricks çalışma alanları atama gerekmez.

Ön satın alma indirim yalnızca DBU kullanımı için geçerlidir. İşlem, depolama ve ağ gibi diğer ücretleri ayrı olarak ücretlendirilir.

## <a name="determine-the-right-size-to-buy"></a>Satın almak için doğru boyutta belirleme

Databricks ön satın alma, tüm Databricks iş yükleri ve katmanları için geçerlidir. Ön satın alma ön ödemeli Databricks işleme birimlerini oluşan bir havuz olarak düşünebilirsiniz. Kullanımı havuz, iş yükü veya katmanını bağımsız olarak çıkarılır. Kullanım, aşağıdaki oranı çıkarılır:

| **İş yükü** | **DBU uygulama oranı - standart katman** | **DBU uygulama oranı — Premium katman** |
| --- | --- | --- |
| Veri Analizi | 0.4 | 0.55 |
| Veri Mühendisliği | 0.15 | 0.30 |
| Veri Mühendisliği Hafif Düzey | 0,07 | 0.22 |

Örneğin, Data Analytics – miktarı standart katman tüketilen, önceden satın alınan Databricks işleme birimlerini 0,4 birimlerle düşülür.

Satın almadan önce farklı iş yükleri ve katmanları için tüketilen toplam DBU miktarı hesaplayın. Önceki oranları için DBCU Normalleştir ve sonra toplam kullanımının bir projeksiyon sonraki bir veya üç yıl boyunca çalıştırmak için kullanın.

## <a name="purchase-databricks-commit-units"></a>Databricks işleme birimi satın

Databricks planları satın alabileceğiniz [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D). Ayrılmış kapasite satın almak için en az bir kurumsal abonelik için sahip rolü olmalıdır.

- Şu anda önceden satın almanıza yalnızca Kurumsal Sözleşme müşterileri tarafından kullanılabilir.
- Bir sahip rolü için en az bir Enterprise aboneliğinizin olması gerekir.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com/). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.

**Satın almak için:**

1. [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D) gidin.
1. Bir abonelik seçin. Kullanım **abonelik** ayrılmış kapasitesi için ödeme için kullanılan aboneliği seçin. Abonelik ödeme yöntemini, ön maliyet ayrılmış kapasite için ücretlendirilir. Ücretleri kayıt ait parasal taahhüt bakiyeden kesilen veya kapasite aşımı olarak ücretlendirilir.
1. Bir kapsam seçin. Kullanım **kapsam** abonelik kapsamını seçin:
    - **Tek bir kaynak grup kapsamı** — ayırma indirimi, eşleşen kaynakları yalnızca seçilen kaynak grubunda uygular.
    - **Tek abonelik kapsamında** — ayırma indirimi, eşleşen kaynaklara seçili Abonelikteki geçerlidir.
    - **Paylaşılan kapsam** — fatura bağlamında uygun aboneliklerin kaynaklarında eşleşen ayırma indirimi geçerlidir. Kurumsal Anlaşma müşterileri için fatura bağlamı kaydı değil.
1. Satın alma ve satın alma işlemini tamamlamak için kaç tane Azure Databricks işleme birimleri seçin.


![Azure portalında Azure Databricks satın alma gösteren örnek](./media/billing-prepay-databricks-reserved-capacity/data-bricks-pre-purchase.png)

## <a name="change-scope-and-ownership"></a>Kapsamı Değiştir ve sahipliği

Rezervasyon satın alma sonra aşağıdaki türde değişiklikler yapabilirsiniz:

- Ayırma kapsamı güncelleştirme
- Rol tabanlı erişim

Bölme veya Databricks işleme birimi ön satın alma birleştirme yapamazsınız. Ayırmalarını yönetme hakkında daha fazla bilgi için bkz. [rezervasyon satın almanızdan sonra yönetme](billing-manage-reserved-vm-instance.md).

## <a name="cancellations-and-exchanges"></a>İptalleri ve değişimler

Databricks ön satın alma planları için İptal'ı ve Exchange'i desteklenmez. Tüm satın alımlar son.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Bir Azure Databricks ön satın alma DBCU indirim nasıl uygulanacağını anlama](billing-reservation-discount-databricks.md)
  - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
