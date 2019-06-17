---
title: Mevcut Azure Service Bus standart ad alanları premium katmanına geçirme | Microsoft Docs
description: Azure Service Bus standart ad alanları mevcut Premium'a geçiş izin vermek için Kılavuzu
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: darosa
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2019
ms.author: aschhab
ms.openlocfilehash: 65c207b4d03e7d156c8c871a3642601fd0489ead
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991410"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-the-premium-tier"></a>Mevcut Azure Service Bus standart ad alanları premium katmanına geçirme
Daha önce Azure Service Bus ad alanları yalnızca standart katmanında sunulur. Ad alanları, aktarım hızının düşük olmasını ve geliştirici ortamları için iyileştirilen çok kiracılı kurulumları ' dir. Premium katmanı, tahmin edilebilir gecikme süresi ve sabit bir fiyat karşılığında artan iş hacmi için ad alanı başına ayrılmış kaynaklar sunar. Premium katmanı, yüksek aktarım hızı ve ek Kurumsal özellikleri gerektiren üretim ortamları için optimize edilmiştir.

Bu makalede, var olan bir standart katman ad alanı premium katmanına geçirmek açıklar.  

>[!WARNING]
> Geçiş, premium katmanına yükseltme Service Bus standart ad alanları için tasarlanmıştır. Geçiş Aracı, eski sürüme düşürmeyi desteklemeyen.

Bazı dikkat edilecek noktaları: 
- Bu geçiş, yani, mevcut yerinde olmasını yöneliktir gönderen ve alıcı uygulamaların **kod veya yapılandırma değişiklikleri gerektirmeyen**. Var olan bağlantı dizesini otomatik olarak yeni bir premium ad alanına işaret etmesini sağlayacaksınız.
- **Premium** ad olmalıdır **varlık** da başarılı olması geçiş için. 
- Tüm **varlıkları** standart ad alanında **kopyalanan** geçiş işlemi sırasında premium ad alanı. 
- Geçiş destekler **Mesajlaşma birimi başına 1.000 varlıkları** premium katmanı. Gereksinim duyduğunuz kaç Mesajlaşma birimi tanımlamak için geçerli bir standart ad alanı üzerinde sahip varlıklar sayısı ile başlayın. 
- Doğrudan geçiş yapamazsınız **temel katman** için **premier katmanı**, ancak bu nedenle dolaylı olarak temel veya standart ilk ve ardından sonraki adımda premium standart geçiş yaparak bunu yapabilirsiniz.

## <a name="migration-steps"></a>Geçiş adımları
Bazı koşullar, geçiş işlemi ile ilişkilidir. Hata olasılığını azaltmak için aşağıdaki adımları hakkında bilgi edinin. Bu adımlar, geçiş işlemi özetlemektedir ve adım adım Ayrıntılar aşağıdaki bölümlerde listelenmiştir.

1. Yeni bir premium ad alanı oluşturun.
1. Standart ve premium ad alanları birbirine eşleştirin.
1. Standart varlıklardan eşitleme (kopya üzerinden) için bir premium ad alanı.
1. Geçiş tamamlama.
1. Geçiş sonrası ad alanı adını kullanarak standart ad alanındaki varlıklar boşaltın.
1. Standart ad alanı silin.

>[!IMPORTANT]
> Geçiş kaydedildi sonra eski standart ad alanı erişmek ve kuyrukları ile aboneliklerinden boşaltın. İletileri boşaltılır sonra bunlar için yeni premium ad alanı alıcı uygulamaları tarafından işlenecek gönderilebilir. Kuyrukları ve aboneliklerinden boşaltılır sonra eski standart ad alanı silmenizi öneririz.

### <a name="migrate-by-using-the-azure-cli-or-powershell"></a>Azure CLI veya PowerShell kullanarak geçirme

Azure CLI veya PowerShell aracını kullanarak, Service Bus standart ad alanı için premium geçirmek için aşağıdaki adımları izleyin.

1. Yeni bir Service Bus premium ad alanı oluşturun. Başvurabileceğiniz [Azure Resource Manager şablonları](service-bus-resource-manager-namespace.md) veya [Azure portalını](service-bus-create-namespace-portal.md). Seçtiğinizden emin olun **premium** için **serviceBusSku** parametresi.

1. Geçiş komutları basitleştirmek için aşağıdaki ortam değişkenlerini ayarlayın.
   ```azurecli
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the standard namespace>
   ```

    >[!IMPORTANT]
    > Geçiş sonrası diğer/ad (post_migration_dns_name) eski standart ad alanı geçiş sonrasında erişmek için kullanılır. Kuyrukları ve aboneliklerinden boşaltma için bunu kullanın ve ardından ad alanı silin.

