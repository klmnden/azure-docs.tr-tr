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
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 1a8ccc818cafac4867cb533c83f297af61a21836
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309111"
---
# <a name="oms-portal-moving-to-azure"></a>OMS portalında Azure'a taşıma
Bir parça art arda Log Analytics müşterilerden heard geri bildirim sağlayarak hem şirket içi hem de Azure iş yüklerini yönetmek ve izlemek tek kullanıcı deneyimini gereksinimidir. Azure portalı, tüm Azure Hizmetleri için hub'ı ve panolar için kaynaklar, akıllı arama bulma kaynakların ve kaynak yönetimi için etiketleme sabitleme gibi özellikler sayesinde zengin yönetim deneyimi sunar bildiğiniz. İzleme ve yönetim iş akışını kolaylaştırın ve birleştirmek için Azure portalında OMS portalı yetenekleri ekleme başlatıldı. OMS Portalı'nın özelliklerinin birçoğunu şimdi Azure portal'ın bir parçasıdır duyurmaktan Mutluluk duyuyoruz. Aslında, bazı trafik analizi gibi yeni özellikleri yalnızca Azure portalında kullanılabilir. Yalnızca Azure portalına taşınacağını işleminde hala bazı çözümler dahil olmak üzere kalan birkaç boşlukları vardır. Bu özellikleri kullanmıyorsanız her şeyi ve daha fazlasını Azure portalı ile OMS portalında yaptığınız işe gerçekleştirmenin mümkün olacaktır. Zaten yapmadıysanız, Azure Portalı'nı hemen kullanmaya başlayın önerilir! 

Aşağı Ağustos 2018 tarihine göre iki Portal arasında kalan boşlukları kapatmak bekliyoruz. Müşteri görüşlerine dayalı olarak, OMS portalında sunsetting için zaman çizelgesi iletir. Azure portalına taşıma ve bir kolayca geçiş beklediğiniz heyecan duyuyoruz. Ancak anlıyoruz değişiklikleri zordur ve karışıklığa neden olabilir. Sorularınız, geri bildirim veya endişeleriniz için gönderme **LAUpgradeFeedback@microsoft.com**. Bu makalenin geri kalanında, anahtar senaryolar, geçerli aralıklar ve bu geçiş için yol haritası üzerinden gider. 

## <a name="progress"></a>İlerleme Durumu
Bu makalenin önceki sürümleri bu yana tamamlanmış güncelleştirmeleri aşağıda verilmiştir.

### <a name="july-27"></a>27 Temmuz

