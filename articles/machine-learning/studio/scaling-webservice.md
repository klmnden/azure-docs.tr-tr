---
title: "Bir Azure Machine Learning web hizmetini eşzamanlılığı artırmak nasıl | Microsoft Docs"
description: "Ek uç noktaları ekleyerek bir Azure Machine Learning web hizmetini eşzamanlılığı artırmak öğrenin."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: srikants
editor: cgronlun
keywords: "azure machine Learning web hizmetlerini ölçeklendirme, uç nokta, eşzamanlılık operationalization"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raymondl
ms.openlocfilehash: 0d9c4b77ae16b5919a62f855c5b472708ba39004
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Ek uç noktaları ekleyerek bir Azure Machine Learning web hizmetini ölçeklendirme
> [!NOTE]
> Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning Web hizmeti. 
> 
> 

Varsayılan olarak, yayımlanan her Web hizmeti 20 eş zamanlı istekleri desteklemek üzere yapılandırılmış ve 200 eş zamanlı istekleri olarak yüksek olabilir. Azure Machine Learning web hizmeti için en iyi performansı sağlamak için bu ayarı otomatik olarak iyileştirir ve portal değeri yoksayılır. 

API 200 en fazla eşzamanlı çağrıları değerden daha yüksek bir yük ile çağırmak için plan destekleyecekse aynı Web hizmetinde birden fazla uç noktası oluşturmanız gerekir. Ardından rastgele yük bunların tümünün arasında dağıtabilirsiniz.

Bir Web hizmeti ölçeklendirme ortak bir görevdir. 200'den fazla eşzamanlı isteği destekler, birden fazla uç noktası aracılığıyla kullanılabilirliğini artırmak veya web hizmeti için ayrı uç noktaları sağlamak için ölçeklendirmek için bazı nedenler şunlardır. Ek uç noktalar aynı Web hizmeti ekleyerek ölçeği artırabilir [Azure Machine Learning Web hizmeti](https://services.azureml.net/) portal.

Yeni uç nokta ekleme hakkında daha fazla bilgi için bkz: [uç noktaları oluşturma](create-endpoint.md).

Bir yüksek eşzamanlılık sayısı kullanarak API gelenlere yüksek oranda ile arıyoruz değil, detrimental göz önünde bulundurun. Yüksek yük için yapılandırılmış bir API nispeten düşük yük yerleştirirseniz gecikme ara sıra zaman aşımları ve/veya ani görebilirsiniz.

Zaman uyumlu API'leri genellikle düşük gecikme süresi burada istenen durumlarda kullanılır. Burada gecikme API bir isteği tamamlamak alır ve tüm ağ gecikme hesabı olmayan zaman anlamına gelir. 50-ms gecikme süresi ile bir API sahip varsayalım. Kullanılabilir kapasiteye sahip azaltma düzeyi yüksek ve en fazla eşzamanlı çağrıları tam olarak kullanmak üzere = 20, bu API 20 * 1000 çağırmanız gerekir / saniye başına 50 = 400 zaman. Bu daha fazla genişletme, en fazla eşzamanlı çağrıları 200, 50-ms gecikme varsayılarak saniyede API 4000 kez çağrılması olanak sağlar.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
