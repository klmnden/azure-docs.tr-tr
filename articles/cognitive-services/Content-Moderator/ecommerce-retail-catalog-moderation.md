---
title: machine learning ve Azure içerik Denetleyici ile AI ile e-ticaret katalog denetleme | Microsoft Docs
description: Otomatik olarak e-ticaret kataloglarıyla machine learning ve AI Orta
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 6177758eaa3e611ad67da0778d889df48b052d90
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095760"
---
# <a name="ecommerce-catalog-moderation-with-machine-learning"></a>machine learning ile e-ticaret katalog denetleme

Bu öğreticide, bir akıllı Kataloğu sistemi sağlamak için İnsan yönetimini makine destekli AI teknolojileri birleştirerek machine-learning-tabanlı akıllı e-ticaret katalog yönetimini uygulamak nasıl öğrenin.

![Sınıflandırılmış ürün görüntüleri](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>İş senaryosu

Sınıflandırmak ve ürün görüntüleri kategorilerdeki Orta için makine destekli teknolojileri kullanır:

1. Yetişkin (çıplak görüntüler)
2. Saldırganlardan (müstehcen)
3. Çok ünlüler
4. ABD bayrakları
5. Toys
6. Kalemler

## <a name="tutorial-steps"></a>Eğitmen adımları

Öğretici, bu adımları uygularken size yol gösterir:

1. Kaydolun ve içerik denetleyici ekibi oluşturun.
2. Olası ünlülerle ve bayrağı içerik yönetimini etiketler (etiketleri) yapılandırın.
3. Olası yetişkin ve saldırganlardan içerik için tarama için içerik denetleyicinin görüntü API kullanın.
4. Olası çok ünlüler için taramak üzere bilgisayar görme API kullanın.
5. Olası bayrakları varlığını taramak için özel görme hizmeti kullanın.
6. İnsan gözden geçirme ve son kararlar nuanced tarama sonuçlarını sunar.

## <a name="create-a-team"></a>Takım oluşturma

Başvurmak [Hızlı Başlangıç](quick-start.md) içerik denetleyici için kaydolun ve bir ekip oluşturmak için sayfa. Not **Takım Kimliği** gelen **kimlik bilgileri** sayfası.


## <a name="define-custom-tags"></a>Özel etiketler tanımlayın

Başvurmak [etiketleri](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) makale özel etiketler ekleyin. Yerleşik yanı sıra **yetişkin** ve **saldırganlardan** etiketleri, yeni etiketler etiketleri için açıklayıcı adlarını görüntülemek İnceleme aracı izin verin.

Örneğimizde, biz bu özel etiketler tanımlayın (**ünlülerle**, **bayrağı**, **bize**, **Oyuncak**, **kalem**):

![Özel etiketler yapılandırın](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>API anahtarları ve uç noktaları listesi

1. Öğretici, üç API'ları ve ilgili anahtarları ve API uç noktaları kullanır.
2. Abonelik bölgeleri ve içerik denetleyici gözden geçirme ekibi kimliğe göre API uç noktalarınız farklı

> [!NOTE]
> Öğretici, aşağıdaki uç noktalardan görünür bölgelerdeki Abonelik anahtarları kullanmak için tasarlanmıştır. API anahtarlarınızı URI'ler aksi anahtarlarınızı şu uç noktalar ile çalışmayabilir bölgesi ile eşleşecek şekilde emin olun:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Yetişkin ve saldırganlardan içeriği tara

1. İşlev, parametre olarak bir resim URL'si ve anahtar-değer çiftleri dizisi alır.
2. Yetişkin ve Racy puanları almak için içerik denetleyicinin görüntü API çağırır.
3. Puan (aralık: 0-1) 0.4 daha büyükse, değeri ayarlar **ReviewTags** için dizi **doğru**.
4. **ReviewTags** dizi gözden geçirme Aracı'nı karşılık gelen etiketinde vurgulamak için kullanılır.

        public static bool EvaluateAdultRacy(string ImageUrl, ref KeyValuePair[] ReviewTags)
        {
            float AdultScore = 0;
            float RacyScore = 0;

            var File = ImageUrl;
            string Body = $"{{\"DataRepresentation\":\"URL\",\"Value\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(ImageUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // {“answers”:[{“answer”:“Hello”,“questions”:[“Hi”],“score”:100.0}]}
                // Parse the response body. Blocking!
                GetAdultRacyScores(response.Content.ReadAsStringAsync().Result, out AdultScore, out RacyScore);
            }

            ReviewTags[0] = new KeyValuePair();
            ReviewTags[0].Key = "a";
            ReviewTags[0].Value = "false";
            if (AdultScore > 0.4)
            {
                ReviewTags[0].Value = "true";
            }

            ReviewTags[1] = new KeyValuePair();
            ReviewTags[1].Key = "r";
            ReviewTags[1].Value = "false";
            if (RacyScore > 0.3)
            {
                ReviewTags[1].Value = "true";
            }
            return response.IsSuccessStatusCode;
        }

## <a name="scan-for-celebrities"></a>Çok ünlüler tara

1. Kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) , [bilgisayar görme API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Tıklatın **alma API anahtarı** düğmesi.
3. Koşulları kabul edin.
4. Oturum açmak için kullanılabilir Internet hesapları listesinden seçin.
5. Hizmet sayfasında görüntülenen API anahtarları unutmayın.
    
   ![Bilgisayar görme API anahtarları](images/tutorial-computer-vision-keys.PNG)
    
6. Bilgisayar görme API görüntüsüyle tarar işlevi için proje kaynak koduna bakın.

         public static bool EvaluateComputerVisionTags(string ImageUrl, string ComputerVisionUri, string ComputerVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage Response = CallAPI(ComputerVisionUri, ComputerVisionKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (Response.IsSuccessStatusCode)
            {
                ReviewTags[2] = new KeyValuePair();
                ReviewTags[2].Key = "cb";
                ReviewTags[2].Value = "false";

                ComputerVisionPrediction CVObject = JsonConvert.DeserializeObject<ComputerVisionPrediction>(Response.Content.ReadAsStringAsync().Result);

                if ((CVObject.categories[0].detail != null) && (CVObject.categories[0].detail.celebrities.Count() > 0))
                {                 
                    ReviewTags[2].Value = "true";
                }
            }

            return Response.IsSuccessStatusCode;
        }

## <a name="classify-into-flags-toys-and-pens"></a>Flags, toys ve kalemler sınıflandırma

1. [Oturum](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/) için [özel görme API Önizleme](https://www.customvision.ai/).
2. Kullanım [Hızlı Başlangıç](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) bayrakları, toys ve kalemler olası varolup olmadığını algılamak için özel sınıflandırıcı oluşturmak için.
   ![Özel görme eğitim görüntüleri](images/tutorial-ecommerce-custom-vision.PNG)
3. [Tahmin uç nokta URL'sini alma](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) özel sınıflandırıcı için.
4. Görüntünüzü taramak için özel sınıflandırıcı tahmin uç noktanızı çağıran işlevi görmek için proje kaynak koduna bakın.

        public static bool EvaluateCustomVisionTags(string ImageUrl, string CustomVisionUri, string CustomVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(CustomVisionUri, CustomVisionKey, CallType.POST,
                                                   "Prediction-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // Parse the response body. Blocking!
                SaveCustomVisionTags(response.Content.ReadAsStringAsync().Result, ref ReviewTags);
            }
            return response.IsSuccessStatusCode;
        }       
 
## <a name="reviews-for-human-in-the-loop"></a>İncelemeler İnsan-içinde--döngü için

1. Önceki bölümlerde gelen yetişkin ve görüntülerinin saldırganlardan (içerik aracı), çok ünlüler (bilgisayar görme) ve bayrakları (özel görme) taraması.
2. Bizim eşleşme eşikleri her tarama bağlı olarak, nuanced durumlarda İnsan gözden geçirme için İnceleme aracı kullanımına.
        ortak statik bool CreateReview (dize ImageUrl, KeyValuePair [] meta veriler) {

            ReviewCreationRequest Review = new ReviewCreationRequest();
            Review.Item[0] = new ReviewItem();
            Review.Item[0].Content = ImageUrl;
            Review.Item[0].Metadata = new KeyValuePair[MAXTAGSCOUNT];
            Metadata.CopyTo(Review.Item[0].Metadata, 0);

            //SortReviewItems(ref Review);

            string Body = JsonConvert.SerializeObject(Review.Item);

            HttpResponseMessage response = CallAPI(ReviewUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            return response.IsSuccessStatusCode;
        }

## <a name="submit-batch-of-images"></a>Görüntülerin toplu iş gönderme

1. Bu öğretici bir "C:Test" dizin görüntü URL'lerin listesini içeren bir metin dosyası varsayar.
2. Aşağıdaki kodu dosyanın varlığını denetler ve tüm URL'leri belleğe okur.
            Görüntü var topdir taramak için URL'lerin listesini içeren bir metin dosyası için bir test dizin denetle = @"C:\test\"; var Urlsfile = topdir +"Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Tüm taramaları başlatmak

1. Daha önce bahsedilen metin dosyasındaki tüm görüntü URL'ler aracılığıyla bu üst düzey işlevi döngüsü.
2. Her API ile tarar ve eşleşme güvenirlik puan bizim ölçüt içinde düşerse bir gözden geçirme için İnsan araburucu oluşturur.
             her biri için resim URL'si dosyasındaki... foreach (URL'lerde var Url) {/ / yeni bir gözden geçirme etiketleri dizi ReviewTags Initiatize yeni KeyValuePair [MAXTAGSCOUNT]; =

                // Evaluate for potential adult and racy content with Content Moderator API
                EvaluateAdultRacy(Url, ref ReviewTags);

                // Evaluate for potential presence of celebrity (ies) in images with Computer Vision API
                EvaluateComputerVisionTags(Url, ComputerVisionUri, ComputerVisionKey, ref ReviewTags);

                // Evaluate for potential presence of custom categories other than Marijuana
                EvaluateCustomVisionTags(Url, CustomVisionUri, CustomVisionKey, ref ReviewTags);

                // Create review in the Content Moderator review tool
                CreateReview(Url, ReviewTags);
            }

## <a name="license"></a>Lisans

Tüm Microsoft Bilişsel Services SDK'ları ve örnekleri MIT lisansı ile lisanslanır. Daha fazla ayrıntı için bkz: [lisans](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları

Bilişsel hizmetler, istemci Kitaplığı & örnek, bu da dahil olmak üzere kullanan geliştiriciler bekleniyor "Developer kod, yürütmek için Microsoft Bilişsel Hizmetler" izleyin, bulunan http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Sonraki adımlar

Derleme ve kullanarak öğreticiyi genişletme [kaynak dosyaları proje](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) github'da.
