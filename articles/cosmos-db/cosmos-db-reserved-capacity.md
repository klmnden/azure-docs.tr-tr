---
title: Azure Cosmos DB kaynak maliyeti ile ayrılmış kapasite en iyi duruma getirme
description: Hesaplama maliyetlerinizden kaydetmek için Azure Cosmos DB ayrılmış kapasite satın alma konusunda bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 7944980ec1806d2c8c4ab908c71efd971ee0d7aa
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968958"
---
# <a name="optimize-cost-with-reserved-capacity-in-azure-cosmos-db"></a>Azure Cosmos DB'de ayrılmış bir kapasiteyle maliyeti iyileştirin

Azure Cosmos DB için Azure Cosmos DB kaynaklarını bir yıl veya üç yıl için önceden ödeme yaparak tasarruf kapasite yardımcı saklıdır. Azure Cosmos DB ayrılmış kapasite ile Cosmos DB kaynaklarını için sağlanan aktarım hızı bir indirim alabilirsiniz. Kaynak veritabanları ve kapsayıcıları (tablolar, koleksiyonlar ve graflar) verilebilir.

Azure Cosmos DB ayrılmış kapasite, Cosmos DB maliyetlerinizi önemli ölçüde azaltabilirsiniz&mdash;bir yıllık veya üç yıllık ön taahhüt ile normal fiyatlarında yüzde 65'e varan. Ayrılmış kapasite bir fatura oranında indirim sağlar ve Azure Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez.

Azure Cosmos DB ayrılmış kapasite kaynaklarınız için sağlanan aktarım hızı kapsar. Depolama ve ağ ücretleri ele alınmamıştır. Rezervasyon satın alma hemen sonra öznitelikler artık ödeme-olarak-ödersiniz ayırma eşleşen aktarım hızı ücretleri oranları gidin. Rezervasyonlar hakkında daha fazla bilgi için bkz. [Azure ayırmaları](../billing/billing-save-compute-costs-reservations.md) makalesi.

