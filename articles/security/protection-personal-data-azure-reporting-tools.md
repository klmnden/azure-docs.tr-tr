---
title: "Belge koruması kişisel veri raporlama araçları Azure ile | Microsoft Docs"
description: "Azure Raporlama Hizmetleri ve teknolojileriyle kişisel verilerin gizliliği korumak için nasıl kullanılacağını."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 0ec9ceb63c3e1872e9815a7895b624276fc46123
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Kişisel veri raporlama araçları Azure ile belge koruması

Bu makalede Azure Raporlama Hizmetleri ve teknolojileriyle kişisel verilerin gizliliği korumak için nasıl kullanılacağını ele alınacaktır.

## <a name="scenario"></a>Senaryo

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, masraflarını Akdeniz, Adriatic ve Baltık seas yanı sıra İngiliz Adaları arasında içinde sunmaya işlemlerini genişletmektedir. Bu çabalarına yardımcı olmak için onu İtalya, Almanya, Danimarka ve İngiltere dayanarak birkaç küçük seyahat satırları aldı

Şirket, işleme ve şirket verilerinin depolanması için Microsoft Azure kullanır. Bu ad, adres, telefon numaraları gibi kişisel olarak tanımlanabilir bilgileri ve genel müşteri tabanı, kredi kartı bilgilerini içerir. Ayrıca tüm konumlarda adresleri, telefon numaralarını, vergi kimlik numaraları ve şirket çalışanlar hakkında diğer bilgiler gibi geleneksel İnsan Kaynakları bilgileri içerir. Seyahat satır de geçerli ve geçmiş müşterilerle ilişkilerini izlemek için kişisel bilgi içeren büyük bir veritabanı ödül ve bağlılık programı üyelerinin tutar.

Şirket çalışanları şirketin şubelere ağ erişimi ve seyahat tüm dünyada bulunan aracıları bazı şirket kaynaklarına erişimi vardır.

## <a name="problem-statement"></a>Sorun bildirimi

Şirket gizlilik müşterilerin çalışanların ve korumanız gerekir ve Azure yönetim ve güvenlik kullanan çok katmanlı güvenlik stratejisi ile kişisel veriler katı denetimlere erişimi ve kişisel veri işleme zorunlu tuttukları özelliklerini ve yapabilmesi gerekir koruyucu ölçüleri iç ve dış denetçiler göstermektedir.

## <a name="company-goal"></a>Şirket hedefi

Kendi savunma güvenlik stratejisinin bir parçası olarak, tüm erişim ve kişisel veriler işlenmesini izleyen ve kişisel veriler için yeterli gizlilik korumaları belgelerine yer ve çalışır durumda olduğundan emin olun bir şirket hedeftir.

## <a name="solutions"></a>Çözümler

Microsoft Azure izlemenize yardımcı olması için kapsamlı izleme, günlüğe kaydetme ve tanılama araçları sağlar ve kayıt etkinliklerini ve olayları erişme ve kişisel veriler, veri ve üçüncü taraf kişisel verilere erişimin coğrafi akış işleme ile ilişkili. Bulutta kişisel verilerin güvenliğini paylaşılan sorumluluk olduğundan, Microsoft müşterilerle da sağlar:

- Müşterilerin veri kendi işleme hakkında ayrıntılı bilgi

- Güvenlik önlemleri Microsoft tarafından yönetilen

- Nerede ve nasıl müşterilerin verileri gönderir.

- Microsoft'un kendi gizlilik incelemeler işleminin ayrıntıları

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un bulut tabanlı, çok kiracılı dizin ve kimlik yönetimi hizmetidir. Hizmetin oturum açma ve raporlama özellikleri denetim ayrıntılı oturum açma ve izlemek ve müşterilerin ve çalışanların kişisel veriler için uygun erişim sağlamanıza yardımcı olmak üzere uygulama kullanımı etkinliği bilgileri sağlar.

Etkinlik raporları iki tür vardır:

- [Denetim etkinlik raporları/günlükleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) sistem etkinlikler/görevleri ayrıntılı bir kayıt sağlayın

- [Gerçekleştirilen oturum açma etkinliği raporu/log](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) denetim raporda listelenen her etkinliği gerçekleştiren gösterir

İki birlikte kullanarak, her görevin gerçekleştirilmesi ve her gerçekleştiren geçmişini izleyebilirsiniz. Raporlar her iki tür özelleştirilebilir ve filtre uygulanabilir.

