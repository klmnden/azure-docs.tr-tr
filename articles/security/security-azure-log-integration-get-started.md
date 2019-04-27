---
title: Azure günlük tümleştirmesini kullanmaya başlama | Microsoft Docs
description: Azure günlük tümleştirmesi hizmeti yüklemek ve günlükleri Azure depolama, Azure denetim günlükleri ve Azure Güvenlik Merkezi uyarılarını tümleştirme hakkında bilgi edinin.
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 244b2d1764f30f790c3e51e23cd2fa0af6375960
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60480290"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure tanılama günlüğünü ve Windows Olay iletme'yi Azure günlük tümleştirmesi


>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/01/2019 tarafından kullanımdan kaldırılacaktır. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirmesi, yalnızca kullanmalısınız bir [Azure İzleyici](../monitoring-and-diagnostics/monitoring-get-started.md) bağlayıcı güvenlik olayı ve Olay yönetimi (SIEM) satıcınızdan kullanılamaz.

Tüm varlıklarınız için birleşik güvenlik Pano oluşturabilmek azure günlük tümleştirmesi Azure günlüklerini sıem sistemlerinizden alınabileceği gibi için kullanılabilmesini sağlar.
Azure İzleyici bağlayıcısının durumu hakkında daha fazla bilgi için SIEM satıcınıza başvurun.

> [!IMPORTANT]
> Birincil ilgi, sanal makine günlüklerinin toplanması, çoğu SIEM satıcısını bu seçenek, çözüme ekleyin. SIEM Bağlayıcısı satıcısının her zaman tercih edilen alternatif kullanmaktır.

Bu makale Azure günlük tümleştirmesini kullanmaya başlamanıza yardımcı olur. Azure günlük tümleştirmesi hizmeti yükleniyor ve Azure Tanılama ile hizmet tümleştirme odaklanır. Azure günlük tümleştirmesi hizmeti bir Azure hizmet olarak altyapı'nda dağıtılan sanal makineler Windows güvenlik olayı kanaldan sonra Windows olay günlüğü bilgilerini toplar. Bu benzer *Olay iletme'yi* , bir şirket içi sistemde kullanabilir.

