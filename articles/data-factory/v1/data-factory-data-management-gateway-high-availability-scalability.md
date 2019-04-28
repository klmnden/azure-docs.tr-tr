---
title: Azure Data Factory veri yönetimi ağ geçidi ile yüksek kullanılabilirlik | Microsoft Docs
description: Bu makalede, daha fazla düğüm ve ölçek ekleyerek veri yönetimi ağ geçidi nasıl ölçeklendirebilirsiniz açıklanmaktadır yukarı bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 08e7341bfd1c384e41e6d3f1bd7810552899849a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60488943"
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Veri Yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik (Önizleme)
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [barındırılan tümleştirme çalışma zamanını içinde](../create-self-hosted-integration-runtime.md). 


Bu makale, veri yönetimi ağ geçidi ile yüksek kullanılabilirlik ve ölçeklenebilirlik çözümünüzü yapılandırmanıza yardımcı olur. / tümleştirme.    

> [!NOTE]
> Bu makalede, zaten Integration Runtime (önceki veri yönetimi ağ geçidi) ilişkin temel bilgileri ile ilgili bilgi sahibi olduğunuz varsayılır. Emin değilseniz, bkz. [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).
> 
> **Bu önizleme özelliğini veri yönetimi ağ geçidi sürümü 2.12.xxxx.x ve yukarıdaki resmi olarak desteklenen**. Sürüm 2.12.xxxx.x kullandığınızdan emin olun veya üzeri. Veri Yönetimi ağ geçidi'nin son sürümünü indirin [burada](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Genel Bakış
Tek bir mantıksal ağ geçidi portalından ile birden çok şirket içi makinede yüklü veri yönetimi ağ geçidi ilişkilendirebilirsiniz. Bu makineler adlı **düğümleri**. En fazla olabilir **dört düğüm** mantıksal bir ağ geçidi ile ilişkili. Mantıksal bir ağ geçidi için birden çok düğüm (yüklü ağ geçidi ile şirket içi makineler) sahip avantajları şunlardır:  

- Şirket içi ve bulut arasında veri taşıma performansını veri depoları.  
- Herhangi bir nedenden dolayı düğümlerinden biri arıza yaparsa, diğer düğümlere veri taşımak için hala kullanılabilir. 
- Düğümlerden biri bakım için çevrimdışı yapılması gerekiyorsa, diğer düğümler veri taşımak için yine kullanılabilir durumdadır.

Sayısını da yapılandırabilirsiniz **eş zamanlı veri taşıma işleri** şirket içi ve bulut arasında veri taşıma yeteneği artırabileceğinizi düğümünde çalıştırabilirsiniz veri depoları. 

Azure portalını kullanarak, bir düğüm mantıksal ağ geçidini ekleyip karar vermenize yardımcı olan bu düğümler durumunu izleyebilirsiniz. 

## <a name="architecture"></a>Mimari 
Aşağıdaki diyagramda mimari genel bakış ölçeklenebilirlik ve kullanılabilirlik özelliği veri yönetimi ağ geçidi sağlar: 

