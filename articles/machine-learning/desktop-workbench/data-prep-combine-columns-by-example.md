---
title: Örnek Azure Machine Learning Workbench'i kullanarak dönüştürme sütunları Birleştir
description: Başvuru belgesini 'Birleştirmek sütunları örneğe göre' dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 621601ad3576aad13f2f71062ee2351cf1a394c8
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650856"
---
# <a name="combine-columns-by-example-transformation"></a>Örnek dönüştürme tarafından sütunları Birleştir
Bu dönüştürme birden fazla sütundaki değerleri birleştiren yeni bir sütun eklemesine izin verir. Kullanıcı, bir ayırıcı belirtin veya bu dönüştürme işlemini gerçekleştirmek için birleşik değerleri örnekleri sağlar. Kullanıcı örnekleri birleşimi sağladığında dönüşümü aynı tarafından işlenir **örnek tarafından** kullanılan altyapısı **sütunu örneğe göre türet** dönüştürün.

## <a name="how-to-perform-this-transformation"></a>Bu dönüşüm gerçekleştirme

Bu dönüştürme işlemini gerçekleştirmek için şu adımları izleyin:
1. Tek bir sütunda birleştirmek istediğiniz iki veya daha fazla sütun seçin. 
2. Seçin **birleştirmek sütunları örneğe göre** gelen **dönüştüren** menüsü. Ya da seçin ve seçili sütunların hiçbirinin başlığına sağ tıklayın **birleştirmek sütunları örneğe göre** bağlam menüsünden. Dönüştürme Düzenleyicisi açılır ve sağ en seçili olan sütunda yanındaki yeni bir sütun eklenir. Yeni bir sütun bir varsayılan ayırıcıyla ayrılmış birleşik değerleri içerir. Seçili sütunları sütun başlıkları, onay kutularını belirlenebilir. Eklenmesini ve kaldırılmasını sütun seçimi yapılabilir onay kutularını kullanarak.
3. Yeni oluşturulan sütundaki değeri güncelleştirebilirsiniz. Güncelleştirilmiş değeri, dönüştürmeyi öğrenmek için örnek olarak kullanılır.
4. Tıklayın **Tamam** dönüştürmeyi kabul etmek için.

### <a name="transform-editor-advanced-mode"></a>Düzenleyici dönüştürme: Gelişmiş mod

Gelişmiş mod sütunların birleştirilmesi için daha zengin bir deneyim sağlar. 

Seçme **ayırıcı** altında **birleştirmek sütunlara göre** kullanıcının dizelerde belirtmesine olanak tanır **ayırıcı** metin kutusu. Çıkış sekmesinden **ayırıcı** veri gird sonuçları önizlemesini görmek için metin kutusu. Tuşuna **Tamam** dönüşümü tamamlanamadı.

Seçme **örnekler** altında **birleştirmek sütunlara göre** birleşik değer örnekleri sağlamak kullanıcının sağlar. Örnek olarak bir satır yükseltmek için ızgaradaki satırların çift tıklayın. Beklenen çıktıyı karşı yükseltilen satır metin kutusuna yazın. Çıkış sekmesinden **ayırıcı** veri gird sonuçları önizlemesini görmek için metin kutusu. Tuşuna **Tamam** dönüşümü tamamlanamadı. 

Kullanıcı arasında geçiş yapmak **temel mod** ve **Gelişmiş mod** dönüştürme Düzenleyicisi bağlantıları tıklayarak.

### <a name="transform-editor-send-feedback"></a>Düzenleyici dönüştürme: geri bildirim gönder

Tıklayarak **geri bildirim gönder** bağlantı açar **geri bildirim** örnekler kullanıcıyla önceden doldurulmuş açıklamaları kutusu iletişim sağlanan. Kullanıcı yorumları kutunun içeriğini gözden geçirmeniz ve sorunu anlamanıza yardımcı olmak için daha fazla ayrıntı sağlanır. Kullanıcı verilerini Microsoft ile paylaşmak istemediği, kullanıcı önceden doldurulmuş bir örnek veri tıklamadan önce silmelisiniz **geri bildirim gönder** düğmesi. 

### <a name="editing-existing-transformation"></a>Var olan dönüştürme düzenleme

Varolan bir kullanıcı düzenleyebilir **sütun örneği ile birleştirme** Dönüştür seçip **Düzenle** dönüştürme adımı seçeneği. Tıklayarak **Düzenle** dönüştürme Düzenleyicisi'nde açılır **temel modu**. Kullanıcının girebileceği **Gelişmiş mod** üst bilgisindeki bağlantısına tıklayarak. Dönüştürme, oluşturma sırasında sağlanan tüm örnekleri var. gösterilmektedir.

## <a name="example-using-separators"></a>Örnek ayırıcılar kullanma

Birleştirmek için bu örnekteki ayırıcı olarak virgül ardından bir boşluk kullanılan *Sokak*, *Şehir*, *durumu*, ve *ZIP* sütunları.

|Cadde|Şehir|Durum|ZIP|Sütun|
|:----|:----|:----|:----|:----|
|16011 N.E. 36th yolu|REDMOND|WA|98052|16011 N.E. 36th way, REDMOND, WA, 98052|
|16021 N.E. 36th yolu|REDMOND|WA|98052|16021 N.E. 36th way, REDMOND, WA, 98052|
|16031 N.E. 36th yolu|REDMOND|WA|98052|16031 N.E. 36th way, REDMOND, WA, 98052|
|16041 N.E. 36th yolu|REDMOND|WA|98052|16041 N.E. 36th way, REDMOND, WA, 98052|
|16051 N.E. 36th yolu|REDMOND|WA|98052|16051 N.E. 36th way, REDMOND, WA, 98052|
|16061 N.E. 36th yolu|REDMOND|WA|98052|16061 N.E. 36th way, REDMOND, WA, 98052|
|3460 157th. Cadde No: NE|REDMOND|WA|98052|3460 157th. Cadde No: NE, REDMOND, WA, 98052|
|3350 157th Ave N.E.|REDMOND|WA|98052|3350 157th Ave N.E., REDMOND, WA, 98052|
|3240 157th. Cadde No: N.E.|REDMOND|WA|98052|3240 157th. Cadde No: N.E., REDMOND, WA, 98052|

## <a name="example-using-by-example"></a>Örnek tarafından kullanma örnek

Değer **kalın** örnek olarak sağlandı.

|Tarih|Ay|Yıl|Saat|Dakika|Saniye|Birleştirilmiş sütun|
|:----|:----|:----|:----|:----|:----|:----|
|13|Eki|2016|15|01|23|**13 Ekim 2016 15:01:23 PDT**|
|16|Eki|2016|16|22|33|16 Ekim 2016 15:01:33 PDT|
|17|Eki|2016|12|43|12|17 Ekim 2016 15:01:12 PDT|
|12|Kas|2016|14|22|44|12 Kasım 2016 15:01:44 PDT|
|23|Kas|2016|01|52|45|23 Kasım 2016 15:01:45 PDT|
|16|Oca|2017|22|34|56|16 Ocak 2016 15:01:56 PDT|
|23|Mar|2017|01|55|25|23 Mart 2016 15:01:25 PDT|
|16|Nis|2017|11|34|36|16 Apr 2016 15:01:36 PDT|

