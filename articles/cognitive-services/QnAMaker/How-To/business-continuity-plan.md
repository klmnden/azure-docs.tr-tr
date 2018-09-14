---
title: İş sürekliliği planları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: c65ef07f3644004c6e453f6a19035115df9d5764
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573986"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Soru-cevap Oluşturucu hizmetiniz için bir iş süreklilik planı oluşturma

İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.

![Soru-cevap Oluşturucu bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Üst düzey yukarıda gösterildiği gibi olur:

1. İki paralel kümesi [soru-cevap Oluşturucu Hizmetleri](../How-To/set-up-qnamaker-service-azure.md) içinde [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Birincil ve ikincil bir Azure search dizinlerini eşitlenmiş halde tutun. Github örneği [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) Azure dizinleri yedekleme geri yükleme yapılacağını görmek için.

3. Application Insights kullanarak yedekleme [sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Birincil ve ikincil yığınları ayarlandıktan sonra kullanın [traffic manager](https://docs.microsoft.com/azure/traffic-manager/) iki uç nokta yapılandırmak ve bir yönlendirme yöntemi ayarlamak için.

5. Traffic manager uç noktanız için bir SSL sertifikası oluşturmanız gerekir. [SSL sertifikası bağlama](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) uygulama hizmetlerinizdeki.

6. Son olarak, traffic manager uç nokta Bot veya uygulamalarda kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu dağıtımınız için kapasite seçin](../Tutorials/choosing-capacity-qnamaker-deployment.md)
