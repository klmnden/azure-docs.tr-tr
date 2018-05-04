---
title: Denetçiler Azure Machine Learning veri hazırlığı ile kullanılabilir desteklenen | Microsoft Docs
description: Bu belgede Azure Machine Learning veri hazırlığı kullanılabilir denetçiler tam bir listesi sağlanmaktadır
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 5d5797ede15be0779873f0a023433f0a915dd74a
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Denetçiler Azure Machine Learning veri hazırlık önizlemesi için desteklenen
Bu belgede, bu Önizleme'de kullanılabilen denetçiler kümesi özetlenmektedir.

## <a name="the-halo-effect"></a>Halo etkisi 
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
- Logaritmik ölçek kullan


### <a name="actions"></a>Eylemler
Bu denetim, tek veya çoklu seçim demet içerebilen demet filtreleme destekler. Daha önce açıklandığı gibi filtre uygulayın.

## <a name="value-counts"></a>Değer sayar
Bu denetim sıklığı tablosu şu anda seçili sütun değerleri gösterir. Varsayılan görüntü üst altı değerlerini ayarıdır. Ancak, herhangi bir sayıya sınırı değiştirebilirsiniz. Top yerine Alttan saymak için ekranı de ayarlayabilirsiniz. Bu denetleyici hale efekti destekler.

### <a name="options"></a>Seçenekler 
- Üst değer sayısı
- Azalan
- Null/hata değerlerini içerir
- Hale Göster
- Logaritmik ölçek kullan


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


## <a name="pattern-frequency"></a>Desen sıklığı 

Bu denetim içinde seçilen dize sütunu desenlerinin bir listesi gösterilir. Desenler bir normal ifade sözdizimini gibi kullanılarak gösterilir. Desenine bekleyerek bu desene tarafından temsil edilen değerler örnekleri gösterilir. Desenler ile birlikte yüzdesi cinsinden yaklaşık coverages da gösterilir.

![Desen denetçisi görüntüsü](media/data-prep-appendix4-supported-inspectors/PatternInspectorProductNumber.png)

### <a name="options"></a>Seçenekler
- Üst değer sayısı
- Azalan
- Hale Göster

### <a name="actions"></a>Eylemler
Bu denetleyici görüntülenen düzenlerini esas alarak filtreleme destekler. Tuşuna **Ctrl** anahtar ve doldurulmuş çubuğu düzeni Denetçisi'nde seçin. Ardından daha önce açıklandığı gibi filtre uygulayın. Kullanıcı acion sonucunda bir Gelişmiş Filtre adım eklenir. Bakın ve Gelişmiş Filtre adım Düzenle seçeneğini çağırarak oluşturulan Python kodu değiştirin.
