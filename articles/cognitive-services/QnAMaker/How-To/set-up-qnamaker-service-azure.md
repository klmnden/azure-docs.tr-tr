---
title: Soru-cevap Oluşturucu bir soru-cevap Oluşturucu hizmeti - Kurulum
titleSuffix: Azure Cognitive Services
description: Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 4bc40c0d4d44ea4dd809f59965ec5d1107be8541
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439781"
---
# <a name="create-a-qna-maker-service"></a>Soru-cevap Oluşturucu hizmeti oluşturma

Herhangi bir soru-cevap Oluşturucu bilgi bankalarından oluşturabilmeniz için önce bir soru-cevap Oluşturucu hizmetini azure'da ilk ayarlamanız gerekir. Bir abonelikte yeni kaynaklar oluşturma yetkisi olan herkes bir soru-cevap Oluşturucu hizmetini ayarlayabilirsiniz.

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

Bu yordam, birkaç Azure kaynaklarını dağıtır. Birlikte, bu kaynaklar, Bilgi Bankası içeriği yönetmek ve bir uç nokta ancak soru-yanıt özellikleri sağlar.

1. Azure portalında oturum açın ve [soru-cevap Oluşturucu Oluşturma](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) kaynak.

1. Seçin **Oluştur** hüküm ve koşulları okuma sonra.

    ![Yeni bir soru-cevap Oluşturucu hizmeti oluşturma](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. İçinde **soru-cevap Oluşturucu**, uygun katmanları ve bölgeleri seçin.

    ![Yeni bir soru-cevap Oluşturucu hizmeti - fiyatlandırma katmanı ve bölgeleri oluşturma](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dolgu **adı** Bu soru-cevap Oluşturucu hizmetini tanımlamak için benzersiz bir ada sahip. Bu ad Ayrıca, bilgi bankalarından ilişkili edileceği soru-cevap Oluşturucu uç nokta tanımlar.
    * Seçin **abonelik** soru-cevap Oluşturucu kaynağın dağıtılacağı.
    * Seçin **fiyatlandırma katmanı** soru-cevap Oluşturucu Yönetim Hizmetleri (portal ve API Yönetimi). Bkz: [burada](https://aka.ms/qnamaker-pricing) SKU'ları fiyatlandırması hakkında ayrıntılı bilgi için.
    * Yeni bir **kaynak grubu** (önerilir) veya bu soru-cevap Oluşturucu kaynak dağıtacağınız var olanı kullanın. Soru-cevap Oluşturucu, çeşitli Azure kaynakları oluşturur. oluşturduğunuzda bu kaynakları saklamak için bir kaynak grubu kolayca bulabilirsiniz, yönetmek ve kaynak grubu adına göre bu kaynakları silin.
    * Seçin bir **kaynak grubu konumu**.
    * Seçin **arama fiyatlandırma katmanı** Azure Search hizmeti. Gri ücretsiz katmanı seçeneğini görürseniz, aboneliğinizde bir ücretsiz Azure arama katmanı zaten sahip olduğunuz anlamına gelir. Bu durumda Azure arama temel katman ile başlatmanız gerekir. Azure arama fiyatlandırma ayrıntılarına [burada](https://azure.microsoft.com/pricing/details/search/).
    * Seçin **arama konumu** dağıtılacak Azure Search veri istediğiniz. Müşteri verilerinin nerede depolanacağını gerekir, kısıtlamaları, Azure arama için seçtiğiniz konumu bilgilendirecektir.
    * App service içinde bir ad verin **uygulama adı**.
    * Varsayılan olarak App service için standart (S1) katman varsayılan olarak. Plan oluşturulduktan sonra değiştirebilirsiniz. App service fiyatlandırması hakkında daha fazla ayrıntı görmek [burada](https://azure.microsoft.com/pricing/details/app-service/).
    * Seçin **Web sitesi konumu** App Service dağıtılacağı.

        > [!NOTE]
        > Arama konumu Web sitesi konumundan farklı olabilir.

    * Etkinleştirmek isteyip istemediğinizi seçin **Application Insights** veya yok. Varsa **Application Insights** olan etkin, soru-cevap Oluşturucu telemetri trafik, sohbet günlükleri ve hataları toplar.
    * Seçin **uygulama öngörülerinin konumu** Application Insights kaynağı dağıtılacağı.
    * Maliyet tasarrufu ölçüler için yapabilecekleriniz [paylaşmak](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) soru-cevap Oluşturucu için oluşturulan bazı ancak tüm Azure kaynakları. 

1. Tüm alanları doğrulandıktan sonra seçebileceğiniz **Oluştur** aboneliğinizde bu hizmetlerin dağıtımı başlatmak için. Tamamlanması birkaç dakika sürer.

1. Dağıtım tamamlandığında, aboneliğinizde oluşturduğunuz aşağıdaki kaynakları görürsünüz.

    ![Yeni bir soru-cevap Oluşturucu hizmeti kaynağı oluşturuldu](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="region-of-management-service"></a>Hizmet Yönetim bölgesi

Soru-cevap Oluşturucu, yalnızca ilk veri işleme için & portal için kullanılan Yönetim hizmetini yalnızca Batı ABD bölgesinde kullanılabilir. Müşteri veri yok, bu Batı ABD hizmetinde depolanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oluşturma ve Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md)
