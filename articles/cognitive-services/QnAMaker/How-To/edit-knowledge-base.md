---
title: Bilgi Bankası - soru-cevap Oluşturucu Düzenle
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/06/2018
ms.author: tulasim
ms.openlocfilehash: adcefe8fed927aca2533ea811bac56f0b92288de
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51279770"
---
# <a name="edit-a-knowledge-base"></a>Bilgi bankası düzenleme

Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.

## <a name="edit-your-knowledge-base-content"></a>Bilgi Bankası içeriğinizi düzenleyin

1.  Seçin **My bilgi bankalarından** üst gezinti çubuğunda. 

    Gördüğünüz oluşturduğunuz ya da azalan sırada sıralanır sizinle paylaşılan tüm hizmetleri **son değiştirme** tarih.

    ![My bilgi Bankalarından](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Düzenlemeler yapmak için belirli bir Bilgi Bankası'nı seçin.
 
3. **Ayarlar**’a tıklayın.

   Burada zorunlu alan hizmet adını düzenleyebilirsiniz.
  
   Tıklayarak, Bilgi Bankası için yeni SSS içeriği eklemek için yeni URL'leri ekleyebilirsiniz **knowledgebase Yönet -> ' + URL Ekle '** bağlantı.
   
   Tıklayarak mevcut URL'ler silebilirsiniz **Sil simgesi**.
   
   En son içeriği mevcut URL'ler gezinmesi için bilgi istiyorsanız, onay kutusu adı işaretleyerek **'Yenile'**, bu Bilgi Bankası son URL içeriklerle güncelleştirir.
   
Tıklayarak Bilgi Bankası, bir parçası olarak desteklenen dosya belge ekleyebilirsiniz **Yönet Bilgi Bankası -> ' + Dosya Ekle '**

Var olan herhangi bir Bilgi Bankası tıklayarak da içeri aktarabilirsiniz **'Ímport Bilgi Bankası'** düğmesi. 
   
Bilgi Bankası'nın updation bağlıdır **Yönetimi fiyatlandırma katmanı** , knowledgbase ile ilgili soru-cevap Oluşturucu hizmetini oluştururken kullanılan. Gerekli olursa yönetim katmanı Azure Portal'dan da güncelleştirebilirsiniz.

4. Bilgi Bankası değişiklikleri tamamladıktan sonra tıklayarak **kaydedin ve eğitme** değişiklikleri kalıcı hale getirmek için sayfanın sağ üst köşesindeki içinde.    

    ![Kaydet ve eğitme](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Sayfanın tıklatmadan önce kaydederken bırakarak ve eğitme değişiklikleri kalıcı olmaz.

## <a name="add-a-qna-pair"></a>Soru-Cevap çifti ekleme

Seçin **ekleme soru-cevap çifti** Bilgi Bankası tabloya yeni satır eklemek için.

![Soru-cevap çifti Ekle](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Soru-cevap çifti Sil

Bir soru-cevap'ı silmek için tıklayın **Sil** simgesine soru-cevap satırın en sağındaki.

![Soru-cevap çifti Sil](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Bir kullanıcı sorgu için bir eşleşme olasılığını artırmak için var olan bir soru-cevap çifti diğer sorular ekleyin.

![Diğer sorular ekleyin](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>meta veri ekleme


Filtre simgesini seçerek meta verileri çifti Ekle

![Meta veri ekleme](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Düzenli olarak kaydedin ve değişiklikleri kaybetmek istemiyorsanız düzenlemeler yaptıktan sonra Bilgi Bankası eğitme emin olun.

## <a name="manage-large-knowledge-bases"></a>Büyük bilgi bankalarından yönetme

1. Bankalarıyla olan **gruplandırılmış** bunlar ayıklanan veri kaynağı tarafından. Genişlet veya daralt veri kaynağı.
2. Yapabilecekleriniz **arama** Bankası üst Bilgi Bankası tablonun metin kutusuna yazarak. Soru, yanıt veya meta veri içeriğini aramak için ENTER'a tıklayın. Arama filtresi kaldırmak için X simgesine tıklayın.
3. **Sayfalandırma** büyük bilgi bankalarından yönetmenize olanak sağlar

    ![Arama, sayfalandırma, grubu](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="delete-knowledge-bases"></a>Bilgi bankaları Sil

Bilgi Bankası (KB) silinmesi kalıcı bir işlemdir. Geri alınamaz. Bilgi Bankası silmeden önce Bilgi Bankası'ndaki vermelisiniz **ayarları** soru-cevap Oluşturucu Portalı'nın sayfasında. 

BB'NİZLE paylaşıyorsa [ortak çalışanlar](collaborate-knowledge-base.md) silin, herkesin KB olarak erişimini kaybeder. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası üzerinde işbirliği yapın](./collaborate-knowledge-base.md)
