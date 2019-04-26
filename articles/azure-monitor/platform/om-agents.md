---
title: Operations Manager bağlanmak için Azure İzleyici | Microsoft Docs
description: System Center Operations Manager'a yaptığınız mevcut yatırımı korumak ve Log Analytics'le sağlanan genişletilmiş özellikleri kullanmak için, Operations Manager'ı çalışma alanınızla tümleştirebilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: magoedte
ms.openlocfilehash: 19ae3322d26447cf7c7dd94d06f073ccf013738e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60255064"
---
# <a name="connect-operations-manager-to-azure-monitor"></a>Operations Manager'ı Azure İzleyicisi ile bağlantı

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Mevcut yatırımınızı korumak için [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/key-concepts?view=sc-om-1807) ve Azure İzleyici ile genişletilmiş özellikleri kullanmak için Operations Manager ile Log Analytics çalışma alanınızın tümleştirebilirsiniz. Bu, Operations Manager için kullanmaya devam ederken Azure İzleyici günlüklerine fırsatlarını yararlanarak sağlar:

* Operations Manager ile BT hizmetlerinizin durumunu izleyebilirsiniz.
* Olay ve sorun yönetimini destekleyen ITSM çözümlerinizle tümleştirmeyi koruyabilirsiniz.
* Operations Manager ile izlediğiniz şirket içi ve genel bulut IaaS sanal makinelerine dağıtılmış aracıların yaşam döngüsünü yönetebilirsiniz

System Center Operations Manager ile tümleştirme ve verimlilikle toplamak, depolamak ve Operations Manager'dan gelen günlük verilerini analiz etme Azure İzleyicisi'ni kullanarak hizmet işlemleri stratejinize değeri ekler. Azure İzleyici günlük Yardım karşılıklı olarak ilişkilendirmek ve iş hataları, sorunları tanımlama ve yinelenme, mevcut sorun yönetimi süreciniz desteklemek üzere görünmesini sorgular. Operations Manager complimenting performans, olay ve uyarı Azure İzleyici'nın gücü panolar ve raporlama özellikleri, bu verileri anlamlı şekillerde kullanıma sunmak için verileri gösterir incelemek için sorgu Altyapısı'nın esneklik getirir.

Operations Manager yönetim grubuna raporlanan aracılar göre sunucularınızdan toplamak [Log Analytics veri kaynaklarını](agent-data-sources.md) ve çalışma alanınızda etkin çözümler. Etkin çözümleri bağlı olarak, kendi veri ya da gönderilir doğrudan bir Operations Manager yönetim sunucusundan hizmet veya aracıyla yönetilen sistemi, toplanan verilerin hacmi nedeniyle gönderilen doğrudan Aracıdan bir Log Analytics çalışma alanına. Yönetim sunucusu verileri doğrudan hizmete iletir; bunlar hiçbir zaman işlem veya veri ambarı veritabanına yazılmaz. Bir yönetim sunucusuna Azure İzleyici ile bağlantısını kaybettiğinde iletişimi yeniden kurulana kadar verileri yerel olarak önbelleğe alır. Yönetim sunucusu planlanan Bakım veya plansız kesinti nedeniyle çevrimdışı ise, yönetim grubundaki başka bir yönetim sunucusuna Azure İzleyici ile bağlantı sürdürür.  

Aşağıdaki diyagramda, System Center Operations Manager yönetim grubu ve bağlantı noktalarını ve yön dahil olmak üzere Azure İzleyici, aracılar ve yönetim sunucuları arasındaki bağlantıyı gösterir.   

![oms-operations-manager-integration-diagram](./media/om-agents/oms-operations-manager-connection.png)

BT güvenlik ilkeleriniz bilgisayarları ağınızdaki Internet'e bağlanmasına izin vermiyorsa, yönetim sunucuları yapılandırma bilgilerini almak ve çözümleri bağlı olarak toplanan verileri göndermek için Log Analytics ağ geçidine bağlanmak için yapılandırılabilir. etkin. Daha fazla bilgi ve Azure İzleyici için bir Log Analytics ağ geçidi üzerinden iletişim kurmak için Operations Manager yönetim grubunuzun yapılandırma adımları için bkz. [Azure Log Analytics ağ geçidini kullanarak İzleyici bilgisayarları bağlama](../../azure-monitor/platform/gateway.md).  

## <a name="prerequisites"></a>Önkoşullar 

Başlamadan önce aşağıdaki gereksinimleri gözden geçirin.

