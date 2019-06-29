---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Yaklaşım analizi kapsayıcı örneği doğrulamak öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f68d9c7098f2b1ca782e2522c632c2e267b35336
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455101"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Yaklaşım analizi kapsayıcı örneği doğrulayın

1. Seçin **genel bakış** sekmesini ve IP adresini kopyalayın.
1. Örneğin, IP adresi kullanın ve yeni bir tarayıcı sekmesi açın `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının giriş sayfası olarak sunulur, kapsayıcı tamamlanamayacağını çalışıyor.

    ![Çalıştığını doğrulamak için kapsayıcı giriş sayfasını görüntüle](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Seçin **hizmet API açıklaması** bağlantı kapsayıcıları swagger sayfasına gidin.

1. Birini **POST** API'ler ve select **deneyin**.  Örnek giriş dahil olmak üzere parametreler görüntülenir:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Giriş, aşağıdaki JSON ile değiştirin:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. Ayarlama **showStats** true.

1. Seçin **yürütme** metinlerdeki belirlemek için.

    Burada negatif 0 ve 1 pozitif 1, 0-arasında bir puan kapsayıcıda paketlenmiş modeli oluşturur.

    Döndürülen JSON yanıtı güncelleştirilmiş metin için yaklaşım içerir:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

Biz belge artık ilişkilendirebilmek `id` özgün istek yükü belgeye JSON yanıt yükleri, `id`ve puanı tamamlandığını üzerinden `.98` çok olumlu bir yaklaşım belirten.