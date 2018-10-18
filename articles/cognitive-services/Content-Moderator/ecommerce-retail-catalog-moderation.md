---
title: 'Öğretici: eTicaret katalog denetimi - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Makine öğrenmesi ve AI ile eTicaret kataloglarını otomatik olarak denetleyin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 0bd61c3f1a4f660076be4e87bb5443302e5dc013
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364003"
---
# <a name="tutorial-ecommerce-catalog-moderation-with-machine-learning"></a>Öğretici: Makine öğrenmesi ile eTicaret katalog denetimi

Bu öğreticide, akıllı bir katalog sistemi sağlamak için makine destekli AI teknolojilerini insan denetimiyle birleştirerek makine öğrenmesi tabanlı akıllı eticaret kataloğu denetiminin nasıl gerçekleştirildiğini öğreneceğiz.

![Sınıflandırılmış ürün resimleri](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>İş senaryosu

Makine destekli teknolojileri kullanarak ürün resimlerini şu kategoriler altında sınıflandırın ve denetleyin:

1. Yetişkin (Çıplaklık)
2. Müstehcen (Kışkırtıcı)
3. Ünlüler
4. US Etiketleri
5. Oyuncaklar
6. Kalemler

## <a name="tutorial-steps"></a>Öğretici adımları

Öğretici, şu adımlarda size yol gösterir:

1. Kaydolma ve Content Moderator takımı oluşturma.
2. Olası ünlü ve bayrak içeriği için denetim etiketlerini yapılandırma.
3. Olası yetişkinlere yönelik ve müstehcen içerik taraması için Content Moderator'ın resim API'sini kullanma.
4. Olası ünlüler taraması için Görüntü İşleme API'sini kullanma.
5. Olası bayrak varlığını taramak için Özel Görüntü İşleme hizmetini kullanma.
6. İnsan incelemesi ve son karar için ayrıntılı tarama sonuçlarını sunma.

## <a name="create-a-team"></a>Takım oluşturma

Content Moderator'a kaydolmak ve takım oluşturmak için [Hızlı Başlangıç](quick-start.md) sayfasına bakın. **Kimlik Bilgileri** sayfasındaki **Takım Kimliği**'ni not alın.


## <a name="define-custom-tags"></a>Özel etiketler tanımlama

Özel etiketler eklemek için [Etiketler](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) makalesine bakın. Yerleşik **adult** ve **racy** etiketlerine ek olarak, yeni etiketler de inceleme aracının etiketler için tanımlayıcı adlar görüntülemesine izin verir.

Biz örneğimizde şu özel etiketleri tanımladık (**celebrity**, **flag**, **us**, **toy**, **pen**):

![Özel etiketleri yapılandırma](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>API anahtarlarınızı ve uç noktalarınızı listeleme

1. Öğreticide üç API, bunlara karşılık gelen anahtarlar ve API uç noktaları kullanılır.
2. Abonelik bölgelerinize ve Content Moderator İnceleme Takımı Kimliğinize bağlı olarak, API uç noktaları farklı olacaktır.

> [!NOTE]
> Öğretici, aşağıdaki uç noktalarda görünür olan bölgelerdeki abonelik anahtarlarını kullanacak şekilde tasarlanmıştır. API anahtarlarınızla bölge Uri'lerinin eşleştiğinden emin olun; aksi takdirde anahtarlarınız aşağıdaki uç noktalarla çalışmayabilir:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Yetişkinlere yönelik ve müstehcen içerikleri tarama

1. İşlev, resim URL'sini ve bir anahtar-değer çifti dizisini parametre olarak alır.
2. Yetişkin ve Müstehcen puanlarını almak için Content Moderator'ın Görüntü API'sini çağırır.
3. Puan 0,4'ten büyükse (0 ile 1 aralığındadır), **ReviewTags** dizisindeki değeri **True** olarak ayarlar.
4. İnceleme aracında buna karşılık gelen etiketi vurgulamak için **ReviewTags** dizisi kullanılır.

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

## <a name="scan-for-celebrities"></a>Ünlüleri tarama

1. [Görüntü İşleme API'sinin](https://azure.microsoft.com/services/cognitive-services/computer-vision/) [ücretsiz denemesine](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) kaydolun.
2. **API Anahtarını Al** düğmesine tıklayın.
3. Koşulları kabul edin.
4. Oturum açmak için, kullanılabilir İnternet hesapları listesinden seçim yapın.
5. Hizmet sayfanızda görüntülenen API anahtarlarını not alın.
    
   ![Görüntü İşleme API'si anahtarları](images/tutorial-computer-vision-keys.PNG)
    
6. Resmi Görüntü İşleme API'si ile tarayan işlev için proje kaynak koduna bakın.

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

## <a name="classify-into-flags-toys-and-pens"></a>Bayraklar, oyuncaklar ve kalemler olarak sınıflandırma

1. [Özel Görüntü İşleme API'si önizlemesinde](https://www.customvision.ai/) [oturum açın](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
2. Bayrakların, oyuncakların ve kalemlerin olası varlığını algılamak üzere kendi özel sınıflandırıcınızı oluşturmak için [Hızlı Başlangıç](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) bölümünü kullanın.
   ![Özel İşleme Eğitimi Resimleri](images/tutorial-ecommerce-custom-vision.PNG)
3. Özel sınıflandırıcınız için [tahmin uç noktası URL'sini alın](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api).
4. Resminizi taramak üzere özel sınıflandırıcı tahmin uç noktanızı çağıran işlevi görmek için proje kaynak koduna bakın.

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
 
## <a name="reviews-for-human-in-the-loop"></a>Döngüye insanı da dahil eden incelemeler

1. Önceki bölümlerde, gelen resimleri yetişkinlere yönelik ve müstehcen içerik (Content Moderator), ünlüler (Görüntü İşleme) ve Bayraklar (Özel Görüntü İşleme) için taradınız.
2. Her tarama için eşleşme eşiklerimiz temelinde, inceleme aracında insan incelemesi için sağlanan ayrıntılı örnekleri hazırlayın.
        public static bool CreateReview(string ImageUrl, KeyValuePair[] Metadata) {

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

## <a name="submit-batch-of-images"></a>Toplu resimleri gönderme

1. Bu öğreticide, resim Url'leri listesini içeren bir metin dosyasının bulunduğu "C:Test" dizinin var olduğu varsayılır.
2. Aşağıdaki kod dosyanın var olup olmadığını denetler ve tüm Url'leri belleğe okur.
            // Resim Url'lerini içeren metin dosyası için test dizinini denetleyip şu taramayı yapın: var topdir = @"C:\test\"; var Urlsfile = topdir + "Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Tüm taramaları başlatma

1. Bu üst düzey işlev, daha önce belirttiğimiz metin dosyasındaki tüm resim URL'lerinde döngü yapar.
2. Her API'yle bunları tarar ve eşleşme güvenilirlik puanı ölçütlerimize uyarsa insan denetleyiciler için bir inceleme oluşturur.
             // dosyadaki her resim URL'si için... foreach (var Url in Urls) { // Yeni bir inceleme etiketleri dizisi başlatın ReviewTags = new KeyValuePair[MAXTAGSCOUNT];

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

Tüm Microsoft Bilişsel Hizmetler SDK'ları ve örnekler, MIT Lisansı ile lisanslanmıştır. Diğer ayrıntılar için bkz. [LİSANS](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları

Bu istemci kitaplığı ve örnek de dahil olmak üzere Bilişsel Hizmetler'i kullanan geliştiricilerin, http://go.microsoft.com/fwlink/?LinkId=698895 adresinde bulunan "Microsoft Bilişsel Hizmetler için Geliştirici Kullanım Kuralları"na uyması beklenir.

## <a name="next-steps"></a>Sonraki adımlar

Github'daki [proje kaynak dosyalarını](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) kullanarak öğreticiyi derleyin ve genişletin.
