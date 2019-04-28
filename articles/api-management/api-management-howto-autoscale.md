---
title: Bir Azure API Management örneğinin otomatik ölçeklendirmeyi yapılandırma | Microsoft Docs
description: Bu konuda, Azure API Management örneği için otomatik ölçeklendirme davranışını ayarlama açıklanmaktadır.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
origin.date: 06/20/2018
ms.date: 12/31/2018
ms.author: v-yiso
ms.openlocfilehash: a01e50debf11daf2f1163a56726f5574f7e3e379
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123476"
---
# <a name="automatically-scale-an-azure-api-management-instance"></a>Azure API Management örneği otomatik olarak ölçeklendirme  

Azure API Management hizmet örneği ölçeğini otomatik olarak bir dizi kurala göre temel. Bu davranış etkinleştirilmeli ve Azure İzleyici yapılandırılır ve yalnızca desteklenen **standart** ve **Premium** Azure API Management hizmet katmanı.

Makale, otomatik ölçeklendirme yapılandırma sürecinde yardımcı olur ve otomatik ölçeklendirme kuralları en iyi yapılandırılmasını önerir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede adımları için yapmanız gerekir:

+ Etkin bir Azure aboneliğine sahip.
+ Azure API Management örneği vardır. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Kavramı anlamak [bir Azure API Management örneğinin kapasite](api-management-capacity.md).
+ Anlamak [el ile bir Azure API Management örneği işlemi ölçeklendirme](upgrade-and-scale.md), maliyet sonuçları dahil olmak üzere.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="azure-api-management-autoscale-limitations"></a>Azure API Management otomatik ölçeklendirme sınırlamaları

Bazı sınırlamalar ve ölçeklendirme kararları sonuçlarını otomatik ölçeklendirme davranışını yapılandırmadan önce göz önünde bulundurulması gerekir.

+ Otomatik ölçeklendirme, yalnızca etkinleştirilebilir **standart** ve **Premium** Azure API Management hizmet katmanı.
+ Fiyatlandırma katmanları de bir hizmet örneği için birim maksimum sayısını belirtin.
+ Ölçeklendirme işlemi, en az 20 dakika sürer.
+ Hizmet başka bir işlem tarafından kilitli olup olmadığını ölçeklendirme isteği başarısız ve otomatik olarak yeniden deneyin.
+ Çok bölgeli dağıtımlar, yalnızca birim hizmetiyle durumunda **birincil konum** ölçeklendirilebilir. Diğer konumlarda birimleri ölçeklendirilemiyor.

## <a name="enable-and-configure-autoscale-for-azure-api-management-service"></a>Etkinleştirme ve Azure API Management hizmeti için otomatik ölçeklendirmeyi yapılandırma

Bir Azure API Management hizmeti için otomatik ölçeklendirmeyi yapılandırmak için aşağıdaki adımları izleyin:

1. Gidin **İzleyici** Azure portalında örneği.

    ![Azure İzleyici](media/api-management-howto-autoscale/01.png)

2. Seçin **otomatik ölçeklendirme** sol taraftaki menüden.

    ![Azure İzleyici otomatik ölçeklendirme kaynak](media/api-management-howto-autoscale/02.png)

3. Azure API Yönetimi hizmetiniz, açılan menüler filtrelere göre bulun.
4. İstenen Azure API Management hizmet örneği seçin.
5. Yeni açılan bölümünde tıklayın **etkinleştirmek otomatik ölçeklendirme** düğmesi.

    ![Azure İzleyici otomatik ölçeklendirme etkinleştir](media/api-management-howto-autoscale/03.png)

6. İçinde **kuralları** bölümünde **+ alınabilecek**.

    ![Azure İzleyici otomatik ölçeklendirme kuralı ekleyin](media/api-management-howto-autoscale/04.png)

