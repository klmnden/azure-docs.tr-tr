---
title: "Operations Manager için günlük analizi bağlanma | Microsoft Docs"
description: "System Center Operations Manager'da varolan yatırımınızı korumak ve günlük analizi ile genişletilmiş özelliklerini kullanmak için Operations Manager OMS çalışma alanınızla tümleştirebilirsiniz."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2017
ms.author: magoedte
ms.openlocfilehash: 6db47c7baa0a345a32d26d56e843acd0204ae50b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="connect-operations-manager-to-log-analytics"></a>Operations Manager günlük Analizi'ne bağlayın
System Center Operations Manager'da varolan yatırımınızı korumak ve günlük analizi ile genişletilmiş özelliklerini kullanmak için Operations Manager OMS çalışma alanınızla tümleştirebilirsiniz.  Bu, Operations Manager için kullanmaya devam ederken OMS fırsatlarını yararlanan sağlar:

* Operations Manager ile BT Hizmetleri sistem durumu izlemeye devam
* Olay ve sorun yönetimi destekleyen ITSM çözümlerinizi ile tümleştirme koru
* Şirket içi ve Operations Manager ile izleme genel bulut Iaas sanal makineleri dağıtılan aracılar yaşam döngüsü yönetme

System Center Operations Manager ile tümleştirme toplamak, depolamak ve Operations Manager'dan veri çözümleme OMS verimliliğini ve hız kullanarak hizmet işlemleri stratejinizi değer ekler.  OMS ilişkilendirmek ve sorunları hataları tanımlama ve varolan sorun yönetimi işleminizi desteklemek tekrarları görünmesini doğru çalışmasına yardımcı olur.   Performans, olay ve panolar ve bu verileri anlamlı şekillerde kullanıma sunmak için raporlama özelliklerini uyarı verileri incelemek için arama motoru esnekliğini Operations Manager complimenting OMS getirir gücünü gösterir.

Operations Manager yönetim grubuna raporlama aracıları sunucularınızdan günlük analizi veri kaynakları ve çözümleri OMS aboneliğinizde etkinleştirdiğiniz temel alarak veri toplar.  Etkinleştirdiğiniz, bu çözümlerin verilerden çözümüne bağlı olarak ya da gönderilir doğrudan bir Operations Manager yönetim sunucusundan OMS web hizmetine veya aracıyla yönetilen sistemde toplanan verilerin hacmi nedeniyle gönderilir doğrudan Aracıdan OMS web hizmetine. Yönetim sunucusu OMS web hizmetine doğrudan OMS veri iletir; Ayrıca, OperationsManager veya OperationsManagerDW veritabanına hiçbir zaman yazılır.  Bir yönetim sunucusu OMS web hizmetiyle bağlantı kaybettiğinde, yerel olarak iletişim OMS ile yeniden kurulur kurulmaz kadar verileri önbelleğe alır.  Yönetim sunucusu planlanan Bakım veya planlanmamış kesinti nedeniyle çevrimdışı ise, yönetim grubundaki başka bir yönetim sunucusu bağlantısı OMS ile sürdürür.  

Aşağıdaki diyagram, System Center Operations Manager yönetim grubu ve bağlantı noktaları ve yön dahil olmak üzere eklemek için aracıları ve yönetim sunucuları arasındaki bağlantıyı gösterir.   

