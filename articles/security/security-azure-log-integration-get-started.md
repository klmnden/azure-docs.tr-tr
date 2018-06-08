---
title: Azure günlük tümleştirme ile çalışmaya başlama | Microsoft Docs
description: Azure günlük tümleştirme hizmeti yüklemek ve Azure Storage, Azure denetim günlükleri ve Azure Güvenlik Merkezi uyarılarını günlüklerinden tümleştirme hakkında bilgi edinin.
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 06/06/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: b8888823b1445dc084ae4c0323d90110c9d384a4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839454"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure tanılama günlüğünü ve Windows Olay iletme özelliğini Azure günlük tümleştirme

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 01/06/2019 tarafından kullanım dışı kalacaktır.  AzLog yüklemeleri 27 Haz 2018 tarafından devre dışı bırakılacak. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında yönergeler için [SIEM araçları ile tümleştirmek için kullanım Azure İzleyicisi](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/preview/?cdn=disable) 

Yalnızca Azure günlük tümleştirme kullanmalısınız bir [Azure İzleyici](../monitoring-and-diagnostics/monitoring-get-started.md) bağlayıcı güvenlik olay ve Olay yönetimi (SIEM) satıcınızdan kullanılamaz.

Tüm varlıklar için bir birleşik güvenlik Pano oluşturabilmesi için azure günlük tümleştirme Azure günlükleri, SIEM sisteminize kullanılabilmesini sağlar.
Bir Azure İzleyici Bağlayıcısı durumu hakkında daha fazla bilgi için SIEM satıcınıza başvurun.

> [!IMPORTANT]
> Birincil ilgi sanal makine günlüklerinin toplanması, çoğu SIEM satıcısını kendi çözümde bu seçenek içerir. SIEM satıcının bağlayıcı her zaman tercih edilen alternatif kullanmaktır.

Bu makalede Azure günlük tümleştirme ile çalışmaya başlamanıza yardımcı olur. Azure günlük tümleştirme hizmeti yükleme ve hizmet Azure Tanılama ile tümleştirme odaklanır. Azure günlük tümleştirme hizmeti bu durumda Azure altyapısının bir hizmet olarak dağıtılan sanal makineleri Windows güvenliği olay kanaldan Windows olay günlüğü bilgilerini toplar. Bu benzer *Olay iletme* bir şirket içi sistemde kullanabilecek.

