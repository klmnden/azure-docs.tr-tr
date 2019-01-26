---
title: İş sürekliliği planları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: 9d5c6854090b98559d701329a2ff01ed7185d983
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911039"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Soru-cevap Oluşturucu hizmetiniz için bir iş süreklilik planı oluşturma

İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.

![Soru-cevap Oluşturucu bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Üst düzey yukarıda gösterildiği gibi olur:

1. İki paralel kümesi [soru-cevap Oluşturucu Hizmetleri](../How-To/set-up-qnamaker-service-azure.md) içinde [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Birincil ve ikincil bir Azure search dizinlerini eşitlenmiş halde tutun. GitHub örneği [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) Azure dizinleri yedekleme geri yükleme yapılacağını görmek için.

3. Application Insights kullanarak yedekleme [sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Birincil ve ikincil yığınları ayarlandıktan sonra kullanın [traffic manager](https://docs.microsoft.com/azure/traffic-manager/) iki uç nokta yapılandırmak ve bir yönlendirme yöntemi ayarlamak için.

5. Traffic manager uç noktanız için bir SSL sertifikası oluşturmanız gerekir. [SSL sertifikası bağlama](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) uygulama hizmetlerinizdeki.

6. Son olarak, traffic manager uç nokta Bot veya uygulamalarda kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu dağıtımınız için kapasite seçin](../Tutorials/choosing-capacity-qnamaker-deployment.md)
