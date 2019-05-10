---
title: Azure Gözcü önizlemesi nedir? | Microsoft Docs
description: Azure Gözcü, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 10cce91a-421b-4959-acdf-7177d261f6f2
ms.service: sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 7d64f02c7bebb6d4326281ba87d118eab075eba9
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228593"
---
# <a name="what-is-azure-sentinel-preview"></a>Azure Gözcü önizlemesi nedir?

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Microsoft Azure Gözcü olduğu bir ölçeklenebilir, bulutta çalışan **güvenlik bilgileri olay Yönetimi (SIEM)** ve **güvenlik düzenleme otomatik yanıt (oranında ARTIRDI)** çözüm. Azure Sentinel, uyarı algılama, tehdit görünürlük, proaktif aramaya ve tehdit yanıt için tek bir çözüm sağlayarak, kuruluş genelinde akıllı güvenlik analiz ve tehdit zekası sunar. 

Azure Sentinel, Kuşbakışı stres giderek karmaşık hale gelen saldırıları ortadan kaldırılmasına kuruluş genelinde hacimli uyarılar ve uzun çözümleme zaman dilimlerine artırma görünümüdür.

- **Bulut ölçeğinde veri toplama** tüm kullanıcılar, cihazlar, uygulamalar ve altyapı, hem şirket içi arasında ve birden çok bulut. 

- **Daha önce algılanmayan tehditleri algılamak**ve Microsoft'un analizi ve benzersiz tehdit bilgilerini kullanarak hatalı pozitif sonuçları en aza indirin. 

- **Yapay zeka ile tehditleri araştırma**ve uygun ölçekte, Microsoft siber güvenlik iş yıl içinde dokunarak şüpheli etkinlikler için hunt. 

- **Olayları hızlı bir şekilde yanıtlar** yerleşik düzenleme ve Otomasyon ortak görevler ile.


![Azure Sentinel çekirdek Özellikler](./media/overview/core-capabilities.png)

Var olan Azure Hizmetleri tam aralığına oluşturma, Azure Gözcü yerel olarak Log Analytics ve Logic Apps gibi kendini kanıtlamış temellerinden içerir. Azure Sentinel yapay ZEKA, algılama ve araştırma zenginleştirir ve Microsoft'un tehdit zekası akışı sağlar ve kendi tehdit zekası getirmenize olanak sağlar. 

 
## <a name="connect-to-all-your-data"></a>Tüm verilerinize bağlanma

Yerleşik Azure Gözcü ilk gerekir [güvenlik kaynaklarınıza bağlanmak](connect-data-sources.md). Azure Sentinel Microsoft çözümleri, kutunun ve Microsoft tehdit koruması çözümlerinin yanı sıra, Office 365, Azure AD, Azure ATP dahil olmak üzere, Microsoft 365 kaynakları dahil olmak üzere, gerçek zamanlı tümleştirme sağlayan dışında kullanılabilir bağlayıcılar sayısı ile birlikte sunulur ve Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft olmayan çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca ortak olay biçimi, veri kaynaklarınızı Azure Gözcü ile de bağlanmak için Syslog veya REST API de kullanabilirsiniz.  

![Veri toplayıcılar](./media/collect-data/collect-data-page.png)

## <a name="dashboards"></a>Panolar

Veri Kaynakları'ı bağladıktan sonra bir Galeriden seçim yapabilirsiniz [ustalıkla panolar oluşturulan](quickstart-get-visibility.md#dashboards) veri kaynaklarınızdan ınsights yüzey. Her bir Pano tamamen özelleştirilebilir. - kendi mantığınızı eklemek ya da sorguları değiştirebilir veya sıfırdan bir Pano oluşturabilirsiniz.

Panolar, etkileşimli görselleştirme Yardım bir daha iyi bir saldırı sırasında neler anlamak, güvenlik analistleri için Gelişmiş analizi kullanarak sağlar. Araştırma araçlarını, derinlemesine herhangi bir alan tüm verilerdeki tehdit bağlamı hızlı bir şekilde geliştirmesini sağlar. 

![Panolar](./media/overview/dashboards.png)

## <a name="analytics"></a>Analiz

Azure Gözcü gürültüsünü azaltmak ve Uyarıları gözden geçirin ve araştırmak için sahip olduğunuz sayısını en aza indirmenize yardımcı olmak için kullandığı [ilişkilendirmek için analiz uyarılarını durumlarına](tutorial-detect-threats.md). **Çalışmaları** birlikte bir eyleme dönüştürülebilir olası-araştırmak ve çözmek tehdit oluşturan ilgili uyarıları gruplarıdır. Yerleşik bağıntı kuralları olarak kullanın-olduğu veya bunları kendi oluşturmak için başlangıç noktası olarak kullanın. Azure Sentinel Ayrıca ağ davranış'ınızı eşleyin ve ardından için anomalileri kaynaklarınız genelinde aramak için machine learning kuralları sağlar. Bu analiz, düşük güvenilirlik uyarıları farklı varlıklarla ilgili olası yüksek kaliteli güvenlik olaylarını birleştirerek nokta bağlanın.

