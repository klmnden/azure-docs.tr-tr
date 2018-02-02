---
title: "Azure Site Recovery kullanan çok katmanlı SharePoint uygulaması çoğaltmak | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery özelliklerini kullanarak çok katmanlı bir SharePoint uygulama çoğaltmak açıklar."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/23/2017
ms.author: sutalasi
ms.openlocfilehash: 3610409691b71fcce0c36a3af94184dbe6db8661
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="replicate-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Azure Site RECOVERY'yi kullanarak olağanüstü durum kurtarma için çok katmanlı bir SharePoint uygulama Çoğalt

Bu makalede kullanarak bir SharePoint uygulama koruma konusunda ayrıntılı olarak [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Genel Bakış

Microsoft SharePoint grubu yardımcı olabilecek güçlü bir uygulamadır veya departman düzenlemek, birlikte çalışın ve bilgileri paylaşın. SharePoint intranet portalları, belge ve dosya yönetimi, işbirliği, sosyal ağlara, extranet'ler, Web siteleri, kurumsal arama ve business Intelligence sağlayabilir. Ayrıca, sistem tümleştirme, işlem tümleştirme ve iş akışı Otomasyon özellikleri de sahiptir. Genellikle, kuruluşlar, Katman 1 uygulama olarak önemli kapalı kalma süresi ve veri kaybını göz önünde bulundurun.

Bugün, Microsoft SharePoint tüm giden kutusu olağanüstü durum kurtarma özellikleri sağlamaz. Türü ve bir olağanüstü durum ölçeğini bakılmaksızın, Kurtarma gruba kurtarabilirsiniz bir yedek veri merkezi kullanmayı içerir. Bekleme veri merkezleri burada yerel olarak yedekli sistemler ve yedeklemeleri birincil veri merkezinde kesinti kurtaramazsınız senaryoları için gereklidir.

İyi olağanüstü durum kurtarma çözümü modelleme kurtarma planlarının bir SharePoint gibi karmaşık uygulama mimarileri geçici izin vermeniz gerekir. Ayrıca çeşitli katmanları ve bu nedenle tek tıklamayla yük devretme ile düşük RTO bir olağanüstü durumda sağlama arasındaki uygulama eşlemeleri işlemek için özelleştirilmiş adımları ekleme yeteneği sahip olmalıdır.

Bu makalede kullanarak bir SharePoint uygulama koruma konusunda ayrıntılı olarak [Azure Site Recovery](site-recovery-overview.md). Bu makalede ele alınacaktır bir üç çoğaltmak için en iyi uygulamalar Azure, nasıl bir olağanüstü durum kurtarma detaylandırma yapabilirsiniz ve uygulamayı Azure'a yük devretme işlemine SharePoint uygulama katmanı.

İzleyebilir aşağıdaki videoya Azure multi katmanı uygulamaya kurtarma hakkında.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri bildiğinizden emin olun:

1. [Bir sanal makine Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
2. Nasıl yapılır [kurtarma ağını tasarlama](site-recovery-network-design.md)
3. [Azure'a yük devretme sınamasını yapmak](site-recovery-test-failover-to-azure.md)
4. [Azure için bir yük devretme işleminden](site-recovery-failover.md)
5. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
6. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint mimarisi

SharePoint sunucularında, bir veya daha çok katmanlı topoloji ve sunucu rollerini belirli hedefler ve hedefleri karşılayan grubu tasarımını uygulamak için kullanılarak dağıtılabilir. Çok sayıda eşzamanlı kullanıcı ve çok sayıda içerik öğelerini destekleyen bir tipik büyük, yüksek isteğe SharePoint sunucu grubu hizmeti gruplandırma kendi ölçeklenebilirlik stratejisinin bir parçası kullanın. Bu yaklaşım, bu hizmetleri birlikte gruplandırma ve ardından sunucuları dışında bir grup olarak ölçekleme adanmış sunuculara Hizmetleri çalıştırılmasını içerir. Aşağıdaki topoloji hizmeti ve üç katmanı bir SharePoint sunucu grubu gruplandırma server gösterir. SharePoint belgeleri ve ürün satır mimari farklı SharePoint topolojileri hakkında ayrıntılı yönergeler için lütfen bakın. SharePoint 2013 dağıtımı hakkında daha fazla ayrıntı bulabilirsiniz [bu belgeyi](https://technet.microsoft.com/en-us/library/cc303422.aspx).



![Dağıtım modeli 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Site kurtarma desteği

Bu makalede oluşturmak için Windows Server 2012 R2 Enterprise VMware sanal makineleri kullanılmıştır. SharePoint 2013 Enterprise edition ve SQL server 2014 Enterprise edition kullanılmıştır. Site Recovery çoğaltma uygulama belirsiz olduğundan, burada sağlanan öneriler de aşağıdaki senaryolar için tutacak beklenir.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **To Azure**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet
**Azure** | NA | Evet

### <a name="sharepoint-versions"></a>SharePoint sürümleri
Aşağıdaki SharePoint server sürümleri desteklenir.

* SharePoint server 2013 standart
* SharePoint server 2013 Enterprise
* SharePoint server 2016 standart
* SharePoint server 2016 Enterprise

### <a name="things-to-keep-in-mind"></a>Göz önünde bulundurmanız gerekenler

Uygulamanızda herhangi bir katmanı paylaşılan bir disk tabanlı küme kullanıyorsanız, daha sonra bu sanal makineleri çoğaltmak için Site Recovery çoğaltma kullanmanız mümkün olmaz. Uygulama tarafından sağlanan yerel çoğaltma kullanın ve ardından bir [kurtarma planı](site-recovery-create-recovery-plans.md) yük devretme için tüm katmanlarını.

## <a name="replicating-virtual-machines"></a>Çoğaltma sanal makineler

İzleyin [bu kılavuz](site-recovery-vmware-to-azure.md) Azure'da sanal makine çoğaltma başlatmak için.

* Çoğaltma işlemi tamamlandıktan sonra her katman her sanal makineye gidin ve aynı kullanılabilirlik kümesinde seçin emin olun ' yinelenmiş öğesi > Ayarlar > Özellikler > işlem ve ağ '. Örneğin, web katmanı 3 VM varsa, 3 tüm sanal makineleri aynı kullanılabilirlik kümesinde Azure'da bir parçası olarak yapılandırılmış emin olun.

    ![Set kullanılabilirlik kümesi](./media/site-recovery-sharepoint/select-av-set.png)

* Active Directory ve DNS'yi koruma ile ilgili yönergeler için bkz [korumak Active Directory ve DNS](site-recovery-active-directory.md) belge.

* Veritabanı katmanı SQL sunucusunda çalışan koruma ile ilgili yönergeler için bkz [SQL Server koruma](site-recovery-active-directory.md) belge.

## <a name="networking-configuration"></a>Ağ yapılandırması

### <a name="network-properties"></a>Ağ Özellikleri

* Sanal makineleri yük devretme sonrasında doğru DR ağa bağlı için uygulama ve Web Katmanı VM'ler için Azure Portalı'nda Ağ ayarlarını yapılandırın.

    ![Ağı seçin](./media/site-recovery-sharepoint/select-network.png)


* Bir statik IP kullanıyorsanız, sanal makine yapılacağını istediğiniz IP belirtin **hedef IP** alan

    ![Statik IP Ayarla](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS ve trafik yönlendirme

Siteler, Internet'e için ['Priority' türünde bir Traffic Manager profili oluşturma](../traffic-manager/traffic-manager-create-profile.md) Azure aboneliği. Ve ardından, DNS ve trafik Yöneticisi profili aşağıdaki şekilde yapılandırın.


| **Burada** | **Kaynak** | **Hedef**|
| --- | --- | --- |
| Genel DNS | Genel DNS SharePoint siteleri için <br/><br/> Örn: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.net |
| Şirket içi DNS | sharepointonprem.contoso.com | Şirket içi grubu üzerinde genel IP |


Trafik Yöneticisi profili içinde [birincil ve kurtarma uç noktaları oluşturma](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Şirket içi uç nokta ve Azure uç noktası için genel IP dış uç noktası kullan. Öncelik şirket içi uç noktasına daha yüksek ayarlandığından emin olun.

Sırada kullanılabilir post yük devretme otomatik olarak algılamak için trafik Yöneticisi'için SharePoint web katmanındaki belirli bir bağlantı noktası (örneğin, 800) test sayfada barındırır. Anonim kimlik doğrulaması, SharePoint siteleri hiçbirinde etkinleştiremiyor durumda geçici bir çözüm budur.

[Trafik Yöneticisi profili yapılandırma](../traffic-manager/traffic-manager-configure-priority-routing-method.md) ile ayarları altında.

* Yönlendirme yöntemi - 'Priority'
* DNS süresi (TTL) - ' 30 saniye
* Anonim kimlik doğrulamasını etkinleştirirseniz, uç nokta izleme ayarları - belirli bir Web uç noktası verebilirsiniz. Veya belirli bir bağlantı noktasına (örneğin, 800) bir test sayfası kullanabilirsiniz.

## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma

Bir kurtarma planı çeşitli katmanlarda çok katmanlı bir uygulama, bu nedenle, uygulama tutarlılığını sağlamak için yük devretme sıralama sağlar. İzleyin aşağıdaki çok katmanlı web uygulaması için bir kurtarma planı oluşturma sırasında adımları. [Bir kurtarma planı oluşturma hakkında daha fazla bilgi](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler yük devretme gruplara ekleme

1. Uygulama ve Web Katmanı VM'ler ekleyerek bir kurtarma planı oluşturun.
2. 'VM'ler gruplandırmak için Özelleştir' üzerinde'i tıklatın. Varsayılan olarak, tüm VM'ler 'Grubu 1' bir parçasıdır.

    ![RP özelleştirme](./media/site-recovery-sharepoint/rp-groups.png)

3. Başka bir grup (Grup 2) oluşturabilir ve Web Katmanı VM'ler yeni grubuna taşıyın. Uygulama katmanınızı VM'ler 'Grubu 1' parçası olması gerekir ve Web Katmanı VM'ler 'Grubu 2' parçası olması gerekir. Bu, uygulama katmanı VM'ler önyükleme yukarı ilk Web Katmanı VM'ler tarafından izlenen sağlamaktır.


### <a name="adding-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleniyor

En yaygın kullanılan Azure Site Recovery betikleri 'Azure'a dağıtma' düğmesini tıklatarak Otomasyon hesabınızda dağıtabilirsiniz. Yayımlanmış herhangi komut dosyası kullanırken, komut dosyası yer alan yönergeleri izleyin emin olun.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Bir ön eylem betiği 'Grubu 1' için yük devretme SQL kullanılabilirlik grubuna ekleyin. Örnek komut dosyalarını yayımlanan 'ASR-SQL-FailoverAG' komut dosyası kullanın. Komut dosyası yer alan yönergeleri izleyin ve uygun şekilde komut dosyasında gerekli değişiklikleri yapın emin olun.

    ![Add-AG-komut dosyası--1. adım](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Add-AG-komut dosyası--2. adım](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Başarısız bir yük dengeleyicideki eklemek için bir post eylem komut dosyası ekleme Web Katmanı (Grup 2) sanal makinelerin üzerinden. Örnek komut dosyalarını yayımlanan 'ASR-AddSingleLoadBalancer' komut dosyası kullanın. Komut dosyası yer alan yönergeleri izleyin ve uygun şekilde komut dosyasında gerekli değişiklikleri yapın emin olun.

    ![Add-LB-komut dosyası--1. adım](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Add-LB-komut dosyası--2. adım](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Azure yeni grupta işaret etmek için DNS kayıtlarını güncelleştirmek üzere el ile bir adımı ekleyin.

    * Siteleri için Internet'e, gerekli post yük devretme hiçbir DNS güncelleştirmelerdir. Trafik Yöneticisi'ni yapılandırmak için 'Ağ Kılavuzu' bölümünde açıklanan adımları izleyin. Trafik Yöneticisi profili önceki bölümde açıklandığı şekilde ayarlanmış olması durumunda Azure VM kukla bağlantı noktası (800 örnekte) açmak için bir komut dosyası ekleyin.

    * Dahili kullanıma yönelik siteleri için yeni Web Katmanı VM'in yük dengeleyici IP işaret edecek şekilde DNS kaydını güncelleştirmek üzere el ile bir adımı ekleyin.

4. Arama uygulaması bir yedekten geri yükleyin veya yeni bir arama hizmeti başlatmak için el ile bir adımı ekleyin.

5. Arama hizmeti uygulaması bir yedekten geri yüklemek için aşağıdaki adımları izleyin.

    * Bu yöntem, arama hizmeti uygulaması yedeğini geri dönülemez olay önce gerçekleştirildi ve yedekleme DR sitede kullanılabilir olduğunu varsayar.
    * Bu kolayca Yedekleme Zamanlama (örneğin, günde bir kez) ve yedekleme DR sitede yerleştirmek için bir kopya yordam kullanılarak gerçekleştirilebilir. Kopyalama yordamlarını AzCopy (Azure kopya) veya DFSR (Dağıtılmış Dosya Hizmetleri çoğaltma) ayarlama gibi komut dosyalı programları içerebilir.
    * SharePoint grubu çalıştığından, Merkezi 'Yedekleme ve geri yükleme' yönetim gidin ve seçin geri yükleyin. Belirtilen yedeklemeyi geri yükleme interrogates (değer güncelleştirmeniz gerekebilir). Geri yüklemek istediğiniz arama hizmeti uygulaması yedeklemeyi seçin.
    * Arama geri yüklendi. Aynı bulmak için geri yükleme bekliyor göz önünde bulundurmanız topolojisi (sunucuları aynı sayıda) ve aynı sabit sürücü harfleri bu sunuculara atanmış. Daha fazla bilgi için bkz: ['Geri arama hizmeti uygulaması SharePoint 2013'te '](https://technet.microsoft.com/library/ee748654.aspx) belge.


6. Yeni bir arama hizmeti uygulaması başlatmak için aşağıdaki adımları izleyin.

    * Bu yöntem, "Arama Yönetim" veritabanının bir yedeğini DR sitede kullanılabilir olduğunu varsayar.
    * Bir arama hizmeti uygulaması veritabanları çoğaltılmamış olduğundan, yeniden oluşturulması gerekir. Bunu yapmak için Yönetim Merkezi'ne gidin ve arama hizmeti uygulaması silin. Arama dizini barındıran tüm sunucularda dizin dosyaları silin.
    * Arama hizmeti uygulaması yeniden oluşturun ve bu veritabanlarını yeniden oluşturur. GUI aracılığıyla tüm eylemleri gerçekleştirmek mümkün olmadığından, bu hizmet uygulaması yeniden oluşturur hazırlanmış bir komut dosyası olması önerilir. Örneğin, arama topolojisi dizin sürücü konumunu ayarlama ve yalnızca SharePoint PowerShell cmdlet'lerini kullanarak mümkündür. Geri yükleme SPEnterpriseSearchServiceApplication Windows PowerShell cmdlet'ini kullanın ve günlük aktarmalı ve çoğaltılmış arama yönetim veritabanı, Search_Service__DB belirtin. Bu cmdlet, arama yapılandırması, şema, yönetilen özellikleri, kuralları ve kaynaklar sağlar ve diğer bileşenleri varsayılan kümesi oluşturur.
    * Arama hizmeti uygulaması olan bir kez yeniden oluşturma, arama hizmeti geri yüklemek her içerik kaynağı için tam gezinme başlatmanız gerekir. Şirket içi gruptan arama önerileri gibi bazı analytics bilgiler kaybedilir.

7. Tüm adımları, kurtarma planı tamamlandıktan sonra son kurtarma planı aşağıdaki gibi görünür.

    ![Kaydedilen RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Yük devretme sınamasını yapmak
İzleyin [bu kılavuz](site-recovery-test-failover-to-azure.md) yük devretme sınamasını yapmak için.

1.  Azure Portalı'na gidin ve kurtarma hizmeti kasanızı seçin.
2.  SharePoint uygulaması için oluşturulmuş kurtarma planı tıklayın.
3.  'Test yük devretme üzerinde' tıklayın.
4.  Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5.  İkincil ortamı kurma olduğunda, doğrulama gerçekleştirebilirsiniz.
6.  Doğrulama tamamlandıktan sonra kurtarma planı üzerinde 'Temizleme test yük devretme' tıklayın ve yük devretme sınama ortamı temizlendi.

AD için yük devretme testi yapılması konusunda yönergeler için ve DNS, başvurmak için [Test yük devretme AD için ilgili önemli noktalar ve DNS](site-recovery-active-directory.md#test-failover-considerations) belge.

Yük devretme testi SQL Always ON kullanılabilirlik grupları yapılması ile ilgili yönergeler için bkz [yapılması Test yük devretme için SQL Server Always On](site-recovery-sql.md#steps-to-do-a-test-failover) belge.

## <a name="doing-a-failover"></a>Bir yük devretme işleminden
İzleyin [bu kılavuz](site-recovery-failover.md) bir yük devretme yapmak için.

1.  Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.
2.  SharePoint uygulaması için oluşturulmuş kurtarma planı tıklayın.
3.  'Failover' üzerinde'ı tıklatın.
4.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgiyi [diğer uygulamaları çoğaltma](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
