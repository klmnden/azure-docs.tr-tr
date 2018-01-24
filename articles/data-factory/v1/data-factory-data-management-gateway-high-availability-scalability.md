---
title: "Azure Data factory'de veri yönetimi ağ geçidi ile yüksek kullanılabilirlik | Microsoft Docs"
description: "Bu makalede daha fazla düğüm ve ölçek ekleyerek veri yönetimi ağ geçidi nasıl ölçeklendirebilirsiniz açıklanmaktadır yukarı bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 195a1a4810de478b77538716fa8d1362428864d8
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Veri Yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik (Önizleme)
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [tümleştirmesi çalışma zamanı sürüm 2'kendi kendini barındıran](../create-self-hosted-integration-runtime.md). 


Bu makalede, veri yönetimi ağ geçidi ile yüksek kullanılabilirlik ve ölçeklenebilirlik çözümü yapılandırmanıza yardımcı olur / tümleştirme.    

> [!NOTE]
> Bu makale, zaten tümleştirmesi çalışma zamanı (önceki veri yönetimi ağ geçidi) Temelleri ile bildiğinizi varsayar. Emin değilseniz, bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).

>**Bu önizleme özelliği resmi olarak veri yönetimi ağ geçidi sürümü 2.12.xxxx.x ve üzeri sürümlerde desteklenen**. Lütfen sürüm 2.12.xxxx.x kullandığınızdan emin olun veya üstü. Veri Yönetimi ağ geçidi en son sürümünü indirme [burada](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Genel Bakış
Tek bir mantıksal ağ geçidi portalından sahip birden çok şirket içi makinelerde yüklü veri yönetimi ağ geçidi ilişkilendirebilirsiniz. Bu makineler adlı **düğümleri**. En fazla olabilir **dört düğüm** mantıksal bir ağ geçidi ile ilişkilendirilmiş. Mantıksal bir ağ geçidi için birden çok düğüm (yüklü ağ geçidi ile şirket içi makineler) sahip avantajları şunlardır:  

- Şirket içi ve bulut arasında veri taşıma performansını verileri depolar.  
- Herhangi bir nedenden dolayı düğümlerinden biri arıza yaparsa, diğer düğümler veri taşımak için hala kullanılabilmektedir. 
- Bir düğümünde bakım için çevrimdışı duruma ihtiyacınız varsa, diğer düğümler veri taşımak için hala kullanılabilir durumdadır.

Sayısı da yapılandırabilirsiniz **eş zamanlı veri taşıma işleri** şirket içi ve bulut arasında veri taşıma özelliği ölçeklendirmek için bir düğüm üzerinde çalışabilir veri depolarına. 

Azure Portalı'nı kullanarak, bir düğüm mantıksal ağ geçidi'nden ekleyip karar vermenize yardımcı olan bu düğümler durumunu izleyebilirsiniz. 

## <a name="architecture"></a>Mimari 
Aşağıdaki diyagramda mimari genel bakış ölçeklenebilirlik ve kullanılabilirlik özelliği veri yönetimi ağ geçidi sağlar: 

![Veri Yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

A **mantıksal ağ geçidi** Azure Portal'daki data factory eklediğiniz ağ geçididir. Daha önce veri yönetimi ağ geçidi mantıksal bir ağ geçidi ile yüklü olan yalnızca bir şirket içi Windows makine ilişkilendirme. Bu ağ geçidi makinesi bir düğüm olarak adlandırılan şirket içi. En fazla şimdi ilişkilendirebilirsiniz **dört fiziksel düğüm** mantıksal bir ağ geçidi ile. Mantıksal bir ağ geçidi birden çok düğüm ile adlı bir **çok düğümlü ağ geçidi**.  

Bu düğümler **etkin**. Tüm şirket içi ve bulut arasında veri taşımak için veri taşıma işleri işleyebilmesi için verileri depolar. Düğümlerden birinde dağıtıcısı ve çalışan davranır. Diğer düğümlere gruplar halinde çalışan düğümleri ' dir. A **dağıtıcısı** düğümü veri taşıma görevleri/işleri bulut hizmetinden çeker ve çalışan düğümlerine (kendisi dahil) gönderir. A **çalışan** düğümü şirket içi ve bulut arasında veri taşımak için veri taşıma işlerini yürütür verileri depolar. Tüm düğümleri çalışanlardır. Yalnızca bir düğüm, gönderme ve çalışan olabilir.    

Tek bir düğüm genellikle başlayabilir ve **ölçeğini** varolan düğümlerini veri taşıma yük ile çok gibi daha fazla düğüm eklemek için. Ayrıca **ölçeği** düğümde çalışmasına izin verilen eşzamanlı iş sayısını artırarak bir ağ geçidi düğümü, veri taşıma özelliği. Bu özellik, tek bir düğüm ile ağ geçidi (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin değilse bile) de kullanılabilir. 

Bir ağ geçidi birden çok düğüm ile veri deposu kimlik tüm düğümlere eşitlenmiş tutar. Bir düğümden düğüme bağlantı sorunu varsa, kimlik bilgileri eşitlenmemiş olabilir. Bir ağ geçidi kullanan bir şirket içi veri deposu için kimlik bilgilerini ayarladığınızda, dağıtıcı/çalışan düğümünde kimlik bilgileri kaydeder. Dağıtıcı düğümü eşitlemeler diğer alt düğümü. Bu işlem olarak bilinir **kimlik eşitleme**. Düğümler arasındaki iletişim kanalını olabilir **şifrelenmiş** ortak SSL/TLS sertifikası tarafından. 

## <a name="set-up-a-multi-node-gateway"></a>Bir çok düğümlü ağ geçidi kurun
Bu bölümde, aşağıdaki iki makaleler veya bu makalelerde kavramları tanıdık aracılığıyla geçmiş olabileceğini varsayar: 

- [Veri Yönetimi ağ geçidi](data-factory-data-management-gateway.md) -ağ geçidi ayrıntılı bir genel bakış sağlar.
- [Şirket içi ve bulut arasında veri taşıma veri depolarına](data-factory-move-data-between-onprem-and-cloud.md) -bir ağ geçidi ile tek bir düğüm kullanmak için adım adım yönergeleri içeren bir kılavuz içerir.  

> [!NOTE]
> Bir şirket içi Windows makinesinde veri yönetimi ağ geçidi'ni yüklemeden önce listelenen önkoşullara bakın [Ana Makalesi](data-factory-data-management-gateway.md#prerequisites).

1. İçinde [izlenecek](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), mantıksal bir ağ geçidi oluştururken, etkinleştirme **yüksek kullanılabilirlik ve ölçeklenebilirlik** özelliği. 

    ![Veri Yönetimi ağ geçidi - enable yüksek kullanılabilirlik ve ölçeklenebilirlik](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. İçinde **yapılandırma** sayfasında, kullanın ya da **Express Kurulum** veya **el ile Kurulum** ilk düğümde (şirket içi Windows makine) bir ağ geçidi yüklemek için bağlantı.

    ![Veri Yönetimi ağ geçidi - hızlı veya el ile Kurulum](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Hızlı kurulum seçeneğini kullanırsanız, düğüm düğüme iletişim şifreleme olmadan yapılır. Düğüm adı, makine adı olarak aynıdır. El ile kuruluma düğümler iletişimi şifrelenmesi gerekiyor veya tercih ettiğiniz bir düğüm adı belirtmek istiyorsanız kullanın. Düğüm adı daha sonra düzenlenemez.
3. Seçerseniz **hızlı kurulumu**
    1. Ağ geçidi başarıyla yüklendikten sonra aşağıdaki iletiyi görürsünüz:

        ![Veri Yönetimi ağ geçidi - hızlı Kurulumu başarılı](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Aşağıdaki ağ geçidi için veri yönetimi Yapılandırma Yöneticisi başlatma [bu yönergeleri](data-factory-data-management-gateway.md#configuration-manager). Ağ geçidi adı, düğüm adı, durumu, vb. görürsünüz.

        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Seçerseniz **el ile kuruluma**:
    1. Ağ geçidini makinenize yüklemek için çalıştırın Microsoft Download Center, yükleme paketini indirin.
    2. Kullanım **kimlik doğrulama anahtarı** gelen **yapılandırma** ağ geçidini kaydetmek için sayfa.
    
        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. İçinde **yeni ağ geçidi düğümü** sayfasında, özel bir sağlayabilir **adı** ağ geçidi düğümü. Varsayılan olarak, makine adı olarak aynı düğüm adı.    

        ![Veri Yönetimi ağ geçidi - adını belirtin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. Sonraki sayfada, seçtiğiniz kullanılıp kullanılmayacağını **düğümü düğümü iletişimi için şifrelemeyi etkinleştirmeniz**. Tıklatın **atla** şifreleme (varsayılan) devre dışı bırakmak için.

        ![Veri Yönetimi ağ geçidi - enable şifreleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Şifreleme modunu değiştirme, yalnızca bir tek ağ geçidi düğümü mantıksal ağ geçidi olduğunda desteklenir. Bir ağ geçidi birden fazla düğüme sahip olduğunda şifreleme modunu değiştirmek için aşağıdaki adımları uygulayın: bir düğüm dışındaki tüm düğümleri silerseniz, şifreleme modunu değiştirin ve ardından düğümler yeniden ekleyin.
        > 
        > Bkz: [TLS/SSL sertifika gereksinimlerini](#tlsssl-certificate-requirements) bir TLS/SSL sertifikası kullanmak için gereksinimleri listesi bölümü. 
    5. Ağ geçidi başarıyla yüklendikten sonra başlatma Configuration Manager'ı tıklatın:
    
        ![El ile kuruluma - başlatma Yapılandırma Yöneticisi](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi bağlantısı durumunu gösteren, düğümde (şirket içi Windows makine), bkz. **ağ geçidi adı**, ve **düğüm adı**.  

        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Bir Azure VM ağ geçidinde sağlıyorsanız, kullanabileceğiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-mutiple-vms-with-data-management-gateway). Bu komut, mantıksal bir ağ geçidi oluşturur, veri yönetimi ağ geçidi yazılımı yüklü Vm'leri ayarlar ve mantıksal ağ geçidi ile kaydeder. 
6. Azure portalında başlatma **ağ geçidi** sayfa: 
    1. Portalı'nda veri fabrikası giriş sayfasında tıklatın **bağlı hizmetler**.
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. seçin **ağ geçidi** görmek için **ağ geçidi** sayfa:
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. Gördüğünüz **ağ geçidi** sayfa:   

        ![Tek düğüm görünümü ile ağ geçidi](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Tıklatın **düğüm Ekle** mantıksal ağ geçidi için bir düğüm eklemek için araç çubuğunda. Hızlı Kurulum kullanmayı planlıyorsanız, ağ geçidi için bir düğüm olarak eklenecek şirket içi makineden bu adımı uygulayın. 

    ![Veri Yönetimi ağ geçidi - düğüm menü ekleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Adımlar, ilk düğümü ayarını benzer. Yapılandırma Yöneticisi kullanıcı Arabirimi, el ile yükleme seçeneğini seçerseniz, düğüm adı ayarlamanızı sağlar: 

    ![Configuration Manager - yükleme ikinci ağ geçidi](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Ağ geçidi düğümüne başarıyla yüklendikten sonra Configuration Manager aracını aşağıdaki ekran görüntüler:  

    ![Configuration Manager - yükleme ikinci ağ geçidi başarılı](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Açarsanız **ağ geçidi** sayfa Portalı'nda, iki ağ geçidi düğümleri şimdi görürsünüz: 

    ![Ağ geçidi portalında iki düğümü](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. Bir ağ geçidi düğümü silmek için tıklatın **silmek düğümü** araç çubuğunda, silme ve ardından istediğiniz düğümü seçin **silmek** araç çubuğundan. Bu eylem seçili düğümün grubundan siler. Bu eylem düğümden (şirket içi Windows makine) veri yönetimi ağ geçidi yazılımını kaldırmaz unutmayın. Kullanım **Program Ekle veya Kaldır'ı** ağ geçidini kaldırmak için Denetim Masası şirket içi'nda. Ağ geçidi düğümü kaldırdığınızda, Portalı'nda otomatik olarak silinir.   

## <a name="upgrade-an-existing-gateway"></a>Mevcut bir ağ geçidi yükseltme
Yüksek kullanılabilirlik ve ölçeklenebilirlik özelliği kullanmak için mevcut bir ağ geçidi yükseltme yapabilirsiniz. Bu özellik, veri yönetimi ağ geçidi sürümü sahip düğümüyle çalışır > 2.12.xxxx =. Veri Yönetimi ağ geçidi bir makinede yüklü sürümünü görmek **yardımcı** sekmesi, veri yönetimi ağ geçidi Yapılandırma Yöneticisi. 

1. Ağ geçidi şirket içi makinede aşağıdaki tarafından yükleyerek güncelleştirin ve Kurulum paketinden bir MSI çalıştıran [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Bkz: [yükleme](data-factory-data-management-gateway.md#installation) ayrıntıları bölümü.  
2. Azure portalına gidin. Başlatma **Data Factory sayfasını** veri fabrikanızın. Bağlı hizmetler kutucuğunda başlatmak için tıklatın **bağlantılı Hizmetler Sayfası**. Başlatmak için ağ geçidi seçin **ağ geçidi sayfa**. ' I tıklatın ve etkinleştirme **önizleme özelliği** aşağıdaki görüntüde gösterildiği gibi: 

    ![Veri Yönetimi ağ geçidi - önizleme özelliğini etkinleştir](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Önizleme özelliğini portalda etkinleştirildikten sonra tüm sayfaları kapatın. Yeniden **ağ geçidi sayfa** yeni önizleme kullanıcı arabirimi (UI) görmek için.
 
    ![Veri Yönetimi ağ geçidi - enable önizleme özelliği başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Veri Yönetimi ağ geçidi - UI Önizleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Yükseltme sırasında ilk düğümü adı makinenin adıdır. 
3. Şimdi, bir düğüm ekleyin. İçinde **ağ geçidi** sayfasında, **düğüm Ekle**.  

    ![Veri Yönetimi ağ geçidi - düğüm menü ekleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Düğümü ayarlamak için önceki bölümdeki yönergeleri izleyin. 

### <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler

- Makine değil hazırda bekletme güç planı ağ geçidi için konak makinedeki yapılandırın. Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez.
- Ağ geçidi ile ilişkili sertifika yedekleyin.
- Tüm düğümler (ideal performans için önerilen) benzer yapılandırmasının emin olun. 
- Yüksek kullanılabilirlik sağlamak için en az iki düğüm ekleyin.  

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifika gereksinimleri
Çalışma zamanı düğümleri tümleştirme arasındaki iletişimi korumak için kullanılan TLS/SSL sertifika için gereksinimler şunlardır:

- Sertifika genel olarak güvenilir X509 olmalıdır v3 sertifikası. Ortak (üçüncü taraf) sertifika yetkilisi (CA) tarafından verilen sertifikaların kullanmanızı öneririz.
- Her tümleştirme Çalışma Zamanı Modülü düğüme kimlik bilgisi Yöneticisi uygulaması çalıştıran istemci makine yanı sıra, bu sertifikaya güvenmesi gerekir. 
> [!NOTE]
> Kimlik bilgisi Yöneticisi uygulaması, kimlik bilgisi Kopyalama Sihirbazı'ndan güvenli bir şekilde ayarlanırken kullanılır / Azure Portal. Ve bu, şirket içi aynı ağ içinde herhangi bir makineden Mazotlu / özel veri deposu.
- Joker karakter sertifikaları desteklenir. FQDN adınız ise **node1.domain.contoso.com**, kullanabileceğiniz ***. domain.contoso.com** sertifikanın konu adı olarak.
- SAN sertifikaları yalnızca son öğeyi konu alternatif adlarını kullanılır ve diğerleri geçerli sınırlamaları nedeniyle yoksayılacak beri önerilmez. Örneğin SAN olan bir SAN sertifikanız **node1.domain.contoso.com** ve **node2.domain.contoso.com**, yalnızca bu sertifikayı FQDN değeri olduğu makinede kullanabileceğiniz **node2.domain.contoso.com**.
- SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutunu destekler.
- CNG kullanarak sertifika anahtarlar desteklenmez. Doesrted DoesDoes CNG anahtarları kullanan sertifikaları desteklemez.

#### <a name="faq-when-would-i-not-enable-this-encryption"></a>Hakkında SSS: Ne zaman t bu şifreleme etkinleştirmemeniz?
Şifrelemeyi etkinleştirme ekleyebilirsiniz belirli altyapınıza (sahibi olan ortak sertifika) bu nedenle maliyet etkinleştirme şifreleme atlayabilirsiniz durumlarda altında:
- Güvenilir bir ağ veya bir ağ IP/sn gibi saydam şifreleme ile tümleştirme çalışma zamanı çalışırken Bu kanal iletişimi yalnızca olduğundan güvenilen ağınızdaki sınırlı, size ek şifreleme gerekmeyebilir.
- Ne zaman tümleştirme çalışma zamanı bir üretim ortamında çalışmıyor. Bu, TLS/SSL sertifikası maliyet azaltmada yardımcı olabilir.


## <a name="monitor-a-multi-node-gateway"></a>İzleyici bir çok düğümlü ağ geçidi
### <a name="multi-node-gateway-monitoring"></a>Birden çok düğümlü ağ geçidi izleme
Azure portalında, ağ geçidi düğümleri durumları yanı sıra her bir düğümde neredeyse gerçek zamanlı anlık görüntüsünü kaynak kullanımı (CPU, bellek, network(in/out), vb.) görüntüleyebilirsiniz. 

![Veri Yönetimi ağ geçidi - birden çok düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Etkinleştirebilirsiniz **Gelişmiş ayarları** içinde **ağ geçidi** gibi gelişmiş ölçümlerini görmek için sayfayı **ağ**(in/out), **rol & kimlik bilgisi durum**, ağ geçidi sorunları, hata ayıklamaya yardımcı olduğu ve **eşzamanlı iş** (çalışan / sınırlamak) hangi olması değiştirilmiş / sırasında buna göre değişen performans ayarlama. Aşağıdaki tabloda yer alan sütun açıklanmakta **ağ geçidi düğümleri** listesi:  

İzleme özelliği | Açıklama
:------------------ | :---------- 
Ad | Ağ geçidi ile ilişkili düğümleri ve mantıksal ağ geçidi adı.  
Durum | Mantıksal ağ geçidi ve ağ geçidi düğümleri durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vs. Bu durumlar hakkında daha fazla bilgi için bkz: [ağ geçidi durumu](#gateway-status) bölümü. 
Sürüm | Mantıksal ağ geçidi ve her ağ geçidi düğümü sürümünü gösterir. Mantıksal ağ geçidi sürümü grubu düğüm çoğunluğu sürümüne göre belirlenir. Varsa düğümleri mantıksal ağ geçidi kurulumunda yalnızca mantıksal ağ geçidi işlevi aynı sürüm numarasına sahip farklı sürümleriyle düzgün. Başkalarının sınırlı modda ve (yalnızca otomatik güncelleştirmeler başarısız olursa) el ile güncelleştirilmesi gerekir. 
Kullanılabilir bellek | Bir ağ geçidi düğümü kullanılabilir bellek. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
CPU kullanımı | Bir ağ geçidi düğümünün CPU kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
Ağ (çıkış) | Bir ağ geçidi düğümünün ağ kullanımı. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. 
(Çalışan / sınırlamak) eşzamanlı işleri | İşlerini veya her bir düğümde çalışan görevlerin sayısıdır. Bu değer yakın gerçek zamanlı anlık görüntüsüdür. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değeri makine boyutuna göre tanımlanır. Gelişmiş senaryolarda eşzamanlı iş yürütme ölçeklendirin sınırı artırabilirsiniz burada CPU / bellek / ağ altında kullanılan ancak etkinlik zaman aşımı. Bu özellik, tek bir düğüm ile ağ geçidi (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin değilse bile) de kullanılabilir. Daha fazla bilgi için bkz: [ölçeklendirme konuları](#scale-considerations) bölümü. 
Rol | Dağıtıcı ve çalışan rolleri – iki tür vardır. Tüm düğümleri, tüm işleri yürütmek için kullanılabilmesi için başka bir deyişle, çalışanlardır. Görevler/işleri bulut hizmetlerinden çekmek ve bunları farklı çalışan düğümlerine (kendisi dahil) gönderme için kullanılan tek bir dağıtıcı düğüm yok. 

![Veri Yönetimi ağ geçidi - Gelişmiş birden çok düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Ağ geçidi durumu

Aşağıdaki tabloda, olası durumlar sağlayan bir **ağ geçidi düğümü**: 

Durum  | Yorumlar/senaryoları
:------- | :------------------
Çevrimiçi | Düğümü, veri fabrikası hizmetine bağlı.
Çevrimdışı | Çevrimdışı düğümdür.
Yükseltiliyor | Düğüm, otomatik olarak güncelleştirilir.
Sınırlı | Bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, hizmet veri yolu bağlantı sorunu veya kimlik bilgisi eşitleme sorunu nedeniyle olabilir. 
Devre dışı | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümdür.<br/><br/> Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir. 


Aşağıdaki tabloda, olası durumlar sağlayan bir **mantıksal ağ geçidi**. Ağ geçidi durumu üzerinde ağ geçidi düğümleri durumlar değişir. 

Durum | Yorumlar
:----- | :-------
Kaydedilmesi gerekiyor | Hiçbir düğümü için bu mantıksal ağ geçidi henüz kayıtlı değil
Çevrimiçi | Ağ geçidi düğümleri çevrimiçi
Çevrimdışı | Çevrimiçi durumu düğümü yok.
Sınırlı | Bu ağ geçidi tüm düğümler sağlıklı durumda. Bu durum bazı düğümü kapalı olabilir bir uyarıdır! <br/><br/>Dağıtıcı/çalışan düğümünde kimlik bilgisi eşitleme sorunu nedeniyle olabilir. 

### <a name="pipeline-activities-monitoring"></a>Ardışık Düzen / etkinliklerini izleme
Azure portal, izleme deneyimine ayrıntılı düğümü düzey ayrıntılara sahip bir işlem hattı sağlar. Örneğin, hangi etkinlikler hangi düğümde çalışan gösterir. Bu bilgiler, belirli bir düğümde performans sorunları anlamak yararlı olabilir, ağ azaltma nedeniyle söyleyin. 

![Veri Yönetimi ağ geçidi - birden çok düğüm işlem hatlarını izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Veri Yönetimi ağ geçidi - ardışık düzen ayrıntıları](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Ölçek konuları

### <a name="scale-out"></a>Ölçeği genişletme
Zaman **kullanılabilir bellek düşük** ve **CPU kullanımının yüksek olduğu**, makine genelinde yardımcı yük genişletme yeni bir düğüm ekleme. Etkinlik zaman aşımı veya ağ geçidi düğümü çevrimdışı olmasından dolayı başarısız oluyorsa, ağ geçidi için bir düğüm eklerseniz yardımcı olur.
 
### <a name="scale-up"></a>Ölçeği artırma
Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak boşta kapasitesi 0 ise, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Ağ geçidi aşırı yüklendiği için etkinlikler zaman aşımına uğruyor zaman ölçeği isteyebilirsiniz. Aşağıdaki görüntüde gösterildiği gibi bir düğüm için maksimum kapasiteyi artırabilir. İle başlamak Katlama öneririz.  

![Veri Yönetimi ağ geçidi - ölçek konuları](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Bilinen sorunlar/sonu değiştirir

- Şu anda, tek bir mantıksal ağ geçidi için en fazla dört fiziksel ağ geçidi düğümleri bulunabilir. Performansı artırmak için dörtten fazla düğüm gerekirse, e-posta Gönder [ DMGHelp@microsoft.com ](mailto:DMGHelp@microsoft.com).
- Bir ağ geçidi düğümü geçerli mantıksal ağ geçidi'nden geçiş yapmak için başka bir mantıksal ağ geçidi kimlik doğrulaması anahtarla yeniden kaydedilemiyor. Yeniden kaydetmek için ağ geçidi düğümden kaldırmak, ağ geçidini yeniden yükleyin ve diğer mantıksal ağ geçidi için kimlik doğrulama anahtarı ile kaydeder. 
- Tüm ağ geçidi düğümleri için HTTP proxy gerekiyorsa, proxy diahost.exe.config ve diawp.exe.config ayarlayın ve tüm düğümleri aynı diahost.exe.config ve diawip.exe.config emin olmak için Sunucu Yöneticisi'ni kullanın. Bkz: [proxy ayarlarını yapılandırma](data-factory-data-management-gateway.md#configure-proxy-server-settings) ayrıntıları bölümü. 
- Ağ geçidi Yapılandırma Yöneticisi'nde düğümü düğümü iletişimi için şifreleme modunu değiştirmek için portal dışındaki tüm düğümleri silin. Ardından düğümler şifreleme modu geri değiştirdikten sonra ekleyin.
- Düğümü düğümü iletişim kanalını şifrelemek isterseniz resmi bir SSL sertifikası kullanın. Aynı sertifikayı diğer makinelere sertifika yetkilisi listesinin içinde güvenilmiyor gibi otomatik olarak imzalanan sertifika bağlantı sorunlarına neden olabilir. 
- Düğüm sürüm mantıksal ağ geçidi sürümden daha düşük olduğunda bir mantıksal ağ geçidi için ağ geçidi düğümü kaydedilemiyor. Mantıksal ağ geçidi'nin tüm düğümleri portalından silmeniz daha düşük bir sürüm node(downgrade) kaydedebilirsiniz. Mantıksal bir ağ geçidi'nin tüm düğümleri silerseniz, el ile yükleyin ve yeni düğümler bu mantıksal ağ geçidine kaydedin. Hızlı Kurulum, bu durumda desteklenmiyor.
- Düğümleri hala bulut kimlik bilgilerini kullanan mevcut bir mantıksal ağ geçidi için yüklemek için hızlı kurulum kullanamazsınız. Kimlik bilgilerini ayarlar sekmesinde ağ geçidi Yapılandırma Yöneticisi'nden depolandığı kontrol edebilirsiniz.
- Düğümleri düğümü düğümü şifreleme etkin olan mevcut bir mantıksal ağ geçidi için yüklemek için hızlı kurulum kullanamazsınız. Şifreleme modu ayarı sertifikaları el ile ekleyerek içerir gibi hızlı yükleme hiçbir daha fazla bir seçenektir. 
- Şirket içi ortamından bir dosya kopyalama için kullanılamaz \\localhost veya artık bu yana localhost veya yerel sürücü C:\files olabilir tüm düğümler erişilebilir. Bunun yerine, kullanın \\ServerName\files dosyalarının konumunu belirtin.


## <a name="rolling-back-from-the-preview"></a>Önizlemesi'nden geri alınıyor 
Önizlemesi'nden geri almak için bir düğüm dışındaki tüm düğümleri silin. Silme, ancak mantıksal ağ geçidi en azından bir düğümün sahip olduğundan emin olun hangi düğümlerin önemli değildir. Bir düğüm makinedeki ağ geçidi kaldırma veya Azure portalını kullanarak silebilirsiniz. Azure portalında içinde **Data Factory** sayfasında, bağlı hizmetleri başlatmak için **bağlantılı Hizmetleri** sayfası. Başlatmak için ağ geçidi seçin **ağ geçidi** sayfası. Ağ geçidi sayfasında, ağ geçidi ile ilişkili düğümleri görebilirsiniz. Sayfa bir düğüm ağ geçidi'nden silmenize olanak sağlar.
 
Sildikten sonra tıklatın **Önizleme özellikleri** aynı Azure portal sayfası ve önizleme özelliğini devre dışı bırakın. Bir düğüm GA (genel kullanılabilirlik) ağ geçidi için ağ geçidiniz sıfırladınız.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri gözden geçirin:
- [Veri Yönetimi ağ geçidi](data-factory-data-management-gateway.md) -ağ geçidi ayrıntılı bir genel bakış sağlar.
- [Şirket içi ve bulut arasında veri taşıma veri depolarına](data-factory-move-data-between-onprem-and-cloud.md) -bir ağ geçidi ile tek bir düğüm kullanmak için adım adım yönergeleri içeren bir kılavuz içerir. 
