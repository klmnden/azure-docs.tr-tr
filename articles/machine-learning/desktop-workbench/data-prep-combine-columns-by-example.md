---
title: Örnek Azure Machine Learning çalışma ekranı kullanarak dönüştürme tarafından sütunu birleştirme
description: Başvuru belge 'Örneğe göre sütunları birleştirmek' dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: ecd2232dcf77715fdccd4e5518674500231f67ca
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830287"
---
# <a name="combine-columns-by-example-transformation"></a>Örnek dönüştürme tarafından sütunu birleştirme
Bu dönüşümü yeni bir sütun birden çok sütun değerlerinden birleştirerek eklemesine izin verir. Kullanıcı bir ayırıcı belirtin veya bu dönüştürme gerçekleştirmek için birleştirilmiş değer örnekleri sağlayın. Kullanıcı örnekleri birleşimi sağladığında, dönüşüm aynı tarafından işlenir **örnek tarafından** kullanılan altyapısı **türetilen sütun örneğe göre** Dönüştür.

## <a name="how-to-perform-this-transformation"></a>Bu dönüştürme gerçekleştirme

Bu dönüştürme gerçekleştirmek için şu adımları izleyin:
1. Tek bir sütunda birleştirmek istediğiniz iki veya daha fazla sütun seçin. 
2. Seçin **birleştirmek sütunları örneğe göre** gelen **dönüştüren** menüsü. Veya herhangi seçin ve seçili sütun başlığına sağ tıklayın **birleştirmek sütunları örneğe göre** ve bağlam menüsünden. Dönüştürme Düzenleyicisi açılır ve yeni bir sütun yanındaki sağ en seçilen sütuna eklenir. Yeni bir sütun tarafından varsayılan ayırıcıyı ayrılmış birleşik değerlerini içerir. Seçili sütunları sütun başlıklarını adresindeki onay kutularını tanımlanabilir. Ekleme ve kaldırma seçimden sütunların yapılabilir onay kutularını kullanarak.
3. Yeni oluşturulan sütunundaki değeri güncelleştirebilirsiniz. Güncelleştirilmiş değere örnek olarak dönüştürme öğrenmek için kullanılır.
4. Tıklatın **Tamam** dönüştürme kabul etmek için.

### <a name="transform-editor-advanced-mode"></a>Düzenleyici Dönüştür: Gelişmiş mod

Gelişmiş mod sütunları birleştirmek için daha zengin bir deneyim sağlar. 

Seçme **ayırıcı** altında **birleştirmek sütunlara göre** kullanıcının dizelerde belirtmesine olanak tanır **ayırıcı** metin kutusu. Öğesinden dışarı sekmesinde **ayırıcı** veri gird sonuçlarında önizlemek için metin kutusu. Tuşuna **Tamam** dönüştürme kaydedilemedi.

Seçme **örnekler** altında **birleştirmek sütunlara göre** örnek birleşik değerler sağlamak kullanıcının sağlar. Örnek olarak bir satır yükseltmek için ızgaradaki satırların çift tıklayın. Beklenen çıktı yükseltilen satır karşı metin kutusuna yazın. Öğesinden dışarı sekmesinde **ayırıcı** veri gird sonuçlarında önizlemek için metin kutusu. Tuşuna **Tamam** dönüştürme kaydedilemedi. 

Kullanıcı arasında geçiş yapmak **temel mod** ve **Gelişmiş mod** bağlantıları dönüştürme Düzenleyicisi'ni tıklatarak.

### <a name="transform-editor-send-feedback"></a>Düzenleyici Dönüştür: geri bildirim gönder

Tıklayarak **geri bildirim gönder** bağlantı açar **geri bildirim** örnekler kullanıcıyla önceden doldurulmaz açıklamaları kutusuyla iletişim sağlanan. Kullanıcı, açıklama kutusunun içeriğini gözden geçirin ve sorunu anlamasına yardımcı olmak için daha fazla ayrıntı sağlar. Kullanıcı, verileri Microsoft ile paylaşmak istiyorsanız değil, kullanıcı önceden doldurulmuş örnek veri tıklatmadan önce silmelisiniz **geri bildirim gönder** düğmesi. 

### <a name="editing-existing-transformation"></a>Varolan dönüştürme düzenleme

Var olan bir kullanıcı düzenleyebilir **sütun örnekle birleştirmek** dönüştürme seçerek **Düzenle** dönüştürme adımının seçeneği. Tıklayarak **Düzenle** dönüştürme Düzenleyicisi'nde açar **temel mod**. Kullanıcının girebileceği **Gelişmiş mod** üstbilgi bağlantıya tıklayarak. Dönüştürme oluşturma sırasında sağlanan tüm örnekler var. gösterilir.

## <a name="example-using-separators"></a>Ayırıcılar kullanma örneği

Birleştirmek için bu örnek ayırıcı olarak virgül bir boşluk bırakarak kullanılan *Sokak*, *Şehir*, *durumu*, ve *ZIP* sütun.

|Cadde|Şehir|Durum|ZIP|Sütun|
|:----|:----|:----|:----|:----|
|16011 N.E. 36th yolu|REDMOND|WA|98052|16011 N.E. 36th yolu, REDMOND, Washington, 98052|
|16021 N.E. 36th yolu|REDMOND|WA|98052|16021 N.E. 36th yolu, REDMOND, Washington, 98052|
|16031 N.E. 36th yolu|REDMOND|WA|98052|16031 N.E. 36th yolu, REDMOND, Washington, 98052|
|16041 N.E. 36th yolu|REDMOND|WA|98052|16041 N.E. 36th yolu, REDMOND, Washington, 98052|
|16051 N.E. 36th yolu|REDMOND|WA|98052|16051 N.E. 36th yolu, REDMOND, Washington, 98052|
|16061 N.E. 36th yolu|REDMOND|WA|98052|16061 N.E. 36th yolu, REDMOND, Washington, 98052|
|3460 157th avenue NE|REDMOND|WA|98052|3460 157th avenue NE, REDMOND, Washington, 98052|
|3350 157th Ave N.E.|REDMOND|WA|98052|3350 157th Ave N.E., REDMOND, Washington, 98052|
|3240 157th avenue N.E.|REDMOND|WA|98052|3240 157th avenue N.E., REDMOND, Washington, 98052|

## <a name="example-using-by-example"></a>Örnek tarafından kullanma örneği

Değer **kalın** örnek olarak sağlandı.

|Tarih|Ay|Yıl|Saat|Dakika|Saniye|Birleşik sütun|
|:----|:----|:----|:----|:----|:----|:----|
|13|Eki|2016|15|01|23|**13 Eki 2016 15:01:23 saati**|
|16|Eki|2016|16|22|33|16 Eki 2016 15:01:33 saati|
|17|Eki|2016|12|43|12|17 Eki 2016 15:01:12 saati|
|12|Kas|2016|14|22|44|12 Kas 2016 15:01:44 saati|
|23|Kas|2016|01|52|45|23 Kas 2016 15:01:45 saati|
|16|Oca|2017|22|34|56|16 Ocak 2016 15:01:56 saati|
|23|Mar|2017|01|55|25|23 Mar 2016 15:01:25 saati|
|16|Nis|2017|11|34|36|16 Apr 2016 15:01:36 saati|

