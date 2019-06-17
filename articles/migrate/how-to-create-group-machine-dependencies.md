---
title: Makineleri Azure geçişi ile makine bağımlılıkları kullanan Grup | Microsoft Docs
description: Makine bağımlılıkları kullanan Azure geçişi hizmeti ile bir değerlendirme oluşturmayı açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: af47678b19209936aed86c132a8a3f400c3a7e8f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60596794"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Makine bağımlılık eşlemesi kullanan Grup makineleri

Bu makalede bu makine için bir grup oluşturmak nasıl [Azure geçişi](migrate-overview.md) makinelerin bağımlılıklarını görselleştirme tarafından değerlendirme. Makine bağımlılıklarını arası denetimi, bir değerlendirme çalıştırmadan önce göre daha yüksek güven düzeylerine sahip VM grupları değerlendirmek istediğiniz zaman genellikle bu yöntemi kullanın. Bağımlılık görselleştirmesi etkili bir şekilde azure'a geçişinizi planlamanıza yardımcı olabilir. Hiçbir şey geride bıraktığı ve Azure'a geçirirken şaşkınlık kesintiler meydana gelmediğinden emin olmanıza yardımcı olur. Geçişini birlikte yaptığınız ve çalışan bir sistemi hala kullanıcıya hizmet veren veya geçiş yerine yetkisinin alınması için bir adaydır olup olmadığını belirlemek için gereken tüm bağımlı sistemlere bulabilir.

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

## <a name="prepare-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için hazırlama
Azure geçişi, hizmet eşlemesi çözümünü'makineler için bağımlılık görselleştirme etkinleştirmek için Azure İzleyici günlüklerine yararlanır.

### <a name="associate-a-log-analytics-workspace"></a>Log Analytics çalışma alanını ilişkilendir
Bağımlılık görselleştirmesi yararlanmak için yeni veya var olan, Log Analytics çalışma alanı yeniden eşlemeniz gerekir ile bir Azure geçişi projesi. Yalnızca oluşturma veya geçiş projesi oluşturulduğu aynı Abonelikteki bir çalışma alanı ekleyin.

- İçinde bir proje için bir Log Analytics çalışma alanı eklemek için **genel bakış**Git **Essentials** projenin bölümünü tıklatın **yapılandırma gerektirir**

    ![Log Analytics çalışma alanını ilişkilendir](./media/concepts-dependency-visualization/associate-workspace.png)

- Bir çalışma alanı ilişkilendirilirken yeni bir çalışma alanı oluşturun veya mevcut bir paylaşımın seçeneği alırsınız:
  - Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı için bir ad belirtmeniz gerekir. Çalışma alanı aynı bölgede oluşturulduktan sonra [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) geçiş projesi olarak.
  - Mevcut bir çalışma alanı eklediğinizde, geçiş projesi ile aynı abonelikte kullanılabilir olan tüm çalışma arasından seçim yapabilirsiniz. Yalnızca bu çalışma alanlarının bir bölgede oluşturulan listelendiğine dikkat edin burada [hizmet eşlemesi desteklenir](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Bir çalışma alanı eklemek için çalışma alanına 'Reader' erişiminiz olduğundan emin olun.

> [!NOTE]
> Bir geçiş projesine ilişkili çalışma alanı değiştiremezsiniz.

### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme
Bir çalışma alanı yapılandırdıktan sonra aracılar değerlendirmek istediğiniz her bir şirket içi makinede yükleyip gerekir. İnternet bağlantısı olmayan makineleriniz varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [Log Analytics gateway](../azure-monitor/platform/gateway.md) bunlar üzerinde.

1. İçinde **genel bakış**, tıklayın **Yönet** > **makineler**, gerekli makineyi seçin.
2. İçinde **bağımlılıkları** sütun tıklayın **aracıları yüklemek**.
3. Üzerinde **bağımlılıkları** sayfasında indirin ve değerlendirmek istediğiniz her sanal makinede Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleyin.
4. Çalışma alanı kimliğini ve anahtarını kopyalayın. Şirket içi makinede MMA'yı yüklediğinizde bunlar gerekir.

> [!NOTE]
> Aracıların yüklenmesini otomatik hale getirmek için herhangi bir dağıtım aracı System Center Configuration Manager gibi kullanın veya iş ortağı aracımızı kullanın [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), Azure geçişi için bir aracı dağıtım çözümü vardır.

### <a name="install-the-mma"></a>MMA’yı yükleme

#### <a name="install-the-agent-on-a-windows-machine"></a>Aracı bir Windows makinesine yükleyin.

Bir Windows makinede aracı yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. İçinde **hedef klasör**, saklamak veya varsayılan yükleme klasörünü değiştirin > **sonraki**.
4. İçinde **Aracı Kurulum Seçenekleri**seçin **Azure Log Analytics** > **sonraki**.
5. Tıklayın **Ekle** yeni bir Log Analytics çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.

Komut satırı veya otomatikleştirilmiş bir yöntem gibi System Center Configuration Manager'ı kullanarak aracıyı yükleyebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) MMA aracısını yüklemek için bu yöntemleri kullanma hakkında.

#### <a name="install-the-agent-on-a-linux-machine"></a>Bir Linux makine üzerinde aracı yükleme

Bir Linux makinesinde aracıyı yüklemek için:

1. Uygun olan paketi (x86 veya x64), scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Paket kullanarak yükleme yükleme bağımsız değişken.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) hakkında MMA tarafından Linux işletim sistemleri desteği listesi.

