---
title: Operations Manager'ı Log Analytics'e bağlama | Microsoft Docs
description: System Center Operations Manager'a yaptığınız mevcut yatırımı korumak ve Log Analytics'le sağlanan genişletilmiş özellikleri kullanmak için, Operations Manager'ı çalışma alanınızla tümleştirebilirsiniz.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: magoedte
ms.openlocfilehash: b11cffcb006ba4f0598bd7f5cf6ed13daad2db42
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="connect-operations-manager-to-log-analytics"></a>Operations Manager'ı Log Analytics'e bağlama
System Center Operations Manager'a yaptığınız mevcut yatırımı korumak ve Log Analytics'le sağlanan genişletilmiş özellikleri kullanmak için, Operations Manager'ı Log Analytics çalışma alanınızla tümleştirebilirsiniz.  Bu sayede Operations Manager'ı kullanmaya devam ederken Log Analytics'in avantajlarından yararlanabilir ve:

* Operations Manager ile BT hizmetlerinizin durumunu izleyebilirsiniz.
* Olay ve sorun yönetimini destekleyen ISTM çözümlerinizle tümleştirmeyi koruyabilirsiniz.
* Operations Manager ile izlediğiniz şirket içi ve genel bulut IaaS sanal makinelerine dağıtılmış aracıların yaşam döngüsünü yönetebilirsiniz

System Center Operations Manager ile tümleştirme, Operations Manager'dan gelen verileri toplama, depolama ve analiz etme sürecinde Log Analytics'in hızı ve verimliliğinin kullanılmasıyla hizmet işlemleri stratejinize değer katar.  Log Analytics bağıntı sağlamaya ve mevcut sorun yönetim işleminizi destekleyecek şekilde sorunların ardındaki hataları tanımlama ve yinelemeleri ortaya çıkarma çalışmaları yapmaya yardımcı olur.  Performans, olay ve uyarı verilerini incelemeye yönelik arama motorunun esnekliği, bu verileri anlamlı yollarla ortaya koymaya yönelik zengin panolar ve raporlama özellikleriyle birlikte, Log Analytics'in Operations Manager'ı tamamlayan gücünü gösterir.

Operations Manager yönetim grubuna raporlayan aracılar, sunucularınızdan verileri toplarken çalışma alanınızda etkinleştirmiş olduğunuz Log Analytics veri kaynakları ve çözümlerini temel alır.  Etkinleştirilen çözümlere bağlı olarak, veriler doğrudan bir Operations Manager yönetim sunucusundan hizmete gönderilir veya aracının yönettiği sistemde toplanan verilerin hacmi nedeniyle aracıdan doğrudan Log Analytics'e gönderilir. Yönetim sunucusu verileri doğrudan hizmete iletir; bunlar hiçbir zaman işlem veya veri ambarı veritabanına yazılmaz.  Yönetim sunucusunun Log Analytics'le bağlantısı kesildiğinde, Log Analytics'le yeniden iletişim kurulana kadar verileri yerel olarak önbelleğe alır.  Planlı bir bakımdan veya planlanmamış bir kesintiden dolayı yönetim sunucusu çevrimdışı kalırsa, yönetim grubundaki başka bir yönetim sunucusu Log Analytics'le bağlantıyı sürdürür.  

Aşağıdaki diyagramda yön ve bağlantı noktalarıyla birlikte, System Center Operations Manager yönetim grubundaki yönetim sunucuları ve aracılarla Log Analytics arasındaki bağlantı gösterilir.   