- [DNS analizi](log-analytics-dns.md) artık Azure portalında tamamen çalışır durumdadır.
- [Güncelleştirme yönetimi](../automation/automation-update-management.md) Azure portalında tam işlevselliği için güncelleştirilmiştir. Bkz: [, OMS güncelleştirme dağıtımlarını Azure'a geçirmek](../automation/migrate-oms-update-deployments.md) Ayrıntılar için.
- [Uyarılar](#changes-to-alerts) artık tam olarak Azure portalında genişletilmiştir.
- [Özel günlükler önizleme özelliği](log-analytics-data-sources-custom-logs.md) artık tüm çalışma alanları için otomatik olarak etkinleştirilir.

## <a name="what-will-change"></a>Değişecek mi? 
Aşağıdaki değişiklikler OMS portalına kullanımdan kaldırma ile bildirilir. Her biri bu değişiklikler aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

- Yalnızca Azure portalında yeni çalışma alanları oluşturmak mümkün olacaktır.
- Yeni uyarı yönetim deneyimi uyarı yönetimi çözümü yerini alır.
- Kullanıcı erişim yönetimi, Azure portalında Azure rol tabanlı erişim denetimi kullanarak gerçekleştirilir.
- Aynı işlevselliği çalışma arası sorgular etkinleştirilebilir beri Application Insights Bağlayıcısı artık gerekli değildir.
- OMS mobil uygulaması kullanım dışı bırakılacaktır. 
- NSG çözüm trafik analizi çözümü sunulan Gelişmiş özelliklerden ile değiştirilmektedir.
- System Center Operations Manager'dan yeni bağlantıları Log analytics'e, güncelleştirilmiş yönetim paketlerini gerektirir.


## <a name="current-known-gaps"></a>Geçerli bilinen boşluklar 
Şu anda OMS portalını kullanmaya devam etmenizi gerektiren bazı işlev eksiklikleri vardır. Bu belge uygun şekilde güncelleştirilir ve bu boşlukları kapalı. Ayrıca başvurmanız gerekir [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=log-analytics) uzantıları ve değişiklikleri hakkında devam eden duyuruları.

- Aşağıdaki çözümlerden henüz Azure portalında tam işlevsel değildir. Klasik portalda bu çözümleri güncelleştirilir kadar kullanmaya devam etmelidir.

    - Windows Analytics çözümleri ([yükseltme hazırlığı](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [cihaz sistem durumu](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), ve [güncelleştirme Uyumluluk](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Azure log Analytics kaynağa erişmek için kullanıcı erişim aracılığıyla verilmelidir [Azure rol tabanlı erişim](#user-access-and-role-migration).


## <a name="what-should-i-do-now"></a>Ne şimdi yapmam?  
Başvurmanız gerekir [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](../log-analytics/log-analytics-oms-portal-faq.md) Azure portalına geçiş hakkında bilgi için. Varsa [boşlukları yukarıda açıklanan](#current-known-gaps) deneyiminizi Birincil Azure portalını kullanarak başlangıç dikkate almanız gereken sonra ortamınız için geçerli değildir. Geri bildirim, sorularınız veya endişeleriniz için gönderme LAUpgradeFeedback@microsoft.com.

## <a name="new-workspaces"></a>Yeni çalışma alanları
29 Temmuz'a itibaren artık yeni çalışma alanları OMS portalını kullanarak oluşturmak mümkün olmayacak. Sunulan yönergeleri [Azure portalında Log Analytics çalışma alanı oluşturma](log-analytics-quick-create-workspace.md) Azure portalında yeni bir çalışma alanı oluşturmak için.

## <a name="changes-to-alerts"></a>Uyarılar için değişiklikler 

### <a name="alert-extension"></a>Uyarı uzantısı  

> [!NOTE]
> Azure portalında uyarıları artık bir tam olarak genişletilmiştir. Mevcut uyarı kuralları OMS portalında görüntülenebilir, ancak Azure portalında yalnızca yönetilebilir.

Uyarılar sürecinde olan [Azure portalında Genişletilmiş](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Bu işlem tamamlandıktan sonra Yönetim eylemleri uyarıları yalnızca Azure portalında kullanılabilir olacaktır. Var olan uyarılar OMS portalında listelenmeye devam eder. Uyarılar programlama yoluyla Log Analytics uyarı REST API veya Log Analytics uyarı kaynak şablonu kullanarak erişirseniz, API çağrıları, Azure Resource Manager şablonları ve PowerShell komutlarında Eylemler yerine eylem gruplarını kullanmanız gerekir.

### <a name="alert-management-solution"></a>Uyarı yönetimi çözümü
Yerine [uyarı yönetimi çözümü](log-analytics-solution-alert-management.md), kullanabileceğiniz [Azure İzleyici'nın birleşik uyarı arabirimi](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) görselleştirip uyarılarınızı yönetme. Bu yeni deneyim, Log Analytics de dahil olmak üzere Azure günlük uyarıları içindeki farklı kaynaklardan uyarılarını toplar. Uyarılarınızı dağıtımlarını görebilir, akıllı grupları ilgili uyarıları otomatik gruplandırması avantajlarından yararlanın ve zengin bir filtre uygulanırken, birden fazla aboneliği analiz uyarıları görüntüleyin. Tüm bu özellikler, 4 Haziran 2018 tarihinden itibaren önizlemede kullanılabilir. Uyarı yönetimi çözümü, Azure portalında kullanılamaz. 

Uyarı yönetimi çözümü (uyarı türünü kayıtlarla) tarafından toplanan veriler, Log Analytics'e çözüm çalışma alanı için yüklü olduğu sürece olmaya devam eder. Ağustos 2018 itibarıyla, birleştirilmiş çalışma alanları halinde uyarı gelen uyarılar, akış, bu özellik değiştirerek etkinleştirilecektir. Bazı şema değişiklikleri beklenir ve daha sonraki bir tarihte duyurulacaktır.

## <a name="user-access-and-role-migration"></a>Kullanıcı erişimi ve rol geçişi
Azure portalında erişim yönetimi daha zengin ve OMS portalında erişim yönetimi daha güçlü, ancak bazı dönüştürmeler gerektirir. Bkz: [çalışma alanlarını yönetme](log-analytics-manage-access.md#manage-accounts-and-users) Log analytics'te erişim yönetimi hakkında ayrıntılar için.

30 Temmuz OMS erişim denetimi izinleri otomatik dönüştürme başlangıç portalı Azure portal izinlere başlar. Dönüştürme tamamlandıktan sonra OMS portalındaki kullanıcı yönetimi bölümünde, azure'da erişim denetimi (IAM) kullanıcılar rota. 

Dönüştürme sırasında sistem her bir kullanıcı veya OMS portalında izinleri olan bir güvenlik grubu denetleyin ve aynı düzeyde veya izinleri Azure'da olup olmadığını belirler. İzinleri yoksa, ilgili çalışma alanları ve çözümler için aşağıdaki rollerin atar.

| OMS portalı izni | Azure rol |
|:---|:---|
| SaltOkunur | Log Analytics Okuyucusu |
| Katılımcı | Log Analytics Katkıda Bulunan |
| Yönetici | Sahip |

Hiçbir aşırı izinleri kullanıcıya atanan emin olmak için sistem otomatik olarak bu kaynak grubu düzeyinde izinleri atar değil. Sonuç olarak, çalışma alanı yöneticileri el ile kendilerini atamanız gerekir _sahibi_ veya _katkıda bulunan_ aşağıdaki eylemleri gerçekleştirmek için kaynak grubu veya abonelik düzeyinde roller.

- Çözümler ekleyin veya kaldırın
- Yeni özel görünümler tanımlama
- Uyarıları yönetme 

Bazı durumlarda, otomatik dönüştürme izin uygulanamıyor ve el ile izinleri atamak için yöneticiyle ister.

## <a name="oms-mobile-app"></a>OMS mobil uygulamasını
OMS mobil uygulaması ile birlikte OMS portalı sunsetted olacaktır. OMS mobil uygulamasını yerine, BT altyapısı, panolar ve kaydedilmiş sorgular hakkındaki bilgilere erişmek için Azure portalında doğrudan tarayıcınızdan mobil cihazınıza erişebilirsiniz. Uyarıları almak için yapılandırmanız [Azure Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) SMS veya sesli çağrı biçiminde bildirimleri almak için

## <a name="application-insights-connector-and-solution"></a>Application Insights Bağlayıcısı ve çözümü
[Application Insights Bağlayıcısı](log-analytics-app-insights-connector.md) Log Analytics çalışma alanınıza Application Insights verileri getirmek için bir yol sağlar. Bu veri çoğaltma, altyapı ve uygulama veriler üzerinde görünürlük etkinleştirmek için gerekli.

Desteğiyle [kaynaklar arası sorgular](log-analytics-cross-workspace-search.md), artık veri çoğaltmak için bu gereksinimi yoktur. Bu nedenle, varolan bir Application Insights çözümü kullanımdan kaldırılacaktır. Temmuz itibaren yeni bir Application Insights kaynaklarını Log Analytics çalışma alanına bağlamak mümkün olmayacaktır. Var olan bağlantıları ve panolar Kasım 2018'e kadar çalışmaya devam eder.


## <a name="azure-network-security-group-analytics"></a>Azure Ağ Güvenlik Grubu Analizi
[Azure ağ güvenlik grubu analizi çözümü](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) değiştirilecek kısa süre önce kullanıma ile [trafik analizi](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) üzerinde bulut ağlarındaki kullanıcı ve uygulama etkinliğiniz görünürlük sağlar. Trafik analizi, kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri denetleme, iş yükü performansı iyileştirmek ve uyumluluğu sürdürün yardımcı olur. 

Bu çözüm, NSG akış günlüklerini analiz ederek ve aşağıdaki Öngörüler sağlar.

- Ağlarınız arasında Azure ve Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasındaki trafik akış.
- Uygulamalar ve algılayıcılar ve adanmış bir akış koleksiyonu gereçler gerek kalmadan, ağınızdaki protokoller.
- Popüler konuşmacılar, sık iletişim kuran uygulamaları, VM konuşmaları bulutta, trafik etkin noktaları.
- Kaynaklar ve hedefler trafik ağlarda, iş açısından kritik hizmetler ve uygulamalar arasında arası ilişki.
- Kötü amaçlı trafik, Internet, uygulamaları veya İnternet'e erişmeye çalışan VM'ler için açık bağlantı noktaları da dahil olmak üzere güvenlik.
- Yardımcı olan kapasite kullanımı sağlama veya Tıkanıklığı üzerinden sorunlarını ortadan kaldırın.

NSG günlüklerini mevcut kayıtlı aramalar, uyarılar, panolar için Log Analytics için çalışmaya devam edecek göndermek için tanılama ayarları kullanan devam edebilirsiniz. Çözüm'i zaten yüklemiş olan müşteriler, bildirime kadar kullanmaya devam edebilirsiniz. Ağustos 15 başlayarak, ağ güvenlik grubu analizi Çözümü marketten kaldırılacak ve topluluk aracılığıyla kullanıma sunulan bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Belirttiyseniz [Operations Manager yönetim grubunuzu Log Analytics'e bağlı](log-analytics-om-agents.md), ardından bu değişikliği olmadan çalışmaya devam edecektir. Yeni bağlantılar için yine de,'deki yönergeleri izlemeniz gereken [Microsoft System Center Operations Manager yönetim Operations Management Suite yapılandırma paketi](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş hakkında sık sorulan](log-analytics-oms-portal-faq.md) OMS portalından Azure portalına taşıma konusunda yönergeler için.