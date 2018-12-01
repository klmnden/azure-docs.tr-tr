---
title: OMS portalında Azure'a taşıma | Microsoft Docs
description: OMS portalında sunsetted Azure portalına taşınıyor tüm işlevlere sahip oluyor. Bu makalede, bu geçiş hakkında ayrıntılar sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 5010426db97a9cd404d265d1ea9b319877eda1de
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52723967"
---
# <a name="oms-portal-moving-to-azure"></a>OMS portalında Azure'a taşıma

> [!NOTE]
> Bu makale Azure genel bulutunda ve tersi belirtilmedikçe dışında kamu bulutu için geçerlidir.

Azure portalı, tüm Azure Hizmetleri için hub'ı ve panolar için kaynaklar, akıllı arama bulma kaynakların ve kaynak yönetimi için etiketleme sabitleme gibi özellikler sayesinde zengin yönetim deneyimi sunar. İzleme ve yönetim iş akışını kolaylaştırın ve birleştirmek için Azure portalında OMS portalı yetenekleri ekleme başlatıldı. OMS Portalı'nın özelliklerin tümü, artık Azure portalında bir parçasıdır. Aslında, bazı trafik analizi gibi yeni özellikleri yalnızca Azure portalında kullanılabilir. Her şeyi ve daha fazlasını Azure portalı ile OMS portalında yaptığınız işe gerçekleştirmek mümkün olacaktır. Zaten yapmadıysanız, Azure Portalı'nı hemen kullanmaya başlayın!

**OMS portalında resmi olarak 15 Ocak 2019 üzerinde kullanımdan kaldırılacaktır.** Azure portalına taşıma ve bir kolayca geçiş beklediğiniz heyecan duyuyoruz. Ancak anlıyoruz değişiklikleri zordur ve karışıklığa neden olabilir. Sorularınız, geri bildirim veya endişeleriniz için gönderme **LAUpgradeFeedback@microsoft.com**. Bu makalenin geri kalanında, senaryoları ve bu geçiş için yol haritası üzerinden gider.

## <a name="what-is-changing"></a>Değişen nedir? 
Aşağıdaki değişiklikler OMS portalına kullanımdan kaldırma ile bildirilir. Her biri bu değişiklikler aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

