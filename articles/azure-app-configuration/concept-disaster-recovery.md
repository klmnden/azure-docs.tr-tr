---
title: Azure uygulama yapılandırması dayanıklılık ve olağanüstü durum kurtarma | Microsoft Docs
description: Azure uygulama yapılandırması ile dayanıklılık ve olağanüstü durum kurtarma uygulama genel bakış.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 05/29/2019
ms.author: yegu
ms.openlocfilehash: 39e7a62899a7d1d6feb5bfd3b45ad4adc3c996a0
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394030"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Şu anda Azure uygulama yapılandırması bölgesel bir hizmettir. Her yapılandırma deposu, belirli bir Azure bölgesinde oluşturulur. Bölge genelinde kaybı, bu bölgedeki tüm depoları etkiler. Uygulama yapılandırması başka bir bölgeye otomatik yük devretme sunmaz. Bu makalede, nasıl birden çok yapılandırma deposu Azure bölgeleri arasında uygulamanızın coğrafi dayanıklılığı artırmak için kullanabileceğiniz üzerinde genel rehberlik sağlar.

## <a name="high-availability-architecture"></a>Yüksek kullanılabilirlik mimarisi

Bölgeler arası yedeklilik hayata geçirmek için farklı bölgelerde birden çok uygulama yapılandırma deposu oluşturmanız gerekir. Bu kurulum ile en az bir uygulama olacaktır birincil deposuyla üzerine geri dönmesi için ek yapılandırma deposu erişilemez durumda olur. Uygulamanız ve onun birincil ve ikincil yapılandırma depoları topolojisi gösteren bir diyagram aşağıda verilmiştir.

![Coğrafi olarak yedekli depoları](./media/geo-redundant-app-configuration-stores.png)

Uygulama mağazalarından hem birincil ve ikincil paralel yapılandırmasıyla yükler. Bunun yapılması, başarılı bir şekilde önemli ölçüde yapılandırma verilerini alma şansını artırır. Veriler her iki depolarında eşitlenmiş tutmak için sorumlu olursunuz. Aşağıdaki bölümler, coğrafi dayanıklılık uygulamanıza nasıl oluşturabileceğinizi açıklar.

## <a name="failover-between-configuration-stores"></a>Yapılandırma depoları arasında yük devretme

Teknik olarak, uygulamanızın bir yük devretme Yürütülüyor değil. Aynı anda iki uygulama yapılandırma mağazalarından aynı kümesi yapılandırma verilerini almaya çalışıyor. Kodunuzu ilk ikincil Mağaza'dan önce yükler ve ardından birincil depolama şekilde düzenlemelisiniz. Bu yaklaşım, kullanılabilir olduğunda yapılandırma verilerini birincil depolama öncelik kazanır sağlayacaktır. Aşağıdaki kod parçacığında nasıl bu .NET Core uygulayabilirsiniz gösterir.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    }
```

Bildirim `optional` parametresi geçirildi `AddAzureAppConfiguration` işlevi. Ayarlandığında `true`, bu parametre, işlev bir yapılandırma verileri yüklenemiyor devam etmek başarısız olan uygulamanın engeller.

## <a name="synchronization-between-configuration-stores"></a>Yapılandırma depoları arasında eşitleme

Tüm, coğrafi olarak yedekli yapılandırma depoları aynı veri kümesi olması önemlidir. Kullanabileceğiniz **dışarı** ikincil isteğe bağlı olarak birincil depolama alanından verileri kopyalamak, uygulama yapılandırmasında işlevi. Bu işlev Azure portalı ve CLI aracılığıyla kullanılabilir.

Azure portalında aşağıdaki adımları izleyerek başka bir yapılandırma deposuna yönelik bir değişiklik gönderebilirsiniz:

1. Gidin **içeri/dışarı aktarma** sekmesinde **dışarı**seçin **uygulama yapılandırması** olarak **hedef** hizmet, tıklayın  **Kaynak Seç**.

2. Açıldığını yeni bir dikey pencere içinde yukarı, abonelik, kaynak grubu ve kaynak ikincil deponuzun adını belirtin ve ardından **Uygula**.

3. Kullanıcı Arabirimi, ikincil deponuza dışarı aktarmak istediğiniz hangi yapılandırma verilerini seçebilmeniz güncelleştirilir. Varsayılan saat değeri olduğu gibi bırakın ve hem **etiketten** ve **etiket** aynı değere. **Uygula**'ya tıklayın.

4. Tüm yapılandırma değişiklikleri için yukarıdaki adımları yineleyin.

Bu, dışa aktarma işlemi Azure CLI kullanarak otomatikleştirebilirsiniz. Aşağıdaki komutu, bir tek bir yapılandırma değişikliği ikincil birincil depodan dışarı aktarma işlemi gösterilmektedir.

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulamanızın çalışma zamanı sırasında uygulama yapılandırması için coğrafi dayanıklılık sağlamak büyütmek öğrendiniz. Alternatif olarak, derleme veya dağıtım zamanında uygulama yapılandırması yapılandırma verilerinden ekleyebilir. Daha fazla bilgi için [bir CI/CD işlem hattı ile tümleştir](./integrate-ci-cd-pipeline.md).

