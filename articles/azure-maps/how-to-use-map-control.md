---
title: Azure haritalar harita denetimini kullanma | Microsoft Docs
description: Azure haritalar harita denetimi istemci tarafı Javascript kitaplığı kullanmayı öğrenin.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 850f9b28c112c11fd98a8abc81a1811cd26d81cc
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166041"
---
# <a name="use-the-azure-maps-map-control"></a>Azure haritalar harita denetimini kullanma

Harita denetimi istemci tarafı Javascript kitaplığı, haritalar ve katıştırılmış Azure haritalar işlevselliği web veya mobil uygulama oluşturmak sağlar.

## <a name="create-a-new-map-in-a-web-page"></a>Bir web sayfasında yeni bir eşleme oluşturma

Harita denetimi istemci tarafı Javascript kitaplığını kullanarak bir web sayfasında bir harita ekleyebilir.

1. Yeni bir dosya oluşturun ve adlandırın **MapSearch.html**.

2. Azure haritalar stil sayfası ve komut dosyası kaynak başvuruları `<head>` öğesi:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```

3. Tarayıcınızda yeni bir harita işlemek için ekleme bir **#map** başvuru `<style>` öğesi:

    ```html
    <style>
        #map {
            width: 100%;
            height: 100%;
        }
    </style>
    ```

4. Harita denetimi başlatmak için html gövdesinde yeni bir bölüm tanımlayın ve bir betik oluşturabilir. Betikte, kendi Azure haritalar hesabı anahtarını kullanın. Bir hesap oluşturun veya, anahtar, bkz: bulmak gerekiyorsa [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md). **SetLanguage** yöntemi harita etiketlerini ve denetimler için kullanılacak dili belirtir. Desteklenen diller hakkında daha fazla bilgi için bkz. [desteklenen diller](https://docs.microsoft.com/azure/azure-maps/supported-languages).

    ```html
    <div id="map">
        <script>
            atlas.setSubscriptionKey("<_your account key_>");
            atlas.setLanguage("en");
            var map = new atlas.Map("map", {
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```

5. Web tarayıcınızda dosyasını açın ve işlenmiş harita görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Tam bir örnek kullanarak bir harita oluşturmayı öğrenin:

> [!div class="nextstepaction"]
> [Bir eşleme oluşturma](map-create.md)

Bir eşlem stili hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Harita stil seçin](choose-map-style.md)
