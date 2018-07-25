---
title: Custom Vision Service'e - Azure Bilişsel hizmetler ile sınıflandırıcı oluşturma | Microsoft Docs
description: Custom Vision Service'e nesneleri olarak keşfedilir bir sınıflandırıcı oluşturmak için kullanmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: c5183078d2f9d5eb16abef4f5df240f77eea6b8b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223378"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Özel görüntü ile bir sınıflandırıcı oluşturma

Özel görüntü işleme hizmeti kullanmak için öncelikle bir sınıflandırıcı oluşturmanız gerekir. Bu belgede, tarayıcınız üzerinden sınıflandırıcı oluşturma konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Sınıflandırıcı oluşturma için öncelikle olması gerekir:

- Geçerli bir [Microsoft hesabı](https://account.microsoft.com/account) veya Azure Active Directory Orgıd ("iş veya Okul hesabı"), customvision.ai oturum açabilir ve kullanmaya başlayın.

    > [!IMPORTANT] 
    > Orgıd oturum açma için Azure Active Directory (Azure AD) kullanıcıları [Ulusal Bulutlar](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) şu anda desteklenmiyor.

- Resimler, sınıflandırıcınızı (ile 30 görüntüleri etiket başına en az) eğitmek için bir dizi.

- Sınıflandırıcınızı sınıflandırıcı eğitildi sonra test etmek için birkaç görüntüler.

- İsteğe bağlı: Microsoft Account veya Orgıd ile ilişkili bir Azure aboneliği. Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

    > [!IMPORTANT]
    > Bir Azure aboneliği, yalnızca oluşturmak mümkün olmayacak __sınırlı deneme__ projeleri. Bir Azure aboneliğiniz varsa, özel görüntü işleme hizmeti eğitim ve tahmin kaynakları oluşturmak için istenir [Azure portalında](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) proje oluşturma sırasında.   

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Yeni bir proje oluşturmak için aşağıdaki adımları kullanın:

1. Web tarayıcınızda gidin [Custom Vision web sayfası](https://customvision.ai). Seçin __oturum__ Service'i kullanmaya başlamak için.

    ![Oturum açma sayfasının görüntüsü](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Özel görüntü işleme hizmeti için oturum açtıktan sonra projelerin bir listesini sunulur. Test etmek için iki "sınırlı deneme" projeler dışında bir Azure kaynakla ilişkilendirilen projelerdir. Bir Azure kullanıcısıysanız, ilişkili tüm projeleri göreceğiniz [Azure kaynaklarını](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) erişim sahibi. 

2. İlk projenizi oluşturmak için Seç **yeni proje**. İlk projeniz için hizmet koşullarını kabul etmeniz istenir. Onay kutusunu işaretleyin ve ardından **kabul ediyorum** düğmesi. **Yeni proje** iletişim kutusu görüntülenir.

    ![Yeni Proje iletişim kutusu, adını, açıklamasını ve etki alanları için alanlar içerir.](./media/getting-started-build-a-classifier/new-project.png)

3. Bir ad ve proje için bir açıklama girin. Kullanılabilir etki alanlarını birini seçin. Aşağıdaki tabloda açıklandığı gibi her etki alanı görüntüleri, belirli türde bir sınıflandırıcı iyileştirir:

    |Etki alanı|Amaç|
    |---|---|
    |__Genel__| Çok sayıda görüntü sınıflandırma görevleri için en iyi duruma getirilmiş. Diğer etki alanlarıyla uygun yok ya da seçmek için hangi etki alanı emin değilseniz, genel etki alanını seçin. |
    |__Gıda__|Bir restoran menüsünde göreceğiniz şekilde çanakları fotoğraflarını için en iyi duruma getirilmiş. Bireysel MEYVELERİ veya et fotoğraflarını sınıflandırma istiyorsanız, Yemek etki alanını kullanın.|
    |__Yer işareti__|Tanınabilir için yer işareti, doğal ve yapay en iyi duruma getirilmiş. Yer işareti fotoğraf açıkça görünür olduğunda bu etki alanı en iyi şekilde çalışır. Bu etki alanında yer işareti biraz önündeki kişiler tarafından engellendiği bile çalışır.|
    |__Perakende__|Bir alışveriş katalog veya alışveriş Web sitesinde bulunan görüntüleri için en iyi duruma getirilmiş. Elbiselerini pants ve gömlekler arasında yüksek duyarlık sınıflandırmak istiyorsanız, bu etki alanını kullanın.|
    |__Yetişkin__|Yetişkinlere yönelik içerik ve yetişkin olmayan içerik daha iyi tanımlamak için en iyi duruma getirilmiş. Örneğin, görüntüleri bathing cins insanların engellemek istiyorsanız, bu etki alanı, bunu yapmak için özel bir sınıflandırıcı oluşturmanıza olanak sağlar.|
    |__Compact etki alanları__| Mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için en iyi duruma getirilmiş. Compact etki alanları tarafından oluşturulan modelleri, yerel olarak çalıştırmak için verilebilir.|

    İsterseniz, etki alanı daha sonra değiştirebilirsiniz.

4. Bir kaynak grubu seçin. Kaynak grubu açılan tüm özel görüntü işleme hizmeti kaynakları içeren Azure kaynak gruplarınızdaki gösterir. Select oluşturabilirsiniz __sınırlı deneme__. Sınırlı deneme giriş, Azure dışı kullanıcı arasından seçim yapmanız mümkün olacaktır yalnızca kaynak grubudur.

    Projeyi oluşturmak için Seç __proje oluştur__.

## <a name="upload-and-tag-images"></a>Karşıya yükleme ve etiket görüntüleri

1. Görüntüleri bir sınıflandırıcı eklemek için __görüntüleri ekleme__ düğmesine ve ardından __yerel dosyalara Gözat__. Seçin __açık__ etiketleme için taşınır.

    > [!TIP]
    > Görüntüleri seçtikten sonra bunları etiketlemeniz gerekir. Etiket, görüntü kullanmayı planladığınız etiketlere göre görüntüleri karşıya yüklemek daha kolay olabilir, karşıya yüklemek için seçtiğiniz gruba uygulanır. Etiketlenmiş ve karşıya sonra etiketi seçilen görüntüler için de değiştirebilirsiniz.

    > [!TIP]
    > Farklı Kamera Açısı, aydınlatma, arka plan, türleri, stiller, grupları, boyutları, vb. ile görüntüleri karşıya yükleyin. Sınıflandırıcınızı değil ağırlıklı ve iyi genelleştirebilirsiniz emin olmak için çeşitli fotoğraf türlerini kullanın.

    Özel görüntü işleme hizmeti kabul eder, .jpg, .png, .bmp biçimi eğitim resmi görüntü başına en fazla 6 MB. (Öngörü görüntülerini görüntü başına en fazla 4 MB olabilir.) Görüntüleri kısa kenarı 256 piksel olmasını öneririz. Kısa kenarı 256 piksel daha kısa herhangi bir görüntü, özel görüntü işleme hizmeti tarafından ölçeklenir.

    ![Denetim Ekle görüntüleri, sol üst ve alt merkezinde düğme olarak gösterilir.](./media/getting-started-build-a-classifier/add-images01.png)

    >[!NOTE] 
    > REST API, URL'lerden eğitim resimleri yüklemek için kullanılabilir.

2. Etiket ayarlamak için metin girin __My etiketleri__ alan ve ardından __+__ düğmesi. Görüntüleri karşıya yüklemek ve bunları etiketi için kullandığınız __[sayı] dosyaları karşıya yükleme__ düğmesi. Birden fazla etiket görüntüleri ekleyebilirsiniz. 

    > [!NOTE]
    > Karşıya yükleme zamanı sayısına ve seçtiğiniz resimlerin boyutunu göre değişir.

    ![Etiket ve karşıya yükleme sayfasının görüntüsü](./media/getting-started-build-a-classifier/add-images03.png)

3. Seçin __Bitti__ görüntüleri karşıya yüklendikten sonra.

    ![İlerleme çubuğu, tamamlanan tüm görevleri gösterir.](./media/getting-started-build-a-classifier/add-images04.png)

4. Başka bir dizi görüntüleri karşıya yüklemek için 1. adıma dönün. Örneğin, köpek ve ponies ayırt etmek istiyorsanız, karşıya yükleme ve ponies görüntülerini etiketleyin.

## <a name="train-and-evaluate-the-classifier"></a>Eğitme ve değerlendirme Sınıflandırıcısı

Sınıflandırıcı eğitmek için seçin **eğitme** düğmesi.

![Tarayıcı penceresinin sağ üst kısımda train düğmesidir.](./media/getting-started-build-a-classifier/train01.png)

Yalnızca bir sınıflandırıcı eğitmek için birkaç dakika sürer. Bu süre boyunca eğitim süreci hakkında bilgi görüntülenir.

![Tarayıcı penceresinin sağ üst kısımda train düğmesidir.](./media/getting-started-build-a-classifier/train02.png)

Eğitim sonra __performans__ görüntülenir. Duyarlık ve geri çağırma göstergeleri sınıflandırıcınızı otomatik teste dayanan olduğunu ne kadar iyi size söyler. Özel görüntü işleme hizmeti adlı bir işlemi kullanarak bu sayıları hesaplamak eğitimi için size gönderilen görüntüleri kullanan [k mızın çapraz doğrulama](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![Eğitim sonuçları genel kesinlik ve geri çağırma ve duyarlık Göster ve sınıflandırıcı her etiket için geri çağırma.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Seçtiğiniz her zaman **eğitme** düğmesi, sınıflandırıcınızı, yeni bir yineleme oluşturun. Eski tüm yinelemelerini görebilirsiniz **performans** sekmesinde silebilirsiniz herhangi eski olabilir. Bir yineleme sildiğinizde, benzersiz olarak kendisiyle ilişkilendirilmiş herhangi bir görüntü silme yukarı sonlandırın.

Sınıflandırıcı tüm görüntüleri her etiket tanımlayan bir model oluşturmak için kullanır. Model kalitesini test etmek için her görüntü modeli bulur görmek üzere modeli üzerinde sınıflandırıcı çalışır.

Kalitelerini sınıflandırıcı sonuçları görüntülenir.

|Sözleşme Dönemi|Tanım|
|---|---|
|__Duyarlık__|Ne zaman, nasıl büyük olasılıkla bir görüntü sınıflandırma sınıflandırıcınızı görüntünün doğru şekilde sınıflandırmak için mi? Sınıflandırıcı (köpek ve ponies) eğitmek için kullanılan tüm görüntüleri dışında ne kadarlık bir model doğru mu? 100 görüntüleri 99 doğru etiketler % 99 duyarlığını sağlar.|
|__Geri çağırma__|Doğru sınıflandırılmış tüm görüntüleri dışında kaç sınıflandırıcınızı doğru tanımlamak? Geri çağırma işlemi % 100 sınıflandırıcı eğitmek için kullanılan görüntüleri 38 köpek görüntüleri varsa, sınıflandırıcı 38 köpekler bulunan anlamına gelir.|

## <a name="next-steps"></a>Sonraki adımlar

[Modeli yeniden eğitme ve test](test-your-model.md)

