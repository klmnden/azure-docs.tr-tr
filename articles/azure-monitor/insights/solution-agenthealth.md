---
title: Azure İzleyici'de aracı durumu çözümü | Microsoft Docs
description: Bu makale, bu çözüm doğrudan Log Analytics veya System Center Operations Manager'a bildirimde bulunan aracılarınızın durumunu izlemek için nasıl kullanılacağını anlamanıza yardımcı olmak için hazırlanmıştır.
services: operations-management-suite
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/19/2017
ms.author: magoedte
ms.openlocfilehash: 568ff99364923c43b7199bed1dacd32cf0d18ab9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64571161"
---
#  <a name="agent-health-solution-in-azure-monitor"></a>Azure İzleyici'de aracı durumu çözümü
Aracı durumu çözümü, Azure, tüm aracıların doğrudan Log Analytics çalışma alanını Azure İzleyici'de raporlama anlamanıza yardımcı olur veya yanıt vermeyen olduğu için Azure İzleyici, bağlı bir System Center Operations Manager yönetim grubu ve işletimsel veri gönderiliyor.  Ayrıca, kaç aracının dağıtıldığını, bunların coğrafi olarak nerelere dağıtıldığını da izleyebilir ve Azure’da, diğer bulut ortamlarında ya da şirket içinde dağıtılmış aracıların dağılımından her zaman haberdar olmaya yönelik diğer sorguları gerçekleştirebilirsiniz.    

## <a name="prerequisites"></a>Önkoşullar
Bu çözümü dağıtmadan önce şu anda desteklenen onaylayın [Windows aracıları](../../log-analytics/log-analytics-windows-agent.md) Log Analytics çalışma alanına raporlama ya da raporlama bir [Operations Manager yönetim grubu](../../azure-monitor/platform/om-agents.md) ile tümleşik Çalışma alanınız.

## <a name="solution-components"></a>Çözüm bileşenleri
Bu çözüm, çalışma alanınıza eklenen aşağıdaki kaynaklardan ve doğrudan bağlanılan aracılardan veya Operations Manager bağlantılı yönetim grubundan oluşur.

### <a name="management-packs"></a>Yönetim paketleri
System Center Operations Manager yönetim grubunuzun bir Log Analytics çalışma alanınıza bağlıysa, aşağıdaki yönetim paketlerini Operations Manager'da yüklenir.  Bu çözüm eklendikten sonra bu yönetim paketleri doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerinde yapılandırılacak veya yönetilecek hiçbir şey yoktur.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="configuration"></a>Yapılandırma
Aracı durumu çözümü açıklanan işlemi kullanarak Log Analytics çalışma alanınıza eklemek [çözüm ekleme](solutions.md). Başka bir yapılandırma işlemi gerekmez.


## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları | Evet | Sinyal olayları doğrudan Windows aracılarından toplanır.|
| System Center Operations Manager yönetim grubu | Evet | Sinyal olayları 60 saniyede bir yönetim grubuna bildirimde bulunan aracılardan toplanır ve sonra Azure İzleyici iletilir. Azure İzleyici Operations Manager aracılarının doğrudan bir bağlantı gerekli değildir. Sinyal olay verileri yönetim grubundan Log Analytics çalışma alanına iletilir.|

## <a name="using-the-solution"></a>Çözümü kullanma
Çözüm, Log Analytics çalışma alanınıza eklediğinizde **aracı sistem durumu** kutucuk, panonuza eklenir. Bu kutucuk, son 24 saat içindeki toplam aracı sayısını ve yanıt vermeyen aracı sayısını gösterir.<br><br> ![Panodaki Aracı Durumu Çözüm kutucuğu](./media/solution-agenthealth/agenthealth-solution-tile-homepage.png)

