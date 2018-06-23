---
title: Azure eşlemeleri harita denetiminin nasıl kullanılacağını | Microsoft Docs
description: Azure eşlemeleri harita denetiminin istemci tarafı Javascript kitaplığı kullanmayı öğrenin.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 0b0549e7a7ea6389597e483f239a75743a96fef1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317973"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Azure eşlemeleri harita denetiminin nasıl kullanılacağını
Harita denetiminin istemci tarafı Javascript kitaplığı eşlemeleri ve katıştırılmış Azure eşlemeleri işlevselliği, web veya mobil uygulamanızı işlemek sağlar. 

## <a name="create-a-new-map-in-a-web-page"></a>Bir web sayfasında yeni bir eşleme oluşturma

Harita denetiminin istemci tarafı Javascript kitaplığını kullanarak bir web sayfasında bir harita eklenebilir.

1. Yeni bir dosya oluşturun ve MapSearch.html adlandırın.

2. Azure eşlemeleri stil ve komut dosyası kaynağı başvurular ekleyin `<head>` dosyasının öğesinin:

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
    
4. Harita denetiminin başlatmak için bir komut dosyası oluşturabilir ve yeni bir bölüm html gövdesinde tanımlayın. Kendi Azure eşlemeleri hesap anahtarını komut dosyasını kullanın. Bir hesap oluşturun veya, anahtar, bkz: Bul gerekiyorsa [Azure eşlemeleri hesabınızın ve anahtarlarını yönetme](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Web tarayıcınızda dosyasını açın ve işlenmiş harita görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure eşlemeleri anahtarınızla temel bir harita oluşturmak nasıl oluşturulacağını gösterir. Eşlemeleri eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 

* [Bir eşleme oluşturma](map-create.md)
* [Bir PIN ekleme](map-add-pin.md)
