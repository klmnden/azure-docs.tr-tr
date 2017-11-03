---
title: "Azure günlük tümleştirme ile çalışmaya başlama | Microsoft Docs"
description: "Azure günlük tümleştirme hizmeti yüklemek ve Azure depolama, Azure denetim günlükleri ve Azure Güvenlik Merkezi uyarılarını günlüklerinden tümleştirme hakkında bilgi edinin."
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
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9d39ecd513386b75b4b640721f80991caaf9ade8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure tanılama günlüğü ve Windows Olay iletme Azure günlük tümleştirme
Azure günlük tümleştirme (AzLog), Azure kaynaklarınızı ham günlükleri, şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlere tümleştirmenize olanak sağlar. Bu tümleştirme Birleşik güvenlik Panosu tüm varlıklarınızı, şirket içi veya bulutta, toplama, bağıntılı çözümlemek, ve güvenlik olayları için uyarı uygulamalarınız ile ilişkili olması mümkündür.
>[!NOTE]
Azure günlük tümleştirme hakkında daha fazla bilgi için gözden geçirebilirsiniz [Azure günlük tümleştirmesine genel bakış](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Bu makalede Azure günlük tümleşme Azlog service yüklemesinde odaklanma ve hizmeti ile Azure tanılama tümleştirme başlamanıza yardımcı olur. Azure günlük tümleştirme hizmeti sonra Azure Iaas dağıtılan sanal makineleri Windows Güvenlik olay kanaldan Windows olay günlüğü bilgilerini toplamak mümkün olur. Bu, "Olay şirket içi kullanmış olabilirsiniz iletme için" çok benzer.

>[!NOTE]
>Azure günlük tümleştirme çıktısını SIEM Getir olanağı SIEM tarafından sağlanır. Lütfen makalesine bakın [, şirket içi SIEM ile Azure günlük tümleştirme tümleştirme](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) daha fazla bilgi için.

Azure günlük tümleştirme hizmeti açıkça - olması için Windows Server 2008 R2 kullanarak bir fiziksel veya sanal bilgisayar veya üstü işletim sistemi çalıştıran (Windows Server 2012 R2 veya Windows Server 2016 tercih edilen).

Fiziksel bilgisayar şirket içi çalıştırabilirsiniz (veya bir barındırıcı sitesinde). Bir sanal makinede Azure günlük tümleştirme hizmeti çalıştırmayı seçerseniz, bu sanal makine şirket içi olabilir veya Microsoft Azure gibi genel bulut.

Fiziksel veya sanal makine Azure günlük tümleştirme hizmeti çalıştıran Azure genel bulut ağ bağlantısı gerektirir. Bu makaledeki adımları yapılandırma ayrıntıları sağlar.

## <a name="prerequisites"></a>Ön koşullar
En azından AzLog aşağıdaki öğeleri gerektirir:
* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* A **depolama hesabı** için Windows Azure tanılama günlük kullanılabilir (bir önceden yapılandırılmış depolama hesabı kullanın veya yeni bir tane oluşturun – bu makalenin sonraki bölümlerinde depolama hesabını yapılandırmak göstereceğiz)
  >[!NOTE]
  Senaryonuza bağlı olarak bir depolama hesabı gerekli olmayabilir. Bu makalede bir ele alınan senaryo için Azure tanılama gereklidir.
* **İki sistem**: Azure günlük tümleştirme hizmeti çalışacak bir makine ve izlenecek ve Azlog hizmet makineye gönderilen kendi günlük bilgileri olan bir makineden.
   * İzlemek istediğiniz – bir makine olarak çalışan bir VM budur bir [Azure sanal makine](../virtual-machines/virtual-machines-windows-overview.md)
   * Azure günlük tümleştirme hizmeti çalışacak bir makine; Bu makineyi daha sonra SIEM içeri aktarılacak tüm günlük bilgi toplar.
    * Bu bir sistem şirket içi olabilir veya Microsoft Azure'da.  
    * X x64 çalıştırılması gerekiyor Windows server 2008 R2 SP1 sürümü veya daha yüksek ve .NET 4.5.1'in yüklü olması. Aşağıdaki başlıklı makaleye bakın tarafından yüklenmiş .NET sürüm belirleyebilirsiniz [nasıl yapılır: belirlemek, .NET Framework sürümlerinin yüklendiğini](https://msdn.microsoft.com/library/hh925568)  
    Azure tanılama günlüğü için kullanılan Azure depolama hesabı bağlantı olmalıdır. Biz bu makalenin sonraki bölümlerinde bu bağlantının nasıl onaylayabilirsiniz üzerinde talimatları içerir

Azure portalını kullanarak bir sanal makine oluşturma işlemi hızlı tanıtımı için video göz atın.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Dağıtma konuları
Azure günlük tümleştirme test ederken, en düşük işletim sistemi gereksinimlerini karşılayan tüm sistemi kullanabilirsiniz. Ancak, bir üretim ortamı için yük veya ölçekleme için planlama gerektirebilir.

Olay birim yüksekse, Azure günlük tümleştirme hizmeti (fiziksel veya sanal makine başına tek örnek) birden çok örneğini çalıştırabilirsiniz. Ayrıca, Azure Diagnostics depolama hesaplarını Windows (WAD) ve abonelik sayısı örneklerine sağlamak kapasite dayanmalıdır Bakiye yükleyebilirsiniz.
>[!NOTE]
Şu anda sizi ne zaman örnekleri Azure günlük tümleştirme makinelerin (yani, Azure günlük tümleştirme hizmeti çalışan sanal makineleri) veya ölçeklendirmek depolama hesapları ya da abonelik için yönelik belirli öneriler sahip değilsiniz. Bu alanların her biri de, performans gözlemleri kararları ölçeklendirme dayanmalıdır.

Ayrıca, performansını iyileştirmeye yardımcı olmak için bir Azure günlük tümleştirme hizmeti ölçeklendirmek seçeneğiniz de vardır. Aşağıdaki performans ölçümleri Azure günlük tümleştirme hizmeti çalıştırmak için seçtiğiniz makineler boyutlandırma yardımcı olabilir:
* Bir 8 işlemci (Temel) makinede Azlog Tümleştirici tek bir örneğini (~1M/hour) günde yaklaşık 24 milyon olay işleyebilir.

* 4 İşlemci (Temel) makinede Azlog Tümleştirici tek bir örneğini (~62.5K/hour) günde yaklaşık 1,5 milyon olayları işleyebilir.

## <a name="install-azure-log-integration"></a>Azure günlük tümleştirme yükleyin
Azure günlük tümleştirme yüklemek için Yükle gerekir [Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324) yükleme dosyası. Kurulum yordamı çalıştırın ve telemetri bilgilerini Microsoft'a vermelisiniz isteyip istemediğinize karar verin.  

![Yükleme ekran telemetri kutusu işaretli](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Microsoft'un telemetri verilerini toplamasına izin öneririz. Bu seçenek kaldırarak telemetri veri koleksiyonunu devre dışı bırakabilirsiniz.
>


Azure günlük tümleştirme hizmeti yüklü olduğu makinede telemetri verileri toplar.  

Toplanan telemetri verileri şöyledir:

* Azure günlük tümleştirme yürütülmesi sırasında oluşan özel durumlar
* Sorgular ve işlenen olayların sayısı hakkında ölçümleri
* Komut satırı seçenekleri hakkında hangi Azlog.exe kullanılan istatistikleri

Yükleme işlemi aşağıdaki videoda ele alınmıştır.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Yükleme ve doğrulama adımlarını sonrası
Temel Kurulum yordamı tamamladıktan sonra post yüklemeyi gerçekleştirmek için hazır adım ve doğrulama adımlarını olduğunuz:
1. Yükseltilmiş bir PowerShell penceresi açın ve gidin **c:\Program Files\Microsoft Azure günlük tümleştirme**
2. Uygulamanız gereken ilk adım, içe aktarılan AzLog cmdlet'leri almaktır. Bu komut dosyasını çalıştırarak yapabilirsiniz **LoadAzlogModule.ps1** (fark ". \" aşağıdaki komutta). Tür **.\LoadAzlogModule.ps1** ve basın **ENTER**.  
Aşağıdaki şekilde görünen gibi bir şey görmeniz gerekir. </br></br>
![Yükleme ekran telemetri kutusu işaretli](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Şimdi AzLog belirli bir Azure ortamı kullanmak üzere yapılandırmanız gerekir. Bir "Azure ortamı" "", çalışmak istediğiniz Azure bulut veri merkezini türüdür. Şu anda birkaç Azure ortamı varken şu anda uygun seçenekleri olan **AzureCloud** veya **AzureUSGovernment**.   Yükseltilmiş PowerShell ortamınızda içinde olduğundan emin olun **c:\program files\Microsoft Azure günlük tümleştirme\** </br></br>
    Bir kez, komutu çalıştırın: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(Azure ticari için)

      >[!NOTE]
      Komut başarılı olduğunda bir geri bildirim almazsınız.  US Government Azure bulut kullanmak istiyorsanız, kullanacağınız **AzureUSGovernment** (- adı değişkeni için) ABD bulutu için. Diğer Azure Bulutları şu anda desteklenmiyor.  
4. Bir sistem izleyebilmeniz için Azure tanılama kullanımda depolama hesabı adı gerekir.  Azure portalında gidin **sanal makineleri** ve sanal makine için izleyeceğiniz bakın. İçinde **özellikleri** bölümünde, seçin **tanılama ayarlarını**.  Tıklayın **Aracısı** ve belirtilen depolama hesabı adını not edin. Bu hesap adı, sonraki adım için gerekir.
![Azure tanılama ayarları](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Azure tanılama ayarları](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      İzleme sırasında sanal makine oluşturma etkinleştirilmediyse yukarıda gösterildiği gibi etkinleştirme seçeneği sunulur.
5. Şimdi biz uygulamamızla Azure günlük tümleştirme makine dön geçiş yaparsınız. Biz, depolama hesabına sistemden Azure günlük tümleştirme yüklendiği bağlantınız olduğunu doğrulamanız gerekir. Fiziksel bilgisayar veya Azure günlük tümleştirme hizmeti çalıştıran sanal makine her izlenen sistemleri üzerinde yapılandırılmış olarak Azure tanılama tarafından günlüğe kaydedilen bilgi almak için depolama hesabına erişim izni gerekiyor.  
  1. Azure Storage Gezgini indirebilirsiniz [burada](http://storageexplorer.com/).
  2. Kurulum yordamı çalıştırma
  3. ' Ye tıklayın yükleme işlemi tamamlandıktan sonra **sonraki** ve onay kutusunu boş bırakın **başlatma Microsoft Azure Storage Gezgini** işaretli.  
  4. Azure'da oturum açın.
  5. Azure tanılama için yapılandırılan depolama hesabı görebildiğini doğrulayın.  
![Depolama hesapları](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Depolama hesapları altında birkaç seçenek olduğuna dikkat edin. Bunlardan biri olan **tabloları**. Altında **tabloları** adlı görmelisiniz **WADWindowsEventLogsTable**. </br></br>
   ![Depolama hesapları](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Azure tanılama günlük tümleştirme
Bu adımda, günlük dosyalarını içeren depolama hesabına bağlanmak üzere Azure günlük tümleştirme hizmeti çalıştıran makine yapılandırır.
Bu adımı tamamlamak için birkaç Önden gerekir.  
* **FriendlyNameForSource:** bu uygulayabileceğiniz bir kolay addır, Azure tanılama bilgilerini depolamak için sanal makine yapılandırdığınız depolama hesabı
* **StorageAccountName:** Azure diagnotics yapılandırdığınızda belirtilen depolama hesabının adıdır.  
* **Depolama anahtarı:** bu depolama hesabının depolama anahtarı burada Azure tanılama bilgileri bu sanal makine için depolanır.  

Depolama anahtarı edinmek için aşağıdaki adımları gerçekleştirin:
 1. Gözat [Azure portal](http://portal.azure.com).
 2. Azure konsolunda Gezinti bölmesinde altına gidin ve'ı tıklatın **daha fazla hizmet.**

 ![Daha fazla hizmet](./media/security-azure-log-integration-get-started/more-services.png)
 3. Girin **depolama** içinde **filtre** metin kutusu. Tıklatın **depolama hesapları** (girdikten sonra bu görünür **depolama**)

   ![Filtre kutusuna](./media/security-azure-log-integration-get-started/filter.png)
 4. Depolama hesapları içeren bir liste görünür, çift günlük depolama alanına atanan hesap tıklayın.

   ![Depolama hesaplarının listesi](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Tıklayın **erişim anahtarları** içinde **ayarları** bölümü.

  ![Erişim tuşları](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopya **key1** ve bir sonraki adımda erişmek için güvenli bir konuma yerleştirin.

   ![iki erişim tuşu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Azure günlük tümleştirme yüklediğiniz sunucuda, yükseltilmiş bir komut istemi (biz yükseltilmiş bir komut istemi penceresi burada kullandığınız Not, yükseltilmiş bir PowerShell konsol değil) açın.
 8. Gidin **c:\Program Files\Microsoft Azure günlük tümleştirme**
 9. ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` öğesini çalıştırın </br> Örneğin ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` abonelik kimliği olay XML görünmesini isterseniz, abonelik kimliği için kolay ad eklemek: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` veya örneğin,``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
60 dakika kadar bekleyin ve sonra depolama hesabından çekilen olayları görüntüleyin. Görüntülemek için açın **Olay Görüntüleyicisi'ni > Windows Günlükleri > iletilen olaylar** Azlog Tümleştirici üzerinde.

Burada, yukarıda ele adımları üzerinden giderek bir video görebilirsiniz.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Ne veri iletilen olaylar klasöründe göstermiyor?
Bir saat sonra veri içinde gösterilmiyorsa **iletilen olaylar** klasörü, sonra:

1. Azure günlük tümleştirme hizmeti çalıştıran makine denetleyin ve Azure erişebildiğinizden emin olun. Bağlantıyı sınamak için açmaya [Azure portal](http://portal.azure.com) tarayıcıdan.
2. Kullanıcı hesabı emin olun **Azlog** klasörde yazma izni **users\Azlog**.
  <ol type="a">
   <li>Açık **Windows Gezgini** </li>
  <li> Gidin **c:\users** </li>
  <li> Sağ tıklayın **c:\users\Azlog** </li>
  <li> Tıklayın **güvenlik**  </li>
  <li> Tıklayın **NT Service\Azlog** ve hesap izinlerini denetleyin. Bu sekmeden hesabı yoksa veya uygun izinleri şu anda değilseniz, gösteren bu sekmede hesap hakları verebilirsiniz.</li>
  </ol>
3.Komuta eklenen depolama hesabı emin olun **Azlog kaynağı Ekle** komutu çalıştırdığınızda, listelenen **Azlog kaynağı listesi**.
4. Git **Olay Görüntüleyicisi'ni > Windows Günlükleri > Uygulama** herhangi bir hata olup olmadığını görmek için Azure günlük entegrasyonunu bildirdi.


Yükleme ve yapılandırma sırasında herhangi bir sorunla çalıştırırsanız, lütfen açık bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md)seçin **günlük tümleştirme** destek isteyen hizmet olarak.

Başka bir destek seçenek [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Burada topluluk birbirine sorular, yanıtlar, ipuçları ve püf noktaları Azure günlük tümleştirme yararlanmak nasıl destekleyebilir. Ayrıca, Azure günlük tümleştirme takım Bu forumda izler ve geçebiliriz her yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar
Azure günlük tümleştirmesi hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Microsoft Azure günlük tümleştirme Azure günlükleri için](https://www.microsoft.com/download/details.aspx?id=53324) – merkezi ayrıntılı bilgi için sistem gereksinimleri, yükleyip yönergeler Azure günlük tümleştirme.
* [Azure günlük tümleştirme giriş](security-azure-log-integration-overview.md) – Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı için bu belge size tanıtır.
* [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir. Geçerli kılavuzumuzu SIEM bileşenleri yapılandırma budur. SIEM satıcınıza ilk ek ayrıntılar için lütfen denetleyin.
* [Azure günlük sık sorulan sorular (SSS) tümleştirme](security-azure-log-integration-faq.md) -bu SSS Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Güvenlik Merkezi tümleştirme uyarıları Azure ile tümleştirme oturum](../security-center/security-center-integrating-alerts-with-log-integration.md) – bu belge, günlük analizi ya da SIEM Çözümle ilişkili Azure tanılama ve Azure etkinlik günlükleri tarafından toplanan sanal makine güvenlik olaylarını yanı sıra Güvenlik Merkezi uyarılarını eşitlemek gösterilmektedir.
* [Azure tanılama ve Azure denetim günlükleri için yeni özellikler](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – bu blog gönderisine Azure denetim günlükleri tanıtır ve yardımcı diğer özellikleri Azure kaynaklarınızı işlemleri alın.
