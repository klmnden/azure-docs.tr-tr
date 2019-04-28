---
title: Azure Site Recovery kullanarak çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, Azure Site Recovery özelliklerini kullanarak çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarma ayarlamayı açıklar.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: 5f477cf20b817d7a6c8be856636bf1e3755b5424
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472156"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Azure Site Recovery ile olağanüstü durum kurtarma için çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarmayı ayarlama

Bu makalede kullanarak bir SharePoint uygulama koruma konusunda ayrıntılı olarak [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Genel Bakış

Microsoft SharePoint grubu yardımcı olabilecek güçlü bir uygulamadır veya departman düzenlemek, işbirliği yapın ve bilgi paylaşın. SharePoint intranet portalları, belge ve dosya yönetimi, işbirliği, sosyal ağlar, extranet'ler, Web siteleri, kurumsal arama ve iş zekası sağlayabilir. Ayrıca, sistem tümleştirmesi, süreç tümleştirme ve iş akışı Otomasyonu özellikleri de vardır. Genellikle, kuruluşlar, 1. Katman uygulama olarak kapalı kalma süresi ve veri kaybına karşı duyarlı göz önünde bulundurun.

Bugün, Microsoft SharePoint, herhangi bir kullanıma hazır olağanüstü durum kurtarma özellikleri sağlamaz. Tür ve olağanüstü bir durum ölçeğini bakılmaksızın, Kurtarma grubuna kurtarabilirsiniz bir yedek veri merkezi kullanımını içerir. Yedek veri merkezleri, burada yerel olarak yedekli sistemler ve yedeklemeler birincil veri merkezinde kesinti kurtaramazsınız senaryoları için gereklidir.

İyi bir olağanüstü durum kurtarma çözümü modelleme kurtarma planları, SharePoint gibi karmaşık uygulama mimarileri etrafında izin vermelidir. Ayrıca, çeşitli katmanları ve bu nedenle tek tıklamayla yük devretme ile daha düşük bir RTO olağanüstü bir durumda sağlama arasındaki uygulama eşlemeleri işlemek için özelleştirilmiş adım ekleme olanağı da olmalıdır.

Bu makalede kullanarak bir SharePoint uygulama koruma konusunda ayrıntılı olarak [Azure Site Recovery](site-recovery-overview.md). Bu makalede ele alınacaktır üç çoğaltmaya yönelik en iyi Azure, olağanüstü durum kurtarma tatbikatı nasıl yapabilirsiniz ve uygulamayı azure'a yük devretme getirmeyi SharePoint uygulama katmanı.

İzleyebilir çok katmanlı uygulamayı azure'a kurtarma hakkında videonun altında.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri bildiğinizden emin olun:

1. [Bir sanal makine Azure'a çoğaltılırken](site-recovery-vmware-to-azure.md)
2. Nasıl yapılır [bir kurtarma ağı tasarlama](site-recovery-network-design.md)
3. [Azure'a yük devretme testi yapılması](site-recovery-test-failover-to-azure.md)
4. [Azure'a yük devretme gerçekleştirmeden](site-recovery-failover.md)
5. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
6. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint mimarisi

SharePoint sunucularında, bir veya daha çok katmanlı topoloji ve sunucu rollerini belirli hedefleriniz ve amaçlarınız uyan bir grup tasarım uygulamak için kullanılarak dağıtılabilir. Çok sayıda eşzamanlı kullanıcıyı ve çok sayıda içerik öğeleri destekleyen bir tipik büyük, çok talep edilen SharePoint sunucu grubu hizmeti gruplandırma kendi ölçeklenebilirlik stratejisinin bir parçası kullanın. Bu yaklaşım, bu hizmetler birlikte gruplandırarak ve ardından sunucularını bir grup olarak ölçeklendirme belinlenmiş sunuculara Hizmetleri çalıştırılmasını içerir. Aşağıdaki topoloji hizmet ve sunucu gruplandırma üç katmanlı bir SharePoint sunucu grubu gösterir. SharePoint belgeleri ve ürün satır mimarileri farklı SharePoint topolojiler hakkında ayrıntılı yönergeler için lütfen bakın. SharePoint 2013 dağıtımı hakkında daha fazla ayrıntı bulabilirsiniz [bu belgeyi](https://technet.microsoft.com/library/cc303422.aspx).



![Dağıtım modeli 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makalede oluşturmak için Windows Server 2012 R2 Enterprise ile VMware sanal makinelerini kullanıldı. SharePoint 2013 Enterprise edition ve SQL server 2014 Enterprise Edition sürümü kullanılmıştır. Site Recovery çoğaltma uygulamadan bağımsız olduğundan, burada sağlanan öneriler de aşağıdaki senaryolar için beklemeye beklenir.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **Azure’a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet
**Azure** | NA | Evet

### <a name="sharepoint-versions"></a>SharePoint sürümleri
Aşağıdaki SharePoint server sürümleri desteklenir.

* SharePoint server 2013'ün standart
* SharePoint server 2013 Enterprise
* SharePoint server 2016 Standard
* SharePoint server 2016 Enterprise

### <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar

Uygulamanızda herhangi bir katmanı paylaşılan disk tabanlı küme kullanıyorsanız, daha sonra bu sanal makineleri çoğaltmak için Site Recovery çoğaltması kullanmak mümkün olmayacaktır. Uygulama tarafından sağlanan yerel çoğaltma kullanın ve ardından bir [kurtarma planı](site-recovery-create-recovery-plans.md) yük devretme için tüm katmanları.

## <a name="replicating-virtual-machines"></a>Çoğaltılan sanal makineler

İzleyin [bu kılavuz](site-recovery-vmware-to-azure.md) sanal makineyi Azure'a çoğaltmaya başlamak için.

* Çoğaltma işlemi tamamlandıktan sonra her katman, her sanal makineye gidin ve aynı kullanılabilirlik kümesinde seçin emin ' çoğaltılan öğe > Ayarlar > Özellikler > işlem ve ağ '. Örneğin, web katmanınızın 3 sanal makineye sahipse, 3 tüm VM'lerin aynı kullanılabilirlik kümesi Azure'da bir parçası olarak yapılandırıldığından emin olun.

    ![Kümesi kullanılabilirlik kümesi](./media/site-recovery-sharepoint/select-av-set.png)

* Active Directory ve DNS'yi koruma ile ilgili yönergeler için bkz [korumak Active Directory ve DNS](site-recovery-active-directory.md) belge.

* Veritabanı katmanı SQL sunucusunda çalışan koruma ile ilgili yönergeler için bkz [SQL Server koruma](site-recovery-active-directory.md) belge.

## <a name="networking-configuration"></a>Ağ yapılandırması

### <a name="network-properties"></a>Ağ özellikleri

* Böylece yük devretme işleminden sonra VM'lerin doğru DR ağa bağlı uygulama ve Web Katmanı VM'ler için ağ ayarları Azure portalında yapılandırın.

    ![Ağ seçin](./media/site-recovery-sharepoint/select-network.png)


* Statik IP kullanıyorsanız, ardından sanal makineyi yapılacağını istediğiniz IP belirtin **hedef IP** alan

    ![Statik IP ayarlayın](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS ve trafik yönlendirme

Internet'e yönelik siteler için ['Öncelik' türünde bir Traffic Manager profili oluşturma](../traffic-manager/traffic-manager-create-profile.md) Azure aboneliğinde. Ve ardından, DNS ve Traffic Manager profilinizi şu şekilde yapılandırın.


| **Burada** | **Kaynak** | **Hedef**|
| --- | --- | --- |
| Genel DNS | SharePoint siteleri için Genel DNS <br/><br/> Örn: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.net |
| Şirket içi DNS | sharepointonprem.contoso.com | Şirket içi Grup genel IP |


Traffic Manager profilindeki [birincil ve kurtarma uç noktaları oluşturma](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Şirket içi uç noktası ve genel IP için Azure uç noktası için dış uç noktası'nı kullanın. Öncelik şirket içi uç noktasına daha yüksek ayarlanmış olduğundan emin olun.

Belirli bir bağlantı noktasını (örneğin, 800) bir test sayfası sırada kullanılabilirlik yük devretme sonrasında otomatik olarak algılamak için Traffic Manager'için SharePoint web katmanındaki barındırın. Anonim kimlik doğrulaması SharePoint siteleriniz birini etkinleştiremiyor durumunda geçici bir çözüm budur.

[Traffic Manager profilinin yapılandırma](../traffic-manager/traffic-manager-configure-priority-routing-method.md) ile ayarları aşağıda.

* Yönlendirme yöntemi - 'Öncelik'
* DNS süresi (TTL) - ' 30 saniye
* Anonim kimlik doğrulamasını etkinleştirirseniz, uç nokta İzleyicisi ayarları - belirli bir Web uç noktası verebilirsiniz. Veya bir test sayfası, belirli bir bağlantı noktasında (örneğin, 800) kullanabilirsiniz.

## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma

Bir kurtarma planı yük devretme çok katmanlı bir uygulama, bu nedenle, çeşitli katmanlarda uygulama tutarlılığı sürdürmeye sıralama sağlar. İzleyin aşağıdaki çok katmanlı web uygulaması için bir kurtarma planı oluşturulurken adımları. [Bir kurtarma planı oluşturma hakkında daha fazla bilgi edinin](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler için yük devretme grupları ekleme

1. Web Katmanı Vm'leri ve uygulama ekleyerek bir kurtarma planı oluşturun.
2. 'Sanal makineleri gruplandırmak için Özelleştir' tıklayın. Varsayılan olarak, tüm VM'ler 'Grubu 1' bir parçasıdır.

    ![RP özelleştirme](./media/site-recovery-sharepoint/rp-groups.png)

3. Başka bir grup (Grup 2) oluşturabilir ve Web Katmanı Vm'lerini yeni gruba taşıyın. Uygulama katmanınızı VM'ler 'Grubu 1' bölümü olmalıdır ve Web Katmanı Vm'lerini 'Grubu 2' parçası olmalıdır. Uygulama katmanı VM'ler önyükleme yukarı emin olmak için ilk Web Katmanı VM'ler tarafından izlenen budur.


### <a name="adding-scripts-to-the-recovery-plan"></a>Betikler, kurtarma planına ekleme

En yaygın olarak kullanılan Azure Site Recovery betikleri için 'Azure'a Dağıt' düğmesini Otomasyon hesabınıza dağıtabilirsiniz. Yayımlanan herhangi bir komut dosyası kullanırken, komut dosyasındaki yönergeleri emin olun.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Bir ön eylem betiği 'Grubu 1' için yük devretme SQL kullanılabilirlik grubuna ekleyin. Örnek betiklerde yayımlanan 'ASR-SQL-FailoverAG' betiğini kullanın. Komut dosyasındaki yönergeleri izleyin ve komut dosyasında gerekli değişiklikleri yapın, uygun şekilde emin olun.

    ![Ekle-AG-betik--1. adım](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Ekle-AG-betik--2. adım](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Yük dengeleyicide başarısız eklemek için son eylem betik ekleme yükü devredilen sanal makineler, Web Katmanı (Grup 2). Örnek betiklerde yayımlanan 'AddSingleLoadBalancer ASR' betiğini kullanın. Komut dosyasındaki yönergeleri izleyin ve komut dosyasında gerekli değişiklikleri yapın, uygun şekilde emin olun.

    ![Ekle-LB-betik--1. adım](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Ekle-LB-betik--2. adım](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Azure'da yeni grubuna işaret edecek şekilde DNS kayıtlarını güncelleştirmeye yönelik el ile bir adım ekleyin.

    * Siteleri için Internet'e, yük devretme sonrasında gerekli hiçbir DNS güncelleştirmelerdir. Traffic Manager'ı yapılandırmak için 'Ağ Kılavuzu' bölümünde açıklanan adımları izleyin. Traffic Manager profili önceki bölümde açıklanan şekilde ayarlandığını gösterdiğinde, Azure sanal makinesinde işlevsiz bağlantı (örnekte 800) açmak için bir komut dosyası ekleyin.

    * İç kullanıma yönelik siteler için DNS kaydı için yeni Web Katmanı sanal makinenin yük dengeleyici IP işaret edecek şekilde güncelleştirmek için el ile bir adım ekleyin.

4. Arama uygulaması bir yedekten geri yükleyin veya yeni bir arama hizmeti başlatmak için el ile bir adım ekleyin.

5. Arama hizmeti uygulaması bir yedeklemeden geri yüklemek için aşağıdaki adımları izleyin.

    * Bu yöntem, arama hizmeti uygulaması yedeğini felaket önce gerçekleştirildi ve yedekleme DR sitede kullanılabilir olduğunu varsayar.
    * Bu kolayca Yedekleme Zamanlama (örneğin, her gün bir kez) ve yedekleme DR sitede yerleştirmek için bir kopya yordam kullanılarak gerçekleştirilebilir. Kopyalama yordamları AzCopy (Azure kopyalama) veya DFSR (Dağıtılmış Dosya Hizmetleri çoğaltma) ayarlama gibi betikleştirilmiş programları içerebilir.
    * SharePoint grubu çalıştırıyor, Merkezi 'Yedekleme ve geri yükleme' yönetim gidin ve geri yükleme seçeneğini belirleyin. Belirtilen yedekleme konumuna geri interrogates (değeri olarak güncelleştirmeniz gerekebilir). Arama hizmeti uygulaması yedeklemeyi geri yüklemek istediğiniz seçin.
    * Geri arama. Aynı bulmak için geri yükleme bekliyor akılda tutulması topolojisi (aynı sayıda sunucuları) ve aynı sabit sürücü harfini bu sunuculara atanmış. Daha fazla bilgi için ['Geri arama hizmeti uygulaması SharePoint 2013'te '](https://technet.microsoft.com/library/ee748654.aspx) belge.


6. Yeni bir arama hizmeti uygulaması'nı başlatmak için aşağıdaki adımları izleyin.

    * Bu yöntem, "Arama yönetimi" veritabanının bir yedeğini DR sitede kullanılabilir olduğunu varsayar.
    * Bir arama hizmeti uygulaması veritabanları çoğaltılamaz olduğundan, yeniden oluşturulması gerekir. Bunu yapmak için Yönetim Merkezi'ne gidin ve arama hizmeti uygulaması silin. Arama dizini barındıran tüm sunucularda dizin dosyaları silin.
    * Arama hizmeti uygulaması yeniden oluşturun ve bu veritabanlarını yeniden oluşturur. GUI aracılığıyla tüm eylemleri gerçekleştirmek mümkün olmadığından, bu hizmet uygulaması yeniden oluşturan hazırlanmış bir komut dosyası olması önerilir. Örneğin, dizin sürücü konumunu ayarlama ve arama topolojisini yapılandırma yalnızca SharePoint PowerShell cmdlet'lerini kullanarak mümkündür. Geri yükleme-SPEnterpriseSearchServiceApplication Windows PowerShell cmdlet'ini kullanın ve günlük aktarmalı ve çoğaltılan arama yönetim veritabanı, Search_Service__DB belirtin. Bu cmdlet, arama yapılandırması, şema, yönetilen özellikleri, kuralları ve kaynakları sunar ve diğer bileşenleri varsayılan kümesi oluşturur.
    * Arama hizmeti uygulaması olan sonra yeniden oluşturulan arama hizmetini geri yüklemek her bir içerik kaynağı için tam gezinme başlatmanız gerekir. Şirket içi grup, arama önerileri gibi bazı analytics bilgileri kaybedersiniz.

7. Sonra tüm adımları tamamlanır, kurtarma planını ve nihai kurtarma planı aşağıdaki gibi görünür.

    ![Kaydedilen RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Yük devretme testi yapılması
İzleyin [bu kılavuz](site-recovery-test-failover-to-azure.md) yük devretme testi yapmak için.

1.  Azure portalına gidin ve kurtarma Hizmetleri kasanızı seçin.
2.  SharePoint uygulaması için oluşturulmuş kurtarma planına tıklayın.
3.  'Test yük devretme sırasında' tıklayın.
4.  Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5.  İkincil ortamı çalışır duruma geldiğinde, uygulamanızı doğrulamaları gerçekleştirebilirsiniz.
6.  Doğrulama tamamlandıktan sonra 'Yük devretme testini Temizle' kurtarma planı üzerinde tıklayın ve yük devretme test ortamı temizlendi.

AD için yük devretme testi yapılması yönergeler ve DNS, başvurduğu [Test yük devretme konuları için bir AD ve DNS](site-recovery-active-directory.md#test-failover-considerations) belge.

Yük devretme testi SQL Always ON kullanılabilirlik grupları yapılması ile ilgili yönergeler için bkz [yük devretme testi yapmak için SQL Server Always On](site-recovery-sql.md#steps-to-do-a-test-failover) belge.

## <a name="doing-a-failover"></a>Bir yük devretme gerçekleştirmeden
İzleyin [bu kılavuz](site-recovery-failover.md) bir yük devretme gerçekleştirme.

1.  Azure portalına gidin ve kurtarma Hizmetleri kasanızı seçin.
2.  SharePoint uygulaması için oluşturulmuş kurtarma planına tıklayın.
3.  'Yük devretme sırasında' tıklayın.
4.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinebilirsiniz [diğer uygulamaların çoğaltılması](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
