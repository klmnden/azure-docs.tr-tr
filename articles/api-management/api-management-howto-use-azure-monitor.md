---
title: "Azure API Management’ta yayımlanmış API’leri izleme | Microsoft Docs"
description: "Azure API Management’ta API’nizi izleme hakkında bilgi edinmek için bu öğreticideki adımları izleyin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 445723242a76dcef4a6b137439728235d5d6e32a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-published-apis"></a>Yayımlanan API’leri izleme

Azure İzleyici, tüm Azure kaynaklarınızı izlemeye yönelik tek bir kaynak sağlayan bir Azure hizmetidir. Azure İzleyici’yi kullanarak, API Management gibi Azure kaynaklarından gelen ölçüm ve günlükleri görüntüleyebilir, sorgulayabilir, yönlendirebilir, arşivleyebilir ve bunlar üzerinde işlem yapabilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüleme
> * API'nizin ölçümlerini görüntüleme 
> * API'niz yetkisiz çağrılar aldığında bir uyarı kuralı ayarlama

Aşağıdaki videoda, Azure İzleyici'yi kullanarak API Management’ı izleme işlemi gösterilmektedir. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Etkinlik günlüklerini görüntüleme

Etkinlik günlükleri, API Management hizmetleriniz üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüklerini kullanarak, API Management hizmetleriniz üzerinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" sorularını yanıtlayabilirsiniz. 

> [!NOTE]
> Etkinlik günlükleri, okuma (GET) işlemlerini ya da Azure portalında gerçekleştirilen veya özgün Yönetim API’leri kullanan işlemleri içermez.

API Management hizmetinizdeki etkinlik günlüklerine veya Azure İzleyici’deki tüm Azure kaynaklarınızın günlüklerine erişebilirsiniz. 

Etkinlik günlüklerini görüntülemek için:

1. APIM hizmet örneğinizi seçin.
2. **Etkinlik günlüğü**’ne tıklayın.

## <a name="view-diagnostic-logs"></a>Tanılama günlüklerini görüntüleme

Tanılama günlükleri, denetim ve sorun giderme konularında önemli işlem ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarınız üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar.

Tanılama günlüklerine erişmek için:

1. APIM hizmet örneğinizi seçin.
2. **Tanılama günlüğü**’ne tıklayın.

## <a name="view-metrics-of-your-apis"></a>API'lerinizin ölçümlerini görüntüleme

API Management, dakika başı yaydığı ölçümlerle API’lerinizin durumu hakkında neredeyse gerçek zamanlı görünürlük sağlar. Bazı kullanılabilir ölçümlerin özeti aşağıda verilmiştir:

* Kapasite (önizleme): APIM hizmetlerinizi yükseltme/eski sürüme düşürme hakkında karar vermenize yardımcı olur. Ölçüm, dakika başına yayılır ve raporlama zamanındaki ağ geçidi kapasitesini yansıtır. Ölçüm 0-100 aralığında değişir ve CPU ile bellek kullanımı gibi ağ geçidi kaynakları temel alınarak hesaplanır.
* Toplam Ağ Geçidi İsteği: dönem içindeki API isteklerinin sayısı. 
* Başarılı Ağ Geçidi İstekleri: 304, 307 ve 301’den küçük herhangi bir kod (örneğin, 200) dahil olmak üzere başarılı HTTP yanıt kodları almış API isteklerinin sayısı. 
* Başarısız Ağ Geçidi İstekleri: 400 ve 500’den büyük herhangi bir kod dahil olmak üzere hatalı HTTP yanıt kodları almış API isteklerinin sayısı.
* Yetkisiz Ağ Geçidi İstekleri: 401, 403 ve 429 dahil olmak üzere HTTP yanıt kodları almış API isteklerinin sayısı. 
* Diğer Ağ Geçidi İstekleri: Yukarıdaki kategorilerin hiçbirine ait olmayan HTTP yanıt kodları (örneğin, 418) almış API isteklerinin sayısı.

Ölçümlere erişmek için:

1. Sayfanın alt kısmındaki menüden **Ölçümler**’i seçin.
2. Açılan listeden, ilgilendiğiniz ölçümleri seçin (birden çok ölçüm ekleyebilirsiniz). 
    
    Örneğin, kullanılabilir ölçümler listesinden **Toplam Ağ Geçidi İsteği** ve **Başarısız Ağ Geçidi İstekleri**’ni seçin.
3. Grafikte, API çağrılarının toplam sayısı gösterilmektedir. Ayrıca, başarısız olan API çağrılarının sayısı gösterilmiştir. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Yetkisiz istekler için uyarı kuralı ayarlama

Ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırmanızı ayarlayabilirsiniz. Azure İzleyici, tetiklendiğinde aşağıdaki işlemleri yapmak üzere bir uyarı yapılandırmanıza olanak tanır:

* E-posta bildirimi gönderme
* Web kancası çağırma
* Bir Azure Mantıksal Uygulamasını çağırma

Uyarıları yapılandırmak için:

1. Sayfanın alt kısmındaki menü çubuğundan **Uyarı kuralları**’nı seçin.
2. **Ölçüm uyarısı ekle**’yi seçin.
3. Bu uyarı için bir **Ad** girin.
4. İzlenecek ölçüm için **Yetkisiz Ağ Geçidi İstekleri** seçeneğini belirleyin.
5. **E-posta sahipleri, katkıda bulunanlar ve okuyucular**’ı seçin.
6. **Tamam**'a basın.
7. Bir API anahtarı olmadan Konferans API’mizi çağırmayı deneyin. Bu API Management hizmetinin sahibi olarak bir e-posta uyarı alırsınız. 

    > [!TIP]
    > Uyarı kuralı tetiklendiğinde bir Web Kancası veya Azure Mantıksal Uygulaması da çağırabilir.

    ![set-up-alert](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüleme
> * API'nizin ölçümlerini görüntüleme 
> * API'niz yetkisiz çağrılar aldığında bir uyarı kuralı ayarlama

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [İzleme çağrıları](api-management-howto-api-inspector.md)