Azure Cosmos DB ayrılmış kapasite satın alabilirsiniz [Azure portalında](https://portal.azure.com). Ayrılmış kapasite satın almak için:

* En az bir kuruluş veya Kullandıkça Öde aboneliği sahip rolünün olması gerekir.  
* Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.
* Yalnızca yönetim aracıları veya satış aracılar için bulut çözümü sağlayıcısı (CSP) programı, Azure Cosmos DB ayrılmış kapasite satın alabilirsiniz.

## <a name="determine-the-required-throughput-before-purchase"></a>Satın almadan önce gerekli aktarım hızı belirleme

Rezervasyon boyutu toplam mevcut veya yakında-için--dağıtılması Azure Cosmos DB kaynaklarını kullanan aktarım hızı miktarına bağlı olmalıdır. Aşağıdaki yollarla gerekli aktarım hızını belirleyebilirsiniz:

* Geçmiş verileri arasında Azure Cosmos DB hesapları, site veritabanları ve koleksiyonlar için sağlanan aktarım tüm bölgelerde alın. Örneğin, günlük kullanım deyimden indirerek günlük ortalama sağlanan aktarım hızı değerlendirilmesi için `https://account.azure.com`.

* Bir Kurumsal Anlaşma (EA) müşterisiyseniz, Azure Cosmos DB üretilen iş ayrıntılarını almak için kullanım dosyanıza indirebilirsiniz. Başvurmak **hizmet türü** değerini **ek bilgi** kullanım dosyasının bölümü.

* Azure Cosmos DB hesaplarınızın sonraki bir veya üç yıl boyunca çalıştırmak için beklediğiniz tüm iş yükleri için ortalama aktarım hızı toplayamaz. Ardından, bu miktar için ayırma kullanabilirsiniz.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasite satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.  

2. Seçin **tüm hizmetleri** > **ayırmaları** > **Ekle**.  

3. Gelen **ürün türü seçin** bölmesinde seçin **Azure Cosmos DB** > **seçin** yeni bir rezervasyon satın alma.  

4. Aşağıdaki tabloda açıklandığı gibi gerekli alanları doldurun:

   ![Ayrılmış kapasite formu doldurun](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png)

   |Alan  |Açıklama  |
   |---------|---------|
   |Ad   |    Ayırma adı. Bu alan otomatik olarak doldurulur `CosmosDB_Reservation_<timeStamp>`. Ayırma oluşturulurken, farklı bir ad sağlayabilirsiniz. Veya ayırma oluşturulduktan sonra yeniden adlandırabilirsiniz.      |
   |Abonelik  |   Azure Cosmos DB için ödeme yapmak üzere kullanılan abonelik kapasite saklıdır. Seçili abonelikte ödeme yöntemini, ön maliyet şarj içinde kullanılır. Abonelik türü aşağıdakilerden biri olmalıdır: <br/><br/>  Kurumsal Anlaşma (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P): Kurumsal aboneliğiniz için ücretler kayıt ait parasal taahhüt bakiyeden kesilen veya kapasite aşımı olarak ücretlendirilir. <br/><br/> Kullandıkça Öde (sayılar sunar: MS-AZR-0003P veya MS-AZR - 0023 P): Bir Kullandıkça Öde aboneliğine ücretleri, aboneliğinizin kredi kartı veya fatura ödeme yöntemi için faturalandırılır.    |
   |`Scope`   |   Kaç aboneliğe ayırma ile ilişkili faturalandırma avantajından yararlanabilirsiniz denetimleri seçeneği. Ayırma belirli abonelikler için nasıl uygulanacağını denetler.   <br/><br/>  Seçerseniz **tek abonelik**, ayırma indirimini seçili Abonelikteki Azure Cosmos DB örneklerine uygulanır. <br/><br/>  Seçerseniz **paylaşılan**, ayırma indirimini herhangi bir abonelik, fatura bağlamı içinde çalışan Azure Cosmos DB örneklerine uygulanır. Fatura bağlamı için Azure kaydolan nasıl dayanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt içinde tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.  <br/><br/> Ayırma kapsamı ayrılmış kapasite satın sonra değiştirebilirsiniz.  |
   |Ayrılmış kapasite türü   |  İstek birimi sağlanan aktarım hızı. Her iki kurulumları için-sağlanan aktarım hızı için bir ayırma satın alabilir tek bölge de olarak birden çok bölgeye yazma yazar.|
   |Ayrılmış kapasite birimleri  |      Ayırmak istediğiniz üretilen iş miktarı. Bölge başına aktarım hızı, Cosmos DB için tüm kaynakları (örneğin, veritabanları veya kapsayıcıları) gerekli belirleyerek bu değeri hesaplayabilirsiniz. Ardından bu Cosmos DB veritabanınıza ile ilişkilendireceksiniz bölge sayısı ile çarpın.  <br/><br/> Örneğin: Beş bölge 1 milyon RU/sn ile her bölgede varsa, 5 milyon RU/sn rezervasyon kapasitesi satın alma için seçin.    |
   |Dönem  |   Bir yıl veya üç yıl.   |

5. İndirim ve rezervasyonu fiyatı gözden **maliyetleri** bölümü. Bu rezervasyon fiyat, tüm bölgelerde sağlanan aktarım hızı ile Azure Cosmos DB kaynakları için geçerlidir.  

6. **Satın al**'ı seçin. Satın alma başarılı olduğunda aşağıdaki sayfayı görürsünüz:

   ![Ayrılmış kapasite formu doldurun](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png)

Rezervasyon satın alma sonra Rezervasyon terimleriyle eşleşen tüm mevcut Azure Cosmos DB kaynaklarına hemen uygulanır. Mevcut Azure Cosmos DB kaynaklar yoksa, ayırma koşullarını karşılayan yeni bir Cosmos DB örneğine dağıttığınızda ayırmanın geçerli olur. Her iki durumda da, ayırma dönemi bir başarılı satın alma işleminden hemen başlar.

Rezervasyonunuz süresi dolduğunda, Azure Cosmos DB örneklerinizin çalışmaya devam eder ve normal Kullandıkça Öde fiyatları üzerinden faturalandırılır.

## <a name="cancellation-and-exchanges"></a>İptal ve değişimler

Şu ayrılmış Kapasite tanımlama konusunda yardım için bkz: [ayırma indirimi, Azure Cosmos DB'ye nasıl uygulandığını anlamanız](../billing/billing-understand-cosmosdb-reservation-charges.md). İhtiyacınız olursa iptal etmek veya bir Azure Cosmos DB ayırması exchange görmek [ayırma değişimleri ve para iadesi](../billing/billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="next-steps"></a>Sonraki adımlar

Ayırma indirimi, öznitelikleri ve ayırma kapsamı eşleşen Azure Cosmos DB kaynaklarını otomatik olarak uygulanır. Azure portalı, PowerShell, Azure CLI veya API üzerinden rezervasyon kapsamı güncelleştirebilirsiniz.

*  Nasıl ayrılmış kapasite indirimleri, Azure Cosmos DB'ye uygulandığını öğrenmek için bkz [Azure ayırma indirimi anlama](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

   * [Azure ayırmaları nelerdir?](../billing/billing-save-compute-costs-reservations.md)  
   * [Azure ayırmalarını yönetme](../billing/billing-manage-reserved-vm-instance.md)  
   * [Kurumsal kayıt için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage.md)
   * [Azure iş ortağı merkezi CSP programı ayırmalarını](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