1. Standart ve premium ad alanları eşleştirebilir ve aşağıdaki komutu kullanarak bir eşitleme başlatın:

    ```azurecli
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


1. Aşağıdaki komutu kullanarak geçiş durumunu denetleyin:
    ```azurecli
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    Geçiş, aşağıdaki değerleri gördüğünüzde tamamlanmış olarak değerlendirilir:
    * MigrationState "Etkin" =
    * pendingReplicationsOperationsCount = 0
    * provisioningState "Başarılı" =

    Bu komut ayrıca geçiş yapılandırmasını görüntüler. Değerlerin doğru ayarlandığından emin olun. Ayrıca, premium ad alanındaki tüm kuyrukları ve konuları oluşturulmuş ve standart ad alanında varolduğunu ne eşleştiğinden emin olmak için portalı denetleyin.

1. Aşağıdaki tam komutu yürüterek geçişini Yürüt:
   ```azurecli
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-by-using-the-azure-portal"></a>Azure portalını kullanarak geçirme

Azure portalını kullanarak geçiş komutları kullanarak geçiş olarak aynı mantıksal akışı vardır. Azure portalını kullanarak geçirmek için aşağıdaki adımları izleyin.

1. Üzerinde **Gezinti** sol bölmesinde, menüde **premium geçiş**. Tıklayın **Başlarken** düğmesine bir sonraki sayfasına devam edin.
    ![Geçiş giriş sayfası][]

1. Tam **Kurulum**.
   ![Ad alanı Kurulumu][]
   1. Oluşturun ve mevcut standart ad alanına geçirmek için bir premium ad alanı atayın.
        ![Ad alanı kurulumu - premium ad alanı oluşturma][]
   1. Seçin bir **geçiş sonrasında adı**. Geçiş tamamlandıktan sonra standart ad alanı erişmek için bu adı kullanacaksınız.
        ![Ad alanı kurulumu - post geçiş adı seçin][]
   1. Seçin **'İleri'** devam etmek için.
1. Varlıklar, standart ve premium ad alanları arasında eşitleyin.
    ![Ad alanı - eşitleme varlıkları - başlangıç Kurulumu][]

   1. Seçin **Eşitlemeyi Başlat** varlıkları eşitlemeye başlamak için.
   1. Seçin **Evet** onaylayın ve Eşitlemeyi Başlat iletişim kutusundaki.
   1. Eşitleme işlemi tamamlanana kadar bekleyin. Durum ve durum çubuğunda kullanılabilir.
        ![Kurulum ad - eşitleme varlıkları - ilerleme durumu][]
        >[!IMPORTANT]
        > Geçiş için herhangi bir nedenle iptal etmek ihtiyacınız varsa lütfen SSS bölümünü bu belgenin iptal flow'da gözden geçirin.
   1. Eşitleme tamamlandıktan sonra seçin **sonraki** sayfanın alt kısmındaki.

1. Özet sayfasında değişiklikleri gözden geçirin. Seçin **geçişi Tamamla** ad alanları geçmek ve Geçişi tamamlamak için.
    ![Geçiş ad alanı - anahtar menü][] geçiş tamamlandığında onay sayfası görüntülenir.
    ![Geçiş ad alanı - başarılı][]

## <a name="faqs"></a>SSS

### <a name="what-happens-when-the-migration-is-committed"></a>Geçiş kararlıdır ne olur?

Geçiş kararlıdır sonra standart ad alanına işaret eden bağlantı dizesi premium ad alanına işaret etmesini sağlayacaksınız.

Gönderen ve alıcı uygulama standart Namespace bağlantısını kesmek ve premium ad alanına otomatik olarak yeniden.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Neler standart premium geçiş tamamlandıktan sonra yapabilirim?

Premium geçiş standart varlık meta verileri konu başlıkları, abonelikler ve filtreleri gibi kopyalanır, standart ad alanından premium ad alanına sağlar. Standart ad alanına işlendi ileti verileri standart ad alanından premium ad alanına kopyalanır değil.

Standart ad alanı, gönderilen ve geçiş işlemi yürütülürken kaydedilmiş bazı iletileri olabilir. Standart Namespace ileti el ile boşaltabilir ve el ile premium Namespace gönderin. İletileri el ile boşaltabilir için bir konsol uygulaması veya geçiş komutlar belirtilen posta geçiş DNS adını kullanarak standart ad alanı varlıkları boşaltır bir betik kullanın. Bu iletiler alıcılar tarafından işlenebilir premium ad alanına gönderin.

İletileri boşaltılır sonra standart ad alanı silin.

>[!IMPORTANT]
> Standart ad alanı iletilerden boşaltılır sonra standart ad alanı silin. Bu, başlangıçta standart ad alanı için artık başvurulan bağlantı dizesi premium ad alanına başvurduğundan önemlidir. Standart Namespace artık gerekmez. Geçirdiğiniz standart ad alanı siliniyor, sonraki karışıklık azaltmaya yardımcı olur.

### <a name="how-much-downtime-do-i-expect"></a>Ne kadar kapalı kalma süresi miyim bekliyorsunuz?
Geçiş işlemi, uygulamalar için beklenen kapalı kalma süresi azaltmak için tasarlanmıştır. Yeni premium ad alanına işaret edecek şekilde gönderen ve alıcı uygulamalarını kullanan bağlantı dizesi kullanarak kapalı kalma süresi azalır.

Uygulama tarafından yaşadı kapalı kalma süresi, premium ad alanına işaret edecek şekilde DNS girişini güncelleştirmek için gereken süreyi sınırlıdır. Kapalı kalma süresi yaklaşık 5 dakikadır.

### <a name="do-i-have-to-make-any-configuration-changes-while-doing-the-migration"></a>Geçiş yaparken yapılandırma değişiklikleri yapmak zorunda mıyım?
Hayır, geçiş yapmak için gereken kod veya yapılandırma değişiklik bulunmamaktadır. Gönderen ve alıcı uygulamaların standart Namespace erişmek için kullandığı bağlantı dizesini, premium ad alanı için bir diğer ad olarak davranmak üzere otomatik olarak eşlenir.

### <a name="what-happens-when-i-abort-the-migration"></a>Geçişi durdurabilir miyim ne olur?
Geçiş kullanarak durdurulmasına `Abort` Azure portalı kullanarak veya komutu. 

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace
```