![Veri Yönetimi ağ geçidi - yüksek kullanılabilirlik ve ölçeklenebilirlik](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

A **mantıksal ağ geçidi** eklediğiniz Azure portalında bir data factory'ye geçididir. Daha önce tek bir şirket içi Windows makineye veri yönetimi ağ geçidi yüklü mantıksal bir ağ geçidi ile ilişkilendirebiliyordunuz. Bu ağ geçidi makinesi vm'lere düğüm denir şirket içi. En fazla şimdi ilişkilendirebilirsiniz **dört fiziksel düğüm** mantıksal bir ağ geçidi ile. Birden fazla düğüme sahip mantıksal bir ağ geçidi adı verilen bir **çok düğümlü ağ geçidi**.  

Bu düğümler **etkin**. Tüm şirket içi ile bulut arasında veri taşımak için veri taşıma işleri işleyebileceği veri depoları. Düğümlerden biri hem gönderen hem de çalışan davranır. Diğer düğümleri gruplar halinde çalışan düğümlerdir. A **dağıtıcı** düğümü, veri taşıma görevleri/işleri bulut hizmetinden çeker ve bunları (kendisi dahil) çalışan düğümlerine dağıtır. A **çalışan** düğümü, şirket içi ile bulut arasında veri taşımak için veri taşıma işleri yürütür veri depoları. Tüm düğümleri çalışanlardır. Yalnızca bir düğüm, hem gönderme hem de çalışan olabilir.    

Genellikle, bir düğüm ile başlayabilir ve **ölçeğini** mevcut düğüm ile veri taşıma yük olmamızdır gibi daha fazla düğüm eklemek için. Ayrıca **ölçeği** düğüm üzerinde çalışmasına izin verilen eşzamanlı iş sayısını artırarak bir ağ geçidi düğümü veri taşıma yeteneğini. Bu özellik, bir tek düğümlü ağ geçidi ile (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin olduğunda bile) de kullanılabilir. 

Birden fazla düğüme sahip bir ağ geçidi veri deposu kimlik bilgileri tüm düğümlerde uyumlu kalmasını sağlar. Bir düğümden düğüme bağlantı sorunu yoksa, kimlik bilgilerini eşitlenmemiş olabilir. Bir ağ geçidi kullanan bir şirket içi veri deposu için kimlik bilgilerini ayarladığınızda, dağıtıcı/çalışan düğümüyle kimlik bilgilerini kaydeder. Diğer çalışan düğümü ile dağıtıcı düğüm eşitler. Bu işlem olarak bilinir **kimlik bilgilerini eşitleme**. Düğümler arasında iletişim kanalını olabilir **şifrelenmiş** ortak SSL/TLS sertifikası tarafından. 

## <a name="set-up-a-multi-node-gateway"></a>Bir çok düğümlü ağ geçidi ayarlama
Bu bölümde, bu makaleler kavramları biliyorsanız ve aşağıdaki iki makaleler aracılığıyla ilerlemiş varsayılır: 

- [Veri Yönetimi ağ geçidi](data-factory-data-management-gateway.md) -ağ geçidi ayrıntılı bir bakış sağlar.
- [Şirket içi ile bulut arasında veri taşıma veri depoları](data-factory-move-data-between-onprem-and-cloud.md) -bir ağ geçidi tek bir düğüm olarak kullanmak için adım adım yönergeler içeren bir kılavuz içerir.  

> [!NOTE]
> Bir şirket içi Windows makinesinde veri yönetimi ağ geçidi yüklemeden önce listelenen önkoşullara bakın [ana makalede](data-factory-data-management-gateway.md#prerequisites).

1. İçinde [izlenecek](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), mantıksal bir ağ geçidi oluşturulurken, etkinleştirme **yüksek kullanılabilirlik ve ölçeklenebilirlik** özelliği. 

    ![Veri Yönetimi ağ geçidi - etkin yüksek kullanılabilirlik ve ölçeklenebilirlik](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. İçinde **yapılandırma** sayfasında, ya da kullanın **hızlı kurulumunu** veya **el ile Kurulum** ilk düğümü (bir şirket içi Windows makinesi) bir ağ geçidi yüklemek için bağlantı.

    ![Veri Yönetimi ağ geçidi - express veya el ile Kurulum](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Hızlı kurulum seçeneğini kullanırsanız, düğümden düğüme iletişim şifreleme olmadan gerçekleştirilir. Düğüm adı, makine adı olarak aynıdır. El ile Kurulum, düğümden düğüme iletişimi şifrelenmesi gerekiyor veya tercih ettiğiniz bir düğüm adı belirtmek istiyorsanız kullanın. Düğüm adları daha sonra düzenlenemez.
3. Seçerseniz **hızlı kurulum**
    1. Ağ geçidi başarıyla yüklendikten sonra aşağıdaki iletiyi görürsünüz:

        ![Veri Yönetimi ağ geçidi - hızlı kurulum başarılı](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. İzleyerek veri yönetimi Configuration Manager ağ geçidi için başlatma [bu yönergeleri](data-factory-data-management-gateway.md#configuration-manager). Ağ geçidi adı, düğüm adı, durum, vb. görürsünüz.

        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Seçerseniz **el ile Kurulum**:
    1. Makinenizde ağ geçidi yüklemek için Microsoft Download Center yükleme paketini indirin.
    2. Kullanım **kimlik doğrulama anahtarı** gelen **yapılandırma** sayfasında ağ geçidini kaydetmek için.
    
        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. İçinde **yeni ağ geçidi düğümü** sayfasında, özel bir sağlayabilir **adı** ağ geçidi düğümü. Varsayılan olarak, düğüm adı, makine adı olarak aynıdır.    

        ![Veri Yönetimi ağ geçidi - ad belirtin](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. Sonraki sayfada, seçtiğiniz kullanılıp kullanılmayacağını **düğüm-düğüm iletişimi için şifrelemeyi etkinleştirme**. Tıklayın **atla** (varsayılan) şifreleme devre dışı bırakmak için.

        ![Veri Yönetimi ağ geçidi - şifrelemeyi etkinleştirme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Şifreleme modunu değiştirmek, yalnızca tek bir ağ geçidi düğümü mantıksal ağ geçidinde olduğunda desteklenir. Bir ağ geçidi birden fazla düğüme sahip olduğunda şifreleme modunu değiştirmek için aşağıdaki adımları uygulayın: tek bir düğüm dışında tüm düğümleri silmek, şifreleme modunu değiştirin ve sonra düğümleri yeniden ekleyin.
        > 
        > Bkz: [TLS/SSL sertifikası gereksinimleri](#tlsssl-certificate-requirements) bölümünde bir liste için bir TLS/SSL sertifikası kullanma gereksinimleri. 
    5. Ağ geçidi başarıyla yüklendikten sonra Configuration Manager'ı Başlat'a tıklayın:
    
        ![El ile Kurulum - Yapılandırma Yöneticisi'ni başlatma](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. bağlantı durumu gösteren, düğüme (şirket içinde Windows makinesi) veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni gördüğünüz **ağ geçidi adı**, ve **düğüm adı**.  

        ![Veri Yönetimi ağ geçidi - yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Bir Azure sanal ağ geçidi sağlıyorsanız, kullanabileceğiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-mutiple-vms-with-data-management-gateway). Bu betik, mantıksal bir ağ geçidi oluşturur, yazılımı yüklü veri yönetimi ağ geçidi sanal makinelerini ayarlar ve bunları mantıksal ağ geçidi ile kaydeder. 
6. Azure portalında başlatma **ağ geçidi** sayfası: 
    1. Portalında data factory giriş sayfasında tıklayın **bağlı hizmetler**.
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. seçin **ağ geçidi** görmek için **ağ geçidi** sayfası:
    
        ![Data factory giriş sayfası](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. Gördüğünüz **ağ geçidi** sayfası:   

        ![Ağ geçidi ile tek bir düğüm görünümü](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Tıklayın **düğümü Ekle** mantıksal ağ geçidine bir düğüm eklemek için araç çubuğunda. Hızlı Kurulum kullanmayı planlıyorsanız, ağ geçidi düğümü olarak eklenecek şirket içi makineden bu adımı uygulayın. 

    ![Veri Yönetimi ağ geçidi - düğüm menü ekleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. İlk düğümü ayarlama adımları benzerdir. Configuration Manager kullanıcı arabirimini el ile yükleme seçeneğini belirlerseniz, düğüm adı ayarlamanızı sağlar: 

    ![Configuration Manager - ikinci ağ geçidi yükleme](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Ağ geçidi düğümüne başarıyla yüklendikten sonra Configuration Manager aracı aşağıdaki ekranda görüntüler:  

    ![Configuration Manager - ikinci gateway yükleme başarılı](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Açarsanız **ağ geçidi** sayfasını portalda iki ağ geçidi düğümleri göreceksiniz: 

    ![Portalda iki düğüm ile ağ geçidi](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. Bir ağ geçidi düğümü silmek için tıklayın **düğümü Sil** araç çubuğunda silin ve ardından istediğiniz düğümü seçin **Sil** araç çubuğundan. Bu eylem, seçili düğüm grubundan siler. Bu eylem düğümden (şirket içinde Windows makinesi) veri yönetimi ağ geçidi yazılımının kaldırmaz unutmayın. Kullanım **Program Ekle veya Kaldır** Denetim Masası şirket içi ağ geçidini kaldırın. Ağ geçidi düğümü kaldırdığınızda, portalda otomatik olarak silinir.   

## <a name="upgrade-an-existing-gateway"></a>Var olan bir ağ geçidini Yükselt
Yüksek kullanılabilirlik ve ölçeklenebilirlik özelliğini kullanmak için mevcut bir ağ geçidi yükseltme yapabilirsiniz. Bu özellik yalnızca veri yönetimi ağ geçidi sürümü olan düğümleri ile çalışır > 2.12.xxxx =. Veri Yönetimi ağ geçidi bir makinede yüklü sürümü gördüğünüz **yardımcı** sekmesi, veri yönetimi ağ geçidi Yapılandırma Yöneticisi. 

1. Ağ geçidini şirket içi makineye, indirerek aşağıdaki tarafından en son sürüme güncelleştirin ve çalışan bir MSI paketi Kurulum [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Bkz: [yükleme](data-factory-data-management-gateway.md#installation) ayrıntıları bölümü.  
2. Azure portalına gidin. Başlatma **Data Factory sayfasını** veri fabrikanızın. Bağlı hizmetler kutucuğunda başlatmak için tıklatın **bağlı hizmetler sayfasında**. Başlatmak için ağ geçidini seçin **ağ geçidi sayfasında**. ' A tıklayın ve etkinleştirme **önizleme özelliği** aşağıdaki görüntüde gösterildiği gibi: 

    ![Veri Yönetimi ağ geçidi - önizleme özelliğini etkinleştirme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Önizleme özelliği portalda etkinleştirildikten sonra tüm sayfaları kapatın. Yeniden **ağ geçidi sayfasında** yeni önizleme kullanıcı arabirimini (UI) görmek için.
 
    ![Veri Yönetimi ağ geçidi - enable önizleme özelliğini başarı](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Veri Yönetimi ağ geçidi - kullanıcı Arabirimi Önizleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Yükseltme sırasında ilk düğümünün adı makinenin adıdır. 
3. Şimdi, bir düğüm ekleyin. İçinde **ağ geçidi** sayfasında **düğümü Ekle**.  

    ![Veri Yönetimi ağ geçidi - düğüm menü ekleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Önceki bölümde düğümü ayarlamak için yönergeleri izleyin. 

### <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler

- Böylece makine olmayan hazırda bekleme ağ geçidi için konak makinedeki güç planı yapılandırmak. Konak makine hazırda bekleme, ağ geçidi veri isteklere yanıt vermez.
- Ağ geçidi ile ilişkili sertifika yedekleyin.
- Tüm düğümleri (ideal performans için önerilir) benzer yapılandırmasının olduğundan emin olun. 
- Yüksek kullanılabilirlik sağlamak için en az iki düğüm ekleyin.  

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifikası gereksinimleri
Çalışma zamanı düğümleri tümleştirme arasındaki iletişimin güvenliğini sağlamak için kullanılan TLS/SSL sertifika için gereksinimler şunlardır:

- Sertifika genel olarak güvenilir X509 olmalıdır v3 sertifikası. Ortak (üçüncü taraf) sertifika yetkilisi (CA) tarafından verilen sertifikaların kullanmanızı öneririz.
- Her Integration runtime düğümü, kimlik bilgileri Yöneticisi uygulaması çalıştıran istemci makine yanı sıra, bu sertifikaya güvenmesi gerekir. 
  > [!NOTE]
  > Kimlik bilgileri Yöneticisi uygulamasını güvenli bir şekilde Kopyalama Sihirbazı'ndan kimlik bilgisi ayarlanırken kullanılır / Azure portalı. Ve bu, şirket içi aynı ağ içinde herhangi bir makineden tetiklenme / özel veri deposu.
- Joker karakterli sertifikalar desteklenir. FQDN adınız ise **node1.domain.contoso.com**, kullanabileceğiniz ***. domain.contoso.com** sertifikanın konu adı olarak.
- SAN sertifika konu diğer adları yalnızca son maddenin kullanılacak ve diğer tüm mevcut sınırlama nedeniyle yoksayılacak önerilmez. Örneğin bir SAN sertifikası, SAN olan sahip **node1.domain.contoso.com** ve **node2.domain.contoso.com**, yalnızca bu sertifika, FQDN: makinede kullanabilirsiniz **node2.domain.contoso.com**.
- SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutu destekler.
- CNG kullanarak sertifika anahtarlar desteklenmez.

#### <a name="faq-when-would-i-not-enable-this-encryption"></a>SSS: Ne zaman bu şifreleme etkinleştirebilirim değil mi?
Şifrelemeyi etkinleştirme ekleyebileceğiniz bazı altyapınızı (sahip ortak sertifika) bu nedenle maliyet şifrelemeyi etkinleştirme atlayabilirsiniz aşağıda durumları:
- Tümleştirme çalışma zamanı güvenilir bir ağ veya saydam şifreleme gibi IP/sn ile ağ üzerinde çalışırken Bu kanal iletişimi yalnızca olduğundan güvenilen ağınızda sınırlı, size ek şifreleme gerekmeyebilir.
- Tümleştirme çalışma zamanının ne zaman bir üretim ortamında çalışmıyor. Bu, TLS/SSL sertifikası maliyet azaltmada yardımcı olabilir.


## <a name="monitor-a-multi-node-gateway"></a>İzleyici bir çok düğümlü ağ geçidi
### <a name="multi-node-gateway-monitoring"></a>Çok düğümlü ağ geçidi izleme
Azure portalında neredeyse gerçek zamanlı anlık görüntü kaynak kullanımı (CPU, bellek, network(in/out), vb.) ağ geçidi düğümleri durumları ile birlikte her bir düğümünde görüntüleyebilirsiniz. 

![Veri Yönetimi ağ geçidi - birden çok düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Etkinleştirebilirsiniz **Gelişmiş ayarları** içinde **ağ geçidi** gibi gelişmiş ölçümler görmek için sayfayı **ağ**(iç/dış), **rol & kimlik bilgisi durumu**, ağ geçidiyle ilgili sorunları, hata ayıklamaya yardımcı olan ve **eşzamanlı iş** (çalışan / sınırlama) olduğu olması değiştirilmiş / sırasında buna göre değişen performans ayarlama. Aşağıdaki tabloda yer alan sütun açıklanmakta **ağ geçidi düğümleri** listesi:  

İzleme özelliği | Açıklama
:------------------ | :---------- 
Ad | Ağ geçidi ile ilişkili düğümleri ve mantıksal ağ geçidi adı.  
Durum | Mantıksal ağ geçidini ve ağ geçidi düğümleri durumu. Örnek: Çevrimiçi/çevrimdışı/sınırlı/vb. Bu durumlar hakkında daha fazla bilgi için bkz. [ağ geçidi durumu](#gateway-status) bölümü. 
Version | Mantıksal ağ geçidi her ağ geçidi düğümü ve sürümü gösterir. Mantıksal ağ geçidi sürümünü grubunda düğüm çoğunluğu sürümüne göre belirlenir. Düğüm varsa, yalnızca mantıksal ağ geçidi işlevi olarak aynı sürüm numarasına sahip mantıksal bir ağ geçidi kurulumunu farklı sürümleriyle düzgün. Başkalarının sınırlı modundaki ve (yalnızca otomatik güncelleştirme başarısız olursa) el ile güncelleştirilmesi gerekir. 
Uygun bellek | Bir ağ geçidi düğümü kullanılabilir bellek. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. 
CPU kullanımı | Bir ağ geçidi düğümü, CPU kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. 
Ağ (daraltma/genişletme) | Bir ağ geçidi düğümü, ağ kullanımı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. 
(Çalışan / sınırlama) eşzamanlı işleri | İşleri veya her bir düğümde çalışan görevler sayısı. Bu değer, neredeyse gerçek zamanlı anlık görüntüsüdür. Her düğüm için en fazla eşzamanlı iş sınırı belirtir. Bu değer, makine boyutuna bağlı olarak tanımlanır. Eş zamanlı iş yürütme Gelişmiş senaryolarda artırabileceğinizi limiti artırabilirsiniz burada CPU / bellek / ağ altında kullanılan ancak etkinlikler zaman aşımına uğruyor. Bu özellik, bir tek düğümlü ağ geçidi ile (ölçeklenebilirlik ve kullanılabilirlik özelliği etkin olduğunda bile) de kullanılabilir. Daha fazla bilgi için [ölçeklendirme konuları](#scale-considerations) bölümü. 
Rol | Dağıtıcı ve çalışan rolleri – iki tür vardır. Tüm düğümleri, yani tüm işleri yürütmek için kullanılabilirler çalışanlardır. Görevler/işleri bulut hizmetlerinden çekme ve bunları farklı çalışan düğümlerine (kendisi dahil) dağıtmak için kullanılan tek bir dağıtıcı düğüm yok. 

![Veri Yönetimi ağ geçidi - Gelişmiş birden çok düğüm izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Ağ geçidi durumu

Aşağıdaki tabloda, olası durumlar sağlayan bir **ağ geçidi düğümü**: 

Durum  | Yorumlar/senaryoları
:------- | :------------------
Çevrimiçi | Düğüm, Data Factory hizmetine bağlı.
Çevrimdışı | Düğümü çevrimdışı durumda.
Yükseltiliyor | Düğüm, otomatik olarak güncelleştirilir.
Sınırlı | Bağlantı sorunundan kaynaklanıyor. HTTP bağlantı noktası 8050 sorunu, service bus bağlantı sorunu veya kimlik bilgileri eşitleme sorunu nedeniyle olabilir. 
Etkin Değil | Diğer Çoğunluk düğüm yapılandırmasından farklı bir yapılandırmada düğümüdür.<br/><br/> Diğer düğümlere bağlanamadığında bir düğüm etkin olabilir. 


Aşağıdaki tabloda, olası durumlar sağlayan bir **mantıksal ağ geçidi**. Ağ geçidi durumu, ağ geçidi düğümleri durumlar üzerinde bağlıdır. 

Durum | Yorumlar
:----- | :-------
Kayıt gerekiyor | Bu mantıksal ağ geçidi için hiçbir düğüm henüz kayıtlı
Çevrimiçi | Ağ geçidi düğümleri çevrimiçi
Çevrimdışı | Çevrimiçi durumu düğümü yok.
Sınırlı | Bu ağ geçidi olarak tüm düğümleri sağlıklı durumda olur. Bu durum bazı düğümü kapalı olabilir bir uyarıdır! <br/><br/>Dağıtıcı/çalışan düğümüyle kimlik eşitleme sorunu nedeniyle olabilir. 

### <a name="pipeline-activities-monitoring"></a>İşlem hattı / izleme etkinlikleri
Azure portalında izleme deneyimini ayrıntılı düğüm düzeyi ayrıntıları ile bir işlem hattı sağlar. Örneğin, hangi etkinliklerinin hangi düğümde çalışan gösterir. Bu bilgiler, belirli bir düğüm üzerinde performans sorunlarını anlama konusunda, ağ kapasitesi azaltıldığı varsayalım. 

![Veri Yönetimi ağ geçidi - birden çok düğüm için işlem hatlarını izleme](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Veri Yönetimi ağ geçidi - işlem hattı ayrıntıları](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Ölçek konuları

### <a name="scale-out"></a>Ölçeği genişletme
Zaman **kullanılabilir bellek düşük** ve **CPU kullanımının yüksek olduğu**, yeni bir düğüm ekleme, makineler arasında yük ölçeğinizi yardımcı. Etkinlik zaman aşımı veya ağ geçidi düğümü çevrimdışı olmasından dolayı başarısız oluyorsa ağ geçidine bir düğüm eklerseniz yardımcı olur.
 
### <a name="scale-up"></a>Ölçeği artırma
Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak boş kapasiteyi 0 ise, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Ağ geçidinin aşırı yüklendiği etkinlikler zaman aşımına uğruyor. zaman ölçeği isteyebilirsiniz. Aşağıdaki görüntüde gösterildiği gibi bir düğüm için kapasite üst sınırı artırabilirsiniz. Başlamak Katlama öneririz.  

![Veri Yönetimi ağ geçidi - ölçek konuları](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Bilinen sorunları/bozucu değişiklikler

- Şu anda tek bir mantıksal ağ geçidi için en fazla dört fiziksel ağ geçidi düğümleri olabilir. Performansla ilgili nedenlerden dolayı dörtten fazla düğüm gerekirse, e-posta Gönder [ DMGHelp@microsoft.com ](mailto:DMGHelp@microsoft.com).
- Bir ağ geçidi düğümü geçerli mantıksal ağ geçidini değiştirmek için başka bir mantıksal ağ geçidi kimlik doğrulama anahtarı ile yeniden kaydedilemez. Yeniden kaydolmak için ağ geçidi düğümü kaldırmak, ağ geçidini yeniden yükleyin ve diğer mantıksal ağ geçidi için kimlik doğrulama anahtarı ile kaydedin. 
- Tüm ağ geçidi düğümleri için HTTP Proxy'si gerekliyse, proxy diahost.exe.config ve diawp.exe.config ayarlayın ve tüm düğümleri aynı diahost.exe.config ve diawip.exe.config emin olmak için Sunucu Yöneticisi'ni kullanın. Bkz: [proxy ayarlarını yapılandırma](data-factory-data-management-gateway.md#configure-proxy-server-settings) ayrıntıları bölümü. 
- Düğümden düğüme iletişimi Ağ Geçidi Yapılandırma Yöneticisi'ndeki şifreleme modunu değiştirmek için portal biri dışında tüm düğümleri silin. Ardından, şifreleme modu geri değiştirdikten sonra düğümleri ekleyin.
- Düğümden düğüme iletişim kanalını şifrelemek isterseniz resmi bir SSL sertifikası kullanın. Aynı sertifikayı diğer makinelere sertifika yetkilisi listesinin içinde güvenilmiyor gibi otomatik olarak imzalanan sertifika bağlantı sorunlarına neden olabilir. 
- Düğüm sürümünün mantıksal ağ geçidi sürümünden daha düşük olduğunda mantıksal bir ağ geçidi için bir ağ geçidi düğümü kaydedilemiyor. Daha düşük bir sürüm node(downgrade) kaydedebilmesi portalından mantıksal ağ geçidinin tüm düğümleri silin. Mantıksal bir ağ geçidi tüm düğümleri silerseniz, el ile yükleyin ve bu mantıksal ağ geçidi için yeni düğümler kaydedin. Hızlı Kurulum, bu durumda desteklenmiyor.
- Hızlı Kurulum hala bulut kimlik bilgilerini kullanarak bir var olan mantıksal ağ geçidi için düğümleri yüklemek için kullanamazsınız. Kimlik bilgilerinin depolandığı Ayarlar sekmesinde ağ geçidi Yapılandırma Yöneticisi'nden kontrol edebilirsiniz.
- Hızlı Kurulum, düğümden düğüme şifrelemesi etkinleştirilmiş bir var olan mantıksal ağ geçidi için düğümleri yüklemek için kullanamazsınız. Şifreleme modu ayarlamak, sertifikaları el ile eklemeyi gerektirir gibi hızlı yükleme hiçbir daha fazla bir seçenektir. 
- Şirket içi ortamdan bir dosya kopyalama için kullanmamalısınız \\localhost veya artık localhost veya yerel sürücüde beri C:\files olmayabilir tüm düğümler erişilebilir. Bunun yerine, \\ServerName\files dosyaların konumunu belirtmek için.


## <a name="rolling-back-from-the-preview"></a>Önizleme geri alınıyor 
Önizleme geri almak için bir düğüm ancak tüm düğümleri silin. Hangi düğümleri silin, ancak mantıksal ağ en az bir düğüme sahip olun önemi yoktur. Ağ geçidi makinesinde kaldırma veya Azure portalını kullanarak bir düğümü silebilirsiniz. Azure portalında, **Data Factory** başlatmak için bağlı hizmetler sayfasında, **bağlı hizmetler** sayfası. Başlatmak için ağ geçidini seçin **ağ geçidi** sayfası. Ağ geçidi sayfasında, ağ geçidi ile ilişkili düğümleri görebilirsiniz. Sayfa ağ geçidini, bir düğüm silmenize olanak sağlar.
 
Sildikten sonra tıklayın **Önizleme özellikleri** aynı Azure portal sayfasındaki ve önizleme özelliğini devre dışı bırakın. Bir düğüm GA (Genel kullanım) ağ geçidi için ağ geçidinizi sıfırlamadan.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri inceleyin:
- [Veri Yönetimi ağ geçidi](data-factory-data-management-gateway.md) -ağ geçidi ayrıntılı bir bakış sağlar.
- [Şirket içi ile bulut arasında veri taşıma veri depoları](data-factory-move-data-between-onprem-and-cloud.md) -bir ağ geçidi tek bir düğüm olarak kullanmak için adım adım yönergeler içeren bir kılavuz içerir. 
