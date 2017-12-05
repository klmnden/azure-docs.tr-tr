---
title: "İzleyici yayımlanan API'leri Azure API Management'te | Microsoft Docs"
description: "API'nizi Azure API Management'te izleneceği hakkında bilgi edinmek için Bu öğreticide adımları izleyin."
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
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Yayımlanan API izleme

Azure İzleyicisi tüm Azure kaynakları izlemek için tek bir kaynak sağlayan bir Azure hizmetidir. Azure izleme ile görselleştirme, sorgulama yapabilir, yönlendirmek, arşiv ve ölçümleri ve API Management gibi Azure kaynakları'ten gelen günlükleri eylemleri gerçekleştirin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüle
> * API'nizi metrikleri görüntüleyin 
> * API'nizi yetkisiz çağrıları aldığında bir uyarı kuralı ayarlayın

Aşağıdaki video Azure İzleyicisi'ni kullanarak API Management izleme gösterir. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Etkinlik günlüklerini görüntüle

Etkinlik günlükleri, API Management services üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik Günlükleri'ni kullanarak, belirleyebilirsiniz "ne, kimin, ne zaman ve" herhangi bir yazma, API Management hizmetlerde yapılan işlemleri (PUT, POST, DELETE) için. 

> [!NOTE]
> Etkinlik günlükleri okuma (GET) işlemleri veya Klasik yayımcı portalında gerçekleştirilen veya özgün yönetim API'leri kullanılarak işlemleri içermez.

API Management hizmetiniz etkinlik günlükleri erişmek veya günlükleri, Azure kaynaklarınızın Azure İzleyicisi'nde erişim. 

Etkinlik günlükleri görüntülemek için:

1. Öğesinden, **API Management** örneği, tıklatın **etkinlik günlüğü**.

## <a name="view-diagnostic-logs"></a>Tanılama günlüklerini görüntüle

Tanılama günlüklerini işlemleri ve denetim yanı sıra sorun giderme amacıyla önemli hataları hakkında zengin bilgi sağlar. Tanılama günlükleri, etkinlik günlükleri farklılık gösterir. Etkinlik günlükleri Azure kaynaklarınızı üzerinde gerçekleştirilen işlemler fikir sağlar. Tanılama günlükleri işlemleri kaynağınız kendisini gerçekleştirilen bir anlayış sağlar.

Tanılama günlüklerine erişmek için:

1. Öğesinden, **API Management** örneği, tıklatın **tanılama günlük**.

## <a name="view-metrics-of-your-apis"></a>Apı'lerinizi metrikleri görüntüleyin

API Management, Apı'lerinizi durumunu ve durumunu gerçek zamanlı görünürlük yakın vermiş, her dakika ölçümleri yayar. Kullanılabilir ölçümler bazıları bir özeti aşağıda verilmiştir:

* Kapasite (Önizleme): yükseltme/APIM hizmetlerinizi önceki sürüme indirme hakkında kararlar almanıza yardımcı olur. Ölçüm dakika başına yayılan ve ağ geçidi kapasite raporlama zamanında yansıtır. Ölçüm 0-100'den aralıkları ve ağ geçidi recourses CPU ve bellek kullanımı gibi temel alınarak hesaplanır.
* : Toplam ağ geçidi API sayısı dönemde istekleri. 
* Başarılı ağ geçidi istekleri: 304, 307 ve 301 (örneğin, 200) daha küçük bir şey de dahil olmak üzere başarılı HTTP yanıt kodları alınan API istek sayısı. 
* Başarısız olan ağ geçidi istekleri: Hatalı HTTP yanıt kodu 400 ve 500'den büyük herhangi bir şey de dahil olmak üzere alınan API istek sayısı.
* Yetkisiz ağ geçidi istekleri: 401, 403 ve 429 dahil olmak üzere HTTP yanıt kodları alınan API istek sayısı. 
* Diğer ağ geçidi istekleri: önceki kategorileri (örneğin, 418) birine ait değil HTTP yanıt kodları alınan API istek sayısı.

Ölçümleri erişmek için:

1. Seçin **ölçümleri** menüsünde sayfanın yakın.
2. Açılan listeden, ilgilendiğiniz ölçümleri seçin (birden çok ölçümleri ekleyebilirsiniz). 
    
    Örneğin, seçin **toplam ağ geçidi isteği** ve **başarısız ağ geçidi istekler** kullanılabilir ölçümler listesinden.
3. Grafik API çağrıları toplam sayısını gösterir. Ayrıca, başarısız API çağrılarının sayısını gösterir. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Yetkisiz isteği için bir uyarı kuralı ayarlayın

Ölçümleri ve etkinlik açtığında göre uyarıları almak üzere yapılandırabilirsiniz. Azure İzleyici tetikler, aşağıdakileri yapmak için bir uyarı yapılandırmanıza olanak sağlar:

* Bir e-posta bildirimi gönder
* bir Web kancası çağırın
* Bir Azure mantıksal uygulamayı çağırmak

Uyarıları yapılandırmak için:

1. Seçin **uyarı kuralları** menü çubuğundan sayfanın yakın.
2. Seçin **ölçüm uyarı Ekle**.
3. Girin bir **adı** bu uyarı için.
4. Seçin **yetkisiz ağ geçidi istekleri** izlemek için ölçüm olarak.
5. Seçin **sahipleri, Katkıda Bulunanlar ve okuyucular e-posta**.
6. **Tamam**'a basın.
7. Bir API anahtarı olmadan bizim konferans API çağırmayı deneyin. Bu API Management hizmeti sahibi olarak e-posta uyarı alın. 

    > [!TIP]
    > Bunu tetiklendiğinde uyarı kuralı ayrıca bir Web kancası veya bir Azure mantıksal uygulama çağırabilirsiniz.

    ![Set up Uyarısı](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Etkinlik günlüklerini görüntüleme
> * Tanılama günlüklerini görüntüle
> * API'nizi metrikleri görüntüleyin 
> * API'nizi yetkisiz çağrıları aldığında bir uyarı kuralı ayarlayın

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [İzleme çağrıları](api-management-howto-api-inspector.md)