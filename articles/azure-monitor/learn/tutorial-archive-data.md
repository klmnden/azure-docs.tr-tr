---
title: Azure Depolama’yı kullanarak Azure ölçümlerini ve günlük verilerini arşivleme
description: Azure içinde oluşturulmuş günlük ve ölçüm verilerini bir depolama hesabında arşivleyin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.subservice: metrics
ms.openlocfilehash: e784b76af478f834a7d1104650da1cbd3ca2a31f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60552935"
---
# <a name="archive-azure-metric-and-log-data-using-azure-storage"></a>Azure Depolama’yı kullanarak Azure ölçümlerini ve günlük verilerini arşivleme

Azure ortamınızın birkaç katmanında, bir Azure Depolama hesabında arşivlenebilen günlük ve ölçüm verileri oluşturulur. Geçmiş izleme verilerini bunların saklanma dönemi bittikten sonra uzun süre düşük maliyetli, aranabilir olmayan bir depoda tutmak için bu arşivlemeyi kullanmanız iyi olur. 

- Azure Monitor platform ölçümleri 93 gün boyunca tutulur. 
- Kaynak tanılama günlükleri yalnızca en az 30 günlük yapılandırılabilir bir saklama sürelerinin olduğu Log Analytics'e yönlendirilmişse görünür. 
- Etkinlik günlüğü girişleri 90 gün boyunca tutulur.  

Bu öğreticide, verileri bir depolama hesabında arşivlemek üzere Azure ortamınızı yapılandırma işlemi adım adım gösterilmektedir.

> [!div class="checklist"]
> * İzleme verilerini tutmak için depolama hesabı oluşturma
> * Abonelik günlüklerini depolama hesabına yönlendirme
> * Kaynak verilerini depolama hesabına yönlendirme
> * Sanal makine (konuk işletim sistemi) verilerini depolama hesabına yönlendirme
> * Depolama hesabındaki izleme verilerini görüntüleme
> * Kaynaklarınızı temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

İlk olarak izleme verilerinin arşivleneceği bir depolama hesabı ayarlamanız gerekir. Bunu yapmak için [buradaki adımları izleyin](../../storage/common/storage-quickstart-create-account.md).

## <a name="route-subscription-logs-to-the-storage-account"></a>Abonelik günlüklerini depolama hesabına yönlendirme

Artık, verileri bir depolama hesabına yönlendirmek üzere Azure ortamınızı ayarlamaya başlamak için hazırsınız. İlk olarak, depolama hesabına yönlendirilecek abonelik düzeyinde verileri (Azure Etkinlik Günlüğünde yer alır) yapılandıracağız. [**Azure Etkinlik Günlüğü**](../../azure-monitor/platform/activity-logs-overview.md), Azure'da abonelik düzeyindeki olayların geçmişini sağlar. *Hangi* kaynakları *kimin*, *ne zaman* oluşturduğunu, güncelleştirdiğini veya sildiğini belirlemek için Azure portalda bu geçmişe göz atabilirsiniz.

1. Sol gezinti listesinde bulunan **İzleyici** düğmesine ve sonra **Etkinlik Günlüğü**’ne tıklayın.

   ![Etkinlik Günlüğü bölümü](media/tutorial-archive-data/activity-log-home.png)

2. Görüntülenen Etkinlik Günlüğü bölümünde **Dışarı Aktar** düğmesine tıklayın.

3. Görüntülenen **Etkinlik günlüğünü dışarı aktar** bölümünde **Bir depolama hesabına aktarın** kutusunu işaretleyin ve **Bir depolama hesabı seçin**’e tıklayın.

   ![Etkinlik Günlüğünü dışarı aktarma](media/tutorial-archive-data/activity-log-export.png)

4. Görüntülenen bölümde **Depolama hesabı** açılır listesini kullanarak önceki **Depolama hesabı oluşturma** adımında oluşturduğunuz depolama hesabı adını seçin ve sonra **Tamam**’a tıklayın.

   ![Depolama hesabı seçme](media/tutorial-archive-data/activity-log-storage.png)

5. **Bekletme (gün)** kaydırıcısını 30’a ayarlayın. Bu kaydırıcı, depolama hesabında izleme verilerinin tutulacağı gün sayısını ayarlar. Azure İzleyici, belirtilen gün sayısından daha eski verileri otomatik olarak siler. Bekletme günü sayısının sıfır olması verileri süresiz olarak depolar.

6. **Kaydet**’e tıklayıp bu bölümü kapatın.

Aboneliğinizdeki izleme verileri artık depolama hesabına akar.

## <a name="route-resource-data-to-the-storage-account"></a>Kaynak verilerini depolama hesabına yönlendirme

Şimdi, **kaynak tanılama ayarlarını** yaparak kaynak düzeyinde verileri (kaynak ölçümleri ve tanılama günlükleri) depolama hesabına yönlendirilecek şekilde yapılandıracağız.

