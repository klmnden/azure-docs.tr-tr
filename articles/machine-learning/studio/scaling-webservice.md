---
title: Bir Azure Machine Learning web hizmetini eşzamanlılığı artırmak nasıl | Microsoft Docs
description: Ek uç noktaları ekleyerek bir Azure Machine Learning web hizmetini eşzamanlılığı artırmak öğrenin.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
keywords: azure machine Learning web Hizmetleri, kullanıma hazır hale getirme, ölçeklendirme, uç nokta, eşzamanlılık
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.component: studio
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.openlocfilehash: 6e827c88d840a111ef3bebfb8f2b2f116c02643e
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52264871"
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Ek uç noktaları ekleyerek bir Azure Machine Learning web hizmetini ölçeklendirme
> [!NOTE]
> Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning Web hizmeti. 
> 
> 

Varsayılan olarak, yayımlanan her Web hizmeti 20 eş zamanlı istek destekleyecek şekilde yapılandırıldığından ve 200 eş zamanlı istekleri olarak yüksek olabilir. Azure Machine Learning web hizmeti için en iyi performansı sağlamak için ayarı otomatik olarak iyileştirilir ve portal değer yoksayılır. 

200 en fazla eş zamanlı çağrılar değerden daha yüksek bir yük ile API'sini çağırmak için plan destekleyecekse aynı Web hizmetini birden fazla uç noktası oluşturmanız gerekir. Ardından rastgele yük tümünün arasında dağıtabilirsiniz.

Bir Web hizmeti ölçeklendirme genel bir görevdir. Ölçeklendirmek için bazı nedenler, 200'den fazla eşzamanlı isteği destekler, birden fazla uç nokta üzerinden kullanılabilirliğini artırmak veya web hizmeti için ayrı bir uç noktaları vermek üzeresiniz. Aynı Web hizmeti aracılığıyla ek uç noktalar ekleyerek ölçeği artırabilir [Azure Machine Learning Web hizmetini](https://services.azureml.net/) portalı.

Yeni uç nokta ekleme hakkında daha fazla bilgi için bkz. [uç noktaları oluşturma](create-endpoint.md).

Yüksek eşzamanlılık sayısı kullanarak gelenlere yüksek oranda bir API arıyoruz değil, verebilirliğinde göz önünde bulundurun. Yüksek yük için yapılandırılmış bir API nispeten düşük yüke yerleştirirseniz, ara sıra zaman aşımları ve/veya ani gecikme süresindeki görebilirsiniz.

Zaman uyumlu API, genellikle düşük gecikme süreli olduğu istenen durumlarda kullanılır. Burada gecikme süresi, bir isteğin tamamlanması için API alır ve herhangi bir ağ gecikmeleri için hesap olmayan zaman anlamına gelir. Bir API 50 MS'den az gecikme süresiyle sahip varsayalım. Kullanılabilir kapasite azaltma düzeyi yüksek ve en fazla eş zamanlı çağrılar tam olarak kullanmak için 20 = 20 * 1000 Bu API'yi çağırmak gereken / saniye başına 50 = 400 saatleri. Bu daha da genişleterek, en fazla eş zamanlı çağrılar 200, saniyede bir 50 MS'den az gecikme süresiyle varsayılarak API 4000 kez çağrılması olanak tanır.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
