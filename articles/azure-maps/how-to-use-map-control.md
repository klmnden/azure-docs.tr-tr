---
title: Azure haritalar harita denetimini kullanma | Microsoft Docs
description: Azure haritalar harita denetimi istemci tarafı Javascript kitaplığı kullanmayı öğrenin.
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 5b8703c218790549a0cf5a319345132a0eca66ce
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025198"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Azure haritalar harita Denetimi'ni kullanma
Harita denetimi istemci tarafı Javascript kitaplığı, haritalar ve katıştırılmış Azure haritalar işlevselliği web veya mobil uygulama oluşturmak sağlar. 

## <a name="create-a-new-map-in-a-web-page"></a>Bir web sayfasında yeni bir eşleme oluşturma

Harita denetimi istemci tarafı Javascript kitaplığını kullanarak bir web sayfasında bir harita ekleyebilir.

1. Yeni bir dosya oluşturun ve MapSearch.html adlandırın.

2. Azure haritalar stil sayfası ve komut dosyası kaynak başvuruları `<head>` öğesi:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```
    
3. Tarayıcınızda yeni bir eşleme oluşturmak için Ekle bir **#map** başvuru `<style>` öğesi.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Harita denetimi başlatmak için html gövdesinde yeni bir bölüm tanımlayın ve bir betik oluşturabilir. Betikte, kendi Azure haritalar hesabı anahtarını kullanın. Bir hesap oluşturun veya, anahtar, bkz: bulmak gerekiyorsa [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md)

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

Bu makalede Azure haritalar anahtarınız ile temel bir harita oluşturmak nasıl oluşturulacağını gösterir. Eşlenir eklemek daha fazla kod örnekleri için aşağıdaki makalelere bakın: 

* [Bir eşleme oluşturma](map-create.md)
* [Harita stil seçin](choose-map-style.md)
