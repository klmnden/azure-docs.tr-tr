---
title: Desteklenen denetçiler Azure Machine Learning veri hazırlama ile kullanılabilir | Microsoft Docs
description: Bu belge, Azure Machine Learning veri hazırlama için kullanılabilir denetçiler tam listesi sağlar
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: ef5f6f3dc7ae0c555b2afe000b54c443313800f1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651046"
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Azure Machine Learning veri hazırlama Önizleme denetçiler desteklenir
Bu belge, bu Önizleme sürümünde kullanılabilir olan denetçiler kümesini açıklar.

## <a name="the-halo-effect"></a>Halo etkisi 
Bazı denetçiler halo etkisi destekler. Bu etkiyi hemen dönüşüm dosyasından görsel olarak göstermek için iki farklı renkler kullanır. Gri önce en son transform değerini temsil eder ve mavi geçerli değeri gösterir. Bu etkiyi etkinleştirilmeli ve seçeneklerinde devre dışı.

## <a name="graphical-filtering"></a>Grafik filtreleme 
Denetçiler bazıları bir düzenleyici olarak Denetçisi'ni kullanarak verileri filtrelemeyi destekler. Bir düzenleyici olarak denetçisini kullanma, grafik öğeleri seçme ve araç denetçisi pencerenin sağ üst kısmında kullanarak içe veya dışa seçilen değerleri filtreleme içerir. 

## <a name="column-statistics"></a>Sütun istatistikleri
Sayısal sütunları için bu denetim hakkında sütunu farklı istatistikleri çeşitli sağlar. İstatistikler, aşağıdaki ölçüleri içerir: 
- Minimum
- Daha düşük dörtte
- ORTANCA
- Üst dörtte
- Maksimum
- Ortalama
- Standart sapma


### <a name="options"></a>Seçenekler 
- None

## <a name="histogram"></a>Çubuk grafik 
Hesaplar ve tek bir sayısal sütun bir histogram gösterir. Varsayılan demet sayısını Scott'ın kuralı kullanılarak hesaplanır. Ancak, kural aracılığıyla seçenekleri geçersiz kılınabilir.

Bu denetim halo etkisi destekler.


### <a name="options"></a>Seçenekler
- En düşük demet sayısı (hatta zaman varsayılan benzeyebilir denetlenir geçerlidir)
- Varsayılan sayıda demete (Scott'ın kuralı) 
- Halo Göster
- Çekirdek yoğunluğu çizim katmana (Gauss çekirdek) 
- Logaritmik ölçek kullan


### <a name="actions"></a>Eylemler
Bu denetim, tek veya çoklu seçim demet içeren demetler filtrelemeyi destekler. Daha önce açıklandığı gibi filtreler uygulayın.

## <a name="value-counts"></a>Değer sayıları
Bu denetim sıklığı tablosu şu anda seçili sütun için değerler sunar. İlk altı değerleri için varsayılan görüntüdür. Ancak, herhangi bir sayı sınırı değiştirebilirsiniz. Ekranın en üstünde değil, Alttan saymak için de ayarlayabilirsiniz. Bu denetim halo etkisi destekler.

### <a name="options"></a>Seçenekler 
- Üst değer sayısı
- Azalan
- Null/hata değerlerini içerir
- Halo Göster
- Logaritmik ölçek kullan


### <a name="actions"></a>Eylemler 
Bu denetim, tek veya çoklu seçim çubukları içerebilen çubukları filtrelemeyi destekler. Daha önce açıklandığı gibi filtreler uygulayın.

## <a name="box-plot"></a>Kutu Çizimi 
Bir kutu yatay çizgi çizimi sayısal bir sütun.

### <a name="options"></a>Seçenekler 
- Sütuna göre Gruplandır

## <a name="scatter-plot"></a>Dağılım
İki sayısal bir sütun için bir dağılım grafiğinde noktalara. Performansla ilgili nedenlerden dolayı alt örneklenen verilerdir. Örnek boyutu seçenekleri geçersiz kılınabilir.

### <a name="options"></a>Seçenekler  
- X ekseni sütun
- Y ekseni sütun
- Örnek boyutu
- Sütuna göre Gruplandır


## <a name="time-series"></a>Zaman serisi
X eksenindeki zaman tanıma ile çizgi grafiği.

### <a name="options"></a>Seçenekler
- Tarih sütunu
- Sayısal bir sütun
- Örnek boyutu


### <a name="actions"></a>Eylemler
Bu denetim, grafikteki bir aralık seçmek için tıklayın ve sürükleyin select yöntemi aracılığıyla filtrelemeyi destekler. Seçimi tamamladıktan sonra daha önce açıklandığı gibi filtreler uygulayın.


## <a name="map"></a>Eşleme 
Enlem ve boylamdan belirtilen varsayarak, çizilir noktalarını sahip bir eşleme. Enlem ilk seçilmelidir.

### <a name="options"></a>Seçenekler
- Enlem sütun
- Boylam sütun
- Üzerinde kümeleme
- Sütuna göre Gruplandır


### <a name="actions"></a>Eylemler
Bu denetim noktası seçimi harita üzerinde aracılığıyla filtrelemeyi destekler. Tuşuna **Ctrl** anahtar, tıklayın ve fareyle noktaları etrafında bir kare oluşturmak için sürükleyin. Ardından, daha önce açıklandığı gibi filtreler uygulayın.

Harita tuşlarına basarak yalnızca olası noktalarını göstermek için hızlı bir şekilde boyutlandırabilirsiniz **E** harita sol tarafındaki.


## <a name="pattern-frequency"></a>Desen sıklığı 

Bu denetim içinde seçilen dize sütunu desenlerinin bir listesi gösterir. Desenler sözdizimi gibi normal bir ifade kullanılarak temsil edilir. Desen vurgulama, bu deseni tarafından gösterilen değerleri örneklerini gösterir. Desenleri yanı sıra, yaklaşık coverages yüzde olarak da gösterilir.

![Desen denetçisinin görüntüsü](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Seçenekler
- Üst değer sayısı
- Azalan
- Halo Göster

### <a name="actions"></a>Eylemler
Bu denetim, görüntülenen düzenlerini esas alarak filtrelemeyi destekler. Tuşuna **Ctrl** anahtar ve desen denetçi'deki doldurulmuş çubuğu seçin. Ardından, daha önce açıklandığı gibi filtreler uygulayın. Gelişmiş Filtre adım kullanıcı acion sonucu olarak eklenir. Görebilir ve Gelişmiş Filtre adımı Düzenle seçeneğini çağırarak oluşturulan Python kodu değiştirin.
