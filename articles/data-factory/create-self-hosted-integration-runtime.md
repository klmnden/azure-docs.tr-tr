---
title: Azure Data Factory'de şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma | Microsoft Docs
description: Veri fabrikaları özel ağdaki veri depoları erişmeye izin veren Azure Data factory'de şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 705f2ce674a31d7dda4d87d893078a2ade26e327
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42443399"
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Oluşturma ve şirket içinde barındırılan tümleştirme çalışma zamanını yapılandırma
Integration Runtime (IR) Azure Data Factory tarafından farklı ağ ortamları veri tümleştirme özellikleri sağlamak için kullanılan işlem altyapısıdır. IR hakkında daha fazla ayrıntı için bkz: [tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md).

Şirket içinde barındırılan tümleştirme çalışma zamanı arasında bir bulut veri kopyalama etkinlikleri çalıştırabilen depoları ve özel ağ ve karşı dönüştürme etkinliklerini dağıtma bir veri deposunda işlem kaynaklarını bir şirket içi veya Azure sanal ağ. Şirket içinde barındırılan tümleştirme çalışma zamanı gereksinimleri bir şirket içi makine ya da bir özel ağ içindeki bir sanal makineye yükleyin.  

Bu belgenin nasıl oluşturabileceğinizi ve şirket içinde barındırılan IR yapılandırma tanıtır.

## <a name="high-level-steps-to-install-self-hosted-ir"></a>Kendinden konak IR yüklemek için üst düzey adımları
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. Kendinden konak IR oluşturmak için ADF kullanıcı Arabirimi kullanabilirsiniz. PowerShell örneği aşağıda verilmiştir:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2. Şirket içinde barındırılan tümleştirme çalışma zamanını (yerel makine) yükleyip yeniden açın.
3. Kimlik doğrulama anahtarı almak ve şirket içinde barındırılan tümleştirme çalışma zamanı anahtarı ile kaydedin. PowerShell örneği aşağıda verilmiştir:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="setting-up-self-hosted-ir-on-azure-vm-using-azure-resource-manager-template-automatation"></a>Azure Resource Manager şablonu (automatation) kullanarak Azure sanal makinesinde kendinden konak IR ayarlama
Bir Azure VM kullanarak şirket içinde barındırılan IR Kurulumu otomatik hale getirebilirsiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Bu, (2 veya daha yüksek düğüm sayısı ayarladığınız sürece) yüksek Avalaibility ve ölçeklenebilirlik özelliğiyle Azure VNet içinde tam olarak işlevsel bir şirket içinde barındırılan IR sağlamak için kolay bir yol sağlar.

## <a name="command-flow-and-data-flow"></a>Komut akışını ve veri akışı
Şirket içi ile bulut arasında veri taşıma, etkinlik verileri şirket içi veri kaynağından buluta geçme veya tam tersi aktarmak için bir şirket içinde barındırılan tümleştirme çalışma zamanı kullanır.

Kendinden konak IR ile kopyalama için adımların özeti için üst düzey veri akışı şu şekildedir:

