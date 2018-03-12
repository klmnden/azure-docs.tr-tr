---
title: "Kendini barındıran tümleştirmesi çalışma zamanı Azure Data Factory oluşturma | Microsoft Docs"
description: "Veri fabrikaları özel bir ağda veri depolarına erişmesini sağlayan Azure veri fabrikası'nda kendi kendini barındıran tümleştirmesi çalışma zamanı oluşturmayı öğrenin."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 3f1b55f2752821de447e6c03bcbf79f01d9f8264
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Oluşturma ve Self-hosted tümleştirmesi çalışma zamanı yapılandırma
Tümleştirme çalışma zamanı (IR), farklı ağ ortamlar genelinde veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır. IR hakkında daha fazla ayrıntı için bkz: [tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md).

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

Kendini barındıran tümleştirmesi çalışma zamanı kopyalama etkinlikler arasında bulut veri çalıştırabilen depoları ve özel ağ ve dönüştürme etkinlikleri göndermeyi veri deposunda işlem kaynaklarını bir şirket içi veya Azure sanal ağı. Bir şirket içi makinede veya özel bir ağ içindeki bir sanal makine kendini barındıran tümleştirme çalışma zamanı gereksinimlerini yükleyin.  

Bu belge nasıl oluşturma ve Self-hosted IR yapılandırma sunar

## <a name="high-level-steps-to-install-self-hosted-ir"></a>Kendini barındıran IR yüklemek için üst düzey adımları
1.  Kendini barındıran tümleştirmesi çalışma zamanı oluşturun. Bir PowerShell örneği aşağıdadır:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2.  Kendini barındıran tümleştirmesi çalışma zamanı (yerel makinedeki) yükleyip yeniden açın.
3.  Kimlik doğrulama anahtarı almak ve kendi kendini barındıran tümleştirmesi çalışma zamanı anahtarla kaydedin. Bir PowerShell örneği aşağıdadır:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="command-flow-and-data-flow"></a>Komut akışının ve veri akışı
Etkinlik bir kendi kendini barındıran tümleştirmesi çalışma zamanı verileri şirket içi ve bulut arasında taşıdığınızda, bulut tersi için şirket içi veri kaynağından verileri aktarmak için kullanır.

Üst düzey veri akışı özeti için kendi kendini barındıran IR kopyayla adımları şöyledir:

