---
title: Azure Cosmos DB kaynaklarını paradan tasarruf etmek ön ödeme | Microsoft Docs
description: Hesaplama maliyetlerinizden kaydetmek için Azure Cosmos DB ayrılmış kapasite satın alma konusunda bilgi edinin.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 1be2d67d8a1ee51c4883ae1f50b80ad3a9691c2d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981976"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Azure Cosmos DB kaynaklarını için ön ödeme ile ayrılmış kapasite

Azure Cosmos DB için Azure Cosmos DB kaynaklarını bir yıllık veya üç yıllık bir dönem için önceden ödeme yaparak tasarruf kapasite yardımcı saklıdır. Azure Cosmos DB ayrılmış kapasite, Cosmos DB için kaynaklar, örneğin, veritabanları, kapsayıcılar (koleksiyonlar/tablolar/grafikler) sağlanan aktarım hızı indirim almak sağlar. Azure Cosmos DB ayrılmış kapasite, Cosmos DB maliyetlerinizi önemli ölçüde azaltabilirsiniz — normal fiyatlarında yüzde 65'e varan – yıllık veya üç yıllık ön taahhüt. Ayrılmış kapasite bir fatura oranında indirim sağlar ve Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez.

Azure Cosmos DB ayrılmış kapasite kaynaklarınız için sağlanan aktarım hızı kapsar, depolama ve ağ ücretleri kapsamaz. Rezervasyon satın alma hemen sonra öznitelikler artık ödeme-olarak-ödersiniz ayırma eşleşen aktarım hızı ücretleri oranları gidin. Rezervasyonlar hakkında daha fazla bilgi için bkz. [Azure ayırmaları](../billing/billing-save-compute-costs-reservations.md) makalesi. 

