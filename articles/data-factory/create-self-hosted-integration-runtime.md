---
title: Azure Data Factory'de şirket içinde barındırılan tümleştirme çalışma zamanı oluştur | Microsoft Docs
description: Veri fabrikaları özel ağdaki veri depoları erişmeye izin veren Azure Data factory'de şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 90e43ab0448646650067dbf151702132f434c01e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65967949"
---
# <a name="create-and-configure-a-self-hosted-integration-runtime"></a>Oluşturma ve şirket içinde barındırılan tümleştirme çalışma zamanını yapılandırma
Integration runtime (IR) farklı ağ ortamları veri tümleştirme özellikleri sağlamak üzere Azure Data Factory kullanan işlem altyapısıdır. IR hakkında daha fazla ayrıntı için bkz: [tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md).

Şirket içinde barındırılan Integration runtime bulut veri deposu ve özel ağdaki veri deposu arasında kopyalama etkinlikleri çalıştırabilirsiniz ve dönüştürme etkinliklerini şirket içi ağ ya da bir Azure sanal ağı bilgi işlem kaynaklarının karşı gönderebilecek. Bir şirket içi makinede veya özel ağ içindeki bir sanal makine (VM) bir şirket içinde barındırılan tümleştirme çalışma zamanı yüklemesi gerekir.  

Bu belgede nasıl oluşturabileceğinizi ve şirket içinde barındırılan IR yapılandırma açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="high-level-steps-to-install-a-self-hosted-ir"></a>Şirket içinde barındırılan IR yüklemek için üst düzey adımları
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. Bu görev için Azure Data Factory kullanıcı Arabirimi kullanabilirsiniz. PowerShell örneği aşağıda verilmiştir:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
  
