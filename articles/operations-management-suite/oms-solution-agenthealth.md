---
title: "OMS’de Aracı Durumu çözümü | Microsoft Docs"
description: "Bu makale, doğrudan OMS veya System Center Operations Manager’a bildirimde bulunan aracılarınızın durumunu izlemek için bu çözümü nasıl kullanabileceğinizi anlamanıza yardımcı olmak için hazırlanmıştır."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: b810e37e393ddab55500f636b72450789285a4f0
ms.contentlocale: tr-tr
ms.lasthandoff: 07/28/2017

---
#  <a name="agent-health-solution-in-oms"></a>OMS’de Aracı Durumu çözümü
OMS’deki Aracı Durumu çözümü, doğrudan bir OMS çalışma alanına veya OMS’ye bağlı bir System Center Operations Manager yönetim grubuna bildirimde bulunan, yanıt vermeyen ve işletimsel veriler gönderen tüm aracıları anlamanıza yardımcı olur.  Ayrıca, kaç aracının dağıtıldığını, bunların coğrafi olarak nerelere dağıtıldığını da izleyebilir ve Azure’da, diğer bulut ortamlarında ya da şirket içinde dağıtılmış aracıların dağılımından her zaman haberdar olmaya yönelik diğer sorguları gerçekleştirebilirsiniz.    

## <a name="prerequisites"></a>Ön koşullar
Bu çözümü dağıtmadan önce, OMS çalışma alanına veya OMS çalışma alanınızla tümleşik bir [Operations Manager yönetim grubuna](../log-analytics/log-analytics-om-agents.md) bildirimde bulunan ve geçerli olarak desteklenen [Windows aracılarınız](../log-analytics/log-analytics-windows-agents.md) olduğunu doğrulayın.    

## <a name="solution-components"></a>Çözüm bileşenleri
Bu çözüm, çalışma alanınıza eklenen aşağıdaki kaynaklardan ve doğrudan bağlanılan aracılardan veya Operations Manager bağlantılı yönetim grubundan oluşur.

### <a name="management-packs"></a>Yönetim paketleri
System Center Operations Manager yönetim grubunuz OMS çalışma alanına bağlıysa, Operation Manager’a aşağıdaki yönetim paketleri yüklenir.  Bu çözüm eklendikten sonra bu yönetim paketleri doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerinde yapılandırılacak veya yönetilecek hiçbir şey yoktur.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../log-analytics/log-analytics-om-agents.md).

## <a name="configuration"></a>Yapılandırma
[Çözüm ekleme](../log-analytics/log-analytics-add-solutions.md) bölümünde açıklanan işlemi kullanarak Aracı Durumu çözümünü OMS çalışma alanınıza ekleyin. Başka bir yapılandırma işlemi gerekmez.


## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları | Evet | Sinyal olayları doğrudan Windows aracılarından toplanır.|
| System Center Operations Manager yönetim grubu | Evet | Sinyal olayları, 60 saniyede bir yönetim grubuna bildirimde bulunan aracılardan toplanır ve sonra Log Analytics’e iletilir. Operations Manager aracılarının doğrudan Log Analytics’e bağlanması gerekmez. Sinyal olay verileri yönetim grubundan Log Analytics deposuna iletilir.|