> [!NOTE]
> Azure günlük tümleştirme çıktısını bir SIEM ile tümleştirme, SIEM tarafından gerçekleştirilir. Daha fazla bilgi için bkz: [, şirket içi SIEM ile Azure günlük tümleştirme tümleştirmek](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

Azure günlük tümleştirme hizmeti fiziksel veya Windows Server 2008 R2 çalıştıran sanal bir bilgisayar üzerinde çalışan veya sonraki bir sürümü (Windows Server 2016 veya Windows Server 2012 R2, tercih edilen).

Bir fiziksel bilgisayar şirket içi çalıştırabilir veya bir barındırma sitesidir. Bir sanal makinede Azure günlük tümleştirme hizmeti çalıştırmayı seçerseniz, şirket içi sanal makine olabilir veya bir genel bulut, Microsoft Azure gibi.

Fiziksel veya sanal makine Azure günlük tümleştirme hizmeti çalıştıran Azure genel bulut ağ bağlantısı gerektirir. Bu makalede gerekli yapılandırma hakkında ayrıntılar sağlar.

## <a name="prerequisites"></a>Önkoşullar

En azından, Azure günlük tümleştirme yükleme aşağıdaki öğeleri gerektirir:

* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* A **depolama hesabı** Windows Azure tanılama (WAD) için kullanılabilir günlük kaydı. Önceden yapılandırılmış depolama hesabı kullanabilir veya yeni bir depolama hesabı oluşturabilirsiniz. Bu makalenin sonraki bölümlerinde, depolama hesabının nasıl yapılandırılacağı açıklanmaktadır.

  > [!NOTE]
  > Senaryonuza bağlı olarak, bir depolama hesabı gerekli olmayabilir. Bu makalede ele alınan Azure tanılama senaryosu için bir depolama hesabı gereklidir.

* **İki sistem**: 
  * Azure günlük tümleştirme hizmeti çalıştıran bir makine. Bu makineyi daha sonra SIEM alınmış tüm günlük bilgilerini toplar. Bu sistem:
    * Microsoft Azure üzerinde barındırılan veya şirket içi olabilir.  
    * X x64 çalıştıran Windows Server 2008 R2 SP1 veya sonraki sürümü ve Microsoft .NET 4.5.1'in yüklü olması. Yüklü .NET sürümünü belirlemek için bkz: [hangi .NET Framework sürümlerinin yüklü olduğunu belirleme](https://msdn.microsoft.com/library/hh925568).  
    * Azure tanılama günlüğü için kullanılan Azure depolama hesabı bağlantısı olması gerekir. Bu makalenin sonraki bölümlerinde bağlanabilirliği doğrulamak nasıl açıklanmaktadır.
  * İzlemek istediğiniz makine. Bu olarak çalışan bir VM olan bir [Azure sanal makinesi](../virtual-machines/virtual-machines-windows-overview.md). Bu makineden günlük bilgilerini Azure günlük tümleştirme hizmeti makineye gönderilir.

Azure portalını kullanarak bir sanal makine oluşturmak nasıl hızlı tanıtımı için aşağıdaki videoda göz atın:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Dağıtma konuları

Test sırasında en düşük işletim sistemi gereksinimlerini karşılayan tüm sistemi kullanabilirsiniz. Bir üretim ortamı için yük ölçeklendirmeyi veya ölçeğini planlamak gerektirebilir.

Azure günlük tümleştirme hizmeti birden çok örneğini çalıştırabilirsiniz. Ancak, hizmet fiziksel veya sanal makine başına yalnızca bir örneği çalıştırabilirsiniz. Ayrıca, Yük Dengeleme Azure Diagnostics depolama hesapları için WAD kullanabilirsiniz. Kapasite örneklerine sağlamak için abonelik sayısını temel alır.

> [!NOTE]
> Şu anda, biz ne zaman örnekleri (diğer bir deyişle, Azure günlük tümleştirme hizmeti çalışan makineler) Azure günlük tümleştirme makinelerin veya ölçeklendirmek depolama hesapları ve abonelikleri hakkında belirli önerimiz yok. Bu alanların her biri de, performans gözlemleri göre ölçeklendirme kararlar.

Performansının artırılmasına yardımcı olmak için ayrıca Azure günlük tümleştirme hizmeti oluşturan ölçeklendirme seçeneğiniz vardır. Aşağıdaki performans ölçümleri Azure günlük tümleştirme hizmeti çalıştırmak için seçtiğiniz makineler size yardımcı olabilir:

* Bir 8 işlemci (Temel) makinede Azure günlük tümleştirme tek bir örneğini (saat başına yaklaşık 1 milyon olayları) günde yaklaşık 24 milyon olay işleyebilir.
* 4 İşlemci (Temel) makine üzerinde Azure günlük tümleştirme tek bir örneğini (yaklaşık olarak saatte 62,500 olayları) günde yaklaşık 1,5 milyon olayları işleyebilir.

## <a name="install-azure-log-integration"></a>Azure günlük tümleştirme yükleyin

Azure günlük tümleştirme yüklemek için Yükle [Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324) yükleme dosyası. Kurulum işlemi tamamlayın. Telemetri bilgilerini Microsoft'a vermelisiniz isteyip istemediğinizi seçin.

Azure günlük tümleştirme hizmeti yüklü olduğu makinede telemetri verileri toplar.  

Toplanan telemetri verileri aşağıdakileri içerir:

* Azure günlük tümleştirme yürütülmesi sırasında oluşan özel durumlar.
* Sorgular ve işlenen olayların sayısı hakkında ölçümleri.
* İstatistikler hakkında hangi Azlog.exe komut satırı seçenekleri kullanılır. 

> [!NOTE]
> Microsoft'un telemetri verilerini toplamasına izin öneririz. Temizleyerek telemetri verilerini toplamayı devre dışı bırakabilir **Microsoft'un telemetri verilerini toplamasına izin ver** onay kutusu.
>

![Telemetri onay kutusu seçili yükleme bölmesinin ekran görüntüsü](./media/security-azure-log-integration-get-started/telemetry.png)


Yükleme işlemi aşağıdaki videoda ele alınmıştır:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Yükleme sonrası ve doğrulama adımları

Temel kurulum tamamlandıktan sonra yükleme sonrası ve doğrulama adımları gerçekleştirmek hazırsınız:

1. PowerShell'i yönetici olarak açın. Ardından, C:\Program Files\Microsoft Azure günlük tümleştirme için gidin.
2. Azure günlük tümleştirme cmdlet'leri içeri aktarın. Cmdlet'lerini içeri aktarmak için komut dosyasını çalıştırmak `LoadAzlogModule.ps1`. ENTER `.\LoadAzlogModule.ps1`, ve ardından Enter tuşuna basın (kullanımına dikkat edin **.\\**  Bu komutta). Aşağıdaki şekilde görünen gibi bir şey görmeniz gerekir:

  ![LoadAzlogModule.ps1 komutunun çıkışını ekran görüntüsü](./media/security-azure-log-integration-get-started/loaded-modules.png)
3. Ardından, belirli bir Azure ortamı kullanmak üzere Azure günlük tümleştirme yapılandırın. Bir *Azure ortamı* birlikte çalışmak istediğiniz Azure bulut veri merkezine türüdür. Olmakla birlikte birkaç Azure ortamı, şu anda, uygun seçenekleri olan **AzureCloud** veya **AzureUSGovernment**. PowerShell'i yönetici olarak çalıştırma C:\Program Files\Microsoft Azure günlük Integration\ içinde olduğundan emin olun. Ardından, bu komutu çalıştırın:

  `Set-AzlogAzureEnvironment -Name AzureCloud` (için **AzureCloud**)
  
  US Government Azure bulut kullanmak istiyorsanız, kullanmak **AzureUSGovernment** için **-Name** değişkeni. Şu anda, diğer Azure bulut desteklenmez.  

  > [!NOTE]
  > Komut başarılı olduğunda, geri bildirim alırsınız yok. 

4. Bir sistem izleyebilmeniz için Azure tanılama kullanılan depolama hesabı adı gerekir. Azure portalında Git **sanal makineleri**. İzleyeceğiniz bir Windows sanal makine için bakın. İçinde **özellikleri** bölümünde, select **tanılama ayarlarını**.  Ardından, seçin **Aracısı**. Belirtilen depolama hesabı adını not edin. Bu hesap adı, sonraki adım için gerekir.

  ![Azure tanılama ayarları bölmesinin ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-account-large.png) 

  ![Konuk düzeyinde izleme düğmesi, ekran etkinleştir](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

  > [!NOTE]
  > Sanal makine oluşturulduğunda izleme değildi etkinleştirilirse, önceki görüntüde gösterildiği gibi etkinleştirebilirsiniz.

5. Şimdi, Azure günlük tümleştirme makineye geri dönün. Bağlantı için depolama hesabı sisteminden Azure günlük tümleştirme yüklendiği sahip olduğunuzu doğrulayın. Azure günlük tümleştirme hizmetini çalıştıran bilgisayarın her izlenen sistemleri tarafından Azure tanılama günlüğe bilgi almak için depolama hesabına erişim izni gerekiyor. Bağlantıyı doğrulamak için: 
  1. [Azure Storage Gezgini karşıdan](http://storageexplorer.com/).
  2. Kurulumu tamamlayın.
  3. Yükleme tamamlandığında seçin **sonraki**. Bırakın **başlatma Microsoft Azure Storage Gezgini** onay kutusu seçili.  
  4. Azure'da oturum açın.
  5. Azure tanılama için yapılandırılan depolama hesabı görebildiğini doğrulayın: 

   ![Depolama hesaplarının Depolama Gezgini ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-explorer.png)

  6. Bazı seçenekler depolama hesapları altında görünür. Altında **tabloları**, adlı bir tablo görmelisiniz **WADWindowsEventLogsTable**.

  Sanal makine oluşturulduğunda izleme değildi etkinleştirilirse, bu, daha önce açıklandığı şekilde etkinleştirebilirsiniz.


## <a name="integrate-windows-vm-logs"></a>Windows VM günlükleri tümleştirme

Bu adımda, günlük dosyalarını içeren depolama hesabına bağlanmak üzere Azure günlük tümleştirme hizmeti çalıştıran makine yapılandırın.

Bu adımı tamamlamak için birkaç ayar yapmanız gerekir:  
* **FriendlyNameForSource**: kolay bir ad, Azure tanılama bilgilerini depolamak sanal makine için yapılandırdığınız depolama hesabı için geçerli olabilir.
* **StorageAccountName**: Azure tanılama yapılandırdığınızda belirtilen depolama hesabının adı.  
* **Depolama anahtarı**: Bu sanal makine için Azure tanılama bilgilerinin depolandığı depolama hesabının depolama anahtarı.  

Depolama anahtarı edinmek için aşağıdaki adımları tamamlayın:
1. [Azure Portal](http://portal.azure.com) gidin.
2. Gezinti bölmesinde seçin **tüm hizmetleri**.
3. İçinde **filtre** kutusuna **depolama**. Ardından, seçin **depolama hesapları**.

  ![Tüm hizmetlerin depolama hesapları gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/filter.png)

4. Depolama hesapları listesi görüntülenir. Depolama oturum atanan hesabını çift tıklatın.

  ![Depolama hesaplarının listesini gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-accounts.png)

5. **Ayarlar** altında **Erişim anahtarları**'nı seçin.

  ![Erişim tuşları seçeneği menüde gösteren ekran görüntüsü](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)

6. Kopya **key1**ve için aşağıdaki adımı erişebileceğiniz güvenli bir konuma kaydedin.
7. Azure günlük tümleştirme yüklü olduğu sunucuda yönetici olarak bir komut istemi penceresi açın. (Bir Yöneticiyseniz ve değil PowerShell bir komut istemi penceresi açmak emin olun).
8. C:\Program Files\Microsoft Azure günlük tümleştirme gidin.
9. Bu komutu çalıştırın: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
  Örnek:
  
  `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

  Abonelik kimliği olay XML görünmesini istiyorsanız, abonelik kimliği için kolay ad ekleyin:

  `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
  Örnek:
  
  `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> 60 dakika kadar bekleyin ve ardından depolama hesabından çekilen olayları görüntüleyin. Azure günlük tümleştirme olaylarını görüntülemek için seçin **Olay Görüntüleyicisi'ni** > **Windows Günlükleri** > **iletilen olaylar**.

Aşağıdaki videoda, yukarıdaki adımları kapsar:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Veri iletilen olaylar klasöründe görünmüyorsa
Veri iletilen olaylar klasöründe bir saat sonra görünmüyorsa, şu adımları tamamlayın:

1. Azure günlük tümleştirme hizmeti çalıştıran makine denetleyin. Azure erişebildiğinizi doğrulayın. Gitmek bir tarayıcı bağlantısı, test etmek için deneyin [Azure portal](http://portal.azure.com).
2. Kullanıcı hesabı Azlog klasörü users\Azlog yazma izni olduğundan emin olun.
  1. Dosya Gezgini'ni açın.
  2. C:\Users gidin.
  3. C:\users\Azlog sağ tıklayın.
  4. Seçin **güvenlik**.
  5. Seçin **NT Service\Azlog**. Hesap izinlerini denetleyin. Bu sekmeden hesabı eksik ya da uygun izinleri görünmüyorsa, bu sekmede hesap izinleri verebilirsiniz.
3. Komutu çalıştırdığınızda `Azlog source list`, emin olun depolama hesabını komutta eklendi `Azlog source add` çıktıda listelenir.
4. Azure günlük tümleştirme hizmetinden rapor herhangi bir hata olmadığını görmek için Git **Olay Görüntüleyicisi'ni** > **Windows Günlükleri** > **uygulama**.

Yükleme ve yapılandırma sırasında herhangi bir sorunla çalıştırırsanız, oluşturabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Hizmeti için seçin **günlük tümleştirme**.

Başka bir destek seçenek [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). MSDN Forumu'nda topluluk sorulara yanıt verilmesi ve ipuçları ve püf noktaları en iyi Azure günlük tümleştirme alma hakkında paylaşarak destek sağlar. Azure günlük tümleştirme takım de Bu forumda izler. Bunlar için her yardımcı olurlar.

## <a name="integrate-azure-activity-logs"></a>Azure etkinlik günlükleri tümleştirme

Azure etkinlik günlüğü, Azure'da oluşan abonelik düzeyinde olaylar hakkında bilgi sağlayan bir abonelik günlüktür. Bu verileri, Azure Resource Manager işlemsel veri hizmeti sistem durumu olayları güncelleştirmeleri için bir aralığı içerir. Azure Güvenlik Merkezi uyarılarını de bu günlüğüne dahil edilir.
> [!NOTE]
> Bu makaledeki adımları denemeden önce gözden geçirmeniz gerekir [başlama](security-azure-log-integration-get-started.md) makalesini inceleyip var. adımlarını tamamlayın.

### <a name="steps-to-integrate-azure-activity-logs"></a>Azure etkinlik günlükleri tümleştirmek için adımları

1. Komut istemi açın ve şu komutu çalıştırın:  ```cd c:\Program Files\Microsoft Azure Log Integration```
2. Bu komutu çalıştırın:  ```azlog createazureid```

    Bu komut için Azure oturum açma bilgilerinizi ister. Komutu daha sonra bir Azure Active Directory oturum açma kullanıcı bir yönetici, bir ortak yönetici veya sahibi olduğu Azure abonelikleri barındıran Azure AD kiracılar hizmet sorumlusu oluşturur. Oturum açma kullanıcı yalnızca Konuk kullanıcı olarak Azure AD kiracısı ise komut başarısız olur. Azure kimlik doğrulaması Azure AD üzerinden yapılır. Azure günlük tümleştirmesi için bir hizmet sorumlusu oluşturma Azure aboneliklerinden okuma erişimi verilir Azure AD kimlik oluşturur.
3.  Abonelik için okuma etkinlik günlüğü önceki adımı erişimin oluşturulan Azure günlük tümleştirme hizmet sorumlusu yetkilendirmek için aşağıdaki komutu çalıştırın. Komutu çalıştırmak için aboneliğe sahip olmanız gerekir.

    ```Azlog.exe authorize subscriptionId``` Örnek:

```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```
4.  Azure Active Directory denetim günlüğü JSON dosyalarını bunları oluşturulduğunu doğrulamak için aşağıdaki klasörler denetleyin:
    - C:\Users\azlog\AzureResourceManagerJson
    - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Güvenlik bilgileri ve Olay yönetimi (SIEM) sistem bilgileri JSON dosyalarında getiren ayrıntılı yönergeler için SIEM satıcınıza başvurun.

Topluluk Yardım ile de kullanılabilir [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Bu forumda birbirine soruları yanıtlar, ipuçları ve püf noktaları ile desteklemek için Azure günlük tümleştirme topluluk kişilere sağlar. Ayrıca, Azure günlük tümleştirme takım Bu forumda izler ve mümkün olduğunca yardımcı olur.

Ayrıca açabilirsiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Günlük tümleştirme destek isteğinde hizmet olarak seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure günlük tümleştirmesi hakkında daha fazla bilgi için aşağıdaki makalelere bakın: Bu makaledeki adımları denemeden önce Get başlatılan makalesini gözden geçirin ve var. adımları gerekir.

* [Azure günlükleri için Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324). İndirme Merkezi'nden ayrıntıları, sistem gereksinimleri ve yükleme yönergeleri için Azure günlük tümleştirmeyi içerir.
* [Azure günlük tümleştirme giriş](security-azure-log-integration-overview.md). Bu makalede Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı tanıtılır.
* [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir. Bu, SIEM bileşenlerini yapılandırma hakkında geçerli kılavuzumuzu açıklar. Daha fazla ayrıntı için SIEM satıcınıza başvurun.
* [Azure günlük tümleştirme sık sorulan sorular (SSS)](security-azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan soruları yanıtlar.
* [Azure Güvenlik Merkezi uyarılarını Azure günlük tümleştirme ile tümleştirme](../security-center/security-center-integrating-alerts-with-log-integration.md). Bu makalede, Güvenlik Merkezi uyarılarını ve Azure tanılama ve Azure etkinlik tarafından toplanan sanal makine güvenlik olaylarını eşitleme gösterilmektedir günlükleri. Azure günlük analizi veya SIEM çözümünüzü kullanarak günlükleri eşitleyin.
* [Azure tanılama ve Azure için yeni özellikler denetim günlüklerini](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Bu blog gönderisine Azure denetim günlükleri tanıtır ve yardımcı olabilecek diğer özellikleri, ayrıca Azure kaynaklarınızın operations kavramanıza.