2. [İndirme](https://www.microsoft.com/download/details.aspx?id=39717) ve şirket içinde barındırılan tümleştirme çalışma zamanı yerel bir makineye yükleyin.

3. Kimlik doğrulama anahtarı almak ve şirket içinde barındırılan tümleştirme çalışma zamanı anahtarı ile kaydedin. PowerShell örneği aşağıda verilmiştir:

    ```powershell

    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName  

    ```

## <a name="setting-up-a-self-hosted-ir-on-an-azure-vm-by-using-an-azure-resource-manager-template-automation"></a>Bir Azure Resource Manager şablonu (Otomasyonu) kullanarak bir Azure sanal makinesinde kendinden konak IR ayarlama
Bir Azure sanal makinesinde şirket içinde barındırılan IR Kurulumu kullanarak otomatikleştirebilirsiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Bu şablon, (2 veya daha yüksek düğüm sayısı ayarladığınız sürece) tam olarak çalışmasını sağlamak için kolay bir yol IR yüksek kullanılabilirlik ve ölçeklenebilirlik özelliklerine sahip Azure sanal ağı içinde şirket içinde barındırılan sağlar.

## <a name="command-flow-and-data-flow"></a>Komut akışını ve veri akışı
Ne zaman şirket içi arasında veri taşıma ve bir şirket içi veri kaynağından buluta ve veri aktarımı için bir şirket içinde barındırılan tümleştirme çalışma zamanı bulut etkinliği kullanır.

Kendinden konak IR ile kopyalamak için adımların özeti için üst düzey veri akışı şu şekildedir:

![Yüksek düzey genel bakış](media/create-self-hosted-integration-runtime/high-level-overview.png)

1. Veri Geliştirici PowerShell cmdlet'ini kullanarak bir şirket içinde barındırılan tümleştirme çalışma zamanı içinde bir Azure data factory oluşturur. Şu anda, Azure portalı, bu özelliği desteklemez.
2. Veri geliştirici bir şirket içi veri deposu için bağlı hizmet, veri depolarında bağlanmak için kullanması gereken şirket içinde barındırılan tümleştirme çalışma zamanı örneğini belirterek oluşturur.
3. Şirket içinde barındırılan Integration runtime düğümü, Windows Data Protection uygulama programlama arabirimi (DPAPI) kullanarak kimlik bilgilerini şifreler ve kimlik bilgilerini yerel olarak kaydeder. Yüksek kullanılabilirlik için birden çok düğüm ayarlarsanız, kimlik bilgilerini diğer düğümler arasında daha fazla eşitlenir. Her düğümü DPAPI kullanarak kimlik bilgilerini şifreler ve bunları yerel olarak depolar. Kimlik bilgisi eşitlemesi veri geliştiriciler için saydamdır ve şirket içinde barındırılan IR tarafından ele alınır    
4. Data Factory hizmetinin zamanlama ve işleri yönetimi için şirket içinde barındırılan tümleştirme çalışma zamanı ile iletişim kuran bir *denetim kanalı* paylaşılan kullanan [Azure Service Bus geçişi](https://docs.microsoft.com/azure/service-bus-relay/relay-what-is-it#wcf-relay). Bir etkinliği işinin çalıştırılması gerektiğinde, Data Factory istekle birlikte tüm kimlik bilgilerini (kimlik bilgileri zaten şirket içinde barındırılan tümleştirme çalışma zamanını depolanmaz durumda) kuyruğa alır. Şirket içinde barındırılan tümleştirme çalışma zamanı işi kuyruğa yoklama sonra başlatıyor.
5. Şirket içinde barındırılan tümleştirme çalışma zamanı verileri bulut depolamaya veya tam tersi veri işlem hattında kopyalama etkinliği nasıl yapılandırıldığına bağlı olarak bir şirket içi depolama alanından kopyalar. Bu adım için şirket içinde barındırılan tümleştirme çalışma zamanı doğrudan Azure Blob Depolama gibi bulut tabanlı depolama hizmetleriyle güvenli (HTTPS) bir kanal üzerinden iletişim kurar.

## <a name="considerations-for-using-a-self-hosted-ir"></a>Şirket içinde barındırılan IR kullanma konuları

- Tek şirket içinde barındırılan tümleştirme çalışma zamanının birden çok şirket içi veri kaynakları için kullanılabilir. Tek şirket içinde barındırılan tümleştirme çalışma zamanı, aynı Azure Active Directory kiracısı içinde başka bir data factory ile paylaşılabilir. Daha fazla bilgi için [şirket içinde barındırılan tümleştirme çalışma zamanı paylaşımı](#sharing-the-self-hosted-integration-runtime-with-multiple-data-factories).
- Tek bir makinede yüklü bir şirket içinde barındırılan tümleştirme çalışma zamanı yalnızca bir örneği olabilir. Kullanın ya da şirket içi veri kaynaklarına erişmesi gereken iki veri fabrikaları varsa [şirket içinde barındırılan IR paylaşımı özelliğini](#sharing-the-self-hosted-integration-runtime-with-multiple-data-factories) şirket içinde barındırılan tümleştirme çalışma zamanı paylaşmak ya da ikisini de şirket içinde barındırılan tümleştirme çalışma zamanını yükleme Böylece, şirket içi bilgisayarlar, her bir veri fabrikası için bir tane.  
- Şirket içinde barındırılan tümleştirme çalışma zamanı veri kaynağı ile aynı makinede olması gerekmez. Ancak, şirket içinde barındırılan tümleştirme çalışma zamanı sahip daha yakın veri kaynağına veri kaynağına bağlanmak şirket içinde barındırılan tümleştirme çalışma zamanı için süreyi azaltır. Şirket içinde barındırılan tümleştirme çalışma zamanı olandan farklı bir makinede ana şirket içi veri kaynağına yüklemenizi öneririz. Şirket içinde barındırılan tümleştirme çalışma zamanı ve veri kaynağını farklı makinelerde olduğunda, şirket içinde barındırılan tümleştirme çalışma zamanı veri kaynağı ile kaynaklar için rekabet edemez.
- Aynı şirket içi veri kaynağına bağlanan farklı makinelerde birden çok şirket içinde barındırılan tümleştirme çalışma zamanları olabilir. Örneğin, iki veri fabrikaları hizmet iki şirket içinde barındırılan tümleştirme çalışma zamanları olabilir, ancak aynı şirket içi veri kaynağı ile veri fabrikaları hem kayıtlı.
- Ayrı şirket içinde barındırılan tümleştirme çalışma zamanı, bir Power BI senaryo sunmak için bilgisayarınızda yüklü bir ağ geçidi zaten varsa, Azure Data Factory için başka bir makineye yükleyin.
- Şirket içinde barındırılan tümleştirme çalışma zamanı, bir Azure sanal ağ içindeki veri tümleştirmesini desteklemek için kullanılmalıdır.
- Veri kaynağınızı bile Azure Expressroute'u kullanırken, bir güvenlik duvarının arkasındaysa şirket içi veri kaynağı olarak değerlendirin. Şirket içinde barındırılan tümleştirme çalışma zamanı hizmeti ve veri kaynağı arasında bağlantı kurmak için kullanın.
- Buluttaki bir Azure Iaas sanal makinesinde veri deposu olsa bile, şirket içinde barındırılan tümleştirme çalışma zamanı kullanmanız gerekir.
- Görevler, hangi FIPS uyumlu üzerinde şifreleme etkin bir Windows sunucusu üzerinde yüklü bir şirket içinde barındırılan tümleştirme çalışma zamanında başarısız olabilir. Bu sorunu çözmek için sunucudaki FIPS uyumlu şifrelemeyi devre dışı bırakın. FIPS uyumlu şifrelemeyi devre dışı bırakmak için 1 (0 (devre dışı) olarak etkin) olan aşağıdaki kayıt defteri değerini Değiştir: `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Önkoşullar

- Desteklenen işletim sistemi sürümleri, Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 ' dir. Şirket içinde barındırılan tümleştirme çalışma zamanı bir etki alanı denetleyicisindeki yükleme desteklenmez.
- .NET framework 4.6.1 veya üstü gereklidir. Windows 7 makinesinde şirket içinde barındırılan tümleştirme çalışma zamanı'nı yüklüyorsanız, .NET Framework 4.6.1 yükleyin veya üzeri. Bkz: [.NET Framework System Requirements](/dotnet/framework/get-started/system-requirements) Ayrıntılar için.
- Şirket içinde barındırılan tümleştirme çalışma zamanı makine için önerilen yapılandırma, en az 2 GHz, dört çekirdek, 8 GB RAM ve 80 GB disk içindir.
- Konak makine hazırda bekleme, şirket içinde barındırılan tümleştirme çalışma zamanı veri isteklere yanıt vermez. Şirket içinde barındırılan Integration runtime'ı yüklemeden önce bilgisayarda uygun bir güç planı yapılandırın. Makine hazırda bekleme için yapılandırılmışsa, şirket içinde barındırılan tümleştirme çalışma zamanı yükleme isteyen bir ileti alırsınız.
- Makinede yüklemek ve şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla yapılandırmak için bir yönetici olması gerekir.
- Kopyalama etkinliği çalıştırma üzerinde belirli bir sıklıkta gerçekleşir. Kaynak Kullanımı (CPU, bellek) makine üzerinde yoğun ve boş kalma sürelerinde ile aynı deseni izler. Kaynak Kullanımı Yoğun taşınan veri miktarı da bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda Yukarı Git bakın.

## <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler
Şirket içinde barındırılan tümleştirme çalışma zamanının bir MSI Kurulumu paketinden indirerek yükleyebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Bkz: [şirket içi ile bulut arasında veri taşıma makale](tutorial-hybrid-copy-powershell.md) adım adım yönergeler için.

- Makine olmayan hazırda bekletme güç planı için şirket içinde barındırılan Integration runtime konak makinedeki yapılandırın. Konak makine hazırda bekleme, şirket içinde barındırılan tümleştirme çalışma zamanı çevrimdışı olur.
- Düzenli olarak şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili kimlik bilgilerini yedekle.

## <a name="install-and-register-self-hosted-ir-from-the-download-center"></a>Yükleme ve İndirme Merkezi'nden kendinden konak IR kaydetme

1. Git [Microsoft tümleştirme çalışma zamanı indirme sayfası](https://www.microsoft.com/download/details.aspx?id=39717).
2. Seçin **indirme**, 64 bit sürümünü seçin (32-bit desteklenmez) seçip **sonraki**.
3. MSI dosyası, doğrudan çalıştırmak veya sabit disk sürücünüze kaydedin ve çalıştırın.
4. Üzerinde **Hoş Geldiniz** sayfasında, bir dil seçip **sonraki**.
5. Microsoft Yazılımı Lisans koşullarını kabul edin ve seçin **sonraki**.
6. Seçin **klasör** şirket içinde barındırılan tümleştirme çalışma zamanını yükleyin ve **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında **yükleme**.
8. Tıklayın **son** yüklemeyi tamamlamak için.
9. Azure PowerShell kullanarak kimlik doğrulama anahtarını alın. Kimlik doğrulama anahtarı almak için bir PowerShell örneği aşağıda verilmiştir:

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Üzerinde **Integration Runtime (şirket içinde barındırılan) Kaydet** sayfa Microsoft tümleştirme çalışma zamanı yapılandırma makinenizde çalışan Yöneticisi, aşağıdaki adımları uygulayın:

    a. Kimlik doğrulama anahtarı metin alanına yapıştırın.

    b. İsteğe bağlı olarak **Show kimlik doğrulama anahtarı** anahtar metnini görmek için.

    c. **Kaydol**’u seçin.


## <a name="high-availability-and-scalability"></a>Yüksek kullanılabilirlik ve ölçeklenebilirlik
Şirket içinde barındırılan tümleştirme çalışma zamanı, birden çok şirket içi makine veya azure'da sanal makineler ile ilişkilendirilebilir. Bu makineler, düğüm olarak adlandırılır. Bir şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili en fazla dört düğümünüz olabilir. Mantıksal bir ağ geçidi için birden çok düğüm (yüklü bir ağ geçidi ile şirket içi makineler) sahip avantajları şunlardır:
* BT'nin tek, büyük veri çözümü veya Bulut veri tümleştirme en fazla dört düğüm ile sürekliliğini sağlama, Azure Data Factory ile bir hata noktası artık kullanıcının bu nedenle şirket içinde barındırılan tümleştirme çalışma zamanı yüksek kullanılabilirlik.
* Geliştirilmiş performans ve aktarım hızını şirket içi ve bulut arasında veri taşıma sırasında veri depoları. Daha fazla bilgi edinin [performans karşılaştırmalar](copy-activity-performance.md).

Şirket içinde barındırılan tümleştirme çalışma zamanı yazılımı yükleyerek birden çok düğüm ilişkilendirebilirsiniz [İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=39717). Ardından, kimlik doğrulaması anahtarlarını birini kullanarak elde ettiğiniz Kaydet **yeni AzDataFactoryV2IntegrationRuntimeKey** açıklandığı cmdlet'i [öğretici](tutorial-hybrid-copy-powershell.md).

> [!NOTE]
> Her düğüm ilişkilendirme için yeni şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmanız gerekmez. Şirket içinde barındırılan tümleştirme çalışma zamanı başka bir makineye yükleyin ve aynı kimlik doğrulama anahtarı kullanarak kaydedin. 

> [!NOTE]
> Yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce emin **uzaktan erişim için intranet** seçeneği etkin olduğunda ilk düğümü (**Microsoft Integration Runtime Yapılandırma Yöneticisi**  >  **Ayarları** > **uzaktan erişim için intranet**). 

### <a name="scale-considerations"></a>Ölçek konuları

#### <a name="scale-out"></a>Ölçeği genişletme

Kendinden konak IR üzerinde kullanılabilir bellek düşükse ve CPU kullanımı yüksek olduğunda, yeni bir düğüm ekleme yük ölçeğinizi makinelerde yardımcı olur. Ağ geçidini bir düğüm ekleme, çünkü bunlar zaman aşımına uğruyor etkinlikleri başarısız oluyorsa ya da şirket içinde barındırılan IR düğümü çevrimdışı olduğu için yardımcı olur.

#### <a name="scale-up"></a>Ölçeği artırma

Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak / eşzamanlı iş yürütme sınırına ulaştı, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Kendinden konak IR aşırı yüklendiği etkinlikler zaman aşımına uğruyor. zaman ölçeği isteyebilirsiniz. Aşağıdaki görüntüde gösterildiği gibi bir düğüm için kapasite üst sınırı artırabilirsiniz:  

![Bir düğümde çalıştırılabilen eşzamanlı iş artırma](media/create-self-hosted-integration-runtime/scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifikası gereksinimleri

Çalışma zamanı düğümleri tümleştirme arasındaki iletişimin güvenliğini sağlamak için kullanılan TLS/SSL sertifika için gereksinimler şunlardır:

- Sertifika genel olarak güvenilir X509 olmalıdır v3 sertifikası. Genel (ortak) tarafından verilen sertifikaların kullanmanızı öneririz sertifika yetkilisi (CA).
- Her Integration runtime düğümü, bu sertifikaya güvenmeleri gerekir.
- Konu alternatif adı (SAN) sertifikaları yalnızca son SAN öğeyi kullanılacak ve diğer tüm geçerli sınırlamalar nedeniyle yoksayılacak çünkü önerilmemektedir. Örneğin, bir SAN sertifika ayarlanmış SAN'lar vardır **node1.domain.contoso.com** ve **node2.domain.contoso.com**, bu sertifika yalnızca FQDN değeri olan bir makinede kullanabilirsiniz  **node2.domain.contoso.com**.
- Sertifikayı SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutu destekler.
- CNG anahtarları kullanan sertifikaları desteklenmez.  

> [!NOTE]
> Bu sertifika, bağlantı noktaları için kullanılan şirket içinde barındırılan IR düğümde şifrelemek için kullanılan **düğümden düğüme iletişim** (bağlı hizmetler içeren ve durum eşitleme eşitleme düğümleri arasında kimlik bilgilerini için) ve sırasında**cmdlet'i için bağlı hizmet kimlik bilgisi PowerShell kullanarak ayarı** gelen yerel ağ içinde. Özel ağ ortamınızı güvenli değilse veya kendi özel ağınızı de içindeki düğümler arasında iletişimi güvenli hale getirmek istiyorsanız, bu sertifika kullanmanızı öneririz. Veri taşıma diğer veri depoları şirket içinde barındırılan IR gelen geçiş her zaman şifreli bir kanal, bu sertifika ayarlanıp bağımsız olarak kullanarak gerçekleşir. 

## <a name="sharing-the-self-hosted-integration-runtime-with-multiple-data-factories"></a>Şirket içinde barındırılan tümleştirme çalışma zamanının birden çok veri fabrikaları ile paylaşma

Bir veri fabrikasında zaten ayarladığınız var olan şirket içinde barındırılan tümleştirme çalışma zamanı altyapısını yeniden kullanabilirsiniz. Bu sayede oluşturmak bir *bağlı şirket içinde barındırılan tümleştirme çalışma zamanı* IR (paylaşılan) var olan bir başvuru tarafından Fabrika farklı bir verileri şirket içinde barındırılan.

PowerShell kullanarak bir şirket içinde barındırılan tümleştirme çalışma zamanı paylaşmak için bkz: [PowerShell ile Azure Data factory'de bir paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](create-shared-self-hosted-integration-runtime-powershell.md).

On iki dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Hybrid-data-movement-across-multiple-Azure-Data-Factories/player]

### <a name="terminology"></a>Terminoloji

- **IR paylaşılan**: Bir fiziksel altyapısı üzerinde çalışan özgün şirket içinde barındırılan IR.  
- **IR bağlı**: IR başka başvuran IR paylaşılan Bu mantıksal bir IR ve başka bir şirket içinde barındırılan IR (paylaşılan) altyapısını kullanır.

### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>Bağlantılı bir şirket içinde barındırılan IR oluşturmaya yönelik üst düzey adımları

1. Paylaşılacak şirket içinde barındırılan IR, data factory, bağlantılı IR oluşturmak istediğiniz izni 

   ![Paylaşım sekmesinde izin verme düğmesi](media/create-self-hosted-integration-runtime/grant-permissions-IR-sharing.png)

   ![İzinler atama seçimleri](media/create-self-hosted-integration-runtime/3_rbac_permissions.png)

2. Paylaşılması için kendinden konak IR kaynak Kimliğini not alın.

   ![Kaynak kodu konumu](media/create-self-hosted-integration-runtime/4_ResourceID_self-hostedIR.png)

3. İzinleri verilmiş olan veri fabrikasında (bağlı) yeni bir şirket içinde barındırılan IR oluşturma ve kaynak kimliği girin.

   ![Bir bağlı şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmak için düğme](media/create-self-hosted-integration-runtime/6_create-linkedIR_2.png)

   ![Adı ve kaynak kimliği için kutuları](media/create-self-hosted-integration-runtime/6_create-linkedIR_3.png)

### <a name="monitoring"></a>İzleme 

- **Paylaşılan IR**

  ![Bir paylaşılan tümleştirme çalışma zamanı bulma seçimleri](media/create-self-hosted-integration-runtime/Contoso-shared-IR.png)

  ![İzleme için sekmesinde](media/create-self-hosted-integration-runtime/contoso-shared-ir-monitoring.png)

- **Bağlantılı IR**

  ![Bir bağlı tümleştirme çalışma zamanı bulma seçimleri](media/create-self-hosted-integration-runtime/Contoso-linked-ir.png)

  ![İzleme için sekmesinde](media/create-self-hosted-integration-runtime/Contoso-linked-ir-monitoring.png)

### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Şirket içinde barındırılan IR paylaşımı bilinen sınırlamalar

* Data factory, bağlı bir IR oluşturulacağı olmalıdır bir [MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Varsayılan olarak, Azure portalında veri fabrikaları oluşturulan veya örtük olarak oluşturulmuş bir MSI PowerShell cmdlet'leri vardır. Ancak bir veri fabrikası, bir Azure Resource Manager şablonu veya SDK oluşturulduğunda **kimlik** özelliği ayarlanmalıdır açıkça Azure Resource Manager içeren bir MSI veri fabrikası oluşturduğundan emin olmak için. 

* Bu özelliği destekleyen Azure Data Factory .NET SDK sürümüdür 1.1.0 veya üzeri.

* İzin vermek için kullanıcının sahip rolü veya paylaşılan IR bulunduğu data factory'de devralınan sahip rolü olmalıdır.

* Paylaşımı özelliğini, yalnızca aynı Azure Active Directory kiracısı içindeki veri fabrikaları için çalışır.

* Active Directory için [Konuk kullanıcılar](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews), (tüm veri fabrikalarını arama anahtar sözcüğü kullanarak listeleme) arama işlevleri kullanıcı arabiriminde [çalışmıyor](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#SearchLimits). Ancak Konuk kullanıcı, data factory'nin sahibi olduğu sürece, IR arama işlevlerinde olmadan doğrudan, paylaşılan IR ile gereken veri fabrikasının MSI yazarak paylaşabilirler **atama izni** metin kutusu ve seçme **Ekle** Azure Data Factory kullanıcı arabiriminde. 

  > [!NOTE]
  > Bu özellik yalnızca Azure Data Factory V2'de kullanılabilir. 

## <a name="notification-area-icons-and-notifications"></a>Bildirim alanı simgesini ve bildirimler

Simge veya bildirim alanında bir ileti üzerinden imlecinizi taşırsanız, şirket içinde barındırılan tümleştirme çalışma zamanı durumu hakkındaki ayrıntıları bulabilirsiniz.

![Bildirim alanında bildirim](media/create-self-hosted-integration-runtime/system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Dikkate alınması gereken iki güvenlik duvarı vardır: *Kurumsal güvenlik duvarınız* kuruluşun merkezi yönlendirici üzerinde çalışan ve *Windows Güvenlik Duvarı* yerel makinede bir arka plan olarak yapılandırılmış olduğu Şirket içinde barındırılan tümleştirme çalışma zamanı yüklenir.

![Güvenlik duvarı](media/create-self-hosted-integration-runtime/firewall.png)

Konumunda *Kurumsal güvenlik duvarınız* düzeyi, aşağıdaki etki alanları ve giden bağlantı noktalarını yapılandırmak gerekir:

Etki alanı adları | Bağlantı Noktaları | Açıklama
------------ | ----- | ------------
*.servicebus.windows.net | 443 | Arka uç veri taşıma hizmeti ile iletişim için kullanılır
*. core.windows.net | 443 | Hazırlanmış kopya (yapılandırılmışsa) Azure Blob Depolama alanı üzerinden kullanılan
*.frontend.clouddatahub.net | 443 | Arka uç veri taşıma hizmeti ile iletişim için kullanılır
download.microsoft.com | 443 | Güncelleştirmeleri yüklemek için kullanılan

Adresindeki *Windows Güvenlik Duvarı* düzeyi (makine düzeyi), şu giden bağlantı noktaları normalde etkinleştirilir. Varsa etki alanları ve bağlantı noktalarını uygun şekilde yapılandırabilirsiniz, şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde.

> [!NOTE]
> Kaynak ve havuz üzerinde bağlı olarak, beyaz liste ek etki alanları ve kurumsal güvenlik duvarınız veya Windows Güvenlik Duvarı giden bağlantı noktaları olabilir.
>
> Bulut için bazı veritabanları (örneğin, Azure SQL veritabanı ve Azure Data Lake), şirket içinde barındırılan tümleştirme çalışma zamanı, güvenlik duvarı yapılandırması makinelerde beyaz liste IP adreslerini gerekebilir.

### <a name="copy-data-from-a-source-to-a-sink"></a>Verileri bir kaynaktan havuza kopyalama
Güvenlik duvarı kuralları düzgün bir şekilde şirket güvenlik duvarı, şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde, Windows Güvenlik Duvarı etkinleştirilir ve kendi veri deposu emin olun. Bu kurallar etkinleştirme, her iki kaynağına bağlanmak ve başarıyla havuz şirket içinde barındırılan tümleştirme çalışma zamanı sağlar. Kopyalama işleminin katılan her veri deposunun kurallarını etkinleştirin.

Örneğin, bir şirket içi veri deposundan bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu kopyalamak için aşağıdaki adımları uygulayın:

1. Windows Güvenlik Duvarı hem kurumsal güvenlik duvarının 1433 numaralı bağlantı noktasında giden TCP iletişimine izin verin.
2. İzin verilen IP adreslerinin listesi için şirket içinde barındırılan tümleştirme çalışma zamanı makinenin IP adresini eklemek için Azure SQL veritabanı güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Güvenlik duvarınızı giden bağlantı noktası 1433 izin vermediği durumlarda, şirket içinde barındırılan tümleştirme çalışma zamanının Azure SQL veritabanı doğrudan erişemez. Bu durumda, kullanabileceğiniz bir [kopyalama aşamalı](copy-activity-performance.md) Azure SQL veritabanı ve Azure SQL veri ambarı. Bu senaryoda, veri taşıma işlemi için yalnızca HTTPS (443 numaralı bağlantı noktası) gerekir.


## <a name="proxy-server-considerations"></a>Proxy server konuları
Kurumsal ağ ortamınızı, internet'e bir proxy sunucusu kullanıyorsa, şirket içinde barındırılan tümleştirme çalışma zamanının uygun proxy ayarlarını kullanmak için yapılandırın. Proxy ilk kayıt aşamasında ayarlayabilirsiniz.

![Ara sunucusunu belirtin](media/create-self-hosted-integration-runtime/specify-proxy.png)

Yapılandırıldığında, şirket içinde barındırılan tümleştirme çalışma zamanı Ara sunucu kullanıyorsa ve bulut hizmetine bağlanmak için kaynak / hedef (kullandığınızdan HTTP / HTTPS protokolü). Bu seçim, **değiştir bağlantısını** ilk kurulum sırasında. Proxy ayarı iletişim kutusunu görürsünüz.

![Küme proxy](media/create-self-hosted-integration-runtime/set-http-proxy.png)

Üç yapılandırma seçeneği vardır:

- **Proxy kullanmayın**: Şirket içinde barındırılan tümleştirme çalışma zamanının açıkça her Proxy'yi bulut hizmetlerine bağlanmak için kullanmaz.
- **Sistem Ara sunucu kullanmak**: Şirket içinde barındırılan tümleştirme çalışma zamanı diahost.exe.config ve diawp.exe.config yapılandırılan proxy ayarı kullanır. Proxy diahost.exe.config ve diawp.exe.config yapılandırılmışsa, şirket içinde barındırılan tümleştirme çalışma zamanı bir ara sunucu olmadan doğrudan bulut hizmetine bağlanır.
- **Özel ara sunucu kullanmak**: HTTP proxy diahost.exe.config ve diawp.exe.config yapılandırmaları kullanmak yerine şirket içinde barındırılan tümleştirme çalışma zamanı için kullanılacak ayarı yapılandırın. **Adresi** ve **bağlantı noktası** gereklidir. **Kullanıcı adı** ve **parola** , proxy kimlik doğrulama ayarlarına bağlı olarak isteğe bağlıdır. Tüm ayarları şirket içinde barındırılan tümleştirme çalışma zamanını Windows DPAPI ile şifrelenen ve makinede yerel olarak depolanır.

Integration runtime konak hizmeti, güncelleştirilen proxy ayarlarını kaydettikten sonra otomatik olarak başlatır.

Ara sunucu ayarlarını görüntüleme veya güncelleştirme istiyorsanız, şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla, kaydedildikten sonra Integration Runtime Yapılandırma Yöneticisi'ni kullanın.

1. Açık **Microsoft Integration Runtime Yapılandırma Yöneticisi**.
2. **Ayarlar** sekmesine geçin.
3. Seçin **değişiklik** bağlantısını **HTTP Proxy** açmak için bölüm **Set HTTP Proxy** iletişim kutusu.
4. **İleri**’yi seçin. Daha sonra proxy ayarı kaydetmek izninizi isteyen bir uyarı görürsünüz ve Integration runtime konak hizmeti yeniden başlatın.

Görüntüleyebilir ve Configuration Manager aracını kullanarak HTTP Ara sunucusunu güncelleştirin.

![Görünümü Ara](media/create-self-hosted-integration-runtime/view-proxy.png)

> [!NOTE]
> NTLM kimlik doğrulaması ile bir proxy sunucusu ayarlayın, Integration runtime konak hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabı için parolayı değiştirirseniz, hizmetin yapılandırma ayarlarını güncelleştirmek ve buna göre yeniden unutmayın. Bu gereksinim nedeniyle, parolanızı sık güncelleştirme gerektirmez proxy sunucusuna erişmek için bir özel etki alanı hesabı kullanmanızı öneririz.

### <a name="configure-proxy-server-settings"></a>Ara sunucu ayarlarını yapılandırma

Seçerseniz **sistem Ara sunucu kullanmak** HTTP proxy ayarı, şirket içinde barındırılan tümleştirme çalışma zamanı proxy diahost.exe.config ve diawp.exe.config ayarını kullanır. Proxy diahost.exe.config ve diawp.exe.config belirtilirse, şirket içinde barındırılan tümleştirme çalışma zamanı Ara sunucu üzerinden geçmeden doğrudan bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyayı güncelleştirmek için yönergeler sağlar:

1. Dosya Gezgini'nde, özgün dosyasını yedeklemek için C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config güvenli bir kopyasını oluşturun.
2. Yönetici olarak çalıştırarak Notepad.exe açın ve C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config metin dosyasını açın. Aşağıdaki kodda gösterildiği gibi varsayılan etiket için system.net bulun:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Ardından, aşağıdaki örnekte gösterildiği gibi proxy sunucusu ayrıntılarının ekleyebilirsiniz:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Ek özellikler gibi gerekli ayarları belirlemek için proxy etiketin içinde verilir `scriptLocation`. Bkz: [proxy öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) söz dizimi.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Yapılandırma dosyası, özgün konuma kaydedin. Ardından, şirket içinde barındırılan tümleştirme çalışma zamanı değişiklikleri toplar konak hizmeti yeniden başlatın. 

   Hizmeti yeniden başlatmak için Denetim Masası'ndaki Hizmetler uygulamasını kullanın. Veya Integration Runtime Yapılandırma Yöneticisi'nden **Hizmeti Durdur** düğmesini ve ardından **Hizmeti'ni Başlat**. 
   
   Hizmet başlatılmazsa, bir XML etiket sözdizimi yanlış düzenlendiğinde uygulama yapılandırma dosyasında eklendiğini olasıdır.

> [!IMPORTANT]
> Diahost.exe.config hem diawp.exe.config güncelleştirileceğini unutmayın.

Microsoft Azure, şirketinizin izin verilenler listesinde olduğundan emin olmanız gerekir. Geçerli Microsoft Azure IP adreslerinden listesini indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Olası Belirtiler güvenlik duvarınızdan ve Ara sunucu ilgili sorunlar
Aşağıdaki ayarlara benzer hatalarla karşılaşırsanız, kendi kimliğini doğrulamak için şirket içinde barındırılan tümleştirme çalışma zamanının Data Factory bağlanmasını engelleyen güvenlik duvarı veya Ara sunucunun yanlış yapılandırması nedeniyle olasıdır. Güvenlik Duvarı ve proxy sunucunuzun düzgün şekilde yapılandırıldığından emin olmak için önceki bölüme bakın.

* Şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek çalıştığınızda şu hatayı alıyorsunuz: "Bu Integration Runtime düğümü kaydedemedi! Kimlik doğrulama anahtarının geçerli olduğunu ve tümleştirme hizmeti konak Hizmeti'nin bu makinede çalıştığını onaylayın."
* Tümleştirme çalışma zamanı Yapılandırma Yöneticisi'ni açın, durumunu görmeniz **bağlantısı kesilmiş** veya **bağlanma**. Ne zaman görüntülemekte olduğunuz Windows olay günlükleri, altında **Olay Görüntüleyicisi'ni** > **uygulama ve hizmet günlükleri** > **Microsoft Integration Runtime**, Bunun gibi hata iletileri görürsünüz:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enabling-remote-access-from-an-intranet"></a>Bir Intranet'ten uzaktan erişimi etkinleştirme  
Şirket içinde barındırılan tümleştirme çalışma zamanının yüklendiği dışındaki başka bir makineden (ağ) kimlik bilgilerini şifrelemek için PowerShell kullanıyorsanız, etkinleştirebilirsiniz **Intranet'ten uzaktan erişim** seçeneği. Şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu aynı makinede kimlik bilgilerini şifrelemek için PowerShell çalıştırırsanız, etkinleştiremezsiniz **Intranet'ten uzaktan erişim**.

Etkinleştirmeniz gereken **Intranet'ten uzaktan erişim** yüksek kullanılabilirlik ve ölçeklenebilirlik için başka bir düğüm eklemeden önce.  

Şirket içinde barındırılan tümleştirme çalışma zamanı kurulumu sırasında (daha sonra sürüm 3.3.xxxx.x), varsayılan olarak, şirket içinde barındırılan tümleştirme çalışma zamanı yükleme devre dışı bırakır. **Intranet'ten uzaktan erişim** şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8060 (veya kullanıcı tarafından yapılandırılan bağlantı noktası) el ile açabilirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanını oluşturan ayarlanırken bir güvenlik duvarı sorun yaşıyorsanız, güvenlik duvarını yapılandırma olmadan şirket içinde barındırılan tümleştirme çalışma zamanı yüklemek için aşağıdaki komutu kullanarak deneyin:

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
``` 

Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde 8060 bağlantı noktası açık değil seçerseniz, veri deposu kimlik bilgilerini yapılandırmak için kimlik bilgilerini ayarlama uygulama dışında mekanizmaları kullanın. Örneğin, kullanabileceğiniz **yeni AzDataFactoryV2LinkedServiceEncryptCredential** PowerShell cmdlet'i.


## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeler için aşağıdaki öğreticiye bakın: [Öğretici: Şirket içi verileri buluta](tutorial-hybrid-copy-powershell.md).
