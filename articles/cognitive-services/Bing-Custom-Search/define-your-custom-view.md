---
title: 'Bing özel arama: özel bir görünüm tanımlama | Microsoft Docs'
description: Bir site ve dikey arama hizmetleri nasıl oluşturulacağını açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 8ffe3087df398d6310828e41d0c6992199fafbed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352906"
---
# <a name="configure-your-custom-search-experience"></a>Özel arama deneyiminiz yapılandırın
Özel arama örneği, kullanıcılarınızın ilgilendiğiniz Web siteleri içerik dahil etmek arama deneyimine olanak tanır. Bir web genelinde arama gerçekleştirmek yerine, yalnızca ilgilendiğiniz web slice Bing arar.
Web özel görünümünü oluşturmak için Bing özel arama kullanın [portal](https://customsearch.ai). Portal oturumu açmada hakkında daha fazla bilgi için bkz: [ilk Bing özel arama örneğinizi oluşturmak](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). Portal, etki alanları, alt ve Bing arama yapmak istediğiniz Web sayfalarını ve arama yapmak istemiyorsanız bu belirten bir arama örneği oluşturmanızı sağlar. URL'ler hakkında bilmeniz içeriğin belirtmeye ek olarak, görünümünüze eklemek istediğiniz içerik önermek için portal sorabilirsiniz. Bir dilim Web tanımlayabilirsiniz yollar şunlardır: 

1.  Etki alanı. Bir etki alanı dilim bir Internet etki alanı içinde bulunan tüm içeriği kapsar. Örneğin, www.microsoft.com. 'Www' atlanması ayrıca etki alanının alt etki alanları arama yapmak için Bing neden olur. Microsoft.com belirtirseniz, örneğin, Bing ayrıca sonuçları support.microsoft.com veya technet.microsoft.com döndürür.
2.  Alt sayfa. Bir alt dilim altındaki yolları ve alt bulunan tüm içeriği kapsar. En fazla iki alt yolu belirtebilirsiniz. Örneğin, www.microsoft.com/en-us/windows/ 
3.  Web sayfası. Bir Web sayfası dilim yalnızca bu Web sayfası özel bir aramada içerebilir. İsteğe bağlı olarak alt sayfalar dahil edilip edilmeyeceğini belirtebilirsiniz.

Tüm etki alanları, alt ve belirttiğiniz Web sayfaları, ortak ve Bing tarafından oluşturulmuş olması gerekir. Aramada dahil etmek istediğiniz ortak bir site kendi ve Bing bu dizine kurmadı Bing bkz [yayımlanması belgelerine](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) bu dizin için Bing alma hakkında ayrıntılar için. Ayrıca, dizin güncel ise gezilen sitenizi güncelleştirmek için Bing alma hakkında ayrıntı yayımlanması belgelerine bakın.

## <a name="adding-slices-to-your-custom-search"></a>Özel aramanızı dilimler ekleme
Özel arama örneğinizi tanımladığınızda, etkin ve engellenen etki alanları, alt ve arama veya değil arama yapmak istediğiniz Web sayfalarını belirtin.  

- Etkin: Etki alanları, alt ya da Web sayfalarının aramasına dahil etmek için bir liste. 
- Engellendi: Listesini etki alanı, alt veya arama dışlamak için Web sayfaları. Etki alanları ve alt altında bulunan, blok içerik olmalıdır öğeleri etkin listenizde listelenir.

Her liste erişmek için özel arama örneğinizi etkin ve bloke sekmeleri tıklatın. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Etkin ve bloke listeler 
Bing arama yapmak istediğiniz web dilimin belirtmek için tıklatın **etkin** sekmesinde ve etki alanları, alt ve aramak için Web sayfalarının listesi. Bir dilim doğrudan listeye eklemek veya karşıya yükleme simgesini kullanarak bir metin dosyası karşıya yükleyerek birden fazla dilim ekleyin.

Dosya karşıya yükleme ayrıntıları: 

- Karşıya dosya yükleme dilimler etkin listeye yalnızca eklemek için kullanılabilir ve dilimler bloke listeye eklemek için kullanamazsınız. 
- Bir metin dosyası oluşturun ve tek bir etki alanı, alt veya Web sayfası her satıra belirtin. Bir hata oluşursa, tüm karşıya yükleme reddedilir. 
- Etki alanı, alt veya karşıya yükleme dosyasında belirtilen Web sayfasının bloke liste içeriyorsa, hizmet bloke listeden kaldırır ve etkin listeye ekler. 
- Hizmet karşıya yükleme dosyasında çoğaltmaları yoksayar.

Düzenlemek veya dilimleri silmek için denetimleri sütun altındaki seçenekleri kullanın. 

Benzer şekilde, (karşıya yüklenen dosyanın dilimleri belirtmek için kullanamazsınız dışında) bloke listeye dilimler ekleyebilirsiniz.

## <a name="pinned-list"></a>Sabit listesi
Portal Ayrıca, kullanıcı belirli bir arama terimi girerse belirli bir Web sayfası arama sonucu üstüne Sabitle olanak tanır. **Pinned** sekmesi belirli bir sorgu için üst sonucunda görüntülenen Web belirtin sorgu Terime ve Web sayfası çiftlerinin listesini içerir. Kullanıcının sorgu Terime sabitlenmiş sorgu Terime tam olarak eşleşmelidir.
Sonuçları sabitleme hakkında daha fazla bilgi için bkz: [ayarlamak derece](#adjustrank).

Sonuçları sabitleme görüntü arama deneyimi için kullanılabilir değil.

## <a name="site-suggestions"></a>Site önerileri
Etkin listeye dilimler ekledikten sonra hizmeti aramanızı eklemek isteyebilirsiniz site ve alt önerileri oluşturur. **Eklemek isteyebilirsiniz** bölüm öneriler içerir. Sadece önerileri mevcutsa örneği Ayarları sayfasında bu bölüm içerir. 

Öneriler etkin listenize eklemek için tıklatın + simgesi.  Hizmet kendi ayarlarınızı temel alan öneriler oluşturduğundan tıklattığınızdan emin olun **yenileme** öneriler ekledikten sonra. 

## <a name="preview-pane"></a>Önizleme bölmesini
Arama sorguları göndermek ve sonuçları görüntülemek için sağ taraftaki Önizleme bölmesini kullanarak, arama örneği test edebilirsiniz. Seçin **My örneği**, bir güvenli arama filtresi ve ne aramak için pazara seçin (bkz [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters). Bir sorgu girin ve tuşuna girin veya geçerli yapılandırmasından sonuçlarını görüntülemek için arama simgesine tıklayın. Tıklatın web sonuçları görmek için **Web**tıklatın görüntü sonuçları görmek için **görüntü**. 

 Önizleme bölmesini kullanarak, ayrıca Bing sonuçları seçerek gözden geçirebilirsiniz **Bing** yerine **My örneği**. Bing tarafından döndürülen sonuçlar için arama deneyiminizi sonuçları karşılaştırmak yararlı olabilir.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>RANK Ayarla
Portal, Bing döndürür sonuçları işlemek için sıralama ayarlamanıza olanak sağlar. Önizleme bölmesinde bir arama terimi girin ve sorguyu çalıştırın. Önizleme bölmesini sorgu için arama sonucu listeleyin. Her bir sonucu sağındaki ayarlamalar listesi, hale getirebilir. 

- Blok. Etki alanı, alt veya Web sayfası bloke listeye taşır. Engellemek için düzeyini seçin. Bing arama sonuçlarında seçili sitesinden içerik dışlar. 
- Artırın. Etki alanı veya arama sonuçlarında daha yüksek alt içerikten artırır. Etki alanından içerik artırabilir ya da Web sayfasının ait olduğu alt sayfayı taşıma seçin.
- Düşürür. Etki alanı ya da alt arama sonuçlarında alt içerikten indirgenir. İçerik etki alanından indirgemek veya Web sayfasının ait olduğu alt sayfayı taşıma seçin. 
- PIN-to-top. Kullanıcının sorgu tam olarak sorgu Terime terimini kullandıysanız sonuçları üstünde görünür Web sayfasını tanımlayın. Etkin liste sabitlemek, Web sayfasının içerecek şekilde sahip değil. 

RANK ayarlama görüntü arama deneyimi için kullanılabilir değil.

## <a name="boosting-and-demoting"></a>Artırma ve indirgeme
Süper artırabilir, artırabilir, veya herhangi bir etki alanı indirgemek veya için etkin listenizde alt sayfayı taşıma. Varsayılan olarak, aynı ağırlıkta tüm dilimleri eklenir. Süper boosted veya Boosted öğeleri yüksek (ile Süper artırma derecelendirme artırma yüksek) arama sonuçlarında sıralanır. İndirilir öğeleri arama sonuçlarında alt sıralanır.

Süper artırabilir, artırmak ve etki alanları veya alt verin ağırlık çeşitleri indirgemek dikkate almak önemlidir. Bu sonuçları sırasını belirlemek için ranker tarafından kullanılan birçok sinyalleri yalnızca biridir. Başka bir deyişle, diğer birçok faktöre genel web sonuçlarını sıralamasını etkileyebilir gibi belirli bir sorgu için etkilerini garanti edilmez.  Olası etkisini belirlemek için bu artırma veya indirgeme ranker, sınama Önizleme bölmesini kullanarak arama deneyimi var.

Süper artırabilir, artırabilir, veya öğeleri etkin listesinde denetimlerini sıralaması Ayarla kullanarak veya artırma kullanarak indirin ve önizleme bölmesinde denetimleri indirgemek. Hizmet etkin listenize dilim ekler ve derecelendirme ayarlar buna göre.

Süper artırabilir, artırmak ve indirgemek değişiklikleri otomatik olarak kaydedilir ve anında karşı özel arama uç noktanızı yansıtır. 

Süper artırabilir, artırmak ve indirgemek görüntü arama deneyimi için kullanılabilir değil.

## <a name="pin-to-top"></a>Üstüne Sabitle
Bir Web sayfası özel bir sorgu için arama sonuçlarını üstüne sabitlemek için aşağıdaki seçeneklerden birini seçin:

1.  Pinned sekmesinde, sonuçları ve sabitleme tetikleyecek tam sorgu üst kısmına sabitlemek için Web sayfası URL'sini girin. 
2.  Önizleme bölmesinde bir sorgu terimine girin ve Ara'yı tıklatın. Ardından, kullanıcı aynı sorgu girerse en çok sonuç sabitlemek istediğiniz sonuçları Web sayfasında belirleyin. Sonra PIN dön'ı tıklatın. Hizmet Web sayfası ve sorgu Pinned listesine ekler. 

PIN Pinned sekmesinden izleyebilirsiniz. PIN gösterildiği gibi\<sorgu\>, \<Web sayfası\>' çiftleri. 

Web sayfası, yalnızca kullanıcının sorgu sorgunuzu tam olarak eşleşmesi durumunda sabitlendi. 

Belirli bir sorgu için en fazla bir Web sayfası sonuçları üstüne sabitleyebilirsiniz.

PIN görüntü arama deneyimi için kullanılabilir değil.

## <a name="use-bing-to-specify-slices"></a>Bing dilimler belirtmek için kullanın
Birkaç özel aramanızı yapmak dilimler Web belirtmek için çeşitli yollar vardır. Örneğinizi dahil etmek istediğiniz dilimler biliyorsanız, bunları, örneğinin etkin listeye ekleyin. Ekleme hakkında bilgi etkin listeye kendiniz öğeleri için bkz: [etkin ve bloke liste](#active-and-blocked-lists).
Ancak dahil etmek için hangi dilimlerin emin değilseniz, Bing önizleme bölmesinde sorguları çalıştırmak ve hangi Bing döndürür bakın. Daha sonra özel aramanızda dahil etmek istediğiniz dilimler seçebilirsiniz. Olasılıkla Örneğiniz için istediğiniz tüm dilimleri tanımlamak emin olmak için birden çok sorgu terimlerinin çalıştırmanız gerekir. 

Özel arama örneğinizi dilimler eklemek için Bing kullanmak için aşağıdaki adımları izleyin. 
1.  Bing özel arama oturum [portal](https://customsearch.ai).
2.  Örnek oluşturmak veya güncelleştirmek için bir örnek seçin.
3.  Sağ taraftaki önizleme bölmesinde Bing açılır listeden seçin.
4.  Arama kutusuna, Örneğiniz için uygun olan bir sorgu Terime girin.
5.  Tıklatın **site Ekle** dahil etmek istediğiniz sonucun yanındaki.
6.  **Tamam** düğmesine tıklayın.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>İstatistiklerini görüntüleme
Uygun düzeyde özel aranacak abone (bkz [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), bir **istatistikleri** sekmesi, üretim örneklerinizi eklenir. İstatistikler sekmesindeki nasıl özel arama noktalarınızı, arama birimi, en sık kullanılan sorguların, coğrafi dağıtım, yanıt kodları ve güvenli arama dahil olmak üzere kullanılan hakkında ayrıntıları gösterir. Sağlanan denetimleri kullanarak ayrıntılarını filtreleyebilirsiniz.

## <a name="understanding-quota"></a>Anlama kota
- Her özel arama örneği sayısı için yapabilir derecelendirme ayarlamalar **etkin** ve **bloke** dilimler 400 sınırlıdır.
- Bir dilim için etkin ya da engellenen sekmeler ekleme bir derecelendirme ayarlama olarak sayılır.
- Artırma ve iki derecelendirme ayarlamalar olarak sayısı indirgeme.
- Her bir özel arama örneği için hale getirebilir PIN sayısını 200 ile sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel aramanızı çağırın](./search-your-custom-view.md)
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)