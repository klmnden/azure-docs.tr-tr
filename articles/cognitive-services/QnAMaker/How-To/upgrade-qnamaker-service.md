---
title: QnA Maker hizmetinizi - Microsoft Bilişsel hizmetler yükseltme | Microsoft Docs
titleSuffix: Azure
description: QnA Maker hizmetinizi yükseltme
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e8d3713d6729c4e30da9a64a382e9d5a647dfefd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354046"
---
# <a name="upgrade-your-qna-maker-service"></a>QnA Maker hizmetinizi yükseltme
QnA Maker yığınının bileşenleri tek tek ilk oluşturulduktan sonra yükseltmeyi seçebilir. Bağımlı bileşenler ve SKU seçimi ayrıntılarını görmek [burada](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>QnA Maker Yönetimi SKU yükseltme
QnA Maker yönetim SKU yükseltmek için:
1. Azure portalında QnA Maker kaynağınız gidin ve seçin **fiyatlandırma katmanı**.

    ![QnA Maker kaynak](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Uygun SKU tuşuna seçip **seçin**.

    ![QnA Maker fiyatlandırma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Yükseltme uygulama hizmeti
Yapabilecekleriniz [ölçeği](https://docs.microsoft.com/azure/app-service/web-sites-scale) veya uygulama hizmeti ölçeklendirin.

1. Azure portalında uygulama hizmeti kaynağa gidin ve seçin **ölçeği** veya **ölçeklendirmeyi azaltın** gerektiği gibi seçenekleri.

    ![QnA Maker app service ölçek](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Azure Search hizmeti yükseltme
Şu anda bir yerinde gerçekleştirmeniz mümkün değil Azure yükseltmesini SKU arayın. Ancak, istenen SKU ile yeni bir Azure search kaynak oluşturmak, yeni kaynak verileri geri yüklemek ve QnA Maker yığına bağlantı.

1. Azure portalında yeni bir Azure search kaynak oluşturmak ve istenen SKU'ı seçin.

    ![QnA Maker Azure arama kaynak](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Dizinler için yeni bir özgün Azure arama kaynaktan geri yükleyin. Yedekleme geri yükleme örnek kodu görmek [burada](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Verileri geri yüklendikten sonra yeni Azure arama kaynağa select Git **anahtarları**ve aşağı Not **adı** ve **yönetici anahtarını**.

    ![QnA Maker Azure arama anahtarları](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Yeni Azure arama kaynak QnA Maker yığına bağlamak için QnA Maker App Service'e gidin.

    ![QnA Maker uygulama hizmeti](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Seçin **uygulama ayarları** ve değiştirme **AzureSearchName** ve **AzureSearchAdminKey** 3. adım alanlardan.

    ![QnA Maker appservice ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Uygulama hizmeti yeniden başlatın.

    ![QnA Maker appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker API kullanın](../Quickstarts/csharp.md)
