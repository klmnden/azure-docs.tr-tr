---
title: Azure haritalar kullanan bir bölge sınırının oluştur | Microsoft Docs
description: Azure haritalar'ı kullanarak bir bölge sınırının ayarlayın.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 112d0bd4b6802179692d0d177775027e552d1170
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60795849"
---
# <a name="set-up-a-geofence-by-using-azure-maps"></a>Azure haritalar'ı kullanarak bir bölge sınırının ayarlayın

Bu öğreticide Azure haritalar'ı kullanarak döndürürüz ayarlamak için temel adımlarında size kılavuzluk eder. Belirtilen yapı alanları taşıyarak Yardım yapım site yöneticileri İzleyici olası tehlikeli donanımı, biz Bu öğreticide ele bir senaryodur. Bir yapı site pahalı donanım ve düzenlemeleri içerir. Genellikle, ekipman yapım site içinde kalır ve izniniz olmadan bırakmaz gerektirir.

Azure haritalar veri karşıya API'si bir bölge sınırının depolamak için kullanırız ve bölge sınırının ekipman konumu bölge sınırının göre denetlemek için API kullanımı Azure eşler. Bölge sınırının sonuçları akış ve bölge sınırının sonuçlarına dayalı bir bildirim ayarlamak için Azure Event Grid kullanacağız.
Event Grid hakkında daha fazla bilgi için bkz: [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).
Bu öğreticide, öğreneceksiniz, nasıl yapılır:

> [!div class="checklist"]
> * Azure haritalar, karşıya yükleme veri API'sini kullanarak veri hizmeti döndürürüz alanında karşıya yükleyin.
> *   Döndürürüz olayları işlemek için bir olay ızgarası ayarlama.
> *   Kurulum döndürürüz olay işleyicisi.
> *   Logic Apps kullanarak döndürürüz olaylara yanıt olarak uyarılar ayarlayın.
> *   Bir yapı varlık oluşturma site içinde olsun veya olmasın izlemek için Azure haritalar döndürürüz hizmet API'lerini kullanın.


## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

Bu öğreticideki adımları tamamlamak için önce görmek ihtiyacınız [hesapları ve anahtarları yönetme](how-to-manage-account-keys.md) oluşturup fiyatlandırma katmanı, hesap aboneliğiniz S1 ile yönetebilirsiniz.

## <a name="upload-geofences"></a>Bölge sınırlarının karşıya yükleme

