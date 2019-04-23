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
ms.date: 01/08/2019
ms.author: bwren
ms.openlocfilehash: c4950d03449f2b293a87ab88f1ea3f49eee29557
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790103"
---
# <a name="oms-portal-moving-to-azure"></a>OMS portalında Azure'a taşıma

> [!NOTE]
> Bu makale Azure genel bulutunda ve tersi belirtilmedikçe dışında kamu bulutu için geçerlidir.

Azure portalı, tüm Azure Hizmetleri için hub'ı ve panolar için kaynaklar, akıllı arama bulma kaynakların ve kaynak yönetimi için etiketleme sabitleme gibi özellikler sayesinde zengin yönetim deneyimi sunar. İzleme ve yönetim iş akışını kolaylaştırın ve birleştirmek için Azure portalında OMS portalı yetenekleri ekleme başlatıldı. OMS Portalı'nın özelliklerin tümü, artık Azure portalında bir parçasıdır. Aslında, bazı trafik analizi gibi yeni özellikleri yalnızca Azure portalında kullanılabilir. Her şeyi ve daha fazlasını Azure portalı ile OMS portalında yaptığınız işe gerçekleştirmek mümkün olacaktır. Zaten yapmadıysanız, Azure Portalı'nı hemen kullanmaya başlayın!

**OMS portalında resmi olarak 15 Ocak 2019 üzerinde kullanımdan kaldırılacak** Azure ABD kamu Bulutu, OMS portalı ve Azure ticari bulutundaki için **resmi olarak 30 Mart 2019 üzerinde kullanımdan kaldırılacaktır.** Azure portalına taşıma ve bir kolayca geçiş beklediğiniz heyecan duyuyoruz. Ancak anlıyoruz değişiklikleri zordur ve karışıklığa neden olabilir. Sorularınız, geri bildirim veya endişeleriniz için gönderme **LAUpgradeFeedback\@microsoft.com**. Bu makalenin geri kalanında, senaryoları ve bu geçiş için yol haritası üzerinden gider.

## <a name="what-is-changing"></a>Değişen nedir? 
Aşağıdaki değişiklikler OMS portalına kullanımdan kaldırma ile bildirilir. Her biri bu değişiklikler aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

