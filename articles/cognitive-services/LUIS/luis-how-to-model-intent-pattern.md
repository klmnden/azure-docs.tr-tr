---
title: Düzeni şablonları HALUK uygulamaları ekleme | Microsoft Docs
titleSuffix: Azure
description: Tahmin doğruluğunu artırmak için dil anlama (HALUK) uygulamalarında düzeni şablonları eklemeyi öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 68c0ea1fd3f2e60e0adec631f33c8bd09a3d9960
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356389"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Tahmin doğruluğunu artırmak için desenleri ekleme
Uç nokta utterances HALUK uygulama aldıktan sonra kullanmak [kavram](luis-concept-patterns.md) sözcük sırasını ve word seçim içindeki bir desenle ortaya utterances tahmin doğruluğunu artırmak için desenleri. Kullanım düzenleri [varlıklar](luis-concept-entity-types.md) ve belirli bir desene sözdizimini kullanarak veri ayıklamak için kullanıcı rolleri. 

## <a name="add-template-utterance-to-create-pattern"></a>Desen oluşturmak için şablon utterance Ekle
1. Şirket adını seçerek uygulamanızı açın **My uygulamaları** sayfasında ve ardından **desenleri** sol panelinde, altında **uygulama performansı**.

    ![Desenler listesinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Desen doğru hedefini seçin. 

    ![Hedefini seçin](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Şablon metin kutusuna şablon utterance yazın ve Enter seçin. Varlık adı girmek istediğiniz zaman doğru deseni varlık sözdizimini kullanın. Varlık sözdizimi ile başlayan `{`. Varlıkları görüntüler listesi. Doğru varlığı seçin ve ardından Enter seçin. 

    ![Varlık düzeni için ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir rolü içeriyorsa, tek bir iki nokta rolüyle belirtmek `:`, varlık adı sonra gibi `{Location:Origin}`. Varlıkların rollerin listesini bir liste görüntüler. Rolü seçin ve ardından Enter seçin. 

    ![Rolü içeren varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlık seçtikten sonra düzeni girmeyi tamamladığınızda ve ardından Enter seçin. Girme desenleri bittiğinde [eğitmek](luis-how-to-train.md) uygulamanızı.

    ![Her iki varlık türlerini girilen desenle ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Arama desenleri
Arama bazı verilen metni içeren desenleri bulmanızı sağlar.  

1. Büyüteç simgesini seçin.

    ![Ekran görüntüsü, desenleri sayfasıyla vurgulanan arama Aracı simgesi](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Arama metni desenleri listenin en sağ üst köşedeki arama kutusuna yazın ve Enter seçin. Arama metninizi dahil olmak üzere desenleri görüntülemek için desenleri listesi güncelleştirildi.

    ![Vurgulanan arama kutusuna arama metni ekran görüntüsü, desenleri sayfası](./media/luis-how-to-model-intent-pattern/search-text.png)

    Aramayı iptal etmek ve desenler tam listesini geri yükleme için yazdığınız arama metni silin.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Bir desen Düzenle
1. Bir desen düzenlemek için bu deseni çizgi sağ ucunda üç noktaya (...) simgesini seçin, sonra seçin **Düzenle**. 

    ![Desen satırda menü öğesi olarak Düzenle ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Herhangi bir değişiklik metin kutusuna girin. İşiniz bittiğinde, select girin. Düzenleme desenleri bittiğinde [eğitmek](luis-how-to-train.md) uygulamanızı.

    ![Desen düzenleme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Farklı bir hedefi için tek tek düzeni yeniden atama

Farklı bir hedefi için tek bir desen yeniden atamak için desen metnin sağına hedefi liste kutusunu seçin ve farklı bir hedefi seçin.

![Farklı bir hedefi için tek tek düzeni yeniden atama işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Farklı bir hedefi birkaç desenlerini yeniden atama

Farklı bir hedefi birkaç desenlerini yeniden atamak için her düzeni solundaki onay kutusunu işaretleyin veya üst onay kutusunu seçin. **Yeniden hedefi** seçeneği, araç çubuğundaki görüntüler. Desenler için doğru hedefini seçin. 

![Farklı bir hedefi birkaç desenlerini yeniden atama işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Tek bir desen Sil

1. Bir desen silmek için bu deseni çizgi sağ ucunda üç noktaya (...) simgesini seçin, ardından seçin **silmek**. 

    ![Utterance ekran görüntüsü, Sil](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Seçin **Tamam** silme işlemini onaylayın.

    ![Ekran görüntüsü, silme onayı](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Birkaç desenleri Sil

1. Birkaç desenleri silmek için her düzeni solundaki onay kutusunu işaretleyin veya üst onay kutusunu seçin. **Silmek desenleri (s)** seçeneği, araç çubuğundaki görüntüler. Seçin **silmek desenleri (s)**.  

    ![Birkaç desenleri silme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. **Silmek desenleri** onay iletişim kutusu görüntülenir. Seçin **Tamam** silme işlemini bitirmek için.

    ![Birkaç desenleri silme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Varlık tarafından filtre düzeni listesi

Belirli bir varlık tarafından desenleri listesini filtrelemek için seçin **varlık filtreleri** desenleri üstündeki araç içinde. 

![Varlık tarafından desenleri filtreleme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Filtre uygulandıktan sonra varlık adı araç çubuğunun altında görüntülenir. 

## <a name="filter-pattern-list-by-intent"></a>Amacına göre filtre düzeni listesi

Belirli bir hedefi tarafından desenleri listesini filtrelemek için seçin **hedefi filtreler** desenleri üstündeki araç çubuğunda. 

![Desenler amacına göre filtreleme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Filtre uygulandıktan sonra hedefi ad araç çubuğunun altında görüntülenir. 

## <a name="remove-entity-or-intent-filter"></a>Varlık veya hedefi filtre Kaldır
Desen listesinden filtre uygulandığında varlık veya hedefi adı araç çubuğu altında görüntülenir. Filtreyi kaldırmak için bir ad seçin.

![Varlık tarafından filtre uygulanmış desenleri ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Filtre kaldırılır ve tüm desenleri görüntüler. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Varolan utterance hedefi veya varlık sayfasında düzeni ekleme
Her iki var olan bir utterance bir desen oluşturabilirsiniz **hedefi** veya **varlık** sayfası. Herhangi bir amaç veya varlık sayfadaki tüm utterances utterance düzeyi seçenekleri erişimi gibi sağlama sağ sütun bir listede görüntülenen **Düzenle**, **silmek**, ve **düzeniolarakEkle**.

1. Utterance seçili satırını, utterance sağındaki üç nokta (...)'i seçip seçin **düzeni olarak Ekle**.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Seçenekler menüsünden vurgulanmış Ekle düzendeki utterances tablosunun ekran görüntüsü")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Desen göre değiştirmek [sözdizimi kurallarına](luis-concept-patterns.md#pattern-syntax). Seçtiğiniz utterance varlıklarıyla sahipse, bu zaten doğru sözdizimine sahip düzeninde varlıklardır.

    ![Varlık tarafından filtre uygulanmış desenleri ekran görüntüsü](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Model desenlerle değiştirdikten sonra uygulamanızın eğitme
Ekleme, düzenleme, kaldırmak veya bir desen yeniden atama sonra [eğitmek](luis-how-to-train.md) ve [yayımlama](PublishApp.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [bir desen yapı](luis-tutorial-pattern.md) bir pattern.any ve roller ile.
* Bilgi edinmek için nasıl [eğitmek](luis-how-to-train.md) uygulamanızı.