---
title: Azure Güvenlik Merkezi’nde algılama özellikleri | Microsoft Belgeleri
description: Bu belge Azure Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığını anlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 4c5599cc-99a1-430f-895f-601615ef12a0
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/2/2018
ms.author: rkarlin
ms.openlocfilehash: 8cd76909c9ce15a97de4ea5af3b21ac120058dd3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60705888"
---
# <a name="azure-security-center-detection-capabilities"></a>Azure Güvenlik Merkezi algılama özellikleri
Bu belge - hem Windows hem de Linux - Microsoft Azure kaynaklarınızı hedefleyen etkin tehditleri belirlemenize yardımcı olur ve hızlı bir şekilde yanıt vermek için gereken bilgileri sağlayan Azure Güvenlik Merkezi'nin Gelişmiş algılama özelliklerini ele alınmaktadır.

Gelişmiş algılamalar Azure Güvenlik Merkezi'nin Standart Katmanında mevcuttur. Ücretsiz deneme sürümü mevcuttur. [Güvenlik İlkesi](tutorial-security-policy.md) bölümündeki Fiyatlandırma Katmanı’ndan yükseltme yapabilirsiniz. Fiyatlandırma hakkında daha fazla bilgi almak için [Güvenlik Merkezi sayfasını](https://azure.microsoft.com/pricing/details/security-center/) ziyaret edin.


## <a name="responding-to-todays-threats"></a>Günümüzün tehditlerine yanıt verme
Son 20 yılda tehdit kapsamında önemli değişiklikler olmuştur. Geçmişte şirketlerin yalnızca çoğunlukla “yapabileceklerini” görmeyle ilgilenen bireysel saldırganların web sitesi tahrifatından endişe duyması gerekiyordu. Bugün ise saldırganlar çok daha karmaşık ve organize hareket etmektedir. Saldırganlar genellikle belirli finansal ve stratejik hedeflere sahiptir. Ayrıca, ulus devletler veya organize suç örgütleri tarafından finanse edilebildiklerinden ellerinde daha fazla kaynak bulunmaktadır.

Bu yaklaşım saldırganlar tarafında eşi görülmemiş bir profesyonellik düzeyine yol açmıştır. Saldırganlar artık web tahrifatı ile ilgilenmemektedir. Artık bilgileri, mali hesapları ve gizli verileri çalmayla ilgilenmektedir ve tüm bunlar açık piyasada nakit oluşturmak ya da belirli bir ticari, siyasi veya askeri konumdan yararlanmak için kullanılabilir. Mali hedefleri olan saldırganlardan çok daha fazla endişe verici olan ise altyapıya ve insanlara zarar vermek için ağları ihlal eden saldırganlardır.

Buna yanıt olarak, kuruluşlar çoğunlukla bilinen saldırı imzalarını arayarak kurumsal çevrelerini veya uç noktalarını savunmaya odaklanan çeşitli nokta çözümleri dağıtmaktadır. Bu çözümler, güvenlik analizi uzmanının belirlemesini ve araştırmasını gerektiren yüksek miktarda düşük güvenilirlik uyarıları oluşturma eğilimindedir. Çoğu kurum bu uyarılara yanıt verecek zaman ve uzmanlığa sahip olmadığından birçoğu çözüme kavuşmamaktadır.  Bu sırada, saldırganlar çok sayıda imza tabanlı savunmayı bozmaya ve [bulut ortamlarına uyarlamaya](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/) yönelik yöntemlerini geliştirmiştir. Ortaya çıkan tehditleri daha hızlı belirlemek ve algılama ile yanıtı hızlandırmak için yeni yaklaşımlar gereklidir.

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Azure Güvenlik Merkezi’nin tehditleri algılaması ve yanıt vermesi
Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Bunlar Microsoft’un bulut ve şirket içindeki genel varlığından edinilen kapsamlı bir telemetri kümesine erişebilmektedir. Geniş kapsamlı ve çeşitlilik barındıran bu veri kümeleri Microsoft’un yeni saldırı modellerini ve şirket içi müşteri ve kuruluş ürünlerinin yanı sıra çevrimiçi hizmetleri üzerindeki eğilimleri keşfetmesini sağlamaktadır. Sonuç olarak, saldırganlar yeni ve giderek karmaşık hale gelen sömürülerini ortaya çıkardıkça Güvenlik Merkezi algılama algoritmalarını hızlı bir şekilde güncelleştirebilmektedir. Bu yaklaşık hızlı hareket eden bir ortama ayak uydurmanıza yardımcı olmaktadır.

Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