#### <a name="how-do-i-access-the-audit-and-security-logs"></a>Denetim ve güvenlik günlüklerini nasıl erişirim?

Denetim ve güvenlik günlüklerini üç farklı yolla Active Directory Portalı'ndan erişilebilen: aracılığıyla **etkinlik** bölümüne (ya da seçin **denetim günlüklerini** veya **oturum açma işlemleri**), veya **kullanıcılar ve gruplar** veya **kurumsal uygulamalar** altında **Yönet** Active Directory'de. Raporlar ayrıca Azure Active raporlama API'si dizin aracılığıyla erişilebilir. 

1. Azure portalında seçin **Azure Active Directory.**

2. İçinde **etkinlik** bölümünde, select **denetim günlükleri.**

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. Liste Görünümü tıklayarak özelleştirme **sütunları** araç.

4.  İlgili tüm kullanılabilir ayrıntıları görmek için liste görünümünde bir öğe seçin.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Azure Active Directory raporlama de iki tür güvenlik raporları içerir **bayrak eklenen kullanıcılar için risk** ve **riskli oturum açma işlemleri**, hangi yardımcı olabilir, Azure ortamınızda olası riskleri izleyin.

Raporlama hizmeti hakkında daha fazla bilgi için bkz: [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)

Ziyaret [Azure Active Directory etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) Azure Active Directory içinde kullanılabilir raporlar hakkında daha fazla ayrıntı için. Bu siteye erişmek ve kullanma hakkında daha fazla ayrıntı içeren [denetim günlüklerini etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) ve [oturum açma etkinliği raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) Portalı'nda. Hakkında bilgiler de içerir [bayrak eklenen kullanıcılar için risk](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) ve [riskli oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins) güvenlik raporları.

Ziyaret [Azure Active Directory denetim API Başvurusu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) program aracılığıyla raporlama Azure dizinine bağlanma hakkında daha fazla bilgi için site.

### <a name="log-analytics"></a>Log Analytics

