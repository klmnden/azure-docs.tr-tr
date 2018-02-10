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
ms.date: 02/01/2018
ms.author: chackdan
ms.openlocfilehash: 7537d7015ee8739be4b9ba08846866d4cfbe38be
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Azure portal kullanarak Azure'da bir Service Fabric kümesi oluştur
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portalı](service-fabric-cluster-creation-via-portal.md)
> 
> 

Azure portal kullanarak azure'da bir Service Fabric kümesi (Linux veya Windows) ayarlama adımlarını anlatan bir adım adım kılavuz budur. Bu kılavuz aşağıdaki adımlarda size yol gösterir:

* Azure portalı üzerinden bir küme oluşturma.
* Yöneticiler sertifikaları kullanarak kimlik doğrulaması.

> [!NOTE]
> Azure Active Directory ile kullanıcı kimlik doğrulaması ve uygulama güvenliği için sertifikalar ayarlama gibi daha gelişmiş güvenlik seçenekleri için [Azure Kaynak Yöneticisi'ni kullanarak kümenizi oluşturduktan][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Küme güvenliği 
Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Service Fabric sertifikaların nasıl kullanıldığını daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

Bu ilk kez ise bir service fabric kümesi oluşturuluyor ve test iş yükleri için bir küme dağıtımı, sonraki bölüme atlayabilirsiniz (**Azure Portalı'nda Küme Oluştur**) ve sistem için gerekli sertifikaları oluşturun test iş yükleri çalıştıran kümeleri. Bir küme üretim iş yükleri için ayarlıyorsanız okuma devam edin.

#### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifika, bir küme güvenli ve yetkisiz erişimi önlemek için gereklidir. Birkaç yolla küme güvenlik sağlar:

* **Küme kimlik doğrulaması:** düğümü düğümü iletişimin küme Federasyon kimlik doğrulamasını yapar. Yalnızca bu sertifikayla kimliğini kanıtlamak düğümleri kümeye katılmasını sağlayabilir.
* **Sunucu kimlik doğrulaması:** management istemcisi, gerçek kümeye Konuşmayı bilir böylece bir yönetim istemcisi küme yönetim Uç noktalara kimliğini doğrular. Bu sertifika de SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaca hizmet eder için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir olarak, anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın **konu adı, etki alanı eşleşmelidir** Service Fabric kümesi erişmek için kullanılır. Bu, kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için SSL sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası elde edemiyor `.cloudapp.azure.com` etki alanı. Kümeniz için özel etki alanı adı satın alır. CA'dan bir sertifika istediğinde sertifikanın konu adı, kümeniz için kullanılan özel etki alanı adı eşleşmelidir.

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

< /a "oluşturma-küme-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Azure portalında küme oluşturma

Uygulama gereksinimlerinizi karşılamak için bir üretim kümesi oluşturursunuz bazı planlama ile yardımcı olmak için okuduğunuzdan ve anladığınızdan kesinlikle önerilir [planlama konuları Service Fabric kümesi] [ service-fabric-cluster-capacity] belge. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağı için arama
![Service Fabric kümesi şablonu Azure portalında arayın.][SearchforServiceFabricClusterTemplate]

1. [Azure portalında][azure-portal] oturum açın.
2. Tıklatın **yeni** yeni bir kaynak şablonu eklemek için. Service Fabric kümesi şablon için arama **Market** altında **her şeyi**.
3. Seçin **Service Fabric kümesi** listeden.
4. Gidin **Service Fabric kümesi** dikey penceresinde tıklatın **oluşturma**,
5. **Oluşturma Service Fabric kümesi** dikey penceresinde aşağıdaki dört adım vardır:

#### <a name="1-basics"></a>1. Temel Bilgiler
![Yeni bir kaynak grubu oluşturma ekran görüntüsü.][CreateRG]

Temel bilgiler dikey penceresinde, kümeniz için temel ayrıntıları sağlamanız gerekir.

1. Kümenizin adını girin.
2. Girin bir **kullanıcı adı** ve **parola** VM'ler için Uzak Masaüstü için.
3. Seçtiğinizden emin olun **abonelik** özellikle birden çok aboneliğiniz varsa, dağıtılacak şekilde kümenizi istiyor.
4. Oluşturma bir **yeni kaynak grubu**. Özellikle, dağıtımınız için değişiklik veya kümenizi sildiğinizden çalışırken bunları daha sonra bulma yardımcı olduğundan küme aynı adı vermek en iyisidir.
   
   > [!NOTE]
   > Varolan bir kaynak grubu kullanacak şekilde karar verebilirsiniz rağmen yeni bir kaynak grubu oluşturmak için iyi bir uygulamadır. Bu, kümeler ve kullandığı tüm kaynakları silmek kolaylaştırır.
   > 
   > 
5. Seçin **bölge** küme oluşturmak istediğiniz. Bir anahtar kasası zaten yüklenmiş var olan bir sertifikayı kullanmayı planlıyorsanız, anahtar kasanızı bulunduğu aynı bölgede kullanmanız gerekir. 

#### <a name="2-cluster-configuration"></a>2. Küme yapılandırması
![Düğüm türü oluşturma][CreateNodeType]

Küme düğümlerinizi yapılandırın. Düğüm türleri, VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlar. Kümenizi birden fazla düğüm türüne sahip olabilir ancak bu düğüm tipi Service Fabric Sistem Hizmetleri yerleştirildiği olarak birincil düğüm türü (portalda tanımladığınız ilk bir) en az beş VM'ler olması gerekir. Yapılandırmayın **yerleşim özellikleri** çünkü "nodetypename"adlı bir varsayılan yerleştirme özelliği otomatik olarak eklenir.

> [!NOTE]
> Yaygın bir senaryo birden çok düğüm türü için bir ön uç hizmeti ve arka uç hizmeti içeren bir uygulamadır. Ön uç hizmeti daha küçük vm'lerde (VM boyutları D2_V2 gibi), bağlantı noktalarının açık Internet'e yerleştirin ve Internet'e yönelik bağlantı noktası açık olan (ile VM boyutları D3_V2, D6_V2, D15_V2 vb. gibi) daha büyük sanal makineleri üzerinde arka uç hizmeti put istiyor.
> 
> 

1. Düğüm türünüz (1-12 karakter yalnızca harfler ve sayılar içeren) için bir ad seçin.
2. En düşük **boyutu** VM'lerin birincil düğüm türü tarafından yönetilir **dayanıklılık** katmanı küme için seçin. Bronz dayanıklılık katmanı için varsayılandır. Dayanıklılık hakkında daha fazla bilgi için bkz: [Service Fabric kümesi dayanıklılık seçme][service-fabric-cluster-durability].
3. VM boyutunu seçin. D-serisi VM'ler SSD sürücülerine sahip ve durum bilgisi olan uygulamalar için tavsiye edilir. Kullanmayın kısmi çekirdeğe sahip tüm VM SKU veya 10 GB kullanılabilir disk kapasiteleri daha az sahip. Başvurmak [göz önünde bulundurarak belge planlama service fabric kümesi] [ service-fabric-cluster-capacity] VM boyutunu seçme hakkında Yardım için.
4. Düğüm türü için VM sayısını seçin. Yukarı veya aşağı düğüm türü VM'ler sayısında daha sonra ölçeklendirebilirsiniz, ancak birincil düğüm türünde beş üretim iş yükleri için en düşük gereksinimdir. Diğer düğüm türleri, en az bir VM'ye sahip olabilir. En düşük **numarası** birincil düğüm türü sürücüler için VM'lerin **güvenilirlik** kümenizin.  
5. **Tek düğümlü bir küme ve üç düğümlü kümeler** -bunlar yalnızca test kullanıma yöneliktir. Çalışan tüm üretim iş yükleri için desteklenmez.
6. Özel uç noktaları yapılandırın. Bu alan, Azure yük dengeleyici, uygulamalarınız için genel internet üzerinden kullanıma sunmak istediğiniz bağlantı noktalarının virgülle ayrılmış bir listesi girmenizi sağlar. Örneğin, bir web uygulaması kümenize dağıtmayı planlıyorsanız, "80" bağlantı noktası 80, kümesine trafiğe izin verecek şekilde olmadığını buraya girin. Uç noktalar hakkında daha fazla bilgi için bkz: [uygulamaları ile iletişim][service-fabric-connect-and-communicate-with-services]
7. Küme yapılandırma **tanılama**. Varsayılan olarak, sorunları gidermenize yardımcı olacak kümenizde tanılama etkinleştirilir. Tanılama değişiklik devre dışı bırakmak istiyorsanız **durum** geç **devre dışı**. Tanılama kapatma olan **değil** önerilir. Ardından oluşturulan Application Insights proje zaten varsa, böylece uygulama izlemelerini kendisine yönlendirilir, anahtarı verin.
8. Kümesi istediğiniz doku yükseltme modu seçin. Seçin **otomatik**, sistemin otomatik olarak en son sürümünü seçin ve kümenizi yükseltmek denemek istiyorsanız. Modu ayarlamak **el ile**, desteklenen bir sürüm seçmek istiyorsanız. Doku hakkında daha fazla ayrıntı modu bakın yükseltmek için [service fabric Küme yükseltme belge.][service-fabric-cluster-upgrade]

> [!NOTE]
> Service Fabric desteklenen sürümlerini çalıştıran kümeler destekliyoruz. Seçerek **el ile** modu yapmakta üzerinde sorumluluk kümenizi desteklenen bir sürüme yükseltmek için. > 
> 

#### <a name="3-security"></a>3. Güvenlik
![Azure Portal'da güvenlik yapılandırmalarını ekran görüntüsü.][BasicSecurityConfigs]

Güvenli bir sınama kümesi ayarı sizin için kolaylaştırmak için sağladık **temel** seçeneği. Zaten bir sertifikaya sahip, keyvault karşıya (ve anahtar kasası dağıtım için etkin değilse), ardından kullanmak **özel** seçeneği

#####<a name="basic-option"></a>Temel seçeneği
Eklemek veya mevcut bir keyvault yeniden ve bir sertifika eklemek için ekranları izleyin. Sertifika eklenmesi zaman uyumlu bir işlemdir ve böylece sertifika oluşturulmasını beklemek zorunda kalır.

Yukarıdaki işlem tamamlanana kadar uzağa doğru ekran gezinme, buradaki eðilim kaçının.

![CreateKeyVault]

Sertifika, keyvault eklenir, erişim ilkeleri, Keyvault için düzenleme isteyen aşağıdaki ekran görebilirsiniz. tıklayın **için düzenleme erişim ilkeleri.** tıklayın.

![CreateKeyVault2]

Gelişmiş erişim ilkeleri'ne tıklayın ve sanal makinelere erişim dağıtımı için etkinleştirin. Şablon dağıtımı da etkinleştirmeniz önerilir.

![CreateKeyVault3]

Şimdi Oluştur küme işleminin geri kalanı için devam etmek hazırsınız.

![CreateKeyVault4]

#####<a name="custom-option"></a>Özel seçeneği
' Ndaki adımları zaten gerçekleştirdiyseniz, bu bölüm atlayın **temel** seçeneği.

![SecurityCustomOption]

CertificateThumbprint, SourceVault ve CertificateURL bilgi güvenlik sayfası tamamlanması gerekir. Elinizin altında yoksa başka bir tarayıcı penceresi açın ve aşağıdakileri yapın


1. Keyvault gidin, sertifikayı seçin. 
2. "Özellikler" sekmesini seçin ve bir tarayıcı penceresinde "kaynak anahtar kasasına" 'Kaynak kimliği' kopyalayın 

    ![CertInfo0]

3. Şimdi, "Sertifikalar" sekmesini seçin.
4. Sürümleri sayfasına gidersiniz sertifika parmak izi'i tıklatın.
5. Geçerli sürümü'nün altında gördüğünüz GUID'ler tıklayın.

    ![CertInfo1]

6. Artık aşağıdaki gibi ekranında olması gerekir. "Sertifika parmak izi" bir tarayıcı penceresinde 'Parmak izi' kopyalayın
7. Diğer tarayıcı penceresinde "Sertifika URL'ye" 'Gizli tanımlayıcı' bilgileri kopyalayın.


![CertInfo2]


Denetleme **Gelişmiş ayarları Yapılandır** için istemci sertifikaları girmek için kutusunu **yönetici istemci** ve **salt okunur istemci**. Bu alanlarda yönetici istemci sertifikanızın parmak izi ve salt okunur kullanıcının istemci sertifikanızın parmak izi varsa girin. Yöneticiler kümeye bağlanmaya çalıştığında, yalnızca bir sertifika parmak izi değerleri eşleşen bir parmak izi ile varsa erişim buraya girilen verilir.  

#### <a name="4-summary"></a>4. Özet

Şimdi küme dağıtmaya hazır olursunuz. Bunu yapmadan önce sertifika arama bağlantısı için büyük mavi bilgilendirme kutu içinde indirin. Sertifika güvenli bir yerde sakladığınızdan emin olun. kümenize bağlanmak için gerekir. Yüklediğiniz sertifikanın bir parola yok olduğundan, bir ekleme önerilir.

Küme oluşturma işlemini tamamlamak için tıklatın **oluşturma**. İsteğe bağlı olarak şablonunu indirebilirsiniz. 

![Özet]

Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst köşesindeki durum çubuğunun yanında bulunan "Zil" simgesine tıklayın.) Kümeyi oluştururken **Başlangıç Panosuna Sabitle**’ye tıkladıysanız, **Service Fabric Kümesi Dağıtılıyor** öğesinin **Başlangıç** panosuna sabitlendiğini görürsünüz.

Powershell veya CLI kullanarak kümenizde yönetim işlemlerini gerçekleştirmek için gerekir, kümeye bağlanmak, okuma daha fazla bilgi için nasıl [, kümeye bağlanma](service-fabric-connect-to-secure-cluster.md).

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
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
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
[BasicSecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/BasicSecurityConfigs.PNG
[CreateKeyVault]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault.PNG
[CreateKeyVault2]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault2.PNG
[CreateKeyVault3]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault3.PNG
[CreateKeyVault4]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault4.PNG
[CertInfo0]: ./media/service-fabric-cluster-creation-via-portal/CertInfo0.PNG
[CertInfo1]: ./media/service-fabric-cluster-creation-via-portal/CertInfo1.PNG
[CertInfo2]: ./media/service-fabric-cluster-creation-via-portal/CertInfo2.PNG
[SecurityCustomOption]: ./media/service-fabric-cluster-creation-via-portal/SecurityCustomOption.PNG
[DownloadCert]: ./media/service-fabric-cluster-creation-via-portal/DownloadCert.PNG
[Özet]: ./media/service-fabric-cluster-creation-via-portal/Summary.PNG
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
