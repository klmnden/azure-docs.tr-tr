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
ms.openlocfilehash: 4d8d34d8dec52dd9173cea80c39097f46d32bff5
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735489"
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


## Real-time availability and vacancy information for bike docking station

The [Get Transit Dock Info API](https://aka.ms/AzureMapsMobilityTransitDock) of the Azure Maps Mobility Service, allows to request static and real-time information for a given bike or scooter docking station. We will make a request to get real-time data for a docking station for bikes. 

In order to make a request to the Get Transit Dock Info API, you will need the **dockId** for that station. You can get the dock ID by making a search request to the [Get Nearby Transit API](https://aka.ms/AzureMapsMobilityNearbyTransit) and setting the **objectType** parameter to "bikeDock". Follow the steps below to get real-time data of a docking station for bikes.


### Get dock ID

To get **dockID**, follow the steps below to make a request to the Get Nearby Transit API:

1. In Postman, click **New Request** | **GET request** and name it **Get dock ID**.

2.  On the Builder tab, select the **GET** HTTP method, enter the following request URL, and click **Send**.
 
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
