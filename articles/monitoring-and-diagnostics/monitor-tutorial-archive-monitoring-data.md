---
title: "Arşiv izleme verilerini Azure | Microsoft Docs"
description: "Bir depolama hesabı için Azure içinde üretilen günlük ve ölçüm verilerini arşivleyebilirsiniz."
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: 445901a740920a74f259aaa9c6b862680c1c807e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="archive-azure-monitoring-data"></a>Arşiv Azure izleme verileri

Azure ortamınıza birkaç katmandan günlük ve bir Azure depolama hesabı arşivlenebilir ölçüm veri üretir. Bir geçmiş verilerin günlük analizi ya da Azure İzleyicisi, saklama süresi geçtikten sonra zaman içinde bir uygun maliyetli, aranabilir olmayan depolama alanına izleme verilerini korumak için bunu isteyebilirsiniz. Bu öğretici adım adım bir depolama hesabına verileri arşivlemek üzere Azure ortamınızı yapılandırma sürecinde.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

İlk izleme verilerini arşivlenir bir depolama hesabı ayarlamanız gerekir. Bunu yapmak için [burada adımları](../storage/common/storage-create-storage-account.md).

## <a name="route-subscription-logs-to-the-storage-account"></a>Rota abonelik günlüklerini depolama hesabı

Artık bir depolama hesabı izleme verileri yönlendirmek için Azure ortamınızı ayarlayın başlamak hazırsınız. İlk biz depolama hesabına yönlendirilecek abonelik düzeyinde veri (Azure etkinlik günlüğünde yer alan) yapılandırın. [ **Azure etkinlik günlüğü** ](monitoring-overview-activity-logs.md) Azure abonelik düzeyinde olayları geçmişini sağlar. Belirlemek için Azure Portal'da Gözat *kimin* oluşturulan, güncellenen veya silinen *ne* kaynakları ve *zaman* yaptıkları işlemin.

1. Tıklatın **İzleyici** düğmesi bulunamadı sol taraftaki gezinti listesinde sonra **etkinlik günlüğü**.

   ![Etkinlik günlüğü bölümü](media/monitor-tutorial-archive-monitoring-data/activity-log-home.png)

2. Görüntülenen etkinlik günlüğü bölümüne tıklayın **verme** düğmesi.

3. İçinde **verme etkinlik günlüğü** görünür, bölüm onay kutusu için **dışarı aktarma bir depolama hesabına** tıklatıp **bir depolama hesabı seçin.**

   ![Etkinlik günlüğü dışarı aktarma](media/monitor-tutorial-archive-monitoring-data/activity-log-export.png)

4. Görüntülenen, bölümün **depolama hesabı** oluşturulan önceki depolama hesabının adını seçmek için açılır **depolama hesabı oluşturma** adım ve ardından **Tamam**.

   ![Bir depolama hesabı seç](media/monitor-tutorial-archive-monitoring-data/activity-log-storage.png)

5. Ayarlama **bekletme (gün)** kaydırıcı 30. Bu kaydırıcı izleme verilerini depolama hesabındaki tutulacak gün sayısını ayarlar. Azure İzleyicisi otomatik olarak gün sayısından daha eski verileri siler belirtilen. Sıfır gün bekletme verileri süresiz olarak depolar.

6. Tıklatın **kaydetmek** ve bu bölümde kapatın.

İzleme verilerini aboneliğiniz storage hesabınıza şimdi akar.

## <a name="route-resource-data-to-the-storage-account"></a>Rota kaynak verileri depolama hesabı

Biz kaynak düzeyi verileri (kaynak ölçümleri ve tanılama günlükleri) için depolama hesabı ayarlama tarafından yönlendirilmesini yapılandırmak artık **kaynak tanılama ayarlarını**.

1. Tıklatın **İzleyici** düğmesi bulunamadı sol taraftaki gezinti listesinde sonra **tanılama ayarlarını**. Burada, aboneliğinizde Azure İzleyicisi İzleme verilerine üretmek tüm kaynakların bir listesini görürsünüz. Bu listedeki tüm kaynakları yoksa, şunları yapabilirsiniz [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md) tanılama ayarını yapılandırabilirsiniz bir kaynağa sahip bir devam etmeden önce.

