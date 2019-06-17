---
title: Azure portalında bir Service Fabric kümesi oluşturma | Microsoft Docs
description: Azure portalı ve Azure anahtar Kasası'nı kullanarak azure'da güvenli Service Fabric kümesi ayarlama konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/06/2018
ms.author: aljo
ms.openlocfilehash: 02312a19c687908b0e1c0e6417dc6b0a9df23912
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125094"
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Azure portalını kullanarak Azure'da bir Service Fabric kümesi oluşturma
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Azure portalını kullanarak azure'da bir Service Fabric kümesine (Linux veya Windows) ayarlama adımlarında size kılavuzluk eden adım adım kılavuzu budur. Bu kılavuz, aşağıdaki adımları gösterilmektedir:

* Bir küme, Azure portalı üzerinden Azure içinde oluşturun.
* Yöneticiler sertifikaları kullanarak kimlik doğrulaması.

> [!NOTE]
> Azure Active Directory ile kullanıcı kimlik doğrulaması ve uygulama güvenliği için sertifikalar ayarlama gibi daha gelişmiş güvenlik seçenekleri için [Azure Resource Manager kullanarak kümenizi oluşturma][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Küme güvenliği 
Sertifikalar, Service Fabric’te bir küme ile uygulamalarının çeşitli yönlerini güvenli hale getirmek üzere kimlik doğrulaması ve şifreleme sağlamak için kullanılır. Sertifikaların Service Fabric'te nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları][service-fabric-cluster-security].

Bu ilk kez kullanıyorsanız, service fabric kümesi oluşturma veya test iş yükleri için bir küme dağıtımı, sonraki bölüme atlayabilirsiniz (**Azure Portal'da küme oluştur**) ve sisteminiz için gerekli sertifikaları oluşturma test iş yükleri çalıştıran kümeleri. Üretim iş yükleri için bir küme ayarlıyorsanız, sonra okumaya devam edin.

#### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifika, küme güvenliğini sağlama ve yetkisiz erişimi önlemek için gereklidir. Küme güvenliği birkaç yolla olanakları sunar:

* **Küme kimlik doğrulaması:** Düğümden düğüme iletişim için küme Federasyon kimlik doğrulaması yapar. Bu sertifika ile kimliğini kanıtlamak düğüm kümesine katılabilirsiniz.
* **Sunucu kimlik doğrulaması:** Böylece gerçek bir küme Bahsediyor yönetim istemci bildiği bir yönetim istemcisinde küme yönetimi Uç noktalara kimliğini doğrular. Bu sertifika da SSL için HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar.

Bu amaçla için sertifikanın aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın **konu adı, etki alanı eşleşmelidir** Service Fabric kümesine erişmek için kullanılır. Bu, kümenin HTTPS yönetim uç noktalarına ve Service Fabric Explorer için SSL sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor `.cloudapp.azure.com` etki alanı. Kümeniz için özel etki alanı edinin. CA'nızdan bir sertifikayı isterken sertifika konu adı kümeniz için kullanılan özel etki alanı adı eşleşmelidir.

#### <a name="client-authentication-certificates"></a>İstemci kimlik doğrulama sertifikaları
Ek istemci sertifikalarını Yöneticiler küme yönetim görevleri için kimlik doğrulaması. Service Fabric iki erişim düzeyi vardır: **yönetici** ve **salt okunur kullanıcı**. En azından, yönetici erişimi için tek bir sertifika kullanılması gerekir. Ek kullanıcı düzeyinde erişim için ayrı bir sertifika sağlanmalıdır. Erişim rolleri hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi][service-fabric-cluster-security-roles].

Key Vault, Service Fabric ile çalışmak için istemci doğrulama sertifikaları yüklemek gerekmez. Bu sertifikalar yalnızca yetkili kullanıcılar için küme yönetimi sağlanması gerekir. 

> [!NOTE]
> Azure Active Directory, küme yönetimi işlemleri için istemcilerin kimliğini doğrulamak için önerilen yoldur. Azure Active Directory'yi kullanmak için şunları yapmanız gerekir [Azure Resource Manager kullanarak küme oluşturma][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Uygulama güvenlik nedenleriyle bir kümedeki herhangi bir sayıda ek sertifikalar yüklenebilir. Kümenizi oluşturmadan önce düğümler üzerinde gibi yüklenmesi için bir sertifika gerektiren uygulama güvenliği senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerleri
* Çoğaltma sırasında düğümler arasında verileri şifreleme 

Uygulama sertifikaları olamaz ne zaman yapılandırılmış [Azure Portal'dan bir küme oluşturma](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-creation-via-portal.md). Küme Kurulum sırasında uygulama sertifikaları yapılandırmak için şunları yapmanız gerekir [Azure Resource Manager kullanarak küme oluşturma][create-cluster-arm]. Oluşturulduktan sonra kümeye uygulama sertifika da ekleyebilirsiniz.

## <a name="create-cluster-in-the-azure-portal"></a>Azure portalda küme oluşturma

Uygulama ihtiyaçlarınızı karşılayabilmek için bir üretim kümesi oluşturma içerir bazı planlama ile yardımcı olmak için okumanız ve anlamanız önemle tavsiye edilir [planlama konuları Service Fabric kümesi] [ service-fabric-cluster-capacity] belge. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Service Fabric küme kaynağı arayın

[Azure portalında][azure-portal] oturum açın.
Tıklayın **kaynak Oluştur** yeni bir kaynak şablonu eklemek için. Service Fabric kümesi şablonunun arama **Market** altında **her şeyi**.
Seçin **Service Fabric kümesi** listeden.

![Azure portalında Service Fabric küme şablonu arayın.][SearchforServiceFabricClusterTemplate]

Gidin **Service Fabric kümesi** dikey penceresinde de **Oluştur**.

**Oluşturma Service Fabric kümesi** dikey penceresinde aşağıdaki dört adım vardır:

### <a name="1-basics"></a>1. Temel Bilgiler
![Yeni bir kaynak grubu oluşturma işleminin ekran görüntüsü.][CreateRG]

Temel bilgiler dikey penceresinde, kümeniz için temel ayrıntılarını sağlamanız gerekir.

1. Kümenizin adını girin.
2. Girin bir **kullanıcı adı** ve **parola** VM'ler için Uzak Masaüstü için.
3. Seçtiğinizden emin olun **abonelik** özellikle birden fazla aboneliğiniz varsa, kümeniz için dağıtılacak istiyor.
4. Yeni bir **kaynak grubu**. Özellikle dağıtımınız için değişiklik veya kümenizi sildiğinizden çalışırken bunları daha sonra bulma konusunda yardımcı olduğundan küme adıyla aynı sağlamak en iyisidir.
   
   > [!NOTE]
   > Mevcut bir kaynak grubunu kullanmak karar olsa da, yeni bir kaynak grubu oluşturmak için iyi bir uygulamadır. Bu, kümeler ve kullandığı tüm kaynakları silmek kolaylaştırır.
   > 
   > 
5. Seçin **konumu** kümeyi oluşturmak istediğiniz. Önceden bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmayı planlıyorsanız, anahtar kasanızın bulunduğu aynı bölgede kullanmanız gerekir. 

### <a name="2-cluster-configuration"></a>2. Küme yapılandırması
![Düğüm türü oluşturma][CreateNodeType]

Küme düğümlerini yapılandırın. Düğüm türleri VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlayın. Kümenizi birden fazla düğüm türü olabilir, ancak bu düğüm türü Service Fabric sistem hizmetlerinin yerleştirildiği olduğu en az beş VM birincil düğüm türü (portalda tanımladığınız ilk öğe) olmalıdır. Yapılandırmayın **yerleşim özellikleri** çünkü nodetypename "adlı" varsayılan yerleştirme özelliğini otomatik olarak eklenir.

> [!NOTE]
> Birden çok düğüm türleri için yaygın bir senaryo bir ön uç hizmeti ve arka uç hizmeti içeren bir uygulamadır. Ön uç hizmeti bağlantı noktalarını açık daha küçük sanal makineleri (VM boyutları D2_V2 gibi) Internet'e yerleştirin ve arka uç hizmeti büyük Vm'lerle (VM boyutları D3_V2, D6_V2, D15_V2 vb. gibi) ile Internet'e yönelik bağlantı noktalarını açık yerleştirmek istediğiniz.
> 

1. (1-12 karakter yalnızca harfler ve sayılar içeren), düğüm türü için bir ad seçin.
2. En düşük **boyutu** VM'lerin birincil düğüm türü tarafından yönetilir **dayanıklılık katmanı** küme için seçin. Bronz dayanıklılık katmanı için varsayılandır. Dayanıklılık hakkında daha fazla bilgi için bkz. [Service Fabric kümesi dayanıklılık seçme][service-fabric-cluster-durability].
3. Seçin **sanal makine boyutu**. D serisi VM'ler, SSD sürücülerine sahip ve durum bilgisi olan uygulamalar için kesinlikle önerilir. Kısmi çekirdeği olan sanal makine SKU'sunu kullanın değil veya 10 GB'tan az kullanılabilir disk kapasitesine sahip. Başvurmak [planlama konuları service fabric kümesi] [ service-fabric-cluster-capacity] VM boyutunu seçme hakkında Yardım için.
4.  **Tek düğümlü bir küme ve üç düğümlü küme** yalnızca test kullanımı için tasarlanmıştır. Çalışan tüm üretim iş yükleri için desteklenmez.
5. Seçin **ilk VM ölçek kümesi kapasitesini** düğüm türü için. Yukarı veya aşağı bir düğüm türünde VM sayısı daha sonra ölçeklendirebilirsiniz, ancak birincil düğüm türünde beş üretim iş yükleri için en düşük gerekliliktir. Diğer düğüm türleri, bir VM en az olabilir. En düşük **numarası** birincil düğüm türü sürücüleri için VM'lerin **güvenilirlik** kümenizin.  
6. Yapılandırma **özel uç noktalar**. Bu alan Azure Load Balancer, uygulamalarınız için genel internet üzerinden kullanıma sunmak istediğiniz bağlantı noktalarının virgülle ayrılmış listesi girmenizi sağlar. Örneğin, bir web uygulaması kümenize dağıtmayı planlıyorsanız, "80" kümenizi içine 80 numaralı bağlantı noktasında trafiğe izin verecek şekilde olmadığını buraya girin. Uç noktaları hakkında daha fazla bilgi için bkz. [uygulamalarla iletişim kurma][service-fabric-connect-and-communicate-with-services]
7. **Ters Proxy'yi Etkinleştir**.  [Service Fabric ters proxy'si](service-fabric-reverseproxy.md) yardımcı mikro hizmetler bir Service Fabric kümesinde çalışan bulmak ve http uç noktaları olan diğer hizmetlerle iletişim kurma.
8. Geri **küme yapılandırması** dikey altında **+ isteğe bağlı ayarları göster**, küme yapılandırma **tanılama**. Varsayılan olarak, tanılama, kümenizde sorunlarını gidermeye yardımcı olmak için etkinleştirilir. Tanılama değişiklik devre dışı bırakmak isterseniz **durumu** geç **kapalı**. Tanılama kapatıldığında kaynağınızın olan **değil** önerilir. Oluşturulan Application Insights proje zaten varsa, kendisine uygulama izlemeler yönlendirilebilmesi anahtarıyla, ardından verin.
9. **DNS hizmetini dahil et**.  [DNS hizmeti](service-fabric-dnsservice.md) DNS protokolünü kullanarak diğer hizmetleri bulmanıza olanak sağlayan isteğe bağlı bir hizmettir.
10. Seçin **yapı yükseltme modu** istediğiniz kümenizi ayarlayın. Seçin **otomatik**sistemin otomatik olarak en son sürümü seçin ve kendisine kümenizi yükseltmek deneyin istiyorsanız. Modunun **el ile**, desteklenen bir sürüm seçmek istiyorsanız. Fabric hakkında daha fazla ayrıntı modu Bkz: yükseltme için [Service Fabric kümesini yükseltme belge.][service-fabric-cluster-upgrade]

> [!NOTE]
> Yalnızca Service Fabric, desteklenen sürümlerini çalıştıran kümeler destekliyoruz. Seçerek **el ile** modunda almak sorumluluğa kümenizi desteklenen bir sürüme yükseltmek için.
> 

### <a name="3-security"></a>3. Güvenlik
![Güvenlik yapılandırmalarını Azure portalı ekran görüntüsü.][BasicSecurityConfigs]

Güvenli bir test kümesi ayarlama, kolaylaştırmak için sağladık **temel** seçeneği. Zaten bir sertifika varsa ve karşıya yüklediğiniz varsa, [anahtar kasası](/azure/key-vault/) (ve dağıtım için anahtar kasasını etkin), ardından **özel** seçeneği

#### <a name="basic-option"></a>Temel seçeneği
Eklemek veya mevcut bir anahtar kasasını yeniden ve bir sertifika eklemek için ekranlarındaki yönergeleri uygulayın. Sertifika eklenmesi zaman uyumlu bir işlemdir ve böylece sertifika oluşturulmasını beklemeniz gerekecektir.

Önceki işlemi tamamlanana kadar ekrandan ayrıldığınızda dürtüsüne kaçının.

![CreateKeyVault]

Anahtar kasası oluşturulur, anahtar kasanız için erişim ilkeleri düzenleyin. 

![CreateKeyVault2]

Tıklayarak **düzenleme erişim ilkeleri**, ardından **Gelişmiş erişim ilkeleri Göster** ve dağıtımı için Azure sanal Makineler'e erişimi etkinleştir. Şablon dağıtımı de etkinleştirmeniz önerilir. Seçimlerinizi yaptıktan sonra tıklamayı değil **Kaydet** dışı kapatmak ve düğme **erişim ilkeleri** bölmesi.

![CreateKeyVault3]

Sertifika adını girin ve tıklayın **Tamam**.

![CreateKeyVault4]

#### <a name="custom-option"></a>Özel seçeneği
Bu bölüm atlayın, adımları gerçekleştirdiğinizden **temel** seçeneği.

![SecurityCustomOption]

Kaynak key vault, sertifika URL'si ve sertifika parmak izi bilgilerini güvenlik sayfası tamamlamak için ihtiyacınız. Elinizin altında yoksa başka bir tarayıcı penceresi açın ve Azure Portalı'nda aşağıdakileri yapın

1. Anahtar kasası hizmetinize gidin.
2. "Özellikler" sekmesini seçin ve bir tarayıcı penceresinde "kaynak anahtar kasasına" 'Kaynak kimliği' kopyalayın 

    ![CertInfo0]

3. Şimdi, "Sertifikalar" sekmesini seçin.
4. Sürümleri sayfasına götürür sertifika parmak izi tıklayın.
5. Geçerli sürümü'nün altında gördüğünüz GUID'leri tıklayın.

    ![CertInfo1]

6. Artık aşağıdaki gibi ekranında olması gerekir. "Sertifika parmak izi" bir tarayıcı penceresinde onaltılık SHA-1 parmak izini kopyalayın
7. Diğer tarayıcı penceresinde "Sertifika URL'si" 'Gizli dizi tanımlayıcısı' kopyalayın.

    ![CertInfo2]

Denetleme **Gelişmiş ayarları Yapılandır** istemci sertifikaları için girmek için kutusu **yönetici istemci** ve **salt okunur istemci**. Bu alanlarda varsa yönetim istemci sertifikanızın parmak izi ve salt okunur kullanıcının istemci sertifikanızın parmak izini girin. Yöneticiler kümeye bağlanmayı denediğinizde, erişimi yalnızca bir sertifika parmak izi değerleri eşleşen bir parmak izi varsa buraya girilen verilir.  

### <a name="4-summary"></a>4. Özet

Kümeyi dağıtmak hazırsınız. Bunu yapmadan önce sertifika arama bağlantısı büyük mavi bilgilendirici kutu içinde indirin. Sertifikayı güvenli bir yerde sakladığınızdan emin olun. kümenize bağlanmak için ihtiyacınız. İndirdiğiniz sertifika bir parola olmadığından birini eklemeniz önerilir.

Küme oluşturma işlemini tamamlamak için tıklatın **Oluştur**. İsteğe bağlı olarak, şablon da indirebilirsiniz.

![Özet]

Oluşturma işleminin ilerleme durumunu bildirimler bölümünden görebilirsiniz. (Ekranınızın sağ üst köşesindeki durum çubuğunun yanında bulunan "Zil" simgesine tıklayın.) Kümeyi oluştururken **Başlangıç Panosuna Sabitle**’ye tıkladıysanız, **Service Fabric Kümesi Dağıtılıyor** öğesinin **Başlangıç** panosuna sabitlendiğini görürsünüz. Bu işlem biraz zaman alabilir. 

Powershell veya CLI kullanarak kümenize yönetim işlemleri için gerekir, kümeye bağlanmak, okuma daha fazla bilgi [kümenize bağlanma](service-fabric-connect-to-secure-cluster.md).

## <a name="view-your-cluster-status"></a>Küme durumunuzu görüntülemek
![Küme ayrıntıları panosunda ekran görüntüsü.][ClusterDashboard]

Kümeniz oluşturulduktan sonra kümenizin portal inceleyebilirsiniz:

1. Git **Gözat** tıklatıp **Service Fabric kümeleri**.
2. Kümenizi bulun ve tıklatın.
3. Kümenin genel uç noktası ve bir Service Fabric Explorer bağlantısıyla birlikte kümenizin ayrıntılarını panoda görebilirsiniz.

**Düğüm İzleyici** bölümü kümenin Pano dikey penceresinde, sağlıklı ve iyi olan VM'lerin sayısını gösterir. Kümenin durumu hakkında daha fazla ayrıntı bulabilirsiniz [Service Fabric sistem durumu modeli giriş][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric kümeleri yedekleme kullanılabilirliği sürdürmek ve durumu "çekirdek koruma olarak" başvurulan - korumak için her zaman olacak şekilde düğümleri belirli sayıda gerektirir. Bu nedenle, bu genellikle tüm makinelerin kümedeki ilk yapmadığınız sürece kapatmak güvenli değil bir [durumunuzu tam yedekleme][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Uzaktan bağlanmak için bir sanal makine ölçek kümesi örneği veya küme düğümü
Her küme sonuçlarınızı ayarı alınırken bir sanal makine ölçek kümesi'ndeki belirttiğiniz NodeType. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, yönetim kimlik doğrulaması için sertifikaları kullanarak güvenli bir kümeye sahip. İleri [kümenize bağlanın](service-fabric-connect-to-secure-cluster.md) ve bilgi edinmek için nasıl [uygulama parolalarını yönetme](service-fabric-application-secret-management.md).  Ayrıca, hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-overview.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
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
