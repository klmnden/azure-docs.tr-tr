---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme | Microsoft Docs
description: Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: rkarlin
ms.openlocfilehash: 582912160c8ed514401be3522e52dcc6eb45d263
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235780"
---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi’ni kullanmanıza yardımcı olur.

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. Ücretsiz bir deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](tutorial-security-policy.md)’nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırması](security-center-pricing.md).
>
>

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.


> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.
>
>

## <a name="managing-security-alerts"></a>Güvenlik ilkelerini yönetme
**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Uyarılar hakkında daha fazla ayrıntıya ulaşmak için şu adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Uyarılar hakkında daha fazla ayrıntı içeren **Güvenlik uyarıları** sayfasını açmak için kutucuğa tıklayın.

   ![Güvenlik Merkezi'nde Güvenlik uyarıları](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Bu sayfanın alt bölümünde her bir uyarı için ayrıntılar bulunur. Sıralamak için hangi sütuna göre sıralamak istediğinizi belirtin. Her sütunun tanımı aşağıda verilmiştir:

* **Açıklama**: Uyarının kısa bir açıklama.
* **Sayısı**: Belirli bir günde algılanan bu belirli türdeki tüm uyarıların bir listesi.
* **Tarafından algılanan**: Uyarıyı tetiklemekten sorumlu hizmet.
* **Tarih**: Olayın gerçekleştiği tarih.
* **Durum**: Bu uyarı için geçerli durumu. İki tür durum mevcuttur:
  * **Etkin**: Güvenlik Uyarısı algılandı.
  * **Kapatıldı**: Güvenlik Uyarısı, kullanıcı tarafından kapatıldı. Bu durum genellikle, araştırılan uyarılar için kullanılır ve gerçek bir saldırı olarak azaltılan ya da değil.
* **Önem derecesi**: Önem düzeyi yüksek, Orta veya düşük olabilir.

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan güvenlik uyarıları, Azure Etkinlik Günlüğü altında ayrıca görünür. Azure Etkinlik Günlüğü’ne erişim hakkında daha fazla bilgi için bkz. [Kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüle](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
>


### <a name="alert-severity"></a>Uyarı önem derecesi

-   **Yüksek**: Kaynağınızın tehlikeye girmemesini yüksek olasılık yoktur. İçine hemen görünmelidir. Güvenlik Merkezi, hem kötü amaçlı bir eyleme ve uyarı vermek üzere kullanılan bulguları yüksek güvenilirliğe sahip. Örneğin, mimikatz'ı kimlik bilgisi hırsızlığı için kullanılan genel bir aracı gibi bilinen bir kötü amaçlı araç yürütülmesini algılarsa bir uyarı. 
-   **Orta**: Bir kaynak tehlikede olduğunu gösterebilecek kuşkulu bir etkinlik büyük olasılıkla budur.
Güvenlik Merkezi'nin ellerde analitik veya arama Orta ve kötü amaçlı güvenini Orta Yüksek. Bunlar genellikle machine learning'e ya da tabanlı anomali algılama olur. Örneğin, bir oturum açma denemesi, anormal bir konumdan.
-   **Düşük**: Bu, zararsız pozitif ya da engellenen bir saldırı olabilir. 
    - Güvenlik Merkezi amaç kötü amaçlı ve etkinlik zararsız başarılara yeterli değil. Örneğin, günlük Temizle izlerini gizlemek bir saldırgan çalışır, ancak çoğu durumda, sıradan bir işlem yöneticileri tarafından gerçekleştirilir, bu sorunla karşılaşabilirsiniz bir eylemdir.
    - İçine bak öneririz ilgi çekici bir durum değilse Güvenlik Merkezi genellikle zaman saldırıları engellendi, sunmayacaktır. 
-   **Bilgilendirme**: Bir güvenlik olayı altında ayrıntıya veya belirli bir uyarı kimliği ile REST API kullanıyorsanız, yalnızca bilgilendirici uyarılar görürsünüz Olay uyarıları, bazıları, yalnızca bilgilendirici olacak şekilde kendi görünebilir, ancak diğer uyarılar bağlamında daha yakından bakın bu durum bir dizi genellikle oluşur.  

> [!NOTE]
> Kullanıyorsanız **2015-06-01-preview** API sürümü, daha sonra hangi alarma önem derecesi türleri uygulanır hangi senaryoları için hangi yukarıda listelenen gelen farklar vardır.  

### <a name="filtering-alerts"></a>Uyarıları filtreleme
Tarihe, duruma ve önem derecesine göre uyarıları filtreleyebilirsiniz. Filtreleme uyarıları, güvenlik uyarıları gösterimi kapsamını daraltmanızın gerektiği senaryolar için faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

1. **Güvenlik Uyarıları**'ndaki **Filtreleme**'ye tıklayın. **Filtreleme** açılır ve görmek istediğiniz tarih, durum ve önem derecesi değerlerini seçersiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama
Uyarıyı tetikleyen olay(lar) ve saldırıyı düzeltmek için (varsa) hangi adımları atmanız gerektiği hakkında daha fazla bilgi edinmek için bir güvenlik uyarısı seçin. Güvenlik uyarıları, türe ve tarihe göre gruplandırılır. Güvenlik uyarısına tıklandığında gruplanan uyarıların listesini içeren sayfa açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

Bu durumda, tetiklenen uyarılar şüpheli Uzak Masaüstü Protokolü (RDP) etkinliğine işaret eder. Birinci sütunda hangi kaynakların saldırıya uğradığı; ikinci sütunda kaynağın kaç kez saldırıya uğradığı; üçüncü sütunda saldırının zamanı; dördüncü sütunda uyarının durumu ve beşinci sütunda saldırının önem derecesi gösterilir. Bu bilgileri gözden geçirdikten sonra, saldırıya uğrayan kaynağa tıklayın.

![Azure Güvenlik Merkezi'nde güvenlik uyarıları hakkında ne yapılacağına ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

**Açıklama** alanında bu olay hakkında daha fazla ayrıntı bulabilirsiniz. Bu ek ayrıntılar, güvenlik uyarısını neyin tetiklediği, hedef kaynak, uygulanabilirse kaynak IP adresi ve düzeltmeye ilişkin öneriler hakkında öngörüler sunar.  Windows güvenlik olayı günlüklerinin tümü IP adresi içermediğinden, bazı durumlarda kaynak IP adresi boştur (yoktur).

Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir. Bazı durumlarda, önerilen düzeltmeyi uygulamak için diğer Azure işlevlerini kullanmak zorunda kalabilirsiniz. Örneğin, bu saldırıya ilişkin düzeltme, [ağ ACL'si](../virtual-network/virtual-networks-acl.md) veya [ağ güvenlik grubu](../virtual-network/security-overview.md#security-rules) kuralı kullanarak bu saldırıyı gerçekleştiren IP adresini kara listeye almaktır. Farklı uyarı türleri hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde Türe Göre Güvenlik Uyarıları](security-center-alerts-type.md) makalesini okuyun.

> [!NOTE]
> Güvenlik Merkezi Linux makinelerdeki kötü amaçlı davranışlarını algılamak için denetim kayıtlarını kullanan yeni bir algılama kümesi ve ortak denetim çerçevesi içeren sınırlı bir önizleme sürümüyle yayımlanmıştır. Önizlemeye katılmak için lütfen abonelik kimliklerinizi [bize](mailto:ASC_linuxdetections@microsoft.com) e-posta ile gönderin.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
* [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
