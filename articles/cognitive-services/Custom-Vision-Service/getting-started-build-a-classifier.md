---
title: Özel görme Service - Azure Bilişsel hizmetler sınıflandırıcı yapı | Microsoft Docs
description: Fotoğraf nesneleri keşfedilir bir sınıflandırıcı oluşturmak için özel görme hizmetini kullanmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: 6dc271c13f53a445c7d1101f5264d890208bd03c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352907"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Bir sınıflandırıcı özel görme ile oluşturma

Özel görme hizmetini kullanmak için önce bir sınıflandırıcı oluşturmanız gerekir. Bu belgede, web tarayıcınız üzerinden bir sınıflandırıcı oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Bir sınıflandırıcı oluşturmak için öncelikle olması gerekir:

- Geçerli bir [Microsoft hesabı](https://account.microsoft.com/account) veya Azure Active Directory Orgıd ("iş veya Okul hesabı"), böylece customvision.ai oturum açabilir ve çalışmaya başlama.

    > [!IMPORTANT] 
    > Azure Active Directory (Azure AD) kullanıcılardan Orgıd LOGIN'in [Ulusal Bulutlar](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) şu anda desteklenmiyor.

- (30 görüntüleri etiket başına en az ile) sınıflandırıcı eğitmek için görüntüleri bir dizi.

- Sınıflandırıcı eğitildi sonra sınıflandırıcı test etmek için birkaç görüntüler.

- İsteğe bağlı: Microsoft Account veya Orgıd ile ilişkili bir Azure aboneliği. Bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

    > [!IMPORTANT]
    > Bir Azure aboneliğiniz, yalnızca oluşturmak açabilirler __sınırlı deneme__ projeleri. Bir Azure aboneliğiniz varsa, özel görme hizmet eğitim ve tahmin kaynaklarında oluşturmak için istenir [Azure portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) proje oluşturma sırasında.   

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Yeni bir proje oluşturmak için aşağıdaki adımları kullanın:

1. Web tarayıcınızda gidin [özel görme web sayfası](https://customvision.ai). Seçin __oturum__ hizmeti kullanmaya başlamak için.

    ![Oturum açma sayfasının görüntüsü](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Özel görme hizmetine oturum açtıktan sonra projeleri bir listesi sunulur. Test etmek için iki "deneme sınırlı" projeleri'dışında projeler bir Azure kaynakla ilişkilendirilmiş. Bir Azure kullanıcı varsa, ilişkili tüm projeleri görürsünüz [Azure kaynakları](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) olan erişim. 

2. İlk projenizi oluşturmak için seçin **yeni proje**. İlk projenizi için hizmet koşullarını kabul etmesi istenir. Onay kutusunu seçin ve ardından **kabul ediyorum** düğmesi. **Yeni proje** iletişim kutusu görüntülenir.

    ![Yeni Proje iletişim kutusu adı, açıklama ve etki alanları vardır.](./media/getting-started-build-a-classifier/new-project.png)

3. Bir ad ve proje için bir açıklama girin. Kullanılabilir etki alanlarını birini seçin. Her etki alanı sınıflandırıcı görüntülerinin belirli türleri için aşağıdaki tabloda açıklandığı gibi en iyi duruma getirir:

    |Etki alanı|Amaç|
    |---|---|
    |__Genel__| Çok çeşitli görüntü sınıflandırma görevleri için en iyi duruma getirilmiş. Diğer etki alanlarıyla hiçbiri uygun veya seçmek için hangi etki alanını emin değilseniz, genel etki alanını seçin. |
    |__Yemek__|Bir restoran menüsünde göreceğiniz şekilde tabaklar fotoğraflar için en iyi duruma getirilmiş. Tek tek fruits veya et fotoğraflarını sınıflandırmak yemek etki alanını kullanın.|
    |__Bilinen yerler__|Tanınabilir işaretleri için doğal ve yapay en iyi duruma getirilmiş. Yer işareti fotoğraf açıkça görülebilir olduğunda bu etki alanında en iyi şekilde çalışır. Yer işareti biraz önüne kişiler tarafından engellendiği olsa bile bu etki alanı çalışır.|
    |__Perakende__|Bir alışveriş katalog veya alışveriş Web sitesinde bulunan görüntüleri için en iyi duruma getirilmiş. Yüksek duyarlılık elbiselerini, pantalonların gömlekler arasında ve sınıflandırma istiyorsanız, bu etki alanı kullanın.|
    |__Yetişkin__|Yetişkin İçeriği ve yetişkin olmayan içerik daha iyi tanımlamak için en iyi duruma getirilmiş. Örneğin, görüntüleri bathing Setleri kişilerin engellemek istiyorsanız, bu etki alanı, bunu yapmak için özel bir sınıflandırıcı oluşturmanıza olanak sağlar.|
    |__Compact etki alanları__| Mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamalar için en iyi duruma getirilmiş. Compact etki alanı tarafından oluşturulan modelleri, yerel olarak çalıştırmak için verilebilir.|

    İsterseniz, etki alanı daha sonra değiştirebilirsiniz.

4. Bir kaynak grubu seçin. Kaynak grubu açılır listesinde, tüm özel görme hizmeti kaynak dahil, Azure kaynak gruplarını gösterir. Ayrıca create select __sınırlı deneme__. Sınırlı deneme giriş Azure olmayan kullanıcı aralarından seçim yapabileceğiniz açabilecektir yalnızca kaynak grubudur.

    Projeyi oluşturmak için seçin __proje oluştur__.

## <a name="upload-and-tag-images"></a>Karşıya yükleme ve etiket görüntüleri

1. Görüntüleri sınıflandırıcı eklemek için kullanın __eklemek görüntüleri__ düğmesine tıklayın ve ardından __göz atın, yerel dosya__. Seçin __açık__ etiketleme için taşımak için.

    > [!TIP]
    > Görüntüleri seçtikten sonra bunları etiketlemeniz gerekir. Etiket görüntüleri kullanmayı planlıyorsanız etiketlere göre karşıya yüklemek daha kolay olabilir, karşıya yüklemek için seçtiğiniz resim grubunu uygulanır. Etiketli ve karşıya sonra etiketi seçilen görüntüler için de değiştirebilirsiniz.

    > [!TIP]
    > Görüntüler farklı Kamera Açısı, aydınlatma, arka plan, türleri, stiller, gruplar, boyutları, vb. ile karşıya yükleyin. Sınıflandırıcı değil ağırlıklı ve generalize komutunu da emin olmak için çeşitli fotoğraf türlerini kullanın.

    Özel görme hizmet eğitim görüntüleri .jpg, .png ve .bmp biçimi kabul 6 MB görüntü başına. (Tahmin görüntüleri görüntü başına en fazla 4 MB olabilir.) Görüntüleri 256 piksel kısa kenarı üzerinde olmasını öneririz. Tüm görüntüleri kısa kenarı üzerinde 256 pikselden kısa özel görme hizmeti tarafından ölçeklenir.

    ![Ekle görüntüleri denetim sol üst ve alt merkezinde düğmesi olarak gösterilir.](./media/getting-started-build-a-classifier/add-images01.png)

    ![Gözat yerel dosyaları düğmesini Alt merkezi gösterilir.](./media/getting-started-build-a-classifier/add-images02.png)

    >[!NOTE] 
    > REST API URL'lerden eğitim yansımaları yüklemek için kullanılabilir.

2. Etiket ayarlamak için metin girin __My etiketleri__ alan ve ardından __+__ düğmesi. Resimler yükleyin ve bunları etiketlemek için kullanmak __[sayı] dosyaları karşıya yükleme__ düğmesi. Birden fazla etiket yansımalarına ekleyebilirsiniz. 

    > [!NOTE]
    > Karşıya yükleme zamanı, sayı ve seçtiğiniz resimlerin boyutu göre değişir.

    ![Etiket ve karşıya yükleme sayfasının görüntüsü](./media/getting-started-build-a-classifier/add-images03.png)

3. Seçin __Bitti__ görüntüleri karşıya sonra.

    ![İlerleme çubuğu tüm görevlerin tamamlanmasını gösterir.](./media/getting-started-build-a-classifier/add-images04.png)

4. Başka bir dizi görüntüyü karşıya yüklemek için adım 1 döndürür. Örneğin, köpekler ponies arasında ayrım yapmak istiyorsanız, karşıya yükleme ve ponies görüntülerini etiketi.

## <a name="train-and-evaluate-the-classifier"></a>Eğitmek ve sınıflandırıcı değerlendir

Sınıflandırıcı Eğitilecek seçin **eğitmek** düğmesi.

![Tren düğmesi tarayıcı penceresinin sağ üst bulunur.](./media/getting-started-build-a-classifier/train01.png)

Yalnızca, sınıflandırıcı eğitmek için birkaç dakika sürer. Bu süre boyunca, eğitim işlemi hakkında bilgi görüntülenir.

![Tren düğmesi tarayıcı penceresinin sağ üst bulunur.](./media/getting-started-build-a-classifier/train02.png)

Eğitim sonra __performans__ görüntülenir. Precision ve geri çağırma göstergeleri, sınıflandırıcı otomatik testlerine dayanır ne kadar iyi size söyler. Özel görme hizmetini kullanan bir işlemi kullanarak bu sayıları hesaplamak eğitim adlı için gönderdiğiniz görüntüleri [k-Katlama çapraz doğrulama](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![Eğitim sonuçları genel duyarlık ve geri çağırma ve duyarlık Göster ve sınıflandırıcı her etiketi için geri çağırma.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Seçtiğiniz her zaman **tren** düğmesi, yeni bir, sınıflandırıcı yinelemesini oluşturun. Eski tüm yinelemelerini görebilirsiniz **performans** sekmesi ve silebilirsiniz herhangi eski olabilir. Yineleme sildiğinizde, benzersiz olarak onunla ilişkili tüm görüntüleri silme yukarı sonlandırın.

Sınıflandırıcı tüm görüntüleri her etiket tanımlayan bir model oluşturmak için kullanır. Model kalitesini test etmek için her modeli bulur görmek üzere modeli görüntüde sınıflandırıcı çalışır.

Nitelikleri sınıflandırıcı sonuçları görüntülenir.

|Sözleşme Dönemi|Tanım|
|---|---|
|__Duyarlılık__|Ne zaman nasıl büyük olasılıkla bir görüntü sınıflandırmak görüntünün doğru sınıflandırmak için sınıflandırıcı mi? (Köpekler ve ponies) sınıflandırıcı eğitmek için kullanılan tüm görüntüleri dışında yüzde model doğru mı aldınız? 100 görüntüleri dışında 99 doğru etiketleri % 99 kesinliğini sağlar.|
|__Geri çağırma__|Doğru sınıflandırılmış tüm görüntüleri dışında kaç, sınıflandırıcı doğru tanımlamak? Geri çağırma işlemi % 100 sınıflandırıcı eğitmek için kullanılan görüntüleri 38 köpek görüntüler varsa sınıflandırıcı 38 köpekler anlamına gelir.|

## <a name="next-steps"></a>Sonraki adımlar

[Test ve model yeniden eğitme](test-your-model.md)

