---
title: Bir Azure depolama hesabı izleme | Microsoft Docs
description: Azure portalını kullanarak bir Azure depolama hesabında izleme hakkında bilgi edinin.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 07/31/2018
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 5a28d69ae5ba9f3b7eeb28b6824ad9a458832bb3
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153627"
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Azure portalında depolama hesabı izleme

[Azure depolama analizi](storage-analytics.md) tabloları ve depolama hizmetleri ve tüm bloblar, kuyruklar için günlükleri ölçümleri sağlar. Kullanabileceğiniz [Azure portalında](https://portal.azure.com) hangi ölçümlerini ve günlüklerini hesabınız için kaydedilen yapılandırma ve ölçüler verilerinizi görsel temsilini sağlamak grafikleri yapılandırın.

> [!NOTE]
> Azure portalında izleme verilerini İnceleme ile ilişkili maliyetler yoktur. Daha fazla bilgi için [depolama analizi](storage-analytics.md).
>
> Azure dosyaları şu anda Storage Analytics ölçümleri destekliyor, ancak henüz günlük desteklemiyor.
>
> Kapsamlı bir kılavuz tanımlamak, tanılamak ve Azure depolama ile ilgili sorunları gidermek için depolama analizi ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Bir depolama hesabı için izlemeyi yapılandırma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **depolama hesapları**, hesap panoyu açmak için daha sonra depolama hesabı adı.
1. Seçin **tanılama** içinde **izleme** menü dikey bölümü.

    ![MonitoringOptions](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)

1. Seçin **türü** ölçümleri veri her **hizmet** izlemek istediğiniz ve **Bekletme İlkesi** veriler için. Ayrıca izleme ayarı devre dışı bırakabilirsiniz **durumu** için **kapalı**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/storage-enable-metrics-01.png)

   Veri Bekletme İlkesi ayarlamak için taşıma **bekletme (gün)** kaydırıcı veya 1 ile 365 arasında korumak için veri gün sayısını girin. Yeni depolama hesapları için varsayılan yedi gündür. Bir bekletme ilkesi ayarlamak istemiyorsanız sıfır girin. Bir bekletme ilkesi varsa, izleme verilerini silmek için size bağlıdır.

   > [!WARNING]
   > Ölçüm verilerini el ile sildiğinizde ücretlendirilir. Eski analiz verilerini (bekletme ilkesini eski veriler), hiçbir ücret ödemeden sistem tarafından silinir. Ne kadar süre, depolama hesabınız için analiz verilerini korumak istediğinize bağlı olarak bir saklama ilkesini belirlemeden öneririz. Bkz: [depolama ölçümleri faturalama](storage-analytics-metrics.md#billing-on-storage-metrics) daha fazla bilgi için.
   >

1. İzleme yapılandırmasını bitirdikten sonra seçin **Kaydet**.

Ölçümleri varsayılan kümesini grafiklerinde bireysel hizmet dikey pencereleri (blob, kuyruk, tablo ve dosya) yanı sıra depolama hesabı dikey penceresi görüntülenir. Bir hizmet için ölçümleri etkinleştirdikten sonra verilerin, grafikte görünmesi bir saate kadar sürebilir. Seçebileceğiniz **Düzenle** hangi ölçümleri grafiğinde görüntülenen yapılandırmak için bir ölçüm grafiği.

Ölçümleri toplama ve günlüğe kaydetme ayarı devre dışı bırakabilirsiniz **durumu** için **kapalı**.

> [!NOTE]
> Azure depolama kullanan [tablo depolama](storage-introduction.md#table-storage) hesabınızdaki tablolarda depolama hesabı ve depolar ölçümler için ölçümleri depolamak için. Daha fazla bilgi için bkz. [Ölçümleri nasıl depolandığını](storage-analytics-metrics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Ölçüm grafikleri özelleştirme

İçinde bir ölçüm grafiği görüntülemek için hangi depolama ölçümleri seçmek için aşağıdaki yordamı kullanın.

1. Azure portalında depolama ölçüm grafiğini görüntüleyerek başlatın. Grafikler bulabilirsiniz **depolama hesabı dikey** ve **ölçümleri** dikey bir bireysel hizmet (blob, kuyruk, tablo, dosya).

   Bu örnekte, görünen aşağıdaki grafikte kullanan **depolama hesabı dikey**:

   ![Azure portalında hesap seçimi](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Grafiği düzenlemek için grafik içinde herhangi bir yere tıklayın.

1. Ardından, **zaman aralığı** grafikte görüntülenecek bir ölçüm ve **hizmet** (blob, kuyruk, tablo, dosya), görüntülemek istediğiniz, ölçümleri. Burada, geçen hafta ölçümleri, blob hizmeti için görüntülenecek seçilir:

   ![Grafiği Düzenle dikey penceresinde zaman aralığı ve hizmet seçimi](./media/storage-monitor-storage-account/storage-edit-metric-time-range.png)

1. Tek tek seçin **ölçümleri** grafikte görüntülenen gibi ardından **Tamam**.

   ![Grafiği Düzenle dikey penceresinde tek tek ölçüm seçimi](./media/storage-monitor-storage-account/storage-edit-metric-selections.png)

Koleksiyon, toplama veya izleme verilerinin depolama hesabındaki depolama hesap ayarlarınızı etkilemez.

### <a name="metrics-availability-in-charts"></a>Grafiklerde ölçümleri kullanılabilirlik

Düzenlediğiniz seçtiğinize açılır ve grafik birim türünü hangi hizmet üzerinde temel kullanılabilir ölçümler değişikliklerin listesi. Örneğin, yüzde ölçümler gibi seçebilirsiniz *Percentnetworkerror'da* ve *Percentthrottlingerror'da* birimleri yüzdesini gösteren bir grafik düzenliyorsanız:

![Azure portalında isteği hatası yüzdesi grafiği](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Ölçümleri çözümleme

Ölçümler, seçtiğiniz **tanılama** hesabınız için mevcut olan ölçümler çözünürlüğünü belirler:

* **Toplama** izleme giriş/çıkış, kullanılabilirlik, gecikme süresi ve başarı yüzdeleri gibi ölçümleri sağlar. Blob, tablo, dosya ve kuyruk Hizmetleri Bu ölçümler toplanır.
* **API başına** daha hassas çözüm, depolama işlemleri için mevcut olan ölçümler ile Ayrıca hizmet düzeyi aggregates sağlar.

## <a name="configure-metrics-alerts"></a>Ölçüm uyarıları yapılandırma

Depolama kaynak ölçümleri için eşikler üst sınırına ulaştınız, bilgilendirmek üzere uyarılar oluşturabilirsiniz.

1. Açmak için **uyarı kuralları dikey**, ekranı aşağı kaydırarak **izleme** bölümünü **menü dikey** seçip **uyarılar (Klasik)**.
2. Seçin **ölçüm uyarısı Ekle (Klasik)** açmak için **bir uyarı kuralı Ekle** dikey penceresi
3. Girin bir **adı** ve **açıklama** , yeni bir uyarı kuralı.
4. Seçin **ölçüm** , uyarı, bir uyarı eklemek istediğiniz için **koşul**ve **eşiği**. Eşik birim türü seçtiğiniz ölçüm bağlı olarak değiştirir. Örneğin, ölçü türü için "count" olan *ContainerCount*, while birimini *Percentnetworkerror'da* ölçüm yüzdesidir.
5. Seçin **süresi**. Ulaşın veya dönem içinde eşiğini aşan ölçümleri uyarı tetikler.
6. (İsteğe bağlı) Yapılandırma **e-posta** ve **Web kancası** bildirimleri. Web kancaları hakkında daha fazla bilgi için bkz. [Azure bir ölçüm uyarısında Web kancası yapılandırma](../../azure-monitor/platform/alerts-webhooks.md). E-posta veya Web kancası bildirimleri yapılandırmazsanız, uyarıları yalnızca Azure portalında görüntülenir.

![Azure portalında "bir uyarı kuralı Ekle" dikey](./media/storage-monitor-storage-account/add-alert-rule.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Ölçüm grafikleri portal panosuna Ekle

Portal panonuza herhangi biri depolama hesaplarınız için Azure depolama ölçüm grafikleri ekleyebilirsiniz.

1. Select tıklatın **panoyu Düzenle** Panonuzda görüntülerken [Azure portalında](https://portal.azure.com).
1. İçinde **kutucuk Galerisi**seçin **Bul kutucukları tarafından** > **türü**.
1. Seçin **türü** > **depolama hesapları**.
1. İçinde **kaynakları**, ölçümleri panoya eklemek istediğiniz depolama hesabını seçin.
1. Seçin **kategorileri** > **izleme**.
1. Sürükle ve bırak grafik kutucuğunda görüntülenmesini istediğiniz ölçümü için panonuzu üzerine. Panoda görüntülenen istediğiniz tüm ölçümler için yineleyin. Aşağıdaki görüntüde, örnek olarak "BLOB'lar - toplam istek" grafik vurgulanmış, ancak tüm grafikler Panonuzda yerleştirme için kullanılabilir.

   ![Azure portalında kutucuk Galerisi](./media/storage-monitor-storage-account/storage-customize-dashboard.png)
1. Seçin **özelleştirme Bitti** ekleme grafikleri işiniz bittiğinde panonun üstüne yakın.

Panonuza grafikleri ekledikten sonra bunları daha fazla özelleştirme ölçüm grafikleri içinde açıklanan şekilde özelleştirebilirsiniz.

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Kaydetme tanılama günlükleri, okuma, yazma ve silme istekleri blob, tablo ve kuyruk Hizmetleri için Azure Depolama'ya bildirebilirsiniz. Ayarladığınız veri bekletme ilkesi, bu günlükleri için de geçerlidir.

> [!NOTE]
> Azure dosyaları şu anda Storage Analytics ölçümleri destekliyor, ancak henüz günlük desteklemiyor.
>

1. İçinde [Azure portalında](https://portal.azure.com)seçin **depolama hesapları**, ardından depolama hesabı dikey penceresini açmak için depolama hesabı adı.
1. Seçin **tanılama** içinde **izleme** menü dikey bölümü.

    ![Azure portalında tanılama menü öğesini izleme altında.](./media/storage-monitor-storage-account/storage-enable-metrics-00.png)

1. Olun **durumu** ayarlanır **üzerinde**seçip **Hizmetleri** için hangi günlük kaydını etkinleştirmek istiyorsanız.

    ![Azure portalında oturum açmayı yapılandırın.](./media/storage-monitor-storage-account/enable-diagnostics.png)
1. **Kaydet**’e tıklayın.

Tanılama günlükleri adlı bir blob kapsayıcısını kaydedilir *$logs* depolama hesabınızda. Gibi bir Depolama Gezgini'ni kullanarak günlük verilerini görüntüleyebilirsiniz [Microsoft Storage Gezgini](https://storageexplorer.com), veya depolama istemci kitaplığı veya PowerShell kullanarak program aracılığıyla.

$Logs kapsayıcı erişme hakkında daha fazla bilgi için bkz: [depolama analizi günlük](storage-analytics-logging.md).

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında daha fazla ayrıntı bulmak [günlüğe kaydetme ve faturalandırma ölçümleri,](storage-analytics.md) depolama analizi için.
