---
title: Azure Haritalar ile arama sonuçlarını gösterme | Microsoft Docs
description: Bir arama gerçekleştirme isteği ile Azure haritalar sonra sonuçları bir Javascript harita üzerinde görüntüleyebilirsiniz
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: be01c9d96386804b8bc074d81041104cbf592df6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59271603"
---
# <a name="show-search-results-on-the-map"></a>Harita üzerinde arama sonuçlarını göster

Bu makalede ilgi konumu aramak ve arama sonuçlarını haritada gösterme gösterilmektedir.

İlgilendiğiniz bir konumu aramak için iki yolu vardır. Arama istekte bulunmak için bir hizmet modülü kullanmak bir yoludur. Bir arama isteğine yapmak için diğer yol ise [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) aracılığıyla [Fetch API'sini](https://fetch.spec.whatwg.org/). Her iki yönde aşağıda ele alınmıştır.

## <a name="make-a-search-request-via-service-module"></a>Bir arama hizmeti modülü aracılığıyla istekte

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada (hizmet Modülü) göster' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show arama sonuçları bir haritada (hizmet Modülü)</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğunu oluşturur bir `SubscriptionKeyCredentialPolicy` abonelik anahtarını Azure haritalar için HTTP isteklerinde kimlik doğrulaması için. Ardından `atlas.service.MapsURL.newPipeline()` alır `SubscriptionKeyCredential` ilke ve oluşturan bir [işlem hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) örneği. `searchURL` Azure haritalar için URL'yi temsil [arama](https://docs.microsoft.com/rest/api/maps/search) operations.

Üçüncü kod bloğunun bir veri kaynağı nesnesi kullanılarak oluşturur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve arama sonuçları ekleyin. A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak.  Bir sembol katmanı sonra oluşturulur ve ardından eşlemesine eklenen sembol katmanı, veri kaynağı eklenir.

Dördüncü kod bloğu kullanır [SearchFuzzy](/javascript/api/azure-maps-rest/atlas.service.models.searchgetsearchfuzzyoptionalparams) yönteminde [hizmeti Modülü](https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js). Serbest biçimli metin arama yoluyla gerçekleştirmenize olanak tanır [alma belirsiz arama rest API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) için ilgi noktası arama. Belirsiz arama API'si, GET, herhangi bir birleşimini belirsiz girdileri başa çıkabilir. Kullanarak bir GeoJSON özellik koleksiyonundan yanıt ayıklanır `geojson.getFeatures()` yöntemi ve sembol katmanı aracılığıyla harita üzerinde işlenen verileri otomatik olarak sonuçlanan veri kaynağı eklenir.

Haritanın kullanarak son blok kod eşlemesi için kamera sınırları ayarlar [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliği.

Veri kaynağı ve sembol katman ve kamera sınırları oluşturulur ve eşleme kümesinde hazır arama isteği [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.


## <a name="make-a-search-request-via-fetch-api"></a>Bir arama isteğinde Fetch API aracılığıyla

<iframe height='500' scrolling='no' title='Arama sonuçları bir haritada gösterme' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Kalem bkz <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Show arama sonuçları bir haritada</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğunu bir harita nesnesi oluşturur ve bir abonelik anahtarı kullanmak için kimlik doğrulama mekanizması ayarlar. Gördüğünüz [bir harita oluşturmak](./map-create.md) yönergeler için.

İkinci kod bloğu için arama istekte bulunmak için bir URL oluşturur. Ayrıca, sınırlar ve arama sonuçları için PIN depolamak için iki dizi oluşturur.

Üçüncü kod bloğunu kullanır [Fetch API'sini](https://fetch.spec.whatwg.org/) isteğin yapılacağı [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ilgi noktalarını aramak için. Belirsiz arama API'si benzer girişler herhangi bir birleşimini başa çıkabilir. Ardından işleme arama yanıt ayrıştırır ve sonuç PIN'ler searchPins diziye ekler.

Dördüncü kod bloğunun bir veri kaynağı nesnesi kullanılarak oluşturur [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı ve arama sonuçları ekleyin. A [sembol katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) sarmalanmış noktası tabanlı veri işleme için metin veya simge kullanan [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) harita üzerinde simgeler olarak. Bir sembol katmanı sonra oluşturulur ve ardından eşlemesine eklenen sembol katmanı, veri kaynağı eklenir.

Son blok kod oluşturur bir [sınırlayıcı kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) Sonuçlar dizisi kullanarak nesne ve sonra haritanın kullanarak kamera sınırları eşlemesi için ayarlar [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-). Ardından, sonuç PIN'ler işler.

Arama isteği, veri kaynağı ve sembol katman ve kamera sınırları haritanın içinde ayarlanan [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) harita tamamen yüklendikten sonra sonuçları görüntülendiğinden emin olmak için.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin **belirsiz arama**:

> [!div class="nextstepaction"]
> [Azure haritalar belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Eşleme](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Bir koordinattan bilgi alma](./map-get-information-from-coordinate.md)
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](./map-route.md)
