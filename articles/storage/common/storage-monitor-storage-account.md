---
title: Bir Azure depolama hesabı izleme | Microsoft Docs
description: Azure portalı kullanarak bir Azure depolama hesabında izlemek öğrenin.
services: storage
documentationcenter: ''
author: tamram
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: tamram
ms.openlocfilehash: ffc7d46bbfa4db47a47e416c395efdfc451cadc1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Azure portalında bir depolama hesabını izleme

[Azure Storage Analytics](../storage-analytics.md) tabloları ve tüm depolama hizmetleri ve günlükleri BLOB, kuyruklar, ölçümleri sağlar. Kullanabileceğiniz [Azure portal](https://portal.azure.com) hesabınız için kaydedilen hangi ölçümleri ve günlükleri yapılandırmak ve ölçümleri verilerinizi görsel gösterimi sağlamak grafikleri yapılandırmak için.

> [!NOTE]
> Azure portalında izleme verileri inceleniyor ile ilişkili maliyetleri vardır. Daha fazla bilgi için bkz: [depolama çözümlemeleri ve faturalama](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Azure dosyaları şu anda Storage Analytics ölçümleri destekliyor, ancak henüz günlük kaydını desteklemiyor.
> 
> Ayrıntılı bir kılavuz tanımlamak, tanılama ve Azure Storage ile ilgili sorunları gidermek için depolama çözümlemeleri ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve Microsoft Azure Storage sorun giderme](../storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Bir depolama hesabı için izlemeyi Yapılandır

1. İçinde [Azure portal](https://portal.azure.com)seçin **depolama hesapları**, hesap panosunu açmak için sonra depolama hesabı adı.
1. Seçin **tanılama** içinde **izleme** menü dikey bölüm.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Seçin **türü** her ölçümleri verilerin **hizmet** , izlemek istediğiniz ve **Bekletme İlkesi** veriler için. Ayrıca ayarlayarak izleme devre dışı bırakabilirsiniz **durum** için **devre dışı**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   Her hizmet için etkinleştirebilirsiniz ölçümleri her ikisi de yeni depolama hesapları için varsayılan olarak etkinleştirilen iki tür vardır:

   * **Birleşik**: giriş/çıkış, kullanılabilirlik, gecikme ve başarı yüzdeleri gibi ölçümleri toplar. Bu ölçümler; blob, kuyruk, tablo ve dosya hizmetleri için toplanır.
   * **API başına**: Birleşik ölçümlerin yanı sıra, Azure depolama hizmeti API'si her depolama işlem ölçümlerini aynı kümesini toplar.

   Veri Bekletme İlkesi ayarlamak üzere taşıma **bekletme (gün)** kaydırıcı veya 1 ile 365 arasında korumak için veri gün sayısı girin. Yeni depolama hesapları için varsayılan yedi gündür. Bir bekletme ilkesi ayarlamak istemiyorsanız sıfır girin. Bekletme İlkesi yok ise, izleme verilerini silmek için size bağlıdır.

   > [!WARNING]
   > Ölçüm verilerini el ile sildiğinizde ücretlendirilirsiniz. Eski analiz verileri (veri bekletme ilkeniz eski) herhangi bir ücret ödemeden sistem tarafından silinir. Ne kadar süreyle, depolama hesabınız için analiz verileri korumak istediğinize bağlı bir bekletme ilkesi ayarı öneririz. Bkz: [ne yapmak tabi storage ölçümleri etkinleştirdiğinizde ücretlendirilen?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) daha fazla bilgi için.
   >

1. İzleme yapılandırmasını tamamladığınızda, seçin **kaydetmek**.

Ölçümleri varsayılan bir bireysel hizmet Kanatlar (blob, kuyruk, tablo ve dosya) yanı sıra depolama hesabı dikey grafiklerinde görüntülenir. Bir hizmet için ölçümleri etkinleştirdikten sonra kendi grafikte görüntülenecek veri bir saate kadar sürebilir. Seçebileceğiniz **Düzenle** ölçüm herhangi grafik [hangi ölçümlerini yapılandırın](#how-to-customize-metrics-charts) grafikte görüntülenir.

Ölçümleri toplama ve günlüğe kaydetme ayarlayarak devre dışı bırakabilirsiniz **durum** için **devre dışı**.

> [!NOTE]
> Azure depolama kullanan [tablo depolama](../common/storage-introduction.md#table-storage) hesabınızda tablolardaki depolama hesabı ve depoları ölçümler için ölçümler depolamak için. Daha fazla bilgi için bkz. [Ölçümleri nasıl depolandığını](../common/storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Ölçümler grafiklerde özelleştirme

Ölçümleri grafik görüntülemek için hangi depolama ölçümleri seçmek için aşağıdaki yordamı kullanın. 

1. Azure portalında bir depolama ölçüm grafik görüntüleyerek başlatın. Grafikler bulabilirsiniz **depolama hesabı dikey** ve **ölçümleri** dikey bir bireysel hizmet (blob, kuyruk, tablo, dosya).

   Bu örnekte, biz kasasındaki aşağıdaki grafikte çalışmak **depolama hesabı dikey**:

   ![Azure portalında grafiği seçimi](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Ardından, açmak için grafik içinde herhangi bir yere tıklayın **ölçüm** dikey. Seçin **grafiği Düzenle** açmak için **grafiği Düzenle** dikey.

   ![Grafik düğmesini grafik dikey Düzenle](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. Üzerinde **grafiği Düzenle** dikey penceresinde, select **zaman aralığı** grafikte görüntülenecek ölçümleri ve **hizmet** (blob, kuyruk, tablo, dosya) görüntülemek istediğiniz, ölçümleri. Burada size blob hizmeti için geçen hafta ölçümlerini görüntülemek için seçtiğiniz:

   ![Grafiği Düzenle dikey penceresinde zaman aralığı ve hizmet seçimi](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Tek tek seçin **ölçümleri** gibi grafikte görüntülenen, ardından **Tamam**. Örneğin, burada biz görüntülemek seçtiğiniz *ContainerCount* ve *ObjectCount* ölçümleri:

   ![Grafiği Düzenle dikey tek tek ölçüm seçim](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

Koleksiyon, toplama veya izleme verilerinin yalnızca ölçüm verilerinin görüntülenmesi depolama hesabındaki depolama grafik ayarlarınızı etkilemez.

### <a name="metrics-availability-in-charts"></a>Ölçümleri kullanılabilirlik grafiklerinde

Düzenlediğiniz açılır ve grafiğin birim türünü seçtiğiniz hangi hizmet üzerinde temel ölçümler değişiklikleri listesi. Örneğin, yüzde ölçümleri gibi seçebilirsiniz *PercentNetworkError* ve *PercentThrottlingError* yalnızca birimleri yüzde olarak gösteren bir grafik düzenliyorsanız:

![İstek hata yüzdesi grafik Azure portalında](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Ölçümleri çözümleme

Tanılama'seçilen ölçümleri hesabınız için kullanılabilir ölçümleri çözünürlüğünü belirler:

* **Birleşik** izleme giriş/çıkış, kullanılabilirlik, gecikme ve başarı yüzdeleri gibi ölçümleri sağlar. Bu ölçümler blob, tablo, dosya ve kuyruk Hizmetleri toplanır.
* **API başına** yüksekse çözümleme tek depolama işlemleri için kullanılabilir ölçümleri Ayrıca hizmet düzeyi toplamalar sağlar.

## <a name="configure-metrics-alerts"></a>Ölçümleri uyarılarını yapılandırma

Depolama kaynak ölçümleri için eşiklerine olduğunda sizi bilgilendirmek üzere uyarılar oluşturabilirsiniz.

1. Açmak için **uyarı kuralları dikey**, aşağı kaydırarak **izleme** bölümünü **menü dikey** seçip **uyarı kuralları**.
1. Seçin **uyarı Ekle** açmak için **uyarı kuralı eklemek** dikey penceresi
1. Seçin bir **kaynak** (blob, dosya, kuyruk, tablo) açılan gelen ve girin bir **adı** ve **açıklama** , yeni bir uyarı kuralı.
1. Seçin **ölçüm** , bir uyarı, bir uyarı eklemek istediğiniz için **koşulu**ve bir **eşik**. Eşik birim türü seçtiğiniz ölçüm bağlı olarak değiştirir. Örneğin, "count" birim türü olduğundan *ContainerCount*, while birimini *PercentNetworkError* ölçümüdür yüzdesi.
1. Seçin **süresi**. Ulaşmak ya da süresi içinde eşiğini aşan ölçümleri bir uyarı tetikler.
1. (İsteğe bağlı) Yapılandırma **e-posta** ve **Web kancası** bildirimleri. Web kancası hakkında daha fazla bilgi için bkz: [bir Web kancası Azure ölçüm uyarıyı yapılandırmak](../../monitoring-and-diagnostics/insights-webhooks-alerts.md). E-posta veya Web kancası bildirimleri yapılandırmazsanız, uyarıları yalnızca Azure Portalı'nda görüntülenir.

!['Bir uyarı kuralı Ekle' dikey Azure portalında](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Portal panosuna ölçümleri grafikler ekleyin

Portalı panonuza herhangi bir depolama hesaplarınızı için Azure Storage ölçümleri grafikleri ekleyebilirsiniz.

1. Select tıklatın **düzenleme Pano** Panonuzda görüntülerken [Azure portal](https://portal.azure.com).
1. İçinde **döşeme galeri**seçin **Bul bölmeleri tarafından** > **türü**.
1. Seçin **türü** > **depolama hesapları**.
1. İçinde **kaynakları**, ölçümleri panoya eklemek istediğiniz depolama hesabını seçin.
1. Seçin **kategorileri** > **izleme**.
1. Sürükle ve bırak grafik döşeme panonuz görüntülenmesini istediğiniz ölçümü için üzerine. Panoda görüntülenen tüm ölçümleri istediğiniz için yineleyin. Aşağıdaki görüntüde, örnek olarak "BLOB'lar - toplam istek" grafik vurgulanmış, ancak tüm grafikler panonuz yerleştirme için kullanılabilir.

   ![Azure portalında döşeme Galerisi](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Seçin **özelleştirme Bitti** üstüne yakın panonun ekleme grafikleri bittiğinde.

Panonuza grafikleri ekledikten sonra daha fazla bunları açıklandığı gibi özelleştirebilirsiniz [ölçümler grafiklerde özelleştirme](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Tanılama günlüklerini okuma kaydetmek yazma ve silme istekleri blob, tablo ve kuyruk Hizmetleri için Azure Storage söyleyebilirsiniz. Ayarladığınız veri bekletme ilkesi, bu günlükler için de geçerlidir.

> [!NOTE]
> Azure dosyaları şu anda Storage Analytics ölçümleri destekliyor, ancak henüz günlük kaydını desteklemiyor.
>

1. İçinde [Azure portal](https://portal.azure.com)seçin **depolama hesapları**, ardından depolama hesabı dikey penceresini açmak için depolama hesabının adı.
1. Seçin **tanılama** içinde **izleme** menü dikey bölüm.

    ![Azure portalında tanılama menü öğesi izleme altında.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Sağlamak **durum** ayarlanır **üzerinde**seçip **Hizmetleri** , günlüğü etkinleştirmek istediğiniz için.

    ![Azure portalında oturum açmayı yapılandırın.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. **Kaydet**’e tıklayın.

Tanılama günlüklerini $logs depolama hesabınızdaki adlı blob kapsayıcısında kaydedilir. Bir Depolama Gezgini gibi kullanarak günlük verilerini görüntüleyebilir [Microsoft Storage Gezgini](http://storageexplorer.com), veya program aracılığıyla depolama istemci kitaplığı veya PowerShell kullanarak.

$Logs kapsayıcı erişme hakkında daha fazla bilgi için bkz: [depolama günlüğünü etkinleştirme ve erişim günlüğü verilerini](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Sonraki adımlar

* İlgili diğer ayrıntıları bulmak [günlüğe kaydetme ve faturalama ölçümleri](../storage-analytics.md) depolama çözümlemeleri için.
* [Azure Storage ölçümleri ve görünüm ölçüm verilerini etkinleştirme](../storage-enable-and-view-metrics.md) PowerShell kullanarak ve C# ile programlı olarak.