![Yüksek düzey genel bakış](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Veri geliştirici bir şirket içinde barındırılan tümleştirme çalışma zamanı içinde bir PowerShell cmdlet'i kullanılarak bir Azure data factory oluşturur. Şu anda, Azure portalı, bu özelliği desteklemez.
2. Veri geliştirici bir şirket içi veri deposu için bağlı hizmet, veri depolarında bağlanmak için kullanması gereken şirket içinde barındırılan tümleştirme çalışma zamanı örneğini belirterek oluşturur. Bağlı hizmet oluşturma işleminin bir parçası olarak, veri geliştiricisi ' kimlik bilgisi Yöneticisi ' uygulamayı kullanır (şu anda desteklenmiyor) kimlik doğrulama türleri ve kimlik bilgilerini ayarlamak için. Kimlik bilgileri Yöneticisi uygulaması iletişim kutusu, bağlantı ve kimlik bilgilerini kaydetmek için şirket içinde barındırılan tümleştirme çalışma zamanı'nı test etmek için veri deposuyla iletişim kurar.
   - Şirket içinde barındırılan Integration runtime düğümü, Windows Data Protection uygulama programlama arabirimi (DPAPI) kullanarak kimlik bilgilerini şifreler ve yerel olarak kaydeder. Yüksek kullanılabilirlik için birden çok düğüm ayarlarsanız, kimlik bilgilerini diğer düğümler arasında daha fazla eşitlenir. Her düğüm DPAPI kullanarak şifreler ve yerel olarak depolar. Kimlik bilgisi eşitlemesi veri geliştiriciye karşı saydamdır ve şirket içinde barındırılan IR tarafından ele alınır    
   - Data Factory hizmeti için yönetim işleri zamanlama ve şirket içinde barındırılan tümleştirme çalışma zamanı ile iletişim kurar **denetim kanalı** bir paylaşılan Azure service bus kuyruğu kullanan. Bir etkinliği işinin çalıştırılması gerektiğinde, Data Factory istekle birlikte tüm kimlik bilgilerini (kimlik bilgileri zaten şirket içinde barındırılan tümleştirme çalışma zamanını depolanmaz durumda) kuyruğa alır. Şirket içinde barındırılan tümleştirme çalışma zamanı, işi kuyruğa yoklama sonra başlatıyor.
   - Şirket içinde barındırılan tümleştirme çalışma zamanı verileri bulut depolamaya veya tam tersi veri işlem hattında kopyalama etkinliği nasıl yapılandırıldığına bağlı olarak bir şirket içi depolama alanından kopyalar. Bu adım için şirket içinde barındırılan tümleştirme çalışma zamanı doğrudan Azure Blob Depolama gibi bulut tabanlı depolama hizmetlerinde güvenli (HTTPS) kanal üzerinden iletişim kurar.

## <a name="considerations-for-using-self-hosted-ir"></a>Kendinden konak IR kullanma konuları

- Tek şirket içinde barındırılan tümleştirme çalışma zamanının birden çok şirket içi veri kaynakları için kullanılabilir. Ancak, bir **tek şirket içinde barındırılan tümleştirme çalışma zamanı yalnızca bir Azure data factory'ye bağlı** ve başka bir data factory ile paylaşılamaz.
- Sahip olduğunuz **şirket içinde barındırılan tümleştirme çalışma zamanı yalnızca bir örneğini** tek bir makinede yüklü. Şirket içi veri kaynaklarına erişmesi gereken iki veri fabrikaları sahip varsayalım, şirket içinde barındırılan tümleştirme çalışma zamanı iki şirket içi bilgisayarlara yüklemeniz gerekir. Diğer bir deyişle, şirket içinde barındırılan tümleştirme çalışma zamanı için belirli bir veri fabrikası bağlıdır
- **Şirket içinde barındırılan tümleştirme çalışma zamanı veri kaynağı ile aynı makinede olması gerekmez**. Ancak, şirket içinde barındırılan tümleştirme çalışma zamanı daha yakın veri kaynağına veri kaynağına bağlanmak şirket içinde barındırılan tümleştirme çalışma zamanı için süreyi azaltır. Şirket içinde barındırılan tümleştirme çalışma zamanı olandan farklı bir makinede ana şirket içi veri kaynağına yüklemenizi öneririz. Şirket içinde barındırılan tümleştirme çalışma zamanı ve veri kaynağını farklı makinelerde olduğunda, şirket içinde barındırılan tümleştirme çalışma zamanı veri kaynağı ile kaynaklar için rekabet edemez.
- Sahip olduğunuz **farklı makinelerde aynı şirket içi veri kaynağına bağlanan birden çok şirket içinde barındırılan tümleştirme çalışma zamanları**. Örneğin, iki veri fabrikaları hizmet veren iki şirket içinde barındırılan tümleştirme çalışma zamanı olabilir ancak aynı şirket içi veri kaynağı ile veri fabrikaları hem kayıtlı.
- Bilgisayar işlevi gören üzerinde yüklü bir ağ geçidi zaten varsa bir **Power BI** senaryo, yükleme bir **Azure Data Factory için ayrı şirket içinde barındırılan tümleştirme çalışma zamanı** başka bir makine üzerinde.
- Şirket içinde barındırılan tümleştirme çalışma zamanı, Azure sanal ağ içindeki veri tümleştirmesini desteklemek için kullanılmalıdır.
- Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak davran kullanırken bile **ExpressRoute**. Şirket içinde barındırılan tümleştirme çalışma zamanı hizmeti ve veri kaynağı arasında bağlantı kurmak için kullanın.
- Üzerinde bulutta veri depolama alanı olsa bile, şirket içinde barındırılan tümleştirme çalışma zamanı kullanmalısınız bir **Azure Iaas sanal makine**.
- Görevleri hangi FIPS uyumlu üzerinde şifreleme etkin bir Windows Server'da yüklü bir şirket içinde barındırılan tümleştirme çalışma zamanı başarısız olabilir. Bu sorunu geçici olarak çözmek için sunucudaki FIPS uyumlu şifrelemeyi devre dışı bırakın. FIPS uyumlu şifrelemeyi devre dışı bırakmak için 1 (0 (devre dışı) olarak etkin) olan aşağıdaki kayıt defteri değerini Değiştir: `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Önkoşullar

- Desteklenen **işletim sistemi** sürümleridir Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. Şirket içinde barındırılan tümleştirme çalışma zamanını yüklemesini bir **etki alanı denetleyicisi desteklenmiyor**.
- **.NET framework 4.6.1 veya üzerini** gereklidir. Windows 7 makinesinde şirket içinde barındırılan tümleştirme çalışma zamanı'nı yüklüyorsanız, .NET Framework 4.6.1 yükleyin veya üzeri. Bkz: [.NET Framework System Requirements](/dotnet/framework/get-started/system-requirements) Ayrıntılar için.
- Önerilen **yapılandırma** şirket içinde barındırılan tümleştirme çalışma zamanı için en az 2 GHz, 4 çekirdek, 8 GB RAM ve 80 GB disk makinedir.
- Konak makine hazırda bekleme, şirket içinde barındırılan tümleştirme çalışma zamanı veri isteklere yanıt vermez. Bu nedenle, şirket içinde barındırılan Integration runtime'ı yüklemeden önce bilgisayarda uygun bir güç planı yapılandırın. Makine hazırda bekleme için yapılandırılmışsa, şirket içinde barındırılan tümleştirme çalışma zamanı yükleme isteyen bir ileti alırsınız.
- Makinede yüklemek ve şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla yapılandırmak için bir yönetici olması gerekir.
- Kopyalama etkinliği çalıştırma üzerinde belirli bir sıklıkta gerçekleşecek şekilde makinesinde kaynak kullanımı (CPU, bellek), ayrıca en yüksek ve boş kalma sürelerinde ile aynı deseni izler. Kaynak Kullanımı Yoğun taşınan veri miktarı da bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda Yukarı Git bakın.

## <a name="installation-best-practices"></a>Yükleme için en iyi yöntemler
Şirket içinde barındırılan tümleştirme çalışma zamanı, bir MSI Kurulumu paketinden indirerek yüklenebilir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Bkz: [şirket içi ile bulut arasında veri taşıma makale](tutorial-hybrid-copy-powershell.md) adım adım yönergeler için.

- Makine olmayan hazırda bekletme güç planı için şirket içinde barındırılan Integration runtime konak makinedeki yapılandırın. Konak makine hazırda bekleme, şirket içinde barındırılan tümleştirme çalışma zamanı çevrimdışı olarak bırakır.
- Düzenli olarak şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili kimlik bilgilerini yedekle.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Yükleme ve İndirme Merkezi'nden kaydetmek şirket içinde barındırılan IR

1. Gidin [Microsoft Integration Runtime indirme sayfası](https://www.microsoft.com/download/details.aspx?id=39717).
2. Tıklayın **indirme**, uygun sürümünü seçin (**32-bit** vs. **64-bit**), tıklatıp **sonraki**.
3. Çalıştırma **MSI** doğrudan veya sabit disk ve çalışma kaydedin.
4. Üzerinde **Hoş Geldiniz** sayfasında, bir **dil** tıklayın **sonraki**.
5. **Kabul** tıklayın ve son kullanıcı lisans sözleşmesi **sonraki**.
6. Seçin **klasör** şirket içinde barındırılan tümleştirme çalışma zamanını yükleme tıklatıp **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında **yükleme**.
8. Tıklayın **son** yüklemeyi tamamlamak için.
9. Azure PowerShell kullanarak kimlik doğrulama anahtarını alın. PowerShell örneği, kimlik doğrulama anahtarı almak için:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Üzerinde **Integration Runtime (şirket içinde barındırılan) Kaydet** sayfa Microsoft tümleştirme çalışma zamanı yapılandırma makinenizde çalışan Yöneticisi, aşağıdaki adımları uygulayın:
    1. Yapıştırma **kimlik doğrulama anahtarı** metin alanında.
    2. İsteğe bağlı olarak, tıklayın **Show kimlik doğrulama anahtarı** anahtar metnini görmek için.
    3. Tıklayın **kaydetme**.


## <a name="high-availability-and-scalability"></a>Yüksek kullanılabilirlik ve ölçeklenebilirlik
Şirket içinde barındırılan tümleştirme çalışma zamanının birden çok şirket içi makinelere ilişkilendirilebilir. Bu makineler, düğüm olarak adlandırılır. Şirket içinde barındırılan tümleştirme çalışma zamanı ile ilişkili en fazla dört düğümünüz olabilir. Mantıksal bir ağ geçidi için birden çok düğüm (yüklü ağ geçidi ile şirket içi makineler) sahip avantajları şunlardır:
1. BT'nin tek, büyük veri çözümü veya Bulut veri tümleştirme en fazla 4 düğüm süreklilikle sağlama, Azure Data Factory ile bir hata noktası artık, bu nedenle şirket içinde barındırılan Integration Runtime'nın yüksek kullanılabilirlik.
2. Geliştirilmiş performans ve aktarım hızını şirket içi ve bulut arasında veri taşıma sırasında veri depoları. Daha fazla bilgi edinin [performans karşılaştırmalar](copy-activity-performance.md).

Şirket içinde barındırılan tümleştirme çalışma zamanı yazılımı yükleyerek birden çok düğüm ilişkilendirebilirsiniz [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) ve alınan kimlik doğrulaması anahtarlarını biri tarafından kaydetme Bölümünde anlatıldığı gibi AzureRmDataFactoryV2IntegrationRuntimeKey yeni cmdlet [Öğreticisi](tutorial-hybrid-copy-powershell.md)

> [!NOTE]
> Yeni ilişkilendirme her düğüm için şirket içinde barındırılan tümleştirme çalışma zamanı Oluştur gerekmez. Şirket içinde barındırılan tümleştirme çalışma zamanı başka bir makineye yükleyin ve aynı kimlik doğrulama anahtarı kullanarak kaydedin. 

> [!NOTE]
> İçin başka bir düğüm eklemeden önce **yüksek kullanılabilirlik ve ölçeklenebilirlik**, olun **'Uzaktan erişim için intranet'** seçenektir **etkin** 1 düğümünde (Microsoft Integration Runtime Yapılandırma Yöneticisi -> Ayarlar -> Uzak intranet erişimi). 

### <a name="scale-considerations"></a>Ölçek konuları

#### <a name="scale-out"></a>Ölçeği genişletme

Zaman **kendinden konak IR kullanılabilir bellek düşük** ve **CPU kullanımının yüksek olduğu**, yeni bir düğüm ekleme, makineler arasında yük ölçeğinizi yardımcı. Etkinlikler zaman aşımı nedeniyle başarısız olan veya çevrimdışı olmasına IR düğümü şirket içinde barındırılan, bir düğüm için ağ geçidi eklerseniz yardımcı olur.

#### <a name="scale-up"></a>Ölçeği artırma

Kullanılabilir bellek ve CPU iyi kullanılmaz, ancak eşzamanlı iş yürütme sınırına ulaştı, bir düğümde çalıştırılabilen eşzamanlı iş sayısını artırarak ölçeği. Kendinden konak IR aşırı yüklendiği etkinlikler zaman aşımına uğruyor. zaman ölçeği isteyebilirsiniz. Aşağıdaki görüntüde gösterildiği gibi bir düğüm için kapasite üst sınırı artırabilirsiniz.  

![](media\create-self-hosted-integration-runtime\scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>TLS/SSL sertifikası gereksinimleri

Çalışma zamanı düğümleri tümleştirme arasındaki iletişimin güvenliğini sağlamak için kullanılan TLS/SSL sertifika için gereksinimler şunlardır:

- Sertifika genel olarak güvenilir X509 olmalıdır v3 sertifikası. Ortak (üçüncü taraf) sertifika yetkilisi (CA) tarafından verilen sertifikaların kullanmanızı öneririz.
- Her Integration runtime düğümü, bu sertifikaya güvenmeleri gerekir.
- Joker karakterli sertifikalar desteklenir. FQDN adınız ise **node1.domain.contoso.com**, kullanabileceğiniz ***. domain.contoso.com** sertifikanın konu adı olarak.
- SAN sertifika konu diğer adları yalnızca son maddenin kullanılacak ve diğer tüm mevcut sınırlama nedeniyle yoksayılacak önerilmez. Örneğin bir SAN sertifikası, SAN olan sahip **node1.domain.contoso.com** ve **node2.domain.contoso.com**, yalnızca bu sertifika, FQDN: makinede kullanabilirsiniz **node2.domain.contoso.com**.
- SSL sertifikaları için Windows Server 2012 R2 tarafından desteklenen herhangi bir anahtar boyutu destekler.
- CNG kullanarak sertifika anahtarlar desteklenmez.  

## <a name="sharing-the-self-hosted-integration-runtime-ir-with-multiple-data-factories"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı (IR) ile birden çok veri fabrikaları paylaşma

Bir veri fabrikasında Kurulumu zaten olabilir var olan şirket içinde barındırılan tümleştirme çalışma zamanı altyapısını yeniden kullanabilirsiniz. Bu sayede oluşturmak bir **bağlı şirket içinde barındırılan tümleştirme çalışma zamanı** içinde farklı bir veri fabrikası zaten varolan bir başvuru tarafından (paylaşılan) Self şirket içinde barındırılan IR'de.

#### <a name="terminologies"></a>**Terimler**

- **IR paylaşılan** – özgün şirket içinde barındırılan IR, bir fiziksel altyapı üzerinde çalışıyor.  
- **IR bağlı** – başka bir paylaşılan IR başvuran IR Bu mantıksal bir IR ve başka bir şirket içinde barındırılan IR (paylaşılan) altyapısını kullanır.

#### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>Bağlı şirket içinde barındırılan IR oluşturmak için yüksek düzey adımları

Paylaşılacak şirket içinde barındırılan IR,

1. Bağlantılı IR oluşturmak istediğiniz Data factory'ye izin ver 

   ![](media\create-self-hosted-integration-runtime\grant-permissions-IR-sharing.png)

2. Not **kaynak kimliği** paylaşılması için şirket içinde barındırılan IR.

   ![](media\create-self-hosted-integration-runtime\4_ResourceID_self-hostedIR.png)

İzinleri verilmiş olan Data Factory içinde

3. (Bağlı) yeni bir şirket içinde barındırılan IR oluşturma ve yukarıdaki girin **kaynak kimliği**

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_2.png)

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_3.png)

#### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Şirket içinde barındırılan IR paylaşımı bilinen sınırlamalar

1. Tek şirket içinde barındırılan IR altında oluşturulabilecek bağlı IR varsayılan sayısıdır **20**. Daha sonra daha ihtiyacınız varsa desteğe başvurun. 

2. Bağlantılı IR olduğu oluşturulacak veri fabrikasının MSI olmalıdır ([yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). Varsayılan olarak, veri fabrikaları Ibiza portalında oluşturulan veya PowerShell cmdlet'lerini MSI örtük olarak oluşturulmuş olur. Bir Azure noktaları Kaynak Yöneticisi şablonu veya SDK kullanarak data factory oluşturdunuz, ancak bazı durumlarda "**kimlik**" **özelliği ayarlanmalıdır** açıkça Azure noktaları Kaynak Yöneticisi'ni bir veri fabrikası oluşturur emin olmak için bir MSI içeren. 

3. Şirket içinde barındırılan IR sürümü eşit veya 3.8.xxxx.xx'dan büyük olmalıdır. Lütfen [son sürümünü indirin](https://www.microsoft.com/download/details.aspx?id=39717) şirket içinde barındırılan IR

4. Bağlantılı IR olduğu oluşturulacak veri fabrikasının MSI olmalıdır ([yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). Varsayılan olarak, MSI Ibiza portalında veya PowerShell cmdlet'lerini oluşturulan veri fabrikaları olacaktır ([yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)).
örtük olarak oluşturulan, Bununla birlikte, Azure Resource Manager (ARM) şablonu veya SDK'sı ile oluşturulan veri fabrikaları gerektirir "Kimlik" özelliği bir MSI oluşacağından emin olmak için ayarlanacak.

5. ADF .net SDK, bu özelliği destekleyen sürümüdür > 1.1.0 =

6. Bu özelliği destekleyen Azure PowerShell sürümüdür > 6.6.0 = (AzureRM.DataFactoryV2 > 0.5.7 =)

  > [!NOTE]
  > Bu özellik yalnızca Azure Data Factory sürüm 2 kullanılabilir 

## <a name="system-tray-icons-notifications"></a>Sistem Tepsisi Simgeleri / bildirimleri

İmleç sistem tepsisi simgesi/bildirim iletisi taşırsanız, şirket içinde barındırılan tümleştirme çalışma zamanı durumu hakkındaki ayrıntıları bulabilirsiniz.

![Sistem tepsisi bildirimi](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Bağlantı noktaları ve güvenlik duvarı
Göz önünde bulundurmanız gereken iki güvenlik duvarı vardır: **Kurumsal güvenlik duvarınız** kuruluşun merkezi yönlendirici üzerinde çalışan ve **Windows Güvenlik Duvarı** yerel makinede bir arka plan olarak yapılandırılmış olduğu Şirket içinde barındırılan tümleştirme çalışma zamanı yüklenir.

![Güvenlik duvarı](media\create-self-hosted-integration-runtime\firewall.png)

Konumunda **Kurumsal güvenlik duvarınız** düzeyi, aşağıdaki etki alanları ve giden bağlantı noktalarını yapılandırmanız:

Etki alanı adları | Bağlantı Noktaları | Açıklama
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Veri taşıma Hizmeti'nde arka ucu ile iletişim kurmak için kullanılır
*. core.windows.net | 443 | Azure Blob (yapılandırılmışsa) kullanarak hazırlanmış kopya için kullanılan
*.frontend.clouddatahub.net | 443 | Veri taşıma Hizmeti'nde arka ucu ile iletişim kurmak için kullanılır
download.microsoft.com | 443 | Güncelleştirmeleri yüklemek için kullanılan

Adresindeki **Windows Güvenlik Duvarı** düzeyi (makine düzeyi), şu giden bağlantı noktaları normalde etkinleştirilir. Etki alanları ve bağlantı noktalarını uygun şekilde üzerinde şirket içinde barındırılan yapılandırabilirsiniz, tümleştirme çalışma zamanı makine.

> [!NOTE]
> Kaynağınız üzerinde temel / havuzlarını sahip olabileceğiniz beyaz liste ek etki alanları ve giden bağlantı noktaları, Kurumsal/Windows Güvenlik Duvarı'nda.
>
> Bazı bulut veritabanları için (örneğin: Azure SQL veritabanı, Azure Data Lake, vb.), beyaz liste IP adresini güvenlik duvarı yapılandırmalarını şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde gerekebilir.

### <a name="copy-data-from-a-source-to-a-sink"></a>Verileri bir kaynaktan havuza kopyalama
Güvenlik duvarı kuralları düzgün bir şekilde şirket güvenlik duvarı, şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde, Windows Güvenlik Duvarı etkinleştirilir ve kendi veri deposu emin olun. Bu kurallar etkinleştirme, her iki kaynağına bağlanmak ve başarıyla havuz şirket içinde barındırılan tümleştirme çalışma zamanı sağlar. Kopyalama işleminin katılan her veri deposunun kurallarını etkinleştirin.

Örneğin, kopyalama için bir **şirket içi verileri depolamak için bir Azure SQL veritabanı havuz veya bir Azure SQL veri ambarı havuzu**, aşağıdaki adımları uygulayın:

- Gidene izin ver **TCP** bağlantı noktasında iletişime **1433** Windows Güvenlik Duvarı hem de kurumsal bir güvenlik duvarı için.
- İzin verilen IP adreslerinin listesi için şirket içinde barındırılan tümleştirme çalışma zamanı makinenin IP adresini eklemek için Azure SQL sunucusunun güvenlik duvarı ayarlarını yapılandırın.

> [!NOTE]
> Şirket içinde barındırılan tümleştirme çalışma zamanı, güvenlik duvarınızı giden bağlantı noktası 1433 izin vermediği durumlarda, Azure SQL doğrudan erişemez. Bu durumda, kullanabilir [hazırlanmış kopya](copy-activity-performance.md) SQL Azure veritabanına / SQL Azure DW. Bu senaryoda, veri taşıma işlemi için yalnızca HTTPS (443 numaralı bağlantı noktası) gerekir.


## <a name="proxy-server-considerations"></a>Proxy server konuları
Kurumsal ağ ortamınızı, internet'e bir proxy sunucusu kullanıyorsa, uygun proxy ayarlarını kullanmak için şirket içinde barındırılan tümleştirme çalışma zamanı yapılandırın. Proxy ilk kayıt aşamasında ayarlayabilirsiniz.

![Ara sunucusunu belirtin](media\create-self-hosted-integration-runtime\specify-proxy.png)

Şirket içinde barındırılan Integration runtime, bulut hizmetine bağlanmak için proxy sunucusunu kullanır. İlk kurulum sırasında Değiştir bağlantısına tıklayın. Proxy ayarı iletişim kutusunu görürsünüz.

![Küme proxy](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Üç yapılandırma seçeneği vardır:

- **Proxy kullanmayın**: şirket içinde barındırılan tümleştirme çalışma zamanı açıkça kullanmayan her Proxy'yi bulut hizmetlerine bağlanmak için.
- **Sistem Ara sunucu kullanmak**: barındırılan tümleştirme çalışma zamanı kullanan proxy ayarı diahost.exe.config ve diawp.exe.config yapılandırılır. Proxy diahost.exe.config ve diawp.exe.config yapılandırılmışsa, şirket içinde barındırılan tümleştirme çalışma zamanı Ara sunucu üzerinden geçmeden doğrudan bulut hizmetine bağlanır.
- **Özel ara sunucu kullanmak**: HTTP proxy diahost.exe.config ve diawp.exe.config yapılandırmaları kullanmak yerine şirket içinde barındırılan tümleştirme çalışma zamanı, kullanılacak ayarı yapılandırın. Adres ve bağlantı noktası gereklidir. Kullanıcı adı ve parola, proxy kimlik doğrulama ayarlarına bağlı olarak isteğe bağlıdır. Tüm ayarları şirket içinde barındırılan tümleştirme çalışma zamanını Windows DPAPI ile şifrelenen ve makinede yerel olarak depolanır.

Integration runtime konak hizmeti, güncelleştirilen proxy ayarlarını kaydettikten sonra otomatik olarak başlatır.

Ara sunucu ayarlarını görüntüleme veya güncelleştirme istiyorsanız, şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla, kaydedildikten sonra Integration Runtime Yapılandırma Yöneticisi'ni kullanın.

1. Başlatma **Microsoft Integration Runtime Yapılandırma Yöneticisi**.
   - **Ayarlar** sekmesine geçin.
   - Tıklayın **değişiklik** bağlantısını **HTTP Proxy** başlatmak için bölüm **Set HTTP Proxy** iletişim.
   - Tıkladıktan sonra **sonraki** düğmesi, proxy ayarını kaydedin ve Integration Runtime konak hizmeti yeniden başlatmak için izninizi isteyen bir uyarı iletişim kutusu görürsünüz.

Görüntüleyebilir ve Configuration Manager aracını kullanarak HTTP Ara sunucusunu güncelleştirin.

![Görünümü Ara](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> NTLM kimlik doğrulaması ile bir proxy sunucusu ayarlayın, Integration runtime konak hizmeti etki alanı hesabı altında çalışır. Daha sonra etki alanı hesabı için parolayı değiştirirseniz, hizmeti için yapılandırma ayarlarını güncelleştirme ve buna göre yeniden unutmayın. Bu gereksinimi nedeniyle parolanızı sık güncelleştirme gerektirmez proxy sunucusuna erişmek için bir özel etki alanı hesabı kullanmanızı öneririz.

### <a name="configure-proxy-server-settings"></a>Ara sunucu ayarlarını yapılandırma

Seçerseniz **sistem Ara sunucu kullanmak** HTTP proxy ayarı, şirket içinde barındırılan tümleştirme çalışma zamanı proxy diahost.exe.config ve diawp.exe.config ayarını kullanır. Proxy diahost.exe.config ve diawp.exe.config belirtilirse, şirket içinde barındırılan tümleştirme çalışma zamanı Ara sunucu üzerinden geçmeden doğrudan bulut hizmetine bağlanır. Aşağıdaki yordam diahost.exe.config dosyayı güncelleştirmek için yönergeler sağlar.

1. Dosya Gezgini'nde, özgün dosyasını yedeklemek için C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config güvenli bir kopyasını oluşturun.
2. Yönetici olarak çalıştırdığınızdan Notepad.exe başlatın ve "C:\Program Files\Microsoft tümleştirme Runtime\3.0\Shared\diahost.exe.config. metin dosyasını açın Aşağıdaki kodda gösterildiği gibi varsayılan etiket için system.net bulun:

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

    Ek özellikler scriptLocation gibi gerekli ayarları belirlemek için proxy etiketin içinde izin verilir. Bkz: [proxy öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) söz dizimi.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Özgün konumuna yapılandırma dosyasını kaydedin ve ardından değişiklikleri toplar şirket içinde barındırılan Integration Runtime konak hizmeti yeniden başlatın. Hizmeti yeniden başlatmayı: Denetim Masası'ndan ya da hizmetler uygulamasını kullanın **Integration Runtime Yapılandırma Yöneticisi** > tıklayın **Hizmeti Durdur** düğmesine ve ardından tıklayın **Başlat Hizmet**. Hizmet başlatılmazsa, bir XML etiket sözdizimi yanlış düzenlendiğinde uygulama yapılandırma dosyasına eklendi olasıdır.

> [!IMPORTANT]
> Diahost.exe.config hem diawp.exe.config güncelleştirileceğini unutmayın.

Bu noktaları ek olarak, ayrıca Microsoft Azure, şirketinizin izin verilenler listesinde olduğundan emin olmak gerekir. Geçerli Microsoft Azure IP adreslerinin listesi indirilebileceğini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Olası Belirtiler güvenlik duvarınızdan ve Ara sunucu ilgili sorunlar
Aşağıdaki ayarlara benzer hatalarla karşılaşırsanız, kendi kimliğini doğrulamak için şirket içinde barındırılan tümleştirme çalışma zamanı, Data Factory bağlanmasını engelleyen güvenlik duvarı veya Ara sunucunun yanlış yapılandırması nedeniyle olasıdır. Güvenlik duvarınızı emin olmak için önceki bölüme bakın ve proxy sunucusu düzgün yapılandırılmış.

1. Şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek çalıştığınızda şu hatayı alıyorsunuz: "Bu Integration Runtime düğümü kaydedemedi! Kimlik doğrulama anahtarının geçerli olduğunu ve tümleştirme konak hizmeti bu makinede çalıştığını onaylayın. "
   - Tümleştirme çalışma zamanı Yapılandırma Yöneticisi'ni açın, durum olarak gördüğünüz "**bağlantısı kesilmiş**"veya"**bağlanma**". Windows olay günlükleri, "Olay Görüntüleyicisi" görüntülerken > "Uygulama ve hizmet günlükleri" > "Microsoft Integration Runtime", şu hata gibi hata iletilerine bakın:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-intranet"></a>Intranet'ten uzaktan erişimi etkinleştirin  
Kullandığınız durumda **PowerShell** veya **kimlik bilgileri Yöneticisi uygulamasının** şirket içinde barındırılan tümleştirme çalışma zamanı, ardından yüklendiği dışındaki başka bir makineden (ağ) kimlik bilgilerini şifrelemek için gerektirecek **'Intranet'ten uzaktan erişim'** etkinleştirilmesi için seçeneği. Çalıştırırsanız **PowerShell** veya **kimlik bilgileri Yöneticisi uygulamasının** şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu, ardından aynı makinede kimlik bilgisi şifrelenemedi **' uzaktan erişim intranet '** etkin değil.

Intranet'ten uzak erişim olmalıdır **etkin** için başka bir düğüm eklemeden önce **yüksek kullanılabilirlik ve ölçeklenebilirlik**.  

Şirket içinde barındırılan tümleştirme çalışma zamanı kurulumu sırasında (ve sonraki sürümlerde v 3.3.xxxx.x), varsayılan olarak, şirket içinde barındırılan tümleştirme çalışma zamanı yükleme devre dışı bırakır. **'Intranet'ten uzaktan erişim'** şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde.

Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, bağlantı noktası 8060 (veya kullanıcı tarafından yapılandırılmış bağlantı noktası) el ile açabilirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı Kurulum sırasında güvenlik duvarı sorunu çalıştırırsanız, güvenlik duvarını yapılandırma olmadan şirket içinde barındırılan tümleştirme çalışma zamanı yüklemek için aşağıdaki komutu kullanarak deneyebilirsiniz.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> **Kimlik bilgileri Yöneticisi uygulamasının** henüz ADFv2 kimlik bilgilerini şifrelemek için kullanılabilir değil. Daha sonra bu destek ekleyeceğiz.  

Şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde 8060 bağlantı noktası açık değil kullanmayı tercih ederseniz kullanarak dışında mekanizmaları kullanma ** kimlik bilgilerini ayarlama ** veri deposu kimlik bilgilerini yapılandırmak için uygulama. Örneğin, New-AzureRmDataFactoryV2LinkedServiceEncryptCredential PowerShell cmdlet'ini kullanabilirsiniz. Bkz. nasıl veri kimlik bilgilerini depolama kimlik bilgilerini ayarlama ve güvenlik bölümü ayarlanabilir.


## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeler için şu öğreticilere bakın: [öğretici: şirket içi verileri buluta](tutorial-hybrid-copy-powershell.md).
