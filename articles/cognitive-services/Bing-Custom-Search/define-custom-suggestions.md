---
title: Öneriler - Bing özel arama özel otomatik öneri tanımlayın
titlesuffix: Azure Cognitive Services
description: Özel otomatik öneri özel öneriler yapılandırılması açıklanmaktadır
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: bbad72b41a177bdbafd6cf98bfd2025190d98b16
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128966"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Özel otomatik öneri deneyiminizi yapılandırın

Özel otomatik öneri özelliği, arama deneyiminizle ilgili önerilen arama sorgu dizelerinin listesini döndürür. Önerilen sorgu dizeleri arama kutusuna kullanıcının sağladığı bir kısmi sorgu dizesi temel alır. Listenin en fazla 10 öneriler içerir. 

Yalnızca özel öneriler döndürmek mi, yoksa Bing önerileri de içerecek şekilde belirtin. Bing önerileri eklerseniz, özel öneriler Bing öneriler önce görünür. Yeterli ilgili öneriler sağlarsanız, öneriler, döndürülen liste Bing önerileri içermez mümkündür. Bing her zaman özel arama örneğinizin bağlamında önerilerdir. 

Örneğiniz için arama Sorgu önerileri yapılandırmak için tıklayın **otomatik öneri** sekmesi.  

> [!NOTE]
> Bu özelliği kullanmak için uygun düzeyde özel arama abone gerekir (bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)).

Bu önerileri (API veya UI barındırılan) Hizmeti uç yansıtılması 24 saate kadar sürebilir.

## <a name="enable-bing-suggestions"></a>Bing önerilerini etkinleştirmek

Bing önerilerini etkinleştirmek için geçiş **Bing otomatik öneri** kaydırıcısını açık konuma. Kaydırıcı mavi haline gelir.

## <a name="add-your-own-suggestions"></a>Önerilerinizi Ekle

Kendi sorgu dizesi önerileri eklemek için bunları altındaki listeye ekleyin **öneriler kullanıcı tanımlı**. Bir öneri listesinde ekledikten sonra enter tuşuna basın veya **+** simgesi. Herhangi bir dilde öneri belirtebilirsiniz. En fazla 5000 sorgu dizesi önerileri ekleyebilirsiniz.

## <a name="upload-suggestions"></a>Öneriler karşıya yükleme

Bir seçenek olarak, bir dosyadan öneriler listesi karşıya yükleyebilirsiniz. Dosya her satırda bir arama sorgu dizesi içermelidir. Dosyayı karşıya yüklemek için karşıya yükleme simgesine tıklayın ve karşıya yüklenecek dosyayı seçin. Hizmet önerileri dosyasından ayıklar ve bunları listeye ekler.

## <a name="remove-suggestions"></a>Öneriler Kaldır

Bir sorgu dizesi öneriyi kaldırmak için kaldırmak istediğiniz öneri yanındaki Kaldır simgesine tıklayın.

## <a name="block-suggestions"></a>Önerilerini engelle

Bing önerileri içeriyorsa, döndürülecek Bing istemediğiniz arama sorgu dizelerinin listesini ekleyebilirsiniz. Engellenen sorgu dizeleri eklemek için tıklatın **önerileri göster engellenen**. Sorgu dizesi listeye ekleyin ve enter tuşuna basın veya **+** simgesi. En fazla 50 engellenen sorgu dizeleri ekleyebilirsiniz.



[!INCLUDE [publish or revert](./includes/publish-revert.md)]

>[!NOTE]  
>Bu özel otomatik öneri yapılandırma değişikliklerinin etkinleşmesi 24 saate kadar sürebilir.


## <a name="enabling-autosuggest-in-hosted-ui"></a>Otomatik öneri barındırılan kullanıcı Arabiriminde etkinleştirme

Barındırılan kullanıcı Arabirimi için sorgu dizesi önerilerini etkinleştirmek için tıklayın **barındırılan UI**. Ekranı aşağı kaydırarak **ek yapılandırma** bölümü. Altında **Web araması**seçin **üzerinde** için **etkinleştir otomatik öneri**. Otomatik öneri etkinleştirmek için bir arama kutusu içeren bir düzen seçmelisiniz.


## <a name="calling-the-autosuggest-api"></a>Çağırma otomatik öneri API'si

Bing özel arama API'si kullanarak önerilen sorgu dizelerini almak için gönderin bir `GET` aşağıdaki uç noktayı isteği.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

Yanıta bir listesini içeren `SearchAction` önerilen sorgu dizesi içeren bir nesne.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Her öneri içeren bir `displayText` ve `query` alan. `displayText` Alan, arama kutusuna kişinin açılan listeyi doldurmak için kullandığınız önerilen sorgu dizesi içerir.

Kullanıcı açılır listeden bir önerilen sorgu dizesi seçerse, sorgu dizesinde kullanmak `query` alan çağrılırken [Bing özel arama API'si](overview.md).


## <a name="next-steps"></a>Sonraki adımlar

- [Özel öneriler alın](./get-custom-suggestions.md)
- [Özel Örneğinize arama](./search-your-custom-view.md)
- [Yapılandırma ve barındırılan özel kullanıcı Arabirimi kullanma](./hosted-ui.md)
