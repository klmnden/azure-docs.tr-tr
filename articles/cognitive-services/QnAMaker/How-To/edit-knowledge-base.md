---
title: Bilgi Bankası - soru-cevap Oluşturucu Düzenle
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c9add80b7494ae2a8e671967a96dc5d3c7307f51
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65541634"
---
# <a name="edit-a-knowledge-base-in-qna-maker"></a>Soru-cevap Oluşturucu, Bilgi Bankası Düzenle

Soru-cevap Oluşturucu, kullanımı kolay bir düzenleme deneyimi sunarak bilgi bankanızı içeriğini yönetmenizi sağlar.

<a name="add-datasource"></a>

## <a name="edit-your-knowledge-base-content"></a>Bilgi Bankası içeriğinizi düzenleyin

1.  Seçin **My bilgi bankalarından** üst gezinti çubuğunda. 

    Gördüğünüz oluşturduğunuz ya da azalan sırada sıralanır sizinle paylaşılan tüm hizmetleri **son değiştirme** tarih.

    ![My bilgi Bankalarından](../media/qnamaker-how-to-edit-kb/my-kbs.png)

1. Düzenlemeler yapmak için belirli bir Bilgi Bankası'nı seçin.
 
1. Seçin **ayarları**. Burada zorunlu alan hizmet adını düzenleyebilirsiniz.
  
    |Hedef|Eylem|
    |--|--|
    |URL ekle|SSS içeriği yeni Bilgi Bankası'na tıklayarak eklemek için yeni URL'leri ekleyebilirsiniz **Bilgi Bankası'Yönet -> ' + URL Ekle '** bağlantı.|
    |URL Sil|Var olan URL'ler Sil simgesini seçerek silebilirsiniz, çöp kutusu.|
    |URL içerik Yenile|En son içeriği mevcut URL'ler gezinmesi için bilgi bankanızı istiyorsanız belirleyin **Yenile** onay kutusu. Bu, Bilgi Bankası'ı en son URL içeriklerle güncelleştirir.|
    |Dosya Ekle|Seçerek bir Bilgi Bankası bir parçası olarak desteklenen dosya belgeye eklediğiniz **Bilgi Bankası yönetme**, ardından seçerek **+ Dosya Ekle**|
    |İçeri Aktarma|Mevcut tüm Bilgi Bankası seçerek de içeri aktarabilirsiniz **Ímport Bankası** düğmesi. |
    |Güncelleştirme|Bilgi Bankası güncelleştirme bağlıdır **Yönetimi fiyatlandırma katmanı** bilgi bankanızı ile ilgili soru-cevap Oluşturucu hizmeti oluşturulurken kullanılır. Gerekli olursa yönetim katmanı Azure Portal'dan da güncelleştirebilirsiniz.

1. Bilgi bankasına tamamladıktan sonra değişiklik yapmaya **kaydedin ve eğitme** değişiklikleri kalıcı hale getirmek için sayfanın sağ üst köşesindeki içinde.    

    ![Kaydet ve eğitme](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!CAUTION]
    >Seçmeden önce sayfadan ayrılırsanız **kaydedin ve eğitme**, tüm değişiklikler kaybolacak.

## <a name="add-a-qna-pair"></a>Soru-Cevap çifti ekleme

Üzerinde **ayarları** sayfasında **ekleme soru-cevap çifti** Bilgi Bankası tabloya yeni satır eklemek için.

![Soru-cevap çifti Ekle](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Soru-cevap çifti Sil

Bir soru-cevap'ı silmek için tıklayın **Sil** simgesine soru-cevap satırın en sağındaki. Bu kalıcı bir işlemdir. Geri alınamaz. Öğesinden, KB verme göz önünde bulundurun **Yayımla** çiftleri silmeden önce sayfa. 

![Soru-cevap çifti Sil](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Bir kullanıcı sorgu için bir eşleşme olasılığını artırmak için var olan bir soru-cevap çifti diğer sorular ekleyin.

![Diğer sorular ekleyin](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>meta veri ekleme

İlk seçerek meta veri çiftlerini eklemek **görüntüleme seçenekleri**, ardından seçerek **Göster meta verileri**. Bu meta veri sütunu görüntüler. Ardından, **+** meta verileri çifte eklemek oturum açın. Bir anahtarı ve tek bir değer bu çifti oluşur.

![Meta veri ekleme](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Düzenli olarak kaydedin ve değişiklikleri kaybetmek istemiyorsanız düzenlemeler yaptıktan sonra Bilgi Bankası eğitme emin olun.

## <a name="manage-large-knowledge-bases"></a>Büyük bilgi bankalarından yönetme

* **Veri kaynağı grupları**: Bankalarıyla içinden ayıklandıkları veri kaynağı tarafından gruplandırılır. Genişlet veya daralt veri kaynağı.

    ![Daralt ve veri kaynağı sorularını ve yanıtlarını genişletmek için soru-cevap Oluşturucu veri kaynağını çubuğundaki kullanın](../media/qnamaker-how-to-edit-kb/data-source-grouping.png)

* **Bilgi Bankası arama**: Metin kutusuna Bilgi Bankası Tablo üst bilgi bankasında arama yapabilirsiniz. Soru, yanıt veya meta veri içeriğini aramak için ENTER'a tıklayın. Arama filtresi kaldırmak için X simgesine tıklayın.

    ![Görünüm yalnızca filtreyle eşleşen öğeler azaltmak için yukarıdaki soruları ve yanıtları soru-cevap Oluşturucu arama kutusunu kullanın](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

* **Sayfalandırma**: Hızlı bir şekilde büyük bilgi bankalarından yönetmek için veri kaynakları arasında taşıma

    ![Sorular ve cevaplar sayfaları aracılığıyla taşımak için yukarıdaki soruları ve yanıtları soru-cevap Oluşturucu sayfalandırma özelliklere kullanın](../media/qnamaker-how-to-edit-kb/pagination.png)

## <a name="delete-knowledge-bases"></a>Bilgi bankaları Sil

Bilgi Bankası (KB) silinmesi kalıcı bir işlemdir. Geri alınamaz. Bilgi Bankası silmeden önce Bilgi Bankası'ndaki vermelisiniz **ayarları** soru-cevap Oluşturucu Portalı'nın sayfasında. 

BB'NİZLE paylaşıyorsa [ortak çalışanlar](collaborate-knowledge-base.md) silin, herkesin KB olarak erişimini kaybeder. 

## <a name="delete-azure-resources"></a>Azure kaynaklarını silme 

Soru-cevap Oluşturucu, bilgi bankaları için kullanılan Azure kaynakları silerseniz, bilgi bankalarından artık çalışmaz. Tüm kaynakları silmeden önce bilgi bankalarından öğesinden dışarı emin olun **ayarları** sayfası. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası üzerinde işbirliği yapın](./collaborate-knowledge-base.md)
