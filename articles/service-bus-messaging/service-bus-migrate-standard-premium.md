---
title: Premium katman için mevcut Azure Service Bus standart ad alanları geçirme | Microsoft Docs
description: Mevcut Azure Service Bus standart ad alanları Premium'a geçişini izin vermek için Kılavuzu
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
ms.date: 02/18/2019
ms.author: aschhab
ms.openlocfilehash: 7b153c36e10f1d4e2be2a0cf42f998c31cb6473a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896589"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-premium-tier"></a>Premium katman için mevcut Azure Service Bus standart ad alanları geçirme

Daha önce Azure Service Bus ad alanları yalnızca standart katmanında sunulur. Bu, aktarım hızının düşük olmasını ve geliştirici ortamları için iyileştirilmiş çok kiracılı kurulumları yoktu.

Yakın geçmişte tahmin edilebilir gecikme süresi ve yüksek aktarım hızı ve üretim ortamları için iyileştirilmiştir, sabit bir fiyat karşılığında artan iş hacmi için ad alanı başına ayrılmış kaynakları sunan Premium katmanı sunmak için Azure Service Bus genişletildi gerektiren ek Kurumsal özellikler.

Premium katmanına geçirilecek standart katman ad alanı mevcut etkinleştirir tooling aşağıda.

>[!WARNING]
> Geçiş için Service Bus standart ad alanı olması amaçlanmıştır ***yükseltilmiş*** Premium katmanına.
>
> Geçiş Araçları ***yok*** eski sürüme düşürme destekler.
>[!NOTE]
> Bu geçiş yapılmasını yöneliktir ***yerinde***.
>
> Bu, var olan gönderen ve alıcı uygulamalar herhangi bir kod veya yapılandırma değişiklik gerektirmeyen anlamına gelir.
>
> Var olan bağlantı dizesini otomatik olarak yeni bir premium ad alanına işaret etmesini sağlayacaksınız.
>
> Ayrıca, standart ad alanındaki tüm varlıkları kullanılabilir **üzerinden kopyalanan** geçiş işlemi sırasında Premium ad alanında.
>
>
> Destekliyoruz ***Mesajlaşma birimi başına 1000 varlık*** kaç adet Mesajlaşma Birimi'ne belirlemek üzere bu nedenle Premium üzerinde Lütfen, geçerli bir standart ad alanı üzerinde sahip varlıklar sayısı ile başlatın.

## <a name="migration-steps"></a>Geçiş adımları

>[!IMPORTANT]
> Geçiş işlemi ile ilişkili bazı uyarılar vardır. Hataları olasılıklarını azaltmak yer alan adımların ile tam olarak tanımak için rica ederiz.

Somut adım adım geçiş işlemi aşağıdaki kılavuzlarda ayrıntılı olarak verilmiştir.

Mantıksal adımlar şunlardır:-

1. Yeni bir Premium ad alanı oluşturun.
2. Standart ve Premium ad alanı birbiriyle eşleştirin.
3. Premium ad alanı için eşitleme (üzerinden kopyalama) standart varlıklardan
4. Geçişi tamamlama
5. Geçiş sonrası ad alanı adını kullanarak standart ad alanındaki varlıklar Boşalt
6. Standart ad alanı Sil

>[!NOTE]
> Geçiş taahhüt silindikten sonra eski standart ad alanı erişmek ve kuyrukları ile aboneliklerinden boşaltma son derece önemlidir.
>
> İletileri boşaltılır sonra bunlar yeni premium ad alanı için alıcı uygulamalar tarafından işlenecek gönderilebilir.
>
> Kuyrukları ve aboneliklerinden boşaltılır sonra eski standart ad alanı siliniyor öneririz. Bu gereksinim duyuyor olması gerekmez!

### <a name="migrate-using-azure-cli-or-powershell"></a>Azure CLI veya PowerShell kullanarak geçirme

Service Bus standart ad alanınız için Premium Azure CLI veya PowerShell aracını kullanarak geçirmek için başvurmak aşağıda Kılavuzu.

1. Yeni bir Service Bus Premium ad alanı oluşturun. Başvurabileceğiniz [Azure Resource Manager şablonları](service-bus-resource-manager-namespace.md) veya [Azure portalını](service-bus-create-namespace-portal.md). İçin "Premium" seçtiğinizden emin olun **serviceBusSku** parametresi.

2. Ayarlama aşağıda geçiş komutları basitleştirmek için ortam değişkenleri.
   ```
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the Premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the Standard namespace>
   ```

    >[!IMPORTANT]
    > Geçiş sonrası adı (post_migration_dns_name) eski standart ad alanı geçiş sonrasında erişmek için kullanılır. Bu, kuyrukları ve aboneliklerinden boşaltın ve ardından ad alanını silmek için kullanmalısınız.

