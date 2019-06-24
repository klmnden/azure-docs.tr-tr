---
title: 'Hızlı Başlangıç: Python ve Anomali algılayıcısı REST API kullanarak bir toplu iş olarak anormallikleri | Microsoft Docs'
description: Toplu olarak ya da akış verileri, veri serisinde prosesler algılamak için Anomali algılayıcısı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 0ab39a7bb86671efb6258e3171e3bf7847aa241f
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341736"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Hızlı Başlangıç: Python ve Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın

Zaman serisi verilerinizdeki anormallikleri algılamak için Anomali algılayıcısı API'SİNİN iki algılama modunu kullanmaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bu bir Python uygulaması JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Toplu iş olarak anomalileri algılayın                        | Zaman serisi verileri ve algılanan anomalileri konumlarını her veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı. |
| En son veri noktası durumunu anomali algılama | Zaman serisi verilerinde en son veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı.                                                                                                                                         |

 Bu uygulama Python'da yazılmıştır, ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

## <a name="prerequisites"></a>Önkoşullar

- [Python 2.x veya 3.x](https://www.python.org/downloads/)

- [İstekleri Kitaplığı](http://docs.python-requests.org) python için

- Bir JSON dosyası içeren zaman serisi verilerini işaret eder. Bu Hızlı Başlangıç için örnek veri çubuğunda bulunabilir [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız metin düzenleyicisinde veya IDE içinde yeni bir python dosyası oluşturun. Aşağıdaki içeri aktarmaları ekleyin.

    ```python
    import requests
    import json
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz bir URI'leri aşağıdadır. Bunlar daha sonra API oluşturmak için hizmet uç noktanıza eklenir istek URL'lerini.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Batch algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasına algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```python
    batch_detection_url = "/anomalydetector/v1.0/timeseries/entire/detect"
    latest_point_detection_url = "/anomalydetector/v1.0/timeseries/last/detect"

    endpoint = "[YOUR_ENDPOINT_URL]"
    subscription_key = "[YOUR_SUBSCRIPTION_KEY]"
    data_location = "[PATH_TO_TIME_SERIES_DATA]"
    ```

3. JSON veri dosyasında, açarak ve kullanarak okuma `json.load()`.

    ```python
    file_handler = open(data_location)
    json_data = json.load(file_handler)
    ```

## <a name="create-a-function-to-send-requests"></a>İstekleri göndermek için bir işlev oluşturma

1. Adlı yeni bir işlev oluşturma `send_request()` yukarıda oluşturulan değişkenleri alır. Ardından aşağıdaki adımları gerçekleştirin.

2. İstek üst bilgileri için bir sözlük oluşturun. Ayarlama `Content-Type` için `application/json`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı.

3. İstek gönderirken `requests.post()`. Uç nokta ve anomali algılama URL'NİZİN tam istek URL'si birleştirin ve üst bilgiler ve json istek verileri içerir. Ve ardından yanıtı döndürür.

```python
def send_request(endpoint, url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json',
               'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(
        endpoint+url, data=json.dumps(request_data), headers=headers)
    return json.loads(response.content.decode("utf-8"))
```

## <a name="detect-anomalies-as-a-batch"></a>Toplu iş olarak anomalileri algılayın

1. Adlı bir yöntem oluşturma `detect_batch()` toplu olarak veri boyunca anomalileri algılamak için. Çağrı `send_request()` , uç noktası, url, abonelik anahtarı ve json verilerini yukarıda oluşturulan yöntemi.

2. Çağrı `json.dumps()` sonucuna biçimlendirin ve konsola yazdırır.

3. Yanıt içeriyorsa `code` alan, hata kodu ve hata iletisi yazdırın.

4. Aksi takdirde, anomalileri konumları, veri kümesinde bulun. Yanıtın `isAnomaly` alan belirli veri noktasına bir anomali olup ilgili bir Boole değeri içerir. Liste üzerinde yineleme yapmak ve herhangi bir dizin yazdırma `True` değerleri. Karşılanmadığı, bu değerler, anormal veri noktası dizini karşılık gelir.

```python
def detect_batch(request_data):
    print("Detecting anomalies as a batch")
    result = send_request(endpoint, batch_detection_url,
                          subscription_key, request_data)
    print(json.dumps(result, indent=4))

    if result.get('code') != None:
        print("Detection failed. ErrorCode:{}, ErrorMessage:{}".format(
            result['code'], result['message']))
    else:
        # Find and display the positions of anomalies in the data set
        anomalies = result["isAnomaly"]
        print("Anomalies detected in the following data positions:")
        for x in range(len(anomalies)):
            if anomalies[x] == True:
                print(x)
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktası durumunu anomali algılama

1. Adlı bir yöntem oluşturma `detect_latest()` en son, zaman serisi veri noktasına bir anomali olup olmadığını belirlemek için. Çağrı `send_request()` uç noktası, url, abonelik anahtarı ve json verilerini yukarıda yöntemi. 

2. Çağrı `json.dumps()` sonucuna biçimlendirin ve konsola yazdırır.

```python
def detect_latest(request_data):
    print("Determining if latest data point is an anomaly")
    # send the request, and print the JSON result
    result = send_request(endpoint, latest_point_detection_url,
                          subscription_key, request_data)
    print(json.dumps(result, indent=4))
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yüklemek ve isteği gönder

1. Bir dosya işleyicisi'ni açarak ve kullanarak JSON zaman serisi verilerinin yük `json.load()` üzerindeki. Ardından anomali algılama yöntemleri yukarıda oluşturulan çağırın.

```python
file_handler = open(data_location)
json_data = json.load(file_handler)

detect_batch(json_data)
detect_latest(json_data)
```

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub üzerinde JSON yanıtı görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek batch algılama yanıt](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [En son noktası algılama yanıt örneği](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
