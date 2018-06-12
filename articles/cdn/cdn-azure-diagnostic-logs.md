---
title: Azure tanılama günlüklerini | Microsoft Docs
description: Müşteri, Günlük çözümlemesi için Azure CDN etkinleştirebilirsiniz.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: v-deasim
ms.openlocfilehash: 98a7fc5c4607115811e17a7cf6acd4e867663833
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261313"
---
# <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri, temel analiz görüntülemek ve bunları dahil olmak üzere bir veya daha fazla hedefleri kaydedin:

 - Azure Storage hesabı
 - Azure Event Hubs
 - [Günlük analizi çalışma alanı](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Bu özellik, tüm fiyatlandırma katmanlarına CDN uç noktalarda kullanılabilir. 

Azure tanılama günlükleri, özelleştirilmiş bir biçimde tüketebileceği temel kullanım ölçümlerini CDN uç noktasından çeşitli kaynakları dışa olanak tanır. Örneğin, aşağıdaki veri verme türlerini yapabilirsiniz:

- Blob depolama, CSV için dışarı aktarma ve Excel'de grafikleri oluşturmak için verileri dışa aktarın.
- Verileri Event Hubs'a verin ve diğer Azure hizmetleriyle verilerle ilişkilendirmek.
- Veri kendi günlük analizi çalışma alanı günlük analizi ve görünüm verileri dışarı aktarma

Aşağıdaki diyagramda verilerinin normal CDN çekirdek analytics görünümü gösterir.

![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Şekil 1 - CDN çekirdek analytics görünümü*

Tanılama günlükleri hakkında daha fazla bilgi için bkz: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-the-azure-portal"></a>Azure portal ile günlük kaydını etkinleştir

CDN çekirdek analytics ile günlüğü bu adımları etkinleştir izleyin:

[Azure Portal](http://portal.azure.com)’da oturum açın. İş akışınız için CDN yok zaten etkinleştirdiğiniz varsa [bir Azure CDN profili ve uç nokta oluşturma](cdn-create-new-endpoint.md) devam etmeden önce.

1. Azure portalında gidin **CDN profili**.

2. Azure portalında aramak için bir CDN profili veya bir Pano seçin. Ardından, tanılama günlüklerini etkinleştirmek istediğiniz CDN uç noktası seçin.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Seçin **tanılama günlükleri** izleme bölümünde.

   **Tanılama günlükleri** sayfası görüntülenir.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Azure Storage ile günlük kaydını etkinleştir

Günlükleri depolamak için bir depolama hesabı kullanmak için şu adımları izleyin:
    
1. İçin **adı**, tanılama günlük ayarlarınızı için bir ad girin.
 
2. Seçin **bir depolama hesabı arşive**seçeneğini belirleyip **CoreAnalytics**. 

2. İçin **bekletme (gün)**, bekletme gün sayısını seçin. Sıfır gün bekletme günlükleri süresiz olarak depolar. 

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. Seçin **depolama hesabı**.

    **Bir depolama hesabı seçin** sayfası görüntülenir.

4. Aşağı açılan listeden bir depolama hesabı seçin ve ardından **Tamam**.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Tanılama günlük ayarlarınızı yaptıktan sonra seçin **kaydetmek**.

### <a name="logging-with-log-analytics"></a>Günlük analizi ile günlüğe kaydetme

Günlük analizi günlükleri depolamak için kullanmak için aşağıdaki adımları izleyin:

1. Gelen **tanılama günlükleri** sayfasında, **için günlük analizi Gönder**. 

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Seçin **yapılandırma** günlük analizi günlüğe kaydetmeyi yapılandırmak için. 

   **OMS çalışma alanları** sayfası görüntülenir.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Seçin **yeni çalışma alanı oluşturma**.

    **OMS çalışma** sayfası görüntülenir.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/07_Create-new.png)

4. İçin **OMS çalışma**, bir OMS çalışma alanı adı girin. OMS çalışma alanı adı benzersiz olmalı ve yalnızca harf, rakam ve kısa çizgi içerebilir; boşluk ve alt çizgi izin verilmiyor. 

5. İçin **abonelik**, mevcut bir aboneliğe aşağı açılan listeden seçin. 

6. İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya varolan bir tanesini seçin.

7. İçin **konumu**, listeden bir konum seçin.

8. Seçin **panoya Sabitle** günlük yapılandırması panonuza kaydetmek istiyorsanız. 

9. Seçin **Tamam** yapılandırmayı tamamlamak için.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/08_Workspace-resource.png)

10. Çalışma alanınızı oluşturulduktan sonra için dönersiniz **tanılama günlükleri** sayfası. Yeni günlük analizi çalışma alanınız adını onaylayın.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. Seçin **CoreAnalytics**seçeneğini belirleyip **kaydetmek**.

12. Yeni günlük analizi çalışma alanınız görüntülemek için seçin **çekirdek analytics** CDN uç noktası sayfanızdan.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Günlük analizi çalışma alanınız verileri günlüğe kaydetmek artık hazırdır. Bu verileri kullanmak için kullanmalısınız bir [günlük analiz çözümü](#consuming-diagnostics-logs-from-a-log-analytics-workspace), bu makalenin sonraki bölümlerinde kapsanan.

Günlük verileri gecikmeler hakkında daha fazla bilgi için bkz: [oturum veri gecikmeler](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlük kaydını etkinleştir

Aşağıdaki örnek, Azure PowerShell cmdlet'leri aracılığıyla tanılama günlüklerini etkinleştirme gösterilmektedir.

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Bir depolama hesabında oturum tanılama etkinleştirme

1. Oturum açmak ve bir abonelik seçin:

    Connect-AzureRmAccount 

    Select-AzureSubscription - Subscriptionıd 

2. Tanılama günlüklerini bir depolama hesabında etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Günlük analizi çalışma alanındaki tanılama günlüklerini etkinleştirmek için şu komutu girin:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Azure depolama biriminden tanılama günlüklerini kullanma
Bu bölümde CDN temel analiz şeması açıklanmaktadır nasıl içinde Azure storage hesabı düzenlenir ve bir CSV dosyasında günlükleri indirmek için örnek kod sağlar.

### <a name="using-microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Gezgini kullanma
Bir Azure depolama hesabından temel analytics verilere erişebilmeniz için önce bir depolama hesabı içeriğine erişmek için bir aracı ilk gerekir. Varken çeşitli araçlar kullanılabilir pazarında, önerdiğimiz Microsoft Azure Storage Gezgini adrestir. Aracı indirmek için bkz: [Azure Storage Gezgini](http://storageexplorer.com/). İndirme ve yazılım yükleme sonrasında CDN tanılama günlükleri için hedef olarak yapılandırılan aynı Azure depolama hesabı kullanacak şekilde yapılandırın.

1.  Açık **Microsoft Azure Storage Gezgini**
2.  Depolama hesabını bulun
3.  Genişletme **Blob kapsayıcıları** düğümü bu depolama hesabı altında.
4.  Adlı kapsayıcıyı seçin *Öngörüler günlükleri coreanalytics*.
5.  Sağ bölmede, ilk düzeyiyle öğe olarak başlatılıyor Göster sonuçları *ResourceId =*. Dosya bulana kadar her düzeyi seçme devam *PT1H.json*. Aşağıdaki bakın *Blob yol biçimi* için bir açıklama yolunu not.
6.  Her bir blob *PT1H.json* dosya belirli bir CDN uç noktası veya kendi özel etki alanı için bir saat analiz günlükleri gösterir.
7.  Bu JSON dosyasının içeriğini Şeması Çekirdek analiz günlükleri bölüm şemada tanımlanır.


> [!NOTE]
> **BLOB yol biçimi**
> 
> Çekirdek analiz günlükleri her saat oluşturulur ve veriler toplanır ve tek bir Azure blob iç bir JSON yükü olarak depolanır. Depolama Gezgini araçları Yorumlar '/' dizin ayırıcı ve programları hiyerarşisi Azure blob yolu hiyerarşik bir yapı olduğu gibi görünür olduğundan ve blob adını temsil eder. Blob adını aşağıdaki adlandırma kuralını izler: 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Alanları açıklaması:**

|Değer|Açıklama|
|-------|---------|
|Abonelik Kimliği    |GUID biçiminde Azure abonelik kimliği.|
|Kaynak Grubu Adı |CDN kaynaklara ait olduğu kaynak grubu adı.|
|Profil adı |CDN profili adı|
|Uç nokta adı |CDN uç noktası adı|
|Yıl|  Dört rakamlı yıl, örneğin, 2017 gösterimi|
|Ay| İki basamaklı ay numarasına gösterimi. 01 Ocak =... 12 Aralık =|
|Gün|   Ayın günü iki basamaklı gösterimi|
|PT1H.json| Analiz verilerinin depolandığı gerçek JSON dosyası|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Temel analiz verileri bir CSV dosyasına dışarı aktarma

Temel analiz erişim kolaylaştırmak için örnek kod bir araç için sağlanır. Bu araç, grafik veya diğer toplamaları oluşturmak için kullanılan bir düz virgülle ayrılmış dosya biçimine JSON dosyaları indirme sağlar.

Aracı'nı nasıl kullanabileceğiniz aşağıda verilmiştir:

1.  Github bağlantıyı ziyaret edin: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Kodu indirme.
3.  Derleme ve yapılandırmak için yönergeleri izleyin.
4.  Aracı çalıştırın.
5.  Sonuçta elde edilen CSV dosyasını analiz verileri basit bir düz hiyerarşi içinde gösterir.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Günlük analizi çalışma alanından tanılama günlüklerini kullanma
Günlük analizi bulut izler ve şirket içi ortamları kendi kullanılabilirliğini ve performansını korumak için bir Azure hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

Günlük analizi kullanmak için şunları yapmalısınız [günlük kaydını etkinleştir](#enable-logging-with-azure-storage) Azure günlük analizi çalışma alanı için hangi ele alınmıştır bu makalenin önceki bölümlerinde.

### <a name="using-the-log-analytics-workspace"></a>Günlük analizi çalışma alanını kullanma

 Aşağıdaki diyagramda, girdi mimarisi ve depo çıkışları gösterilmektedir:

![Log Analytics çalışma alanı](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Şekil 3 - günlük analizi deposu*

Yönetim çözümleri kullanarak çeşitli yollarla verileri görüntüleyebilirsiniz. Yönetim çözümlerinden edinebilirsiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Azure Marketi'nden yönetim çözümleri seçerek yükleyebilirsiniz **Şimdi Al** her çözümün altındaki bağlantıyı.

### <a name="add-a-log-analytics-cdn-management-solution"></a>Günlük analizi CDN yönetim çözümü ekleyin

Günlük analizi yönetim çözümü eklemek için aşağıdaki adımları izleyin:

1.   Azure aboneliğinizi kullanarak Azure portalında oturum açın ve panonuza gidin.
    ![Azure Panosu](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. İçinde **yeni** sayfasında **Market**seçin **izleme + Yönetim**.

    ![Market](./media/cdn-diagnostics-log/14_Marketplace.png)

3. İçinde **izleme + Yönetim** sayfasında, **tümünü görmek**.

    ![Tümünü incele](./media/cdn-diagnostics-log/15_See-all.png)

4. CDN arama kutusuna arayın.

    ![Tümünü incele](./media/cdn-diagnostics-log/16_Search-for.png)

5. Seçin **Azure CDN temel analiz**. 

    ![Tümünü incele](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. Siz seçtikten sonra **oluşturma**, yeni bir günlük analizi çalışma alanı oluşturun veya var olan bir kullanın istenir. 

    ![Tümünü incele](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Önce oluşturduğunuz çalışma alanını seçin. Ardından bir Otomasyon hesabı eklemeniz gerekir.

    ![Tümünü incele](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Aşağıdaki ekran doldurmak zorunda Otomasyon hesabı form gösterilmektedir. 

    ![Tümünü incele](./media/cdn-diagnostics-log/20_Automation.png)

9. Otomasyon hesabı oluşturduktan sonra çözümünüzü eklemek için hazır olursunuz. **Oluştur** düğmesini seçin.

    ![Tümünü incele](./media/cdn-diagnostics-log/21_Ready.png)

10. Çözümünüzü alanınıza şimdi eklendi. Azure portalı panonuza döndür.

    ![Tümünü incele](./media/cdn-diagnostics-log/22_Dashboard.png)

    Sunucunuzdan çalışma alanınıza gitmek için oluşturulan günlük analizi çalışma alanını seçin. 

11. Seçin **OMS portalı** yeni çözümünüz görmek için döşeme.

    ![Tümünü incele](./media/cdn-diagnostics-log/23_workspace.png)

12. Portalınızı, aşağıdaki ekran gibi görünmelidir:

    ![Tümünü incele](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Verilerinizi birkaç görünüm görmek için kutucukları birini seçin.

    ![Tümünü incele](./media/cdn-diagnostics-log/25_Interior-view.png)

    Daha fazla kutucukları tek bir görünüm verilerini temsil eden görmek için sola veya sağa kaydırma yapabilirsiniz. 

    Verileriniz hakkında daha fazla ayrıntı için kutucukları birini seçin.

     ![Tümünü incele](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Teklifler ve fiyatlandırma katmanları

Teklifler ve yönetim çözümleri için fiyatlandırma katmanlarına görebilirsiniz [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Görünümlerini özelleştirme

Kullanarak verilerinizi görünümünü özelleştirebilirsiniz **Görünüm Tasarımcısı**. Tasarlamaya için günlük analizi çalışma alanına gidin ve seçin **Görünüm Tasarımcısı** döşeme.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/27_Designer.png)

Sürükle ve bırak türlerini grafikler ve veri doldurun ayrıntıları, çözümlemek istediğiniz.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Günlük verileri gecikmeleri

Aşağıdaki tabloda günlük için veri gecikmeler gösterilmektedir **Azure CDN standart Microsoft**, **akamai'den Azure CDN standart**, ve **Azure CDN standart/Premium verizon'dan**.

Microsoft günlük veri gecikmeleri | Verizon günlük veri gecikmeleri | Akamai günlük veri gecikmeleri
--- | --- | ---
1 saatlik olarak ertelendi. | 1 saatlik olarak Gecikmeli ve uç nokta yayma işlemi tamamlandıktan sonra görünen başlatmak için 2 saat sürebilir. | 24 saat Gecikmeli; 24 saatten daha önce oluşturulmuş olsa bile, görünen başlatmak için 2 saat sürer. Yeni oluşturulduysa, görünen başlatmak günlükleri 25 saate kadar sürebilir.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>CDN temel analiz için tanılama günlük türleri

Microsoft, şu anda HTTP yanıtı istatistiklerinin ve çıkış istatistikleri görülen CDN POP/kenarlarından gösteren ölçümleri içeren çekirdek analiz günlükleri yalnızca sunar.

### <a name="core-analytics-metrics-details"></a>Çekirdek analytics ölçümleri ayrıntıları
Aşağıdaki tabloda, analiz günlükleri için çekirdek kullanılabilir ölçümleri listesini gösterir **Azure CDN standart Microsoft**, **akamai'den Azure CDN standart**, ve **Azure CDN standart/Premium verizon'dan**. Bu farklara en az olmasına rağmen tüm ölçümleri tüm sağlayıcılardan kullanılabilir. Belirli bir metrik sağlayıcıdan kullanılabilir olup olmadığını tabloyu de görüntüler. Ölçümleri trafiği üzerlerinde sahip CDN uç kullanılabilir.


|Ölçüm                     | Açıklama | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Bu süre boyunca isteği isabet toplam sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatus2xx | Bir 2xx HTTP kod (örneğin, 200, 202) sonuçlandı tüm isteklerin sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatus3xx | Bir 3xx HTTP kod (örneğin, 300, 302) sonuçlandı tüm isteklerin sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatus4xx | Bir 4xx HTTP kod (örneğin, 400, 404) sonuçlandı tüm isteklerin sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatus5xx | Bir 5xx HTTP kod (örneğin, 500, 504) sonuçlandı tüm isteklerin sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatusOthers | Diğer tüm HTTP kodları (dışında 2xx-5xx) sayısı. | Evet | Evet |Evet |
| RequestCountHttpStatus200 | 200 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| RequestCountHttpStatus206 | 206 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| RequestCountHttpStatus302 | Bir 302 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| RequestCountHttpStatus304 | 304 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| RequestCountHttpStatus404 | 404 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı. | Evet | Hayır  |Evet |
| RequestCountCacheHit | İsabet önbellekte sonuçlandı tüm isteklerin sayısı. Varlık doğrudan POP istemciye sunulduğu. | Evet | Evet | Hayır  |
| RequestCountCacheMiss | Önbellek isabetsizliği sonuçlandı tüm isteklerin sayısı. Önbellek isabetsizliği varlık istemcinin en yakın POP bulunamadı ve bu nedenle kaynaktan alınan anlamına gelir. | Evet | Evet | Hayır |
| RequestCountCacheNoCache | Bir varlık için bir kullanıcı yapılandırmasını kenar nedeniyle önbelleğe engellenir tüm isteklerin sayısı. | Evet | Evet | Hayır |
| RequestCountCacheUncacheable | Varlığın Cache-Control ve onu POP veya HTTP istemci tarafından önbelleğe alınması gereken değil olduğunu belirtmek Expires üstbilgileri tarafından önbelleğe engellenir varlıklar için tüm isteklerin sayısı. | Evet | Evet | Hayır |
| RequestCountCacheOthers | Tarafından yukarıda kapsanmayan önbellek durumundaki tüm isteklerin sayısı. | Hayır | Evet | Hayır  |
| EgressTotal | Giden veri aktarımı GB | Evet |Evet |Evet |
| EgressHttpStatus2xx | Giden veri aktarımı * 2xx HTTP durum kodları GB ile yanıtlar için. | Evet | Evet | Hayır  |
| EgressHttpStatus3xx | Giden veri yanıtları 3xx HTTP durum kodları GB ile aktarın. | Evet | Evet | Hayır  |
| EgressHttpStatus4xx | Giden veri yanıtları 4xx HTTP durum kodları GB ile aktarın. | Evet | Evet | Hayır  |
| EgressHttpStatus5xx | Giden veri yanıtları 5xx HTTP durum kodları GB ile aktarın. | Evet | Evet | Hayır |
| EgressHttpStatusOthers | Giden veri yanıtları diğer HTTP durum kodları GB ile aktarın. | Evet | Evet | Hayır  |
| EgressCacheHit | Giden veri teslim edilen yanıtlar için CDN POP/kenarları doğrudan CDN önbellekten aktarın. | Evet | Evet | Hayır |
| EgressCacheMiss. | Giden veri sırasında değil en yakın POP sunucusunda bulunan ve kaynak sunucudan alınan yanıt aktarın. | Evet | Evet | Hayır |
| EgressCacheNoCache | Bir kullanıcı yapılandırmasını kenar nedeniyle önbelleğe engellenir varlıklar için giden veri aktarımı. | Evet | Evet | Hayır |
| EgressCacheUncacheable | Varlığın Cache-Control ve/veya Expires üstbilgileri tarafından önbelleğe engellenir varlıklar için giden veri aktarımı. Bu POP veya HTTP istemci tarafından önbelleğe alınması gereken değil olduğunu gösterir. | Evet | Evet | Hayır |
| EgressCacheOthers | Giden veri diğer önbellek senaryolar için aktarır. | Hayır | Evet | Hayır |

* Giden veri aktarımı trafiği CDN POP sunucularından istemcisine teslim ifade eder.


### <a name="schema-of-the-core-analytics-logs"></a>Çekirdek analiz günlükleri şeması 

Tüm günlükler, JSON biçiminde depolanır ve her bir giriş alanlarına göre aşağıdaki şema vardır:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Burada *zaman* istatistikleri rapor edilen saat sınır başlangıç saatini gösterir. Ölçüm çift veya tamsayı değeri yerine bir CDN sağlayıcı tarafından desteklenmediğinde bir null değer yoktur. Bu null değer, bir ölçüm olmadığını gösteren ve 0 değerinden farklı. Bu ölçümleri noktadaki yapılandırılmış etki alanı başına bir dizi yoktur.

Örnek özellikleri:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Azure CDN ek Portalı aracılığıyla temel analiz](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure günlük analizi REST API'si](https://docs.microsoft.com/rest/api/loganalytics)