**Aracı Durumu** kutucuğuna tıklayarak **Aracı Durumu** panosunu açın.  Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütunda, ilgili sütunun belirtilen zaman aralığına ilişkin ölçütlerle eşleşen ilk on olay sayılarına göre listelenir. Her sütunun sağ alt tarafındaki **Tümünü görüntüle**’yi seçerek ya da sütun başlığına tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|--------|-------------|
| Zaman içinde aracı sayısı | Hem Linux hem de Windows aracıları için yedi günlük bir dönem boyunca aracı sayınızın eğilimi.|
| Yanıt vermeyen aracı sayısı | Son 24 saat içinde bir sinyal göndermemiş tüm aracıların listesi.|
| İşletim Sistemi Türüne Göre Dağılım | Ortamınızda kaçar tane Windows ve Linux aracısı olduğuna ilişkin bir bölüm.|
| Aracı Sürümüne Göre Dağılım | Ortamınızda yüklü olan farklı aracı sürümlerine ve her birinin sayısına ilişkin bir bölüm.|
| Aracı Kategorisine Göre Dağılım | Sinyal olayları gönderen farklı kategorilerdeki araçlara ilişkin bir bölüm: Doğrudan aracılar, OpsMgr aracıları veya OpsMgr Management Server.|
| Yönetim Grubuna Göre Dağılım | Ortamınızdaki farklı Operations Manager yönetim gruplarına ilişkin bir bölüm.|
| Aracıların coğrafi konumu | Aracıları ve her ülkesinde/bölgesinde yüklenmiş aracıların toplam sayısı sahip olduğunuz farklı ülkelerde/bölgelerde ilişkin bir bölüm.|
| Yüklü Ağ Geçidi Sayısı | Log Analytics ağ geçidinin yüklü olduğu sunucular sayısı ve bu sunucuların listesi.|

![Aracı Durumu Çözüm panosu örneği](./media/solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="azure-monitor-log-records"></a>Azure İzleyici kayıtlarını günlüğe kaydet
Çözüm, Log Analytics çalışma alanında bir tür kayıt oluşturur.  

### <a name="heartbeat-records"></a>Sinyal kayıtları
**Sinyal** türünde bir kayıt oluşturulur.  Bu kayıtlar aşağıdaki tabloda gösterilen özelliklere sahiptir.  

| Özellik | Açıklama |
| --- | --- |
| `Type` | *Sinyal*|
| `Category` | Değer *Doğrudan Aracı*, *SCOM Aracısı* veya *SCOM Yönetim Sunucusu*’dur.|
| `Computer` | Bilgisayar adı.|
| `OSType` | Windows veya Linux işletim sistemi.|
| `OSMajorVersion` | İşletim sistemi ana sürümü.|
| `OSMinorVersion` | İşletim sistemi alt sürümü.|
| `Version` | Log Analytics aracısı veya Operations Manager Aracısı sürümü.|
| `SCAgentChannel` | Değer *Doğrudan* ve/veya *SCManagementServer*’dır.|
| `IsGatewayInstalled` | Log Analytics gateway yüklü değilse, değeri olan *true*, aksi takdirde *false*.|
| `ComputerIP` | Bilgisayarın IP adresi.|
| `RemoteIPCountry` | Bilgisayarın dağıtıldığı coğrafi konum.|
| `ManagementGroupName` | Operations Manager yönetim grubunun adı.|
| `SourceComputerId` | Bilgisayarın benzersiz kimliği.|
| `RemoteIPLongitude` | Bilgisayarın coğrafi konumunun boylamı.|
| `RemoteIPLatitude` | Bilgisayarın coğrafi konumunun enlemi.|

Bir Operations Manager yönetim sunucusuna rapor veren her bir aracı iki sinyal gönderir ve SCAgentChannel özelliğinin değeri hem de içerecektir **doğrudan** ve **SCManagementServer** ne bağlı olarak veri kaynakları ve izleme çözümleri, aboneliğinizde etkinleştirmiş olmanız gerekir. Hatırlayacağınız, veri çözümlerinden ya da gönderilir doğrudan bir Operations Manager yönetim sunucusundan Azure İzleyici ya da aracıda toplanan verilerin hacmi nedeniyle gönderilen doğrudan Aracıdan Azure İzleyici. **SCManagementServer** değerine sahip sinyal olayları için ComputerIP değeri, verileri aslında karşıya yükleyen yönetim sunucusunun IP adresidir.  SCAgentChannel’ın **Doğrudan** olarak ayarlandığı sinyaller için bu adres, aracının genel IP adresidir.  

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan kayıtlara ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
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
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Log Analytics yüklü ağ geçidi sayısı |




## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Azure İzleyici'de uyarılar](../platform/alerts-overview.md) uyarıları günlük sorguları oluşturma hakkında bilgi. 
