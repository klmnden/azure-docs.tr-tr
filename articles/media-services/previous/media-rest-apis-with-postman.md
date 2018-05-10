---
title: Postman Azure Media Services REST API çağrıları için yapılandırma
description: Postman Media Services REST API çağrıları için yapılandırmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2017
ms.author: juliako
ms.openlocfilehash: 72b110cac8d4945c958d760ff98e2da2f2796b62
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Postman Media Services REST API çağrıları için yapılandırma

Bu öğretici nasıl yapılandırılacağını göstermektedir **Postman** böylece Azure Media Services (AMS) REST Apı'lerinizi çağırmak için kullanılabilir. Öğretici ortamı ve koleksiyon dosyalarıyla içeri aktarmak nasıl gösterir **Postman**. Koleksiyon, Azure Media Services (AMS) API'lerini çağırmanın HTTP isteği gruplandırılmış tanımlarını içerir. Ortam dosyası koleksiyon tarafından kullanılan değişkenleri içerir.

Bu ortam ve koleksiyon Azure Media Services REST API'leri ile çeşitli görevlerin nasıl yerine getirileceğini gösteren makalelerinde kullanılır.

## <a name="prerequisites"></a>Önkoşullar

- Yükleme [Postman](https://www.getpostman.com/) AMS REST öğreticileri bazıları gösterilen REST API'leri yürütmek için REST istemci. 

    Kullanıyoruz **Postman** ancak herhangi bir REST aracı uygun olacaktır. Diğer seçenekleri şunlardır: **Visual Studio Code** REST eklentisi ile veya **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Ortamını yapılandırma 

1. AMS eğitimlerine kullanılan ortam değişkeni içeren bir .json dosyası oluşturun. Dosya adı (örneğin, **AzureMediaServices.postman_environment.json**). Dosyasını açın ve Postman ortamından tanımlar kodu yapıştırın [Bu kod listesi](postman-environment.md). 
2. Açık **Postman**.
3. Ekranın sağ tarafta seçin **Yönet ortamı** seçeneği.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-env.png)
4. Gelen **Yönet ortamı** iletişim kutusunda, tıklatın **alma**.
5. Göz atın ve seçim **AzureMediaServices.postman_environment.json** dosya.
6. **AzureMedia** ortam eklenir.
7. İletişim kutusunu kapatın.
8. Seçin **AzureMedia** ortamı.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Koleksiyonunu yapılandırma

1. İçeren bir .json dosyası oluşturma **Postman** Media Services'e bir dosyayı karşıya yüklemek için gereken tüm işlemleri içeren koleksiyon. Dosya adı (örneğin, **AzureMediaServicesOperations.postman_collection.json**). Dosyasını açın ve tanımlar kodu yapıştırın **Postman** koleksiyonundan [Bu kod listesi](postman-collection.md).
2. Tıklatın **alma** koleksiyon dosyasını içeri aktarmak için.
3. Seçin **AzureMediaServicesOperations.postman_collection.json** dosya.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [varlıklar karşıya](media-services-rest-upload-files.md) makalesi.  
