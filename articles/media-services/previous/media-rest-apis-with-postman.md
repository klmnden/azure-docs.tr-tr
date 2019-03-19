---
title: Azure Media Services REST API çağrıları için Postman'ı yapılandırma
description: Media Services REST API çağrıları için Postman'ı yapılandırmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: da52827107fcf1672047644d2d5ac5ed2e6beed5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58007753"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Media Services REST API çağrıları için Postman'ı yapılandırma  

Bu öğreticide nasıl yapılandırılacağı gösterilmektedir **Postman** böylece Azure Media Services (AMS) REST API'leri çağırmak için kullanılabilir. Bu öğreticide, ortam ve koleksiyon dosyasına aktarmak gösterilmektedir **Postman**. Koleksiyon, Azure Media Services (AMS) REST API'lerini çağırma HTTP isteklerinin gruplandırılmış tanımları içerir. Koleksiyon tarafından kullanılan değişkenleri ortam dosyası içerir.

Bu ortam ve koleksiyonu gösteren Azure Media Services REST API'leri ile çeşitli görevleri elde etmek nasıl makaleleri kullanılır.

## <a name="prerequisites"></a>Önkoşullar

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer alternatifler: **Visual Studio Code** REST eklentisiyle veya **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. AMS öğreticilerinde kullanılan ortam değişkenlerini içeren bir .json dosyası oluşturun. Dosya adı (örneğin, **AzureMediaServices.postman_environment.json**). Dosyasını açın ve Postman ortamından tanımlar kodu yapıştırın [Bu kod listesi](postman-environment.md). 
2. **Postman**'ı açın.
3. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
5. Göz atın ve seçim **AzureMediaServices.postman_environment.json** dosya.
6. **AzureMedia** ortam eklenir.
7. İletişim kutusunu kapatın.
8. Seçin **AzureMedia** ortamı.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. İçeren bir .json dosyası oluşturma **Postman** Media Services için bir dosyayı karşıya yüklemek için gereken tüm işlemleri içeren koleksiyon. Dosya adı (örneğin, **AzureMediaServicesOperations.postman_collection.json**). Dosyasını açın ve tanımlar kodu yapıştırın **Postman** koleksiyonundan [Bu kod listesi](postman-collection.md).
2. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
3. Seçin **AzureMediaServicesOperations.postman_collection.json** dosya.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [varlıklar karşıya](media-services-rest-upload-files.md) makalesi.  