![OMS-işlemleri-manager-tümleştirme-diyagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

BT güvenlik ilkelerinizi bilgisayarları Internet'e bağlanmak için ağınızdaki izin vermiyorsa, yönetim sunucuları yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak toplanan verileri göndermek için OMS ağ geçidine bağlanmak için yapılandırılabilir.  Daha fazla bilgi ve OMS hizmetine bir OMS ağ geçidi üzerinden iletişim kurmak için Operations Manager yönetim grubunuzu yapılandırma adımları için bkz: [OMS ağ geçidini kullanarak OMS bilgisayarları bağlamak](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Sistem gereksinimleri
Başlamadan önce önkoşulları karşılaması doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

* OMS yalnızca Operations Manager 2016, Operations Manager 2012 SP1 UR6 destekler ve daha büyük ve Operations Manager 2012 R2 UR2 büyük.  Operations Manager 2012 SP1 UR7 ve Operations Manager 2012 R2 UR3'e ara sunucu desteği eklenmiştir.
* Tüm Operations Manager aracıları, minimum destek gereksinimlerini karşılaması gerekir. Minimum güncelleştirmeyi aracıları olan, aksi takdirde Windows Aracısı trafiği başarısız olabilir ve birçok hata Operations Manager olay günlüğünü doldurma emin olun.
* Bir OMS abonelik.  Daha fazla bilgi için gözden [günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md).

### <a name="network"></a>Ağ
Operations Manager Aracısı, yönetim sunucuları ve işletim Konsolu OMS ile iletişim kurması gerekli proxy ve güvenlik duvarı yapılandırma bilgilerini listesi aşağıdaki bilgileri.  Her bileşenin OMS hizmete ağınızdan giden trafiğidir.     

|Kaynak | Bağlantı noktası numarası| Atlama HTTP denetleme|  
|---------|------|-----------------------|  
|**Aracı**|||  
|\*.ods.opinsights.azure.com| 443 |Evet|  
|\*.oms.opinsights.azure.com| 443|Evet|  
|\*.blob.core.windows.net| 443|Evet|  
|\*.azure-automation.net| 443|Evet|  
|**Yönetim sunucusu**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Evet|  
|\*.ods.opinsights.azure.com| 443| Evet|  
|*.azure-automation.net | 443| Evet|  
|**OMS için Operations Manager Konsolu**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 ve 443||  
|\*.microsoft.com| 80 ve 443||  
|\*.microsoftonline.com| 80 ve 443||  
|\*.mms.microsoft.com| 80 ve 443||  
|login.windows.net| 80 ve 443||  


## <a name="connecting-operations-manager-to-oms"></a>Operations Manager için OMS bağlanma
OMS alanlarınızı birine bağlanabilmeleri için Operations Manager yönetim grubu yapılandırma adımları aşağıdaki dizisini gerçekleştirin.

1. Operations Manager konsolunda seçin **Yönetim** çalışma.
2. Operations Management Suite düğümünü genişletin ve tıklatın **bağlantı**.
3. Tıklatın **kaydetmek için Operations Management Suite** bağlantı.
4. Üzerinde **Operations Management Suite Ekleme Sihirbazı: kimlik doğrulaması** sayfasında, e-posta adresi veya telefon numarasını ve OMS aboneliğinizle ilişkili yönetici hesabının parolasını girin ve tıklayın **oturum**.
5. Başarılı bir şekilde, üzerinde doğrulandıktan sonra **Operations Management Suite Ekleme Sihirbazı: çalışma alanı seçin** , sizden istenir, OMS çalışma seçmek için sayfa.  Birden fazla çalışma alanı varsa, aşağı açılan listeden Operations Manager yönetim grubuyla kaydedin ve ardından istediğiniz çalışma alanını seçin **sonraki**.
   
   > [!NOTE]
   > Operations Manager, aynı anda yalnızca bir OMS çalışma destekler. Bağlantı ve OMS'de önceki çalışma alanıyla kayıtlı olan bilgisayarları OMS kaldırılır.
   > 
   > 
6. Üzerinde **Operations Management Suite Ekleme Sihirbazı: Özet** sayfasında, ayarlarınızı doğrulayın ve doğru olmaları durumunda tıklayın **oluşturma**.
7. Üzerinde **Operations Management Suite Ekleme Sihirbazı: son** sayfasında, **Kapat**.

### <a name="add-agent-managed-computers"></a>Aracıyla yönetilen bilgisayarlar Ekle
Tümleştirme OMS çalışma alanınızla yapılandırma sonra bu yalnızca OMS ile bir bağlantı kurar, yönetim grubuna raporlama aracılardan gelen hiçbir veri toplanmadı. Hangi belirli aracıyla yönetilen bilgisayarlar için günlük analizi veri toplar yapılandırdıktan sonra bu kadar gerçekleşmez. Windows bilgisayar nesnelerini içeren bir grup seçebilirsiniz veya bilgisayar nesnelerinin ayrı ayrı seçebilirsiniz. Mantıksal diskleri veya SQL veritabanları gibi başka bir sınıfın örneklerini içeren bir grup seçemezsiniz.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. Operations Management Suite düğümünü genişletin ve tıklatın **bağlantı**.
3. Tıklatın **bilgisayar/Grup Ekle** Eylemler altında bağlantıyı sağ tarafında bölmenin başlık.
4. İçinde **bilgisayar arama** iletişim kutusu, arayabilirsiniz bilgisayarları veya grupları Operations Manager tarafından izlenen. Bilgisayarları veya grupları OMS eklemek için seçin, **Ekle**ve ardından **Tamam**.

Bilgisayarları ve grupları Operations Management Suite yönetilen bilgisayarlar düğümünden veri toplamak üzere yapılandırılmış görüntüleyebilirsiniz **Yönetim** Operations konsolunun çalışma.  Buradan, ekleyin veya gerektiğinde bilgisayarları ve grupları kaldırın.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Operations konsolunda OMS proxy ayarlarını yapılandırın
Bir iç proxy sunucu OMS web hizmetine ve yönetim grubu arasında ise aşağıdaki adımları gerçekleştirin.  Bu ayarlar merkezi olarak yönetilen yönetim grubundan ve OMS için veri toplamak için kapsam içinde yer alan aracıyla yönetilen sistemler için Dağıtılmış.  Bu zaman belirli çözümleri yönetim sunucusu atlayabilir ve doğrudan OMS web hizmetine veri göndermek için faydalıdır.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. Operations Management Suite genişletin ve ardından **bağlantıları**.
3. OMS Bağlantısı görünümünde, **Ara Sunucuyu Yapılandır**'a tıklayın.
4. Üzerinde **Operations Management Suite Sihirbazı: Proxy sunucusu** sayfasında, **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak**, örneğin, http://corpproxy:80 bağlantı noktası numarasıyla URL'sini yazın ve ardından **son**.

Proxy sunucusu kimlik doğrulaması gerektiriyorsa, kimlik bilgileri ve yönetim grubu için OMS raporları yönetilen bilgisayarlara yaymak için gereken ayarları yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. **RunAs Yapılandırması** altında, **Profiller**'i seçin.
3. **System Center Advisor Farklı Çalıştır Profili Ara Sunucusu** profilini açın.
4. Farklı Çalıştır profili Sihirbazı içinde bir farklı çalıştır hesabı kullanmak için Ekle'yi tıklatın. Oluşturabileceğiniz bir [farklı çalıştır hesabı](https://technet.microsoft.com/library/hh321655.aspx) veya var olan bir hesap kullanın. Doğrudan ara sunucuya geçiş yapmak için bu hesabın yeterli izinlere sahip olması gerekir.
5. Yönetmek için hesabınız ayarlamak için seçin **seçilen sınıf, Grup veya nesne**, tıklatın **seçin...** ve ardından **grup...** açmak için **grup arama** kutusu.
6. İçin arama yapın ve ardından **Microsoft System Center Advisor Monitoring Server grubu**.  Tıklatın **Tamam** kapatmak için Grup seçtikten sonra **grup arama** kutusu.
7. Tıklatın **Tamam** kapatmak için **bir farklı çalıştır hesabı ekleyin** kutusu.
8. Tıklatın **kaydetmek** Sihirbazı'nı tamamlayın ve değişikliklerinizi kaydedin.

Bağlantı oluşturulur ve hangi Aracıların toplamak ve veriler için OMS rapor yapılandırdıktan sonra aşağıdaki yapılandırma yönetim grubunda, mutlaka sırayla uygulanır:

* Farklı Çalıştır hesabı **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** oluşturulur.  Farklı Çalıştır profiliyle ilişkilendirildiği **Microsoft System Center Advisor çalıştırmak olarak profili Blob** ve iki sınıf - hedefleme **toplama sunucusuna** ve **Operations Manager yönetim grubu**.
* İki bağlayıcı oluşturulur.  İlk adlı **Microsoft.SystemCenter.Advisor.DataConnector** ve yönetim grubundaki tüm sınıfların örneklerinden OMS günlük analizi için üretilen tüm uyarılarını iletir bir abonelik ile otomatik olarak yapılandırılır. İkinci Bağlayıcısı **Advisor Bağlayıcısı**, OMS web hizmeti ile iletişim ve veri paylaşımını sorumlu olduğu.
* Aracılar ve yönetim grubundaki veri toplamak üzere seçtiğiniz gruplar eklenir **Microsoft System Center Advisor Monitoring Server grubu**.

## <a name="management-pack-updates"></a>Yönetim Paketi güncelleştirmeleri
Yapılandırma tamamlandıktan sonra Operations Manager yönetim grubu OMS hizmetiyle bir bağlantı kurar.  Yönetim sunucusu web hizmetiyle eşitler ve formunda yönetim paketlerinin Operations Manager ile tümleştirerek etkinleştirdiğiniz çözümler için güncelleştirilmiş yapılandırma bilgilerini alır.   Operations Manager, güncelleştirmeleri bu yönetim paketlerinin ve otomatik olarak denetler indirin ve kullanılabilir olduğunda bunları alır.  İki kurallar vardır özellikle, bu davranışı denetlemek:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -temel OMS yönetim paketleri güncelleştirir. Varsayılan olarak her 12 saatte çalışır.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -çalışma alanınızda etkin çözüm yönetim paketleri güncelleştirir. Varsayılan olarak beş (5) dakikada bir çalışır.

Devre dışı bırakarak otomatik indirme engellemek veya yönetim sunucusu ile yeni bir Yönetim Paketi kullanılabilir ve indirilmesi belirlemek için OMS ne sıklıkla eşitleneceğini sıklığını değiştirmek için bu iki kuralın geçersiz kılabilirsiniz.  Adımları [bir kural veya izleyici geçersiz kılmak nasıl](https://technet.microsoft.com/library/hh212869.aspx) değiştirmek için **sıklığı** eşitleme zamanlamasını değiştirmek veya değiştirmek için saniye cinsinden bir değer parametresiyle **etkin** kuralları devre dışı bırakmak için parametre.  Operations Manager yönetim grubu sınıfın tüm nesneleri için geçersiz kılmalar hedefleyin.

Üretim yönetim grubunuzdaki Yönetim Paketi sürümleri denetlemek için var olan değişiklik denetimi işlemi aşağıdaki devam etmek istiyorsanız, kuralları devre dışı bırakabilir ve ne zaman güncelleştirmelerine izin belirli zamanlarda etkinleştirin. Ortamınızda bir geliştirme veya QA yönetim grubu olması ve Internet bağlantısı varsa, bu yönetim grubu bu senaryoyu desteklemek için bir OMS çalışma ile yapılandırabilirsiniz.  Bu, gözden geçirmek ve üretim yönetim grubuna serbest bırakmadan önce OMS yönetim paketleri yinelemeli sürümleri değerlendirmek sağlar.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Bir Operations Manager grubuna yeni bir OMS çalışma alanı anahtarı
1. OMS aboneliğinizi oturum açın ve bir çalışma alanında oluşturmak [Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Operations Manager Yöneticiler rolünün bir üyesi olan bir hesapla Operations Manager konsolunu açın ve seçin **Yönetim** çalışma.
3. Operations Management Suite genişletin ve seçin **bağlantıları**.
4. Seçin **yeniden yapılandırma işlemi Yönetim Paketi** bölmesinin Orta taraftaki bağlantı.
5. İzleyin **Operations Management Suite Ekleme Sihirbazı** e-posta adresi veya telefon numarası ve yeni OMS çalışma alanınızla ilişkilendirilen yönetici hesabının parolasını girin.
   
   > [!NOTE]
   > **Operations Management Suite Ekleme Sihirbazı: çalışma alanı seçin** kullanımda olan bir çalışma sayfası gösterir.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>OMS ile Operations Manager tümleştirmesini doğrula
Operations Manager tümleştirmesi için OMS başarılı olduğunu doğrulamak birkaç farklı yolu vardır.

### <a name="to-confirm-integration-from-the-oms-portal"></a>OMS Portalı'ndan tümleştirme onaylamak için
1. OMS Portalı'nda tıklatın **ayarları** döşeme
2. Seçin **bağlı kaynakları**.
3. System Center Operations Manager bölümünün altında tabloda veri son alındığında listelenen aracıları ve durum sayısı ile yönetim grubunun adını görmeniz gerekir.
   
   ![OMS ayarları connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Not **çalışma alanı kimliği** sol taraftaki ayarları sayfasının altında değeri.  Bu Operations Manager yönetim grubunuzu karşı aşağıdaki doğrulayın.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Operations konsolundan tümleştirme onaylamak için
1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. Seçin **yönetim paketleri** ve **arayın:** metin kutusuna **Danışmanı** veya **Intelligence**.
3. Etkinleştirdiğiniz çözümleri bağlı olarak, arama sonuçları listesinde karşılık gelen bir Yönetim Paketi bakın.  Örneğin, uyarı yönetimi çözümü etkinleştirilirse, Yönetim Paketi Microsoft System Center Advisor uyarı yönetim listede ' dir.
4. Gelen **izleme** görüntülemek için gitmek **Operations Management Suite\Health durumu** görünümü.  Bir yönetim sunucusu altında seçin **yönetim sunucusu durumu** bölmesinde ve **ayrıntılı Görünüm** bölmesinde Onayla özelliğinin değeri **kimlik doğrulama hizmeti URI'si** OMS çalışma alanı kimliği ile eşleşir
   
   ![OMS-OpsMgr-MG-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>OMS ile tümleştirme Kaldır
Operations Manager yönetim grubu ve OMS çalışma arasında tümleştirme artık ihtiyaç duymadığınızda, gerektiği gibi yönetim grubunda bağlantısını ve yapılandırmasını kaldırmak için gereken birkaç adım vardır. Aşağıdaki yordam, yönetim grubunuzun başvuru silerek, OMS çalışma güncelleştirmek, OMS bağlayıcılarını silip ve OMS destekleyen yönetim paketlerini silin sahiptir.   

Çözümler için yönetim paketleri Operations Manager ile tümleşen etkinleştirdiğiniz ve OMS hizmeti ile tümleştirme desteklemek için gereken yönetim paketleri yönetim grubundan kolayca silinemez.  OMS yönetim paketlerinden başka ilgili yönetim paketlerine bağımlılıkları olan olmasıdır.  Başka yönetim paketlerine bir bağımlılığa sahip yönetim paketleri silmek için komut dosyasını karşıdan yükleyin [bağımlılıkları olan bir Yönetim paketini kaldırmak](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) TechNet Komut Merkezi.  

1. Operations Manager komut kabuğu Operations Manager Yöneticiler rolünün bir üyesi olan bir hesap ile açın.
   
    > [!WARNING]
    > Tüm özel yönetim paketlerinde word Danışmanı veya IntelligencePack ile devam etmeden önce adı yok ve aksi halde yönetim grubundan silin aşağıdaki adımları doğrulayın.
    > 

2. Komut kabuğu isteminde yazın`Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Sonraki türü`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. Diğer System Center Advisor yönetim paketlerine bağımlı olan kalan herhangi bir yönetim paketi kaldırmak için komut dosyası kullanma *RecursiveRemove.ps1* TechNet Komut Merkezi'nden daha önce indirdiğiniz.  
 
    > [!NOTE]
    > Microsoft System Center Danışmanı veya Microsoft System Center Advisor iç Yönetim Paketi silmeyin.  
    >  

5. Operations Manager Yöneticiler rolünün bir üyesi olan bir hesapla Operations Manager işletim konsolunu açın.
6. Altında **Yönetim**seçin **yönetim paketleri** düğümü ve **arayın:** kutusuna **Danışmanı** ve aşağıdaki yönetim paketleri yönetim grubunuza içeri doğrulamak:
   
   * Microsoft System Center Danışmanı
   * Microsoft System Center Advisor iç
7. OMS portalında **Ayarlar** kutucuğuna tıklayın.
8. Seçin **bağlı kaynakları**.
9. System Center Operations Manager bölümünün altında tabloda çalışma alanından kaldırmak istediğiniz yönetim grubunun adını görmeniz gerekir.  Sütununun altında **son veri**, tıklatın **kaldırmak**.  
   
    > [!NOTE]
    > **Kaldırmak** bağlantı kullanılamayacak kadar 14 gün sonra bağlı yönetim grubundan algılanan etkinlik yoksa.  
    > 

10. Kaldırma işlemine devam etmek istediğinizi onaylamanızı isteyen bir pencere görüntülenir.  Tıklatın **Evet** devam etmek için. 

İki bağlayıcı - Microsoft.SystemCenter.Advisor.DataConnector ve Advisor Bağlayıcısı silmek için aşağıdaki PowerShell Betiği bilgisayarınıza kaydedin ve aşağıdaki örnekleri kullanarak yürütün:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Bir yönetim sunucusu değilse, bu komut dosyasından çalıştırın bilgisayarda Operations Manager komut kabuğu, yönetim grubunuzun sürümüne bağlı olarak yüklü olması gerekir.
> 
> 

```
    param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

Yönetim grubunuz için bir OMS çalışma alanına yeniden bağlanmayı üzerinde planlıyorsanız, gelecekte, yeniden içeri aktarmanız gerekir `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` Yönetim Paketi yönetim grubuna uygulanan en son güncelleştirme paketi dosyasından.  Bu dosyada bulabilirsiniz `%ProgramFiles%\Microsoft System Center 2012` veya `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` klasör.

## <a name="next-steps"></a>Sonraki adımlar
İşlevsellik ekleme ve veri toplamak için bkz: [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).


