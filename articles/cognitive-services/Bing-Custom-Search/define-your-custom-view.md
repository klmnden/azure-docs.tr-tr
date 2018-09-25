---
title: 'Bing özel arama: özel bir görünüm tanımlamak | Microsoft Docs'
description: Bir site ve dikey search hizmetlerini nasıl oluşturulacağını açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 59fef9a370ca56080d06f0920ed5409c141683a4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980670"
---
# <a name="configure-your-custom-search-experience"></a>Özel arama deneyiminizi yapılandırın

Özel arama örneği kullanıcılarınızın çok önem verdiğiniz Web sitelerinden içerik eklemek için arama deneyimini uyumlu hale getirmenizi sağlar. Bing web genelinde arama yapmak yerine, yalnızca ilginizi çeken web dilimin arar. Özel web görünümünü oluşturmak için Bing özel arama kullanın [portalı](https://customsearch.ai). Portalda oturum açarken hakkında daha fazla bilgi için bkz: [ilk Bing özel arama örneğinizin oluşturma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). 

Portal, etki alanlarını, alt ve Bing arama yapmak istediğiniz Web sayfalarını ve arama yapmak istemiyorsanız bu belirten bir arama örneği oluşturmanızı sağlar. Bilmeniz içeriği URL'lerini belirtmeye ek olarak, görünümünüze eklemek isteyebileceğiniz içerik önermek için portalı da sorabilirsiniz. 

Bir dilim Web tanımlayabilirsiniz yollar şunlardır: 

1.  Etki alanı. Bir etki alanı dilim bir internet etki alanı içinde bulunan tüm içerik içerir. Örneğin, www.microsoft.com. "Www" atlama, ayrıca etki alanının alt etki alanlarını arama yapmak için Bing neden olur. Microsoft.com belirtirseniz, örneğin, Bing ayrıca sonuçları support.microsoft.com veya technet.microsoft.com döndürür.  
  
2.  Alt sayfa. Bir alt dilim aşağıdaki yolları ve alt bulunan tüm içeriği de bulunmaktadır. En fazla iki alt yolu belirtebilirsiniz. Örneğin, www.microsoft.com/en-us/windows/  
  
3.  Web sayfası. Bir Web sayfası dilim yalnızca Web sayfası özel bir arama içerebilir. İsteğe bağlı olarak alt eklenip eklenmeyeceğini belirtebilirsiniz.

Tüm etki alanlarını, alt ve belirttiğiniz Web sayfaları, genel ve Bing tarafından dizini oluşturulmuş olması gerekir. Aramaya dahil etmek istediğiniz genel bir site sahibi olduğunuz ve Bing, dizinlemedi Bing bkz [yayımlanması belgeleri](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) bu dizin için Bing alma hakkında ayrıntılar için. Ayrıca, güncel dizinidir gezinilen sitenizi güncelleştirmek için Bing alma hakkında ayrıntılar için yayımlanması belgelerine bakın.

## <a name="adding-slices-to-your-custom-search"></a>Özel arama ekleme dilimleri

Özel arama örneği kullanımınızın tanımladığınızda, etkin ve engellenen etki alanları, alt ve arayabilir veya arama istediğiniz Web sayfalarını belirtin.  

- Etkin: Etki alanları, alt veya aramaya eklenecek Web sayfalarının bir listesi.  
  
- Engellendi: Bir liste etki alanı, alt veya arama hariç tutmak için Web sayfaları. Etkin listenizde, blok etki alanları ve alt altında bulunan içerik olmalıdır öğeleri listelenir.

Her listesine erişmek için özel arama örneği kullanımınızın etkin ve engellenen sekmelere tıklayın. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Etkin ve bloke liste 

Bing arama yapmak istediğiniz web dilimin belirtmek için **etkin** sekmesini ve etki alanlarını, alt ve aramak için Web sayfalarının listesi. Bir dilim doğrudan listeye ekleyin veya karşıya yükleme simgesini kullanarak metin dosyasını karşıya yükleyerek birden fazla dilim ekleyin.

Dosya karşıya yükleme ayrıntıları: 

- Dosyayı karşıya yükleme yalnızca etkin listeye dilimleri eklemek için kullanılabilir, dilimleri bloke listeye eklemek için kullanamazsınız.  
  
- Bir metin dosyası oluşturun ve tek bir etki alanı, alt veya Web sayfasını her satırı belirtin. Bir hata oluşursa, tüm karşıya yükleme reddedildi.  
  
- Engellenen listesi, etki alanı, alt veya karşıya yükleme dosyasında belirtilen Web sayfasını içeriyorsa, hizmet bloke listeden kaldırır ve etkin listesine ekler.  
  
- Hizmet, karşıya dosya yükleme, yinelenenleri yok sayar.

Düzenleme veya silme dilimleri için seçenekleri altında kullanın **denetimleri** sütun. 

Benzer şekilde, (karşıya yükleme dosyasını dilimleri belirtmek için kullanamazsınız dışında) bloke listeye dilimleri ekleyebilirsiniz.

## <a name="pinned-list"></a>Sabit listesi

Portal Ayrıca, kullanıcı belirli bir arama terimi girerse belirli bir Web sayfası arama sonucu en üstüne Sabitle sağlar. **Pinned** sekmesi, belirli bir sorgu için en iyi sonucu olarak görünen Web sayfasını belirtin sorgu terim ve Web sayfası çiftlerinin listesini içerir. Sonuçları sabitleme hakkında daha fazla bilgi için bkz: [ayarlamak derece](#adjustrank).

Sonuçları sabitleme resim arama için kullanılabilir değil ve Video arama deneyimleri.

## <a name="website-suggestions"></a>Web sitesi önerileri

Dilimler etkin listesine eklendikten sonra hizmeti aramanızı eklemek isteyebilirsiniz Web sitesi ve alt öneriler oluşturur. **Eklemek isteyebilirsiniz** bölüm öneriler içerir. Sadece önerileri mevcutsa örneği ayarlar sayfasından Bu bölüm içerir. 

Öneriler etkin listenize eklemek için + simgesini.  Hizmet kendi ayarlarınızı temel alan öneriler oluşturduğundan tıkladığınızdan emin olun **Yenile** öneriler ekledikten sonra. 

## <a name="preview-pane"></a>Önizleme Bölmesi

Önizleme bölmesinde arama sorguları göndermek ve sonuçları görüntülemek için sağ taraftaki kullanarak arama örneğiniz test edebilirsiniz. 

1. Seçin **Örneğim**
2. Güvenli arama filtresi ve hangi aramak için Pazar seçin (bkz [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).
3. Bir sorgu girin ve enter tuşuna basın veya geçerli yapılandırmasından sonuçlarını görüntülemek için arama simgesine tıklayın. 

Arama gerçekleştirmek için türünü seçmek için tıklayın **Web** web sonuçları elde etmek için **görüntü** görüntü sonuçları elde etmek için veya **Video** video sonuçları elde etmek için. 

Önizleme bölmesini kullanarak, ayrıca Bing sonuçları seçerek inceleyebilirsiniz **Bing** yerine **My örneği**. Bu, arama deneyimini sonuçlarına Bing tarafından döndürülen sonuçları karşılaştırmak yararlı olabilir.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Boyut sayısı Ayarla

Portal derecelendirmesi Bing döndürdüğü sonuçları işlemek için ayarlamanıza olanak sağlar. Önizleme bölmesi, bir arama terimi girin ve sorguyu çalıştırın. Önizleme bölmesi, sorgu sonuçları listesi. Her sonuç sağında ayarlamalar listesi, hale getirebilirsiniz. 

- Blok. Etki alanı, alt veya Web sayfası bloke listeye taşır. Engellemek için düzeyi seçin. Bing içerik seçili sitenin arama sonuçlarındaki dışlar.  
  
- Artırın. Etki alanı veya alt arama sonuçlarında daha yüksek içerik artırıyor. Etki alanından içerik artırabilir ya da Web sayfasının ait olduğu alt sayfayı taşıma seçin. [Daha fazla bilgi edinin](#boosting-and-demoting)  
  
- Düşürür. Etki alanı veya alt arama sonuçlarında alt içeriği indirger. Etki alanından içerik indirgemek ya da Web sayfasının ait olduğu alt sayfayı taşıma seçin. [Daha fazla bilgi edinin](#boosting-and-demoting).  
  
- PIN yukarıya. Kullanıcının sorgu dizesi eşleşiyorsa PIN'ler sorgu dizesi PIN'in eşleşme koşulu temel alarak sonuçların üst kısmında görünür Web sayfasını tanımlayın. Sabitlemek, Web sayfasının içermesini etkin liste yok. [Daha fazla bilgi edinin](#pin-to-top).

Boyut ayarlama resim arama için kullanılabilir değil ve Video arama deneyimleri.

## <a name="boosting-and-demoting"></a>Yükseltme ve indirgeme

Süper artırın, artırma, veya herhangi bir etki alanı düzeyini düşür veya yapabilirsiniz etkin listenizde alt sayfayı taşıma. Varsayılan olarak, aynı ağırlıkta tüm dilimleri eklenir. Süper boosted veya Boosted öğeleri (Süper boost derecelendirme boost yüksek ile) arama sonuçlarında daha yüksek sıralanır. İndirilir öğeleri arama sonuçlarında alt sıralanır.

Süper artırın, artırın ve etki alanları veya alt verin ilgili ağırlık çeşitleri indirgemek unutulmaması önemlidir. Bu sonuçları sırasını belirlemek için derecelendiricisini uygulama tarafından kullanılan birçok sinyalleri yalnızca biridir. Başka bir deyişle, diğer faktörlerden web sonuçlarının toplam derecelendirme etkileyebilir gibi belirli bir sorgu için etkilerini garanti edilmez.  Olası etkisini belirlemek için Önizleme bölmesinde kullanarak arama deneyimi derecelendiricisini, test, yükseltme veya indirgeme vardır.

Süper artırın, artırın, veya etkin listesini sıralama ayarlamak denetimleri kullanma veya Boost kullanarak öğeleri indirgemek ve önizleme bölmesinde denetim düzeyini düşür. Hizmet etkin listenize dilim ekler ve derecelendirme ayarlar uygun şekilde.

Süper artırın, artırın ve indirgeme için resim araması ve Video arama deneyimleri kullanılabilir değil.

## <a name="pin-to-top"></a>En üste Sabitle

Belirli bir sorgu için arama sonuçlarını üstüne bir Web sayfasına sabitlemek için aşağıdaki seçeneklerden birini seçin:

1.  İçinde **Pinned** sekmesinde, sonuçları ve sabitleme tetikler sorgu üst kısmına sabitlemek için Web sayfasının URL'sini girin.  
  
2.  İçinde **Önizleme** bölmesinde bir sorgu terimine girin ve Ara'yı tıklatın. Ardından, kullanıcı aynı sorgu girerse sonuçları en üst kısmına sabitlemek istediğiniz sonuçları Web sayfasını tanımlayın. ' A tıklayarak **en üste Sabitle**. Sorgu ve Web hizmeti ekler **Pinned** listesi. 

İçinde PIN izleyebilirsiniz **Pinned** sekmesi. PIN gösterildiği gibi\<sorgu\>, \<Web sayfası\>' çiftleri. 

Belirli bir sorgu için en fazla bir Web sayfası sonuçları en üstüne sabitleyebilirsiniz.

PIN resim arama için kullanılabilir değil ve Video arama deneyimleri.

### <a name="specifying-the-pins-match-condition"></a>PIN'in eşleşme koşulu belirtme

Yalnızca kullanıcı sorgu dizesini PIN'in eşleşme koşulu temel alarak PIN'in sorgu dizesi eşleşiyorsa Web sayfası sonuçları üstüne sabitlenir. PIN'in sorgu dizesi ve kullanıcının sorgu dizesi yalnızca tam olarak eşleşirse, varsayılan olarak, bir PIN eşleştirilir. Aşağıdaki eşleşme koşullardan birini belirterek varsayılan davranışı değiştirebilirsiniz.

- İle başlayan &mdash; kullanıcı sorgu dizesini PIN'in sorgu dizesi ile başlıyorsa PIN bir eşleşmedir.
- İle biten &mdash; kullanıcı sorgu dizesini PIN'in sorgu dizesi ile bitiyorsa PIN bir eşleşmedir.
- İçeren &mdash; kullanıcı sorgu dizesini PIN'in sorgu dizesi varsa bir eşleşme pın'dir.

PIN'in eşleşme koşulu değiştirmek için PIN'in Düzenle simgesine (görünümler bir kalemin gibi) tıklayın. İçinde **sorgu eşleşme koşulu** sütununda açılan listeye tıklayın ve kullanmak için yeni koşulu seçin. Kaydet'i tıklatın değişikliği kaydetmek için simge.

Tüm karşılaştırmalar büyük/küçük harfe duyarsızdır.

### <a name="changing-the-order-of-the-pins"></a>PIN sırasını değiştirme

PIN sırasını değiştirmek için sürükle-bırak PIN olabilir veya PIN düzenleyin ve kendi sıra numarasını değiştirmek. Sürükle ve bırak bir PIN için sabitle'ye listesinde taşımak, fare düğmesini basılı tutun ve PIN kodunu değiştirmek istediğiniz PIN sürükleyin ve sonra fare tuşunu bırakın. PIN'in sipariş numarası buna uygun olarak değiştiğine dikkat edin.

PIN'in sipariş numarası da düzenleyebilirsiniz. İçinde **denetimleri** sütun PIN'in Düzenle simgesine (görünümler bir kalemin gibi) tıklayın. Sipariş numarası, PIN listesinde görünmesini istediğiniz girin ve enter tuşuna basın veya tıklayın simgesi. Örneğin, sipariş numarası için PIN listesinde şu anda altıncı ve ikinci olmasını istiyorsanız değiştirmek iki (2).

Birden çok PIN eşleşme koşulu karşılaması durumunda Bing kullanan hangi PIN PIN'ler sırasını belirler. Örneğin, PIN'leri iki ve üç eşleşme koşulu karşılayan, Bing, iki PIN kullanır.

## <a name="use-bing-to-specify-slices"></a>Dilimler belirtmek için Bing kullanın

Birkaç olun, özel arama'yı web Kesitler belirtmek için çeşitli yollar vardır. Örneğinizde dahil etmek istediğiniz dilimleri biliyorsanız bunları Örneğinize ait ekleme **etkin** listesi. Öğeler ekleme hakkında bilgi için **etkin** kendiniz listelemek için bkz: [etkin ve bloke liste](#active-and-blocked-lists).

Ancak dahil etmek için hangi dilimlerin emin değilseniz, Bing sorguları çalıştırabilirsiniz **Önizleme** bölmesi ve hangi Bing döndürür. Ardından, özel arama dahil etmek istediğiniz dilimleri seçebilirsiniz. Olasılıkla Örneğiniz için istediğiniz tüm dilimleri tanımlamak emin olmak için birden çok sorgu Kullanım Koşulları'nı çalıştırmanız gerekir. 

Bing özel arama örneğinizin dilimleri eklemek için kullanmak için aşağıdaki adımları izleyin. 

1.  Bing özel arama için oturum açın [portalı](https://customsearch.ai).
2.  Örneği oluşturmak veya güncelleştirmek için örneği seçin.
3.  Sağ taraftaki önizleme bölmesinde, Bing, açılır listeden seçin.
4.  Arama kutusuna, Örneğiniz için ilgili bir sorgu terimi girin.
5.  Tıklayın **Ekle site** dahil etmek istediğiniz sonucun yanında.
6.  **Tamam** düğmesine tıklayın.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>İstatistikleri görüntüleme

Özel arama uygun düzeyde abone (bkz [fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), **istatistikleri** sekmesi, üretim örneklerine eklenir. İstatistikler sekmesindeki nasıl, özel arama uç noktaları, çağrı hacmi, en sık kullanılan sorgular, coğrafi dağıtım, yanıt kodları ve güvenli arama dahil olmak üzere kullanıldığı hakkında ayrıntılar gösterilir. Sağlanan denetimleri kullanarak ayrıntıları filtreleyebilirsiniz.

## <a name="understanding-quota"></a>Kota anlama

- Her özel arama örneği için olun derecelendirme ayarlamalar sayısı **etkin** ve **bloke** dilimleri 400 sınırlıdır.
- Etkin veya engellenmiş sekmeleri bir dilim bir derecelendirme ayarlama olarak sayılır.
- Artırma ve sayısı iki derecelendirme ayarlamalar olarak indirgeme.
- Her bir özel arama örneği için 200'e hale getirebilir PIN'ler sayısı sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel arama çağırın](./search-your-custom-view.md)
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)