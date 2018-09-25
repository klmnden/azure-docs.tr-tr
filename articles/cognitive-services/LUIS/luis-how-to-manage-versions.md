---
title: LUIS uygulamalarında sürümlerini yönetme
titleSuffix: Azure Cognitive Services
description: Sürümleri, farklı modelleri yayımladığınız olanak tanır. Modele bir değişiklik yapmadan önce uygulamanın farklı bir sürümünü geçerli etkin modele kopyalama iyi bir uygulamadır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: db461191b70aabc322e570ecc814a076c21206f2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033796"
---
# <a name="manage-versions"></a>Sürümleri yönetme

Sürümleri, farklı modelleri yayımladığınız olanak tanır. Başka bir geçerli etkin model kopyalamak için iyi bir uygulama olan [sürüm](luis-concept-version.md) modelde değişiklikler yapmadan önce uygulamanın. 

Sürümlerle çalışma için uygulamanızın adını seçerek açın **uygulamalarım** sayfasında ve ardından **Yönet** üst çubuktaki seçip **sürümleri** sol gezinti bölmesinde. 

Sürümlerinin listesi göster burada yayımlanmalarından ve hangi sürümü şu anda etkin olan hangi sürümleri yayımlanır. 

[![](./media/luis-how-to-manage-versions/versions-import.png "Bölümünde, sürümler sayfasını yönetme")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Sürümü Kopyala

1. Ardından kopyalama istediğiniz sürümü seçin **kopya** araç çubuğundan. 

2. İçinde **kopya sürüm** iletişim kutusunda, "0.2" gibi yeni sürümü için bir ad yazın.

   ![Kopya sürüm iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Sürüm kimliği, yalnızca karakterlerinden basamak oluşabilir veya '.' ve 10 karakterden uzun olamaz.
 
 Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin bir sürüm olarak ayarlayın.

## <a name="set-active-version"></a>Etkin sürümü Ayarla

Listeden bir sürüm seçin ve ardından **olun etkin** araç çubuğundan. 

[![](./media/luis-how-to-manage-versions/versions-other.png "Bölümünde, sürümler sayfasını yönetme")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Alma sürümü

1. Seçin **alma sürümü** araç çubuğundan. 

2. İçinde **yeni sürüm içeri aktarma** açılır penceresinde, yeni on karakter sürümü adını girin. JSON dosyasındaki sürüm uygulamada zaten varsa, bir sürüm kimliği ayarlamak yeterlidir.

    ![Bölümünde, sürümler sayfasını yönetme](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Bir sürümü içeri aktardığınızda, yeni sürümü etkin sürümü haline gelir.

<a name = "export-version"></a>

## <a name="other-actions"></a>Diğer eylemler

* İçin **Sil** bir sürümü listeden bir sürüm seçin ve ardından **Sil** araç çubuğundan. **Tamam**’ı seçin. 
* İçin **Yeniden Adlandır** bir sürümü listeden bir sürüm seçin ve ardından **Yeniden Adlandır** araç çubuğundan. Yeni bir ad girin ve seçin **Bitti**. 
* İçin **dışarı** bir sürümü listeden bir sürüm seçin ve ardından **dışarı aktarma uygulama** araç çubuğundan. Dosyanın yerel makinenize indirilir. 