![Durumları](./media/overview/cases.png)

## <a name="user-analytics"></a>Kullanıcı analizi

Machine learning (ML), yerel tümleştirmeleri ile ve [kullanıcı analizi](user-analytics.md), Azure Gözcü yardımcı tehditleri hızla algılayın. Azure Sentinel Gelişmiş tehdit Koruması kullanıcı davranışı çözümleyebilir ve bunların uyarıları ve şüpheli etkinlik desenlerini Gözcü Azure ve Microsoft 365 arasında göre ilk olarak araştırmanız gereken hangi kullanıcıların önceliklendirmek için Azure ile sorunsuzca tümleştirilir.

![Kullanıcı analizi](./media/overview/user-analytics.png)


## <a name="security-automation--orchestration"></a>Güvenlik otomasyon ve düzenleme

Ortak görevlerinizi otomatikleştirin ve [playbook'ları ile güvenlik düzenleme basitleştirmek](tutorial-respond-threats-playbook.md) var olan araçlarınızla yanı sıra Azure Hizmetleri ile tümleştirin. Azure Logic Apps foundation üzerinde oluşturulan, otomasyon ve düzenleme çözümü Azure Gözcü'nın yeni bir teknoloji olarak ölçeklenebilir Otomasyon sağlayan yüksek düzeyde genişletilebilir bir mimari sağlar ve tehditleri ortaya çıkmaya başladı. Azure Logic Apps playbook'ları oluşturmak için büyümekte olan bir yerleşik playbook'ları Galeriden seçebilirsiniz. Bunlar [200'den fazla bağlayıcı](https://docs.microsoft.com/azure/connectors/apis-list) Azure işlevleri gibi hizmetler için. Bağlayıcılar herhangi kod, ServiceNow, Jıra, Zendesk, HTTP istekleri, Microsoft Teams, Slack, Windows Defender ATP ve Cloud App Security özel mantığı uygulamanıza imkan sağlar.

Örneğin, ServiceNow bilet sistemi kullanıyorsanız, Azure Logic Apps iş akışlarınızı otomatikleştirin ve ServiceNow'ı her zaman belirli bir olay algılandığında bir bilet kullanmak için sağlanan araçları kullanabilirsiniz.

![Playbook'lar](./media/tutorial-respond-threats-playbook/logic-app.png)



## <a name="investigation"></a>Araştırma

Azure Sentinel [derin araştırma](tutorial-investigate-cases.md) araçları anlamak ve olası bir güvenlik tehdidi kök nedenini bulmak için Yardım. Belirli bir varlık için ilgi çekici soru sorun ve bu varlığın ve tehdit kök nedenini almak için kendi bağlantıları incelemek için etkileşimli grafikteki bir varlığı seçebilirsiniz. 

![Araştırma](./media/overview/investigation.png)


## <a name="hunting"></a>Avlanma

Azure Gözcü'nın kullanın [güçlü arama sorgusu araçları avcılık](hunting.md)göre MITRE framework, bir uyarının tetiklenmesinden önce güvenlik tehditleri için kuruluşunuzun veri kaynaklarında proaktif olarak hunt olanak sağlar. Hangi avcılık sorgu olası saldırıları yüksek değerli Öngörüler sağlar bulduktan sonra sorguyu temel alan özel algılama kuralları oluşturma da bu Öngörüler, güvenlik olay Yanıtlayıcı uyarıları yüzey. Aramaya çalışırken, bunları daha sonra geri, bunları başkalarıyla paylaşmanızı ve araştırma için ilgi çekici bir çalışmasının ilişkilendirilmesini diğer olaylarla gruplamak olanak sağlayan ilgi çekici olayları için yer işaretleri oluşturabilirsiniz.

![Avlanma](./media/overview/hunting.png)

## <a name="community"></a>Topluluk

Azure Gözcü topluluk, tehdit algılama ve Otomasyon için güçlü bir kaynaktır. Bizim Microsoft Güvenlik analistleri, sürekli olarak oluşturun ve bunları ortamınızda kullanabilmeniz için topluluk nakil yeni panolar, playbook'ları, aramaya sorguları ve diğer ekleyin. Özel topluluktan GitHub örnek içerik indirip indiremeyeceğini [depo](https://aka.ms/asicommunity) Azure Gözcü için özel panolar, aramaya sorguları, not defterlerini ve playbook'ları oluşturmak için. 

![Topluluk](./media/overview/community.png)

## <a name="next-steps"></a>Sonraki adımlar

- Gözcü Azure ile çalışmaya başlamak için Microsoft Azure aboneliği gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Bilgi nasıl [ekleme verilerinizi Azure Gözcü](quickstart-onboard.md), ve [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