![Güvenlik Merkezi Veri toplama ve sunumu](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri ve [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerindeki sıçramalar, tüm bulut yapısındaki olayları değerlendirmek için kullanılır: elle yaklaşımlar kullanılarak ve saldırıların gelişimi tahmin edilerek belirlenmesi imkansız olan tehditler algılanır. Bu güvenlik analizleri şunlardır:

* **Tümleşik tehdit bilgileri**: Microsoft ürün ve hizmetleri, Microsoft Dijital Suçlar Birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların küresel tehdit bilgilerinden yararlanarak bilinen kötü aktörleri arar.
* **Davranış analizi**: kötü amaçlı davranışları bulmak için bilinen modelleri uygular.
* **Anormallik algılama**: geçmiş taban çizgisi oluşturmak için istatistiksel profil oluşturmayı kullanır. Olası bir saldırı vektörüne uygun olan yerleşik taban çizgilerinden sapmalar konusunda uyarır.

### <a name="threat-intelligence"></a>Tehdit bilgileri
Microsoft yoğun miktarda genel tehdit bilgisine sahiptir. Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Dijital Suçlar Birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden fazla kaynaktan telemetri akışı sağlanır. Araştırmacılar ayrıca büyük bulut hizmeti sağlayıcıları ve üçüncü tarafların bilgi akışlarına abone olan kişiler arasında paylaşılan tehdit bilgilerini alır. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir. Bazı örnekler:

* **Kötü amaçlı bir IP adresine giden iletişim**: bilinen bir botnet veya darknet ile giden trafik, kaynağınızın tehlikeye girdiğini ve bir saldırganın ilgili sistem üzerinde komut yürütmeye ya da verileri çalmaya çalıştığını gösterir. Azure Güvenlik Merkezi, ağ trafiğini Microsoft'un genel tehdit veritabanıyla karşılaştırır ve kötü amaçlı bir IP adresi iletişimi algılarsa sizi uyarır.

## <a name="behavioral-analytics"></a>Davranış analizi
Davranış analizi, verileri analiz eden ve bilinen modeller koleksiyonuyla karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir. Bunlar, kötü amaçlı davranışların uzman analistler tarafından dikkatlice çözümlenmesiyle de belirlenir. Azure Güvenlik Merkezi, güvenliği ihlal edilen kaynakları belirlemek amacıyla sanal makine günlükleri, sanal ağ cihazı günlükleri, yapı günlükleri, kilitlenme dökümleri ve diğer kaynakların analizine bağlı olarak davranış analizi kullanabilir.

Ayrıca, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle bir bağlantı vardır. Bu bağıntı yerleşik tehlike göstergeleriyle tutarlı olayları tanımlamaya yardımcı olur. Bazı örnekler:

* **Şüpheli işlem yürütme**: Saldırganlar tespit edilmeden kötü amaçlı yazılım yürütmeye yönelik çeşitli teknikler kullanmaktadır. Örneğin, bir saldırgan kötü amaçlı yazılıma yasal sistem dosyalarıyla aynı adı verip, bu dosyaları alternatif konumlara yerleştirebilir, iyi amaçlı bir dosyaya çok benzer bir ad kullanabilir ya da dosyanın gerçek uzantısını maskeleyebilir. Güvenlik Merkezi modelleri, davranışları işler ve işlem yürütmeleri izleyerek bunlar gibi aykırı değerleri algılar.  
* **Gizli kötü amaçlı yazılım ve istismarı denemeleri**: Karmaşık kötü amaçlı yazılımlar diske hiçbir zaman yazmayarak veya diske depolanmış yazılım bileşenlerini şifreleyerek geleneksel kötü amaçlı yazılımdan koruma ürünlerini atlatabilir.  Ancak, bu tür kötü amaçlı yazılımlar çalışmak için bellekte iz bırakmak zorunda olduğundan bellek analizi kullanılarak algılanabilir. Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar.  Kilitlenme dökümündeki belleği analiz eden Azure Güvenlik Merkezi, yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve makinenizin performansını etkilemeden tehlikeye giren bir makineye gizlice sızmak için kullanılan teknikleri algılayabilir.
* **Yana hareket ve iç keşif**: İçinde güvenliği aşılmış bir ağ ve bulmak/toplamak değerli verileri kalıcı hale getirmek için saldırganlar genellikle riskli makineden başkalarının aynı ağdaki hareket etmeye çalışır. Güvenlik Merkezi bir saldırganın ağda kapladığı yeri genişletme denemelerini bulmak için uzaktan komut yürütme, ağ araştırma ve hesap numaralandırma gibi işlem ve oturum açma etkinliklerini izler.
* **Kötü amaçlı PowerShell betikleri**: PowerShell çeşitli amaçlarla hedef sanal makinelerde kötü amaçlı kod yürütmek üzere saldırganlar tarafından kullanılıyor. Güvenlik Merkezi şüpheli etkinliklerin kanıtı için PowerShell etkinliğini inceler.
* **Giden saldırılar**: Saldırganlar genellikle bulut kaynaklarını ek saldırılar yerleştirmek üzere kullanma amacıyla bulut kaynaklarını hedefler. Örneğin, diğer sanal makinelere karşı deneme yanılma saldırıları başlatmak, istenmeyen posta göndermek veya açık bağlantı noktalarını ya da internet üzerindeki diğer cihazları taramak için riskli sanal makineler kullanılabilir. Ağ trafiğine machine learning uygulayan Güvenlik Merkezi giden ağ iletişimlerinin normu aştığını algılayabilir. İstenmeyen posta durumunda Güvenlik Merkezi, Office 365’ten alınan bilgilerle olağandışı e-posta trafiğini ilişkilendirerek, postanın kötü amaçlı ya da yasal bir e-posta kampanyasının sonucu olup olmadığını belirler.  

### <a name="anomaly-detection"></a>Anormallik algılama
Azure Güvenlik Merkezi, tehditleri tanımlamak için anormallik algılamayı da kullanır. Davranış analizinden (büyük veri kümelerinden türetilmiş bilinen modellere bağlıdır) farklı olarak anormallik algılama daha fazla “kişiselleştirilmiştir” ve dağıtımlarınıza özel taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliğini belirlemek için machine learning uygulanır ve sonra bir güvenlik olayını gösterebilecek aykırı değer koşullarını tanımlamak üzere kurallar oluşturulur. Örnek aşağıda verilmiştir:

* **Gelen RDP/SSH deneme yanılma saldırıları**: Dağıtımlarınız her gün ve çok az olan sanal makinelerin çok sayıda oturum açılmayan sanal makineler olabilir ya da hiç oturum. Azure Güvenlik Merkezi bu sanal makineler için taban çizgisi oturum açma etkinliğini belirler ve machine learning kullanarak normal oturum açma etkinliğinin dışındaki olayları belirler. Oturum açma sayısı, oturum açma saati, oturum açmanın istendiği konum veya oturum açmayla ilgili diğer özellikler taban çizgisine göre oldukça farklı ise bir uyarı oluşturulabilir. Yine machine learning neyin önemli olduğunu belirler.

## <a name="continuous-threat-intelligence-monitoring"></a>Sürekli tehdit bilgisi izleme
Azure Güvenlik Merkezi, tehdit kapsamındaki değişiklikleri sürekli olarak izleyen güvenlik araştırması ve veri bilimi ekipleri çalıştırır. Buna aşağıdaki girişimler dahildir:

* **Tehdit bilgisi izleme**: Tehdit zekası mekanizmaları, Göstergeler, uygulamaları ve var olan veya yeni ortaya çıkan tehditlere eyleme dönüştürülebilir öneriler içerir. Bu bilgileri güvenlik topluluğu içinde paylaşılır ve Microsoft, iç ve dış kaynaklardan gelen tehdit bilgisi akışlarını sürekli olarak izler.
* **Sinyal paylaşımı**: Güvenlik Öngörüler takımlar arasında Microsoft'un geniş Portföyünde Bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası cihazları paylaşılan ve analiz.
* **Microsoft Güvenlik uzmanları**: Devamlı etkileşim hukuk ve web saldırıcı algılama gibi Uzman güvenlik alanlarında çalışan ekiplerle Microsoft arasında.
* **Algılama ayarı**: Gerçek müşteri veri kümelerine göre algoritmalar çalıştırılır ve güvenlik Araştırmacıları sonuçları doğrulamak üzere müşterilerle. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleşik çabalar yeni ve geliştirilmiş algılamalarla sonuçlanır ve sizin hiçbir şey yapmanıza gerek kalmaz.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi’nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