![oms-operations-manager-integration-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

BT güvenlik ilkeleriniz ağınızdaki bilgisayarların İnternet'e bağlanmasına izin vermiyorsa, yönetim sunucuları yapılandırma bilgilerini almak ve etkinleştirilen çözümlere bağlı olarak toplanan verileri göndermek için OMS Ağ Geçidi'ne bağlanacak şekilde yapılandırılabilir.  Operations Manager yönetim grubunuzu OMS Ağ Geçici üzerinden Log Analytics hizmetiyle iletişim kuracak şekilde yapılandırma hakkında daha fazla bilgi edinmek ve bu işlemin adımlarını öğrenmek için bkz. [OMS Ağ Geçidi'ni kullanarak bilgisayarları OMS'ye bağlama](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Sistem gereksinimleri
Başlamadan önce, önkoşullara uyduğunuzu doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

* Log Analytics yalnızca System Center Operations Manager 1801, Operations Manager 2016, Operations Manager 2012 SP1 UR6 ve üstü ile Operations Manager 2012 R2 UR2 ve üstünü destekler.  Operations Manager 2012 SP1 UR7 ve Operations Manager 2012 R2 UR3'e ara sunucu desteği eklenmiştir.
* Tüm Operations Manager aracılarının en düşük destek gereksinimlerini karşılaması gerekir. Aracıların minimum güncelleştirme düzeyinde olduğundan emin olun; aksi takdirde Windows aracı trafiği başarısız olabilir ve Operations Manager olay günlüğü çok sayıda hatayla dolabilir.
* Log Analytics çalışma alanı.  Daha fazla bilgi için [Log Analytics'i kullanmaya başlama](log-analytics-get-started.md) konusunu gözden geçirin.

### <a name="network"></a>Ağ
Aşağıda Operations Manager aracısına, yönetim sunucularına ve Log Analytics'le iletişim kurmak için Operations konsoluna gereken ara sunucu ve güvenlik duvarı yapılandırma bilgileri listelenmiştir.  Her bileşenden gelen trafik, ağınızdan Log Analytics hizmetine giden trafiktir.     

|Kaynak | Bağlantı noktası numarası| HTTP İncelemesini atlama|  
|---------|------|-----------------------|  
|**Aracı**|||  
|\*.ods.opinsights.azure.com| 443 |Yes|  
|\*.oms.opinsights.azure.com| 443|Yes|  
|\*.blob.core.windows.net| 443|Yes|  
|\*.azure-automation.net| 443|Yes|  
|**Yönetim sunucusu**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Yes|  
|\*.ods.opinsights.azure.com| 443| Yes|  
|*.azure-automation.net | 443| Yes|  
|**Operations Manager konsolundan OMS'ye**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 ve 443||  
|\*.microsoft.com| 80 ve 443||  
|\*.microsoftonline.com| 80 ve 443||  
|\*.mms.microsoft.com| 80 ve 443||  
|login.windows.net| 80 ve 443||  
|portal.loganalytics.io| 80 ve 443||
|api.loganalytics.io| 80 ve 443||
|docs.loganalytics.io| 80 ve 443||  

## <a name="connecting-operations-manager-to-log-analytics"></a>Operations Manager'ı Log Analytics'e bağlama
Operations Manager yönetim grubunuzu Log Analytics çalışma alanlarınızdan birine bağlanacak şekilde yapılandırmak için aşağıdaki adım serisini uygulayın.

Operations Manager yönetim grubunuz ilk kez bir Log Analytics çalışma alanına kaydediliyorsa ve yönetim sunucularının hizmetle bir ara sunucu veya OMS Ağ Geçidi sunucusu üzerinden iletişim kurması gerekiyorsa, Operations konsolunda yönetim grubu için ara sunucu yapılandırmasını belirtme seçeneği sağlanmaz.  Bu seçeneğin sağlanması için önce yönetim grubunun hizmete başarıyla kaydedilmiş olması gerekir.  Tümleştirmeyi ve yönetim grubundaki tüm yönetim sunucularını yapılandırmak için Operations konsolunu çalıştırdığınız sistemde Netsh kullanarak sistem ara sunucu yapılandırmasını güncelleştirmeniz gerekir.  

1. Yükseltilmiş bir komut istemi açın.
1. Aşağıdaki komutu girin ve **Enter** tuşuna basın:

    `netsh winhttp set proxy <proxy>:<port>`

Aşağıdaki Log Analytics ile tümleştirme adımları tamamladıktan sonra, `netsh winhttp reset proxy` komutunu çalıştırarak yapılandırmayı kaldırabilir ve ara sunucuyu veya OMS Ağ Geçici sunucusunu belirtmek için Operations konsolunda **Ara sunucuyu yapılandır** seçeneğini kullanabilirsiniz. 

1. Operations Manager konsolunda **Yönetim** çalışma alanını seçin.
2. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
3. **Operations Management Suite'e kaydolun** bağlantısına tıklayın.
4. **Operations Management Suite Ekleme Sihirbazı: Kimlik Doğrulama** sayfasında, OMS aboneliğinizle ilişkilendirilmiş yönetici hesabının e-posta adresi veya telefon numarasını ve parolasını girin ve **Oturum aç**'a tıklayın.
5. Kimlik doğrulamanız başarılı olduktan sonra, **Operations Management Suite Ekleme Sihirbazı: Çalışma Alanı Seçin** sayfasında Log Analytics çalışma alanınızı seçmeniz istenir.  Birden çok çalışma alanınız varsa, açılan listeden Operations Manager yönetim grubuna kaydetmek istediğiniz çalışma alanını seçin ve ardından **İleri**'ye tıklayın.
   
   > [!NOTE]
   > Operations Manager bir kerede tek bir Log Analytics çalışma alanını destekler. Önceki çalışma alanıyla Log Analytics'e kaydedilmiş olan bilgisayarlar ve bağlantı Log Analytics'ten kaldırılır.
   > 
   > 
6. **Operations Management Suite Ekleme Sihirbazı: Özet** sayfasında ayarlarınızı onaylayın ve bunlar doğruysa **Oluştur**'a tıklayın.
7. **Operations Management Suite Ekleme Sihirbazı: Son** sayfasında **Kapat**'a tıklayın.

### <a name="add-agent-managed-computers"></a>Aracı tarafından yönetilen bilgisayarlar ekleme
Log Analytics çalışma alanıyla tümleştirmeyi yapılandırdıktan sonra, bu yapılandırma yalnızca hizmetle bağlantı kurar; yönetim grubunuza raporlayan aracılardan hiç veri toplanmaz. Siz aracı tarafından yönetilen hangi bilgisayarların Log Analytics için veri toplayacağını yapılandırana kadar veri toplama işlemi yapılmaz. Bilgisayar nesnelerini tek tek seçebileceğiniz gibi, Windows bilgisayar nesnelerini içeren bir grup da seçebilirsiniz. Mantıksal diskler veya SQL veritabanları gibi başka bir sınıfın örneklerini içeren grupları seçemezsiniz.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
3. Bölmenin sağ tarafındaki Eylemler başlığı altında **Bilgisayar/Grup Ekle** bağlantısına tıklayın.
4. **Bilgisayar Araması** iletişim kutusunda Operations Manager tarafından izlenen bilgisayarları veya grupları arayabilirsiniz. Log Analytics'e eklemek için bilgisayarları veya grupları seçin, **Ekle**'ye tıklayın ve ardından **Tamam**'a tıklayın.

İşletim konsolunun **Yönetim** çalışma alanında Operations Manager Suite'in altındaki Yönetilen Bilgisayarlar düğümünden veri toplamak için yapılandırılmış bilgisayarlar ve grupları görüntüleyebilirsiniz.  Burada, gerekirse bilgisayarları ve grupları ekleyebilir veya kaldırabilirsiniz.

### <a name="configure-proxy-settings-in-the-operations-console"></a>İşletim konsolunda ara sunucu ayarlarını yapılandırma
Yönetim grubu ile Log Analytics hizmeti arasında bir dahili ara sunucu varsa aşağıdaki adımları uygulayın.  Bu ayarlar yönetim grubunda merkezi olarak yönetilir ve Log Analytics'ten veri toplama kapsamına dahil edilmiş olan, aracı tarafından yönetilen sistemlere dağıtılır.  Bazı çözümlerin yönetim sunucusunu atladığı ve doğrudan hizmete veri gönderdiği durumlarda, bu yararlı olur.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. Operations Management Suite'i genişletin ve **Bağlantılar**'a tıklayın.
3. OMS Bağlantısı görünümünde, **Ara Sunucuyu Yapılandır**'a tıklayın.
4. **Operations Management Suite Sihirbazı: Ara Sunucu** sayfasında **Operations Management Suite erişimi için bir ara sunucu kullan**'ı seçin, ardından bağlantı noktası numarasını içeren URL'yi yazın (örneğin, http://corpproxy:80) ve **Son**'a tıklayın.

Ara sunucunuz için kimlik doğrulaması gerekiyorsa, aşağıdaki adımları uygulayarak yönetim grubunda OMS'ye raporlayan yönetilen bilgisayarlara dağıtılması gereken kimlik bilgilerini ve ayarları yapılandırın.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. **RunAs Yapılandırması** altında, **Profiller**'i seçin.
3. **System Center Advisor Farklı Çalıştır Profili Ara Sunucusu** profilini açın.
4. Farklı Çalıştır Profili Sihirbazı'nda, bir Farklı Çalıştır hesabı kullanmak için Ekle'ye tıklayın. [Farklı Çalıştır hesabı](https://technet.microsoft.com/library/hh321655.aspx) oluşturabilir veya mevcut bir hesabı kullanabilirsiniz. Doğrudan ara sunucuya geçiş yapmak için bu hesabın yeterli izinlere sahip olması gerekir.
5. Yönetilecek hesabı belirlemek için, **Seçilen bir sınıf, grup veya nesne**'yi seçin, **Seç...** düğmesine tıklayın ve ardından **Grup...** öğesine tıklayarak **Grup Araması** kutusunu açın.
6. **Microsoft System Center Advisor İzleme Sunucusu Grubu**'nu arayın ve ardından seçin.  Grubu seçtikten sonra **Grup Araması** kutusunu kapatmak için **Tamam**'a tıklayın.
7. **Farklı Çalıştır Hesabı Ekle** kutusunu kapatmak için **Tamam**'a tıklayın.
8. Sihirbazı tamamlamak ve yaptığınız değişiklikleri kaydetmek için **Kaydet**'e tıklayın.

Bağlantı oluşturulduktan ve siz hangi aracıların Log Analytics'e veri toplayıp raporlayacağını yapılandırdıktan sonra, yönetim grubuna aşağıdaki yapılandırma uygulanır (sırayla olması gerekmez):

* **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** Farklı Çalıştır Hesabı oluşturulur.  Bu hesap **Microsoft System Center Advisor Farklı Çalıştır Profili Blobu** Farklı Çalıştır profiliyle ilişkilendirilmiştir ve iki sınıfı hedefler: **Koleksiyon Sunucusu** ve **Operations Manager Yönetim Grubu**.
* İki bağlayıcı oluşturulur.  İlki **Microsoft.SystemCenter.Advisor.DataConnector** olarak adlandırılır ve yönetim grubundaki tüm sınıfların örneklerinden oluşturulmuş uyarıları Log Analytics'e ileten bir abonelikle otomatik olarak yapılandırılır. İkincisi **Advisor Bağlayıcısı**'dır ve OMS web hizmetliyle iletişim kurup veri paylaşmaktan sorumludur.
* Yönetim grubunda veri toplamak için seçilmiş olan aracılar ve gruplar **Microsoft System Center Advisor Sunucu İzleme Grubu**'na eklenir.

## <a name="management-pack-updates"></a>Yönetim paketi güncelleştirmeleri
Yapılandırma tamamlandıktan sonra, Operations Manager yönetim grubu Log Analytics hizmetiyle bağlantı kurar.  Yönetim sunucusu web hizmetiyle eşitlenir ve Operations Manager'la tümleştirilmek üzere etkinleştirmiş olduğunuz çözümler için yönetim paketleri biçiminde güncelleştirilmiş yapılandırma bilgilerini alır.   Operations Manager bu yönetim paketlerinin güncelleştirmelerini denetler, güncelleştirme sağlandığında bunları otomatik olarak indirir ve içeri aktarır.  Bu davranışı denetleyen özellikle iki kural vardır:

* **Microsoft.SystemCenter.Advisor.MPUpdate** - Temel Log Analytics yönetim paketlerini güncelleştirir. Varsayılan olarak her 12 saatte bir çalıştırılır.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - Çalışma alanınızda etkinleştirilmiş olan çözüm yönetim paketlerini güncelleştirir. Varsayılan olarak her beş (5) dakikada bir çalıştırılır.

Bunları devre dışı bırakıp otomatik indirmeyi engellemek veya yönetim sunucusunun yeni bir yönetim paketi olup olmadığını ve bunun indirilmesinin gerekip gerekmediğini saptamak üzere OMS ile eşitlenme sıklığını değiştirmek için bu iki kuralı geçersiz kılabilirsiniz.  **Frequency** parametresini saniye cinsinden bir değerle değiştirip eşitleme zamanlamasında değişiklik yapmak veya **Enabled** parametresini değiştirip kuralları devre dışı bırakmak için, [Kuralı veya İzlemeyi Geçersiz Kılma](https://technet.microsoft.com/library/hh212869.aspx) altındaki adımları izleyin.  Geçersiz kılmalarda, Operations Manager Yönetim Grubu sınıfındaki tüm nesneleri hedefleyin.

Üretim yönetim grubunuzda yönetim paketi sürümlerini denetlemek için mevcut değişiklik denetim sürecini izlemeye devam etmek istiyorsanız, kuralları devre dışı bırakabilir ve bunları güncelleştirmelere izin verilen belirli zamanlarda etkinleştirebilirsiniz. Ortamınızda bir geliştirme veya QA yönetim grubu varsa ve İnternet'e bağlıysa, bu senaryoyu desteklemek için söz konusu yönetim grubunu Log Analytics çalışma alanıyla yapılandırabilirsiniz.  Bu sayede Log Analytics yönetim paketlerini üretim yönetim grubunuzun kullanımına sunmadan önce bu yönetim paketlerinin yinelemeli sürümlerini gözden geçirebilir ve değerlendirebilirsiniz.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Operations Manager grubunu yeni bir Log Analytics Çalışma Alanına geçirme
1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**'i seçin ve bir çalışma alanı oluşturun.  
3. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
4. Operations Management Suite'i genişletin ve **Bağlantılar**'ı seçin.
5. Bölmenin orta kısmında **Operation Management Suite'i Yeniden Yapılandır** bağlantısını seçin.
6. **Operations Management Suite Ekleme Sihirbazı**'nı izleyin ve yeni Log Analytics çalışma alanınızla ilişkilendirilmiş yönetici hesabının e-posta adresi veya telefon numarasını ve parolasını girin.
   
   > [!NOTE]
   > **Operations Management Suite Ekleme Sihirbazı: Çalışma Alanı Seçin** sayfasında kullanımda olan mevcut çalışma alanı gösterilir.
   > 
   > 

## <a name="validate-operations-manager-integration-with-log-analytics"></a>Operations Manager'ın Log Analytics Tümleştirmesini Doğrulama
Log Analytics'in Operations Manager ile tümleştirmesinin başarılı olduğunu birkaç farklı yolla doğrulayabilirsiniz.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Azure portalında tümleştirmeyi onaylamak için
1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir.
2. Log Analytics çalışma alanlarınızın listesinde uygun çalışma alanını seçin.  
3. **Gelişmiş ayarlar**'ı, **Bağlı Kaynaklar**'ı ve sonra da **System Center**'ı seçin. 
4. System Center Operations Manager bölümünün altındaki tabloda yönetim grubu adının listelendiğini, ayrıca aracı sayısının ve veriler son alındığındaki durumun gösterildiğini görüyor olmalısınız.
   
   ![oms-settings-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>İşletim konsolunda tümleştirmeyi onaylamak için
1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
2. **Yönetim Paketleri**'ni seçin ve **Aranan:** metin kutusuna **Advisor** veya **Intelligence** yazın.
3. Etkinleştirdiğiniz çözümlere bağlı olarak, arama sonuçlarında ilgili yönetim paketinin listelendiğini görürsünüz.  Örneğin Uyarı Yönetimi çözümünü etkinleştirdiyseniz, listede Microsoft System Center Advisor Uyarı Yönetimi yönetim paketi yer alır.
4. **İzleme** görünümünden **Operations Management Suite\Sistem Durumu** görünümüne gidin.  **Yönetim Sunucusu Durumu** bölmesinin altında bir Yönetim sunucusu seçin ve **Ayrıntı Görünümü** bölmesinde **Kimlik doğrulama hizmeti URI'si** özelliğinin değerinin Log Analytics Çalışma Alanı Kimliği ile eşleştiğini onaylayın.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-log-analytics"></a>Log Analytics ile tümleştirmeyi kaldırma
Artık Operations Manager yönetim grubunuzda Log Analytics çalışma alanı arasında tümleştirmeye ihtiyacınız kalmadığında, yönetim grubunda bağlantıyı ve yapılandırmayı düzgün kaldırmak için izlenmesi gereken bazı adımlar vardır. Aşağıdaki yordam, yönetim grubunuzun başvurusunu siler, Log Analytics bağlayıcılarını siler, ardından hizmetle tümleştirmeyi destekleyen yönetim paketlerini siler ve bu şeklide Log Analytics çalışma alanınızı güncelleştirir.   

Etkinleştirdiğiniz çözümler için Operations Manager ile tümleştirilen yönetim paketleri ve Log Analytics hizmetiyle tümleştirme için gereken yönetim paketleri, yönetim grubundan kolayca silinemez.  Bunun nedeni, bazı Log Analytics yönetim paketlerinin diğer ilgili yönetim paketlerinde bağımlılıkları olmasıdır.  Diğer yönetim paketlerinde bağımlılıkları olan yönetim paketlerini silmek için, TechNet Betik Merkezi'nden [bağımlılıkları olan yönetim paketini kaldırma](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) betiğini indirin.  

1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager Komut Kabuğu'nu açın.
   
    > [!WARNING]
    > Devam etmeden önce adında Advisor veya IntelligencePack terimi bulunan hiçbir özel yönetim paketiniz olmadığını doğrulayın; aksi takdirde, aşağıdaki adımları o paketleri de yönetim grubundan siler.
    > 

2. Komut kabuğu istemcisine `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
3. Sonra `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
4. Diğer System Center Advisor yönetim paketlerinde bağımlılığı olan kalan yönetim paketlerini kaldırmak için, daha önce TechNet Betik Merkezi'nden indirmiş olduğunuz *RecursiveRemove.ps1* betiğini kullanın.  
 
    > [!NOTE]
    > Microsoft System Center Advisor veya Microsoft System Center Advisor Internal yönetim paketlerini silmeyin.  
    >  

5. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager İşletim konsolunu açın.
6. **Yönetim**'in altında **Yönetim Paketleri** düğümünü açın, **Aranan:** kutusuna **Advisor** yazın ve aşağıdaki yönetim paketlerinin yönetim grubunuza aktarılmış durumda olduğunu doğrulayın:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor Internal
1. Azure portalında Log Analytics çalışma alanının **Gelişmiş ayarlar** menüsünü açın.
1. **Bağlı Kaynaklar**’ı ve ardından **System Center**’ı seçin.
1. Çalışma alanından kaldırmak istediğiniz yönetim grubunun adını görüyor olmalısınız.  **Son Veriler** sütununun altında **Kaldır**'a tıklayın.  
   
    > [!NOTE]
    > Bağlı yönetim grubundan hiçbir etkinlik algılanmazsa 14 gün geçene kadar **Kaldır** bağlantısı kullanılamaz.  
    > 

10. Kaldırma işlemine devam etmek istediğinizi onaylamanızı isteyen bir pencere görüntülenir.  Devam etmek için **Evet**'e tıklayın. 

İki bağlayıcıyı (Microsoft.SystemCenter.Advisor.DataConnector ve Advisor Connector) silmek için, aşağıdaki PowerShell betiğini bilgisayarınıza kaydedin ve aşağıdaki örnekleri kullanarak yürütün:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Bu betiği çalıştırdığınız bilgisayar bir yönetim sunucusu değilse, yönetim grubunuzun sürümüne bağlı olarak bu bilgisayarda Operations Manager komut kabuğunun yüklü olması gerekir.
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

Gelecekte yönetim grubunuzu yeniden Log Analytics çalışma alanına bağlamayı planlıyorsanız, `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` yönetim paketi dosyasını yeniden içeri aktarmanız gerekecektir.  Ortamınıza dağıtılan System Center Operations Manager sürümüne bağlı olarak bu dosyayı aşağıdaki konumda bulabilirsiniz:

* System Center 2016 - Operations Manager ve üstü için kaynak medyada `\ManagementPacks` klasörünün altında.
* Yönetim grubunuza uygulanan en son güncelleştirme dağıtımından.  Kaynak klasör Operations Manager 2012 için ` %ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` klasörüdür ve 2012 R2 için `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` altında yer alır.

## <a name="next-steps"></a>Sonraki adımlar
İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisi'nden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).


