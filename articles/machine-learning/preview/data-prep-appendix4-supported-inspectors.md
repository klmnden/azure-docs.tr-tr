---
title: "Denetçiler Azure Machine Learning veri hazırlığı ile kullanılabilir desteklenen | Microsoft Docs"
description: "Bu belgede Azure Machine Learning veri hazırlığı kullanılabilir denetçiler tam bir listesi sağlanmaktadır"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 35d7c04f245e93d8cc795dca7c01c2bab5a14eb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Denetçiler Azure Machine Learning veri hazırlık önizlemesi için desteklenen
Bu belgede, bu Önizleme'de kullanılabilen denetçiler kümesi özetlenmektedir.

## <a name="the-halo-effect"></a>Hale efekti 
Bazı denetçiler hale efekti destekler. Bu etkiyi hemen değişikliği görsel olarak bir dönüşüm göstermek için iki farklı renk kullanır. Gri son dönüştürme önce değerini temsil eder ve mavi geçerli değeri gösterir. Bu etkiyi etkin ve seçenekleri devre dışı.

## <a name="graphical-filtering"></a>Grafik filtreleme 
Denetçiler bazıları düzenleyici olarak Inspector'ı kullanarak veri filtreleme destekler. Düzenleyici olarak Inspector'ı kullanarak, grafik öğeleri seçerek ve araç denetçisi pencerenin sağ üst kısmında veya seçilen değerleri uzaklaştırma filtre kullanmayı içerir. 

## <a name="column-statistics"></a>Sütun istatistikleri
Sayısal sütunlar için bu denetçisi farklı istatistik sütun hakkında çeşitli sağlar. İstatistikler aşağıdaki ölçüleri içerir: 
- Minimum
- Alt DÖRTTEBİRLİK
- ORTANCA
- Üst DÖRTTEBİRLİK
- Maksimum
- Ortalama
- Standart sapma


### <a name="options"></a>Seçenekler 
- None

## <a name="histogram"></a>Çubuk grafik 
Hesaplar ve tek bir sayısal sütunun bir histogram görüntüler. Varsayılan aralık sayısı, Scott'ın kuralı kullanılarak hesaplanır. Bununla birlikte, kural seçenekleri ile geçersiz kılınabilir.

Bu denetleyici hale efekti destekler.


### <a name="options"></a>Seçenekler
- En az sayıda demete (hatta zaman varsayılan bucketing denetlenir geçerlidir)
- Varsayılan aralık sayısı (Scott'ın kuralı) 
- Hale Göster
- Çekirdek yoğunluğu çizim katmana (Gauss çekirdek) 


### <a name="actions"></a>Eylemler
Bu denetim, tek veya çoklu seçim demet içerebilen demet filtreleme destekler. Daha önce açıklandığı gibi filtre uygulayın.

## <a name="value-counts"></a>Değer sayar
Bu denetim sıklığı tablosu şu anda seçili sütun değerleri gösterir. Varsayılan görüntü üst altı değerlerini ayarıdır. Ancak, herhangi bir sayıya sınırı değiştirebilirsiniz. Top yerine Alttan saymak için ekranı de ayarlayabilirsiniz. Bu denetleyici hale efekti destekler.

### <a name="options"></a>Seçenekler 
- Üst değer sayısı
- Azalan
- Null/hata değerlerini içerir
- Hale Göster


### <a name="actions"></a>Eylemler 
Bu denetim, tek veya çoklu seçim çubukları içerebilen çubukları filtreleme destekler. Daha önce açıklandığı gibi filtre uygulayın.

## <a name="box-plot"></a>Kutu Çizimi 
Bir kutu yatay çizgi çizimi sayısal bir sütun.

### <a name="options"></a>Seçenekler 
- Sütuna göre Gruplandır

## <a name="scatter-plot"></a>Çizim dağılım
Dağılım çizim iki sayısal sütunlar için. Performansı artırmak için aşağı örneklenen verilerdir. Örnek boyut seçenekleri geçersiz kılınabilir.

### <a name="options"></a>Seçenekler  
- X ekseni sütun
- Y ekseni sütun
- Örnek boyutu
- Sütuna göre Gruplandır


## <a name="time-series"></a>Zaman serisi
Çizgi grafiği eksenindeki zaman tanıma sahip.

### <a name="options"></a>Seçenekler
- Tarih sütunu
- Sayısal sütun
- Örnek boyutu


### <a name="actions"></a>Eylemler
Bu denetleyici grafikte bir aralık seçmek üzere tıklatın ve sürükleyin select yöntemi aracılığıyla filtreleme destekler. Seçimi tamamladıktan sonra daha önce açıklandığı gibi filtre uygulayın.


## <a name="map"></a>Eşleme 
Enlem ve boylam belirtilen varsayarsak, çizilir noktaları ile eşleme. Enlem önce seçilmelidir.

### <a name="options"></a>Seçenekler
- Enlem sütun
- Boylam sütun
- Üzerinde kümeleme
- Sütuna göre Gruplandır


### <a name="actions"></a>Eylemler
Bu denetim noktası seçimini harita üzerinde aracılığıyla filtreleme destekler. Tuşuna **Ctrl** anahtar ve ardından bir kare noktaları etrafında oluşturmak üzere fareyle sürükleyin. Ardından daha önce açıklandığı gibi filtre uygulayın.

Harita tuşlarına basarak yalnızca olası noktalarını göstermek için hızlı bir şekilde boyutlandırabilirsiniz **E** harita sol tarafındaki.