![Yüksek düzey genel bakış](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Veri geliştirici, kendi kendini barındıran tümleştirmesi çalışma zamanı PowerShell cmdlet'ini kullanarak Azure data factory içinde oluşturur. Şu anda Azure portalı, bu özelliği desteklemez.
2. Veri Geliştirici veri depolarına bağlanmak için kullanması gereken kendini barındıran tümleştirmesi çalışma zamanı örneği belirterek bir şirket içi veri deposu için bağlı hizmet oluşturur. Bağlantılı hizmet kurulumunun bir parçası olarak, veri Geliştirici ' kimlik bilgileri Yöneticisi ' uygulama kullanır (şu anda desteklenmiyor) kimlik doğrulama türleri ve kimlik bilgilerini ayarlamak için. Kimlik bilgileri Yöneticisi uygulama iletişim bağlantı ve kimlik bilgilerini kaydetmek için kendi kendini barındıran tümleştirmesi çalışma zamanı test etmek için veri deposuyla iletişim kurar.
4.  Kendini barındıran tümleştirmesi çalışma zamanı düğümü Windows Data Protection uygulama programlama arabirimi (DPAPI) kullanarak kimlik bilgilerini şifreler ve yerel olarak kaydeder. Yüksek kullanılabilirlik için birden çok düğüm ayarlarsanız, kimlik bilgileri daha fazla diğer düğümleri arasında eşitlenir. Her düğüm DPAPI kullanarak şifreler ve yerel olarak depolar. Kimlik bilgisi eşitleme veri geliştiriciler için saydamdır ve kendi kendini barındıran IR tarafından işlenir    
5.  Data Factory Hizmeti'ne zamanlama & işlerin Yönetim için kendi kendini barındıran tümleştirme çalışma zamanı ile iletişim kurar **denetim kanalı** bir paylaşılan Azure hizmet veri yolu kuyruğu kullanır. Bir etkinlik iş çalıştırılması gerektiğinde, veri fabrikası (kimlik bilgileri zaten kendini barındıran tümleştirmesi Çalışma Zamanı Modülü depolanmış olmayan olasılığına) kimlik bilgileri ile birlikte isteğini sıraya koyar. Kendini barındıran tümleştirmesi çalışma zamanı, sıra yoklama sonra işi başlatır.
6.  Kendini barındıran tümleştirmesi çalışma zamanı verileri bulut depolamaya veya tersi kopyalama etkinliği veri ardışık düzeninde nasıl yapılandırıldığına bağlı olarak bir şirket içi mağazadan kopyalar. Bu adım için kendi kendini barındıran tümleştirmesi çalışma zamanı doğrudan Azure Blob Depolama gibi bulut tabanlı depolama hizmetleri güvenli (HTTPS) kanal üzerinden iletişim kurar.

## <a name="considerations-for-using-self-hosted-ir"></a>Kendini barındıran IR kullanma konuları

- Bir tek kendini barındıran tümleştirmesi çalışma zamanı birden çok şirket içi veri kaynakları için kullanılabilir. Ancak, bir **tek kendini barındıran tümleştirmesi çalışma zamanı için yalnızca bir Azure data factory bağlı** ve başka bir data factory ile paylaşılamaz.
- Sağlayabilirsiniz **kendini barındıran Integration zamanının yalnızca bir örneğini** tek bir makinede yüklü. Şirket içi veri kaynaklarına erişmek için gereken iki veri fabrikaları olduğunu varsayalım kendini barındıran tümleştirmesi çalışma zamanı iki şirket içi bilgisayarlara yüklemeniz gerekir. Diğer bir deyişle, kendi kendini barındıran tümleştirmesi çalışma zamanı için belirli veri fabrikası bağlıdır
- **Kendini barındıran tümleştirmesi çalışma zamanı veri kaynağı ile aynı makinede olması gerekmez**. Ancak, tümleştirme çalışma zamanı kendi kendini barındıran daha yakın veri kaynağına veri kaynağına bağlanmak kendi kendini barındıran tümleştirmesi çalışma zamanı için süreyi azaltır. Kendi kendini barındıran tümleştirmesi çalışma zamanı olandan farklı bir makinede konakları şirket içi veri kaynağına yüklemenizi öneririz. Kendini barındıran tümleştirmesi çalışma zamanı ve veri kaynağı farklı makinelerde olduğunda, kendi kendini barındıran tümleştirmesi çalışma zamanı kaynakların veri kaynağı ile rekabet edemez.
- Sağlayabilirsiniz **farklı makinelerde aynı şirket içi veri kaynağına bağlanan birden çok kendini barındıran tümleştirme çalışma zamanları**. Örneğin, iki veri fabrikaları hizmet veren iki kendini barındıran tümleştirmesi çalışma zamanı olabilir ancak aynı şirket içi veri kaynağı ile hem veri fabrikaları kaydedilir.
- Bilgisayar hizmet yüklü bir ağ geçidi zaten varsa bir **Power BI** senaryosu, yükleme bir **Azure Data Factory için ayrı kendini barındıran tümleştirmesi çalışma zamanı** başka bir makinede.
- Kendini barındıran tümleştirmesi çalışma zamanı, Azure sanal ağ içindeki veri tümleştirmesini desteklemek için kullanılmalıdır.
- Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak davran kullandığınızda bile **ExpressRoute**. Kendini barındıran tümleştirmesi çalışma zamanı hizmeti ve veri kaynağı arasında bağlantı kurmak için kullanın.
- Veri deposu bulutta üzerinde olsa bile, kendini barındıran tümleştirmesi çalışma zamanı kullanmalısınız bir **Azure Iaas sanal makine**.
- Görevler Windows hangi FIPS uyumlu üzerinde şifreleme etkin bir sunucuda yüklü bir Self-hosted tümleştirme çalışma zamanında başarısız olabilir. Bu sorunu çözmek için sunucu üzerindeki FIPS uyumlu şifreleme devre dışı bırakın. FIPS uyumlu şifreleme devre dışı bırakmak için (0 (devre dışı) olarak etkin) 1'den aşağıdaki kayıt defteri değerini değiştirin: `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Önkoşullar

- Desteklenen **işletim sistemi** sürümleri Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. Kendini barındıran tümleştirmesi Çalışma Zamanı Modülü yüklemesini bir **etki alanı denetleyicisi desteklenmiyor**.
- **.NET framework 4.6.1 veya yukarıdaki** gereklidir. Windows 7 makinede kendini barındıran tümleştirmesi çalışma zamanı yüklüyorsanız, .NET Framework 4.6.1 yükleyin veya sonraki bir sürümü. Bkz: [.NET Framework sistem gereksinimleri](/dotnet/framework/get-started/system-requirements) Ayrıntılar için.
- Önerilen **yapılandırma** kendini barındıran tümleştirmesi çalışma zamanı için en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disk makinesidir.
- Konak makine hazırda bekleme, kendini barındıran tümleştirmesi çalışma zamanı verileri isteklere yanıt vermez. Bu nedenle, kendi kendini barındıran tümleştirmesi çalışma zamanı yüklemeden önce bilgisayarda uygun güç planı yapılandırın. Makine hazırda bekleme için yapılandırılmışsa, kendi kendini barındıran tümleştirmesi çalışma zamanı yükleme isteyen bir ileti alırsınız.
- Yükleme ve kendi kendini barındıran tümleştirme çalışma zamanının başarılı bir şekilde yapılandırmak için makinede yönetici olması gerekir.
- Kopyalama etkinliği çalıştırır belirli frekansında durum gibi makinedeki kaynak kullanımı (CPU, bellek) de en yüksek ve boşta kalma süreleri ile aynı düzeni izler. Kaynak Kullanımı Yoğun bir şekilde de taşınan veri miktarına bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda gidebilir bakın.

## <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler
Kendini barındıran tümleştirmesi çalışma zamanı, bir MSI kurulum paketi yükleyerek yüklenebilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Bkz: [şirket içi ve bulut arasında veri taşıma makale](tutorial-hybrid-copy-powershell.md) adım adım yönergeler için.

- Makine değil hazırda bekletme güç planı kendini barındıran tümleştirmesi çalışma zamanı için konak makinedeki yapılandırın. Konak makine hazırda bekleme, kendini barındıran tümleştirmesi çalışma zamanı çevrimdışı olarak bırakır.
- Kendini barındıran tümleştirmesi çalışma zamanı ile düzenli olarak ilişkili kimlik bilgilerini yedekle.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Yükleme ve İndirme Merkezi'nden Self-hosted IR kaydetme

1. Gidin [Microsoft tümleştirme çalışma zamanı yükleme sayfası](https://www.microsoft.com/download/details.aspx?id=39717).
2. Tıklatın **karşıdan**, uygun sürümü seçin (**32-bit** vs. **64-bit**), tıklatıp **sonraki**.
3. Çalıştırma **MSI** doğrudan ya da sabit diske ve Çalıştır kaydedin.
4. Üzerinde **Hoş Geldiniz** sayfasında, bir **dil** tıklatın **sonraki**.
5. **Kabul** tıklatın ve son kullanıcı lisans sözleşmesi **sonraki**.
6. Seçin **klasörü** kendini barındıran tümleştirmesi çalışma zamanı yükleme tıklatıp **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında, **yükleme**.
8. Tıklatın **son** yüklemeyi tamamlamak için.
9. Azure PowerShell kullanarak kimlik doğrulama anahtarı edinin. Kimlik doğrulama anahtarı almak için PowerShell örnek:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Üzerinde **tümleştirme (kendi kendini barındıran) çalışma zamanı kaydetmek** sayfa Microsoft tümleştirme çalışma zamanı yapılandırması, makinede çalışan Yöneticisi'nin, aşağıdaki adımları uygulayın:
    1. Yapıştır **kimlik doğrulama anahtarı** metin alanındaki.
    2. İsteğe bağlı olarak, tıklayın **Göster kimlik doğrulama anahtarı** anahtar metnini görmek için.
    3. Tıklatın **kaydetmek**.


## <a name="high-availability-and-scalability"></a>Yüksek kullanılabilirlik ve ölçeklenebilirlik
Self-hosted tümleştirme çalışma zamanı için birden çok şirket içi makineler ilişkili olabilir. Bu makineler, düğümler olarak adlandırılır. En fazla dört düğüm Self-hosted tümleştirmesi çalışma zamanı ile ilişkili olabilir. Mantıksal bir ağ geçidi için birden çok düğüm (yüklü ağ geçidi ile şirket içi makineler) sahip avantajları şunlardır:
1. Yüksek kullanılabilirlik çalışma zamanının Self-hosted tümleştirme, BT'nin artık tek hata noktası oluşturur, büyük veri çözüm ya da bulut veri tümleştirmesi ile en fazla 4 düğüm sürekliliğini sağlama Azure Data Factory ile gelir.
2. Geliştirilmiş performans ve işleme şirket içi ve bulut arasında veri taşıma sırasında veri depolar. Daha fazla bilgi almak [performans karşılaştırmaları](copy-activity-performance.md).

Birden çok düğüm Self-hosted tümleştirmesi çalışma zamanı yazılımını yükleyerek ilişkilendirebilirsiniz [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) ve tarafından alınan kimlik doğrulama anahtarları birini kaydetme Bölümünde açıklandığı gibi AzureRmDataFactoryV2IntegrationRuntimeKey yeni cmdlet [Öğreticisi](tutorial-hybrid-copy-powershell.md)

> [!NOTE]
> Her düğüm ilişkilendirmek için Self-hosted tümleştirmesi çalışma zamanı Yeni Oluştur gerekmez. Kendini barındıran tümleştirmesi çalışma zamanı başka bir makineye yükleyin ve aynı kimlik doğrulama anahtarı kullanarak kaydedin. 

> [!NOTE]
> İçin başka bir düğüm eklemeden önce **yüksek kullanılabilirlik ve ölçeklenebilirlik**, lütfen emin olun **'Uzaktan erişim için intranet'** seçenek **etkin** (Microsoft 1 düğümde Tümleştirme çalışma zamanı Configuration Manager -> Ayarlar -> Uzaktan intranet erişimi). 

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifika gereksinimleri
Çalışma zamanı düğümleri tümleştirme arasındaki iletişimi korumak için kullanılan TLS/SSL sertifika için gereksinimler şunlardır:

- Sertifika genel olarak güvenilir X509 olmalıdır v3 sertifikası. Ortak (üçüncü taraf) sertifika yetkilisi (CA) tarafından verilen sertifikaların kullanmanızı öneririz.
- Her tümleştirme Çalışma Zamanı Modülü düğüme bu sertifikaya güvenmeleri gerekir.
- Joker karakter sertifikaları desteklenir. FQDN adınız ise **node1.domain.contoso.com**, kullanabileceğiniz ***. domain.contoso.com** sertifikanın konu adı olarak.
- SAN sertifikaları yalnızca son öğeyi konu alternatif adlarını kullanılır ve diğerleri geçerli sınırlamaları nedeniyle yoksayılacak beri önerilmez. Örneğin SAN olan bir SAN sertifikanız **node1.domain.contoso.com** ve **node2.domain.contoso.com**, yalnızca bu sertifikayı FQDN değeri olduğu makinede kullanabileceğiniz **node2.domain.contoso.com**.
- SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutunu destekler.
- CNG kullanarak sertifika anahtarlar desteklenmez. Doesrted DoesDoes CNG anahtarları kullanan sertifikaları desteklemez.

## <a name="system-tray-icons-notifications"></a>Sistem Tepsisi Simgeleri / bildirimleri
İmleç sistem tepsisi simgesi/bildirim iletisi taşırsanız, kendi kendini barındıran tümleştirmesi çalışma zamanı durumunu ayrıntılarını bulabilirsiniz.

![Sistem tepsisi bildirimleri](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Göz önünde bulundurmanız gereken iki güvenlik duvarı vardır: **Kurumsal Güvenlik Duvarı** kuruluşun merkezi yönlendirici üzerinde çalışan ve **Windows Güvenlik Duvarı** yerel makine üzerinde bir arka plan programı olarak yapılandırılan nerede kendini barındıran tümleştirmesi çalışma zamanı yüklendi.

![Güvenlik duvarı](media\create-self-hosted-integration-runtime\firewall.png)

Konumundaki **Kurumsal Güvenlik Duvarı** düzeyi, aşağıdaki etki alanları ve giden bağlantı noktalarını yapılandırmanız:

Etki alanı adları | Bağlantı Noktaları | Açıklama
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Veri Taşıma hizmeti arka uç ile iletişim için kullanılan
*.core.windows.net | 443 | Azure Blob (yapılandırılmışsa) kullanarak hazırlanmış kopyalama için kullanılan
*.frontend.clouddatahub.net | 443 | Veri Taşıma hizmeti arka uç ile iletişim için kullanılan

Konumundaki **Windows Güvenlik Duvarı** düzeyi (makine düzey), bu giden bağlantı noktaları normal şekilde etkinleştirilir. Etki alanları ve buna göre üzerinde kendi kendini barındıran bağlantı noktalarını yapılandırabilirsiniz, varsa tümleştirmesi çalışma zamanı makine.

> [!NOTE]
> Kaynağını temel alan / havuzlarını olabilecek beyaz liste ek etki alanları ve giden bağlantı noktaları, Kurumsal/Windows Güvenlik Duvarı'nda.
>
> Bazı bulut veritabanları için (örneğin: Azure SQL Database, Azure Data Lake, vb.), kendi güvenlik duvarı yapılandırmasını kendini barındıran tümleştirmesi çalışma zamanı makinede beyaz liste IP adresini gerekebilir.

### <a name="copy-data-from-a-source-to-a-sink"></a>Bir havuz için veri kaynağından Kopyala
Güvenlik duvarı kurallarının düzgün Kurumsal Güvenlik Duvarı'nda, kendi kendini barındıran tümleştirmesi çalışma zamanı makine üzerindeki Windows Güvenlik Duvarı etkinleştirilir ve kendisini verileri depolamak emin olun. Bu kurallar etkinleştirme hem kaynağına bağlanmak ve başarıyla havuzu kendini barındıran tümleştirmesi çalışma zamanı sağlar. Kopyalama işlemi dahil her veri deposu için kuralları etkinleştirin.

Örneğin, kopyalama kaynağı için bir **şirket verilerini depolamak için bir Azure SQL veritabanı havuz veya bir Azure SQL Data Warehouse havuz**, aşağıdaki adımları uygulayın:

- Gidene izin ver **TCP** iletişim bağlantı noktası **1433** Windows Güvenlik Duvarı ve kurumsal güvenlik duvarı için.
- Kendini barındıran tümleştirmesi çalışma zamanı makinenin IP adresinin IP adresine izin listesine eklemek için Azure SQL server'ın güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Güvenlik duvarını giden bağlantı noktası 1433 izin verme, kendi kendini barındıran tümleştirmesi çalışma zamanı doğrudan Azure SQL erişemiyor. Bu durumda, kullanabilir [hazırlanan kopyalama](copy-activity-performance.md) SQL Azure veritabanına / SQL Azure DW. Bu senaryoda, yalnızca HTTPS (443 numaralı bağlantı noktası) için veri taşıma gerektirir.


## <a name="proxy-server-considerations"></a>Proxy sunucusu hususları
Kurumsal ağ ortamınıza Internet'e erişmek için bir proxy sunucusu kullanıyorsa, kendi kendini barındıran tümleştirme çalışma uygun proxy ayarlarını yapılandırın. İlk kayıt aşamasında proxy ayarlayabilirsiniz.

![Proxy belirtin](media\create-self-hosted-integration-runtime\specify-proxy.png)

Kendini barındıran tümleştirmesi çalışma zamanı, bulut hizmetine bağlanmak için proxy sunucusunu kullanır. İlk kurulum sırasında Değiştir bağlantısına tıklayın. Proxy ayarı iletişim kutusunu görürsünüz.

![Set proxy](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Üç yapılandırma seçeneği vardır:

- **Proxy kullanmayın**: kendini barındıran tümleştirmesi çalışma zamanı açıkça kullanmaz her Proxy'yi bulut hizmetlerine bağlanmak için.
- **Sistem proxy kullanacak**: diahost.exe.config ve diawp.exe.config ayarı proxy yapılandırılmış tümleştirmesi çalışma zamanı kullanır'kendi kendini barındırır. Proxy diahost.exe.config ve diawp.exe.config yapılandırılmışsa, kendi kendini barındıran tümleştirmesi çalışma zamanı proxy üzerinden geçmeden doğrudan bulut hizmetine bağlanır.
- **Özel ara sunucu kullanmak**: diahost.exe.config ve diawp.exe.config yapılandırmalarında kullanmak yerine kendi kendini barındıran tümleştirmesi çalışma zamanı için kullanılacak HTTP proxy ayarlarını yapılandır. Adresi ve bağlantı noktası gereklidir. Kullanıcı adı ve parola, proxy'nin kimlik doğrulama ayarını bağlı olarak isteğe bağlıdır. Tüm ayarları Windows DPAPI ile kendini barındıran tümleştirmesi Çalışma Zamanı Modülü şifrelenir ve makinede yerel olarak depolanır.

Güncelleştirilen proxy ayarlarını kaydettikten sonra tümleştirmesi çalışma zamanı ana bilgisayar hizmeti otomatik olarak yeniden başlatılır.

Görüntülemek veya proxy ayarlarını güncelleştirmek istiyorsanız, kendi kendini barındıran tümleştirmesi çalışma zamanı başarıyla, kaydedildikten sonra tümleştirme çalışma zamanı Yapılandırma Yöneticisi'ni kullanın.

1.  Başlatma **Microsoft tümleştirme çalışma zamanı Configuration Manager**.
2.  **Ayarlar** sekmesine geçin.
3.  Tıklatın **değişiklik** bağlamak **HTTP Proxy** başlatmak için bölüm **Set HTTP Proxy** iletişim.
4.  Tıklattıktan sonra **sonraki** düğmesi, proxy ayarı kaydetmek ve çalışma zamanı ana bilgisayar hizmeti yeniden başlatmak için izninizi isteyen bir uyarı iletişim kutusu görürsünüz.

Görüntüleyin ve Configuration Manager aracını kullanarak HTTP proxy güncelleştirin.

![Görünüm proxy](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Bir proxy sunucusu NTLM kimlik doğrulaması ile ayarladıysanız, tümleştirme çalışma zamanı ana bilgisayar hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabı için parolayı değiştirirseniz, hizmeti için yapılandırma ayarlarını güncelleştirin ve uygun şekilde yeniden unutmayın. Bu gereksinim nedeniyle, parolayı sık güncelleştirmeye gerektirmez proxy sunucusuna erişmek için bir özel etki alanı hesabı kullanmak öneririz.

### <a name="configure-proxy-server-settings"></a>Proxy sunucusu ayarlarını yapılandırın

Seçerseniz **sistem proxy kullanmak** için HTTP Proxy'sini ayarlamayı, kendi kendini barındıran tümleştirmesi çalışma zamanı proxy diahost.exe.config ve diawp.exe.config ayarını kullanır. Proxy diahost.exe.config ve diawp.exe.config belirtilirse, kendi kendini barındıran tümleştirmesi çalışma zamanı proxy üzerinden geçmeden doğrudan bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyasını güncelleştirmek için yönergeler sağlar.

1. Dosya Gezgini'nde, özgün dosyasını yedeklemek için C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config güvenli bir kopyasını oluşturun.
2. Yönetici olarak çalıştırarak Notepad.exe başlatın ve metin dosyasını "C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config. açın Aşağıdaki kodda gösterildiği gibi varsayılan etiket için system.net bulun:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Proxy sunucu ayrıntıları aşağıdaki örnekte gösterildiği gibi daha sonra ekleyebilirsiniz:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Ek özellikler proxy etiketinin içine scriptLocation gibi gerekli ayarları belirtmek için izin verilir. Bkz: [proxy öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) sözdizimi için.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Yapılandırma dosyasını özgün konumuna kaydedin, sonra değişiklikleri toplar Self-hosted tümleştirmesi çalışma zamanı ana bilgisayar hizmeti yeniden başlatın. Hizmetini yeniden başlatmak için: Denetim Masası'ndan ya da hizmetler uygulamasını kullanın **tümleştirmesi çalışma zamanı Configuration Manager** > tıklatın **Hizmeti Durdur** düğmesine ve ardından **Başlat Hizmet**. Hizmet başlatılmazsa, hatalı bir XML etiket söz dizimini düzenlendi uygulama yapılandırma dosyasına eklendi olasıdır.

> [!IMPORTANT]
> Diahost.exe.config ve diawp.exe.config güncelleştirmek unutmayın.

Bu noktalarının yanı sıra de Microsoft Azure, şirketinizin beyaz olduğundan emin olmanız gerekir. Geçerli Microsoft Azure IP adreslerinin listesi yüklenebilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Güvenlik Duvarı ve proxy sunucusu ile ilgili sorunlar için olası Belirtiler
Aşağıdaki ayarlara benzer hatalarla karşılaşırsanız, kendi kimliğini doğrulamak için Data Factory bağlanmasını kendini barındıran tümleştirmesi çalışma zamanı engelleyen güvenlik duvarı veya proxy sunucu hatalı yapılandırma nedeniyle olasıdır. Güvenlik duvarınızın emin olmak için önceki bölüme bakın ve proxy sunucusu doğru şekilde yapılandırılmış.

1.  Kendini barındıran tümleştirmesi çalışma zamanı kaydetmeye çalıştığınızda, aşağıdaki hatayı alırsınız: "Bu tümleştirme çalışma zamanı düğüm kaydedilemedi! Kimlik doğrulama anahtarı geçerli olduğundan ve tümleştirme hizmeti ana bilgisayar hizmeti bu makinede çalışıyor olduğunu onaylayın. "
2.  Tümleştirme çalışma zamanı Yapılandırma Yöneticisi'ni açın, durum olarak görmek "**bağlantı kesildi**"veya"**bağlanıyor**". Windows olay günlükleri, "Olay Görüntüleyici" görüntülerken > "Uygulama ve hizmet günlükleri" > "Microsoft tümleştirme çalışma zamanı", aşağıdaki hata gibi hata iletilerine bakın:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-intranet"></a>İntranet uzaktan erişimi etkinleştirin  
Kullanırsanız durumda **PowerShell** veya **kimlik bilgileri Yöneticisi uygulama** kendini barındıran tümleştirmesi çalışma zamanı, ardından yüklendiği dışında (ağındaki) başka bir makineden kimlik bilgilerini şifrelemek için gerektirecek **'İntranet uzaktan erişim'** seçeneği etkinleştirilmiş olmalıdır. Çalıştırırsanız **PowerShell** veya **kimlik bilgileri Yöneticisi uygulama** kendini barındıran tümleştirmesi çalışma zamanı yüklü olduğu, ardından aynı makineye kimlik bilgilerini şifrelemek için **' uzaktan erişim intranet '** etkin değil.

Uzaktan erişim intranet olmalıdır **etkin** için başka bir düğüm eklemeden önce **yüksek kullanılabilirlik ve ölçeklenebilirlik**.  

Kendini barındıran tümleştirmesi çalışma zamanı Kurulum sırasında (veya sonraki sürümleri v 3.3.xxxx.x), varsayılan olarak, kendi kendini barındıran tümleştirmesi çalışma zamanı yükleme devre dışı bırakır **'İntranet uzaktan erişim'** kendini barındıran tümleştirmesi çalışma zamanı makinede.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8060 (veya kullanıcı tarafından yapılandırılmış bağlantı noktası) el ile açabilirsiniz. Kendini barındıran tümleştirmesi çalışma zamanı kurulumu sırasında güvenlik duvarı sorunu yaşayıp çalıştırırsanız, güvenlik duvarı yapılandırması olmadan kendini barındıran tümleştirmesi çalışma zamanı yüklemek için aşağıdaki komutu kullanarak deneyebilirsiniz.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> **Kimlik bilgileri Yöneticisi uygulama** henüz ADFv2 kimlik bilgilerini şifrelemek için kullanılabilir değil. Daha sonra bu destek ekleyeceğiz.  

Kendini barındıran tümleştirmesi çalışma zamanı makinedeki bağlantı noktası 8060 açmamak seçerseniz kullanma dışındaki mekanizmalarını kullanmak ** ayarı kimlik bilgilerini ** veri deposu kimlik yapılandırmak için uygulama. Örneğin, yeni AzureRmDataFactoryV2LinkedServiceEncryptCredential PowerShell cmdlet'ini kullanabilirsiniz. Bkz. nasıl veri kimlik bilgilerini depolamak üzerinde kimlik bilgilerini ayarlama ve güvenlik bölümü ayarlanabilir.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki Öğreticisi Adım adım yönergeler için bkz: [öğretici: kopyalama şirket içi buluta veri](tutorial-hybrid-copy-powershell.md).
