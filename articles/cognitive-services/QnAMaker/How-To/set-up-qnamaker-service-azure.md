---
title: Soru-cevap Oluşturucu bir soru-cevap Oluşturucu hizmeti - Kurulum
titleSuffix: Azure Cognitive Services
description: Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: f90c4cb1e98fe7ac43b21cd8ff01733f1d15cc50
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862235"
---
# <a name="create-a-qna-maker-service"></a>Soru-cevap Oluşturucu hizmeti oluşturma

Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.

Bu kurulum, birkaç Azure kaynaklarını dağıtır. Birlikte, bu kaynaklar, Bilgi Bankası içeriği yönetmek ve bir uç nokta ancak soru-yanıt özellikleri sağlar.

1. [Azure Portal](<https://portal.azure.com>)’da oturum açın.

2.  Tıklayarak **yeni kaynağı ekleyin**, arama "soru-cevap Oluşturucu" yazın ve soru-cevap Oluşturucu kaynağı seçin

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma - yeni kaynak ekleme](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Tıklayarak **Oluştur** hüküm ve koşulları okuma sonra.

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. İçinde **soru-cevap Oluşturucu**, uygun katmanları ve bölgeleri seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti - fiyatlandırma katmanı ve bölgeleri oluşturma](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dolgu **adı** Bu soru-cevap Oluşturucu hizmetini tanımlamak için benzersiz bir ada sahip. Bu ad Ayrıca, bilgi bankalarından ilişkili edileceği soru-cevap Oluşturucu uç nokta tanımlar.
    * Seçin **abonelik** soru-cevap Oluşturucu kaynağın dağıtılacağı.
    * Seçin **Yönetimi fiyatlandırma katmanı** soru-cevap Oluşturucu Yönetim Hizmetleri (portal ve API Yönetimi). Bkz: [burada](https://aka.ms/qnamaker-pricing) SKU'ları fiyatlandırması hakkında ayrıntılı bilgi için.
    * Yeni bir **kaynak grubu** (önerilir) veya bu soru-cevap Oluşturucu kaynak dağıtacağınız var olanı kullanın.
    * Seçin **arama fiyatlandırma katmanı** Azure Search hizmeti. Gri ücretsiz katmanı seçeneğini görürseniz, aboneliğinizde bir ücretsiz Azure arama katmanı zaten sahip olduğunuz anlamına gelir. Bu durumda Azure arama temel katman ile başlatmanız gerekir. Azure arama fiyatlandırma ayrıntılarına [burada](https://azure.microsoft.com/pricing/details/search/).
    * Seçin **arama konumu** dağıtılacak Azure Search veri istediğiniz. Müşteri verilerinin nerede depolanacağını gerekir, kısıtlamaları, Azure arama için seçtiğiniz konumu bilgilendirecektir.
    * App service içinde bir ad verin **uygulama adı**.
    * Varsayılan olarak App service için standart (S1) katman varsayılan olarak. Plan oluşturulduktan sonra değiştirebilirsiniz. App service fiyatlandırması hakkında daha fazla ayrıntı görmek [burada](https://azure.microsoft.com/pricing/details/app-service/).
    * Seçin **Web sitesi konumu** App Service dağıtılacağı.

        > [!NOTE]
        > Arama konumu Web sitesi konumundan farklı olabilir.

    * Etkinleştirmek isteyip istemediğinizi seçin **Application Insights** veya yok. Varsa **Application Insights** olan etkin, soru-cevap Oluşturucu telemetri trafik, sohbet günlükleri ve hataları toplar.
    * Seçin **uygulama öngörülerinin konumu** Application Insights kaynağı dağıtılacağı.

5. Tüm alanları doğrulandıktan sonra tıklayabilirsiniz **Oluştur** aboneliğinizde bu hizmetlerin dağıtımı başlatmak için. Tamamlanması birkaç dakika sürer.

6.  Dağıtım tamamlandığında, aboneliğinizde oluşturduğunuz aşağıdaki kaynakları görürsünüz.

    ![Yeni bir soru-cevap Oluşturucu hizmeti kaynağı oluşturuldu](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md)
