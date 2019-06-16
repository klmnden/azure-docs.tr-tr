---
title: Özel konuşma - konuşma Hizmetleri için model dağıtma
titlesuffix: Azure Cognitive Services
description: Bu belgede, özel konuşma tanıma portalı kullanarak bir uç nokta oluşturup dağıtmayı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: fc51c1d9d47340da85d42f7c398c8ee21c601beb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65025877"
---
# <a name="deploy-a-custom-model"></a>Özel bir modeli dağıtma

Karşıya ve veri inceledi, doğruluk hesaplanan ve özel bir modeli eğitilir sonra uygulamalar, Araçlar ve ürünlerle kullanmak için özel bir uç noktasını dağıtabilirsiniz. Bu belgede, özel konuşma tanıma portalı kullanarak bir uç nokta oluşturup dağıtmayı öğreneceksiniz.

## <a name="create-a-custom-endpoint"></a>Özel uç nokta oluşturma

Yeni özel uç nokta oluşturmak için Seç **dağıtım** sayfanın üstündeki özel konuşma menüsünde. Bu kez ise, tabloda listelenen uç nokta olduğunu fark edeceksiniz. Bir uç nokta oluşturduktan sonra dağıtılan her uç nokta izlemek için bu sayfayı kullanacaksınız.

Ardından, **uç noktası ekleme** girin bir **adı** ve **açıklama** özel uç noktanız için. Ardından bu uç nokta ile ilişkilendirmek istediğiniz özel bir model seçin. Bu sayfadan günlük kaydını etkinleştirebilirsiniz. Günlüğe kaydetme, uç nokta trafiği izlemenize olanak sağlar. Devre dışı bırakılırsa traffic kaydedilmez.

![Bir model dağıtma](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Kullanım ve fiyatlandırma ayrıntıları koşullarını kabul etmek unutmayın.

Ardından, **Oluştur**. Bu eylem, döndürür **dağıtım** sayfası. Tablo artık özel uç noktanıza karşılık gelen bir giriş içerir. Uç noktasının durumu geçerli durumunu gösterir. Bu özel Modellerinizi kullanarak yeni bir uç noktayı örneklemek için en fazla 30 dakika sürebilir. Dağıtım durumu değiştiğinde **tam**, uç noktayı kullanıma hazırdır.

Uç noktanız dağıtıldıktan sonra uç nokta adına bir bağlantı olarak görünür. Uç nokta, uç nokta URL'si ve örnek kod gibi uç noktanıza özel bilgileri görüntülemek için bağlantıya tıklayın.

## <a name="view-logging-data"></a>Günlük verilerini görüntüleme

Günlük verilerini şu altında indirilebilir **uç noktası > Ayrıntılar**.

## <a name="next-steps"></a>Sonraki adımlar

* Özel uç noktanız ile [Speech SDK'sı](speech-sdk.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Hazırlama ve test, verileri](how-to-custom-speech-test-data.md)
* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)
