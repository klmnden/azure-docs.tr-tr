---
title: QnA Maker hizmeti - Microsoft Bilişsel hizmetler ayarlama | Microsoft Docs
titleSuffix: Azure
description: QnA Maker hizmet ayarlama
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce452dd686529e017b4eae4717eadb044b389409
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354070"
---
# <a name="create-a-qna-maker-service"></a>QnA Maker hizmet oluşturma

Tüm QnA Maker Bilgi Bankası oluşturmadan önce ilk Azure QnA Maker hizmetinde ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir QnA Maker hizmet ayarlayabilirsiniz.

Bu kurulum birkaç Azure kaynaklarını dağıtır. Birlikte, bu kaynakları Bilgi Bankası içeriği yönetme ve bir uç nokta olsa soruyu yanıtlarken özellikleri sağlar.

1. [Azure Portal](<https://portal.azure.com>)’da oturum açın.

2.  Tıklayın **yeni kaynak Ekle**, arama "qna maker" yazın ve QnA Maker kaynak seçin

    ![Yeni bir QnA Maker hizmet oluştur](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Tıklayın **oluşturma** hüküm ve koşulları okuma sonra.

    ![Yeni bir QnA Maker hizmet oluştur](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. İçinde **QnA Maker**, bölgeler ve uygun katmanları seçin.

    ![Yeni bir QnA Maker hizmet oluştur](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dolgu **adı** bu QnA Maker Hizmeti'ni tanımlamak için benzersiz bir ada sahip. Bu ad Ayrıca, Bilgi Bankası ilişkili edileceği QnA Maker endpoint tanımlar.
    * Seçin **abonelik** QnA Maker kaynak dağıtılacak içinde.
    * Seçin **fiyatlandırma katmanı Yönetim** QnA Maker Yönetim Hizmetleri (portal ve yönetim API'leri). Bkz: [burada](https://aka.ms/qnamaker-pricing) SKU'ları fiyatlandırması hakkında bilgi.
    * Yeni bir **kaynak grubu** (önerilen) veya mevcut bir bu QnA Maker kaynak dağıtmak üzere kullanın.
    * Seçin **fiyatlandırma katmanı arama** Azure Search hizmeti. Gri ücretsiz katmanı seçeneğini görürseniz, aboneliğinizde dağıtılan bir ücretsiz Azure arama katmanı zaten anlamına gelir. Bu durumda temel Azure Search katmanı ile başlatmanız gerekir. Azure arama fiyatlandırma ayrıntıları görmek [burada](https://azure.microsoft.com/en-us/pricing/details/search/).
    * Seçin **arama konumunu** dağıtılacak Azure Search verileri istediğiniz. Müşteri verilerinin depolandığı gerekir içinde kısıtlamaları Azure arama için seçtiğiniz konum size bildirir.
    * Uygulama hizmetiniz için bir ad verin **uygulama adı**.
    * Varsayılan olarak standart (S1) katmanına uygulama hizmeti varsayılan olarak ayarlanır. Plan oluşturulduktan sonra değiştirebilirsiniz. App service fiyatlandırması hakkında daha fazla ayrıntı görmek [burada](https://azure.microsoft.com/en-in/pricing/details/app-service/).
    * Seçin **Web sitesi konumu** uygulama hizmeti dağıtılacağı.

        > [!NOTE]
        > Arama konumu Web sitesi konumundan farklı olabilir.

    * Etkinleştirmek isteyip istemediğinizi seçin **Application Insights** veya değil. Varsa **Application Insights** olduğu etkin QnA Maker telemetri trafiği, sohbet günlükleri ve hatalarını toplar.
    * Seçin **App ınsights konumu** Application Insights kaynağı dağıtılacağı.

5. Tüm alanlar doğrulandıktan sonra tıklatabilirsiniz **oluşturma** aboneliğinizde bu hizmetleri dağıtımını başlatmak için. İşlemin tamamlanması birkaç dakika sürecek.

6.  Dağıtım tamamlandığında, aboneliğinizde oluşturulan aşağıdaki kaynakları görürsünüz.

    ![Yeni bir QnA Maker hizmet oluştur](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md)