#### <a name="azure-portal"></a>Azure portal

![Akış Durdur - eşitleme iptal][]
![iptal akış - tam iptal][]

Geçiş işlemi iptal edildiğinde, varlıkları (konular, abonelikler ve filtreleri) standart premium ad alanına kopyalama işlemi durdurur ve eşleştirme keser.

Bağlantı dizesi, premium ad alanına işaret edecek şekilde güncelleştirilmez. Mevcut uygulamalarınızı geçişi başlatmadan önce olduğu gibi çalışmaya devam edin.

Ancak, bunu değil varlıkları premium ad alanında veya bir premium ad alanı silebilirsiniz. Geçişe devam değil karar verdiyseniz varlıkları el ile silin.

>[!IMPORTANT]
> Geçiş iptal etmek karar verirseniz, geçiş için sağlanan ve böylece kaynaklar için ücretlendirilmez Namespace premium silin.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>İletileri boşaltma zorunda kalmak istemiyorum. Ne yapmalıyım?

Standart Namespace depolama alanı geçişi gerçekleşirken ve yalnızca geçiş işlemi gerçekleştirilmeden önce kaydedilmiş ve gönderen uygulama tarafından gönderilen iletileri olabilir.

Geçiş sırasında gerçek ileti veri/yükü standart katmandan premium ad alanına kopyalanmıyor. İletileri el ile boşaltılır ve premium ad alanına gönderilmesi gerekir.

Ancak, planlı bakım/temizlik penceresi sırasında geçirebileceğiniz ve el ile boşaltabilir ve ileti göndermek istemiyorsanız, aşağıdaki adımları izleyin:

1. Gönderen uygulamaları durdurun. Alıcı uygulamaları şu anda standart ad alanında ve kuyruğu boşaltacak iletileri işler.
1. Kuyruklar ve standart Namespace aboneliklerinde boş olduktan sonra geçiş standart premium ad alanına yürütmek için daha önce açıklanan yordamı izleyin.
1. Geçiş tamamlandıktan sonra gönderici uygulama yeniden başlatabilirsiniz.
1. Göndericiler ve alıcılar artık otomatik olarak premium ad alanı ile bağlanır.

    >[!NOTE]
    > Geçiş için alıcı uygulamalarını durdurmanız gerekmez.
    >
    > Geçiş tamamlandıktan sonra alıcı uygulamaların standart ad alanından bağlantısını kesmek ve otomatik olarak premium ad alanınıza bağlanın.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [standart ve premium Mesajlaşma arasındaki farklar](./service-bus-premium-messaging.md).
* Hakkında bilgi edinin [Service Bus premium için yüksek kullanılabilirlik ve coğrafi olağanüstü durum kurtarma özelliklerini](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium).

[Geçiş giriş sayfası]: ./media/service-bus-standard-premium-migration/1.png
[Ad alanı Kurulumu]: ./media/service-bus-standard-premium-migration/2.png
[Ad alanı kurulumu - premium ad alanı oluşturma]: ./media/service-bus-standard-premium-migration/3.png
[Ad alanı kurulumu - post geçiş adı seçin]: ./media/service-bus-standard-premium-migration/4.png
[Ad alanı - eşitleme varlıkları - başlangıç Kurulumu]: ./media/service-bus-standard-premium-migration/5.png
[Kurulum ad - eşitleme varlıkları - ilerleme durumu]: ./media/service-bus-standard-premium-migration/8.png
[Geçiş ad alanı - anahtar menüsü]: ./media/service-bus-standard-premium-migration/9.png
[Geçiş ad alanı - başarılı]: ./media/service-bus-standard-premium-migration/12.png

[Akış Durdur - eşitleme Durdur]: ./media/service-bus-standard-premium-migration/abort1.png
[Akış Durdur - tam Durdur]: ./media/service-bus-standard-premium-migration/abort3.png