- Yeni Oluştur [çalışma alanları yalnızca](#new-workspaces) Azure portalında.
- Yeni uyarı yönetim deneyimi [uyarı yönetimi çözümü değiştirir](#changes-to-alerts).
- [Kullanıcı erişim yönetimi](#user-access-and-role-migration) artık Azure portalında Azure rol tabanlı erişim denetimi kullanarak gerçekleştirilir.
- [Application Insights bağlayıcısı gereklidir artık](#application-insights-connector-and-solution) aynı işlevselliği geçici çalışma alanı sorguları etkin olduğundan.
- [OMS mobil uygulamasını](#oms-mobile-app) kullanımdan kaldırılıyor. 
- [NSG çözüm değiştirilir](#azure-network-security-group-analytics) Gelişmiş işlevsellikle trafik analizi çözümü kullanılabilir.
- Log Analytics için System Center Operations Manager'dan yeni bağlantı iste [yönetim paketlerini güncelleştirme](#system-center-operations-manager).
- Bkz: [, OMS güncelleştirme dağıtımlarını Azure'a geçirmek](../automation/migrate-oms-update-deployments.md) değişiklikler hakkında ayrıntılı bilgi için [güncelleştirme yönetimi](../automation/automation-update-management.md).


## <a name="what-should-i-do-now"></a>Ne şimdi yapmam?
Çoğu özelliği herhangi bir geçiş gerçekleştirmeye olmadan çalışmaya devam edecek, ancak aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Şunları yapmanız [kullanıcı izinlerinizi geçirme](#user-access-and-role-migration) Azure portalında.
- Bkz: [, OMS güncelleştirme dağıtımlarını Azure'a geçirmek](../automation/migrate-oms-update-deployments.md) güncelleştirme yönetimi çözümünü geçiş ile ilgili ayrıntılar.

Başvurmak [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](../log-analytics/log-analytics-oms-portal-faq.md) Azure portalına geçiş hakkında bilgi için. Geri bildirim, sorularınız veya endişeleriniz için gönderme **LAUpgradeFeedback@microsoft.com**.

## <a name="user-access-and-role-migration"></a>Kullanıcı erişimi ve rol geçişi
Azure portalında erişim yönetimi daha zengin ve OMS portalında erişim yönetimi daha güçlü. Bkz: [çalışma alanlarını yönetme](log-analytics-manage-access.md#manage-accounts-and-users) Log analytics'te erişim yönetimi hakkında ayrıntılar için.

> [!NOTE]
> Bu makalenin önceki sürümleri, izinler otomatik olarak OMS portalından Azure portalına dönüştürülür, belirtilen. Bu otomatik dönüştürme artık planlı ve kendiniz dönüştürme gerçekleştirmeniz gerekir.

Bu durumda herhangi bir değişiklik yapmanız gerekmez Azure Portalı'nda zaten uygun erişime sahip olabilir. Bu durumda, yönetici izinleri atamanız gerekir birkaç burada uygun erişimi olmayabilir örneklerinin vardır.

- Azure portalında herhangi bir izin vermeden OMS portalında salt okunur kullanıcı izinleri var. 
- OMS portalında katkıda bulunan izinleri ancak Azure portalında yalnızca okuyucu erişimi var.
 
Her iki durumda, yöneticinize el ile aşağıdaki tablodan uygun rol ataması gerekiyor. Kaynak grubu veya abonelik düzeyinde bu rolü atamanızı öneririz.  Kısa bir süre sonra bu iki durumda için daha normatif bir Rehber sağlanır.

| OMS portalı izni | Azure rol |
|:---|:---|
| SaltOkunur | Log Analytics Okuyucusu |
| Katılımcı | Log Analytics Katkıda Bulunan |
| Yönetici | Sahip | 
 

## <a name="new-workspaces"></a>Yeni çalışma alanları
Olan artık yeni çalışma alanları OMS portalını kullanarak oluşturamazsınız. Sunulan yönergeleri [Azure portalında Log Analytics çalışma alanı oluşturma](log-analytics-quick-create-workspace.md) Azure portalında yeni bir çalışma alanı oluşturmak için.

## <a name="changes-to-alerts"></a>Uyarılar için değişiklikler

### <a name="alert-extension"></a>Uyarı uzantısı  

> [!NOTE]
> Uyarıları artık tam olarak Azure portalında genel bulut için genişletilmiştir. Mevcut uyarı kuralları OMS portalında görüntülenebilir, ancak Azure portalında yalnızca yönetilebilir. Azure portalında uyarıları uzantısını Azure kamu bulutunda Ekim, 2018'de başlayacaktır.

Uyarılar olmuştur [Azure portalında Genişletilmiş](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Bu işlem tamamlandıktan sonra Yönetim eylemleri uyarıları yalnızca Azure portalında kullanılabilir olacaktır. Var olan uyarılar OMS portalında listelenmeye devam eder. Uyarılar programlama yoluyla Log Analytics uyarı REST API veya Log Analytics uyarı kaynak şablonu kullanarak erişirseniz, API çağrıları, Azure Resource Manager şablonları ve PowerShell komutlarında Eylemler yerine eylem gruplarını kullanmanız gerekir.

### <a name="alert-management-solution"></a>Uyarı yönetimi çözümü
Yerine [uyarı yönetimi çözümü](../azure-monitor/platform/alert-management-solution.md), kullanabileceğiniz [Azure İzleyici'nın birleşik uyarı arabirimi](../monitoring-and-diagnostics/monitoring-overview-alerts.md) görselleştirip uyarılarınızı yönetme. Bu yeni deneyim, Log Analytics de dahil olmak üzere Azure günlük uyarıları içindeki farklı kaynaklardan uyarılarını toplar. Uyarılarınızı dağıtımlarını görebilir, akıllı grupları ilgili uyarıları otomatik gruplandırması avantajlarından yararlanın ve zengin bir filtre uygulanırken, birden fazla aboneliği analiz uyarıları görüntüleyin. Tüm bu özellikler, 4 Haziran 2018 tarihinden itibaren önizlemede kullanılabilir. Uyarı yönetimi çözümü, Azure portalında kullanılamaz. 

Uyarı yönetimi çözümü (uyarı türünü kayıtlarla) tarafından toplanan veriler, Log Analytics'e çözüm çalışma alanı için yüklü olduğu sürece olmaya devam eder. Ağustos 2018 itibarıyla, birleştirilmiş çalışma alanları halinde uyarı gelen uyarılar, akış, bu özellik değiştirerek etkinleştirilecektir. Bazı şema değişiklikleri beklenir ve daha sonraki bir tarihte duyurulacaktır.

## <a name="oms-mobile-app"></a>OMS mobil uygulamasını
OMS mobil uygulaması ile birlikte OMS portalı sunsetted olacaktır. OMS mobil uygulamasını yerine, BT altyapısı, panolar ve kaydedilmiş sorgular hakkındaki bilgilere erişmek için Azure portalında doğrudan tarayıcınızdan mobil cihazınıza erişebilirsiniz. Uyarıları almak için yapılandırmanız [Azure Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) SMS veya sesli çağrı biçiminde bildirimleri almak için

## <a name="application-insights-connector-and-solution"></a>Application Insights Bağlayıcısı ve çözümü
[Application Insights Bağlayıcısı](../azure-monitor/platform/app-insights-connector.md) Log Analytics çalışma alanınıza Application Insights verileri getirmek için bir yol sağlar. Bu veri çoğaltma, altyapı ve uygulama veriler üzerinde görünürlük etkinleştirmek için gerekli.

Desteğiyle [kaynaklar arası sorgular](log-analytics-cross-workspace-search.md), artık veri çoğaltmak için bu gereksinimi yoktur. Bu nedenle, varolan bir Application Insights çözümü kullanımdan kaldırılacaktır. Ekim dan başlayarak, yeni bir Application Insights kaynaklarını Log Analytics çalışma alanına bağlamak mümkün olmayacaktır. Var olan bağlantıları ve panolar 15 Ocak 2019 kadar çalışmaya devam eder.


## <a name="azure-network-security-group-analytics"></a>Azure Ağ Güvenlik Grubu Analizi
[Azure ağ güvenlik grubu analizi çözümü](../azure-monitor/insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) değiştirilecek kısa süre önce kullanıma ile [trafik analizi](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) üzerinde bulut ağlarındaki kullanıcı ve uygulama etkinliğiniz görünürlük sağlar. Trafik analizi, kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri denetleme, iş yükü performansı iyileştirmek ve uyumluluğu sürdürün yardımcı olur. 

Bu çözüm, NSG akış günlüklerini analiz ederek ve aşağıdaki Öngörüler sağlar.

- Ağlarınız arasında Azure ve Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasındaki trafik akış.
- Uygulamalar ve algılayıcılar ve adanmış bir akış koleksiyonu gereçler gerek kalmadan, ağınızdaki protokoller.
- Popüler konuşmacılar, sık iletişim kuran uygulamaları, VM konuşmaları bulutta, trafik etkin noktaları.
- Kaynaklar ve hedefler trafik ağlarda, iş açısından kritik hizmetler ve uygulamalar arasında arası ilişki.
- Kötü amaçlı trafik, Internet, uygulamaları veya İnternet'e erişmeye çalışan VM'ler için açık bağlantı noktaları da dahil olmak üzere güvenlik.
- Yardımcı olan kapasite kullanımı sağlama veya Tıkanıklığı üzerinden sorunlarını ortadan kaldırın.

NSG günlüklerini mevcut kayıtlı aramalar, uyarılar, panolar için Log Analytics için çalışmaya devam edecek göndermek için tanılama ayarları kullanan devam edebilirsiniz. Çözüm'i zaten yüklemiş olan müşteriler, bildirime kadar kullanmaya devam edebilirsiniz. 5 Eylül başlayarak, ağ güvenlik grubu analizi Çözümü marketten kaldırılacak ve topluluk aracılığıyla kullanıma sunulan bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Belirttiyseniz [Operations Manager yönetim grubunuzu Log Analytics'e bağlı](log-analytics-om-agents.md), ardından bu değişikliği olmadan çalışmaya devam edecektir. Yeni bağlantılar için yine de,'deki yönergeleri izlemeniz gereken [Microsoft System Center Operations Manager yönetim Operations Management Suite yapılandırma paketi](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](log-analytics-oms-portal-faq.md) OMS portalından Azure portalına taşıma konusunda yönergeler için.
