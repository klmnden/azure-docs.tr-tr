---
title: QnA Maker hizmetinizi - Microsoft Bilişsel hizmetler için bir iş continuty planı oluşturun | Microsoft Docs
titleSuffix: Azure
description: QnA Maker hizmetiniz için bir iş sürekliliği planı oluşturma
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: ca6e54b8a8ca8b38e8ef6b1a148f8b2c54bd43da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353986"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>QnA Maker hizmetiniz için bir iş sürekliliği planı oluşturun

Birincil amacı, iş sürekliliği planı yok kesinti Bot veya bunu kullanan uygulama için olun bir esnek Bilgi Bankası uç oluşturmaktır.

![QnA Maker bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Üst düzey yukarıda belirtildiği şekilde aşağıdaki gibi olur:

1. İki paralel kümesi [QnA Maker Hizmetleri](../How-To/set-up-qnamaker-service-azure.md) içinde [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

2. Birincil ve ikincil Azure search dizinlerini eşitlenmiş tut. Github örneği kullanarak [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) Azure dizinleri yedekleme geri yükleme hakkında bilgi için.

3. Application Insights kullanarak yedekleme [sürekli verme](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry).

4. Birincil ve ikincil yığınları ayarlandıktan sonra kullanmak [trafik Yöneticisi](https://docs.microsoft.com/en-us/azure/traffic-manager/) iki uç nokta yapılandırmak ve bir yönlendirme yöntemi ayarlamak için.

5. Trafik Yöneticisi uç noktanız için bir SSL sertifikası oluşturmanız gerekir. [SSL sertifikası bağlama](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl) uygulama hizmetinizde.

6. Son olarak, trafik Yöneticisi uç noktası Bot veya uygulama kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker dağıtımınız için kapasiteyi seçin](../Tutorials/choosing-capacity-qnamaker-deployment.md)
