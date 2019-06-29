---
title: İş sürekliliği planları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: f9892acb387a655e173ee5d2bde28e7346a6c535
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447529"
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
