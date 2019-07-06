---
title: 'Öğretici: E-ticaret ürün görüntüleri - Content Moderator Orta'
titlesuffix: Azure Cognitive Services
description: Analiz ve ürün görüntüleri (Azure görüntü işleme ve özel görüntü kullanarak) belirtilen etiketlerle sınıflandırmak için bir uygulama ayarlayın. (Azure Content Moderator'ı kullanarak) daha fazla gözden geçirilmesi uygunsuz görüntüleri etiketleyin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: ec17f9f0206ef639bd47d694880c064a012ea1cf
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604185"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Öğretici: Azure Content Moderator ile orta e-ticaret ürün görüntüleri

Bu öğreticide, Azure Bilişsel Content Moderator dahil olmak üzere hizmetler sınıflandırmak için nasıl kullanılacağını ve bir e-ticaret senaryo Orta ürün görüntüleri öğreneceksiniz. Content Moderator'ın makine öğrenmesi tabanlı teknolojiler akıllı denetleme sistemi sağlamak için insan tarafından İnceleme teams ile birleştiren bir takım gözden geçirme daha sonra oluşturacağınız ve etiket (etiketler) görüntüleri uygulamak için görüntü işleme ve özel görüntü kullanırsınız.

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

Ardından, özel etiketler gözden geçirme Aracı'nda oluşturun (bkz [etiketleri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) bu işlemle ilgili yardıma ihtiyacınız olursa makale). Bu durumda, aşağıdaki etiketler ekleyeceğiz: **ünlü**, **ABD**, **bayrağı**, **çocuğunun**, ve **kalem**. Tüm etiketleri görüntü işleme algılanabilir kategorilerde olması gerekir (gibi **ünlü**); daha sonra algılamak için özel görüntü işleme sınıflandırıcı eğitme sürece, kendi özel etiketler ekleyebilirsiniz.

![Özel etiketleri yapılandırma](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio'da yeni proje iletişim kutusunu açın. Genişletin **yüklü**, ardından **Visual C#** , ardından **konsol uygulaması (.NET Framework)** .
1. Uygulama adı **EcommerceModeration**, ardından **Tamam**.
1. Bu proje için varolan bir çözümü ekliyorsanız, bu projeyi tek başlangıç projesi olarak seçin.

Bu öğreticide, projeye merkezi kodu vurgular, ancak her kod satırı ele olmaz. Tam içeriğini kopyalayın _Program.cs_ örnek projeden ([Eticaret katalog denetimi örnekleri](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) içine _Program.cs_ yeni projenizin dosya. Ardından, adımı kendiniz proje nasıl çalıştığı ve nasıl kullanılacağı hakkında bilgi edinmek için aşağıdaki bölümleri aracılığıyla.

## <a name="define-api-keys-and-endpoints"></a>API anahtarları ve uç noktalarını tanımlayın

Bu öğretici, üç bilişsel hizmetler kullanır. Bu nedenle, üç ilgili anahtarları ve API uç noktalarını gerektirir. Aşağıdaki alanları görmek **Program** sınıfı:

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

Güncellemeniz gerekecektir `___Key` abonelik anahtarlarınızın değerlerini içeren alanlar (elde edecekleriniz `CustomVisionKey` daha sonra), ve değiştirmeniz gerekebilir `___Uri` doğru bölgeyi tanımlayıcıları içerdikleri için alanları. Doldurun `YOURTEAMID` parçası `ReviewUri` alanında daha önce oluşturduğunuz gözden geçirme takım kimliği. Son bölümünde doldururlar `CustomVisionUri` daha sonra alan.

## <a name="primary-method-calls"></a>Birincil yöntem çağrıları

Aşağıdaki kodda bkz **ana** yönteminin resim URL'leri bir listesi üzerinden döngü. Her üç farklı hizmet görüntüsüyle çözümler, uygulanan etiketler, kayıtları **ReviewTags** dizisi ve Content Moderator araç gözden görüntüleri göndererek gözden geçirme için İnsan Moderatörler oluşturur. Aşağıdaki bölümlerde bu yöntemleri inceleyeceksiniz. İsterseniz, gözden geçirmek için hangi görüntüleri gönderilen kullanarak denetleyebilirsiniz **ReviewTags** etiketleri uygulanan denetlemek için bir koşullu deyimde dizisi.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>EvaluateAdultRacy yöntemi

Bkz: **EvaluateAdultRacy** yönteminde **Program** sınıfı. Bu yöntem, parametre bir resim URL'si ve anahtar-değer çiftleri dizisi alır. Content Moderator's resim API'si (REST kullanarak), görüntünün yetişkin ve Racy puanlarını almak için çağırır. Ya da için puan (aralığı 0 ile 1 arasında) 0.4 daha büyükse, buna karşılık gelen bir değer ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecomputervisiontags-method"></a>EvaluateComputerVisionTags yöntemi

Sonraki yöntem, bir resim URL'si ve görüntü işleme abonelik bilgilerinizi alır ve ünlüleri varlığını görüntüyü inceler. Bir veya daha fazla ünlüleri bulunursa buna karşılık gelen bir değer ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateCustomVisionTags yöntemi

Ardından, bkz **EvaluateCustomVisionTags** gerçek ürünleri sınıflandırır yöntemi&mdash;kalemler ve bu durumda toys bayraklar. Bölümündeki yönergeleri [sınıflandırıcı oluşturma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) kılavuzda bayrakları, toys ve kalemler (veya özel etiketlerinizi seçtiğiniz) görüntüleri algılamak için kendi özel görüntünüzü sınıflandırıcı oluşturma. Görüntüleri kullanabileceğiniz **örnek görüntüleri** klasörü [GitHub deposunu](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) hızlı bir şekilde Bu kategorilerden bazıları eğitmek için.

![Özel görüntü işleme web sayfası, kalemler, toys ve bayrakları eğitim resmi](images/tutorial-ecommerce-custom-vision.PNG)

Sınıflandırıcınızı eğittiğimize sonra tahmin uç nokta URL'si ve tahmin anahtar Al (bkz [URL'si ve tahmin anahtarını alma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key) bunları alınırken yardıma gereksinim duyarsanız) ve bu değerleri atamak, `CustomVisionKey` ve `CustomVisionUri` , sırasıyla alanları. Yöntemi, bir sınıflandırıcı sorgulamak için bu değerleri kullanır. Sınıflandırıcı görüntüde bir veya daha fazla özel etiketler bulursa, bu yöntem karşılık gelen değerleri ayarlar **ReviewTags** için dizi **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>İncelemeler için İnceleme aracı oluşturma

Önceki bölümlerde nasıl uygulama yetişkinlere yönelik ve müstehcen içerik (Content Moderator), ünlüleri (görüntü işleme) ve çeşitli diğer nesneleri (özel görüntü işleme) gelen görüntüleri tarar incelediniz. Ardından, bkz **CreateReview** uygulanan etiketlerini görüntülerle yükleyen yöntemi (olarak geçirilen _meta verileri_) için Content Moderator İnceleme aracı.

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

Görüntüleri İnceleme sekmesinde görünür [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/).

![Birkaç görüntüyü ve vurgulanan etiketlerini Content Moderator gözden geçirme Aracı'nın ekran görüntüsü](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Test görüntülerin listesini gönderin

İçinde gördüğünüz gibi **ana** yöntemi, bu program "C:Test" dizinle arar bir _Urls.txt_ görüntü URL'lerin bir listesini içeren dosya. Bu dosya ve dizin oluşturma veya yolu, bir metin dosyasına işaret edecek şekilde değiştirin. Ardından bu URL'leri test etmek istediğiniz görüntü dosyasıyla doldurun.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Programı çalıştırma

Yukarıdaki tüm adımları uyguladıysanız, program (ilgili etiketlerini için tüm üç hizmeti sorgulama) her bir görüntü işleme ve sonra Content Moderator İnceleme aracını etiketi bilgileri görüntülerle karşıya yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, içerik denetleme hakkında bilinçli kararlar için ürün görüntüleri analiz ederek, bunları ürün türüne göre etiket ve bir gözden geçirme ekibi izin vermek için bir program ayarlama. Ardından, görüntü denetimi ile ilgili ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Denetlenen görüntüleri İnceleme](./review-tool-user-guide/review-moderated-images.md)
