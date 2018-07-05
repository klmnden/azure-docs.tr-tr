---
title: 'Azure Analysis Services öğreticisi 3. ders: Tarih Tablosu olarak işaretleme | Microsoft Docs'
description: Azure Analysis Services öğretici projesinde bir tarih tablosunun nasıl işaretleneceğini açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6627cf74154e66a84f34d5d5bc1d78ed3e0c38a2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443608"
---
# <a name="mark-as-date-table"></a>Tarih Tablosu olarak işaretleme

2. Ders: Verileri alma sırasında DimDate adlı bir boyut tablosunu içeri aktardınız. Modelinizde bu tablo DimDate olarak adlandırılsa da, tarih ve saat verilerini içermesi bakımından *Tarih tablosu* olarak da bilinir.  
  
Daha sonra ölçü oluştururken yapacağınız gibi DAX akıllı zaman gösterimi işlevlerini her kullandığınızda, bir *Tarih tablosu* ve bu tabloda *Tarih sütunu* benzersiz tanımlayıcısını içeren özellikler belirtmeniz gerekir.
  
Bu derste, DimDate tablosunu *Tarih tablosu* ve Tarih sütununu (Veri tablosundaki) *Tarih sütunu* (benzersiz tanımlayıcı) olarak işaretlersiniz.  

Tarih tablosu ve tarih sütununu işaretlemeden önce, modelinizi daha kolay anlaşılır hale getirmek üzere biraz temizlik yapmak için uygun bir zamandır. DimDate tablosunda **FullDateAlternateKey** adlı sütuna dikkat edin. Bu sütun, tabloya dahil edilen her takvim yılındaki her gün için bir satır içerir. Bu sütunu ölçü formüllerinde ve raporlarda çok fazla kullanırsınız. Ancak, FullDateAlternateKey bu sütun için çok iyi bir tanımlayıcı değildir. Bu değeri **Date** olarak adlandırıp değerin formüllerde tanımlanmasını ve kullanılmasını kolaylaştırabilirsiniz. Mümkün olduğunda, SSDT’de ve Power BI ile Excel gibi istemci raporlama uygulamalarında tanımlanmalarını kolaylaştırmak üzere tablo ve sütun gibi nesnelerin yeniden adlandırılması iyi bir fikirdir. 
  
Bu dersi tamamlamak için tahmini süre: **Üç dakika**  
  
## <a name="prerequisites"></a>Önkoşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirmeden önce, bir önceki dersi tamamlamış olmanız gerekir: [2. Ders: Veri alma](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>FullDateAlternateKey sütununu yeniden adlandırmak için

1.  Model tasarımcısında **DimDate** tablosuna tıklayın.

2.  **FullDateAlternateKey** sütununun üst bilgisine çift tıklayın ve sonra **Date** olarak yeniden adlandırın.

  
### <a name="to-set-mark-as-date-table"></a>Tarih Tablosu olarak işaretleme özelliğini ayarlamak için  
  
1.  **Tarih** sütununu seçin ve ardından **Özellikler** penceresindeki **Veri Türü** altında **Tarih** öğesinin seçildiğinden emin olun.  
  
2.  **Tablo** menüsüne, ardından **Tarih** öğesine ve sonra **Tarih Tablosu olarak işaretle**’ye tıklayın.  
  
3.  **Tarih Tablosu olarak işaretle** iletişim kutusundaki **Tarih** liste kutusunda benzersiz tanımlayıcı olarak **Tarih** sütununu seçin. Bu sütun genellikle varsayılan olarak seçilidir. **Tamam**’a tıklayın. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Sırada ne var?
[4. Ders: İlişki oluşturma](../tutorials/aas-lesson-4-create-relationships.md).
  
