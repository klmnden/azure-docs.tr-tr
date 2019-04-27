---
title: Azure ve Azure MySQL Contoso Linux hizmet Masası uygulaması yeniden barındırma | Microsoft Docs
description: Nasıl geçiş yaparak Contoso şirket içi Linux uygulama rehosts öğrenmek için Azure Vm'lerinizden ve Azure MySQL.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 114f4ccccaa861928263eb59b4e43379989abcca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60670504"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Contoso geçişi: Şirket içi bir Linux uygulamasını Azure VM’lerinde ve Azure MySQL’de yeniden barındırın

Contoso geçiş yaparak kendi şirket içi iki katmanlı Linux hizmet Masası uygulaması (osTicket) nasıl rehosts Bu makale, Azure ve Azure MySQL.

Bu belge, şirket içi kaynaklara Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir 
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Kullanılabilir
Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Bu makalede
[Makale 9: Azure Web Apps ve Azure SQL veritabanında bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulamayı yeniden düzenleme](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir 
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir


Bu makalede, Contoso bir iki katmanlı Linux Apache MySQL PHP (LAMP) servis Masası Uygulaması'nı (osTicket) Azure'a geçirir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyüyor ve sonuç olarak şirket içi sistemler ve altyapı Basıncı yoktur.
- **Risk sınırlamak**: Hizmet Masası uygulaması iş açısından önemlidir. Contoso Azure'a sıfır riskle taşımak istiyor.
- **Genişletme**:  Contoso uygulaması şu anda değiştirmek istememektedir. Yalnızca uygulama tutarlı tutmak istiyor.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı en iyi geçiş yöntemini belirlemek üzere sabitlenmiş:

- Bugün, şirket içi VMware ortamlarında olduğu gibi geçişten sonra uygulamanızı Azure'a aynı performans özelliklerine sahip olmalıdır.  Uygulama, şirket içi olarak bulutta kritik olarak kalır. 
- Contoso, bu uygulamada yatırım yapmaya istememektedir.  İş için önemlidir, ancak mevcut haliyle Contoso basitçe istediğiniz buluta güvenli bir şekilde taşımak.
- Windows uygulama geçişleri birkaç tamamlandıktan sonra Azure'da bir Linux tabanlı altyapı kullanmayı öğrenmek Contoso istiyor.
- Uygulamayı buluta taşındıktan sonra veritabanı yönetim görevleri en aza indirmek contoso istiyor.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Uygulama, iki VM arasında (OSTICKETWEB ve OSTICKETMYSQL) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Azure Iaas VM'LERİNİ OSTICKETWEB web katmanı uygulamasında geçirilecektir.
- Uygulama veritabanı için MySQL PaaS hizmeti için Azure veritabanı geçişi.
- Contoso, üretim iş yükü geçişi olduğundan, kaynaklar üretim kaynak grubunda yer alacağı **ContosoRG**.
- Kaynaklar (Doğu ABD 2) birincil bölgeye çoğaltılır ve üretim ağı (VNET-PROD-EUS2) yerleştirilir:
    - VM web ön uç (FE-PROD-EUS2) alt ağda yer alacaktır.
    - Veritabanı örneği (PROD-DB-EUS2) veritabanı alt ağda yer alacaktır.
- Uygulama veritabanı, MySQL araçlarını kullanarak Azure MySQL geçirilecektir.
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


![Senaryo mimarisi](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>Geçiş işlemi

Contoso geçiş işlemi aşağıdaki gibi tamamlayın:

VM web geçirmek için:

1. İlk adım, Contoso Azure'ı ayarlar ve şirket içi Site Recovery dağıtmak için gerekli altyapı.
2. Sonra Azure ve şirket içi bileşenleri hazırlama, Contoso kurar ve web VM için çoğaltmayı etkinleştirir.
3. Contoso yukarı süreli çoğaltma olduktan sonra VM Azure'a devrederek tarafından geçirir.

Veritabanını geçirmek için:

1. Contoso, azure'da bir MySQL örneği sağlar.
2. Contoso, MySQL workbench ' ayarlar ve yerel olarak veritabanını yedekler.
3. Contoso daha sonra geri veritabanının yerel yedek kopyadan Azure'a.

![Geçiş işlemi](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma için Azure Vm'leri yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/) | Veritabanı açık kaynak MySQL Server altyapısını temel alır. Bir tam olarak yönetilen, Kurumsal kullanıma hazır, topluluk tabanlı, uygulama geliştirmeye ve dağıtıma yönelik bir hizmet olarak MySQL veritabanı sağlar. 

 
## <a name="prerequisites"></a>Önkoşullar

Bu senaryoda Contoso gerekenler aşağıda verilmiştir.

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso abonelikleri daha önceki bir makalede sırasında oluşturuldu. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | Contoso bölümünde anlatıldığı gibi Azure altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayarı<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan VM'ler.
**Şirket içi Vm'leri** | [Linux VM gereksinimlerini gözden geçirme](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) Site Recovery ile geçiş için desteklenir.<br/><br/> Doğrulama desteklenen [Linux dosya ve depolama sistemlerini](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Contoso yöneticileri geçişi nasıl tamamlanacağını garanti aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Bunlar, çoğaltılan verileri tutmak ve bir kurtarma Hizmetleri kasası oluşturmak için bir Azure depolama hesabı oluşturun.
> * **2. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: Bunlar için VM bulma ve aracı yükleme hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerine bağlanmak için hazırlık yapma.
>   * **3. adım: Veritabanını sağlamak]**: Azure'da, bunlar Azure MySQL veritabanı örneği sağlayın.
> * **4. adım: Vm'lerini çoğaltma**: Bunlar Site Recovery kaynak ve hedef ortamını yapılandırma, bir çoğaltma ilkesi ayarlayın ve VM'ler için Azure depolama çoğaltmaya başlayın.
> * **5. adım: Veritabanı geçişi**: Bunlar, geçiş MySQL araçları ile ayarlayın.
> * **6. adım: Site Recovery ile Vm'leri geçirme**: Son olarak, bunlar her şeyin çalıştığından emin olmak için yük devretme testi çalıştırma ve ardından sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırın.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. Adım: Azure Site Recovery hizmeti için hazırlama

Contoso, Site Recovery için birkaç Azure bileşenlerini gerekir:

- Bir Vnet'te yük devretti kaynakları bulunur. Contoso sırasında sanal ağ oluşturmuş [Azure altyapı dağıtımı](contoso-migration-infrastructure.md)
- Çoğaltılan verileri tutmak için yeni bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.

Contoso yöneticileri, bir depolama hesabı oluşturun ve şu şekilde Kasası:

1. Bunlar depolama hesabı oluşturma (**contosovmsacc20180528**) Doğu ABD 2 bölgesinde.

   - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
   - Bunlar, standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabı kullanın.

     ![Site Kurtarma Depolama](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Ağ ve depolama hesabı ile yerinde, bunlar (ContosoMigrationVault) bir kasa oluşturun ve içine yerleştirin **ContosoFailoverRG** birincil Doğu ABD 2 bölgesinde bir kaynak grubu.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. Adım: Site Recovery için şirket içi Vmware'leri hazırlama

Contoso admins gibi şirket içi VMware altyapınızı hazırlayın:

- Bunlar, VM bulmayı otomatikleştirmek için vCenter sunucusundaki bir hesap oluşturun.
- Çoğaltılacak VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap oluştururlar.
- Bunlar, geçiş sonrasında oluşturulduğunda Azure Vm'lerinin bağlanabilmesi için şirket içi Vm'leri hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri de çalıştırabilirsiniz bir hesabınız olmalıdır.

Contoso admins hesabı aşağıdaki gibi ayarlayın:

1. Bunlar, bir rolü vCenter düzeyinde oluşturun.
2. Bunlar, ardından bu rol gerekli izinleri atamanız.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Contoso geçmek istediği her sanal makinede Mobility hizmetinin yüklenmesi gerekir.

- VM'ler için çoğaltmayı etkinleştirdiğinizde site Recovery bu bileşeni otomatik gönderim yüklemesi yapabilirsiniz.
- Otomatik yükleme için. Site Recovery, sanal Makineye erişmek için gerekli izinlere sahip bir hesap gerekir. 
- Hesap ayrıntıları çoğaltma kurulumu sırasında giriş. 
- Yükleme izinleri olduğu sürece hesap etki alanı veya yerel hesap olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında Azure sanal makinelere bağlanabilmesi Contoso istiyor. Bunu yapmak için Contoso yöneticilerinin şunları yapmanız gerekir:

- İnternet üzerinden erişmek için geçiş işleminden önce şirket içi Linux VM üzerinde SSH bunlar etkinleştirin.  Ubuntu için bu aşağıdaki komutu kullanarak tamamlayabilirsiniz: **Sudo yüklemeyi apt-get ssh -y**.
- Yük devretme sonrasında, onlar iade **önyükleme tanılaması** VM'nin bir ekran görüntülemek için.
- Bu işe yaramazsa, VM çalıştıran ve bunlar gözden olduğunu doğrulamak ihtiyaç duydukları [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesabı oluşturuluyor.


## <a name="step-3-provision-azure-database-for-mysql"></a>3. Adım: MySQL için Azure veritabanı sağlama

Contoso yöneticileri birincil Doğu ABD 2 bölgesinde bir MySQL veritabanı örneğinde sağlayın.

1. Azure portalında kaynak MySQL için Azure veritabanı oluştururlar. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Adı ekledikleri **contosoosticket** için Azure veritabanı. Bunlar üretim kaynak grubuna veritabanı ekleyin **ContosoRG**ve kimlik bilgilerini belirtin.
3. Şirket içi MySQL veritabanı sürümü, 5.7 olduğundan, uyumluluk için bu sürümü seçin. Bunlar, kendi veritabanı gereksinimlerine göre varsayılan değerleri kullanır.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. İçin **fazladan yedek seçenekleri**, kullanmayı seçmeleri **coğrafi olarak yedekli**. Bu seçenek bir kesinti oluşursa, kendi ikincil Orta ABD bölgesinde veritabanını geri yükleme sağlar. Veritabanı'na sağladığınızda, bu seçenek yalnızca yapılandırabilirsiniz.

     ![Yedeklilik](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. İçinde **VNET PROD EUS2** Ağ > **hizmet uç noktalarını**, bunların SQL Hizmeti için hizmet uç noktası (veritabanı alt ağ) ekleyin.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Alt ağ ekledikten sonra bunlar bir üretim ağında veritabanı alt ağından erişime izin veren bir sanal ağ kuralı oluşturur.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>4. Adım: Şirket içi sanal makinelerini çoğaltma

Azure'a web VM geçişi yapmadan önce Contoso yöneticileri ayarlama ve çoğaltmayı etkinleştirin.

### <a name="set-a-protection-goal"></a>Koruma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi ayarlayın (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar, makinelerini şirket içi olduğunu, VMware Vm'leri ve Azure'a çoğaltmak istediğiniz olduğunuzu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız onaylayın **Evet, yaptım**. Contoso tek bir VM Bu senaryoda yalnızca geçiş olan ve dağıtım planlama gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso yöneticileri artık kaynak ortamı yapılandırın. Bunu yapmak için Site Recovery yapılandırma sunucusunun yüksek oranda kullanılabilir, olarak dağıttıkları bir OVF şablonunu kullanarak şirket içi VMware VM. Yapılandırma sunucusunu çalışır duruma geldikten sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu, çeşitli bileşenler çalıştırır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu bileşeni.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso yöneticileri bunu şu şekilde yapabilirsiniz:


1. Bunlar OVF şablonu indirin **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Bunlar, şablonu VM oluşturmak için Vmware'e aktarın ve VM'yi dağıtın.

    ![OVF şablonu](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
4. Yükleme tamamlandıktan sonra VM için yönetici olarak oturum açın. İlk oturum açma işleminde, varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı'nda, yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler.
7. Bağlantı kurulduktan sonra bunlar Azure aboneliği için oturum açın. Kimlik bilgileri, yapılandırma sunucusu kaydedeceksiniz kasa erişiminiz olması gerekir.

    ![Yapılandırma sunucusunu kaydetmek](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
9. Bunların makinede yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
10. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
11. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek istediğiniz kasaya seçerler.

    ![Kasa](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Şimdi indirin ve MySQL Server ve VMWare powerclı'yı yükleyin. 
13. Doğrulama sonrasında, bunlar vCenter sunucusunda veya vSphere konağının FQDN'sini veya IP adresini belirtin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve vCenter sunucusu için bir kolay ad belirtin.
14. Bunlar, bunlar otomatik bulma için oluşturduğunuz hesabı ve Site Recovery Mobility hizmetini otomatik olarak yüklemek için kullanacağı kimlik bilgilerini girin. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Azure portalında kayıt tamamlandıktan sonra bunlar üzerinde yapılandırma sunucusunun ve VMware sunucusunun listelenip listelenmediğini denetleyin **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
15. Her yerde, Site Recovery için VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso yöneticileri hedef çoğaltma ayarlarını girin.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef ağda olup olmadığını denetler.


### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlanan Contoso yöneticileri bir çoğaltma ilkesi oluşturmak hazır olursunuz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: Varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.

### <a name="enable-replication-for-the-web-vm"></a>Web VM için çoğaltmayı etkinleştirme

Contoso yöneticileri çoğaltmaya başlamak şimdi **OSTICKETWEB** VM.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, sanal makineleri sağlar ve vCenter sunucusu ve yapılandırma sunucusu da dahil olmak üzere kaynak ayarlarını istediğiniz gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Artık hedef ayarlarını belirtin. Bunlar, kaynak grubunu ve ağ, Azure VM yük devretme işleminden sonra yerleştirilir ve çoğaltılan verilerin depolanacağı depolama hesabı içerir. 

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Seçmeleri **OSTICKETWEB** çoğaltma. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. VM Özellikleri'nde, bunlar otomatik olarak Mobility hizmeti VM üzerinde yüklemek için kullanılacak hesabı seçin.

     ![Mobility hizmeti](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. içinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırma**, bunların doğru çoğaltma ilkesinin uygulanan ve seçim olup olmadığını denetleyin **çoğaltmayı etkinleştirme**. Mobility hizmetinin otomatik olarak yüklenir.
6.  Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


**Daha fazla yardıma mı ihtiyacınız var?**

Bu adımlar tam bir kılavuza edinebilirsiniz [çoğaltmayı etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>5. Adım: Veritabanını geçirme

Contoso yöneticileri veritabanını yedekleme ve geri yükleme ile MySQL araçlarını kullanarak geçirin. Bunlar MySQL Workbench'i yükleyin, OSTICKETMYSQL veritabanını yedekleyin ve sonra MySQL sunucusu için Azure veritabanı'na geri.

### <a name="install-mysql-workbench"></a>MySQL Workbench’i yükleme

1. Onlar iade [önkoşulları ve indirmeler MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. MySQL Workbench ile uyumlu olarak Windows için yükledikleri [yükleme yönergeleri](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. MySQL Workbench içinde bir MySQL bağlantı OSTICKETMYSQL oluştururlar. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Bunlar veritabanı olarak dışarı aktarma **osticket**, yerel kendi içinde bir dosya için.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Veritabanı yerel olarak yedeklendiğinden sonra bunlar MySQL örneği için Azure veritabanına bağlantı oluşturun.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Şimdi, (Kurtarma) Azure MySQL örneği, kendi içinde veritabanında dosyasını içeri aktarabilirsiniz. Yeni bir şema (osticket) için örneği oluşturulur.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>6. Adım: Site Recovery ile Vm'leri geçirme

Son olarak, Contoso yöneticileri hızlı çalıştırın, yük devretme testi ve sonra VM'yi geçirme.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırma yardımcı her şeyi geçiş işleminden önce beklendiği gibi çalıştığını doğrulayın. 

1. Yük devretme testi için son noktası sürede çalıştırdığı (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 

    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Bunlar, VM doğru ağa bağlandığından uygun boyutta olduğundan ve çalıştığından emin denetleyin. 
4. Doğruladıktan sonra bunlar devretmeyi temizlemek ve gözlemlerinizi kaydetmek ve.

### <a name="migrate-the-vm"></a>VM'yi geçirme

Sanal Makineyi geçirmek için Contoso yöneticileri VM içeren bir kurtarma planı oluşturur ve planı azure'a yük devretme.

1. Bir plan oluşturur ve ekleme **OSTICKETWEB** ona.

    ![Kurtarma planı](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Bunlar, planı üzerinde bir yük devretme çalıştırın. Bunlar en son kurtarma noktası seçin ve Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi denemesi gerektiğini belirtin. Yöneticiler yük devretme işleminin ilerleyişini izleyebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Yük devretme sırasında vCenter Server ESXi ana bilgisayarında çalışan iki VM durdurmak için komutlar verir.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Yük devretmeden sonra bunlar Azure VM Azure Portalı'nda beklendiği gibi göründüğünü doğrulayın.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. VM denetledikten sonra geçişi tamamlayın. Bu VM için çoğaltma durdurulur ve sanal makine için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a devretmek.


### <a name="connect-the-vm-to-the-database"></a>VM veritabanına bağlanma

Son adım olarak geçiş sürecinin Contoso yöneticileri MySQL için Azure veritabanı'na işaret edecek şekilde bağlantı dizesini güncelleştirin. 

1. Putty kullanarak sanal Makineye OSTICKETWEB yaptıkları bir SSH bağlantısı veya başka bir SSH istemcisi. Özel IP adresini kullanarak bağlanmak için özel bir vm'dir.

    ![Veritabanı'na bağlanma](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Veritabanı'na bağlanma](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Bunlar ayarlarını güncelleştirmek için **OSTICKETWEB** VM ile iletişim kurabilir **OSTICKETMYSQL** veritabanı. Şu anda şirket içi IP adresiyle 172.16.0.43 sabit kodlanmış bir yapılandırmadır.

    **Güncelleştirmeden önce**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Güncelleştirme sonrasında**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Hizmetle yeniden başlatıldığında **systemctl yeniden apache2**.

    ![Yeniden Başlatma](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Son olarak, bunlar için DNS kayıtlarını güncelleştirmek **OSTICKETWEB**, Contoso etki alanı denetleyicilerinin birindeki.

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile osTicket uygulama katmanları Azure Vm'leri üzerinde çalışıyor.

Şimdi, Contoso şunları yapmanız gerekir: 
- VCenter stok VMware sanal makineleri Kaldır
- Şirket içi Vm'leri yerel yedekleme işlerden kaldırın.
- Güncelleştirme iç belgeleri, yeni konumlar ve IP adreslerini gösterir. 
- Şirket içi sanal makineleri ile etkileşim kuran tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- Contoso değerlendirmek için bağımlılık eşlemesi ile Azure geçişi hizmeti kullanılan **OSTICKETWEB** VM geçiş için. Artık bu amaçla VM'den yüklü aracıları (Microsoft İzleme Aracısı/bağımlılık Aracısı) kaldırmanız gerekir.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Artık çalışan bir uygulamayla Contoso tam olarak çalışır hale getirme ve kendi yeni altyapısının güvenliğini sağlamak için gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi VM ve veritabanını güvenlik sorunları belirlemek için gözden geçirin.

- Bunlar, erişimi denetlemek için sanal makine için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler, yalnızca uygulamaya izin trafik geçirebilirsiniz emin olmak için kullanılır.
- Bunlar, Disk şifreleme ve Azure anahtar Kasası'nı kullanarak VM disk üzerindeki veri güvenliğini sağlamak için göz önünde bulundurun.
- VM ve veritabanı örneği arasındaki iletişim için SSL yapılandırılmamış. Veritabanı trafiği ele emin olmak için bunu yapmanız gerekecektir.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) VM'ler için önerilen güvenlik uygulamaları hakkında.

### <a name="bcdr"></a>BCDR

Contoso, iş sürekliliği ve olağanüstü durum kurtarma için aşağıdaki işlemleri yapar:

- **Verileri güvende tutun**: Contoso uygulaması Azure Backup hizmetini kullanarak VM verileri yedekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Veritabanı için yedeklemeyi yapılandırma gerekmez. MySQL için Azure veritabanı, otomatik olarak oluşturur ve sunucu yedeklemelerini depolar. Bunlar, dayanıklı ve üretime hazır, bu nedenle veritabanı için coğrafi yedeklilik kullanmayı seçtiniz.
- **Uygulamalarınızı çalışır halde tutun**: Contoso uygulama Azure sanal makinelerini Site Recovery kullanarak ikincil bir bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).


### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Kaynakları dağıttıktan sonra Contoso sırasında bunlar alınan kararları uygun olarak Azure etiketler atar [Azure altyapı](contoso-migration-infrastructure.md#set-up-tagging) dağıtım.
- Contoso Ubuntu sunucular için hiçbir lisans sorunları vardır.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.


## <a name="next-steps"></a>Sonraki adımlar

Bu senaryoda, biz son rehost senaryosu gösterilmiştir. Contoso, VM şirket içi Linux osTicket uygulamanın ön uç bir Azure VM geçişi ve uygulama veritabanı Azure MySQL örneğine geçişi.

Contoso içeren basit lift-and-shift ile taşıma geçişleri yerine uygulama, yeniden düzenleme, geçişleri daha karmaşık bir dizi nasıl gerçekleştirileceğini göstermek için öğretici geçiş serideki sonraki kümesinde ekleyeceğiz.
