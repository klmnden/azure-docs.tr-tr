---
title: Gözcü Azure Önizlemedeki özellikler avcılık | Microsoft Docs
description: Bu makalede, Azure Gözcü avcılık işlevlerini nasıl kullanacağınız açıklanır.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: adedc8bc1f574ae089f2a11033fab4f390c57a9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714865"
---
# <a name="hunt-for-threats-with-in-azure-sentinel-preview"></a>Azure Önizleme Gözcü ile tehditleri hunt

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Güvenlik tehditleri için arama hakkında proaktif olarak isteyen bir araştırmacı değilseniz Azure Gözcü güçlü güvenlik tehditleri için kuruluşunuzun veri kaynaklarında hunt arama ve sorgulama araçları avcılık. Ancak, sistem ve güvenlik Gereçleri dağlarında yürüyüş ayrıştırma ve anlamlı olayları filtrelemek zor olabilir veri oluşturur. Güvenlik amacıyla analistleri proaktif olarak güvenlik tarafından uygulamalarınıza Azure Gözcü algılanan olmayan yeni anomalileri arayın ' yerleşik avcılık sorguları konusunda size sorunları ağınızda zaten sahip verileri bulmak için doğru soruları sormaya içine. 

Örneğin, yerleşik bir sorgu altyapınızda çalışan en yaygın olmayan işlemler hakkında veri sağlar - çalıştıkları her zaman hakkında uyarı istemezsiniz, tamamen zararsız olabilir ancak, bazen görmek için sorguyu göz atmak isteyebilirsiniz th ere olağan dışı bir şey güçlendirin. 



Azure Gözcü avcılık ile aşağıdaki özelliklerden gerçekleştirebilirsiniz:

- Yerleşik sorgular: Başlamanıza yardımcı olmak için bir başlangıç sayfası sağlamak için tasarlanan önceden yüklenmiş sorgu örnekleri başlatıldı ve tabloları ve sorgu dili ile tanımak sağlar. Bu yerleşik avcılık sorguları yeni sorgular ekleme Microsoft Güvenlik Araştırmacıları tarafından sürekli olarak geliştirilir ve ince ayar yapma mevcut sorgular için yeni algılamalar ve avcılık için başlangıç noktası ekleyeceğimi bir giriş noktası sağlamak için Yeni saldırı beginnings. 

- IntelliSense ile güçlü sorgu dili: Avcılık düzeye yapmanız esneklik sunan bir sorgu dili üzerine kurulmuştur.

- Kendi yer işaretleri oluşturabilirsiniz: Avcılık işlemi sırasında üzerinde eşleşen veya bulguları, panolar veya olağan dışı veya şüpheli etkinlikleri gelebilir. Bu öğeler, geri kendisine gelecekte gelebilirim işaretlemek için yer işareti işlevlerini kullanın. Yer işaretleri öğeleri daha sonra kullanmak üzere, bir olayı araştırma oluşturmak için kullanılacak kaydetmenize olanak tanır. Yer işaretleri hakkında daha fazla bilgi için kullanım [avcılık yer işaretleri] bakın.

- Araştırma otomatik hale getirmek için not defterlerini kullanma: Not defterlerini araştırmasının adım adım ve hunt oluşturabileceğinizi adım adım playbook'ları gibi ' dir.  Not defterleri, kuruluşunuzdaki diğer kullanıcılarla paylaşılabilir yeniden kullanılabilir bir playbook avcılık adımların tümünü kapsüller. 
- Depolanan verilerde sorgu: Verileri tablolarda, sorgulama erişilebilir. Örneğin, işlem oluşturma, DNS olayları ve diğer birçok olay türleri sorgulayabilirsiniz.

- Topluluk bağlantıları: Ek sorgu ve veri kaynaklarını bulmak için büyük topluluk gücünden yararlanın.
 
## <a name="get-started-hunting"></a>Aramaya başlayın

1. Gözcü Azure portalında **avcılık**.
  ![Azure Sentinel aramaya başlar.](media/tutorial-hunting/hunting-start.png)

2. Açtığınızda **avcılık** sayfasında, tüm avcılık sorgular tek bir tabloda görüntülenir. Tabloda herhangi ek sorgu oluşturulduğunda veya değiştirildiğinde yanı sıra güvenlik analistleri Microsoft'un ekibi tarafından yazılan tüm sorgular listelenir. Her sorgu için bunu avlamak ve ne tür veriler üzerinde çalıştığı bir açıklaması verilmiştir. Bu şablonlar, çeşitli taktikleri tarafından gruplandırılır - ilk erişim, Kalıcılık ve sızdırma gibi bir tehdit türü simgeleri sağdaki kategorilere ayırın. Bu avcılık sorgu şablonları tüm alanları kullanarak filtreleyebilirsiniz. Herhangi bir sorguyu Sık Kullanılanlara kaydedebilirsiniz. Bir sorguyu Sık Kullanılanlara kaydederek sorguyu otomatik olarak her çalıştığında **avcılık** sayfa erişildiğinde. Kendi avcılık sorgu ya da kopyalama oluşturabilir ve varolan avcılık sorgu şablonunu özelleştirin. 
 
2. Tıklayın **sorgusu** içinde avcılık avcılık sayfasından çıkmadan herhangi bir sorgu çalıştırmak için Sorgu Ayrıntıları sayfası.  Eşleşme sayısı, tablo içinde görüntülenir. Aramaya sorgular ve bunların eşleşme listesini gözden geçirin. Eşleşme ile ilişkili sonlandırma zinciri hangi aşamasında göz atın.