#### <a name="install-the-agent-on-a-machine-monitored-by-scom"></a>SCOM tarafından izlenen bir makinedeki aracıyı yükleyin.

System Center Operations Manager 2012 R2 veya üzeri izlenen makineler için MMA aracısını yüklemek için gerek yoktur. Hizmet eşlemesi, gerekli bağımlılık verileri toplamak için SCOM MMA'yı yararlanan SCOM ile bir tümleştirmeye sahiptir. Tümleştirme yönergeleri kullanarak etkinleştirebilirsiniz [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Ancak, bu makinelerde yüklü bağımlılık Aracısı'nı gerektiğini unutmayın.


### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme
1. Bir Windows makinede bağımlılık Aracısı'nı yüklemek için kurulum dosyasına çift tıklayın ve sihirbazı izleyin.
2. Bağımlılık Aracısı'nı bir Linux makineye yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

Bağımlılık Aracısı desteği hakkında daha fazla bilgi [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) ve [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) işletim sistemleri.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) bağımlılık Aracısı'nı yüklemek için komut dosyalarını nasıl kullanabileceğiniz hakkında.


## <a name="create-a-group"></a>Grup oluşturma

1. Aracıları yükledikten sonra portal ve tıklayın Git **Yönet** > **makineler**.
2. Aracıların yüklü olduğu makinenin arayın.
3. **Bağımlılıkları** makine için sütun olarak artık göstermelidir **bağımlılıklarını görüntüleme**. Sütun makinenin bağımlılıklarını görüntülemek için tıklayın.
4. Makine bağımlılık eşlemesi aşağıdaki ayrıntıları gösterir:
    - (İstemciler) gelen ve giden (sunucu) TCP bağlantıları/makineden
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarını tarafından gruplandırılır.
        - MMA ve bağımlılık aracısının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - İşlemler, makinenin içinde çalışan işlemleri görüntülemek için her makine kutusunu genişletebilirsiniz.
    - Tam etki alanı adı, işletim sistemi, her makinenin MAC adresi vb. gibi özellikler, bu ayrıntıları görüntülemek için her makine kutusuna tıklayın

      ![Makine bağımlılıklarını görüntüleme](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Zaman aralığı etikette süre tıklayarak için farklı süreler sırasında bağımlılıkları bakabilirsiniz. Varsayılan olarak aralığı bir saattir. Zaman aralığı değiştirmek veya başlangıç ve bitiş tarihlerini ve süresini belirtin.

   > [!NOTE]
   >    Şu anda, bağımlılık görselleştirmesi UI bir saatten uzun bir zaman aralığı seçimini desteklemez. Azure İzleyicisi'ni oturumu [bağımlılık verileri sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) üzerinden uzun bir süre.

5. Gruplamak istediğiniz bağımlı makineler tanımladıktan sonra harita üzerinde birden çok makine seçin ve Ctrl + tıklama kullanın **Grup makineleri**.
6. Bir grup adı belirtin. Bağımlı makinelere Azure geçişi tarafından bulunduğundan emin olun.

    > [!NOTE]
    > Bağımlı bir makine Azure geçişi tarafından bulunamadı, gruba eklenemiyor. Tür makineler gruba eklemek için doğru kapsamda vCenter sunucusu ile yeniden bulma işlemini çalıştırın ve Azure geçişi tarafından makine bulunduğundan emin olun gerekir.  

7. Bu grup için bir değerlendirme oluşturmak istiyorsanız, grup için yeni bir değerlendirme oluşturmak için onay kutusunu seçin.
8. Tıklayın **Tamam** grubunu kaydetmek için.

Grup oluşturulduktan sonra grubun tüm makinelerde aracıları yüklemek ve tüm Grup bağımlılığı görselleştirerek grubu geliştirmek için önerilir.

## <a name="query-dependency-data-from-azure-monitor-logs"></a>Azure İzleyici günlüklerine bağımlılık verileri Sorgulama

Hizmet eşlemesi tarafından yakalanan bağımlılık verileri sorgulamak için Azure geçişi projenizle ilişkili Log Analytics çalışma alanında kullanılabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) hizmet eşlemesi veri tabloları, Azure İzleyici'de sorgulamak için ilgili günlüğe kaydeder. 