[Günlük analizi](https://azure.microsoft.com/services/log-analytics/) için [Azure İzleyicisi'nden veri toplama](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage) diğer verilerle ilişkilendirmek ve ek çözümleme sağlamak için. Azure İzleyici toplar ve Azure ortamınız için izleme verilerini analiz eder. 

Tüm ortamınız merkezi analizini sağlayan tüm toplanan verileri karşı günlük analizi analiz araçları günlük aramalar, görünümler ve çözümleri gibi çalışır. Günlük analizi toplamak ve Windows olay günlükleri, IIS günlüklerini ve kişisel verilerin yetkisiz kullanıcılara maruz bırakabileceğinden olası kişisel veriler ihlallerini belirlemenize yardımcı olabilir Syslog modüllerini analiz edin.

#### <a name="how-do-i-use-log-analytics"></a>Günlük analizi nasıl kullanabilirim?

Günlük analizi OMS portalında veya Azure portalından herhangi bir web tarayıcısı üzerinden erişebilirsiniz. Log Analytics, depodaki verileri hızlı bir şekilde almak ve birleştirmek için bir sorgu dili içerir. Oluşturun ve günlük doğrudan portal içinde verileri çözümlemek için aramaları kaydedin.

Azure portalında günlük analizi çalışma alanı oluşturmak için aşağıdakileri yapın:

1. Seçin **günlük analizi** Market Hizmetleri'nde listesinden.

2. Seçin **oluşturun,** sonra OMS çalışma adını belirtin, abonelik, kaynak grubu, konum seçin ve fiyatlandırma katmanı.

3. Tıklatın **Tamam** alanlarınızı listesini görüntülemek için.

4. Ayrıntılarını görmek için bir çalışma alanı seçin.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

Ziyaret [günlük analizi belgeleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) hizmeti hakkında daha fazla bilgi için.

Ziyaret [günlük analizi çalışma alanı ile çalışmaya başlama](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) bir değerlendirme çalışma alanı oluşturmak ve hizmetini kullanmayı temel bilgileri öğrenmek için öğretici.

Yukarıda açıklanan günlükleri ile günlük analizi kullanılacak bağlanma hakkında daha fazla bilgi edinmek için aşağıdaki web sayfalarını ziyaret edin:

[Windows olay günlüklerini veri kaynaklarında, günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[IIS günlük analizi günlüğe kaydeder](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[Syslog veri kaynaklarında günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Azure İzleyici/Azure etkinlik günlüğü 

[Azure İzleyici](https://azure.microsoft.com/services/monitor/) taban düzeyi altyapı ölçümleri ve günlükleri çoğu Microsoft Azure hizmetlerini sağlar.
İzleme Azure uygulamalarınızı hakkında ayrıntılı Öngörüler elde size yardımcı olabilir. Azure İzleyici çoğu uygulama düzeyindeki ölçümlerini ve günlükleri toplamak için Azure tanılama uzantısını (Windows veya Linux) kullanır. [Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) Azure İzleyicisi ile görüntüleyebilirsiniz kaynakları biridir. Her API çağrısı izler ve bol miktarda oluşan etkinlikleri hakkında bilgi sağlayan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
Azure altyapısı tarafından görülen, kaynak hakkında bilgi için (daha önce işlemsel veya denetim günlüklerini denir) etkinlik günlüğü arayın. 

Etkinlik günlüğüne bilgilerin büyük bölümünü performans ve hizmet sistem durumu için ilgilidir rağmen da aynı zamanda veri koruma ile ilgili bilgileri yoktur. Etkinlik günlüğü kullanarak, belirleyebilirsiniz "ne, kimin, ne zaman ve" herhangi bir yazma işlemleri (PUT, POST, DELETE), Azure aboneliğinizin kaynaklarında alınan için.

Örneğin, yönetici kişisel verilerinizi koruma etkileyebilir bir ağ güvenlik grubu sildiğinde bir kaydı sağlar. Etkinlik günlüğü girişleri Azure İzleyicisi'nde 90 gün süreyle depolanır.

#### <a name="how-do-i-use-the-data-collected-by-azure-monitor"></a>Azure İzleyici tarafından toplanan verileri nasıl kullanabilirim?

Bir etkinlik günlüğü ve diğer Azure İzleyici kaynakları veriler kullanmak için çeşitli yöntemler vardır.

- Gerçek satır başka bir konumda veri akışını sağlayabilirsiniz.

- Kullanarak Varsayılanları daha uzun süreler için verileri depolayabilirsiniz bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction) ve bir bekletme ilkesi ayarlama.

- Kullanarak visual grafik ve grafikler, veriler yapabilirsiniz [Azure portal](https://azure.microsoft.com/features/azure-portal/), [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft PowerBI](https://powerbi.microsoft.com/), veya üçüncü taraf görselleştirme araçları.

- Azure İzleyici REST API'sini CLI komutları kullanarak verileri sorgulayabilir [PowerShell](https://docs.microsoft.com/powershell/) cmdlet'lerini veya .NET SDK'sı.

Azure İzleyicisi ile çalışmaya başlamak için seçin **daha Hizmetleri** Azure portalında.

1. Ekranı aşağı kaydırarak **İzleyici** içinde **izleme ve yönetme** bölümü.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  İzleyici açılır **etkinlik günlüğü** görünümü.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

Ortak filtrelere yönelik sorgular oluşturup kaydedebilir ve sonra ölçütlerinizi karşılayan olayların gerçekleşip gerçekleşmediğinden her zaman haberdar olmak için en önemli sorguları bir portal panosuna sabitleyebilirsiniz.

1. Kaynak grubu, timespan ve olay kategorisi görünümünü filtreleyebilirsiniz.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Tıklayarak sonra portal panosu için sorgular sabitleyebilirsiniz **PIN** düğmesi. Bu bilgi işlem verilerini için tek bir kaynağı hizmetlerinizi üzerinde oluşturmanıza yardımcı olur. Sorgu adı ve sonuç sayısı panosunda görüntülenir.

Tüm Azure kaynakları için ölçümleri görüntülemek için tanılama ayarları ve Uyarıları yapılandırmak ve günlüğe arama İzleyicisi'ni de kullanabilirsiniz. Etkinlik günlüğü ve Azure İzleyicisi'ni kullanma hakkında daha fazla bilgi için bkz: [Azure İzleyicisi ile çalışmaya başlama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Azure Tanılama 

Azure tanılama yeteneği çeşitli kaynaklardan veri koleksiyonunu sağlar. Güvenlik günlüğü dahil, Windows olay günlükleri, izleme ve kişisel verilere yönelik korumayı belgeleme özellikle yararlı olabilir. Güvenlik günlüğü oturum açma başarılı ve başarısız olaylar yanı sıra izin değişiklikleri, belirli türde bir saldırıları, güvenlikle ilgili ilkelerini yapılan değişiklikler, güvenlik grubu üyeliği değişiklikleri ve daha fazlasını belirten desenleri algılanması izler.

Örneğin, olay kimliği 4695 denetlenebilir korumalı verilere denenen unprotection için sizi uyarır. İçin veri koruma API'si (özel anahtarları, depolanan kimlik bilgileri ve diğer gizli bilgiler gibi verileri korumaya yardımcı DPAPI), ilgilidir.

#### <a name="how-do-i-enable-the-diagnostics-extension-for-windows-vms"></a>Windows VM'ler için nasıl Tanılama uzantısını etkinleştirilsin mi?

Günlük veri toplamak amacıyla bir Windows VM için tanılama uzantısını etkinleştirmek için PowerShell kullanın. Bunu yapmak için adımlar hangi dağıtım modeline (Resource Manager veya Klasik) kullandığınız bağlıdır. Resource Manager dağıtım modeli oluşturulmuş mevcut bir VM'yi tanılama uzantısını etkinleştirmek için kullanabileceğiniz [Set-AzureRMVMDiagnosticsExtension PowerShell cmdlet'ini](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1).

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$diagnosticsconfig_path* XML tanılama yapılandırması içeren dosyanın yolu. Daha ayrıntılı bir VM'de Azure tanılama etkinleştirme yönergeleri için bkz: [PowerShell Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için kullanın.](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics)

Azure tanılama uzantısını toplanan veriler bir Azure depolama hesabı aktarın veya Application Insights gibi hizmetleri gönderin. Denetim için daha sonra verileri kullanabilirsiniz.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Nasıl depolamak ve tanılama verilerini görüntüleme?

Microsoft Azure storage öykünücüsü veya Azure depolama transfer sürece Tanılama verileri kalıcı olarak depolanmaz unutmamak önemlidir. Depolamak ve Tanılama verileri Azure depolama alanında görüntülemek için aşağıdaki adımları izleyin:

1. Bir depolama hesabı ServiceConfiguration.cscfg dosyasında belirtin. Azure tanılama Blob hizmeti veya veri türüne bağlı olarak tablo hizmetini kullanabilirsiniz. Windows olay günlükleri, tablo biçiminde depolanır.

2. Veri aktarım. Yapılandırma dosyası aracılığıyla tanılama veri aktarmak isteyebilir. SDK 2.4 ve önceki, istek program aracılığıyla da yapabilirsiniz.

3. Kullanarak verileri görüntülemek [Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), [Sunucu Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) Visual Studio'da veya [Azure tanılama Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) Azure Management Studio'da.

Bu adımların her biri gerçekleştirme hakkında daha fazla bilgi için bkz: [deposu ve görünüm Tanılama verileri Azure depolama.](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)

### <a name="azure-storage-analytics"></a>Azure Depolama Analizi 

Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder. Bu bilgiler hizmete depolanmış kişisel verilere erişimi belgeleme yardımcı olabilecek istekleri ayrı ayrı izlemek için kullanılabilir. Ancak, depolama çözümlemeleri günlüğü depolama hesabınız için varsayılan olarak etkin değildir. Azure Portalı'nda etkinleştirebilirsiniz.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>İçin bir depolama hesabı izleme nasıl yapılandırabilirim?

İzleme için bir depolama hesabı yapılandırmak için aşağıdakileri yapın:

1. Seçin **depolama hesapları** Azure portalında ardından izlemek istediğiniz hesabın adını seçin.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. İçinde **izleme** bölümünde, select **tanılama.**

3.  Seçin **türü** her hizmet için (Blob, tablo, dosyası) izlemek istediğiniz ölçümleri verileri. Okuma, yazma ve silme isteklerinin blob, tablo ve kuyruk Hizmetleri için tanılama günlükleri kaydetmek için Azure Storage istemek üzere seçin **Blob günlükleri, tablo günlükleri** ve **sıraya günlükleri.**

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. Alt kısmında, kaydırıcıyı kullanarak **bekletme** İlkesi gün (1-365 değeri). Varsayılan yedi gündür.

5. Seçin **kaydetmek** yapılandırma ayarlarını uygulamak için.

Depolama günlük günlük girişlerinin istekleri ayrı ayrı hakkında aşağıdaki bilgileri içerir:

- Başlangıç saati, uçtan uca gecikme süresi ve sunucu gecikme süresi gibi zamanlama bilgileri.

- Depolama işlemi işlemi gibi ayrıntılarını yazın, depolama nesnesi istemci erişimi, başarı veya başarısızlık anahtarıdır ve istemciye HTTP durum kodunu döndürdü.

- İstemcinin kullandığı kimlik doğrulama türü gibi kimlik doğrulama ayrıntıları.

- Son değiştirilen zaman damgasını ve ETag değeri gibi eşzamanlılık bilgileri.

- İstek ve yanıt iletileri boyutları.

Daha ayrıntılı Storage Analytics günlüğü etkinleştirme hakkında yönergeler için bkz: [Azure portalında bir depolama hesabını izleme.](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi 

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) önlemek ve tehditleri algılayabilir ve yanıtlamak için öneriler sunmak için Azure kaynaklarınızın güvenlik durumunu izler. Bu belge yardımcı olmak için çeşitli yollar kişisel verilerin gizliliği korumak, güvenlik önlemleri sağlar.

Güvenlik durumunu izleme, güvenlik ilkeleriyle uyumluluğunu olmanıza yardımcı olur. Güvenlik İzleme, kuruluş standartlarıyla veya en iyi yöntemler karşılamayan sistemlerini tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye olur. Aşağıdaki kaynaklar güvenlik durumunu izleyebilirsiniz:

- İşlem (sanal makineler ve bulut Hizmetleri)

- Ağ (sanal ağlar)

- Depolama ve verileri (sunucu ve veritabanı denetim ve tehdit algılama, TDE, depolama şifrelemesi)

- Uygulamaları (olası güvenlik sorunlarını)

Aşağıdaki kategorilerden herhangi birinde bulunan güvenlik sorunlarını kişisel verilerin gizliliği için bir tehdit teşkil.

#### <a name="how-do-i-view-the-security-state-of-my-azure-resources"></a>My Azure kaynaklarınızın güvenlik durumunu nasıl görüntüleyebilirim?

Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Tanımlar, olası güvenlik açıkları görüntüleyebilirsiniz **önleme** bölümü.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. İçinde **önleme** bölümünde, select **işlem** döşeme. Burada görürsünüz bir **genel bakış,** ile birlikte **sanal makineleri** tüm sanal makineleri ve güvenlik durumlarını ve **bulut hizmetlerini** web ve çalışan rolleri listesi Güvenlik Merkezi tarafından izlenen.

2. Üzerinde **genel bakış** sekmesinde, daha fazla bilgi görüntülemek için bir öneri ikinci.

3. Üzerinde **sanal makineleri** sekmesinde, ek ayrıntıları görüntülemek için bir VM seçin.

Veri toplamayı Azure Güvenlik Merkezi'nde etkinleştirildiğinde, tüm var olan Microsoft Monitoring Agent otomatik olarak sağlanır ve desteklenen herhangi bir yeni dağıtılan sanal makineler. Bu Aracıdan toplanan verileri ya da var olan içinde depolanan [günlük analizi](https://azure.microsoft.com/services/log-analytics/) aboneliğinizi veya yeni bir çalışma alanı ile ilişkili çalışma.

[Tehdit yönetim bilgileri raporları](https://docs.microsoft.com/azure/security-center/security-center-threat-report) Güvenlik Merkezi tarafından sağlanır. Bu, saldırganın kimlik, hedefler, geçerli ve geçmiş saldırı kampanyaları ve taktiği, araçları ve kullanılan yordamlar keşfedilir yardımcı olmak için yararlı bilgiler sağlar. Azaltma ve düzeltme bilgileri de eklenmiştir.

Bu tehdit raporları birincil amacı, hemen tehdit etkili bir şekilde yanıt yardımcı olur ve daha sonra sorunu azaltmak için önlemleri yardımcı olmaktır. Raporlama ve denetim amacıyla, olay yanıtlama belgelerken raporlardaki bilgileri da yararlı olabilir.

Tehdit yönetim bilgileri raporları sunulur. Bir bağlantı üzerinden erişilen, PDF biçimli **raporları** alanını **yürütülen şüpheli işlem** Azure Güvenlik Merkezi'nde her güvenlik uyarısı için dikey.

Görüntüleme ve tehdit Intelligence rapor kullanma hakkında daha fazla bilgi için bkz: [Azure Güvenlik Merkezi tehdit Intelligence rapor.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

## <a name="next-steps"></a>Sonraki Adımlar:

[Azure Active raporlama API'si Directory ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Log Analytics nedir?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[Microsoft Azure'da İzlemeye Genel Bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Azure etkinlik günlüğü (video) giriş](https://azure.microsoft.com/resources/videos/intro-activity-log/)