1. Sol gezinti listesinde bulunan **İzleyici** düğmesine ve sonra **Tanılama Ayarları**’na tıklayın. Burada, aboneliğinizde Azure İzleyici ile izleme verileri oluşturan tüm kaynakların bir listesini görürsünüz. Bu listede herhangi bir kaynak yoksa, bir tanılama ayarını açık olarak yapılandırabileceğiniz bir kaynağınızın olması için devam etmeden önce [bir mantıksal uygulama oluşturabilirsiniz](../../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Listedeki bir kaynağa ve ardından **Tanılamayı aç**’a tıklayın.

   ![Tanılamayı açma](media/tutorial-archive-data/diagnostic-settings-turn-on.png)

   Zaten yapılandırılmış bir ayar varsa, bunun yerine mevcut ayarları ve **Tanılama ayarı ekle** düğmesini görürsünüz. Bu düğmeye tıklayın.

   Kaynak tanılama ayarı, belirli bir kaynaktan *hangi* izleme verilerinin yönlendirilmesi gerektiğine ve bu izleme verilerinin *nereye* gideceğine ilişkin bir tanımdır.

3. Görüntülenen bölümde, ayarınıza bir **ad** verin ve **Bir depolama hesabında arşivle** kutusunu işaretleyin.

   ![Tanılama ayarları bölümü](media/tutorial-archive-data/diagnostic-settings-home.png)

4. **Bir depolama hesabında arşivle** altındaki **Yapılandır** düğmesine tıklayın ve önceki bölümde oluşturduğunuz depolama hesabını seçin. **Tamam**’a tıklayın.

   ![Tanılama ayarları depolama hesabı](media/tutorial-archive-data/diagnostic-settings-storage.png)

5. **Günlük** ve **Ölçüm** altındaki tüm kutuları işaretleyin. Kaynak türüne bağlı olarak, bu seçeneklerden yalnızca birini kullanabilirsiniz. Bu onay kutuları, seçtiğiniz hedefe (bu örnekte bir depolama hesabına) ilgili kaynak türü için kullanılabilen günlük ve ölçüm verileri kategorilerinden hangilerinin gönderildiğini denetler.

   ![Tanılama ayarları kategorileri](media/tutorial-archive-data/diagnostic-settings-categories.png)

6. **Bekletme (gün)** kaydırıcısını 30’a ayarlayın. Bu kaydırıcı, depolama hesabında izleme verilerinin tutulacağı gün sayısını ayarlar. Azure İzleyici, belirtilen gün sayısından daha eski verileri otomatik olarak siler. Bekletme günü sayısının sıfır olması verileri süresiz olarak depolar.

7. **Kaydet**’e tıklayın.

Kaynağınızdaki izleme verileri artık depolama hesabına akar.

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="route-virtual-machine-guest-os-data-to-the-storage-account"></a>Sanal makine (konuk işletim sistemi) verilerini depolama hesabına yönlendirme

1. Aboneliğinizde henüz bir sanal makine yoksa [bir sanal makine oluşturun](../../virtual-machines/windows/quick-create-portal.md).

2. Portalın sol gezinti listesinde **Sanal Makineler**’e tıklayın.

3. Görüntülenen sanal makineler listesinde, oluşturduğunuz sanal makineye tıklayın.

4. Görüntülenen bölümde, sol gezinti listesindeki **Tanılama Ayarları**’na tıklayın. Bu bölüm, sanal makinenizde Azure İzleyici’den kullanıma hazır izleme uzantısını ayarlamanıza ve Windows ya da Linux tarafından oluşturulan verileri bir depolama hesabına yönlendirmenize olanak tanır.

   ![Tanılama ayarlarına gidin](media/tutorial-archive-data/guest-navigation.png)

5. Görüntülenen bölümde **Konuk düzeyinde izlemeyi etkinleştir**’e tıklayın.

   ![Tanılama ayarlarını etkinleştirme](media/tutorial-archive-data/guest-enable.png)

6. Tanılama ayarı doğru şekilde kaydedildikten sonra **Genel Bakış** sekmesinde toplanmakta olan verilerin bir listesi ve nerede depolandıkları gösterilir. Toplanmakta olan Windows performans sayaçları kümesini gözden geçirmek için **Performans sayaçları** bölümüne tıklayın.

   ![Performans sayaçları ayarları](media/tutorial-archive-data/guest-perf-counters.png)

7. **Günlükler** sekmesine tıklayın ve Uygulama ile Sistem günlüklerindeki **Bilgi** düzeyi günlüklerinin onay kutularını işaretleyin.

   ![Günlük ayarları](media/tutorial-archive-data/guest-logs.png)

8. **Aracı** sekmesine tıklayın ve **Depolama hesabı** altında gösterilen depolama hesabının adına tıklayın.

   ![Depolama hesabını güncelleştirme](media/tutorial-archive-data/guest-storage-home.png)

9. Görüntülenen bölümde, önceki **Depolama hesabı oluşturma** adımında oluşturduğunuz depolama hesabını seçin.

10. **Kaydet**’e tıklayın.

Sanal makinelerinizdeki izleme verileri artık depolama hesabına akar.

## <a name="view-the-monitoring-data-in-the-storage-account"></a>Depolama hesabındaki izleme verilerini görüntüleme

> [!WARNING]
> Depolama hesabındaki günlük verilerinin biçimi, 1 Kasım 2018 tarihinde JSON Satırları olarak değişecektir. [Etkinin açıklaması ve yeni biçimi işlemek üzere araçlarınızı güncelleştirme için bu makaleye bakın.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

Yukarıdaki adımları izlediyseniz, veriler depolama hesabınıza akmaya başlamıştır.

1. Bazı veri türleri için (örneğin, Etkinlik Günlüğü), depolama hesabında olay oluşturan bazı etkinliklerin olması gerekir. Etkinlik Günlüğünde etkinlik oluşturmak için [bu yönergeleri](./../../azure-monitor/platform/quick-audit-notify-action-subscription.md) takip edin. Olayın depolama hesabında görünmesi için beş dakikaya kadar beklemeniz gerekebilir.

2. Portalda, sol gezinti çubuğundaki **Depolama Hesapları** bölümüne gidin.

3. Önceki bölümde oluşturduğunuz depolama hesabını belirleyin ve tıklayın.

4. **Bloblar**’a tıklayın, ardından **insights-operational-logs** etiketli kapsayıcıya ve son olarak **name=default** etiketli kapsayıcıya tıklayın. Etkinlik Günlüğünüz bu kapsayıcının içindedir. İzleme verileri, kaynak kimliğine (yalnızca Etkinlik Günlüğünün abonelik kimliği) ve sonra tarih ve saate göre kapsayıcılara ayrılır. Bu blobların tam biçimi şöyledir:

   insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{abonelik kimliği}/y={dört basamaklı sayı olarak yıl}/m={iki basamaklı sayı olarak ay}/d={iki basamaklı sayı olarak gün}/h={iki basamaklı 24 saat biçiminde saat}/m=00/PT1H.json

5. Kaynak kimliği, tarih ve saat için kapsayıcılara tıklayarak PT1H.json dosyasına gidin. PT1H.json dosyasına ve **İndir**’e tıklayın. Her PT1H.json blobu, blob URL’sinde belirtilen saat (örneğin, h=12) içinde gerçekleşen bir JSON olay blobu içerir. Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Günlük olayları saat başına bloblara ayrıldığı için dakika değeri (m=00) her zaman 00’dır.

   Artık depolama hesabında depolanmış JSON olayını görüntüleyebilirsiniz. Kaynak tanılama günlükleri için blob biçimi şu şekildedir:

   insights-logs-{günlük kategorisi adı}/resourceId=/{kaynak kimliği}/y={dört basamaklı sayı olarak yıl}/m={iki basamaklı sayı olarak ay}/d={iki basamaklı sayı olarak gün}/h={iki basamaklı 24 saat biçiminde saat}/m=00/PT1H.json

6. Konuk işletim sistemi izleme verileri tablolarda depolanır. Depolama hesabı giriş sayfasına geri dönüp **Tablolar**’a tıklayın. Ölçümler, performans sayaçları ve olay günlüklerine yönelik tablolar bulunur.

Bir depolama hesabında arşivlenecek izleme verilerini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Önceki **Abonelik günlüklerini depolama hesabına yönlendirme** adımında bulunan **Etkinlik Günlüğünü Dışarı Aktarma** bölümüne geri gidin ve **Sıfırla**’ya tıklayın.

2. **Tanılama Ayarları** bölümüne gidin, önceki **Kaynak verilerini depolama hesabına yönlendirme** adımında tanılama ayarını oluşturduğunuz kaynağa tıklayın, sonra oluşturduğunuz ayarı bulun, **Ayarı düzenle** düğmesine ve **Sil**’e tıklayın.

3. Önceki **Sanal makine (konuk işletim sistemi) verilerini depolama hesabına yönlendirme** adımında yapılandırdığınız sanal makinede **Tanılama Ayarları** bölümüne gidin ve **Aracı** sekmesi altında **Kaldır**’a tıklayın (**Azure Tanılama aracısını kaldırma** bölümü altında).

4. Önceki **Depolama hesabı oluşturma** adımında oluşturduğunuz depolama hesabına gidip **Depolama hesabını sil**’e tıklayın. Depolama hesabının adını yazıp **Sil**'e tıklayın.

5. Önceki adımlar için bir sanal makine ya da mantıksal uygulama oluşturduysanız onları da silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure ortamınızdan bir depolama hesabında arşivlenecek izleme verilerini (abonelik, kaynak ve konuk işletim sistemi) ayarlamayı öğrendiniz.


> [!div class="checklist"]
> * İzleme verilerini tutmak için depolama hesabı oluşturma
> * Abonelik günlüklerini depolama hesabına yönlendirme
> * Kaynak verilerini depolama hesabına yönlendirme
> * Sanal makine (konuk işletim sistemi) verilerini depolama hesabına yönlendirme
> * Depolama hesabındaki izleme verilerini görüntüleme
> * Kaynaklarınızı temizleme

Verilerinizden daha iyi şekilde yararlanmak ve ek bilgiler edinmek için verilerinizi Log Analytics’e de gönderin.

> [!div class="nextstepaction"]
> [Log Analytics'i kullanmaya başlama](../../azure-monitor/log-query/log-query-overview.md)