> [!NOTE]
> Azure günlük tümleştirmesi çıktısını bir SIEM ile tümleştirme, SIEM tarafından gerçekleştirilir. Daha fazla bilgi için [Azure günlük tümleştirmesi, şirket içi SIEM ile tümleştirme](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

Azure günlük tümleştirmesi hizmeti fiziksel veya Windows Server 2008 R2 çalıştıran bir sanal bilgisayar üzerinde çalışan veya üzeri (Windows Server 2016 veya Windows Server 2012 R2 tercih edilir).

Şirket içi fiziksel bir bilgisayarda çalıştırabilir veya bir barındırma sitesidir. Azure günlük tümleştirmesi hizmeti sanal bir makinede çalıştırmak isterseniz, şirket içi sanal makine olabilir veya bir genel bulut, Microsoft Azure gibi.

Fiziksel veya sanal makine Azure günlük tümleştirmesi hizmeti çalıştıran Azure genel bulutunda ağ bağlantısı gerektirir. Bu makalede, gerekli yapılandırma hakkında ayrıntılar sağlar.

## <a name="prerequisites"></a>Önkoşullar

En azından, Azure günlük Tümleştirmesi'ni yükleme aşağıdakileri gerektirir:

* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* A **depolama hesabı** Windows Azure tanılama (WAD) için kullanılabilir günlük kaydı. Önceden yapılandırılmış depolama hesabı kullanın veya yeni bir depolama hesabı oluşturun. Bu makalede, biz depolama hesabının nasıl yapılandırılacağı açıklanmaktadır.

  > [!NOTE]
  > Senaryonuza bağlı olarak, bir depolama hesabı gerekli olmayabilir. Bu makalede ele alınan Azure tanılama senaryo için bir depolama hesabı gereklidir.

* **İki sistem**: 
  * Azure günlük tümleştirmesi hizmeti çalıştıran bir makine. Bu makine daha sonra SIEM ile içeri aktarılan tüm günlük bilgilerini toplar. Bu sistemi:
    * Microsoft Azure'da barındırılan veya şirket içi olabilir.  
    * X x64 çalıştıran Windows Server 2008 R2 SP1 ya da sonraki bir sürümü ve Microsoft .NET 4.5.1 yüklü olması. Yüklü .NET sürümünü belirlemek için bkz: [hangi .NET Framework sürümlerinin yüklü olduğunu belirleme](https://msdn.microsoft.com/library/hh925568).  
    * Azure tanılama günlükleri için kullanılan Azure depolama hesabı bağlantı olması gerekir. Bu makalenin sonraki bölümlerinde biz bağlantısını onaylayın açıklanmaktadır.
  * İzlemek istediğiniz bir makine. Bu, bir VM olarak çalışan bir [Azure sanal makine](../virtual-machines/virtual-machines-windows-overview.md). Bu makinenin içinden bilgileri günlüğe kaydetme Azure günlük tümleştirmesi hizmeti makineye gönderilir.

Azure portalını kullanarak bir sanal makine oluşturmak nasıl hızlı bir örnek için aşağıdaki videoya göz atın:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Dağıtma konuları

Test sırasında en düşük işletim sistemi gereksinimlerini karşılayan herhangi bir sistem kullanabilirsiniz. Bir üretim ortamı için yük artırma veya genişletme için plan yapmanızı gerektirebilir.

Azure günlük tümleştirmesi hizmeti birden çok örneğini çalıştırabilirsiniz. Ancak, hizmet fiziksel veya sanal makine başına yalnızca bir örneğini çalıştırabilirsiniz. Ayrıca, Yük Dengeleme Azure tanılama depolama hesapları için WAD kullanabilirsiniz. Kapasite örnekleri sağlamak için abonelik sayısını temel alır.

> [!NOTE]
> Şu anda, biz ne zaman ölçek örneklerine (diğer bir deyişle, Azure günlük tümleştirmesi hizmeti çalıştıran makineler) Azure günlük tümleştirmesi makinelerin veya depolama hesapları veya abonelikleri için belirli önerimiz yok. Bu alanlar her, performansını gözlemler göre ölçeklendirme kararlar alın.

Performansı artırmak için de Azure günlük tümleştirmesi hizmetini ölçeklendirmek için seçeneğiniz vardır. Aşağıdaki performans ölçümleri, Azure günlük tümleştirmesi hizmeti çalıştırmak için seçtiğiniz makineler boyut yardımcı olabilir:

* 8 işlemci (çekirdek) makinede, Azure günlük tümleştirmesi tek bir örneğini (saat başına yaklaşık 1 milyon olay) günde yaklaşık 24 milyon olayları işleyebilir.
* 4 İşlemci (çekirdek) makine üzerinde tek bir Azure günlük tümleştirmesi örneği (yaklaşık 62,500 olayları saat başına) günde yaklaşık 1,5 milyon olayları işleyebilir.

## <a name="install-azure-log-integration"></a>Azure günlük Tümleştirmesi'ni yükleyin

Kurulum yordamı üzerinden çalıştırın. Telemetri bilgilerini Microsoft'a vermelisiniz isteyip istemediğinizi seçin.

Azure günlük tümleştirmesi hizmeti, yüklü olduğu makinede telemetri verileri toplar.  

Toplanan telemetri verilerini aşağıdakileri içerir:

* Azure günlük tümleştirmesi yürütülmesi sırasında oluşan özel durumlar.
* Sorgular ve işlenen olayların sayısı ile ilgili ölçümleri.
* İstatistikler hakkında hangi Azlog.exe komut satırı seçenekleri kullanılır. 

> [!NOTE]
> Telemetri verilerini toplamasına izin ver öneririz. Telemetri veri koleksiyonunu devre dışı temizleyerek kapatabilirsiniz **Microsoft'un telemetri verilerini toplamasına izin ver** onay kutusu.
>

![Telemetri onay kutusu seçili yükleme bölmesinin ekran görüntüsü](./media/security-azure-log-integration-get-started/telemetry.png)


Yükleme işlemi aşağıdaki videoda ele alınmıştır:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Yükleme sonrası ve doğrulama adımları

Temel kurulum tamamlandıktan sonra yükleme sonrası ve doğrulama adımlarını gerçekleştirmek için hazır:

1. PowerShell'i yönetici olarak açın. Ardından, C:\Program Files\Microsoft Azure günlük tümleştirmesi gidin.
2. Azure günlük tümleştirmesi cmdlet'leri içeri aktarın. Cmdlet'lerini içeri aktarmak için komut dosyasını çalıştırmak `LoadAzlogModule.ps1`. ENTER `.\LoadAzlogModule.ps1`, ve ardından Enter tuşuna basın (kullanımına dikkat edin **.\\**  Bu komutta). Aşağıdaki resimde görünen benzer bir şey görmeniz gerekir:

   ![LoadAzlogModule.ps1 komut çıktısının ekran görüntüsü](./media/security-azure-log-integration-get-started/loaded-modules.png)
3. Ardından, belirli bir Azure ortamı kullanmak için Azure günlük tümleştirmesi yapılandırın. Bir *Azure ortamı* birlikte çalışmak istediğiniz Azure bulut veri merkezine türüdür. Olmakla birlikte birkaç Azure ortamları şu anda, uygun seçenekleri olan **AzureCloud** veya **AzureUSGovernment**. PowerShell'i yönetici olarak çalışan C:\Program Files\Microsoft Azure günlük Integration\ olduğundan emin olun. Ardından, bu komutu çalıştırın:

   `Set-AzlogAzureEnvironment -Name AzureCloud` (for **AzureCloud**)
  
   ABD devlet kurumları Azure bulutuna kullanmak istiyorsanız, kullanın **AzureUSGovernment** için **-adı** değişkeni. Diğer Azure bulutlarını şu anda desteklenmiyor.  

   > [!NOTE]
   > Komut başarılı olduğunda, geri bildirim gönderilmez. 

4. Bir sistem izleyebilmeniz için önce Azure tanılama için kullanılan depolama hesabı adı gereklidir. Azure portalında Git **sanal makineler**. İzleyeceğiniz bir Windows sanal makine için bakın. İçinde **özellikleri** bölümünden **tanılama ayarları**.  Ardından, **aracı**. Belirtilen depolama hesabı adını not edin. Bu hesap adı, sonraki adım için ihtiyacınız.

   ![Azure tanılama ayarları bölmesinin ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-account-large.png) 

   ![Konuk düzeyinde izleme düğmesi, ekran etkinleştir](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

   > [!NOTE]
   > Sanal makine oluşturulduğunda, izleme etkin değildi, önceki görüntüde gösterildiği gibi etkinleştirebilirsiniz.

5. Şimdi, Azure günlük tümleştirmesi makineye geri dönün. Bağlantı depolama hesabına sistemden Azure günlük Tümleştirmesi'ni yüklü olduğunu doğrulayın. Azure günlük tümleştirmesi hizmetini çalıştıran bilgisayarın, Azure tanılama tarafından izlenen sistemlerinin her oturum bilgilerini almak için depolama hesabına erişmesi gerekir. Bağlantıyı doğrulamak için: 
   1. [Azure Depolama Gezgini'ni indir](https://storageexplorer.com/).
   2. Kurulumu tamamlayın.
   3. Yükleme tamamlandığında seçin **sonraki**. Bırakın **başlatma Microsoft Azure Depolama Gezgini** onay kutusu seçili.  
   4. Azure'da oturum açın.
   5. Azure tanılama için yapılandırılmış depolama hesabı görebildiğini doğrulayın: 

   ![Depolama Gezgini'nde depolama hesabı ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-explorer.png)

   1. Birkaç seçenek, depolama hesapları altında görünür. Altında **tabloları**, adlı bir tablo görürsünüz **WADWindowsEventLogsTable**.

   Sanal makine oluşturulduğunda, izleme etkin değildi, daha önce açıklandığı gibi etkinleştirebilirsiniz.


## <a name="integrate-windows-vm-logs"></a>Windows VM günlükleri tümleştirme

Bu adımda, günlük dosyaları içeren depolama hesabına bağlanmak için Azure günlük tümleştirmesi hizmeti çalıştıran makinenin yapılandırın.

Bu adımı tamamlamak için birkaç şey gerekir:  
* **FriendlyNameForSource**: Azure tanılama bilgilerini depolamak sanal makine için yapılandırmış olduğunuz depolama hesabı uygulayabileceğiniz bir kolay ad.
* **StorageAccountName**: Azure Tanılama'yı yapılandırırken belirtilen depolama hesabı adı.  
* **Depolama anahtarı**: Bu sanal makine için Azure tanılama bilgilerini depolandığı depolama hesabı için depolama anahtarı.  

Depolama anahtarını elde etmek için aşağıdaki adımları tamamlayın:
1. [Azure Portal](https://portal.azure.com) gidin.
2. Gezinti bölmesinde seçin **tüm hizmetleri**.
3. İçinde **filtre** kutusuna **depolama**. Ardından, **depolama hesapları**.

   ![Depolama hesapları tüm hizmetlerin gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/filter.png)

4. Depolama hesaplarının listesi görüntülenir. Depolama günlük atanmış hesaba çift tıklayın.

   ![Depolama hesaplarının bir listesini gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-accounts.png)

5. **Ayarlar** altında **Erişim anahtarları**'nı seçin.

   ![Erişim anahtarları seçeneği menüde gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)

6. Kopyalama **key1**ve ardından aşağıdaki adımı için erişebileceğiniz güvenli bir konuma kaydedin.
7. Azure günlük tümleştirmesi yüklediğiniz sunucuda, yönetici olarak bir komut istemi penceresi açın. (Bir Yöneticiyseniz ve PowerShell değil bir komut istemi penceresi açmak emin olun).
8. C:\Program Files\Microsoft Azure günlük tümleştirmesi için gidin.
9. Bu komutu çalıştırın: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
   Örnek:
  
   `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

   Abonelik kimliği, olay XML görünmesini istiyorsanız, abonelik kimliği için kolay ad ekleyin:

   `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
   Örnek:
  
   `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> 60 dakika kadar bekleyin ve ardından depolama hesabından çekilen olayları görüntüleyin. Azure günlük tümleştirmesi olayları görüntülemek için seçin **Olay Görüntüleyicisi'ni** > **Windows Günlükleri** > **iletilen olaylar**.

Aşağıdaki video, önceki adımları ele alınmıştır:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>İletilen olaylar klasöründe veri listemde görünmüyor
Veri iletilen olaylar klasöründe bir saat sonra listemde görünmüyor, bu adımları tamamlayın:

1. Azure günlük tümleştirmesi hizmeti çalıştıran makinenin kontrol edin. Azure erişebildiğinizi doğrulayın. Bağlantı, tarayıcıda test etmek için gitmek deneyin [Azure portalında](https://portal.azure.com).
2. Kullanıcı hesabı Azlog klasör users\Azlog için yazma izni olduğundan emin olun.
   1. Dosya Gezgini'ni açın.
   2. C:\Users gidin.
   3. C:\users\Azlog sağ tıklayın.
   4. Seçin **güvenlik**.
   5. Seçin **NT Service\Azlog**. Hesap izinlerini denetleyin. Hesap Bu sekmeden eksik ya da uygun izinleri görünmüyorsa, bu sekmedeki hesap izinleri verebilirsiniz.
3. Komutu çalıştırdığınızda `Azlog source list`, emin depolama hesabını komut eklendi `Azlog source add` çıktısında listelenir.
4. Azure günlük tümleştirmesi hizmeti herhangi bir hata bildirilirse görmek için Git **Olay Görüntüleyicisi'ni** > **Windows Günlükleri** > **uygulama**.

Yükleme ve yapılandırma sırasında herhangi bir sorunla karşılaşırsanız çalıştırırsanız, oluşturabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Hizmet için seçin **günlük tümleştirmesi**.

Başka bir destek seçeneği [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). MSDN forum sorularını yanıtlama ve ipuçları ve püf noktaları hakkında nasıl en iyi Azure günlük Tümleştirmesi'ı paylaşarak destek topluluk sağlayabilir. Azure günlük tümleştirmesi takım, bu Forumu da izler. Bunlar mümkün olduğunca yardımcı olurlar.

## <a name="integrate-azure-activity-logs"></a>Azure etkinlik günlüklerini tümleştirme

Azure etkinlik günlüğü oluşan Azure'da abonelik düzeyindeki olayların bir anlayış sağlayan bir abonelik günlüktür. Bu verileri, hizmet durumu olayları güncelleştirmelerinin Azure Resource Manager işletimsel verileri içerir. Azure Güvenlik Merkezi uyarıları, ayrıca bu günlüğüne dahil edilir.
> [!NOTE]
> Bu makaledeki adımlarda çalışmadan önce gözden geçirmeniz gerekir [başlama](security-azure-log-integration-get-started.md) makalesini inceleyin ve orada adımları tamamlayın.

### <a name="steps-to-integrate-azure-activity-logs"></a>Azure etkinlik günlüklerini tümleştirmek için adımları

1. Bir komut istemi açın ve şu komutu çalıştırın:  ```cd c:\Program Files\Microsoft Azure Log Integration```
2. Şu komutu çalıştırın:  ```azlog createazureid```

    Bu komut için Azure oturum açma bilgilerinizi ister. Komut daha sonra Azure Active Directory Hizmet sorumlusu oturum açma kullanıcı bir yönetici, ortak yönetici veya sahibi olduğu Azure abonelikleri barındıran Azure AD kiracılarıyla oluşturur. Yalnızca Konuk kullanıcı Azure AD kiracısında oturum açan kullanıcı olduğundan komut başarısız olur. Azure kimlik doğrulamasını Azure AD gerçekleştirilir. Azure günlük tümleştirmesi için hizmet sorumlusu oluşturma, Azure aboneliklerinden okumak üzere erişim verilen Azure AD kimliğini oluşturur.
3. Azure günlük tümleştirmesi, abonelik için Etkinlik günlüğünü okuma için önceki adımı erişim oluşturulan hizmet sorumlusuna yetkisi vermek için aşağıdaki komutu çalıştırın. Abonelik komutu çalıştırmak için bir sahip olmanız gerekir.

   ```Azlog.exe authorize subscriptionId``` Örnek:

   ```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```

4. Azure Active Directory denetim günlüğü JSON dosyalarını bunları oluşturulduğunu onaylamak için aşağıdaki klasörleri denetleyin:
   - C:\Users\azlog\AzureResourceManagerJson
   - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Güvenlik bilgileri ve Olay yönetimi (SIEM) sistemi JSON dosyalarındaki bilgileri getirme ayrıntılı yönergeler için SIEM satıcınıza başvurun.

Topluluk Yardımı, aracılığıyla [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Bu forum birbirine sorular ve yanıtlar, ipuçları ve püf noktaları ile desteklemek için Azure günlük tümleştirmesi topluluğundaki kişiler sağlar. Ayrıca, Azure günlük tümleştirmesi takım bu Forumu izler ve mümkün olduğunda yardımcı olur.

Ayrıca açabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Günlük tümleştirmesi, destek istemeden hizmet olarak seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure günlük tümleştirmesi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın: Bu makaledeki adımlarda denemeden önce Get başlatılan makalesini gözden geçirin ve orada adımları tamamlayın.

* [Azure günlük tümleştirmesine giriş](security-azure-log-integration-overview.md). Bu makalede Azure günlük tümleştirmesi, önemli işlevleri ve nasıl çalıştığını tanıtılmaktadır.
* [İş ortağı yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Bu blog gönderisini Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesi yapılandırma gösterilmektedir. Bu, SIEM bileşenleri yapılandırma hakkında geçerli kılavuzumuzu açıklar. Ek ayrıntılar için SIEM satıcınıza başvurun.
* [Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS)](security-azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan sorular yanıtlanmaktadır.
* [Azure günlük Tümleştirmesi ile Azure Güvenlik Merkezi uyarılarını tümleştirme](../security-center/security-center-integrating-alerts-with-log-integration.md). Bu makalede, Güvenlik Merkezi uyarıları ve Azure tanılama ve Azure etkinlik tarafından toplanan sanal makine güvenlik olaylarını eşitleme işlemini göstermektedir günlükleri. Günlükleri, Azure İzleyici günlüklerine veya SIEM çözümünden kullanarak eşitleyin.
* [Yeni özellikler için Azure tanılama ve Azure denetim günlükleri](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Bu blog gönderisinde size Azure denetim günlükleri tanıtır ve yardımcı olabilecek diğer özellikleri, Azure kaynaklarınızın işlemleri öngörü.
