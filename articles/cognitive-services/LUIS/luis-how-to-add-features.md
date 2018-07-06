---
title: LUIS uygulama özellikleri ekleme | Microsoft Docs
description: Bu kategori ve desenleri algılama veya hedefleri ve varlıkların tahmin iyileştirebilir uygulama özelliklerini eklemek için Language Understanding (LUIS) kullanın
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/30/2018
ms.author: v-geberr
ms.openlocfilehash: 3a63aa13b7c3c98fb4c9c0177474e2c848aa7d1f
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869199"
---
# <a name="use-features-to-improve-your-luis-apps-performance"></a>LUIS uygulamanızın performansını artırmak için özellikleri kullanın  

LUIS uygulamanızı kendi doğruluğunu artırmak için özellikler ekleyebilirsiniz. Bu belirli kelimeleri ipuçları sağlayarak Yardım LUIS özellikleri ve ifadeleri kategori bir parçasıdır. Bir kategori üyesi anlamayı LUIS öğrenir, bunu diğer benzer şekilde davranabilirsiniz.

## <a name="add-phrase-list"></a>İfade listesi ekleme

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **derleme**, ardından **tümcecik listeleri** uygulamanızın sol panelde. 

    ![İfade listesi Gezinti](./media/luis-add-features/phrase-list-nav.png)

2. Üzerinde **tümcecik listeleri** sayfasında **oluştur yeni ifade listesi**. 
 
    ![Yeni ifade listesi oluşturma](./media/luis-add-features/create-new-phrase-list.png)
    
3. İçinde **tümcecik listesine Ekle** iletişim kutusunda, ifade listesi adı olarak "Şehir" yazın. İçinde **değer** tümcecik listesi değerlerini yazın. Bir saat veya virgülle ayrılmış değerler kümesi tek bir değer yazın ve sonra basın **Enter**.

    ![İfade listesi şehirler Ekle](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS, tümcecik listenize eklemek için ilgili değerleri önerebilirsiniz. Tıklayın **önerilir** bir grup için added value(s) anlamsal olarak ilişkili önerilen değerleri almak için. Önerilen değerlerden herhangi birini tıklatın veya tıklatın **Ekle** bunları eklemek için tüm.

    ![Önerilen değerler ifade listesi](./media/luis-add-features/related-values.png)

5. Tıklayın **birbirinin yerine bu değerleri** birbirlerinin yerine kullanılabilir seçenekleri eklendi ifade listesi değerler.

    ![Önerilen değerler ifade listesi](./media/luis-add-features/interchangeable.png)

6. **Kaydet**’e tıklayın. "Şehir" tümceciği listeye eklenen **tümcecik listeleri** sayfası.

    ![Eklenir tümcecik listesi](./media/luis-add-features/phrase-list-cities.png)

## <a name="edit-phrase-list"></a>Tümcecik listesini düzenle

İfade listesinin adı tıklayarak **tümcecik listeleri** sayfası. İçinde **tümcecik listesini düzenle** gerekli iletişim kutusu açılır, yaptığınız tüm değişiklikleri ve ardından düzenleme **Kaydet**.

 ![Eklenir tümcecik listesi](./media/luis-add-features/edit-phrase-list.png)

## <a name="delete-phrase-list"></a>Tümcecik listeyi Sil 

Üç nokta simgesine tıklayın (***...*** ) düğmesini satırının sonundaki ve **Sil**.

 ![Eklenen listeyi Sil](./media/luis-add-features/delete-phrase-list.png)

## <a name="deactivate-phrase-list"></a>İfade listesi devre dışı bırak 

Üç nokta simgesine tıklayın (***...*** ) düğmesini satırının sonundaki ve **devre dışı bırak**.

 ![Eklenen liste devre dışı bırak](./media/luis-add-features/deactivate-phrase-list.png)

## <a name="pattern-regular-expression-feature"></a>Desen (normal ifade) özelliği 
**Bu özellik devre dışı**. Yeni düzen özellikleri için LUIS eklenemez. Var olan herhangi bir desen özelliği Mayıs 2018'e kadar desteklenir. Standart LUIS normal ifade eşleme çekme isteği ile katkıda [tanıyıcıları metin Github deposu](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Sonraki adımlar

Sonra ekleme, düzenleme, silme veya bir ifade listesi devre dışı bırakma [eğitme ve uygulamayı test etme](interactive-test.md) performansı artıran yeniden görmek için.