Karşıya veri API'si kullanarak yapı sitenin döndürürüz karşıya yüklemek için postman uygulama kullanacağız. Bu öğretici için yapı ekipman değil ihlal sabit bir parametre bir genel yapı site alanı yok varsayıyoruz. Bu sınır ihlalleri olan ciddi bir red ve Operations Manager'a raporlanır. Ek sınırlar en iyi duruma getirilmiş bir dizi farklı bir yapım izleyen kullanılabilir alanları genel yapı alan zamanlamaya göre. Ana Bölge sınırının süre sonu Ayarla sahip bir subsite1 olduğunu varsayıyoruz saat ve bundan sonra dolar. Gereksinimlerinize göre daha fazla iç içe geçmiş bölge sınırlarının oluşturabilirsiniz. Örneğin, burada iş zamanlamasını 1-4 hafta gerçekleşen ve alt site 2 iş 5-7 hafta içinde gerçekleştiği subsite1 olabilir. Tüm dilimleri proje başına tek bir veri kümesi olarak yüklenir ve kuralları zaman ve yer göre izlemek için kullanılır. Bölge sınırının veri biçimi hakkında daha fazla bilgi için bkz. [Döndürürüz GeoJSON veri](https://docs.microsoft.com/azure/azure-maps/geofence-geojson). Azure haritalar hizmetine karşıya veri yükleme ile ilgili daha fazla bilgi için bkz: [verileri karşıya yükleme API belgeleri](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) .

Postman uygulamasını açın ve Azure haritalar, verileri karşıya yükleme API kullanan yapı site döndürürüz karşıya yüklemek için aşağıdaki adımları izleyin.

1. Postman uygulamasını açıp yeni tıklayın | Yeni oluştur ve istek'i seçin. Bir koleksiyon ya da kaydedin ve klasör kaydetmek karşıya yükleme döndürürüz veriler için select bir istek adı girin.

    ![Bölge sınırlarının Postman kullanarak karşıya yükleme](./media/tutorial-geofence/postman-new.png)

2. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve bir POST isteğinde bulunmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    URL yolu GEOJSON parametresinde yüklenen verilerin veri biçimini temsil eder.

3. Tıklayın **Params**, POST isteği URL'sini kullanılacak aşağıdaki anahtar/değer çiftleri girin. Abonelik anahtarı değerini Azure haritalar abonelik anahtarınız ile değiştirin.
   
    ![Anahtar-değer parametreleri: Postman](./media/tutorial-geofence/postman-key-vals.png)

4. Tıklayın **gövdesi** sonra ham giriş biçimini seçin ve JSON giriş biçimi açılır listeden seçin. Aşağıdaki JSON verileri olarak karşıya yüklenecek sağlar:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Gönder ve yanıt üst bilgisi gözden geçirin. Location üst bilgisini gelecekte kullanım için veri indirin veya URI içeriyor. Ayrıca bir benzersiz içerdiği `udId` karşıya yüklenen veriler için.

   ```HTTP
   https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

## <a name="set-up-an-event-handler"></a>Bir olay işleyicisini

Operations Manager enter ve çıkış olayları ile ilgili bilgilendirmek için bildirimleri alan bir olay işleyicisi oluşturmalısınız.

İki oluşturacağız [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps) işlemek için Hizmetleri girin ve çıkış olayları. Ayrıca bu olaylar tarafından tetiklenen Logic Apps içinde olay tetikleyicilerini oluşturacağız. Fikirdir uyarıları göndermek için bu durumda e-postalar için Operations Manager her donanım girer veya yapım site çıkar. Olay döndürürüz girmek için aşağıdaki şekilde bir mantıksal uygulama oluşturma gösterilmektedir. Benzer şekilde, başka bir çıkış olayının oluşturabilirsiniz.
Tümünü gör [olay işleyicileri desteklenen](https://docs.microsoft.com/azure/event-grid/event-handlers) daha fazla bilgi için.

1. Azure Portalı'nda mantıksal uygulama oluşturma

   ![Mantıksal uygulamalar oluşturma](./media/tutorial-geofence/logic-app.png)

2. Bir HTTP isteği tetikleyicisini seçin ve ardından "outlook Bağlayıcısı'nı bir eylem olarak bir e-posta Gönder"
  
   ![Logic Apps şeması](./media/tutorial-geofence/logic-app-schema.png)

3. HTTP URL'sini kopyalayın ve HTTP URL uç noktasını oluşturmak için mantıksal uygulamayı kaydedin.

   ![Logic Apps uç noktası](./media/tutorial-geofence/logic-app-endpoint.png)


## <a name="create-an-azure-maps-events-subscription"></a>Azure haritalar olay aboneliği oluşturma

Azure haritalar, üç olay türlerini destekler. Azure haritalar'ın desteklenen olay türleri göz olabilir [burada](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps). İki farklı Aboneliklerde, enter için bir ve çıkış olayları için diğer oluşturacağız.

Olayları döndürürüz girmek için bir olay aboneliği oluşturmak için aşağıdaki adımları izleyin. Benzer şekilde döndürürüz çıkış olaylarına abone olabilirsiniz.

1. Azure haritalar hesabınız gidin [portal bu bağlantıyı](https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/dashboard/) ve olayları sekmesini seçin.

   ![Azure haritalar olayları](./media/tutorial-geofence/events-tab.png)

2. Bir olay aboneliği oluşturmak için olaylar sayfasından olay aboneliği seçin.

   ![Azure haritalar olayları aboneliği](./media/tutorial-geofence/create-event-subscription.png)

3. Olay aboneliği adlandırın ve Enter olay türüne abone olun. Artık Web kancası "Uç noktası türü" olarak seçin ve "Uç", mantıksal uygulama HTTP URL uç noktasını kopyalayın

   ![Olay aboneliği](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Bölge sınırının API kullanın

Bölge sınırının API denetlemek için kullanabileceğiniz olup olmadığını bir **cihaz** (donanım durum bir parçası değildir) içinde veya dışında bir döndürürüz. API alma döndürürüz daha iyi anlamak için. Biz, zaman içinde belirli bir donanım taşındığı farklı konumlara karşı sorgu. Aşağıdaki şekilde, belirli bir yapı cihazlarınızla benzersiz bir beş konumlarını gösterilmektedir **cihaz kimliği** kronolojik sırayla gözlemlenen. Bu beş konumların her biri döndürürüz değerlendirmek için kullanılan giriş ve çıkış dilimi karşı durum değişikliği. Bir durum değişikliği ortaya çıkarsa, bölge sınırının service mantıksal uygulama için Event Grid tarafından gönderilen bir olay tetikler. Sonuç olarak işlem Yöneticisi'ni karşılık gelen enter alır veya bir e-posta aracılığıyla bildirim çıkın.

> [!Note]
> Yukarıdaki senaryo ve davranışı temel aynı **cihaz kimliği** böylece aşağıdaki şekilde gösterildiği gibi beş farklı konumlara yansıtır.

![Bölge sınırının eşleme](./media/tutorial-geofence/geofence.png)

Postman uygulamasında aynı koleksiyonda yukarıda oluşturduğunuz yeni bir sekme açın. Oluşturucu sekmesinde GET HTTP yöntemini seçin:

Donanım farklı karşılık gelen konum koordinatlarını ile beş HTTP alma bölge sınırlaması API isteklerinin kronolojik sırayla gözlemlenen aşağıda verilmiştir. Her istek, yanıt gövdesi tarafından izlenir.
 
1. 1. konum:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Bölge sınırının sorgu 1](./media/tutorial-geofence/geofence-query1.png)

   Yanıt yukarıdaki bakarsanız, ana bölge sınırının negatif mesafe ekipman bölge sınırının içinde olduğu ve alt bölge sınırı'ndan pozitif alt bölge sınırının dışında olduğu anlamına gelir anlamına gelir. 

2. 2. konum: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Bölge sınırının sorgu 2](./media/tutorial-geofence/geofence-query2.png)

   Yukarıdaki JSON yanıtında dikkatle bakarsanız ekipman alt site dışında ancak ana sınırı içinde. Bir olayı tetiklenmiyor ve hiç e-posta gönderilir.

3. 3. konum: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Bölge sınırının sorgu 3](./media/tutorial-geofence/geofence-query3.png)

   Bir durum değişikliği oluştu ve artık ekipman içinde her iki ana ve alt site, bölge sınırlarının. Bu olay ve bildirim yayımlar için Operations Manager e-posta gönderilir.

4. 4. konum: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Bölge sınırının sorgu 4](./media/tutorial-geofence/geofence-query4.png)

   Karşılık gelen yanıt dikkatle gözlemleyerek ekipman alt bölge sınırının çıkıldı olsa bile hiçbir olay burada yayımlanabilmesi fark edebilirsiniz. Kullanıcının belirtilen GET isteğine zamanında bakarsanız, alt bölge sınırının göre bu süre sona ve donanım yine de ana bölge sınırının içinde olduğunu görebilirsiniz. Alt bölge sınırının altında geometri kimliği atabilirsiniz `expiredGeofenceGeometryId` yanıt gövdesi içinde.


5. 5. konum:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Bölge sınırının sorgu 5](./media/tutorial-geofence/geofence-query5.png)

   Donanım ana yapım site döndürürüz ayrıldığını görebilirsiniz. Bir olayı yayımlar, ciddi ihlal eder ve Operations Manager için kritik bir uyarı e-posta gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz, nasıl Azure haritalar, karşıya yükleme veri API'sini kullanarak veri hizmeti yükleyerek döndürürüz ayarlanır. Ayrıca Azure haritalar olayları Grid abone olma ve bölge sınırının olayları işlemek için nasıl kullanılacağını öğrendiniz. 

* Bkz: [Azure Logic apps'te içerik türlerini işleme](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type), Logic Apps, daha karmaşık bir mantık oluşturmak için JSON ayrıştırmak için kullanmayı öğrenin.
* Event Grid olay işleyicileri hakkında daha fazla bilgi edinmek için bkz: [Event Grid olay işleyicilerini desteklenen](https://docs.microsoft.com/azure/event-grid/event-handlers).