2. Listedeki bir kaynakta tıklayın ve ardından **tanılamayı açın**.
   
   ![Tanılamayı açın](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-turn-on.png)

   Yapılandırılmış bir ayarı ise, bunun yerine var olan ayarları ve bir düğmeye gördüğünüz **tanılama ayar Ekle**. Bu düğmeye tıklayın.

   Kaynağın tanılama ayarını tanımıdır *ne* verilerin izlenmesi yönlendirilir belirli bir kaynaktan ve *burada* izleme verilerini tamamlamalıdır.

3. Görüntülenen bölümünde ayarınız verin bir **adı** ve için kutuyu **bir depolama hesabı arşive**.

   ![Tanılama ayarları bölümü](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-home.png)

4. Tıklayın **yapılandırma** altında düğmesini **bir depolama hesabı arşive** ve önceki bölümde oluşturduğunuz depolama hesabını seçin. **Tamam** düğmesine tıklayın.

   ![Tanılama ayarları depolama hesabı](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-storage.png)

5. Altındaki tüm onay kutularını işaretleyin **günlük** ve **ölçüm**. Kaynak türüne bağlı olarak, aşağıdaki seçeneklerden birini yalnızca olabilir. Bu onay kutuları için kullanılabilir kaynak türü gönderileceği, bu durumda, seçtiğiniz hedef depolama hesabı günlüğü ve ölçüm verilerinin kategoriler denetler.

   ![Tanılama ayarları kategorileri](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-categories.png)
   
6. Ayarlama **bekletme (gün)** kaydırıcı 30. Bu kaydırıcı izleme verilerini depolama hesabındaki tutulacak gün sayısını ayarlar. Azure İzleyicisi otomatik olarak gün sayısından daha eski verileri siler belirtilen. Sıfır gün bekletme verileri süresiz olarak depolar.

7. **Kaydet** düğmesine tıklayın.

İzleme verileri, kaynaktan storage hesabınıza şimdi akar.

## <a name="route-virtual-machine-guest-os-data-to-the-storage-account"></a>Rota sanal makine (konuk işletim sistemi) veri depolama hesabı

1. Bir sanal makine, aboneliğinizde zaten yoksa [bir sanal makine oluşturmak](../virtual-machines/windows/quick-create-portal.md).

2. Portalın sol gezinti listesinde tıklayın **sanal makineleri**.

3. Görüntülenen listede sanal makinelerin, oluşturduğunuz sanal makineye tıklayın.

4. Görüntülenen bölümüne tıklayın **tanılama ayarlarını** sol gezinti çubuğunda. Bu bölüm out-of-box İzleme uzantı, sanal makine ve bir depolama hesabına Windows veya Linux tarafından üretilen rota verilerini Azure İzleyicisi'nden ayarlamanıza olanak sağlar.

   ![Tanılama ayarlarına gidin](media/monitor-tutorial-archive-monitoring-data/guest-navigation.png)

5. Tıklatın **Konuk düzeyinde izlemeyi etkinleştir** bölümünde görüntülenir.

   ![Tanılama ayarlarını etkinleştirin](media/monitor-tutorial-archive-monitoring-data/guest-enable.png)

6. Tanılama ayarını doğru kaydedildikten sonra **genel bakış** sekmesini toplanmakta olan verilerin listesini gösterir ve depolanır. Tıklayın **performans sayaçları** Windows Performans kümesi gözden geçirmek için bölümüne sayaçları toplanmakta olan.

   ![Performans sayaçları ayarları](media/monitor-tutorial-archive-monitoring-data/guest-perf-counters.png)
   
7. Tıklayın **günlükleri** sekmesinde ve onay kutularını kontrol **bilgi** düzeyi günlüğe kaydeder, uygulama ve sistem günlüklerini.

   ![Günlükleri ayarları](media/monitor-tutorial-archive-monitoring-data/guest-logs.png)

8. Tıklayın **Aracısı** sekmesi ve altında **depolama hesabı** gösterilen depolama hesabı adına tıklayın.

   ![Depolama hesabı güncelleştirme](media/monitor-tutorial-archive-monitoring-data/guest-storage-home.png)

9. Görüntülenen bölümünde oluşturulan önceki depolama hesabı seç **depolama hesabı oluşturma** adım.

10. **Kaydet** düğmesine tıklayın.

Sanal makinelerinizi verilerden izleme storage hesabınıza şimdi akar.

## <a name="view-the-monitoring-data-in-the-storage-account"></a>Depolama hesabında izleme verilerini görüntüleme

Yukarıdaki adımları izlediyseniz, veri depolama hesabınıza akan başladı.

