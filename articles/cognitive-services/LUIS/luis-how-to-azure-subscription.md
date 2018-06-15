---
title: Azure uç noktası aboneliği yönetme | Microsoft Docs
description: Bu makalede, ödeme planı aşağıdaki uç noktanız için sınırsız trafiği sağlamak HALUK hesabınız için ölçülen uç noktası anahtarı oluşturun.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 3526871f126ac975f323fe84b14883b361b684ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355768"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Azure uç noktası Abonelik anahtarları Yönet

Test etme ve yalnızca prototip için ücretsiz (F0) katmanını kullanın. Üretim sistemleri için bir [Ücretli](https://aka.ms/luis-price-tier) katmanı. 

> [!NOTE]
> Kullanmayın [anahtar yazma](luis-concept-keys.md#authoring-key) üretim uç noktası sorgular için.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>HALUK uç noktası anahtarı oluşturma

1. Oturum  **[Microsoft Azure](https://ms.portal.azure.com/)** 
2. Yeşil tıklatın **+** oturum açma üst sol paneli ve Market'te "HALUK" arayın, ardından tıklayın **dil anlama** ve izleyin **deneyimi oluşturma**  HALUK abonelik hesabı oluşturmak için. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Abonelik ayarları fiyatlandırma katmanları, vb. hesap adı dahil olmak üzere yapılandırın. 

    ![Azure API seçimi](./media/luis-azure-subscription/azure-api-choice.png) 

4. HALUK hizmeti oluşturduktan sonra oluşturulan erişim anahtarları görüntüleyebilirsiniz **kaynak yönetimi -> anahtarlar**.  

    ![Azure anahtarları](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Bölgenin günlüğüne [HALUK](luis-reference-regions.md) Web sitesi ve [yeni HALUK uç noktası anahtarı eklemek](Manage-Keys.md#assign-endpoint-key). 
    > * Adı unutmayın gerek bölgenin üzerinde seçmek için oluşturduğunuz Azure hizmet [HALUK](luis-reference-regions.md) Yayımla Sayfası.  

## <a name="change-luis-pricing-tier"></a>Değişiklik HALUK fiyatlandırma katmanı

1.  İçinde [Azure](https://portal.azure.com), HALUK aboneliği bulunamıyor. HALUK aboneliğe tıklayın.
    ![HALUK aboneliğinizi Bul](./media/luis-usage-tiers/find.png)
2.  Tıklatın **fiyatlandırma katmanı** mevcut fiyatlandırma katmanlarına görmeniz için. 
    ![Fiyatlandırma katmanlarına görüntüleyin](./media/luis-usage-tiers/subscription.png)
3.  Fiyatlandırma Katmanı'nı tıklatın ve'ı tıklatın **seçin** değişikliklerinizi kaydetmek için. 
    ![HALUK ödeme katmanınızı değiştirin](./media/luis-usage-tiers/plans.png)
4.  Fiyatlandırma değişikliği tamamlandıktan sonra bir açılır pencere yeni fiyatlandırma katmanı doğrular. 
    ![HALUK ödeme katmanınız doğrulayın](./media/luis-usage-tiers/updated.png)
5. Unutmayın [Bu uç noktası anahtarı atama](manage-keys.md#assign-endpoint-key) üzerinde **Yayımla** sayfasında ve tüm uç nokta sorgularda kullanın. 

## <a name="exceed-pricing-tier-usage"></a>Fiyatlandırma katmanı kullanım aşıyor
Her katman, belli bir oranda HALUK hesabınıza uç nokta isteklere izin verir. İstekleri oranını ölçülen hesabınızın ay veya dakika başına izin verilen hızından daha yüksek olursa isteklerini HTTP hatası alırsınız "429: çok fazla istek."

Her katman aylık biriktirici isteklere izin verir. Toplam istek sayısı izin verilen hızından daha yüksek olan istekleri HTTP hatası alıyorsunuz "403: Yasak".  

## <a name="viewing-summary-usage"></a>Özet kullanım görüntüleme
Azure'da HALUK kullanım bilgilerini görüntüleyebilirsiniz. **Genel bakış** sayfa çağrıları ve hatalar da dahil olmak üzere son Özet bilgilerini gösterir. Son nokta isteğinin bir HALUK yaparsanız, hemen izleme **genel bakış sayfasında**, gösterilmeye kullanım beş dakika bekleyin.

![Özet kullanım görüntüleme](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Kullanım grafikleri özelleştirme
Ölçümleri verileri içine daha ayrıntılı bir görünüm sağlar.

![Varsayılan ölçümleri](./media/luis-usage-tiers/metrics-default.png)

Ölçümleri grafiklerinizi zaman dilimi ve ölçü türü için yapılandırabilirsiniz. 

![Özel ölçümler](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Toplam işlemleri eşiği uyarı
Belirli bir işlem eşiği, örneğin 10.000 işlemleri üst sınırına ulaştınız zaman bilmek istiyorsanız, bir uyarı oluşturabilirsiniz. 

![Varsayılan Uyarıları](./media/luis-usage-tiers/alert-default.png)

Ölçüm bir uyarı Ekle **toplam çağrıları** belirli bir süre için ölçüm. Uyarı alması gereken tüm kişilerin e-posta adreslerini ekleyin. Uyarı alması gereken tüm sistemleri için Web kancası ekleyin. Uyarı tetiklendiğinde bir mantıksal uygulama de çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [sürümleri](luis-how-to-manage-versions.md) HALUK uygulamanıza değişikliklerini yönetme.