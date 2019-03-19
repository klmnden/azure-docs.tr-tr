---
title: Azure önizlemesindeki Gözcü not defterlerini kullanarak avcılık özellikleri | Microsoft Docs
description: Bu makalede, Azure Gözcü avcılık özellikleriyle not defterlerini kullanmayı açıklar.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 63ce2be847017ed7e80fe5e573d5553311f6af2f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58107687"
---
# <a name="use-notebooks-to-hunt-for-anomalies"></a>Anormallikleri için hunt için not defterlerini kullanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel içgörü ve ortamınızda güvenlik anormallikleri için hunt veya araştırmak için işlemler sağlamak için Jupyter etkileşimli not defterleri gücünden yararlanır. Azure Sentinel Microsoft'un güvenlik analistleri tarafından geliştirilen not defterleri ile önceden yüklenmiş olarak gelir. Her bir not defteri, belirli bir kullanım örneği için kendi içinde bir iş akışı ile oluşturulmuş amaçtır. Görselleştirmeler her not defteri için daha hızlı veri keşfi ve tehdit aramaya dahil edilmiştir. İhtiyaçlarınızı karşılamak, sıfırdan yeni bir not defterleri oluşturma veya not defterleri Azure Gözcü içeri aktarma için yerleşik not defterlerini Özelleştir ' GitHub topluluğuna. Jupyter not defterleri Azure not defterleri sayfasındaki bir proje olarak içeri aktarılan – bu kullanıcının tüm kullanıcıların Azure Gözcü not defterlerini tek bir yerden erişmesini sağlar. Not defterlerini bir düğme tıklatma ile çalıştırılabilir veya ortamlarında eşleştirmek için kullanıcı tarafından yapılandırılabilir.

Dizüstü bilgisayarlar, bulut bilgi işlem ve depolama sayesinde temel bir bakım yükü çalıştırmak tam olarak tümleştirilmiş bir deneyim sağlar. Mevcut bir Jupyter not defterleri ekosistem yararlanma olanağına kitle kaynak modelleri için müşteri verilerini paylaşmadan sağlar. 


Her bir not defteri Azure Not Defterleri (şu anda önizlemede), etkileşimli geliştirme ortamında Azure bulutunda barındırılır. Not defterleri, erişilebilir, dünyanın her yerinden herhangi bir tarayıcıdan her zaman kullanılabilir her zaman.  Azure Gözcü ' araştırma ve avcılık yerleşik not defterleri için aittir ve hangi değiştirebilir ve ortamınıza uyarlamak projesine kopyalandığı. En popüler yerleşik not defteri bazıları şunlardır:

- **Uyarı araştırma ve avcılık**: Bu etkileşimli bir not defteri uyarıları farklı sınıflardaki hızlı değerlendirme ilgili etkinliği alınıyor ve uyarı ile ilişkili etkinlik ve uyarının oluşturulduğu veri zenginleştirme sağlar.

- **Uç nokta ana bilgisayar destekli avcılık**: Güvenlik ihlalinden doğabilecek zararları emarelere bir uç nokta ana bilgisayarı ile ilişkilendirilen güvenlik ilgili etkinlikleri araştırıp bulma tarafından hunt sağlar.  

- **Office oturum avcılık destekli**: Şüpheli oturum açma işlemleri için şüpheli günlükleri coğrafi konumlarını görselleştirmenin yanı sıra Office 365 verilerden türetilmiş olağan dışı oturum açma desenlerini görüntüleme tarafından hunt olanak tanır.

## <a name="run-a-notebook"></a>Bir not defteri çalıştırma
Aşağıdaki örnekte, derin herkes beklenmeyen bir konumda ağınızdaki bir şeyler yaptığını, görmek için konumu anomaliler halinde girmeden için aranacak yerleşik not defteri çalıştırıyoruz.

1. Gözcü Azure portalında **not defterlerini** ve yerleşik not defterlerini birini seçin.
  
   ![Not defterini seçin](./media/notebooks/select-notebook.png)

3. Tıklayın **alma** için GitHub deposunu Azure not defterleri projenize kopyalayın.
   ![İçeri aktarma not defteri](./media/notebooks/import1.png)
4. Her bir not defteri hunt veya araştırma gerçekleştirme adımlarında size kılavuzluk eder. Modelleri, kitaplıklar ve diğer bağımlılıkları ve Azure Gözcü bağlantısı için yapılandırma otomatik olarak içeri aktarılır tek tıklamayla yürütmeyi etkinleştirme. Tüm kod ve not defteri çalıştırmak için gereken kitaplıkları önceden yüklenmiştir. Log Analytics çalışma alanınızın hiçbir yapılandırma karşı bir not defteri çalıştırma hemen başlayabilirsiniz.

   ![Depoyu içeri aktarma](./media/notebooks/import2.png)

5. Keşfedin, değiştirmek ve sağlanan tüm örnek not defterleri çalıştırın. Bu yapı taşı olarak birçok farklı senaryolar için kullanılabilir.

   ![Not defterini seçin](./media/notebooks/import3.png)



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Gözcü not defterlerini kullanarak bir aramaya araştırma çalıştırma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Proaktif olarak tehditleri hunt](hunting.md)
- [Aramaya çalışırken ilginç bilgileri kaydetmek için yer işaretlerini kullanma](bookmarks.md)