3. Sorgu Ayrıntılar bölmesinde temel alınan sorgunun hızlı bir inceleme gerçekleştirme veya tıklayın **sorgu sonucu görüntülemek** Log Analytics'te sorgu açın. En altında sorgunun eşleşmeleri gözden geçirin.

4.  Tıklayarak seçin ve satır **Ekle yer işareti** araştırılması için - satır eklemek için bunu şüpheli görünen şeyler yapabilirsiniz. 

5. Ardından, ana dönün **avcılık** sayfasında ve tıklayın **yer işaretleri** tüm şüpheli etkinlikleri görmek için sekmesinde. 

6. Bir yer işaretini seçin ve ardından **Araştır** araştırma deneyimi açın. Yer işaretleri filtreleyebilirsiniz. Örneğin, bir kampanya araştırdığınızı, kampanya için bir etiket oluşturmak ve ardından kampanyaya dayalı tüm yer imlerini filtreleyin.

1. Hangi avcılık sorgu olası saldırıları yüksek değerli Öngörüler sağlar bulunan sonra kuralları, sorguyu temel alan ve bu Öngörüler, güvenlik olay Yanıtlayıcı uyarıları yüzey özel algılama da oluşturabilirsiniz.

 

## <a name="query-language"></a>Sorgu dili 

Azure Gözcü içinde avcılık Azure Log Analytics sorgu diline bağlıdır. Sorgu dili ve desteklenen işleçleri hakkında daha fazla bilgi için bkz. [sorgu dili başvurusu](https://docs.loganalytics.io/docs/Language-Reference/).

## <a name="public-hunting-query-github-repository"></a>Aramaya genel sorgu GitHub deposu

Kullanıma [avcılık sorgu deposu](https://github.com/Azure/Orion). Katkıda bulunan ve müşterilerimiz tarafından paylaşılan örnek sorguları kullanın.

 

## <a name="sample-query"></a>Örnek sorgu

Tipik bir sorgu işleçleri ile ayrılmış bir dizi arkasından bir tablo adı ile başlar \|.

Başlangıç tablosuyla yukarıdaki örnekte SecurityEvent adlandırın ve yöneltilen öğeleri gerektiği gibi ekleyin.

1. Yalnızca son yedi gün kayıtları gözden geçirmek için süresi filtre tanımlar.

2. Yalnızca olay kimliği 4688 göstermek için sorguya bir filtre ekleyin.

3. Sorgu yalnızca cscript.exe örneklerini içeren komut satırı üzerinde bir filtre ekleyin.

4. Yalnızca araştırma içinde ilginizi çeken ve 1000 sonuçları sınırlandırmak ve tıklayın sütunları proje **sorgusu**.
5. Yeşil üçgene tıklayın ve sorguyu çalıştırın. Sorguyu test etmek ve anormal davranış için aranacak çalıştırın.

## <a name="useful-operators"></a>Yararlı işleçler

Sorgu dili, güçlü ve birçok kullanılabilir işleçleri olan, burada listelenen bazı yararlı işleçleri:

**Burada** -bir tabloda bir koşulu karşılayan satırların alt filtre.

**Özetleme** -girdi tablosunun içeriği toplayan bir tablo oluşturur.

**birleştirme** -eşleşen değerler her tablodan belirtilen sütunların tarafından yeni bir tablo oluşturmak için iki tablo satırları birleştirir.

**sayısı** -girdi kayıt kümesine kayıt sayısını döndürür.

**üst** -Return ilk N kayıtları belirtilen sütunlara göre sıralanır.

**sınırı** -kadar belirtilen satır sayısını döndürür.

**Proje** - içermez, yeniden adlandırmak veya drop sütunları seçin ve yeni hesaplanmış sütun ekleyin.

**genişletme** - hesaplanan sütunlar oluşturabilir ve bunları sonuç kümesine ekleyin.

**makeset** -gruba bir dinamik ifade alan farklı değerler kümesini (JSON) dizisi döndürür

**bulma** -tabloları kümesi arasında bir koşula uyan satırları bulmak.

## <a name="save-a-query"></a>Bir sorguyu Kaydet

Oluşturun veya bir sorguyu değiştirmek ve kendi sorgusu olarak kaydetmek veya aynı kiracıya kullanıcılar ile paylaşabilirsiniz.

   ![Sorguyu kaydet](./media/tutorial-hunting/save-query.png)

Yeni bir aramaya sorgu oluşturun:

1. Tıklayın **yeni sorgu** seçip **Kaydet**.
2. Tüm boş alanları doldurun ve seçin **Kaydet**.

   ![Yeni sorgu](./media/tutorial-hunting/new-query.png)

Kopyalama ve avcılık sorguyu değiştirin:

1. Değiştirmek istediğiniz tabloda avcılık sorguyu seçin.
2. Üç nokta (...) satırı değiştirin ve istediğiniz sorguyu seçin **sorguyu Klonla**.

   ![Sorguyu Klonla](./media/tutorial-hunting/clone-query.png)
 

3. Seçin ve sorguyu değiştirmek **Oluştur**.

   ![Özel sorgu](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Gözcü ile avcılık araştırma çalıştırma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:


- [Otomatik avcılık kampanyaları için not defterlerini kullanma](notebooks.md)
- [Aramaya çalışırken ilginç bilgileri kaydetmek için yer işaretlerini kullanma](bookmarks.md)