7. Yeni bir ölçek genişletme kuralı tanımlayın.

   Örneğin, ortalama kapasite ölçüm son 30 dakika boyunca % 80'i aştığında ölçek genişletme kuralı ek bir Azure API Management biriminin tetikleyebilir. Aşağıdaki tabloda, böyle bir kural yapılandırmasını sağlar.

    | Parametre             | Değer             | Notlar                                                                                                                                                                                                                                                                           |
    |-----------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Ölçüm kaynağı         | Geçerli kaynak  | Geçerli Azure API Management resource ölçümlere göre kural tanımlayın.                                                                                                                                                                                                     |
    | *Ölçütler*            |                   |                                                                                                                                                                                                                                                                                 |
    | Zaman toplama      | Ortalama           |                                                                                                                                                                                                                                                                                 |
    | Ölçüm adı           | Kapasite          | Kapasite ölçüm, bir Azure API Management örneğinin kaynak kullanımı yansıtan bir Azure API Management unsurdur.                                                                                                                                                            |
    | Zaman dilimi istatistiği  | Ortalama           |                                                                                                                                                                                                                                                                                 |
    | İşleç              | Büyüktür      |                                                                                                                                                                                                                                                                                 |
    | Eşik             | %80               | Ortalama kapasite ölçüm için eşik değer.                                                                                                                                                                                                                                 |
    | Süre (dakika) | 30                | Kapasite ölçüm üzerinden ortalaması alınacak timespan kullanım desenlerini özeldir. Uzun zamandır, daha yumuşak tepki olacaktır - aralıklı olarak ortaya çıkan ani genişleme karar üzerinde daha az etkisi olmayacaktır. Bununla birlikte, Ölçek genişletme tetikleyici de geciktirir. |
    | *Eylem*              |                   |                                                                                                                                                                                                                                                                                 |
    | İşlem             | Sayıyı şu kadar artır: |                                                                                                                                                                                                                                                                                 |
    | Örnek sayısı        | 1                 | Azure API Management örneği tarafından 1 birim ölçeği.                                                                                                                                                                                                                          |
    | Soğuma (dakika)   | 60                | Ölçeği genişletmek Azure API Management hizmeti için en az 20 dakika sürer. Çoğu durumda, bekleme süresinde 60 dakika birçok ölçek çıkarmayı tetikleme öğesinden engeller.                                                                                                  |

8. Tıklayın **Ekle** kuralını kaydetmek için.

    ![Azure İzleyici ölçek genişletme kuralı](media/api-management-howto-autoscale/05.png)

9. Tekrar tıklayarak **+ alınabilecek**.

    Bu kez, bir ölçek kuralında tanımlanması gerekir. API kullanımı azalttığında kaynakları, boşa değil sağlayacaktır.

10. Yeni bir ölçek kuralında tanımlayın.

    Örneğin, son 30 dakika boyunca ortalama kapasite ölçüm 35 %'den daha düşük olduğunda bir ölçek kuralında bir Azure API Management birim kaldırılmasını tetikleyebilir. Aşağıdaki tabloda, böyle bir kural yapılandırmasını sağlar.

    | Parametre             | Değer             | Notlar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    |-----------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Ölçüm kaynağı         | Geçerli kaynak  | Geçerli Azure API Management resource ölçümlere göre kural tanımlayın.                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | *Ölçütler*            |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Zaman toplama      | Ortalama           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Ölçüm adı           | Kapasite          | Ölçek genişletme kuralı için kullanılan aynı ölçümü.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Zaman dilimi istatistiği  | Ortalama           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | İşleç              | Şu değerden az:         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Eşik             | 35%               | Benzer şekilde ölçek genişletme kuralı, bu değer Azure API Management kullanım modellerini yoğun bir şekilde bağlıdır. |
    | Süre (dakika) | 30                | Ölçek genişletme kuralı için kullanılanla aynı değeri.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | *Eylem*              |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | İşlem             | Sayıyı şu kadar azalt: | Ne ters yönde, Ölçek genişletme kuralı için kullanıldı.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Örnek sayısı        | 1                 | Ölçek genişletme kuralı için kullanılanla aynı değeri.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Soğuma (dakika)   | 90                | Bekleme süresinde daha uzun bu nedenle ölçek genişletme, daha fazla koruyucu olmalıdır.                                                                                                                                                                                                                                                                                                                                                                                                    |

11. Tıklayın **Ekle** kuralını kaydetmek için.

    ![Azure İzleyici ölçek kuralı](media/api-management-howto-autoscale/06.png)

12. Ayarlama **maksimum** Azure API Yönetimi birimleri sayısı.

    > [!NOTE]
    > Azure API Management örneği için ölçeğini genişletebilirsiniz birimleri sınırı vardır. Sınır bir hizmet katmanına bağlıdır.

    ![Azure İzleyici ölçek kuralı](media/api-management-howto-autoscale/07.png)

13. **Kaydet**’e tıklayın. Otomatik ölçeklendirme yapılandırıldı.

## <a name="next-steps"></a>Sonraki adımlar

+ [Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md)