1. Bazı veri türleri için örneğin, etkinlik günlüğü vardır ve depolama hesabında bir olay oluşturur bazı etkinlikler olması gerekir. Etkinlik günlüğünde etkinlik oluşturmak için takip [bu yönergeleri](./monitor-quick-audit-notify-action-in-subscription.md). Depolama hesabında olay görünmeden önce en fazla beş dakika beklemeniz gerekebilir.

2. Portalı'nda gidin **depolama hesapları** sol gezinti çubuğunda bulma tarafından bölümü.

3. Önceki bölümde oluşturduğunuz depolama hesabı tanımlayabilir ve tıklayın.

4. Tıklayın **BLOB'lar**, sonra etiketli kapsayıcısı üzerinde **Öngörüler işletimsel-günlükleri** ve son olarak kapsayıcısında etiketli **adı = varsayılan**. Etkinlik günlüğü içerdiği kapsayıcıdır. İzleme verilerini kapsayıcılarına kaynak kimliği (yalnızca abonelik kimliği için etkinlik günlüğü), sonra tarih ve saat tarafından ayrılır. Bu BLOB'ları için tam biçimi şöyledir:

   Öngörüler-işletimsel-günlükleri/name = varsayılan/ResourceId/ABONELİKLERİ = / {abonelik kimliği} / y = {dört basamaklı sayısal year} / m = {iki basamaklı sayısal ay} / d = {iki basamaklı sayısal günü} / h {iki basamaklı 24 saatlik hour}/m=00/PT1H.json =

5. Kaynak Kimliği, tarih ve saat için kapsayıcılarına tıklayarak PT1H.json dosyasına gidin. PT1H.json dosyayı ve'ı tıklatın **karşıdan**. Her bir PT1H.json blob JSON blobu blob URL'SİNDE belirtilen saat içinde gerçekleşen olayların içerir (örneğin, h = 12). Bunlar ortaya çıktığında mevcut saatte olayları PT1H.json dosyasına eklenir. Dakika değeri (m 00 =) her zaman günlük olaylarının saat başına tek tek bloblar ayrılmış bu yana 00.

   Artık depolama hesabında depolanan JSON olay görüntüleyebilirsiniz. Kaynak için tanılama günlükleri, BLOB biçimi şöyledir:

   Öngörüler - günlükleri-{günlük kategori adı} / ResourceId = / {kaynak kimliği} / y = {dört basamaklı sayısal year} / m = {iki basamaklı sayısal ay} / d = {iki basamaklı sayısal günü} / h = {iki basamaklı 24 saatlik hour}/m=00/PT1H.json

6. Konuk işletim sistemi izleme verilerini tablolarında depolanır. geri depolama hesabına giriş gidin ve tıklayın **tabloları**. Tablolar ölçümleri, performans sayaçları ve olay günlüklerini için vardır.

Şimdi başarıyla izleme verilerini bir depolama hesabına arşivlenecek ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Geri gidin **etkinlik günlüğü dışarı** önceki bölümde **depolama hesabı için abonelik günlüklerini rota** tıklayın ve adım **sıfırlama**.

2. Gidin **tanılama ayarlarını** bölümünde, üzerinde oluşturduğunuz bir tanılama ayarını önceki kaynağa tıklayın **kaynak verileri depolama hesabına rota** adım sonra ayarını bulun, oluşturulan, tıklatın **ayarını Düzenle** düğmesini tıklatın ve tıklatın **silmek**.

3. Gidin **tanılama ayarlarını** yapılandırılmış önceki sanal makine bölüm **sanal makine (konuk işletim sistemi) verileri depolama hesabına rota** adım ve altında  **Aracı** sekmesini tıklatın **kaldırmak** (altındaki **kaldırmak Azure Tanılama Aracı** bölümü).

4. Oluşturulan önceki depolama hesabına gidin **depolama hesabı oluşturma** adım ve tıklatın **depolama hesabı Sil**. Depolama hesabı adını yazın ve ardından **silmek**.

5. Bir sanal makine ya da mantıksal uygulama için yukarıdaki adımları oluşturduysanız, bu da silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure ortamınıza (abonelik, kaynak ve konuk işletim sistemi) bir depolama hesabına arşivlenecek verilerden izleme işlevini ayarlama öğrendiniz. Verilerinizi dışında daha anlamlı yapmak ve Öngörüler türetilmesi için günlük analizi de verilerinizi gönderme deneyin.

> [!div class="nextstepaction"]
> [Günlük Analytics ile çalışmaya başlama](../log-analytics/log-analytics-get-started.md)
