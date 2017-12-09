---
title: "Azure portalında Service Fabric kümesi oluştur | Microsoft Docs"
description: "Bu makalede, Azure anahtar kasası ve Azure portal kullanarak azure'da güvenli bir Service Fabric kümesi ayarlayın açıklar."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 6ddadad6f5697fed006e3f938ef3c3faedb6a354
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Azure portal kullanarak Azure'da bir Service Fabric kümesi oluştur
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Azure portal kullanarak azure'da güvenli bir Service Fabric kümesi ayarlama adımlarını anlatan bir adım adım kılavuz budur. Bu kılavuz aşağıdaki adımlarda size yol gösterir:

* Küme güvenlik anahtarını yönetmek için anahtar kasasını ayarlayın.
* Azure portalı üzerinden güvenli bir küme oluşturma.
* Yöneticiler sertifikaları kullanarak kimlik doğrulaması.

> [!NOTE]
> Azure Active Directory ile kullanıcı kimlik doğrulaması ve uygulama güvenliği için sertifikalar ayarlama gibi daha gelişmiş güvenlik seçenekleri için [Azure Kaynak Yöneticisi'ni kullanarak kümenizi oluşturduktan][create-cluster-arm].
> 
> 

Güvenli bir küme dağıtma, yükseltme ve uygulamaları, hizmetleri ve içerdikleri veriler silme içeren yönetim işlemleri için yetkisiz erişimi engelleyen bir kümedir. Güvenli olmayan bir küme herkes herhangi bir zamanda bağlanmak ve böylelikle yönetim işlemleri bir kümedir. Güvenli olmayan bir küme oluşturmak mümkün olsa da, olan **güvenli bir küme oluşturmak için tavsiye**. Güvenli olmayan bir küme **daha sonra korunamıyor** -yeni bir küme oluşturulması gerekir.

Kümeleri Linux kümeleri veya Windows kümeleri olup kavramları güvenli kümeleri oluşturmak için aynıdır. Güvenli Linux kümeleri oluşturmak için daha fazla bilgi ve yardımcı komut dosyaları için lütfen bkz. [güvenli küme oluşturma](service-fabric-cluster-creation-via-arm.md). Sağlanan yardımcı komut dosyası tarafından alınan parametreleri doğrudan portalda bölümde açıklandığı gibi girilebilir [Azure portalında bir küme oluşturmak](#create-cluster-portal).

## <a name="configure-key-vault"></a>Key Vault'u Yapılandır 
### <a name="log-in-to-azure"></a>Azure'da oturum açma
Bu kılavuzu kullanır [Azure PowerShell][azure-powershell]. Yeni bir PowerShell oturumu başlatılırken Azure hesabınızda oturum açın ve Azure komutları çalıştırmadan önce aboneliğinizi seçin.

Azure hesabınızda oturum açın:

```powershell
Login-AzureRmAccount
```

Aboneliğinizi seçin:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="set-up-key-vault"></a>Anahtar Kasası ayarlama
Kılavuzun bu bölümü Service Fabric uygulamaları ve Azure Service Fabric kümesi için bir anahtar kasası oluşturmada size yol gösterir. Anahtar kasası üzerinde tam bir kılavuz için bkz: [anahtar kasası kullanmaya başlama Kılavuzu][key-vault-get-started].

Service Fabric X.509 sertifikaları bir küme güvenliğini sağlamak için kullanır. Azure anahtar kasası, Azure Service Fabric kümeleri sertifikalarını yönetmek için kullanılır. Bir küme Azure'da dağıtıldığında, Azure kaynak sağlayıcısı Service Fabric kümeleri oluşturmak için sorumlu sertifikaları anahtar Kasası'nı çeker ve VM'ler kümede yükler.

Aşağıdaki diyagramda, anahtar kasası, Service Fabric kümesi ve küme oluşturduğunda, anahtar kasasında depolanan sertifika kullanan Azure kaynak sağlayıcısı arasındaki ilişki gösterilmektedir:

![Sertifika Yükleme][cluster-security-cert-installation]

#### <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma
İlk adım, özellikle anahtar kasası için yeni bir kaynak grubu oluşturmaktır. Anahtarları ve gizli anahtarları kaybetmeden işlem ve depolama kaynak grupları - gibi Service Fabric kümesi sahip olan kaynak grubunu - kaldırabilmeniz anahtar kasası, kendi kaynak grubuna koyma önerilir. Anahtar kasası sahip bir kaynak grubu tarafından kullanıldığı kümesi ile aynı bölgede olması gerekir.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

#### <a name="create-key-vault"></a>Key Vault Oluştur
Bir anahtar kasası yeni kaynak grubu oluşturun. Anahtar kasası **dağıtımı için etkinleştirilmelidir** sertifikaları elde ve küme düğümlerine yüklemek Service Fabric kaynak sağlayıcısı izin vermek için:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

Var olan bir anahtar kasası varsa Azure CLI kullanarak dağıtım için etkinleştirebilirsiniz:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


### <a name="add-certificates-to-key-vault"></a>Anahtar Kasası'na sertifikaları Ekle
Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Service Fabric sertifikaların nasıl kullanıldığını daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

#### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifika, bir küme güvenli ve yetkisiz erişimi önlemek için gereklidir. Birkaç yolla küme güvenlik sağlar:

* **Küme kimlik doğrulaması:** düğümü düğümü iletişimin küme Federasyon kimlik doğrulamasını yapar. Yalnızca bu sertifikayla kimliğini kanıtlamak düğümleri kümeye katılmasını sağlayabilir.
* **Sunucu kimlik doğrulaması:** management istemcisi, gerçek kümeye Konuşmayı bilir böylece bir yönetim istemcisi küme yönetim Uç noktalara kimliğini doğrular. Bu sertifika de SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaca hizmet eder için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir olarak, anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın konu adı, Service Fabric kümesi erişmek için kullanılan etki alanı eşleşmesi gerekir. Bu, kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için SSL sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası elde edemiyor `.cloudapp.azure.com` etki alanı. Kümeniz için özel etki alanı adı satın alır. CA'dan bir sertifika istediğinde sertifikanın konu adı, kümeniz için kullanılan özel etki alanı adı eşleşmelidir.

#### <a name="client-authentication-certificates"></a>İstemci kimlik doğrulama sertifikaları
Ek istemci sertifikalarını Yöneticiler küme yönetim görevleri için kimlik doğrulaması. Service Fabric sahip iki erişim düzeyleri: **yönetici** ve **salt okunur kullanıcı**. En azından, yönetici erişimi için tek bir sertifika kullanılması gerekir. Ek kullanıcı düzeyinde erişim için ayrı bir sertifika sağlanmalıdır. Erişim rolleri hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi][service-fabric-cluster-security-roles].

Anahtar kasası ile Service Fabric çalışma istemci kimlik doğrulama sertifikaları yüklemeniz gerekmez. Bu sertifikalar, yalnızca yetkili kullanıcılar için küme yönetimi için sağlanması gerekir. 

> [!NOTE]
> Azure Active Directory, küme yönetimi işlemleri için istemcilerin kimliğini doğrulamak için önerilen yoldur. Azure Active Directory'yi kullanmak için size gereken [Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Ek sertifikaların herhangi bir sayıda uygulama güvenlik amacıyla bir kümede yüklenebilir. Kümenizi oluşturmadan önce düğümlerine gibi yüklenmesi için bir sertifika gerektiren uygulama güvenlik senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerleri
* Çoğaltma sırasında düğümler arasında veri şifreleme 

Uygulama sertifikaları Azure Portalı aracılığıyla bir küme oluştururken yapılandırılamaz. Küme Kurulum sırasında uygulama sertifikaları yapılandırmak için şunları yapmalısınız [Azure Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak][create-cluster-arm]. Oluşturulduktan sonra uygulama sertifikaları kümeye ekleyebilirsiniz.

#### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Azure kaynak sağlayıcısı kullanmak için sertifikaları biçimlendirme
Özel anahtar dosyaları (.pfx) eklenir ve doğrudan anahtar kasası kullanılır. Ancak, Azure kaynak sağlayıcısı anahtarlarının base-64 olarak .pfx içeren özel bir JSON biçiminde depolanmasını gerektirir kodlanmış dize ve özel anahtar parolası. Bu gereksinimleri karşılamak için anahtarları bir JSON dizesinde yerleştirilir ve gerekir olarak depolanan *gizli* anahtar Kasası'nda.

Bu işlemi kolaylaştırmak için bir PowerShell modülüdür [github'da][service-fabric-rp-helpers]. Modülü kullanmak için aşağıdaki adımları izleyin:

1. Depodaki tüm içeriğini bir yerel dizine indirin. 
2. PowerShell penceresinde modülünü içeri aktarın:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

`Invoke-AddCertToKeyVault` Bu PowerShell modülünü komutu otomatik olarak bir sertifika özel anahtarı bir JSON dizeye biçimlendirir ve anahtar Kasası'na yükler. Küme sertifikası ve herhangi bir ek uygulama sertifika anahtar Kasası'na eklemek için kullanın. Kümenizdeki yüklemek istediğiniz ek sertifika için bu adımı yineleyin.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Bu düğümü kimlik doğrulaması, yönetim uç noktası güvenlik ve kimlik doğrulaması için sertifikaları yükler bir Service Fabric kümesi Resource Manager şablonu ve X.509 sertifikalarını kullanan ek uygulama güvenliği özellikleri yapılandırmak için tüm anahtar kasası önkoşullar bulunmaktadır. Bu noktada, Azure'da şimdi aşağıdaki Kurulumu olması gerekir:

* Anahtar kasası kaynak grubu
  * Anahtar Kasası
    * Küme sunucu kimlik doğrulama sertifikası

< /a "oluşturma-küme-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Azure portalında küme oluşturma
### <a name="search-for-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağı için arama
![Service Fabric kümesi şablonu Azure portalında arayın.][SearchforServiceFabricClusterTemplate]

1. [Azure portalında][azure-portal] oturum açın.
2. Tıklatın **yeni** yeni bir kaynak şablonu eklemek için. Service Fabric kümesi şablon için arama **Market** altında **her şeyi**.
3. Seçin **Service Fabric kümesi** listeden.
4. Gidin **Service Fabric kümesi** dikey penceresinde tıklatın **oluşturma**,
5. **Oluşturma Service Fabric kümesi** dikey penceresinde aşağıdaki dört adım vardır.

#### <a name="1-basics"></a>1. Temel Bilgiler
![Yeni bir kaynak grubu oluşturma ekran görüntüsü.][CreateRG]

Temel bilgiler dikey penceresinde, kümeniz için temel ayrıntıları sağlamanız gerekir.

1. Kümenizin adını girin.
2. Girin bir **kullanıcı adı** ve **parola** VM'ler için Uzak Masaüstü için.
3. Seçtiğinizden emin olun **abonelik** özellikle birden çok aboneliğiniz varsa, dağıtılacak şekilde kümenizi istiyor.
4. Oluşturma bir **yeni kaynak grubu**. Özellikle, dağıtımınız için değişiklik veya kümenizi sildiğinizden çalışırken bunları daha sonra bulma yardımcı olduğundan küme aynı adı vermek en iyisidir.
   
   > [!NOTE]
   > Varolan bir kaynak grubu kullanacak şekilde karar verebilirsiniz rağmen yeni bir kaynak grubu oluşturmak için iyi bir uygulamadır. Bu, ihtiyacınız olmayan kümeleri silmek kolaylaştırır.
   > 
   > 
5. Seçin **bölge** küme oluşturmak istediğiniz. Anahtar kasası bulunduğu aynı bölgede kullanmanız gerekir.

#### <a name="2-cluster-configuration"></a>2. Küme yapılandırması
![Düğüm türü oluşturma][CreateNodeType]

Küme düğümlerinizi yapılandırın. Düğüm türleri, VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlar. Kümenizi birden fazla düğüm türüne sahip olabilir ancak bu düğüm tipi Service Fabric Sistem Hizmetleri yerleştirildiği olarak birincil düğüm türü (portalda tanımladığınız ilk bir) en az beş VM'ler olması gerekir. Yapılandırmayın **yerleşim özellikleri** çünkü "nodetypename"adlı bir varsayılan yerleştirme özelliği otomatik olarak eklenir.

> [!NOTE]
> Yaygın bir senaryo birden çok düğüm türü için bir ön uç hizmeti ve arka uç hizmeti içeren bir uygulamadır. Ön uç hizmeti bağlantı noktalarını açık daha küçük sanal makineleri (örneğin, D2 VM boyutları) Internet'e yerleştirmek istediğiniz, ancak Internet'e yönelik bağlantı noktalarının açık olan (ile VM boyutları D4, D6, D15 vb. gibi) daha büyük VM'ler arka uç hizmetine yerleştirmek istediğiniz.
> 
> 

1. Düğüm türünüz (1-12 karakter yalnızca harfler ve sayılar içeren) için bir ad seçin.
2. En düşük **boyutu** VM'lerin birincil düğüm türü tarafından yönetilir **dayanıklılık** katmanı küme için seçin. Bronz dayanıklılık katmanı için varsayılandır. Dayanıklılık hakkında daha fazla bilgi için bkz: [Service Fabric kümesi güvenilirlik ve dayanıklılık seçme][service-fabric-cluster-capacity].
3. VM boyutu ve fiyatlandırma katmanı seçin. D-serisi VM'ler SSD sürücülerine sahip ve durum bilgisi olan uygulamalar için tavsiye edilir. Kısmi çekirdeğe sahip tüm VM SKU kullanın veya değil 7 GB kullanılabilir disk kapasiteleri daha az sahip. 
4. En düşük **numarası** VM'lerin birincil düğüm türü tarafından yönetilir **güvenilirlik** seçtiğiniz katmanı. Güvenilirlik katmanı için Gümüş varsayılandır. Güvenilirliği hakkında daha fazla bilgi için bkz: [Service Fabric kümesi güvenilirlik ve dayanıklılık seçme][service-fabric-cluster-capacity].
5. Düğüm türü için VM sayısını seçin. Yukarı veya aşağı düğüm türü VM'ler sayısında daha sonra ölçeklendirebilirsiniz, ancak birincil düğüm türünde en düşük seçmiş olduğunuz güvenilirlik düzeyi tarafından yönetilir. Diğer düğüm türleri, en az 1 VM olabilir.
6. Özel uç noktaları yapılandırın. Bu alan, Azure yük dengeleyici, uygulamalarınız için genel internet üzerinden kullanıma sunmak istediğiniz bağlantı noktalarının virgülle ayrılmış bir listesi girmenizi sağlar. Örneğin, bir web uygulaması kümenize dağıtmayı planlıyorsanız, "80" bağlantı noktası 80, kümesine trafiğe izin verecek şekilde olmadığını buraya girin. Uç noktalar hakkında daha fazla bilgi için bkz: [uygulamaları ile iletişim][service-fabric-connect-and-communicate-with-services]
7. Küme yapılandırma **tanılama**. Varsayılan olarak, sorunları gidermenize yardımcı olacak kümenizde tanılama etkinleştirilir. Tanılama değişiklik devre dışı bırakmak istiyorsanız **durum** geç **devre dışı**. Tanılama kapatma olan **değil** önerilir.
8. Kümesi istediğiniz doku yükseltme modu seçin. Seçin **otomatik**, sistemin otomatik olarak en son sürümünü seçin ve kümenizi yükseltmek denemek istiyorsanız. Modu ayarlamak **el ile**, desteklenen bir sürüm seçmek istiyorsanız.

> [!NOTE]
> Service Fabric desteklenen sürümlerini çalıştıran kümeler destekliyoruz. Seçerek **el ile** modu yapmakta üzerinde sorumluluk kümenizi desteklenen bir sürüme yükseltmek için. Doku hakkında daha fazla ayrıntı modu bakın yükseltmek için [service fabric Küme yükseltme belge.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Güvenlik
![Azure Portal'da güvenlik yapılandırmalarını ekran görüntüsü.][SecurityConfigs]

Son adım, bilgi daha önce oluşturduğunuz sertifika ve anahtar kasası kullanarak küme güvenliğini sağlamak için sertifika bilgilerini sağlamaktır.

* Karşıya yükleme alanından elde edilen çıkış birincil sertifikası alanları doldurmak **küme sertifika** anahtar kasası kullanmaya `Invoke-AddCertToKeyVault` PowerShell komutu.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Denetleme **Gelişmiş ayarları Yapılandır** için istemci sertifikaları girmek için kutusunu **yönetici istemci** ve **salt okunur istemci**. Bu alanlarda yönetici istemci sertifikanızın parmak izi ve salt okunur kullanıcının istemci sertifikanızın parmak izi varsa girin. Yöneticiler kümeye bağlanmaya çalıştığında, yalnızca bir sertifika parmak izi değerleri eşleşen bir parmak izi ile varsa erişim buraya girilen verilir.  

#### <a name="4-summary"></a>4. Özet

Küme oluşturma işlemini tamamlamak için tıklatın **Özet** sağladığınız yapılandırmaları bakın veya kümeniz dağıtmak için kullanılan Azure Resource Manager şablonunu indirebilir. Zorunlu ayarları girdikten sonra **Tamam** düğmesi yeşil olur ve küme oluşturma işlemi tıklatarak başlatabilirsiniz.

Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst köşesindeki durum çubuğunun yanında bulunan "Zil" simgesine tıklayın.) Tıkladıysanız **başlangıç panosuna Sabitle** göreceğiniz küme oluştururken **Service Fabric kümesi dağıtma** sabitlenmiş **Başlat** Panosu.

### <a name="view-your-cluster-status"></a>Küme durumunu görüntüleme
![Küme ayrıntıları panosunda ekran görüntüsü.][ClusterDashboard]

Kümenizi oluşturduktan sonra kümenizi portalında inceleyebilirsiniz:

1. Git **Gözat** tıklatıp **Service Fabric kümeleri**.
2. Kümenizi bulun ve tıklatın.
3. Kümenin genel uç noktası ve bir Service Fabric Explorer bağlantısıyla birlikte kümenizin ayrıntılarını panoda görebilirsiniz.

**Düğümü İzleyici** kümenin Pano dikey bölümde sağlıklı ve iyi VM'lerin sayısını gösterir. Kümenin durumu hakkında daha fazla ayrıntı bulabilirsiniz [Service Fabric sistem durumu modeli giriş][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric kümeleri belirli sayıda düğümleri yukarı kullanılabilirliği sürdürmek ve "çekirdek Bakımı olarak" başvurulan durumunu - korumak için her zaman olması gerekir. Bu nedenle, bu genellikle kümedeki tüm makineler ilk yapmadığınız sürece kapatmak üzere güvenli değil bir [tam yedekleme, durumu][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Bir sanal makine ölçek kümesi örneği veya bir küme düğümü için uzaktan bağlanma
Her küme sonuçlarınızın ayarı alınırken bir sanal makine ölçek kümesi içinde belirttiğiniz NodeTypes. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, yönetim kimlik doğrulaması için sertifikaları kullanarak güvenli bir küme var. Ardından, [kümenize bağlanmak](service-fabric-connect-to-secure-cluster.md) ve öğrenin nasıl [uygulama parolaları yönetme](service-fabric-application-secret-management.md).  Ayrıca, öğrenin [Service Fabric destek seçenekleri](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