## <a name="using-the-solution"></a>Çözümü kullanma
Çözümü OMS çalışma alanınıza eklediğinizde OMS panonuza **Aracı Durumu** kutucuğu eklenir. Bu kutucuk, son 24 saat içindeki toplam aracı sayısını ve yanıt vermeyen aracı sayısını gösterir.<br><br> ![Panodaki Aracı Durumu Çözüm kutucuğu](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

**Aracı Durumu** kutucuğuna tıklayarak **Aracı Durumu** panosunu açın.  Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütunda, ilgili sütunun belirtilen zaman aralığına ilişkin ölçütlerle eşleşen ilk on olay sayılarına göre listelenir. Her sütunun sağ alt tarafındaki **Tümünü görüntüle**’yi seçerek ya da sütun başlığına tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|--------|-------------|
| Zaman içinde aracı sayısı | Hem Linux hem de Windows aracıları için yedi günlük bir dönem boyunca aracı sayınızın eğilimi.|
| Yanıt vermeyen aracı sayısı | Son 24 saat içinde bir sinyal göndermemiş tüm aracıların listesi.|
| İşletim Sistemi Türüne Göre Dağılım | Ortamınızda kaçar tane Windows ve Linux aracısı olduğuna ilişkin bir bölüm.|
| Aracı Sürümüne Göre Dağılım | Ortamınızda yüklü olan farklı aracı sürümlerine ve her birinin sayısına ilişkin bir bölüm.|
| Aracı Kategorisine Göre Dağılım | Sinyal olayları gönderen farklı kategorilerdeki araçlara ilişkin bir bölüm: Doğrudan aracılar, OpsMgr aracıları veya OpsMgr Management Server.|
| Yönetim Grubuna Göre Dağılım | Ortamınızdaki farklı SCOM Yönetim gruplarına ilişkin bir bölüm.|
| Aracıların coğrafi konumu | Aracılarınızın bulunduğu farklı ülkelere ve her ülkede yüklü olan toplam aracı sayısına ilişkin bir bölüm.|
| Yüklü Ağ Geçidi Sayısı | OMS Ağ Geçidi’nin yüklü olduğu sunucu sayısı ve bu sunucuların listesi.|

![Aracı Durumu Çözüm panosu örneği](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Çözüm, OMS deposunda bir tür kayıt oluşturur.  

### <a name="heartbeat-records"></a>Sinyal kayıtları
**Sinyal** türünde bir kayıt oluşturulur.  Bu kayıtlar aşağıdaki tabloda gösterilen özelliklere sahiptir.  

| Özellik | Açıklama |
| --- | --- |
| Tür | *Sinyal*|
| Kategori | Değer *Doğrudan Aracı*, *SCOM Aracısı* veya *SCOM Yönetim Sunucusu*’dur.|
| Bilgisayar | Bilgisayar adı.|
| OSType | Windows veya Linux işletim sistemi.|
| OSMajorVersion | İşletim sistemi ana sürümü.|
| OSMinorVersion | İşletim sistemi alt sürümü.|
| Sürüm | OMS Aracısı veya Operations Manager Aracısı sürümü.|
| SCAgentChannel | Değer *Doğrudan* ve/veya *SCManagementServer*’dır.|
| IsGatewayInstalled | OMS Ağ Geçidi yüklüyse değer *true*, aksi takdirde *false* olur.|
| ComputerIP | Bilgisayarın IP adresi.|
| RemoteIPCountry | Bilgisayarın dağıtıldığı coğrafi konum.|
| ManagementGroupName | Operations Manager yönetim grubunun adı.|
| SourceComputerId | Bilgisayarın benzersiz kimliği.|
| RemoteIPLongitude | Bilgisayarın coğrafi konumunun boylamı.|
| RemoteIPLatitude | Bilgisayarın coğrafi konumunun enlemi.|

Bir Operations Manager yönetim sunucusuna bildirimde bulunan her bir aracı iki sinyal gönderir ve OMS aboneliğinizde hangi Log Analytics veri kaynaklarını ve çözümlerini etkinleştirdiğinize bağlı olarak SCAgentChannel özelliğinin değeri hem **Doğrudan** hem de **SCManagementServer**’ı içerir. Geri çağırırsanız, çözümlerden toplanan veriler doğrudan bir Operations Manager yönetim sunucusundan OMS web hizmetine gönderilir ya da aracıda toplanan verilerin hacmi nedeniyle doğrudan aracıdan OMS web hizmetine gönderilir. **SCManagementServer** değerine sahip sinyal olayları için ComputerIP değeri, verileri aslında karşıya yükleyen yönetim sunucusunun IP adresidir.  SCAgentChannel’ın **Doğrudan** olarak ayarlandığı sinyaller için bu adres, aracının genel IP adresidir.  

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan kayıtlara ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
| --- | --- |
| Type=Heartbeat &#124; distinct Computer |Toplam aracı sayısı |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-24HOURS |Son 24 saat içinde yanıt vermeyen aracı sayısı |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-15MINUTES |Son 15 dakika içinde yanıt vermeyen aracı sayısı |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Çevrimiçi olan bilgisayarlar (son 24 saat) |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Son 30 dakika içinde Toplam Çevrimdışı Aracı Sayısı (son 24 saat için) |
| Type=Heartbeat &#124; measure countdistinct(Computer) by OSType |İşletim sistemi türüne göre zaman içinde aracı sayısı eğilimini görün|
| Type=Heartbeat&#124;measure countdistinct(Computer) by OSType |İşletim Sistemi Türüne Göre Dağılım |
| Type=Heartbeat&#124;measure countdistinct(Computer) by Version |Aracı Sürümüne Göre Dağılım |
| Type=Heartbeat&#124;measure count() by Category |Aracı Kategorisine Göre Dağılım |
| Type=Heartbeat&#124;measure countdistinct(Computer) by ManagementGroupName | Yönetim Grubuna Göre Dağılım |
| Type=Heartbeat&#124;measure countdistinct(Computer) by RemoteIPCountry |Aracıların coğrafi konumu |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |Yüklü OMS Ağ Geçidi Sayısı |


>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](../log-analytics/log-analytics-log-search-upgrade.md) yükseltilmişse, yukarıdaki sorguların aşağıdaki gibi değiştirilmesi gerekir.
>
>| Sorgu | Açıklama |
|:---|:---|
| Heartbeat &#124; distinct Computer |Toplam aracı sayısı |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Son 24 saat içinde yanıt vermeyen aracı sayısı |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Son 15 dakika içinde yanıt vermeyen aracı sayısı |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Çevrimiçi olan bilgisayarlar (son 24 saat) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Son 30 dakika içinde Toplam Çevrimdışı Aracı Sayısı (son 24 saat için) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |İşletim sistemi türüne göre zaman içinde aracı sayısı eğilimini görün|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |İşletim Sistemi Türüne Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Aracı Sürümüne Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Aracı Kategorisine Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Yönetim Grubuna Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Aracıların coğrafi konumu |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Yüklü OMS Ağ Geçidi Sayısı |

## <a name="next-steps"></a>Sonraki adımlar

* Log Analytics’ten uyarı oluşturma hakkında daha ayrıntılı bilgi edinmek için bkz. [Log Analytics’teki Uyarılar](../log-analytics/log-analytics-alerts.md) .