Azure Cosmos DB ayrılmış kapasite satın alabilirsiniz [Azure portalında](https://portal.azure.com). Ayrılmış bir kapasite satın almak için:

* En az bir kuruluş veya Kullandıkça Öde aboneliği sahip rolünün olması gerekir.  
* Kurumsal abonelikler için Azure rezervasyon satın alma işlemleri içinde etkinleştirilmelidir [EA portal.](https://ea.azure.com/)  
* Yalnızca yönetim aracıları veya satış aracılar için bulut çözümü sağlayıcısı (CSP) programı, Azure Cosmos DB ayrılmış kapasitesi satın alabilirsiniz.

## <a name="determine-the-required-throughput-before-purchase"></a>Satın almadan önce gerekli aktarım hızı belirleme

Rezervasyon boyutu toplam mevcut veya yakında-için--dağıtılması Azure Cosmos DB kaynaklar (örneğin, veritabanları ya da kapsayıcıları - koleksiyonlar, tablolar, grafikler) kullanılan aktarım hızı miktarına bağlı olmalıdır. Aşağıdaki yollarla gerekli aktarım hızını belirleyebilirsiniz:

* Gidin [Azure portalı](https://portal.azure.com), Azure Cosmos DB hesabınızı bulun, ölçümler dikey penceresini açın ve ortalama aktarım hızı/sn ayrıntılarını alma **aktarım hızı** 3-6 aylık bir sürede sekmesi. Bu boyut ayrılmış kapasite birimleri satın alırken sağlar.

Alternatif olarak, bir Kurumsal Anlaşma (EA) yoksa, kullanım dosyanız indirebilir ve başvurduğu **hizmet türü** değerini **ek bilgi** Azure Cosmos DB almak için kullanım dosyasının işleme ayrıntıları.

Ayrıca tüm iş yükleri için ortalama aktarım hızı, Azure Cosmos DB hesaplarında sonraki bir veya üç yıl boyunca çalışmasını tahmin ve bu miktar için ayırma kullanın toplamak.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasite satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.  

2. Seçin **tüm hizmetleri** > **ayırmaları** > **Ekle**.  

3. Gelen **ürün türü seçin** bölmesinde seçin **Azure Cosmos DB**, ardından **seçin** yeni bir rezervasyon satın almak için.  

4. Aşağıdaki tabloda açıklandığı gibi gerekli alanları doldurun:

   ![Ayrılmış kapasite formu doldurun](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Alan  |Açıklama  |
   |---------|---------|
   |Ad   |    Ayırma adı. Bu alan otomatik olarak doldurulur `CosmosDB_Reservation_<timeStamp>`. Ayırma oluşturulduktan sonra yeniden adlandırmanız ya da rezervasyon oluştururken farklı bir ad girin.      |
   |Abonelik  |   Azure Cosmos DB ayrılmış kapasitesi için ödeme için kullanılan abonelik. Seçili abonelikte ödeme yöntemini, ön maliyet şarj içinde kullanılır. Abonelik türü aşağıdakilerden biri olmalıdır: <br/><br/>  [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) (teklif numarası: MS-AZR - 0017 P)-enterprise aboneliğiniz için ücretler kayıt ait parasal taahhüt bakiyeden kesilen veya kapasite aşımı olarak ücretlendirilir. <br/><br/> [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) (teklif numarası: MS-AZR - 0003 P)-Kullandıkça Öde aboneliğine ücretleri bir Abonelikteki kredi kartı veya fatura ödeme yöntemini faturalandırılır.    |
   |Kapsam   |   Ayırma'nın kapsamı kaç aboneliğe ayırma ile ilişkili faturalandırma avantajı yararlanabilir denetler ve ayırma belirli abonelikler için nasıl uygulanacağını denetler. Tek veya paylaşılan abonelik kapsamında bir ayırma vardır. Seçerseniz:   <br/><br/>  **Tek bir abonelik** -ayırma indirimi, seçili Abonelikteki Azure Cosmos DB örneklerine uygulanır. <br/><br/>  **Paylaşılan** – ayırma indirimi, herhangi bir abonelik, fatura bağlamı içinde çalışan Azure Cosmos DB örneklerine uygulanır. Fatura bağlamı, Azure için kaydolan nasıl göre belirlenir. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt (geliştirme ve test abonelikleri) hariç tüm aboneliklere dahildir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.  <br/><br/> Ayırma kapsamı ayrılmış kapasite satın aldıktan sonra değiştirebilirsiniz.  |
   |Ayrılmış kapasite türü   |  Ayrılmış kapasite, istek birimleri cinsinden sağlanan aktarım türüdür.|
   |Ayrılmış kapasite birimleri  |      Ayırmak istediğiniz üretilen iş miktarı. Bölge başına tüm, Cosmos DB kaynaklarını (örneğin, veritabanları veya kapsayıcıları) için gereken aktarım hızı belirleyerek bu değeri hesaplamak ve Cosmos DB veritabanınıza ile ilişkilendireceksiniz bölge sayısı ile çarpın.  <br/> Örneğin: 5 milyon RU/sn rezervasyon kapasitesi satın alma için beş bölge 1 milyon RU/sn ile her bölgede varsa, ardından seçin.    |
   |Sözleşme Dönemi  |   Bir yıl veya üç yıl.   |

5. İndirim ve rezervasyonu fiyatı gözden **maliyetleri** bölümü. Bu rezervasyon fiyat tüm bölgelerde sağlanan aktarım hızı ile Azure Cosmos DB kaynakları için geçerlidir.  

6. **Satın al**'ı seçin. Satın alma başarılı olduğunda, aşağıdaki ekran görüntüsünde görebilirsiniz. 

   ![Ayrılmış kapasite formu doldurun](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Rezervasyon satın aldıktan sonra Rezervasyon terimleriyle eşleşen tüm mevcut Azure Cosmos DB kaynaklarına hemen uygulanır. Mevcut Azure Cosmos DB kaynaklar yoksa, ayırma koşullarını karşılayan yeni bir Cosmos DB örneğine dağıttığınızda ayırmanın geçerli olur. Her iki durumda da, ayırma dönemi bir başarılı satın alma işleminden hemen başlar. 

Rezervasyon süresi dolduğunda, Azure Cosmos DB örneklerinizin çalışmaya devam eder ve normal Kullandıkça Öde fiyatları üzerinden faturalandırılır.

## <a name="next-steps"></a>Sonraki adımlar

Ayırma indirimi, öznitelikleri ve ayırma kapsamı eşleşen Azure Cosmos DB kaynaklarını otomatik olarak uygulanır. PowerShell, CLI, Azure portalı veya API üzerinden rezervasyon kapsamı güncelleştirebilirsiniz.

*  Nasıl ayrılmış kapasite indirimleri, Azure Cosmos DB'ye uygulandığını öğrenmek için bkz [anlamak Azure ayırmaları indirim](../billing/billing-understand-cosmosdb-reservation-charges.md)

* Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

   * [Azure ayırmaları nelerdir?](../billing/billing-save-compute-costs-reservations.md)  
   * [Azure ayırmalarını yönetme](../billing/billing-manage-reserved-vm-instance.md).  
   * [Kurumsal kayıt için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage.md)
   * [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

