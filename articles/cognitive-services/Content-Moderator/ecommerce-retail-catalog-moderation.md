---
title: 'Öğretici: E-ticaret ürün görüntüleri - Content Moderator Orta'
titlesuffix: Azure Cognitive Services
description: Analiz ve ürün görüntüleri (Azure görüntü işleme ve özel görüntü kullanarak) belirtilen etiketlerle sınıflandırmak için bir uygulama ayarlayın ve daha fazla etiket uygunsuz görüntüleri (Azure Content Moderator'ı kullanarak) Gözden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 900ad8b7f676eb67f9ac0fc808600779f832a102
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539505"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Öğretici: Azure Content Moderator ile orta e-ticaret ürün görüntüleri

Bu öğreticide, Content Moderator dahil olmak üzere, Azure bilişsel hizmetler, etkili bir şekilde sınıflandırmak için nasıl kullanılacağını ve bir e-ticaret senaryo Orta ürün görüntüleri öğreneceksiniz. Görüntüleri çeşitli etiketleri (etiketler) uygulamak için görüntü işleme ve özel görüntü kullanır ve sonra Content Moderator'ın makine öğrenmesi tabanlı teknolojiler akıllı bir denetimi sağlamak için insan tarafından İnceleme teams ile birleştiren bir takım gözden geçirme oluşturur sistemi.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Content Moderator için kaydolun ve bir gözden geçirme ekibi oluşturun.
> * Olası yetişkinlere yönelik ve müstehcen içerik taraması için Content Moderator'ın resim API'sini kullanma.
> * Görüntü işleme hizmeti, ünlü içeriği (veya diğer bilgisayar-işleme-algılanabilir etiketleri) taramak için kullanın.
> * Bayrakları, toys ve kalemler (veya diğer özel etiketler) varlığını taramak için Custom Vision Service'i kullanın.
> * İnsan tarafından inceleme ve son karar birleşik tarama sonuçlarını sunar.

Tam örnek kodu [Eticaret katalog denetimi örnekleri](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) github deposu.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Content Moderator abonelik anahtarı. Bölümündeki yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Content Moderator hizmete abone olmak ve anahtarınızı alın.
- Görüntü işleme abonelik anahtarı (yukarıdaki yönergeleri).
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
- Custom Vision sınıflandırıcı (büyük/küçük harf bu toys, kalemler ve BİZE bayrakları) kullanan her bir etiket için görüntü kümesi.

## <a name="create-a-review-team"></a>Bir gözden geçirme takım oluştur

Başvurmak [deneyin Content Moderator Web'de](quick-start.md) kaydolmak yönergeler için Hızlı Başlangıç [Content Moderator gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) ve bir gözden geçirme ekibi oluşturun. Not **Takım Kimliği** değerini **kimlik bilgilerini** sayfası.

## <a name="create-custom-moderation-tags"></a>Özel denetimi etiketlerini oluştur

Ardından, özel etiketler gözden geçirme Aracı'nda oluşturun (başvurmak [etiketleri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) bu işlemle ilgili yardıma ihtiyacınız olursa makale). Bu durumda, aşağıdaki etiketler ekleyeceğiz: **ünlü**, **ABD**, **bayrağı**, **çocuğunun**, ve **kalem**. Tüm etiketleri görüntü işleme algılanabilir kategorilerde olması gerektiğini unutmayın (gibi **ünlü**); daha sonra algılamak için özel görüntü işleme sınıflandırıcı eğitme sürece, kendi özel etiketler ekleyebilirsiniz.

![Özel etiketleri yapılandırma](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio'da yeni proje iletişim kutusunu açın. Genişletin **yüklü**, ardından **Visual C#** , ardından **konsol uygulaması (.NET Framework)**.
1. Uygulama adı **EcommerceModeration**, ardından **Tamam**.
1. Bu proje için varolan bir çözümü ekliyorsanız, bu projeyi tek başlangıç projesi olarak seçin.

Bu öğreticide, projeye merkezi kodu vurgulanır, ancak bunu her gerekli kod satırı kapsamaz. Tam içeriğini kopyalayın _Program.cs_ örnek projeden ([Eticaret katalog denetimi örnekleri](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) içine _Program.cs_ yeni projenizin dosya. Ardından, adımı kendiniz proje nasıl çalıştığı ve nasıl kullanılacağı hakkında bilgi edinmek için aşağıdaki bölümleri aracılığıyla.

## <a name="define-api-keys-and-endpoints"></a>API anahtarları ve uç noktalarını tanımlayın

Yukarıda belirtildiği gibi Bu öğretici üç bilişsel hizmetler kullanır; Bu nedenle, üç ilgili anahtarları ve API uç noktalarını gerektirir. Aşağıdaki alanları görmek **Program** sınıfı:

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

Güncellemeniz gerekecektir `___Key` abonelik anahtarlarınızın değerlerini içeren alanlar (elde edecekleriniz `CustomVisionKey` daha sonra), ve değiştirmeniz gerekebilir `___Uri` böylece doğru bölgeyi tanımlayıcıları içerdikleri alanları. Doldurun `YOURTEAMID` parçası `ReviewUri` alanında daha önce oluşturduğunuz gözden geçirme takım kimliği. Son bölümünde doldurur `CustomVisionUri` daha sonra alan.

## <a name="primary-method-calls"></a>Birincil yöntem çağrıları

Aşağıdaki kodda bkz **ana** yönteminin resim URL'leri bir listesi üzerinden döngü. Her üç farklı hizmet görüntüsüyle çözümler, uygulanan etiketler, kayıtları **ReviewTags** dizisi ve İnsan Moderatörler (gönderen Content Moderator gözden geçirme Aracı'nı görüntülerin) için bir inceleme oluşturur. Aşağıdaki bölümlerde bu yöntemleri inceleyeceksiniz. Unutmayın isterseniz, burada, hangi görüntüleri gözden geçirmek için gönderilen kullanarak denetleyebilirsiniz **ReviewTags** etiketleri uygulanan denetlemek için bir koşullu deyimde dizisi.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>EvaluateAdultRacy yöntemi

Bkz: **EvaluateAdultRacy** yönteminde **Program** sınıfı. Bu yöntem, parametre bir resim URL'si ve anahtar-değer çiftleri dizisi alır. Content Moderator's resim API'si (REST kullanarak), görüntünün yetişkin ve Racy puanlarını almak için çağırır. Ya da için puan (aralığı 0-1) 0.4 daha büyükse, buna karşılık gelen bir değer ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecomputervisiontags-method"></a>EvaluateComputerVisionTags yöntemi

Sonraki yöntem, bir resim URL'si ve görüntü işleme abonelik bilgilerinizi alır ve ünlüleri varlığını görüntüyü inceler. Bir veya daha fazla ünlüleri bulunursa buna karşılık gelen bir değer ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateCustomVisionTags yöntemi

Ardından, bkz **EvaluateCustomVisionTags** gerçek ürünleri sınıflandırır yöntemi&mdash;kalemler ve bu durumda toys bayraklar. Bölümündeki yönergeleri [sınıflandırıcı oluşturma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) görüntülerde bayrakları, toys ve kalemler (veya özel etiketlerinizi seçtiğiniz) varolup olmadığını algılamak için kendi özel görüntünüzü sınıflandırıcı oluşturma kılavuzu. Görüntüleri kullanabileceğiniz **örnek görüntüleri** klasörü [GitHub deposunu](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) hızlı bir şekilde Bu kategorilerden bazıları eğitmek için.

![Özel görüntü işleme web sayfası, kalemler, toys ve bayrakları eğitim resmi](images/tutorial-ecommerce-custom-vision.PNG)

Sınıflandırıcınızı eğitim almış sonra tahmin uç nokta URL'si ve tahmin anahtar Al (bkz [URL'si ve tahmin anahtarını alma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key) bunları alınırken yardıma ihtiyacınız varsa) ve bu değerleri atamak, `CustomVisionKey` ve `CustomVisionUri` alanları , sırasıyla. Yöntemi, bir sınıflandırıcı sorgulamak için bu değerleri kullanır. Sınıflandırıcı görüntüde bir veya daha fazla özel etiketler bulursa, bu yöntem karşılık gelen değerleri ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>İncelemeler için İnceleme aracı oluşturma

Önceki bölümlerde, gelen görüntüleri yetişkinlere yönelik ve müstehcen içerik (Content Moderator), ünlüler (Computer Vision) ve çeşitli diğer nesneler (Custom Vision) bakımından tarama yöntemleri hakkında bilgi edindiniz. Şimdi, görüntüleri, uygulanan tüm etiketlerle birlikte, kullanıcıların incelemeleri için Content Moderator İnceleme aracına yükleyen **CreateReview** yöntemine (_Metadata_ olarak geçirilir) göz atın. 

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

Görüntüleri İnceleme sekmesinde görünür [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/).

![Birkaç görüntüyü ve vurgulanan etiketlerini Content Moderator gözden geçirme Aracı'nın ekran görüntüsü](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Test görüntülerin listesini gönderin

İçinde gördüğünüz gibi **ana** yöntemi, bu program "C:Test" dizinle arar bir _Urls.txt_ görüntü URL'lerin bir listesini içeren dosya. Böyle bir dosya ve dizin oluşturma veya yolun metin dosyanıza işaret edecek şekilde değiştirin ve bu dosyayla görüntüleri test etmek istediğiniz URL'leri Doldur.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Programı çalıştırma

Yukarıdaki adımları izlediyseniz, program (tüm üç hizmeti ilgili etiketlerini için sorgulama) her bir görüntü işleme ve Content Moderator gözden geçirme Aracı'etiketi bilgileri görüntülerle yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ürün görüntüleri bunları ürün türüne göre etiketleme ve bir gözden geçirme takım, içerik denetleme hakkında bilinçli kararlar verme amacıyla analiz etmek için program ayarlama. Ardından, görüntü denetimi ile ilgili ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Denetlenen görüntüleri İnceleme](./review-tool-user-guide/review-moderated-images.md)