Kusto sorguları çalıştırmak için:

1. Aracıları yükledikten sonra portal ve tıklayın Git **genel bakış**.
2. İçinde **genel bakış**Git **Essentials** yanındaki sağlanan çalışma alanı adına tıklayın ve proje bölümünü **OMS çalışma alanı**.
3. Log Analytics çalışma alanı sayfasında tıklayın **genel** > **günlükleri**.
4. Azure İzleyici günlüklerine kullanarak bağımlılık veri toplamak üzere sorgunuzu yazın. Örnek sorgular, sonraki bölümde bulun.
5. Sorguyu Çalıştır'ı tıklayarak çalıştırın. 

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) Kusto sorguları yazma hakkında. 

### <a name="sample-azure-monitor-logs-queries"></a>Örnek Azure İzleyici sorguları günlüğe kaydeder.

Bağımlılık verileri ayıklamak için kullanabileceğiniz örnek sorgular aşağıda verilmiştir. Tercih edilen veri noktalarınızı ayıklamak için sorguları değiştirebilirsiniz. Kapsamlı bir liste bağımlılık veri kayıtlarının alanların kullanılabilir [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records). Daha fazla örnek sorguları bulmak [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches).

#### <a name="summarize-inbound-connections-on-a-set-of-machines"></a>Bir küme makinede gelen bağlantıları özetleme

İçin bağlantı ölçümü, VMConnection, tablodaki kayıtları tek tek bir fiziksel ağ bağlantıları temsil etmiyor unutmayın. Birden fazla fiziksel ağ bağlantıları, mantıksal bir bağlantı içinde gruplandırılır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) VMConnection tek bir mantıksal kayıt içine nasıl fiziksel ağ bağlantısı hakkında veriler toplanır. 

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="summarize-volume-of-data-sent-and-received-on-inbound-connections-between-a-set-of-machines"></a>Gelen bağlantılar makineler kümesi arasında alınan veri hacmi özetleme

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z); 
VMConnection
| where Direction == 'inbound' 
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) bağımlılık görselleştirme hakkında sık sorulan sorular hakkında.
- [Bilgi nasıl](how-to-create-group-dependencies.md) Grup bağımlılıklarını görselleştirerek grubu geliştirmek için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
