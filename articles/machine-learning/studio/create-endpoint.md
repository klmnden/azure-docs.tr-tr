---
title: Web Hizmeti uç noktaları oluşturma
titleSuffix: Azure Machine Learning Studio
description: Web Hizmeti uç noktaları, Azure Machine Learning Studio'da oluşturun. Her web hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/15/2019
ms.openlocfilehash: ac434a696f6e77e5ce61b430232166e7727eda38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60751187"
---
# <a name="create-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Dağıtılan Azure Machine Learning Studio web hizmetleri için uç noktası oluşturma

> [!NOTE]
> Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning web hizmeti.

Bir web hizmeti dağıtıldıktan sonra, bu hizmet için varsayılan bir uç noktası oluşturulur. Varsayılan uç noktası, API anahtarı kullanılarak çağrılabilir. Web Hizmetleri portalından, kendi anahtarlarına sahip ek uç noktalar ekleyebilirsiniz.
Her web hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve. Her uç nokta, müşterileriniz için dağıtabileceğiniz bir yetkilendirme anahtarına sahip benzersiz bir URL'dir.

## <a name="add-endpoints-to-a-web-service"></a>Bir web hizmeti için uç noktaları Ekle

Azure Machine Learning Web Hizmetleri portalını kullanarak bir web hizmeti için bir uç nokta ekleyebilirsiniz. Uç nokta oluşturulduktan sonra zaman uyumlu API'leri aracılığıyla, batch API'leri, kullanma ve excel çalışma sayfaları.

> [!NOTE]
> Web hizmetine ek uç noktalar eklediyseniz, varsayılan uç nokta nelze odstranit.

1. Machine Learning Studio'da, sol gezinti sütununda, Web Hizmetleri.
2. Web hizmeti Pano altındaki tıklatın **uç noktalarını yönetme**. Azure Machine Learning Web Hizmetleri portalında web hizmeti uç noktaları sayfası açılır.
3. **Yeni**’ye tıklayın.
4. Bir ad ve yeni uç nokta için bir açıklama yazın. Uç nokta adları 24 karakter veya uzunluğunda olmalıdır ve küçük harfler veya sayıdan bayraklardan gerekir. Günlüğe kaydetme düzeyini ve örnek veriler etkin olup olmadığını seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

## <a id="scaling"></a> Ek uç noktaları ekleyerek bir web hizmetini ölçeklendirme

Varsayılan olarak, yayımlanan web hizmeti her 20 eş zamanlı istek destekleyecek şekilde yapılandırıldığından ve 200 eş zamanlı istekleri olarak yüksek olabilir. Azure Machine Learning Studio web hizmeti için en iyi performansı sağlamak için ayarı otomatik olarak iyileştirilir ve portal değer yoksayılır.

200 en fazla eş zamanlı çağrılar değerden daha yüksek bir yük ile API'sini çağırmak için plan destekleyecekse aynı web hizmetini birden fazla uç noktası oluşturmanız gerekir. Ardından rastgele yük tümünün arasında dağıtabilirsiniz.

Bir web hizmeti ölçeklendirme genel bir görevdir. Ölçeklendirmek için bazı nedenler, 200'den fazla eşzamanlı isteği destekler, birden fazla uç nokta üzerinden kullanılabilirliğini artırmak veya web hizmeti için ayrı bir uç noktaları vermek üzeresiniz. Aynı web hizmeti aracılığıyla ek uç noktalar ekleyerek ölçeği artırabilir [Azure Machine Learning Web hizmetini](https://services.azureml.net/) portalı.

Yüksek eşzamanlılık sayısı kullanarak gelenlere yüksek oranda bir API arıyoruz değil, verebilirliğinde göz önünde bulundurun. Yüksek yük için yapılandırılmış bir API nispeten düşük yüke yerleştirirseniz, ara sıra zaman aşımları ve/veya ani gecikme süresindeki görebilirsiniz.

Zaman uyumlu API, genellikle düşük gecikme süreli olduğu istenen durumlarda kullanılır. Burada gecikme süresi, bir isteğin tamamlanması için API alır ve herhangi bir ağ gecikmeleri için hesap olmayan zaman anlamına gelir. Bir API 50 MS'den az gecikme süresiyle sahip varsayalım. Kullanılabilir kapasite azaltma düzeyi yüksek ve en fazla eş zamanlı çağrılar tam olarak kullanmak için 20 = 20 * 1000 Bu API'yi çağırmak gereken / saniye başına 50 = 400 saatleri. Bu daha da genişleterek, en fazla eş zamanlı çağrılar 200, saniyede bir 50 MS'den az gecikme süresiyle varsayılarak API 4000 kez çağrılması olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

[Bir Azure Machine Learning web hizmetini kullanma](consume-web-services.md).