* Azure İzleyici, yalnızca System Center Operations Manager 2016 veya sonraki bir sürümü, Operations Manager 2012 SP1 UR6 destekler veya sonraki bir sürümü ve Operations Manager 2012 R2 UR2 veya üzeri. Operations Manager 2012 SP1 UR7 ve Operations Manager 2012 R2 UR3'e ara sunucu desteği eklenmiştir.
* System Center Operations Manager 2016 ABD kamu Bulutu ile tümleştirme, güncelleştirme paketi 2 veya daha sonra güncelleştirilmiş bir Advisor Yönetim Paketi gerektirir. System Center Operations Manager 2012 R2 güncelleştirme paketi 3 veya daha sonra güncelleştirilmiş bir Advisor Yönetim Paketi gerektirir.
* Tüm Operations Manager aracılarının en düşük destek gereksinimlerini karşılaması gerekir. En düşük güncelleştirmeyi aracılardır Windows aracı iletişimi Aksi halde ve başarısız Operations Manager olay günlüğündeki hatalara neden emin olun.
* Log Analytics çalışma alanı. Daha fazla bilgi için gözden [Log Analytics çalışma alanına genel bakış](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json).   
* Azure'de bir üyesi olan bir hesapla kimlik doğrulamasını [Log Analytics katkıda bulunan rolü](../../azure-monitor/platform/manage-access.md#manage-accounts-and-users).  

