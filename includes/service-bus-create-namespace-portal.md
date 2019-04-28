---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/20/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: a95f5ee5105c45ba9e5b1705e83d60bf24b1dc12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60333586"
---
## <a name="create-a-namespace-in-the-azure-portal"></a>Azure portalında bir ad alanı oluşturma
Azure'da Service Bus mesajlaşma varlıklarını kullanmaya başlamak için öncelikle Azure'da benzersiz olan bir ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1. [Azure portalda](https://portal.azure.com) oturum açma
2. Portalın sol gezinti bölmesinde seçin **+ kaynak Oluştur**seçin **tümleştirme**ve ardından **Service Bus**.

    ![Kaynak Oluştur tümleştirme -> Service Bus ->](./media/service-bus-create-namespace-portal/create-resource-service-bus-menu.png)
3. İçinde **ad alanı oluşturma** iletişim kutusunda, aşağıdaki adımları uygulayın: 
    1. Girin bir **ad alanı adı**. Adın kullanılabilirliği sistem tarafından hemen denetlenir. Namespaces adlandırma kurallarının bir listesi için bkz. [Namespace REST API oluşturma](/rest/api/servicebus/create-namespace).
    2. Ad alanı için fiyatlandırma katmanını (temel, standart veya Premium) seçin. Kullanmak istiyorsanız [konuları ve abonelikleri](../articles/service-bus-messaging/service-bus-queues-topics-subscriptions.md#topics-and-subscriptions), standart veya Premium seçin. Konular/abonelikler, Temel fiyatlandırma katmanında desteklenmez.
    3. Seçtiyseniz **Premium** fiyatlandırma katmanında, şu adımları izleyin: 
        1. Sayısını **Mesajlaşma birimleri**. Premium katmanı, her iş yükü yalıtımlı şekilde çalışır. böylece CPU ve bellek düzeyinde kaynak yalıtımına sunar. Bu kaynak kapsayıcısı Mesajlaşma birimi olarak adlandırılır. Bir premium ad alanı en az bir Mesajlaşma birimi var. 1, 2 veya 4 Mesajlaşma birimi her Service Bus Premium ad alanı için seçebilirsiniz. Daha fazla bilgi için [Service Bus Premium Mesajlaşma](../articles/service-bus-messaging/service-bus-premium-messaging.md).
        2. Ad alanı yapmak isteyip istemediğinizi belirtin **bölgesel olarak yedekli**. Bölge artıklığı kullanılabilirlik bölgelerindeki ek maliyet olmadan tek bir bölgede kopyaların yayılmasını sağlayarak gelişmiş kullanılabilirlik sağlar. Daha fazla bilgi için [Azure kullanılabilirlik alanlarında](../articles/availability-zones/az-overview.md).
    4. İçin **abonelik**, ad alanı oluşturulacağı bir Azure aboneliği seçersiniz.
    5. İçin **kaynak grubu**, hangi ad alanı canlı, veya yeni bir kaynak grubu seçin.      
    6. İçin **konumu**, bölgeyi hangi ad alanınızın barındırılması seçin.
    7. **Oluştur**’u seçin. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika beklemeniz gerekebilir.
   
        ![ad alanı oluşturma](./media/service-bus-create-namespace-portal/create-namespace.png)
4. Service bus ad alanı başarıyla dağıtıldığından emin olun. Bildirimleri görmek için seçin **zil simgesini (Uyarılar)** araç. Seçin **kaynak grubunun adı** görüntüde gösterildiği gibi bildirim. Service bus ad alanı içeren kaynak grubunu görürsünüz.

    ![Dağıtım Uyarısı](./media/service-bus-create-namespace-portal/deployment-alert.png)
5. Üzerinde **kaynak grubu** seçin, kaynak grubunuzun sayfasında, **service bus ad alanı**. 

    ![Kaynak grubu sayfası - service bus ad alanınızı seçin](./media/service-bus-create-namespace-portal/resource-group-select-service-bus.png)
6. Service bus ad alanı için giriş sayfasını görürsünüz. 

    ![Service bus ad alanınız için giriş sayfası](./media/service-bus-create-namespace-portal/service-bus-namespace-home-page.png)

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma 
Yeni bir ad alanı oluşturulduğunda, her biri ad alanının tüm yönleri üzerinde tam denetim veren ilişkili bir çift birincil ve ikincil anahtara sahip bir ilk Paylaşılan Erişim İmzası (SAS) kuralı otomatik olarak oluşturulur. Bkz: [Service Bus kimlik doğrulama ve yetkilendirme](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) ile daha fazla kuralları oluşturma hakkında daha fazla bilgi için kısıtlı haklar sıradan göndericiler ve alıcılar için. Ad alanınız için birincil ve ikincil anahtarları kopyalamak için aşağıdaki adımları izleyin: 

1. **Tüm kaynaklar**’a ve sonra yeni oluşturulan ad alanı adına tıklayın.
2. Ad alanı penceresinde **Paylaşılan erişim ilkeleri**'ne tıklayın.
3. **Paylaşılan erişim ilkeleri** ekranında **RootManageSharedAccessKey** seçeneğine tıklayın.
   
    ![bağlantı bilgisi](./media/service-bus-create-namespace-portal/connection-info.png)
4. İçinde **İlkesi: RootManageSharedAccessKey** penceresi, yanındaki Kopyala düğmesine tıklatarak **PRIMARY CONNECTION Strıng'i**, bağlantı dizesini Panonuza daha sonra kullanmak üzere kopyalayın. Bu değeri Not Defteri veya başka bir geçici konuma yapıştırın.
   
    ![bağlantı dizesi](./media/service-bus-create-namespace-portal/connection-string.png)
5. **Birincil anahtar** değerini daha sonra kullanmak üzere geçici bir konuma kopyalayarak önceki adımı tamamlayın.

<!--Image references-->

