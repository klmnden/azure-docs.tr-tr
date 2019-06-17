---
title: Bing özel arama deneyiminizi yapılandırma | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Site ve dikey search hizmetlerini nasıl oluşturulacağını açıklar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: e4799ca099d608c3b8ecd16612b790f5654df7dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390411"
---
# <a name="configure-your-bing-custom-search-experience"></a>Bing özel arama deneyiminizi yapılandırın

Özel arama örneği kullanıcılarınızın çok önem verdiğiniz Web sitelerinden içerik eklemek için arama deneyimini uyumlu hale getirmenizi sağlar. Bing web genelinde arama yapmak yerine, yalnızca ilginizi çeken dilimleri Web arar. Size özel web görünümünü oluşturmak için Bing Özel Arama [portalını](https://customsearch.ai) kullanın.

Portal web dilimlerini belirten bir arama örneği oluşturmanıza olanak sağlar: etki alanlarını, alt ve Bing arama yapmak istediğiniz Web sayfalarını ve arayacak şekilde istemediğiniz olanlar. Portal, dahil etmek istediğiniz içeriği de önerebilir.

Web uygulamanızın dilimleri tanımlarken aşağıdakileri kullanın:

| Dilim adı | Açıklama                                                                                                                                                                                                                                                                                                |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Etki Alanı     | Bir etki alanı dilim bir internet etki alanı içinde bulunan tüm içerik içerir. Örneğin, `www.microsoft.com`. Atlama `www.` da etki alanının alt etki alanlarını arama yapmak için Bing neden olur. Örneğin, belirttiğiniz `microsoft.com`, Bing, ayrıca sonuçları döndürür `support.microsoft.com` veya `technet.microsoft.com`. |
| Alt sayfa    | Bir alt dilim aşağıdaki yolları ve alt bulunan tüm içeriği de bulunmaktadır. En fazla iki alt yolu belirtebilirsiniz. Örneğin, `www.microsoft.com/en-us/windows/`                                                                                                                       |
| Web sayfası    | Bir Web sayfası dilim yalnızca Web sayfası özel bir arama içerebilir. İsteğe bağlı olarak alt eklenip eklenmeyeceğini belirtebilirsiniz.                                                                                                                                                                                  |

> [!IMPORTANT]
> Tüm etki alanlarını, alt ve belirttiğiniz Web sayfaları, genel ve Bing tarafından dizini oluşturulmuş olması gerekir. Aramaya dahil etmek istediğiniz genel bir site sahibi olduğunuz ve Bing, dizinlemedi Bing bkz [yayımlanması belgeleri](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) bu dizin için Bing alma hakkında ayrıntılar için. Ayrıca, güncel dizinidir gezinilen sitenizi güncelleştirmek için Bing alma hakkında ayrıntılar için yayımlanması belgelerine bakın.

## <a name="add-slices-of-the-web-to-your-custom-search-instance"></a>Özel arama örneğinizin web dilimleri Ekle

Özel arama örneği kullanımınızın oluşturduğunuzda, web Kesitler belirtebilirsiniz: etki alanlarını, alt ve arama sonuçlarınızdan dahil engellendi veya sahip olmasını istediğiniz Web sayfaları. 

Özel arama örneğinizin dahil etmek istediğiniz dilimleri biliyorsanız bunları Örneğinize ait ekleme **etkin** listesi. 

Dahil etmek için hangi dilimlerin emin değilseniz Bing arama sorguları gönderebilirsiniz **Önizleme** bölmesi ve istediğiniz dilimleri seçin. Bunu yapmak için: 

1. "Bing" Önizleme bölmesinde aşağı açılan listeden seçin ve bir arama sorgusu girin

2. Tıklayın **Ekle site** dahil etmek istediğiniz sonucun yanında. Ardından Tamam'a tıklayın.

>[!NOTE]
> [!INCLUDE[publish or revert](./includes/publish-revert.md)]

<a name="active-and-blocked-lists"></a>

### <a name="customize-your-search-experience-with-active-and-blocked-lists"></a>Etkin ve bloke liste ile arama deneyiminizi özelleştirin 

Etkin ve engellenen dilimlerin listesini tıklayarak erişebileceğiniz **etkin** ve **bloke** özel arama örneği kullanımınızın sekmeleri. Özel arama dilimleri etkin listesine dahil edilir. Engellenen dilimler aranmayacaktır ve, arama sonuçlarında görünmez.

Bing arama yapmak istediğiniz web dilimlerini belirtmek için **etkin** sekmesini ve bir veya daha fazla URL ekleyin. Düzenleme veya silme URL'ler için seçenekleri altında kullanın **denetimleri** sütun. 

URL'lere eklenirken **etkin** ekleyebileceğiniz tek URL'leri ya da birden fazla URL tek seferde karşıya yükleme simgesini kullanarak metin dosyasını karşıya yükleyerek listesi.

![Bing özel arama etkin sekme](media/file-upload-icon.png)

Bir dosyayı karşıya yüklemek için bir metin dosyası oluşturun ve tek bir etki alanı, alt veya Web sayfasını her satırı belirtin. Dosyanız varsa doğru biçimlendirilmemiş reddedilir.

> [!NOTE]
> * Yalnızca bir dosyayı karşıya yükleyebilirsiniz **etkin** listesi. Dilimlere eklemek için kullanamazsınız **bloke** listesi.  
> * Varsa **engellendi** listesini içeren bir etki alanı, alt veya karşıya yükleme dosyasında belirtilen Web sayfasını CİHAZDAN kaldırılır **engellenen** listelemek ve eklenen **etkin** listesi .
> * Karşıya yükleme dosyanızdaki yinelenen girişler Bing özel arama tarafından göz ardı edilir. 

### <a name="get-website-suggestions-for-your-search-experience"></a>Arama deneyiminiz için Web sitesi öneriler alın

Web dilimlere ekledikten sonra **etkin** listesinde, Bing özel arama portal Web sitesi ve alt önerileri sekmesinin altındaki üretir. Bing özel arama eklemek isteyebilirsiniz gördüğü dilimleri şunlardır. Tıklayın **Yenile** özel arama örneğinizin ayarlarını güncelleştirdikten sonra güncelleştirilmiş önerileri almak için. Bu bölüm, yalnızca önerileri varsa görülebilir.

## <a name="search-for-images-and-videos"></a>Görüntü ve video arayın

Görüntüleri ve videoları benzer şekilde web içeriği kullanarak arayabilirsiniz [Bing özel resim arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference) veya [Bing özel Video arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference). Bu sonuçları görüntüleyebilirsiniz [UI barındırılan](hosted-ui.md), veya API'leri. 

Bu API'ler, olmayan-özel benzer [Bing resim arama](../Bing-Image-Search/overview.md) ve [Bing Video arama](../Bing-Video-Search/search-the-web.md) API'leri, ancak tüm Web'de arama ve gerekli olmayan `customConfig` sorgu parametresi. Görüntü ve video ile çalışma hakkında daha fazla bilgi için şu belge kümeleri bakın. 

## <a name="test-your-search-instance-with-the-preview-pane"></a>Önizleme bölmesinde arama örneğinizle test

Arama sorguları göndermek ve sonuçları görüntülemek için portalın sağ tarafta önizleme bölmesinde kullanarak arama örneğinizin test edebilirsiniz. 

1. Arama kutusunun altında seçin **My örneği**. Seçerek, Bing arama deneyiminizi sonuçları karşılaştırabilirsiniz **Bing**. 
2. Güvenli arama filtresi ve aramak için pazara sunma seçin (bkz [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters)).
3. Bir sorgu girin ve enter tuşuna basın veya geçerli yapılandırmasından sonuçlarını görüntülemek için arama simgesine tıklayın. Gerçekleştirmek tıklayarak arama türünüzü değiştirebilirsiniz **Web**, **görüntü**, veya **Video** karşılık gelen sonuçları elde etmek için. 

<a name="adjustrank"></a>

## <a name="adjust-the-rank-of-specific-search-results"></a>Belirli bir arama sonuçları derecesini ayarla

Portal Web sayfalarını özel etki alanları ve alt içeriği arama sıralamasını ayarlamanıza olanak sağlar. Önizleme bölmesinde arama sorgusu gönderdikten sonra her bir arama sonucunun bir listesi için yapabileceğiniz ayarlamalar içerir:  

|            |                                                                                                                                                                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Engelle      | Etki alanı, alt veya Web sayfası bloke listeye taşır. Bing içerik arama sonuçlarında görüntülenmesini Seçili site dışında bırakır.                    |
| Boost      | Etki alanı veya arama sonuçlarında daha yüksek olması için alt sayfa içeriği artırıyor.                                                                                        |
| İndirgeme     | Etki alanı veya alt arama sonuçlarında alt içeriği indirger. Etki alanından içerik indirgemek ya da Web sayfasının ait olduğu alt sayfayı taşıma seçin. |
| En üste Sabitle | Etki alanı, alt veya Web sayfasına taşır **Pinned** listesi. Bu, verilen arama sorgusu için ilk arama sonucu olarak görüntülenecek Web sayfasının zorlar.                   |

Boyut ayarlama, görüntü veya video aramaları için kullanılabilir değil.

### <a name="boosting-and-demoting-search-results"></a>Yükseltme ve indirgeme arama sonuçları

Süper artırın, artırma, veya herhangi bir etki alanı düzeyini düşür veya içinde alt sayfayı taşıma **etkin** listesi. Varsayılan olarak, tüm dilimler hiçbir sıralama düzeltme ile eklenir. Süper boosted web veya Boosted dilimler (Süper boost derecelendirme boost yüksek ile) arama sonuçlarında daha yüksek sıralanır. İndirilir öğeleri arama sonuçlarında alt sıralanır.

Süper artırın, artırın veya yapabilirsiniz kullanarak öğeleri indirgemek **sıralaması ayarlamak** denetimlerini **etkin** listesini veya göre Boost kullanarak ve önizleme bölmesinde denetim düzeyini düşür. Hizmet etkin listenize dilim ekler ve derecelendirme ayarlar uygun şekilde.

> [!NOTE] 
> Artırma ve etki alanları ve alt indirgeme Bing özel arama, arama sonuçlarını sıralamasını belirlemek için kullandığı birçok yöntem biridir. Farklı bir web içeriği derecelendirmesini etkileyen diğer faktörlere nedeniyle boyut ayarlama etkilerini farklılık gösterebilir. Önizleme bölmesinde arama sonuçlarınızı derecesini ayarlama etkilerini test etmek için kullanın. 

Süper artırın, artırın ve indirgeme görüntü ve video aramaları için kullanılamaz.

## <a name="pin-slices-to-the-top-of-search-results"></a>Arama sonuçları en üstüne Sabitle dilimleri

Portal ayrıca sağlar kullanarak belirli bir arama terimleri, URL'ler arama sonuçları en üstüne Sabitle **Pinned** sekmesi. En iyi sonucu olarak görünen Web sayfasını belirtmek için bir URL ve sorgu girin. Bir Web sayfası arama sorgu başına en fazla sabitleyebilirsiniz ve aramalarda yalnızca dizini oluşturulmuş Web sayfalarında görüntülenir unutmayın. Sonuçları sabitleme, görüntü veya video aramaları için kullanılabilir değil.

İki yolla bir Web sayfası en üstüne sabitleyebilirsiniz:

* İçinde **Pinned** sekmesinde, üst ve alt karşılık gelen sorgu sabitlemek için Web sayfasının URL'sini girin.

* İçinde **Önizleme** bölmesinde arama sorgusu girin ve Ara'yı tıklatın. Sorgunuz için sabitleme ve istediğiniz Web bulmak **en üste Sabitle**. Sorgu ve Web sayfası eklenir **Pinned** listesi.

### <a name="specify-the-pins-match-condition"></a>PIN'in eşleşme koşulu belirtin

Bir kullanıcının sorgu dizesi tam olarak listelenen eşleştiğinde varsayılan olarak, Web sayfaları yalnızca arama sonuçları üstüne Sabitlenen **Pinned** listesi. Aşağıdaki eşleşme koşullardan birini belirterek bu davranışı değiştirebilirsiniz:

> [!NOTE]
> Kullanıcı Arama sorgusu PIN'in arama sorgusu arasındaki tüm karşılaştırmalar büyük/küçük harfe duyarsızdır.

| Değer | Açıklama                                                                          |
|---------------|----------------------------------------------------------------------------------|
| İle başlar | Kullanıcının sorgu dizesi PIN'in sorgu dizesi ile başlıyorsa PIN bir eşleşme olacak. |
| Şununla biter   | Kullanıcının sorgu dizesi PIN'in sorgu dizesi ile bitiyorsa PIN bir eşleşmedir.  |
| İçerir    | Kullanıcının sorgu dizesi PIN'in sorgu dizesini içeriyorsa PIN bir eşleşmedir.   |


PIN'in eşleşme koşulu değiştirmek için PIN'in düzenleme simgesine tıklayın. İçinde **sorgu eşleşme koşulu** sütununda açılan listeye tıklayın ve kullanmak için yeni koşulu seçin. Kaydet'i tıklatın değişikliği kaydetmek için simge.

### <a name="change-the-order-of-your-pinned-sites"></a>Sabitlenmiş sitelerinizi sırasını değiştirme

PIN sırasını değiştirmek için sürükle ve bırak bunları olabilir veya sıra numaralarına "Düzenle" simgesine tıklayarak düzenleme **denetimleri** sütununun **Pinned** listesi.

Birden çok PIN bir eşleşme koşulu karşılayan, Bing özel arama listedeki en yüksek olanı kullanır.

## <a name="view-statistics"></a>İstatistikleri görüntüleme

Özel arama uygun düzeyde abone (bkz [fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), **istatistikleri** sekmesi, üretim örneklerine eklenir. İstatistikler sekmesindeki nasıl, özel arama uç noktaları, çağrı hacmi, en sık kullanılan sorgular, coğrafi dağıtım, yanıt kodları ve güvenli arama dahil olmak üzere kullanıldığı hakkında ayrıntılar gösterilir. Sağlanan denetimleri kullanarak ayrıntıları filtreleyebilirsiniz.

## <a name="usage-guidelines"></a>Kullanım yönergeleri

- Her özel arama örneği için olun derecelendirme ayarlamalar sayısı **etkin** ve **bloke** dilimleri 400 sınırlıdır.
- Etkin veya engellenmiş sekmeleri bir dilim bir derecelendirme ayarlama olarak sayılır.
- Artırma ve sayısı iki derecelendirme ayarlamalar olarak indirgeme.
- Her bir özel arama örneği için 200'e hale getirebilir PIN'ler sayısı sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel arama örneğinizi çağırma](./search-your-custom-view.md)
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
