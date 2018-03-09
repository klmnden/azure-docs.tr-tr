---
title: "Azure konum tabanlı Hizmetleri harita denetiminin nasıl kullanılacağını | Microsoft Docs"
description: "Azure konum tabanlı Hizmetleri harita denetiminin istemci tarafı Javascript kitaplığı kullanmayı öğrenin."
services: location-based-services
keywords: "SEO uzmanınıza danışmadan anahtar sözcük eklemeyin veya anahtar sözcükleri düzenlemeyin."
author: kgremban
ms.author: kgremban
ms.date: 11/22/2017
ms.topic: article
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 9843387a9e4f3b5751bafceb48d6fc949dff7e23
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Azure konum tabanlı Hizmetleri harita denetiminin kullanma
Harita denetiminin istemci tarafı Javascript kitaplığı eşlemeleri ve katıştırılmış Azure konum Hizmetleri temel işlevselliği, web veya mobil uygulamanızı işlemek sağlar. 

## <a name="prerequisites"></a>Önkoşullar
Bir Azure konum tabanlı hizmetleri hesabı ve anahtarı. Bir hesap oluşturma ve bir anahtar alma hakkında daha fazla bilgi için bkz: [Azure konum tabanlı hizmetleri hesabı ve anahtarlarını yönetme](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Harita denetimi API kullanarak bir web sayfasında yeni bir eşleme oluşturma
Harita denetiminin istemci tarafı Javascript kitaplığını kullanarak bir web sayfasında bir harita eklenebilir.

1. Yeni bir dosya oluşturun ve MapSearch.html adlandırın.

2. Azure konum tabanlı Hizmetleri stil ve komut dosyası kaynağı başvurular ekleyin `<head>` dosyasının öğesinin:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Tarayıcınızda yeni bir harita oluşturmak için Ekle bir **#map** başvuru `<style>` öğesi.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Harita denetiminin başlatmak için bir komut dosyası oluşturabilir ve yeni bir bölüm html gövdesinde tanımlayın. Kendi Azure konum tabanlı Hizmetleri hesap anahtarını komut dosyasını kullanın. 

    ```html
    <div id="map">
        <script>
            var LBSAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": LBSAccountKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Web tarayıcınızda dosyasını açın ve işlenmiş harita görüntüleyin.