---
title: "Azure tanılama günlüklerini | Microsoft Docs"
description: "Müşteri, Günlük çözümlemesi için Azure CDN etkinleştirebilirsiniz."
services: cdn
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: v-deasim
ms.openlocfilehash: 3e8727e80571be70124fb439f4c7e448f521b692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri, temel analiz görüntülemek ve bunları dahil olmak üzere bir veya daha fazla hedefleri kaydedin:

 - Azure Storage hesabı
 - Azure Event Hubs
 - [OMS günlük analizi deposu](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Bu özellik, Verizon (standart ve Premium) ve Akamai (standart) CDN profilleri ait tüm CDN uç noktası için kullanılabilir. 

Azure tanılama günlükleri, özelleştirilmiş bir biçimde tüketebileceği temel kullanım ölçümlerini CDN uç noktasından çeşitli kaynakları dışa olanak tanır. Örneğin, aşağıdaki veri verme türlerini yapabilirsiniz:

- Blob depolama, CSV için dışarı aktarma ve Excel'de grafikleri oluşturmak için verileri dışa aktarın.
- Verileri Event Hubs'a verin ve diğer Azure hizmetleriyle verilerle ilişkilendirmek.
- Analytics oturum ve kendi OMS çalışma alanınızda verileri görüntülemek için verileri dışarı aktarma

Aşağıdaki şekilde verilerinin normal CDN çekirdek analytics görünümü gösterilmektedir.

![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Şekil 1 - CDN çekirdek analytics görünümü*

Tanılama günlükleri hakkında daha fazla bilgi için bkz: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-azure-portal"></a>Azure portal ile günlük kaydını etkinleştir

CDN çekirdek analytics ile günlüğü bu adımları etkinleştir izleyin:

[Azure Portal](http://portal.azure.com) oturum açın. İş akışınız için etkin CDN zaten yoksa [Azure CDN'yi etkinleştirme](cdn-create-new-endpoint.md) devam etmeden önce.

1. Portalı'nda gidin **CDN profili**.
2. CDN profili seçin ve ardından etkinleştirmek istediğiniz CDN uç noktası **tanılama günlükleri**.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Seçin **tanılama günlükleri** içinde **izleme** bölümü.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Azure Storage ile günlük kaydını etkinleştir
    
1. Azure depolama günlükleri depolamak için kullanmak için **bir depolama hesabı arşive**seçin **CoreAnalytics**ve ardından altında bekletme gün sayısını seçin **bekletme (gün)**. Sıfır gün bekletme günlükleri süresiz olarak depolar. 
2. Ayarınız için bir ad girin ve ardından **depolama hesabı**. Bir depolama hesabı seçtikten sonra tıklatın **kaydetmek**.

![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Şekil 2 - Azure Storage ile günlüğe kaydetme*

### <a name="logging-with-oms-log-analytics"></a>OMS günlük analizi ile günlüğe kaydetme

Günlükleri depolamak için OMS günlük analizi kullanmak için aşağıdaki adımları izleyin:

1. Gelen **tanılama günlükleri** dikey penceresinde, select **için günlük analizi Gönder**. 

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Tıklatın **yapılandırma** günlük analizi günlüğe kaydetmeyi yapılandırmak için. OMS çalışma alanlarını iletişim kutusunda önceki bir çalışma alanı seçin veya yeni bir tane oluşturun.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Tıklatın **yeni çalışma alanı oluşturma**.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/07_Create-new.png)

4. Yeni bir OMS çalışma alanı adı girin. Bir OMS çalışma alanı adı benzersiz olmalı ve yalnızca harf, rakam ve kısa çizgi içerebilir; boşluk ve alt çizgi izin verilmiyor. 
5. Ardından, var olan abonelik, kaynak grubu (yeni veya var olan), konum ve fiyatlandırma katmanı seçin. Ayrıca bu yapılandırma, Pano için sabitleme seçeneğiniz vardır. Tıklatın **Tamam** yapılandırmayı tamamlamak için.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5.  Çalışma alanınızı oluşturulduktan sonra tanılama günlüklerini windows döndürülür. Yeni günlük analizi çalışma alanı adını doğrulayın.

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Günlük analizi yapılandırması ayarladıktan sonra seçtiğinizden emin olun **CoreAnalytics**.

6. **Kaydet** düğmesine tıklayın.

7. Yeni bir OMS çalışma alanınızı görüntülemek için Azure portalı panonuza gidin ve günlük analizi çalışma alanınız adına tıklayın. Çalışma alanınızı OMS depoya görüntülemek için OMS portalı kutucuğa tıklayın. 

    ![Portal - tanılama günlükleri](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    OMS deponuz verileri günlüğe kaydetmek artık hazırdır. Bu verileri kullanmak için kullanmalısınız bir [OMS çözüm](#consuming-oms-log-analytics-data), bu makalenin sonraki bölümlerinde kapsanan.

Günlük verileri gecikmeler hakkında daha fazla bilgi için bkz: [oturum veri gecikmeler](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlük kaydını etkinleştir

Aşağıdaki örnek, Azure PowerShell cmdlet'leri aracılığıyla tanılama günlüklerini etkinleştirme gösterilmektedir.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Bir depolama hesabında oturum tanılama etkinleştirme

İlk oturum açın ve bir abonelik seçin:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


Etkinleştirme bir depolama hesabında tanılama günlükleri için aşağıdaki komutu kullanın:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
Etkinleştirme tanılama günlükleri için bir OMS çalışma alanında, bu komutu kullanın:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Azure depolama biriminden tanılama günlüklerini kullanma
Bu bölümde CDN temel analiz şeması açıklanmaktadır nasıl içinde Azure storage hesabı düzenlenir ve bir CSV dosyasında günlükleri indirmek için örnek kod sağlar.

### <a name="using-microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Gezgini kullanma
Temel analiz verileri Azure depolama hesabından erişebilmeniz için önce bir depolama hesabı içeriğine erişmek için bir aracı ilk gerekir. Varken çeşitli araçlar kullanılabilir pazarında, önerdiğimiz Microsoft Azure Storage Gezgini adrestir. Aracı indirmek için bkz: [Azure Storage Gezgini](http://storageexplorer.com/). İndirme ve yazılım yükleme sonrasında CDN tanılama günlükleri için hedef olarak yapılandırılan aynı Azure depolama hesabı kullanacak şekilde yapılandırın.

1.  Açık **Microsoft Azure Storage Gezgini**
2.  Depolama hesabını bulun
3.  Git **"Blob kapsayıcıları"** düğümü altında bu depolama hesabı ve düğümünü genişletin
4.  Adlı kapsayıcıyı seçin **"Öngörüler günlükleri coreanalytics"** ve çift tıklatın
5.  Gibi görünen ilk düzeyi ile başlayan sağ taraftaki bölmede Göster sonuçları **"ResourceId ="**. Tüm dosya görene kadar tıklayarak devam **PT1H.json**. Yolun açıklaması için aşağıdaki nota bakın.
6.  Her bir blob **PT1H.json** belirli bir CDN uç noktası veya kendi özel etki alanı için bir saat analiz günlükleri temsil eder.
7.  Bu JSON dosyasının içeriğini şeması bölümünde çekirdek analiz günlüklerinin şema açıklanan


> [!NOTE]
> **BLOB yol biçimi**
> 
> Çekirdek analiz günlükleri her saat oluşturulur ve veriler toplanır ve tek bir Azure blob iç bir JSON yükü olarak depolanır. Depolama Gezgini araçları Yorumlar '/' dizin ayırıcı ve programları hiyerarşisi Azure blob yolu hiyerarşik bir yapı olduğu gibi görünür olduğundan ve blob adını temsil eder. Blob adını aşağıdaki adlandırma kuralını izler: 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Alanları açıklaması:**

|değer|açıklama|
|-------|---------|
|Abonelik Kimliği    |GUID biçiminde Azure abonelik kimliği.|
|Kaynak |Grup adı CDN kaynaklara ait olduğu kaynak grubu.|
|Profil adı |CDN profili adı|
|Uç nokta adı |CDN uç noktası adı|
|Yıl|  4 rakamlı yıl, örneğin, 2017 gösterimi|
|Ay| ay sayısı 2 basamaklı gösterimi. 01 Ocak =... 12 Aralık =|
|Gün|   Ayın günü 2 basamaklı gösterimi|
|PT1H.JSON| Analiz verilerinin depolandığı gerçek JSON dosyası|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Temel analiz verileri bir CSV dosyasına dışarı aktarma

Temel analiz erişim kolaylaştırmak için örnek kod bir araç için sağlanır. Bu araç, kolayca grafikleri veya diğer toplamaları oluşturmak için kullanılan bir düz virgülle ayrılmış dosya biçimine JSON dosyaları indirme sağlar.

Aracı'nı nasıl kullanabileceğiniz aşağıda verilmiştir:

1.  Github bağlantıyı ziyaret edin: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Kodu indirme.
3.  Derleme ve yapılandırmak için yönergeleri izleyin.
4.  Aracı çalıştırın.
5.  Sonuçta elde edilen CSV dosyasını analiz verileri basit bir düz hiyerarşi içinde gösterir.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Tanılama günlüklerini bir OMS günlük analizi depodan kullanma
Günlük analizi içinde Operations Management Suite (bulut izler ve şirket içi kendi kullanılabilirliğini ve performansını korumak için ortamları OMS) bir hizmettir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

Günlük analizi kullanmak için şunları yapmalısınız [günlük kaydını etkinleştir](#enable-logging-with-azure-storage) Azure OMS günlük analizi depoya hangi ele alınmıştır bu makalenin önceki bölümlerinde.

### <a name="using-the-oms-repository"></a>OMS deposunu kullanma

 Aşağıdaki diyagramda, girdi mimarisi ve depo çıkışları gösterilmektedir:

![OMS günlük analizi deposu](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Şekil 3 - günlük analizi deposu*

Yönetim çözümleri kullanarak çeşitli yollarla verileri görüntüleyebilirsiniz. Yönetim çözümlerinden edinebilirsiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Tıklayarak yönetim çözümleri Azure Marketi'nden yükleyebilirsiniz **Şimdi Al** her çözümün altındaki bağlantıyı.

### <a name="adding-an-oms-cdn-management-solution"></a>OMS CDN yönetim çözümünü ekleme

Bir yönetim çözümü eklemek için aşağıdaki adımları izleyin:

1.   Zaten yapmadıysanız, Azure aboneliğinizi kullanarak Azure portalında oturum açın ve panonuza gidin.
    ![Azure Panosu](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. İçinde **yeni** altında dikey **Market**seçin **izleme + Yönetim**.

    ![Market](./media/cdn-diagnostics-log/14_Marketplace.png)

3. İçinde **izleme + Yönetim** dikey penceresinde tıklatın **tümünü görmek**.

    ![Tümünü incele](./media/cdn-diagnostics-log/15_See-all.png)

4.  CDN arama kutusuna arayın.

    ![Tümünü incele](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Seçin **Azure CDN temel analiz**. 

    ![Tümünü incele](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  ' I tıklattıktan sonra **oluşturma**, yeni bir OMS çalışma alanı oluşturun veya var olan bir kullanın istenir. 

    ![Tümünü incele](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Önce oluşturduğunuz çalışma alanını seçin. Ardından bir Otomasyon hesabı eklemeniz gerekir.

    ![Tümünü incele](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Aşağıdaki ekran doldurmak zorunda Otomasyon hesabı form gösterilmektedir. 

    ![Tümünü incele](./media/cdn-diagnostics-log/20_Automation.png)

9. Otomasyon hesabı oluşturduktan sonra çözümünüzü eklemek için hazır olursunuz. **Oluştur** düğmesine tıklayın.

    ![Tümünü incele](./media/cdn-diagnostics-log/21_Ready.png)

10. Çözümünüzü alanınıza şimdi eklendi. Azure portalı panonuza döndür.

    ![Tümünü incele](./media/cdn-diagnostics-log/22_Dashboard.png)

    Sunucunuzdan çalışma alanınıza gitmek için oluşturulan günlük analizi çalışma alanı'ı tıklatın. 

11. Tıklatın **OMS portalı** yeni çözümünüz OMS portalında görmek için döşeme.

    ![Tümünü incele](./media/cdn-diagnostics-log/23_workspace.png)

12. OMS portalı, aşağıdaki ekran gibi görünmelidir:

    ![Tümünü incele](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Verilerinizi birkaç görünüm görmek için kutucukları birini tıklatın.

    ![Tümünü incele](./media/cdn-diagnostics-log/25_Interior-view.png)

    Daha fazla kutucukları tek bir görünüm verilerini temsil eden görmek için sola veya sağa kaydırma yapabilirsiniz. 

    Döşeme birine tıklayarak verileriniz hakkında daha fazla ayrıntı sağlar.

     ![Tümünü incele](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Teklifler ve fiyatlandırma katmanları

Teklifler ve OMS yönetim çözümleri için fiyatlandırma katmanlarına görebilirsiniz [burada](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Görünümlerini özelleştirme

Kullanarak verilerinizi görünümünü özelleştirebilirsiniz **Görünüm Tasarımcısı**. Tasarlamaya için OMS çalışma alanına gidin ve **Görünüm Tasarımcısı** döşeme.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/27_Designer.png)

Sürükleme ve grafik türleri bırakma ve analiz etmek istediğiniz veri ayrıntıları doldurun.

![Görünüm Tasarımcısı](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Günlük verileri gecikmeleri

Verizon günlük veri gecikmeleri | Akamai günlük veri gecikmeleri
--- | ---
Verizon günlük verilerini 1 saat Gecikmeli ve uç nokta yayma işlemi tamamlandıktan sonra görünen başlatmak için 2 saat sürebilir. | Akamai günlük verilerini tarafından 24 saat ertelendi; 24 saatten daha önce oluşturulmuş olsa bile, görünen başlatmak için 2 saat sürer. Yeni oluşturulduysa, görünen başlatmak günlükleri 25 saate kadar sürebilir.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>CDN temel analiz için tanılama günlük türleri

HTTP yanıtı istatistiklerinin ve görülen çıkış istatistikleri CDN POP/kenarlarından gösteren ölçümleri içeren yalnızca çekirdek analiz günlükleri, şu anda sunuyoruz.

### <a name="core-analytics-metrics-details"></a>Çekirdek analytics ölçümleri ayrıntıları
Aşağıdaki tabloda ölçümleri çekirdek analytics günlükleri'nde bulunan bir listesini gösterir. Bu farklara en az olmasına rağmen tüm ölçümleri tüm sağlayıcılardan kullanılabilir. Aşağıdaki tabloda, ayrıca belirli bir metrik sağlayıcıdan kullanılabilir olup olmadığını gösterir. Ölçümleri trafiği üzerlerinde sahip CDN uç için kullanılabilir olduğunu unutmayın.


|Ölçüm                     | Açıklama   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Bu süre boyunca isteği isabet toplam sayısı| Evet  |Evet   |
| RequestCountHttpStatus2xx |Bir 2xx HTTP kod (örneğin, 200, 202) sonuçlandı tüm isteklerin sayısı              | Evet  |Evet   |
| RequestCountHttpStatus3xx | Bir 3xx HTTP kod (örneğin, 300, 302) sonuçlandı tüm isteklerin sayısı              | Evet  |Evet   |
| RequestCountHttpStatus4xx |Bir 4xx HTTP kod (örneğin, 400, 404) sonuçlandı tüm isteklerin sayısı               | Evet   |Evet   |
| RequestCountHttpStatus5xx | Bir 5xx HTTP kod (örneğin, 500, 504) sonuçlandı tüm isteklerin sayısı              | Evet  |Evet   |
| RequestCountHttpStatusOthers |  Diğer tüm HTTP kodları (dışında 2xx-5xx) sayısı | Evet  |Evet   |
| RequestCountHttpStatus200 | 200 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı              |Hayır   |Evet   |
| RequestCountHttpStatus206 | 206 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı              |Hayır   |Evet   |
| RequestCountHttpStatus302 | Bir 302 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı              |Hayır   |Evet   |
| RequestCountHttpStatus304 |  304 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı             |Hayır   |Evet   |
| RequestCountHttpStatus404 | 404 HTTP kod yanıtıyla sonuçlanan tüm isteklerin sayısı              |Hayır   |Evet   |
| RequestCountCacheHit |Önbelleği isabet sonuçlandı tüm isteklerin sayısı. Varlık doğrudan POP istemciye sunulduğu.               | Evet  |Hayır   |
| RequestCountCacheMiss | Önbellek isabetsizliği sonuçlandı tüm isteklerin sayısı. Bu varlık istemcinin en yakın POP bulunamadı ve bu nedenle kaynaktan alınan anlamına gelir.              |Evet   | Hayır  |
| RequestCountCacheNoCache | Bir varlık için bir kullanıcı yapılandırmasını kenar nedeniyle önbelleğe engellenir tüm isteklerin sayısı.              |Evet   | Hayır  |
| RequestCountCacheUncacheable | Varlığın Cache-Control ve onu POP veya HTTP istemci tarafından önbelleğe alınması gereken değil olduğunu belirtmek Expires üstbilgileri tarafından önbelleğe engellenir varlıklar için tüm isteklerin sayısı                |Evet   |Hayır   |
| RequestCountCacheOthers | Tarafından yukarıda kapsanmayan önbellek durumundaki tüm isteklerin sayısı.              |Evet   | Hayır  |
| EgressTotal | Giden veri aktarımı GB              |Evet   |Evet   |
| EgressHttpStatus2xx | Giden veri aktarımı * 2xx HTTP durum kodları GB ile yanıtlar için            |Evet   |Hayır   |
| EgressHttpStatus3xx | 3xx HTTP durum kodları GB ile yanıtlar için giden veri aktarımı              |Evet   |Hayır   |
| EgressHttpStatus4xx | 4xx HTTP durum kodları GB ile yanıtlar için giden veri aktarımı               |Evet   | Hayır  |
| EgressHttpStatus5xx | 5xx HTTP durum kodları GB ile yanıtlar için giden veri aktarımı               |Evet   |  Hayır |
| EgressHttpStatusOthers | Giden veri aktarımı için diğer HTTP durum kodları GB yanıtları                |Evet   |Hayır   |
| EgressCacheHit |  Giden veri teslim edilen yanıtlar için doğrudan CDN önbellekten CDN POP/kenarları aktarımı  |Evet   |  Hayır |
| EgressCacheMiss | Giden veri aktarımı sırasında değil en yakın POP sunucusunda bulunan ve kaynak sunucudan alınan yanıt için              |Evet   |  Hayır |
| EgressCacheNoCache | Bir kullanıcı yapılandırmasını kenar nedeniyle önbelleğe engellenir varlıklar için giden veri aktarımı.                |Evet   |Hayır   |
| EgressCacheUncacheable | Varlığın Cache-Control ve/veya Expires üstbilgileri tarafından önbelleğe engellenir varlıklar için giden veri aktarımı. Bu POP veya HTTP istemci tarafından önbelleğe alınması gereken değil olduğunu gösterir.                   |Evet   | Hayır  |
| EgressCacheOthers |  Giden veri diğer önbellek senaryolar için aktarır.             |Evet   | Hayır  |

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

Burada 'saati' istatistikleri bildirilen saat sınır Başlangıç saati gösterir. Ölçüm çift veya tamsayı değeri yerine bir CDN sağlayıcı tarafından desteklenmediğinde bir null değer yoktur. Bu null değer, bir ölçüm olmadığını gösteren ve 0 değerinden farklı. Bu ölçümleri noktadaki yapılandırılmış etki alanı başına bir dizi yoktur.

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
* [Azure OMS günlük analizi](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [Azure günlük analizi REST API'si](https://docs.microsoft.com/en-us/rest/api/loganalytics)







