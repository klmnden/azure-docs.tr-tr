---
title: Azure haritalar'ın gerçek zamanlı verilerde nasıl | Microsoft Docs
description: Azure haritalar Mobility hizmetini kullanarak gerçek zamanlı veri isteği.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: aaab5ef4d8fc3d60a12f9e9f85f2846695fd1ab4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329668"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmetini kullanarak gerçek zamanlı veri isteği

Bu makalede Azure haritalar'ı kullanmayı gösterir [Mobility hizmeti](https://aka.ms/AzureMapsMobilityService) isteği gerçek zamanlı aktarım sırasında verileri.

Bu makalede, öğreneceksiniz nasıl yapılır:


 * Tüm satırların verilen durağında gelen gerçek zamanlı sonraki gelen istek
 * Verilen bisiklet takma için gerçek zamanlı bilgi isteyin.


## <a name="prerequisites"></a>Önkoşullar

Azure haritalar genel aktarım API çağrıları yapmak için bir haritalar hesabı ve anahtarı gereklidir. Hesap oluşturma ve anahtar alma hakkında daha fazla bilgi için bkz: [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md).

Bu makalede [Postman uygulamasını](https://www.getpostman.com/apps) REST çağrılarını oluşturulacak. Tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz.


## <a name="request-real-time-arrivals-for-a-stop"></a>Gerçek zamanlı varış Durma için istek

Gerçek zamanlı varış verilerini bir belirli genel geçişi durdurma isteğinde bulunmak için bir isteğin yapılacağı gerekecektir [gerçek zamanlı varış API](https://aka.ms/AzureMapsMobilityRealTimeArrivals) Azure haritalar'ın [Mobility hizmeti](https://aka.ms/AzureMapsMobilityService). İhtiyacınız olacak **metroID** ve **stopID** istek tamamlanamadı. İstek bu parametreleri hakkında daha fazla bilgi için kılavuzundan bizim nasıl yapılır bkz [genel geçiş yolları istek](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

"522" bizim metro kullanalım kullanımını durdurma kimliği "2060603" ve "Seattle – Ankara – Bellevue, WA" alanı için bir veri yolu kimlik Durdur metro olan kimliği "ye 24 St & 162nd Ave Ne, Bellevue WA". Bu durdurma, sonraki tüm Canlı varış sonraki beş gerçek zamanlı varış veri istemek için aşağıdaki adımları tamamlayın:

1. İstekleri depolanacağı bir koleksiyon oluşturun. Postman uygulamasında seçin **yeni**. İçinde **Yeni Oluştur** penceresinde **koleksiyon**. Koleksiyonu adlandırın ve seçin **Oluştur** düğmesi.

2. İsteği oluşturmak için Seç **yeni** yeniden. İçinde **Yeni Oluştur** penceresinde **istek**. Girin bir **istek adı** isteği, istek kaydedin ve ardından konum olarak önceki adımda oluşturduğunuz koleksiyonu seçin **Kaydet**.

    ![Postman içinde bir isteği oluştur](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde GET HTTP yöntemini seçin ve bir GET isteği oluşturmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=2060603&transitType=bus
    ```

4. Başarılı bir isteği sonra şu yanıtı alırsınız.  Bu parametre 'scheduleType' tahmini varış zamanı gerçek zamanlı veya statik veri dayalı olup olmadığını tanımlar dikkat edin.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 4,
                "scheduleType": "realTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 30,
                "scheduleType": "scheduledTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```


## <a name="real-time-data-for-bike-docking-station"></a>Gerçek zamanlı verileri bisiklet takma

[Alma geçiş Dock bilgisi API](https://aka.ms/AzureMapsMobilityTransitDock) kullanılabilirlik gibi statik ve gerçek zamanlı bilgiler ve belirli bir bisiklet veya scooter takma birimi boş konum bilgilerini istemek için Azure haritalar Mobility hizmeti, sağlar. Gerçek zamanlı veriler için takma bisiklet alma isteği yapacağız.

Alma geçiş Dock bilgisi API için bir istekte bulunmak için ihtiyacınız olacak **dockId** istasyona ait. Bir arama isteğine yaparak dock kimliği alabilirsiniz [alma yakında aktarım API](https://aka.ms/AzureMapsMobilityNearbyTransit) ve ayarı **objectType** "bikeDock" parametresi. Bisiklet için takma, gerçek zamanlı veri almak için aşağıdaki adımları izleyin.


### <a name="get-dock-id"></a>Dock Kimliğini alın

Alınacak **dockID**, yakında geçiş Al API'si için istekte bulunmak için aşağıdaki adımları izleyin:

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **Get dock kimliği**.

2.  Oluşturucu sekmesinde **alma** HTTP yöntemi, aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Başarılı bir isteği sonra şu yanıtı alırsınız. Artık bildirim **kimliği** yanıtta kullanılabilir daha sonra alma geçiş Dock bilgisi API isteğinde sorgu parametresi olarak.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 30,
                    "lastUpdated": "2019-05-21T20:06:59-04:00",
                    "operatorInfo": {
                        "id": "80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Gerçek zamanlı bisiklet dock durumunu Al

Seçili dock için gerçek zamanlı veri almak için alma geçiş Dock bilgisi API isteği yapmak için aşağıdaki adımları izleyin.

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **gerçek zamanlı dock veri alma**.

2.  Oluşturucu sekmesinde **alma** HTTP yöntemi, aşağıdaki istek URL'sini girin ve tıklatın **Gönder**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Başarılı bir isteği sonra aşağıdaki yapıya yanıtı alırsınız:

    ```JSON
    {
        "availableVehicles": 1,
        "vacantLocations": 29,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-05-21T20:26:47-04:00",
        "operatorInfo": {
            "id": "80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Sonraki adımlar

İstek Mobility Hizmeti'ni kullanarak geçiş verilerini öğrenin:

> [!div class="nextstepaction"]
> [İstek aktarım sırasında verileri nasıl](how-to-request-transit-data.md)

Azure haritalar Mobility hizmeti API belgelerini keşfedin:

> [!div class="nextstepaction"]
> [Mobility hizmeti API'si belgeleri](https://aka.ms/AzureMapsMobilityService)
