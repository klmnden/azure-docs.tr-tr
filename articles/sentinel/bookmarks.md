---
title: Azure Gözcü avcılık yer işaretlerini kullanma önizlemesinde avcılık veri kaydını | Microsoft Docs
description: Bu makalede Azure Gözcü avcılık yer işaretleri veri izlemek için nasıl kullanılacağını açıklar.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 320ccdad-8767-41f3-b083-0bc48f1eeb37
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: b1a438b9645dbb37d852eb0092355850d816872d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65207459"
---
# <a name="keep-track-of-data-during-hunting"></a>Veri aramaya sırasında izler

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Tehdit avcılık genellikle dağlarında yürüyüş arayan kötü amaçlı davranışları kanıtı için günlük verileri gözden geçirme gerektirir. Bu işlem sırasında araştırmacıya unutmayın, yeniden ziyaret ve hikayenin bir tehlike anlama ve olası hipotezi doğrulama kapsamında çözümlemek istediğiniz olayları bulun.
Aramaya yer işaretleri, ilgili bulduğunuz sorgu sonuçları yanı sıra Log Analytics'te çalıştırılan sorgular koruma tarafından bunu yapmanıza yardımcı olmak. Ayrıca, bağlamsal gözlemler kaydetmek ve notları ve etiketler ekleyerek bulgularınızı başvuru. İşaretli verileri arkadaşlarınızla kolayca işbirliği için görülebilir.   

İşaretli verileriniz üzerinde herhangi bir zamanda erişebilirsiniz **yer işareti** sekmesinde **avcılık** sayfası. Filtrelemeyi kullanın ve Seçenekler belirli verileri geçerli araştırmanızı için hızlı bir şekilde bulmak için arama yapın. Alternatif olarak, doğrudan işaretli verilerinizi görüntüleyebileceğiniz **HuntingBookmark** Log analytics'te tablo. Bu, filtre, özetlemenize ve işaretli verileri diğer veri kaynaklarıyla kanıt corroborating için aranacak kolaylaştırma sağlar.

Tıklayarak işaretli verilerinizi görselleştirebilirsiniz **Araştır**. Bu araştırma deneyimi görüntülemek, araştırmak ve görsel bir etkileşimli varlık grafik Diyagram ve zaman çizelgesi kullanarak bulgularınızı iletişim başlatır.


## <a name="run-a-log-analytics-query-from-azure-sentinel"></a>Azure Gözcü bir Log Analytics sorgusu çalıştırma

1. Gözcü Azure portalında **avcılık** şüpheli ve anormal davranış sorguları çalıştırmak için.

1. Aramaya kampanya çalıştırmak için bir aramaya sorguların ve sol, inceleme üzerinde sonuçları seçin. 

1. Tıklayın **sorgu sonuçları görüntüle** avcılık sorgusunda **ayrıntıları** Log Analytics'te sonuçlar sorguyu görüntülemek için sayfası. Özel SSH bruteforce saldırı sorgu dönüştürdüyseniz bkz örneği aşağıda verilmiştir.
  
   ![sonuçları göster](./media/bookmarks/ssh-bruteforce-example.png)

## <a name="add-a-bookmark"></a>Bir yer işareti Ekle

1. Log Analytics sorgu sonuçları listesinde ilgi duyduğunuz bilgileri içeren satırını genişletin.

4. Satır sonundaki üç nokta (...) seçip **avcılık yer işaretleri ekleme**.
5. Sağ taraftaki içinde **ayrıntıları** sayfasında adını güncelleştirin ve etiketleri ve ne hakkında öğesi ilginç tanımlamanıza yardımcı olacak notlar ekleyin.
6. Tıklayın **Kaydet** değişikliklerinizi işleyebilirsiniz. Tüm işaretli verileri diğer araştırmacıya ile paylaşılır ve işbirliğine dayalı araştırma deneyimi yönelik ilk adımdır.

   ![sonuçları göster](./media/bookmarks/add-bookmark-la.png)

 
