---
title: Azure uç noktası aboneliği yönetme | Microsoft Docs
description: Bu makalede, LUIS hesabınızın ödeme planı aşağıdaki uç noktanıza sınırsız trafiği sağlamak için bir tarifeli uç noktası anahtarı oluşturun.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: dbcbdb0f806241c015e6fbc160465390bead7c37
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340782"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Azure uç noktası Abonelik anahtarları yönetme

Test ve yalnızca prototip için ücretsiz katman (F0) kullanın. Üretim sistemleri için bir [Ücretli](https://aka.ms/luis-price-tier) katmanı. 

> [!NOTE]
> Kullanmayın [anahtar yazma](luis-concept-keys.md#authoring-key) için üretim uç noktası sorgular.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>LUIS uç noktası anahtarı oluşturma

1. Oturum  **[Microsoft Azure](https://ms.portal.azure.com/)** 
2. Yeşil tıklayın **+** "LUIS" Market'te arayın ve üst sol paneli oturum açma ve ardından tıklayarak **Language Understanding** izleyin **deneyimi oluşturun**  LUIS abonelik hesabı oluşturmak için. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Abonelik ayarları fiyatlandırma katmanları, vb. hesap adı dahil olmak üzere yapılandırın. 

    ![Azure API seçim](./media/luis-azure-subscription/azure-api-choice.png) 

4. LUIS hizmeti oluşturduktan sonra oluşturulan erişim anahtarlarını görüntüleyebilir **kaynak yönetimi -> anahtarlar**.  

    ![Azure anahtarları](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Oturum, bölgenin [LUIS](luis-reference-regions.md) Web sitesi ve [yeni LUIS uç noktası anahtarı ekleme](luis-how-to-manage-keys.md#assign-endpoint-key). 
    > * Adı Anımsanması gereken bölgenin üzerinde seçmek için oluşturduğunuz Azure hizmetinden [LUIS](luis-reference-regions.md) yayımlama sayfası.  

## <a name="change-luis-pricing-tier"></a>LUIS fiyatlandırma katmanını Değiştir

1.  İçinde [Azure](https://portal.azure.com), LUIS aboneliğinizi bulun. LUIS aboneliğe tıklayın.
    ![LUIS aboneliğinizi bulun](./media/luis-usage-tiers/find.png)
2.  Tıklayın **fiyatlandırma katmanı** kullanılabilir fiyatlandırma katmanlarına görmek için. 
    ![Fiyatlandırma katmanları görüntüleyin](./media/luis-usage-tiers/subscription.png)
3.  Fiyatlandırma katmanına tıklayıp **seçin** değişikliği kaydetmek için. 
    ![LUIS ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
4.  Fiyat değişikliği tamamlandıktan sonra bir açılır pencere yeni fiyatlandırma katmanına doğrular. 
    ![LUIS ödeme katmanınızı doğrulayın](./media/luis-usage-tiers/updated.png)
5. Unutmayın [Bu uç noktası anahtarı atama](luis-how-to-manage-keys.md#assign-endpoint-key) üzerinde **Yayımla** sayfasında ve tüm uç nokta sorguları kullanın. 

## <a name="exceed-pricing-tier-usage"></a>Fiyatlandırma katmanı kullanımı aşıyor
Her katman, uç nokta istekleri LUIS hesabınıza belli bir oranda sağlar. İstekleri oranını tarifeli hesabınızın dakika başına veya aylık izin verilen oranı yüksekse isteklerini HTTP hatası almak "429: çok fazla istek."

Her katman, aylık biriktirici istek sağlar. Toplam istek sayısı izin verilen hızından daha yüksek olan, istekler HTTP hatası alırsınız "403: Yasak".  

## <a name="viewing-summary-usage"></a>Özet kullanımı görüntüleme
Azure'da LUIS kullanım bilgilerini görüntüleyebilirsiniz. **Genel bakış** sayfası çağrı ve hata dahil olmak üzere son Özet bilgilerini gösterir. Bir LUIS uç nokta isteği yapıyorsa, hemen izleyin **genel bakış sayfasında**, gösterilecek kullanım beş dakika bekleyin.

![Özet kullanımı görüntüleme](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Kullanım grafikleri özelleştirme
Ölçümleri verilerine daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümleri](./media/luis-usage-tiers/metrics-default.png)

Zaman dilimi ve ölçü türü için ölçüm grafikleri yapılandırabilirsiniz. 

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Toplam işlem eşiği Uyarısı
Belirli bir işlem eşiği, örneğin 10.000 işlem ulaştığınız zaman bilmek istiyorsanız, bir uyarı oluşturabilirsiniz. 

![Varsayılan Uyarıları](./media/luis-usage-tiers/alert-default.png)

İçin ölçüm uyarısı Ekle **toplam çağrıları** belirli bir süre ölçümü. Uyarı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemleri için Web kancaları ekleyin. Uyarı tetiklendiğinde bir mantıksal uygulama da çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [sürümleri](luis-how-to-manage-versions.md) değişiklikleri LUIS uygulamanızı yönetme.