>[!NOTE]
>Azure API'leri yapılan son değişikliklerin, müşteriler kendi yönetim grubu ve Azure İzleyici arasındaki tümleştirme ilk kez başarıyla yapılandırabiliyor olmanın engeller. Var olan bağlantınızı yapılandırmak gerekli olmadıkça zaten kendi yönetim grubu hizmeti ile tümleştirilmiş müşteriler için etkilenmez.  
>Yeni bir Yönetim Paketi Operations Manager'ın şu sürümleri için kullanıma sundu:
>  
>* System Center Operations Manager 1801'için yönetim paketinden indirme [burada](https://www.microsoft.com/download/details.aspx?id=57173)  
>* System Center 2016 - Operations Manager uygulamasının yönetim paketinden indirme [burada](https://www.microsoft.com/download/details.aspx?id=57172)  
>* System Center Operations Manager 2012 R2 için yönetim paketinden indirme [burada](https://www.microsoft.com/download/details.aspx?id=57171)  
>
>Bu Yönetim Paketi güncelleştirme bir güncelleştirme sürümü sürümü 1801'e ve ürünün değil tam bir derleme olan System Center Operations Manager 1807 için uygulanamaz.   

### <a name="network"></a>Ağ

Azure İzleyici ile iletişim kurmak Operations Manager Aracısı, yönetim sunucuları ve işletim Konsolu için gerekli proxy ve güvenlik duvarı yapılandırma bilgileri listesi aşağıdaki bilgileri. Azure İzleyici ağınızdan giden trafik her bileşeni.   

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
|*.azure-automation.net | 443| Evet|  
|**Azure İzleyici için Operations Manager Konsolu**|||  
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

### <a name="tls-12-protocol"></a>TLS 1.2 Protokolü

Azure İzleyici geçiş verilerinin güvenliğini sağlamak üzere en az kullanılacak aracı ve yönetim grubunu yapılandırmak için önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**. Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="connecting-operations-manager-to-azure-monitor"></a>Azure İzleyici için Operations Manager'ı bağlama

Operations Manager yönetim grubunuzu Log Analytics çalışma alanlarınızdan birine bağlanacak şekilde yapılandırmak için aşağıdaki adım serisini uygulayın.

Operations Manager yönetim grubunuzun bir Log Analytics çalışma alanıyla ilk kayıt sırasında yönetim grubu için proxy yapılandırmasını belirtme seçeneği Operations konsolunda kullanılamaz.  Bu seçeneğin sağlanması için önce yönetim grubunun hizmete başarıyla kaydedilmiş olması gerekir.  Bu sorunu çözmek için Netsh sistemde işletim konsolundan çalıştıran tümleştirme ve tüm yönetim sunucuları, yönetim grubunda yapılandırmak için kullanarak sistem proxy yapılandırmasını güncelleştirmeniz gerekiyor.  

1. Yükseltilmiş bir komut istemi açın.
   a. Git **Başlat** ve türü **cmd**.
   b. Sağ **komut istemi** ve farklı çalıştır yönetici ** seçin.
1. Aşağıdaki komutu girin ve **Enter** tuşuna basın:

    `netsh winhttp set proxy <proxy>:<port>`

Azure İzleyici ile tümleştirmek için aşağıdaki adımları tamamladıktan sonra yapılandırmayı çalıştırarak kaldırabilirsiniz `netsh winhttp reset proxy` ve ardından **proxy sunucusunu yapılandır** proxy veya günlük belirtmek için işletim konsolunda seçeneği Analytics ağ geçidi sunucusu. 

1. Operations Manager konsolunda **Yönetim** çalışma alanını seçin.
1. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
1. **Operations Management Suite'e kaydolun** bağlantısına tıklayın.
1. Üzerinde **Operations Management Suite Ekleme Sihirbazı: Kimlik doğrulaması** sayfasında, e-posta adresi veya telefon numarası ve OMS aboneliğinizle ilişkili yönetici hesabının parolasını girin ve tıklayın **oturum**.

   >[!NOTE]
   >Operations Management Suite adı kullanımdan kaldırılmıştır. 
   
1. Başarılı bir şekilde, üzerinde kimlik doğrulaması yaptıktan sonra **Operations Management Suite Ekleme Sihirbazı: Çalışma alanı seçin** istenir Azure kiracısı, aboneliğiniz ve Log Analytics çalışma alanı seçin sayfasında. Birden çok çalışma alanınız varsa, açılan listeden Operations Manager yönetim grubuna kaydetmek istediğiniz çalışma alanını seçin ve ardından **İleri**'ye tıklayın.
   
   > [!NOTE]
   > Operations Manager bir kerede tek bir Log Analytics çalışma alanını destekler. Azure İzleyici'den, bağlantı ve Azure İzleyici önceki çalışma alanı ile kaydedilmiş bilgisayarların kaldırılır.
   > 
   > 
1. Üzerinde **Operations Management Suite Ekleme Sihirbazı: Özet** sayfasında, ayarları doğrulayın ve doğru olmaları durumunda tıklayın **Oluştur**.
1. Üzerinde **Operations Management Suite Ekleme Sihirbazı: Son** sayfasında **Kapat**.

### <a name="add-agent-managed-computers"></a>Aracı tarafından yönetilen bilgisayarlar ekleme

Tümleştirme ile Log Analytics çalışma alanınızı yapılandırma sonra yalnızca bir hizmet ile bağlantı kurar, yönetim grubunuza raporlama aracılardan gelen hiçbir veri toplanmadı. Hangi belirli aracıyla yönetilen bilgisayarların, Azure İzleyici'için günlük verilerinin toplanmasıyla yapılandırdıktan sonra bu kadar gerçekleşmez. Bilgisayar nesnelerini tek tek seçebileceğiniz gibi, Windows bilgisayar nesnelerini içeren bir grup da seçebilirsiniz. Mantıksal diskler veya SQL veritabanları gibi başka bir sınıfın örneklerini içeren grupları seçemezsiniz.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Operations Management Suite düğümünü genişletin ve **Bağlantı**'ya tıklayın.
1. Bölmenin sağ tarafındaki Eylemler başlığı altında **Bilgisayar/Grup Ekle** bağlantısına tıklayın.
1. **Bilgisayar Araması** iletişim kutusunda Operations Manager tarafından izlenen bilgisayarları veya grupları arayabilirsiniz. Bilgisayarları veya grupları ekleme Azure İzleyicisi'ni seçin, **Ekle**ve ardından **Tamam**.

İşletim konsolunun **Yönetim** çalışma alanında Operations Manager Suite'in altındaki Yönetilen Bilgisayarlar düğümünden, veri toplamak için yapılandırılmış bilgisayarları ve grupları görüntüleyebilirsiniz. Burada, gerekirse bilgisayarları ve grupları ekleyebilir veya kaldırabilirsiniz.

### <a name="configure-proxy-settings-in-the-operations-console"></a>İşletim konsolunda ara sunucu ayarlarını yapılandırma

Azure İzleyici ve yönetim grubu arasında bir iç proxy sunucusu ise, aşağıdaki adımları gerçekleştirin. Bu ayarlar merkezi yönetim grubundan yönetilen ve Azure İzleyici'için günlük verilerini toplamak için bir kapsam içinde yer aracıyla yönetilen sistemi dağıtılmış.  Bazı çözümlerin yönetim sunucusunu atladığı ve doğrudan hizmete veri gönderdiği durumlarda, bu yararlı olur.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Operations Management Suite'i genişletin ve **Bağlantılar**'a tıklayın.
1. OMS Bağlantısı görünümünde, **Ara Sunucuyu Yapılandır**'a tıklayın.
1. Üzerinde **Operations Management Suite Sihirbazı: Proxy sunucusu** sayfasında **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak**, ve ardından URL'si bağlantı noktası numarası ile örneğin http://corpproxy:80 ve ardından **son** .

Ara sunucunuz kimlik doğrulaması gerektiriyorsa, kimlik bilgilerini ve Azure İzleyici yönetim grubuna raporlayan yönetilen bilgisayarlara yayılmalıdır ayarlarını yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. **RunAs Yapılandırması** altında, **Profiller**'i seçin.
1. **System Center Advisor Farklı Çalıştır Profili Ara Sunucusu** profilini açın.
1. Farklı Çalıştır Profili Sihirbazı'nda, bir Farklı Çalıştır hesabı kullanmak için Ekle'ye tıklayın. [Farklı Çalıştır hesabı](https://technet.microsoft.com/library/hh321655.aspx) oluşturabilir veya mevcut bir hesabı kullanabilirsiniz. Doğrudan ara sunucuya geçiş yapmak için bu hesabın yeterli izinlere sahip olması gerekir.
1. Yönetilecek hesabı belirlemek için, **Seçilen bir sınıf, grup veya nesne**'yi seçin, **Seç...** düğmesine tıklayın ve ardından **Grup...** öğesine tıklayarak **Grup Araması** kutusunu açın.
1. **Microsoft System Center Advisor İzleme Sunucusu Grubu**'nu arayın ve ardından seçin. Grubu seçtikten sonra **Grup Araması** kutusunu kapatmak için **Tamam**'a tıklayın.
1. **Farklı Çalıştır Hesabı Ekle** kutusunu kapatmak için **Tamam**'a tıklayın.
1. Sihirbazı tamamlamak ve yaptığınız değişiklikleri kaydetmek için **Kaydet**'e tıklayın.

Bağlantı oluşturulur ve aracıların toplamak ve rapor günlük verilerini Azure İzleyici'ı yapılandırdıktan sonra aşağıdaki yapılandırma yönetim grubundaki mutlaka sırayla uygulanır:

* **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** Farklı Çalıştır Hesabı oluşturulur. Bu hesap **Microsoft System Center Advisor Farklı Çalıştır Profili Blobu** Farklı Çalıştır profiliyle ilişkilendirilmiştir ve iki sınıfı hedefler: **Koleksiyon Sunucusu** ve **Operations Manager Yönetim Grubu**.
* İki bağlayıcı oluşturulur.  İlk adlı **Microsoft.SystemCenter.Advisor.DataConnector** ve Azure İzleyici yönetim grubundaki tüm sınıfların örneklerini üretilen tüm uyarılarını iletir bir abonelik ile otomatik olarak yapılandırılır. İkinci bağlayıcı **Advisor Bağlayıcısı**, Azure İzleyici ile iletişim kurmasını ve veri paylaşımı sorumlu olduğu.
* Yönetim grubunda veri toplamak için seçilmiş olan aracılar ve gruplar **Microsoft System Center Advisor Sunucu İzleme Grubu**'na eklenir.

## <a name="management-pack-updates"></a>Yönetim paketi güncelleştirmeleri

Operations Manager yönetim grubu, yapılandırma tamamlandıktan sonra Azure İzleyici ile bağlantı kurar. Yönetim sunucusu web hizmetiyle eşitlenir ve Operations Manager'la tümleştirilmek üzere etkinleştirmiş olduğunuz çözümler için yönetim paketleri biçiminde güncelleştirilmiş yapılandırma bilgilerini alır. Operations Manager denetler ve otomatik olarak bu yönetim paketleri için güncelleştirmeleri indirmek ve bunları kullanılabilir olduklarında içeri aktarır. Bu davranışı denetleyen özellikle iki kural vardır:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -temel Azure İzleyicisi yönetim paketleri güncelleştirir. Varsayılan olarak her 12 saatte bir çalıştırılır.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - Çalışma alanınızda etkinleştirilmiş olan çözüm yönetim paketlerini güncelleştirir. Varsayılan olarak her beş (5) dakikada bir çalıştırılır.

Bu iki kılabilirsiniz kurallar devre dışı bırakarak otomatik yüklenmesini engelle veya yönetim sunucusu ile Azure Monitorto ne sıklıkla eşitleneceğini sıklığını değiştirmek için yeni bir Yönetim Paketi kullanılabilir ve indirilmesi gerektiğini belirler. **Frequency** parametresini saniye cinsinden bir değerle değiştirip eşitleme zamanlamasında değişiklik yapmak veya **Enabled** parametresini değiştirip kuralları devre dışı bırakmak için, [Kuralı veya İzlemeyi Geçersiz Kılma](https://technet.microsoft.com/library/hh212869.aspx) altındaki adımları izleyin. Geçersiz kılmalarda, Operations Manager Yönetim Grubu sınıfındaki tüm nesneleri hedefleyin.

Aşağıdaki üretim yönetim grubunuzdaki Yönetim Paketi sürümleri denetlemek için mevcut değişiklik denetimi işlemini devam etmek için kuralları devre dışı bırakabilir ve belirli zamanlarda güncelleştirmeleri ne zaman izin sağlamak. Ortamınızda bir geliştirme veya QA yönetim grubu varsa ve İnternet'e bağlıysa, bu senaryoyu desteklemek için söz konusu yönetim grubunu Log Analytics çalışma alanıyla yapılandırabilirsiniz. Bu, gözden geçirin ve üretim yönetim grubunuza bırakmadan önce Azure izleme yönetim paketlerinin yinelemeli yayınları değerlendirmek sağlar.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Operations Manager grubunu yeni bir Log Analytics Çalışma Alanına geçirme

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**'i seçin ve bir çalışma alanı oluşturun.  
1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. Log Analytics genişletin ve seçin **bağlantıları**.
1. Bölmenin orta kısmında **Operation Management Suite'i Yeniden Yapılandır** bağlantısını seçin.
1. İzleyin **Log Analytics Ekleme Sihirbazı** e-posta adresi veya telefon numarası ve yeni Log Analytics çalışma alanınızla ilişkili yönetici hesabının parolasını girin.
   
   > [!NOTE]
   > **Operations Management Suite Ekleme Sihirbazı: Çalışma alanı seçin** kullanımda olan mevcut bir çalışma sayfası sunar.
   > 
   > 

## <a name="validate-operations-manager-integration-with-azure-monitor"></a>Azure İzleyici ile Operations Manager tümleştirmesini doğrulama

Operations Manager tümleştirme için Azure İzleyici başarılı olduğunu doğrulamak birkaç farklı yolu vardır.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Azure portalında tümleştirmeyi onaylamak için

1. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir.
1. Log Analytics çalışma alanlarınızın listesinde uygun çalışma alanını seçin.  
1. **Gelişmiş ayarlar**'ı, **Bağlı Kaynaklar**'ı ve sonra da **System Center**'ı seçin. 
1. System Center Operations Manager bölümünün altındaki tabloda yönetim grubu adının listelendiğini, ayrıca aracı sayısının ve veriler son alındığındaki durumun gösterildiğini görüyor olmalısınız.
   
   ![oms-settings-connectedsources](./media/om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>İşletim konsolunda tümleştirmeyi onaylamak için

1. Operations Manager konsolunu açın ve **Yönetim** çalışma alanını seçin.
1. **Yönetim Paketleri**'ni seçin ve **Aranan:** metin kutusuna **Advisor** veya **Intelligence** yazın.
1. Etkinleştirdiğiniz çözümlere bağlı olarak, arama sonuçlarında ilgili yönetim paketinin listelendiğini görürsünüz.  Örneğin Uyarı Yönetimi çözümünü etkinleştirdiyseniz, listede Microsoft System Center Advisor Uyarı Yönetimi yönetim paketi yer alır.
1. **İzleme** görünümünden **Operations Management Suite\Sistem Durumu** görünümüne gidin.  **Yönetim Sunucusu Durumu** bölmesinin altında bir Yönetim sunucusu seçin ve **Ayrıntı Görünümü** bölmesinde **Kimlik doğrulama hizmeti URI'si** özelliğinin değerinin Log Analytics Çalışma Alanı Kimliği ile eşleştiğini onaylayın.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-azure-monitor"></a>Azure İzleyici ile tümleştirmesi Kaldır

Artık Operations Manager yönetim grubunuzda Log Analytics çalışma alanı arasında tümleştirmeye ihtiyacınız kalmadığında, yönetim grubunda bağlantıyı ve yapılandırmayı düzgün kaldırmak için izlenmesi gereken bazı adımlar vardır. Aşağıdaki yordam, yönetim grubunuzun başvuru silerek Log Analytics çalışma alanınızı güncelleştirmek, Azure İzleyici bağlayıcıları silme ve hizmeti ile tümleştirmeyi yönetim paketlerini silin sahiptir.  

Çözümler için yönetim paketleri Operations Manager ile tümleştirilen etkinleştirdiğiniz ve Azure İzleyici ile tümleştirmeyi desteklemek için gereken yönetim paketleri yönetim grubundan kolayca silinemiyor. Bazı Azure izleme yönetim paketlerinin diğer ilgili yönetim paketlerine bağımlılıkları sahip olmasıdır. Diğer yönetim paketlerinde bağımlılıkları olan yönetim paketlerini silmek için, TechNet Betik Merkezi'nden [bağımlılıkları olan yönetim paketini kaldırma](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) betiğini indirin.  

1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager Komut Kabuğu'nu açın.
   
    > [!WARNING]
    > Devam etmeden önce adında Advisor veya IntelligencePack terimi bulunan hiçbir özel yönetim paketiniz olmadığını doğrulayın; aksi takdirde, aşağıdaki adımları o paketleri de yönetim grubundan siler.
    > 

1. Komut kabuğu istemcisine `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
1. Sonra `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue` yazın
1. Diğer System Center Advisor yönetim paketlerinde bağımlılığı olan kalan yönetim paketlerini kaldırmak için, daha önce TechNet Betik Merkezi'nden indirmiş olduğunuz *RecursiveRemove.ps1* betiğini kullanın.  
 
    > [!NOTE]
    > PowerShell ile Danışman yönetim paketleri kaldırmak için adımı System Center Advisor'ı Microsoft veya Microsoft System Center iç Danışman yönetim paketleri otomatik olarak silinmez.  Silmek üzere çalışmayın.  
    >  

1. Operations Manager Yöneticiler rolüne üye olan bir hesapla Operations Manager İşletim konsolunu açın.
1. **Yönetim**'in altında **Yönetim Paketleri** düğümünü açın, **Aranan:** kutusuna **Advisor** yazın ve aşağıdaki yönetim paketlerinin yönetim grubunuza aktarılmış durumda olduğunu doğrulayın:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor Internal

1. Azure portalında **ayarları** Döşe.
1. Seçin **bağlı kaynakları**.
1. System Center Operations Manager bölümünün altındaki tabloda, çalışma alanından kaldırmak istediğiniz yönetim grubunun adını görmeniz gerekir. **Son Veriler** sütununun altında **Kaldır**'a tıklayın.  
   
    > [!NOTE]
    > Bağlı yönetim grubundan hiçbir etkinlik algılanmazsa 14 gün geçene kadar **Kaldır** bağlantısı kullanılamaz.  
    > 

1. Kaldırma işlemine devam etmek istediğinizi onaylamanızı isteyen bir pencere görüntülenir.  Devam etmek için **Evet**'e tıklayın. 

İki bağlayıcıyı (Microsoft.SystemCenter.Advisor.DataConnector ve Advisor Connector) silmek için, aşağıdaki PowerShell betiğini bilgisayarınıza kaydedin ve aşağıdaki örnekleri kullanarak yürütün:

```
    .\OM2012_DeleteConnectors.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SystemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> Bu betiği çalıştırdığınız bilgisayar bir yönetim sunucusu değilse, yönetim grubunuzun sürümüne bağlı olarak bu bilgisayarda Operations Manager komut kabuğunun yüklü olması gerekir.
> 
> 

```powershell
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

Yönetim grubunuza bir Log Analytics çalışma alanı yeniden bağlanmayı üzerinde planlıyorsanız, gelecekte yeniden içeri aktarmak ihtiyacınız `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` Yönetim Paketi dosyası. Ortamınıza dağıtılan System Center Operations Manager sürümüne bağlı olarak bu dosyayı aşağıdaki konumda bulabilirsiniz:

* System Center 2016 - Operations Manager ve üstü için kaynak medyada `\ManagementPacks` klasörünün altında.
* Yönetim grubunuza uygulanan en son güncelleştirme dağıtımından. Operations Manager 2012 için kaynak klasördür `%ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` ve 2012 R2 için bulunur `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>Sonraki adımlar

İşlev eklemek ve veri toplamak için bkz: [Azure İzleyici'yi ekleyin çözüm galeri'sinden](../../azure-monitor/insights/solutions.md).