- Yeni Oluştur [çalışma alanları yalnızca](#new-workspaces) Azure portalında.
- Yeni uyarı yönetim deneyimi [uyarı yönetimi çözümü değiştirir](#changes-to-alerts).
- [Kullanıcı erişim yönetimi](#user-access-and-role-migration) artık Azure portalında Azure rol tabanlı erişim denetimi kullanarak gerçekleştirilir.
- [Application Insights bağlayıcısı gereklidir artık](#application-insights-connector-and-solution) aynı işlevselliği geçici çalışma alanı sorguları etkin olduğundan.
- [OMS mobil uygulamasını](#oms-mobile-app) kullanımdan kaldırılıyor. 
- [NSG çözüm değiştirilir](#azure-network-security-group-analytics) Gelişmiş işlevsellikle trafik analizi çözümü kullanılabilir.
- Log Analytics için System Center Operations Manager'dan yeni bağlantı iste [yönetim paketlerini güncelleştirme](#system-center-operations-manager).
- Bkz: [, OMS güncelleştirme dağıtımlarını Azure'a geçirmek](../../automation/migrate-oms-update-deployments.md) değişiklikler hakkında ayrıntılı bilgi için [güncelleştirme yönetimi](../../automation/automation-update-management.md).


## <a name="what-should-i-do-now"></a>Ne şimdi yapmam?
Çoğu özelliği herhangi bir geçiş gerçekleştirmeye olmadan çalışmaya devam edecek, ancak aşağıdaki görevleri gerçekleştirmeniz gerekir:

- Şunları yapmanız [kullanıcı izinlerinizi geçirme](#user-access-and-role-migration) Azure portalında.
- Bkz: [, OMS güncelleştirme dağıtımlarını Azure'a geçirmek](../../automation/migrate-oms-update-deployments.md) güncelleştirme yönetimi çözümünü geçiş ile ilgili ayrıntılar.

Başvurmak [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](oms-portal-faq.md) Azure portalına geçiş hakkında bilgi için. Geri bildirim, sorularınız veya endişeleriniz için gönderme **LAUpgradeFeedback\@microsoft.com**.

## <a name="user-access-and-role-migration"></a>Kullanıcı erişimi ve rol geçişi
Azure portalında erişim yönetimi daha zengin ve OMS portalında erişim yönetimi daha güçlü. Bkz: [çalışma alanlarını yönetme](manage-access.md#manage-accounts-and-users) Log analytics'te erişim yönetimi hakkında ayrıntılar için.

> [!NOTE]
> Bu makalenin önceki sürümleri, izinler otomatik olarak OMS portalından Azure portalına dönüştürülür, belirtilen. Bu otomatik dönüştürme artık planlı ve kendiniz dönüştürme gerçekleştirmeniz gerekir.

Bu durumda herhangi bir değişiklik yapmanız gerekmez Azure Portalı'nda zaten uygun erişime sahip olabilir. Bu durumda, yönetici izinleri atamanız gerekir birkaç burada uygun erişimi olmayabilir örneklerinin vardır.

- Azure portalında herhangi bir izin vermeden OMS portalında salt okunur kullanıcı izinleri var. 
- OMS portalında katkıda bulunan izinleri ancak Azure portalında yalnızca okuyucu erişimi var.
 
Her iki durumda, yöneticinize el ile aşağıdaki tablodan uygun rol ataması gerekiyor. Kaynak grubu veya abonelik düzeyinde bu rolü atamanızı öneririz.  Kısa bir süre sonra bu iki durumda için daha normatif bir Rehber sağlanır.

| OMS portalı izni | Azure rol |
|:---|:---|
| ReadOnly | Log Analytics Okuyucusu |
| Katılımcı | Log Analytics Katkıda Bulunan |
| Yönetici | Sahip | 
 

## <a name="new-workspaces"></a>Yeni çalışma alanları
Olan artık yeni çalışma alanları OMS portalını kullanarak oluşturamazsınız. Sunulan yönergeleri [Azure portalında Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md) Azure portalında yeni bir çalışma alanı oluşturmak için.

## <a name="changes-to-alerts"></a>Uyarılar için değişiklikler

### <a name="alert-extension"></a>Uyarı uzantısı  

> [!NOTE]
> Uyarıları artık tam olarak Azure portalında genel bulut için genişletilmiştir. Mevcut uyarı kuralları OMS portalında görüntülenebilir, ancak Azure portalında yalnızca yönetilebilir. Azure portalında uyarıları uzantısını Azure kamu bulutunda Şubat 2019 üzerinde başlayacaktır.

Uyarılar olmuştur [Azure portalında Genişletilmiş](alerts-extend.md). Bu işlem tamamlandıktan sonra Yönetim eylemleri uyarıları yalnızca Azure portalında kullanılabilir olacaktır. Var olan uyarılar OMS portalında listelenmeye devam eder. Uyarılar programlama yoluyla Log Analytics uyarı REST API veya Log Analytics uyarı kaynak şablonu kullanarak erişirseniz, API çağrıları, Azure Resource Manager şablonları ve PowerShell komutlarında Eylemler yerine eylem gruplarını kullanmanız gerekir.

### <a name="alert-management-solution"></a>Uyarı yönetimi çözümü
Önceki bir duyuru değişiklik olarak [uyarı yönetimi çözümü](alert-management-solution.md) Azure portalında tam olarak desteklenir ve kullanılabilir olmaya devam edecektir. Azure Market'te çözüm yüklemeye devam edebilirsiniz.

Uyarı yönetimi çözümü kullanılabilmesi amacıyla bu devam ederken kullanmanızı öneriyoruz [Azure İzleyici'nın birleşik uyarı arabirimi](alerts-overview.md) görselleştirip azure'a tüm uyarıları yönetme. Bu yeni deneyim, uyarılar yerel olarak Azure Log Analytics de dahil olmak üzere günlük uyarıları içindeki farklı kaynaklardan toplar. Azure İzleyici'nın birleşik uyarı arabirimi kullanıyorsanız, sonra uyarı yönetimi çözümü yalnızca azure'a gelen System Center Operation Manager uyarıları tümleştirmeyi etkinleştirmek için gereklidir. Azure İzleyici'nın birleşik uyarı arabiriminde, dağıtımları uyarılarınızı görmek, akıllı grupları ilgili uyarıları otomatik gruplandırması yararlanın ve zengin bir filtre uygulanırken, birden fazla aboneliği analiz uyarıları görüntüleyin. Uyarı Yönetimi gelecekteki geliştirmeleri, öncelikle bu yeni deneyiminden kullanıma sunulacaktır. 

Uyarı yönetimi çözümü (uyarı türünü kayıtlarla) tarafından toplanan veriler, Log Analytics'e çözüm çalışma alanı için yüklü olduğu sürece olmaya devam eder. 

## <a name="oms-mobile-app"></a>OMS mobil uygulamasını
OMS mobil uygulaması ile birlikte OMS portalı sunsetted olacaktır. OMS mobil uygulamasını yerine, BT altyapısı, panolar ve kaydedilmiş sorgular hakkındaki bilgilere erişmek için Azure portalında doğrudan tarayıcınızdan mobil cihazınıza erişebilirsiniz. Uyarıları almak için yapılandırmanız [Azure Eylem grupları](action-groups.md) SMS veya sesli çağrı biçiminde bildirimleri almak için

## <a name="application-insights-connector-and-solution"></a>Application Insights Bağlayıcısı ve çözümü
[Application Insights Bağlayıcısı](app-insights-connector.md) Application Insights verilerini Log Analytics çalışma alanınıza eklemek için bir yol sağlar. Bu veri çoğaltma, altyapı ve uygulama veriler üzerinde görünürlük etkinleştirmek için gerekli. Application Insights'ın Mart 2019 veri saklama desteği genişletilmiş ve gerçekleştirme becerisi [kaynaklar arası sorgular](../log-query/cross-workspace-query.md) imkanına yanı sıra [birden çok Azure İzleyici Application Insights kaynaklarını görüntüleme ](../log-query/unify-app-resource-data.md), yinelenen verileri, Application Insights kaynaklarını ve Log Analytics'e göndermek için gerek yoktur. Ayrıca, bağlayıcı uygulama özelliklerinin bir alt Log Analytics'e gönderir, kaynaklar arası sorgular getirirken esneklik Gelişmiş.  

Bu nedenle, Application Insights Bağlayıcısı kullanım dışı ve varolan bağlantılar 30 Haziran 2019 kadar çalışmaya devam edecek, ancak 30 Mart 2019 tarihinde OMS portalı kullanımdan kaldırma ile birlikte Azure Marketi'nden kaldırıldı. OMS portalı kullanımdan kaldırma ile yapılandırmak ve mevcut bağlantıları Portalı'ndan kaldırmak için hiçbir yolu yoktur. Bu Ocak 2019 içinde kullanılabilir hale getirilir REST API'sini kullanarak desteklenmeyecek ve bildirim tarihinde gönderildi [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/). 

## <a name="azure-network-security-group-analytics"></a>Azure Ağ Güvenlik Grubu Analizi
[Azure ağ güvenlik grubu analizi çözümü](../insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-azure-monitor) değiştirilecek kısa süre önce kullanıma ile [trafik analizi](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) üzerinde bulut ağlarındaki kullanıcı ve uygulama etkinliğiniz görünürlük sağlar. Trafik analizi, kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri denetleme, iş yükü performansı iyileştirmek ve uyumluluğu sürdürün yardımcı olur. 

Bu çözüm, NSG akış günlüklerini analiz ederek ve aşağıdaki Öngörüler sağlar.

- Ağlarınız arasında Azure ve Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasındaki trafik akış.
- Uygulamalar ve algılayıcılar ve adanmış bir akış koleksiyonu gereçler gerek kalmadan, ağınızdaki protokoller.
- Popüler konuşmacılar, sık iletişim kuran uygulamaları, VM konuşmaları bulutta, trafik etkin noktaları.
- Kaynaklar ve hedefler trafik ağlarda, iş açısından kritik hizmetler ve uygulamalar arasında arası ilişki.
- Kötü amaçlı trafik, Internet, uygulamaları veya İnternet'e erişmeye çalışan VM'ler için açık bağlantı noktaları da dahil olmak üzere güvenlik.
- Yardımcı olan kapasite kullanımı sağlama veya Tıkanıklığı üzerinden sorunlarını ortadan kaldırın.

NSG günlüklerini mevcut kayıtlı aramalar, uyarılar, panolar için Log Analytics için çalışmaya devam edecek göndermek için tanılama ayarları kullanan devam edebilirsiniz. Çözüm'i zaten yüklemiş olan müşteriler, bildirime kadar kullanmaya devam edebilirsiniz. 5 Eylül başlayarak, ağ güvenlik grubu analizi Çözümü marketten kaldırılacak ve topluluk aracılığıyla kullanıma sunulan bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Belirttiyseniz [Operations Manager yönetim grubunuzu Log Analytics'e bağlı](om-agents.md), ardından bu değişikliği olmadan çalışmaya devam edecektir. Yeni bağlantılar için yine de,'deki yönergeleri izlemeniz gereken [Microsoft System Center Operations Manager yönetim Operations Management Suite yapılandırma paketi](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](oms-portal-faq.md) OMS portalından Azure portalına taşıma konusunda yönergeler için.
