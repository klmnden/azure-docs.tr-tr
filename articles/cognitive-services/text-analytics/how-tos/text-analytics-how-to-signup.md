---
title: Metin analizi API'sine kaydolma
titleSuffix: Azure Cognitive Services
description: Kaydolma ve metin analizi hizmeti kullanmaya yönelik yönergeler.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 53532a19482a33f8727e71d44ae169225b5b1c98
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60829676"
---
# <a name="how-to-sign-up-for-the-text-analytics-api"></a>Metin analizi API'sine kaydolma

Metin Analizi kaynakları 7/24 bulutta kullanılabilir. İçeriğinizi analiz edilmek üzere yüklemek için kaydolarak erişim anahtarı edinmeniz gerekir. API'ye yapılan her çağrı için bir erişim anahtarı gerekir.

+ Bir Azure aboneliğiyle başlayın. Deneme yapmak için [ücretsiz hesap](https://azure.microsoft.com/free/) oluşturabilirsiniz.

+ [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun ve **Metin Analizi API'sini** seçin. Kaydolduğunuzda anahtarınız oluşturulur.

Metin Analizi için keşfetme ve değerlendirme amacıyla kullanabileceğiniz Ücretsiz katmanın yanı sıra üretim iş yükleri için faturalanabilir katmanlar vardır. Her abonelikte birden fazla kaydolma olabilir: ücretsiz, ücretli bir ve VS. İstek hacminizin artması durumunda daha fazla işlem sunan bir katmana geçebilirsiniz.

Önizleme veya ücretsiz katmandaki hizmetler için hizmet düzeyi sözleşmesi yoktur. Daha fazla bilgi için bkz. [Bilişsel Hizmetler SLA'sı](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/)

## <a name="how-to-change-tiers"></a>Katman değiştirme

Ücretsiz katman ile başlayıp üretim iş yükleri için faturalanabilir katmanlardan birine geçebilirsiniz. Yüksek düzeylerde standart faturalama sunulur. Katman değiştirip aynı uç noktayı ve erişim anahtarlarını kullanmaya devam edebilirsiniz.

1. [Azure portalda](https://portal.azure.com) oturum açın ve [hizmetinizi bulun](text-analytics-how-to-access-key.md).

2. **Fiyat katmanı**'na tıklayın.

   ![Sol gezinti menüsündeki fiyat katmanı komutu](../media/portal-pricing-tier.png)

3. Bir katman seçip **Seç**'e tıklayın.  Seçim işleme alındığında yeni sınırlar uygulamaya alınır. 

   ![Katman seçim sayfasındaki kutucuklar ve Seç düğmesi](../media/portal-choose-tier.png)

## <a name="how-billing-works"></a>Faturalandırma nasıl çalışır?

Faturalandırma, işlem sayısına göre hesaplanır. Aylık faturalama döngüsünde belirli bir katman için işlem bloğu satın alabilirsiniz. Sınırı aşmanız halinde işlem başına ufak bir fazla kullanım ücreti alınır. Üst sınırı düzenli olarak aşıyorsanız üst katmanlardan birine geçebilirsiniz.

Daha fazla bilgi için lütfen [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) bakın.

### <a name="what-constitutes-a-transaction-in-the-text-analytics-api"></a>Metin Analizi API’sinde neler işlem olarak kabul edilir?
Bir belgede yapılan her ek açıklama bir işlem olarak hesaplanır. Toplu puanlama çağrılarında, bir işlemde puanlanması gereken belge sayısı da hesaba katılır. Sonuç olarak, örneğin bir API çağrısında yaklaşım analizi için 1.000 belge gönderilirse bu çağrı 1.000 işlem olarak hesaplanır.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine Genel Bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir erişim anahtarı alma](text-analytics-how-to-access-key.md)