3. **Çifti** standart ve Premium ad alanları ve **Eşitlemeyi Başlat** kullanarak aşağıdaki komutu -

    ```
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


4. Geçiş kullanarak durumu denetleyin - komut aşağıda
    ```
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    Geçiş ne zaman tam olarak kabul edilir
    * MigrationState "Etkin" =
    * pendingReplicationsOperationsCount = 0
    * provisioningState "Başarılı" =

    Bu komut ayrıca geçiş yapılandırmasını görüntüler. Lütfen olarak bildirilen değerleri önceki'olarak ayarlandığından emin olmak için kontrol edin.

    Ayrıca, tüm kuyrukları ve konuları oluşturulmuş ve standart ad alanı üzerinde varolan ne eşleştiğinden emin olmak için Portalı'nda Premium ad alanı da denetleyin.

5. Aşağıdaki tam komutu yürüterek geçişini Yürüt
   ```
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-using-azure-portal"></a>Azure portalını kullanarak geçirme

Azure portalı üzerinden geçiş komutlarını kullanarak geçiş olarak aynı mantıksal akışı vardır. Başvurmak aşağıda Portalı'nı kullanarak geçirmek için adım adım işlem kılavuzu.

1. Çekme **'Premium'e geçirme'** sol bölmede Gezinti menüsünde menü seçeneği. Tıklayın **'Başlarken'** düğmesine bir sonraki sayfasına devam edin.
    ![Geçiş giriş sayfası][]

2. Tam **Kurulum**.
   ![Ad alanı Kurulumu][]
   1. Oluşturun ve mevcut standart ad alanına geçirmek için bir Premium ad alanı atayın.
        ![Ad alanı kurulumu - premium ad alanı oluşturma][]
   2. Çekme **'Geçiş sonrasında name'** geçiş tamamlandıktan sonra standart ad alanı tarafından erişmek için.
        ![Ad alanı kurulumu - post geçiş adı seçin][]
   3. Tıklayın **'İleri'** devam etmek için.
3. **Eşitleme** varlıklar arasında standart ve Premium ad alanı.
    ![Ad alanı - eşitleme varlıkları - başlangıç Kurulumu][]

   1. Tıklayın **Başlat'Sync '** varlıkları eşitlemeye başlamak için.
   2. Tıklayın **'Evet'** Eşitlemeyi başlatmak için onaylayan açılır pencere üzerinde.
   3. Bekle **eşitleme** tamamlandı. Durum ve durum çubuğunda kullanılabilir hale getirilir.
        ![Kurulum ad - eşitleme varlıkları - ilerleme durumu][]
        >[!IMPORTANT]
        > Gerekirse **iptal** herhangi bir sebeple Lütfen SSS bölümünü bu belgenin iptal flow'da gözden geçirin.
   4. Eşitleme tamamlandıktan sonra tıklayın **'İleri'** sayfanın alt kısmındaki düğmesi.

4. Özet sayfasında değişiklikleri gözden geçirin.
    ![Geçiş ad alanı - anahtar menüsü][]

5. Tıklayarak **'Geçişi Tamamla'** ad alanları geçmek ve Geçişi tamamlamak için.
    ![Geçiş ad alanı - başarılı][]

## <a name="faqs"></a>SSS

### <a name="what-happens-when-the-migration-is-committed"></a>Geçiş kararlıdır ne olur?

Geçiş kararlıdır sonra standart ad alanına işaret eden bağlantı dizesi Premium ad alanına işaret etmesini sağlayacaksınız.

Gönderen ve alıcı uygulama standart Namespace bağlantısını kesmek ve Premium ad alanına otomatik olarak yeniden.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>Neler standart Premium geçiş tamamlandıktan sonra yapabilirim?

Varlık meta verileri (konular, abonelikler, filtreler, et al.) kopyaladığınız üzerinden standart katmandan Premium ad alanı için Premium geçiş standart sağlar. Standart ad alanına işlendi ileti verileri üzerinde standart katmandan Premium ad alanına kopyalanmaz.

Bu nedenle, standart ad alanı gönderilen ve geçiş işlemi yürütülürken kaydedilmiş bazı iletileri olabilir. Bu iletiler el ile standart Namespace boşaltılır ve gönderilen el ile için Premium Namespace üzerinden gerekir.

Bunu yapmak için ***gerekir*** bir konsol uygulaması kullanıp kullanarak standart ad alanı varlıkları boşaltma betiği **Post geçiş DNS adı** üzerinde belirtilen geçiş komutlar ve ardından gönderin iletileri Premium Namespace, böylece alıcılar tarafından işlenebilir.

İletileri boşaltılır sonra standart ad alanı silmek için lütfen geçin.

>[!IMPORTANT]
> Standart ad alanı iletilerden boşaltılır sonra lütfen unutmayın, **gerekir** standart ad alanı silin.
>
> Bu, başlangıçta standart ad alanı için artık başvurulan bağlantı dizesi aslında Premium ad alanına başvurduğundan önemlidir. Bu standart Namespace artık gerek gerekmez.
>
> Yardımcı geçişi standart ad alanı siliniyor daha sonraki bir tarihte karmaşıklığı azaltır. 

### <a name="how-much-downtime-do-i-expect"></a>Ne kadar kapalı kalma süresi miyim bekliyorsunuz?
Yukarıda açıklanan geçiş işlemi, uygulamalar için beklenen kapalı kalma süresi azaltmak için tasarlanmıştır. Bu, yeni bir Premium ad alanına işaret edecek şekilde gönderen ve alıcı uygulamalarını kullanan bağlantı dizesini kullanarak gerçekleştirilir.

Kapalı kalma süresi uygulama tarafından deneyimli, Premium ad alanına işaret edecek şekilde DNS girişini güncelleştirmek için gereken süreyi sınırlıdır.

Bu olmasını varsayılabilir ***yaklaşık 5 dakika***.

### <a name="do-i-have-to-make-any-configuration-changes-while-performing-the-migration"></a>Geçiş gerçekleştirirken yapılandırma değişiklikleri yapmak zorunda mıyım?
Hayır, bu geçiş yapmak için gereken kod/yapılandırma değişiklik bulunmamaktadır. Gönderen ve alıcı uygulamaların standart Namespace erişmek için kullandığı bağlantı dizesini olarak davranmak üzere otomatik olarak eşlenmiş bir **diğer** Premium Namespace için.

### <a name="what-happens-when-i-abort-the-migration"></a>Geçişi durdurabilir miyim ne olur?
Geçiş 'Durdur' komutunu kullanarak veya Azure Portalı aracılığıyla iptal. 

#### <a name="azure-cli-or-powershell"></a>Azure CLI veya PowerShell

    az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace

#### <a name="azure-portal"></a>Azure portal

![Akış Durdur - eşitleme iptal][]
![iptal akış - tam iptal][]

Geçiş işlemi iptal edildiğinde gerçekten varlıkları (konular, abonelikler ve filtreleri) kopyalama işlemi standart katmandan Premium ad alanına durdurur ve eşleştirme keser.

Bağlantı dizesi **değil** Premium ad alanına işaret edecek şekilde güncelleştirildi. Mevcut uygulamalarınızı geçişi başlatmadan önce olduğu gibi çalışmaya devam edin.

Ancak, bunu **yok** varlıkları Premium ad alanında veya Premium ad silebilirsiniz. Bu, geçiş işlemine tüm ilerlemek değil karar el ile yapılması gerekir.

>[!IMPORTANT]
> Lütfen geçişi durdurun karar verirseniz, böylece kaynaklar için ücret alınmaz geçiş için sağlanan Premium Namespace silin.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>İletileri boşaltma zorunda kalmak istemiyorum. Ne yapmalıyım?

Gönderen uygulama tarafından gönderilen ve geçiş gerçekleşirken depolama alanında standart Namespace taahhüdünde iletileri olabilir ve hemen önce geçiş kararlıdır.

Geçiş sırasında gerçek ileti veri/yükü üzerinden standart katmandan Premium kopyalanmaz düşünüldüğünde, bu el ile boşaltılır ve Premium ad alanına gönderilmesi gerekir.

Planlı bakım/temizlik penceresi sırasında geçirebilirsiniz ve el ile boşaltabilir ve ileti göndermek istemiyorsanız, ancak izleme Lütfen aşağıdaki adımları -

1. Gönderen uygulamaları durdurun ve şu anda standart ad alanında ve boşaltma kuyruğu iletileri işlemek üzere alıcılar sağlar.
2. Kuyruklar ve standart Namespace aboneliklerinde boş olduğunda, geçiş standart katmandan Premium ad alanına yürütmek için yukarıda açıklanan yordamı izleyin.
3. Geçiş işlemi tamamlandıktan sonra gönderici uygulama yeniden başlatabilirsiniz.
4. Göndericiler ve alıcılar artık otomatik olarak Premium ad alanı ile bağlanır.

    >[!NOTE]
    > Alıcı, geçiş için durdurulmuş.
    >
    > Geçiş işlemi tamamlandıktan sonra alıcılar standart ad alanından bağlantısını kesmek ve otomatik olarak Premium ad alanınıza bağlanın.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [standart ve Premium Mesajlaşma arasındaki farklar](./service-bus-premium-messaging.md)
* Yüksek kullanılabilirlik ve coğrafi Diaster kurtarma yönleri hakkında bilgi edinmek için Service Bus Premium [burada](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium)

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
