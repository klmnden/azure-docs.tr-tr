---
title: Derinlikli okuyucu API nedir?
titleSuffix: Azure Cognitive Services
description: Derinlikli okuyucu API'si hakkında bilgi edinin.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 4500b6213c549ab9977fe8f2d849ffa8089d04b9
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718442"
---
# <a name="what-is-immersive-reader"></a>Tam Ekran Okuyucu nedir?

[Sürükleyici okuyucu](https://www.onenote.com/learningtools) okuma kavramayı Gelişmekte olan okuyucular, language learners artırmak ve okuma zorluğu gibi farkları öğrenmek kişiler için kendini kanıtlamış teknikleri uygulayan aralığında tasarlanmış bir araçtır.

Tam Ekran Okuyucu SDK’sını kullanarak web uygulamanızda Tam Ekran Okuyucu’yu kullanabilirsiniz.

## <a name="what-does-immersive-reader-do"></a>Derinlikli okuyucu ne yapar?

Derinlikli okuyucu okuma erişilebilirliği herkes için hale getirecek şekilde tasarlanmıştır.

* En az Okuma Görünümü'nde içerik gösterir

  ![Tam Ekran Okuyucu](./media/immersive-reader.png)

* Yaygın olarak kullanılan bir kelimelerin resimleri görüntüler

  ![Resim sözlüğü](./media/picture-dictionary.png)

* Adlar, fiilleri sıfat ve zarflar vurgular

  ![Konuşma bölümü](./media/parts-of-speech.png)

* İçeriğinizi, sesli okur

  ![Sesli okuyun](./media/read-aloud.png)

* İçeriğinizi başka bir dile çevirir

  ![Çeviri](./media/translation.png)

* Hece sözcüklere ayırır

  ![Syllabification](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Derinlikli okuyucu nasıl çalışır?

Bir tek başına web uygulaması sürükleyici okuyucusudur, mevcut web uygulamanızı üzerinde derinlikli okuyucu JavaScript SDK'sını kullanarak çağrıldığında, görüntülenen bir `iframe`. Derinlikli okuyucu başlatmak için API çağırdığınızda, sürükleyici okuyucusunda göstermek istediğiniz içeriği belirtin. SDK'mız oluşturulmasını ve stilini işleme `iframe` ve konuşma bölümü, metin okuma, çeviri ve benzeri içeriğini işler sürükleyici okuyucu arka uç hizmetiyle iletişim.

## <a name="next-steps"></a>Sonraki adımlar

Tam Ekran Okuyucu’yu kullanmaya başlama:

* İçine atlanamaz [hızlı başlangıç](./quickstart.md)
* Keşfedin [sürükleyici okuyucu GitHub üzerinde SDK](https://github.com/Microsoft/immersive-reader-sdk)
* Okuma [sürükleyici okuyucu SDK başvurusu](./reference.md)