---
title: Oluşturun, görüntüleyin ve ölçüm uyarıları kullanarak Azure izleme yönetme
description: Oluşturun, görüntüleyin ve ölçüm uyarı kuralları yönetmek için Azure portal veya CLI kullanmayı öğrenin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 117b65265c853194e93a97fe5e2b2dcc6e9f5bc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60713002"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve ölçüm uyarıları Azure İzleyicisi'ni kullanarak yönetme

Azure İzleyici ölçüm uyarılarını bildirim almak için bir yol sağlar bir eşiği ölçümlerinizi birini çapraz zaman. Ölçüm uyarıları, bir dizi platform çok boyutlu ölçümler, özel ölçümler, Application Insights standart ve özel ölçümler üzerinde çalışır. Bu makalede, biz oluşturun, görüntüleyin ve Azure portalı ve Azure CLI aracılığıyla ölçüm uyarı kurallarını yönetme konusunda açıklanmıştır. Ölçüm uyarı kuralları açıklanan Azure Resource Manager şablonlarını kullanarak da oluşturabilirsiniz [ayrı bir makale](alerts-metric-create-templates.md).

Daha fazla hakkında nasıl ölçüm uyarıları çalıştığınızın edinebilirsiniz [ölçüm uyarıları genel bakış](alerts-metric-overview.md).

## <a name="create-with-azure-portal"></a>Azure portalı ile oluşturma

Aşağıdaki yordamda, Azure portalında bir ölçüm uyarısı kuralının oluşturulacağı açıklanmaktadır:

1. İçinde [Azure portalında](https://portal.azure.com), tıklayarak **İzleyici**. İzleyici dikey penceresinde, izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

2. Tıklayın **uyarılar** ardından **+ yeni uyarı kuralı**.

    > [!TIP]
    > Çoğu kaynak dikey pencerelerinin de **uyarılar** altında kendi kaynak menüsünde **izleme**, oradan da uyarılar oluşturabilirsiniz.

3. Tıklayın **hedefi seçme**, yükler içerik bölmesinde, uyarı istediğiniz hedef kaynak seçin. Kullanım **abonelik** ve **kaynak türü** izlemek istediğiniz kaynak bulmak için açılan listeler. Kaynak bulmak için arama çubuğunu da kullanabilirsiniz.

4. Seçilen kaynak ölçümleri, uyarılar oluşturabilirsiniz varsa **kullanılabilir sinyaller** altta sağ ölçümleri içerir. Bu ölçüm uyarıları için desteklenen kaynak türleri tam listesini görüntüleyebileceğiniz [makale](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

5. Hedef kaynak seçtikten sonra tıklayarak **koşul Ekle**.

6. Kaynak için desteklenen sinyalleri listesini, bir uyarı oluşturmak istediğiniz ölçümü seçin.

7. İsteğe bağlı olarak ayarlayarak ölçüm İyileştir **süresi** ve **toplama**. Ölçüm, boyutları değiştiyse, göreceğiniz **boyutları** sunulan tablo. Boyut başına bir veya daha fazla değer seçin. Ölçüm uyarısı çalışacak seçilen değerlerin tüm bileşimleri için koşulu değerlendirmek. [Çok boyutlu ölçümler üzerinde uyarı nasıl çalıştığı hakkında daha fazla bilgi](alerts-metric-overview.md). Ayrıca **seçin \***  için tüm boyutlarının. **Seçin \***  dinamik olarak ölçek bir boyut için tüm mevcut ve gelecekteki değerleri seçimi olacaktır.

8. Son 6 saat boyunca ölçüm için bir grafik görürsünüz. Uyarı parametreleri tanımlayın. **Koşul türü**, **sıklığı**, **işleci** ve **eşiği** veya **duyarlılık**, bu Ölçüm uyarısı kuralının değerlendirecek mantığı belirleyin. [Dinamik eşikler koşul türü ve duyarlılık seçenekleri hakkında daha fazla bilgi](alerts-dynamic-thresholds.md).

9. Statik bir eşiği kullanıyorsanız, ölçüm grafiğini makul bir eşiği ne olabilir belirlemeye yardımcı olabilir. Dinamik eşikler kullanıyorsanız, son verileri temel alan hesaplanan eşikleri ölçüm grafiği görüntüler.

10. **Bitti**’ye tıklayın

11. Karmaşık bir uyarı kuralı izlemek istiyorsanız, isteğe bağlı olarak, başka bir ölçüt ekleyin. Şu anda kullanıcılar tek bir ölçüt olarak Dinamik Eşik ölçütlerle uyarı kuralları olabilir.

12. Doldurun **uyarı ayrıntıları** gibi **uyarı kuralı adı**, **açıklama** ve **önem derecesi**

13. Yeni bir eylem grubu oluşturma veya mevcut bir eylem grubu seçerek uyarıyı bir eylem grubu ekleyin.

14. Tıklayın **Bitti** ölçüm uyarı kuralını kaydetmek için.

> [!NOTE]
> Ölçüm uyarı kuralları portalı üzerinden oluşturuldu, hedef kaynağın aynı kaynak grubunda oluşturulur.

## <a name="view-and-manage-with-azure-portal"></a>Görüntüleme ve Azure portalıyla yönetme

Görüntüleyebilir ve ölçüm uyarı kuralları altında uyarı kurallarını yönet dikey penceresini kullanarak yönetin. Aşağıdaki yordam, ölçüm uyarı kuralları görüntüleyin ve bunlardan birinin Düzenle gösterilmektedir.

1. Azure portalında gidin **İzleyicisi**

2. Tıklayarak **uyarılar** ve **kurallarını yönetme**

3. İçinde **yönetme kuralları** dikey penceresinde, abonelikler arasında tüm uyarı kurallarınızı görüntüleyebilirsiniz. Daha fazla kullanarak kuralları filtrelemek **kaynak grubu**, **kaynak türü** ve **kaynak**. Yalnızca ölçüm uyarıları görmek istiyorsanız, seçin **sinyal türü** ölçümler olarak.

    > [!TIP]
    > İçinde **yönetme kuralları** dikey penceresinde, birden çok uyarı kuralları seçebilir ve bunları etkinleştir/devre. Belirli hedef kaynak, bakım moduna gerektiğinde bu kullanışlı olabilir

4. Düzenlemek istediğiniz ölçüm uyarısı kuralının adına tıklayın

5. Düzenleme kuralda tıklayarak **Uyarı ölçütleri** düzenlemek istediğiniz. Ölçüm ve eşik koşuluna diğer alanları gerektiği gibi değiştirebilirsiniz

    > [!NOTE]
    > Düzenleyemediğiniz **hedef kaynak** ve **uyarı kuralı adı** ölçüm uyarı oluşturulduktan sonra.

6. Tıklayın **Bitti** yaptığınız düzenlemeleri kaydetmek için.

## <a name="with-azure-cli"></a>Azure CLI ile

Önceki bölümlerde oluşturun, görüntüleyin ve Azure portalını kullanarak ölçüm uyarı kuralları yönetmek nasıl kaydedileceği açıklanır. Bu bölümde, platformlar arası kullanarak aynı şeyi nasıl anlatılacaktır [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Hızlı şekilde kullanmaya başlamak Azure CLI aracılığıyladır [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). Bu makalede, Cloud Shell'i kullanacağız.

1. Azure portalına gidin, tıklayarak **Cloud shell**.

2. İstemde, komutları ile kullanabileceğiniz ``--help`` komut ve nasıl kullanılacağı hakkında daha fazla bilgi için seçeneği. Örneğin, aşağıdaki komut, oluşturma, görüntüleme ve ölçüm Uyarıları yönetmek için kullanılabilir komutların listesini gösterir

    ```azurecli
    az monitor metrics alert --help
    ```

3. Bir VM'de ortalama CPU yüzdesi 90'dan büyükse, izleyen basit bir ölçüm uyarısı kuralının oluşturabilirsiniz.

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. Aşağıdaki komutu kullanarak bir kaynak grubundaki tüm ölçüm uyarılarını görüntüleyebilirsiniz

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Adını veya kuralın kaynak Kimliğini kullanarak belirli ölçüm uyarı kuralı ayrıntılarını görebilirsiniz.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. Aşağıdaki komutu kullanarak bir ölçüm uyarısı kuralının devre dışı bırakabilirsiniz.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. Aşağıdaki komutu kullanarak bir ölçüm uyarısı kuralının silebilirsiniz.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager şablonlarını kullanarak ölçüm uyarıları oluşturma](../../azure-monitor/platform/alerts-enable-template.md).
- [Nasıl iş ölçüm uyarıları anlamak](alerts-metric-overview.md).
- [Nasıl dinamik eşikler koşul çalışmak ölçüm uyarıları anlamak](alerts-dynamic-thresholds.md).
- [Ölçüm uyarıları için web kancası şeması anlama](../../azure-monitor/platform/alerts-metric-near-real-time.md#payload-schema)