> [!NOTE]
> Yer işaretleri, Azure Log Analytics günlükleri Gözcü sayfasından veya Log Analytics sayfasından yaklaştığında ve avcılık sayfasından açılan oluşturulan sorgular başlatılan rastgele Log Analytics sorguları ile de kullanabilirsiniz. Log Analytics'ten Azure Gözcü dışında başlatmak, bir yer işareti eklemek mümkün olmayacaktır. 

## <a name="view-and-update-bookmarks"></a>Görüntüleme ve güncelleştirme yer işaretleri 

1. Gözcü Azure portalında **avcılık**. 
2. Tıklayın **yer işaretleri** yer işaretleri listesini görüntülemek için sayfanın ortasındaki sekmesi.
3. Belirli bir yer işareti bulmak için arama kutusu veya filtre seçeneklerini kullanın.
4. Sağ taraftaki Ayrıntılar bölmesinde yer işareti ayrıntılarını görüntülemek için kılavuzunda yer işaretlerine ayrı ayrı seçin.
5. Etiketler ve notlar güncelleştirmek için düzenlenebilir metin kutuları ve **Kaydet** değişikliklerinizi korumak için.

   ![sonuçları göster](./media/bookmarks/view-update-bookmarks.png)

## <a name="view-bookmarked-data-in-log-analytics"></a>Log analytics'te veri bozulmasına görüntüle 

İşaretli verilerinizi Log Analytics'te görüntülemek için birden çok seçenek vardır. 

İstenen yer işaretini seçerek işaretli sorguları, sonuçları veya geçmişini görüntülemek için en kolay yolu olan **yer işaretleri** tablo ve Ayrıntılar bölmesinde sağlanan bağlantıları kullanabilirsiniz. Şu seçenekler mevcuttur: 
- Tıklayarak **sorguyu görüntüle** Log Analytics'te kaynak sorguyu görüntülemek için.  
- Tıklayarak **yer işareti geçmişi görüntüleyebilir** görmek için meta veriler dahil olmak üzere tüm yer işareti: güncelleştirme, güncelleştirilmiş değerleri ve güncelleştirme zamanını kimin yaptığını. 

- Tıklayarak tüm yer imlerini ham yer işareti verilerini görüntüleyebilirsiniz **yer işareti günlükleri** yukarıda yer işareti kılavuz. Bu görünüm tüm işaretlerinizi avcılık yer işareti tablosunda ilişkili meta verileri gösterir. En son sürümünü aradığınız belirli yer işareti aşağı filtrelemek için KQL sorguları kullanabilirsiniz.  


> [!NOTE]
> Bir yer işareti oluşturulmasını ve ne zaman görüntüleneceğini arasında önemli gecikme (dakika cinsinden ölçülür) olabilir **HuntingBookmark** tablo. Yer işaretlerinizi oluşturun ve sonra veri alınan ve sonra bunları çözümlemek için önerilir. 

## <a name="delete-a-bookmark"></a>Bir yer işareti Sil
Aşağıdaki yer işareti do silmek istiyorsanız: 
1.  Th açın **avcılık yer işareti** sekmesi. 
2.  Hedef yer işaretini seçin.
3.  Üç noktasını (...) seçin ve satır sonunda **silme yer işareti**.
    
Yer işareti silmeden kaldırır yer işareti listeden **yer işareti** sekmesi.  Log Analytics "HuntingBookmark" Tablo önceki yer işareti girişleri devam eder, ancak en son giriş değiştirecek **SoftDelete** değeri true olarak eski yer işaretleri filtre kolaylaştırır.  Bir yer işareti silme herhangi bir varlık diğer yer işaretleri veya uyarılar ile ilişkili olan araştırma deneyiminden kaldırmaz. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Gözcü içinde yer işaretlerini kullanma bir aramaya araştırma çalıştırma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:


- [Proaktif olarak tehditleri hunt](hunting.md)
- [Otomatik avcılık kampanyaları için not defterlerini kullanma](notebooks.md)