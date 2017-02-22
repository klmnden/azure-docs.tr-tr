---
title: "Azure Güvenlik Merkezi&quot;nde güvenlik uyarılarını yönetme | Microsoft Docs"
description: "Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 9852981e530cd147c2d34ac2ede251b58a167a0a
ms.openlocfilehash: 0093beda6af6d3b2400185fcd63bc73b2c8c9ba9


---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi’ni kullanmanıza yardımcı olur.

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. 90 günlük ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](security-center-policies.md)’nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).
>
>

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.


> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.
>
>

## <a name="managing-security-alerts"></a>Güvenlik ilkelerini yönetme
**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Azure Portal’ı açın ve her bir uyarı hakkında daha fazla ayrıntıya ulaşmak için aşağıdaki adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Aşağıda gösterildiği gibi, uyarılar hakkında daha fazla ayrıntı içeren **Güvenlik uyarıları** dikey penceresini açmak için kutucuğa tıklayın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları dikey penceresi](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Bu dikey pencerenin alt bölümünde her bir uyarı için ayrıntılar bulunur. Sıralamak için hangi sütuna göre sıralamak istediğinizi belirtin. Her sütunun tanımı aşağıda verilmiştir:

* **Açıklama**: Uyarının kısa bir açıklaması.
* **Sayı**: Belirtilmiş türün belirli bir günde algılanan tüm uyarılarının listesi.
* **Algılayan**: Uyarıyı tetiklemekten sorumlu hizmet.
* **Tarih**: Olayın gerçekleştiği tarih
* **Durum**: Bu uyarı için geçerli durum. İki tür durum mevcuttur:
  * **Etkin**: Güvenlik uyarısı algılandı.
* **Önem derecesi**: Yüksek, orta veya düşük önem düzeyi.

### <a name="filtering-alerts"></a>Uyarıları filtreleme
Tarihe, duruma ve önem derecesine göre uyarıları filtreleyebilirsiniz. Filtreleme uyarıları, güvenlik uyarıları gösterimi kapsamını daraltmanızın gerektiği senaryolar için faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

1. **Güvenlik Uyarıları** dikey penceresindeki **Filtreleme**'ye tıklayın. **Filtreleme** dikey penceresi açılır ve görmek istediğiniz tarih, durum ve önem derecesi değerlerini seçersiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama
Uyarıyı tetikleyen olay(lar) ve saldırıyı düzeltmek için (varsa) hangi adımları atmanız gerektiği hakkında daha fazla bilgi edinmek için bir güvenlik uyarısı seçin. Güvenlik uyarıları, türe ve tarihe göre gruplandırılır. Güvenlik uyarısına tıklandığında gruplanan uyarıların listesini içeren dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

Bu durumda, tetiklenen uyarılar şüpheli Uzak Masaüstü Protokolü (RDP) etkinliğine işaret eder. Birinci sütunda hangi kaynakların saldırıya uğradığı; ikinci sütunda kaynağın kaç kez saldırıya uğradığı; üçüncü sütunda saldırının zamanı; dördüncü sütunda uyarının durumu ve beşinci sütunda saldırının önem derecesi gösterilir. Bu bilgileri gözden geçirdikten sonra, saldırıya uğrayan kaynağa tıkladığınızda yeni bir dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarıları hakkında ne yapılacağına ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

Bu dikey pencerenin **Açıklama** alanında bu olay hakkında daha fazla ayrıntı bulabilirsiniz. Bu ek ayrıntılar, güvenlik uyarısını neyin tetiklediği, hedef kaynak, uygulanabilirse kaynak IP adresi ve düzeltmeye ilişkin öneriler hakkında öngörüler sunar.  Windows güvenlik olayı günlüklerinin tümü IP adresi içermediğinden, bazı durumlarda kaynak IP adresi boştur (yoktur).

Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir. Bazı durumlarda, önerilen düzeltmeyi uygulamak için diğer Azure işlevlerini kullanmak zorunda kalabilirsiniz. Örneğin, bu saldırıya ilişkin düzeltme, [ağ ACL'si](../virtual-network/virtual-networks-acl.md) veya [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) kuralı kullanarak bu saldırıyı gerçekleştiren IP adresini kara listeye almaktır.

> [!NOTE]
> Farklı uyarı türleri hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md) makalesini okuyun.
>
>

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
* [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Feb17_HO1-->


