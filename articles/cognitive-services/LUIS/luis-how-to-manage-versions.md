---
title: Sürümleri yönetme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Sürümleri, farklı modelleri yayımladığınız olanak tanır. Modele bir değişiklik yapmadan önce uygulamanın farklı bir sürümünü geçerli etkin modele kopyalama iyi bir uygulamadır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: dfe23baa67c87b04a65630611ef71758beda268d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754898"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Düzenle ve hazırlık veya üretim uygulamaları etkilemeden test sürümleri kullanın

Sürümleri, farklı modelleri yayımladığınız olanak tanır. Başka bir geçerli etkin model kopyalamak için iyi bir uygulama olan [sürüm](luis-concept-version.md) modelde değişiklikler yapmadan önce uygulamanın. 

Sürümlerle çalışma için uygulamanızın adını seçerek açın **uygulamalarım** sayfasında ve ardından **Yönet** üst çubuktaki seçip **sürümleri** sol gezinti bölmesinde. 

Sürümlerinin listesi göster burada yayımlanmalarından ve hangi sürümü şu anda etkin olan hangi sürümleri yayımlanır. 

[![Yönet bölümünün, sürümler sayfasını](./media/luis-how-to-manage-versions/versions-import.png "Yönet bölümünün, sürümler sayfasını")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Sürümü Kopyala

1. Ardından kopyalama istediğiniz sürümü seçin **kopya** araç çubuğundan. 

2. İçinde **kopya sürüm** iletişim kutusunda, "0.2" gibi yeni sürümü için bir ad yazın.

   ![Kopya sürüm iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Sürüm kimliği, yalnızca karakterlerinden basamak oluşabilir veya '.' ve 10 karakterden uzun olamaz.
 
   Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin bir sürüm olarak ayarlayın.

## <a name="set-active-version"></a>Etkin sürümü Ayarla

Listeden bir sürüm seçin ve ardından **olun etkin** araç çubuğundan. 

[![Yönet bölümünün, sürümler sayfasını bir sürüm eylemi olun](./media/luis-how-to-manage-versions/versions-other.png "Yönet bölümünde, sürümler sayfasını bir sürüm eylemi olun")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Alma sürümü

1. Seçin **alma sürümü** araç çubuğundan. 

2. İçinde **yeni sürüm içeri aktarma** açılır penceresinde, yeni on karakter sürümü adını girin. JSON dosyasındaki sürüm uygulamada zaten varsa, bir sürüm kimliği ayarlamak yeterlidir.

    ![Yönet bölümünde, sürümler sayfasında, yeni sürüm içeri aktarılıyor](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Bir sürümü içeri aktardığınızda, yeni sürümü etkin sürümü haline gelir.

### <a name="import-errors"></a>İçeri Aktarma hataları

* Simgeleştirici hataları: Alırsanız bir **simgeleştirici hata** alırken, farklı bir kullanan bir sürümü içeri aktarmaya çalıştığınız [simgeleştirici](luis-language-support.md#custom-tokenizer-versions) uygulamanın şu anda kullandığı daha. Bu sorunu düzeltmek için bkz: [simgeleştirici sürümler arasında geçiş](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Diğer eylemler

* İçin **Sil** bir sürümü listeden bir sürüm seçin ve ardından **Sil** araç çubuğundan. **Tamam**’ı seçin. 
* İçin **Yeniden Adlandır** bir sürümü listeden bir sürüm seçin ve ardından **Yeniden Adlandır** araç çubuğundan. Yeni bir ad girin ve seçin **Bitti**. 
* İçin **dışarı** bir sürümü listeden bir sürüm seçin ve ardından **dışarı aktarma uygulama** araç çubuğundan. Dosyanın yerel makinenize indirilir. 

