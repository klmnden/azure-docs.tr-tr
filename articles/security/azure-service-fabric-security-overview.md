---
title: Azure service fabric güvenliğine genel bakış | Microsoft Docs
description: Bu makalede Azure Service Fabric güvenliğine genel bakış sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 19e4e55c4bd63e5d7a9ef6ea3b0bf6ae63f929d5
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-service-fabric-security-overview"></a>Azure Service Fabric güvenliğine genel bakış
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) paketini, dağıtmak ve ölçeklenebilir ve güvenilir mikro daha kolay hale getirir dağıtılmış sistemler platformudur. Service Fabric geliştirme ve bulut uygulamalarını yönetme önemli sorunları giderir. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

Azure Service Fabric güvenlik genel bakış makalede aşağıdaki alanlar üzerinde odaklanır:

-   Kümenizi güvenliğini sağlama
-   Anlama izleme ve tanılama
-   Sertifikaları kullanarak daha güvenli ortamları oluşturma
-   Rol tabanlı erişim denetimi (RBAC) kullanarak
-   Windows güvenliği kullanarak kümeleri güvenliğini sağlama
-   Service Fabric uygulama güvenliğini yapılandırma
-   Azure Service Fabric hizmetler için iletişimin güvenliğini sağlama 

## <a name="secure-your-cluster"></a>Kümenizin güvenliğini sağlama
Azure Service Fabric makine bir kümede Hizmetleri düzenler. Kümeler, yetkisiz kullanıcıların özellikle üretim iş yükleri çalıştırırken, kendilerine bağlanmasını önlemek için güvenli hale getirilmelidir. Güvenli olmayan bir küme oluşturmak mümkün olsa da, bu (Yönetim uç noktalarının genel internet gösterir) kendisine bağlanmak anonim kullanıcılar sağlayabilir.

Bu bölümde, Azure üzerinde veya tek başına çalışan kümeler için güvenlik senaryoları genel bir bakış sağlar. Ayrıca, bu senaryolar uygulamak için kullanılan teknolojiler açıklanmaktadır. Küme güvenlik senaryolar şunlardır:

-   Düğümü düğümü güvenlik
-   İstemcisi düğümü güvenlik

### <a name="node-to-node-security"></a>Düğümü düğümü güvenlik
Düğümü düğümü güvenlik VM'ler veya kümede makineler arasındaki iletişimin güvenliğini sağlar. Düğümü düğümü güvenlik ile uygulamaları ve Hizmetleri kümedeki barındırma Kümeye katılma yetkisi olan bilgisayarları katılabilirsiniz.

Windows üzerinde çalışan Azure veya tek başına kümelerinde çalışan kümeler kullanabilirsiniz [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenliği](https://msdn.microsoft.com/library/ff649396.aspx) Windows Server makinelerini için.

**Düğümü düğümü Sertifika Güvenliği anlama**

Service Fabric bir küme oluştururken belirttiğiniz X.509 sertifikaları kullanır. Bu sertifikalar nedir ve nasıl elde veya bunları oluşturmanız Hızlı bakış için bkz: [sertifikalarla çalışma](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Azure portalı, Azure Resource Manager şablonları veya tek başına JSON şablon ile Küme oluşturduğunuzda, sertifika güvenliği yapılandırın. Birincil bir sertifika ve sertifika rollover için kullanılan isteğe bağlı ikincil bir sertifika belirtebilirsiniz. Belirttiğiniz birincil ve ikincil sertifikalar için belirlediğiniz salt okunur istemci sertifikalarını ve yönetici istemci farklı [istemcisi düğümü güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>İstemcisi düğümü güvenlik
İstemcisi düğümü güvenlik istemci kimlikleri kullanarak yapılandırın. Bir istemci ve küme arasında güven oluşturmak için hangi istemci, güvenilir kimlikleri bilmeniz küme yapılandırmanız gerekir. Bu iki farklı şekillerde yapılabilir:

-   Bağlanabilmesi için etki alanı grubu kullanıcıları belirtin. 
-   Bağlanabilmesi için etki alanı düğümü kullanıcıları belirtin.

Service Fabric Service Fabric kümeye bağlı istemciler için iki farklı erişim denetim türlerini destekler:

-   Yönetici
-   Kullanıcı

Erişim denetimi kullanarak küme yöneticileri belirli türde bir küme işlemlerini erişimi sınırlandırabilirsiniz. Bu küme daha güvenli hale getirir.

 Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır.

**İstemci düğüm Sertifika Güvenliği anlama**

Azure portalı, Resource Manager şablonları veya tek başına JSON şablonu ile bir küme oluştururken istemcisi düğümü sertifika güvenliği yapılandırın. Bir yönetici istemci sertifikası ve/veya bir kullanıcı istemci sertifikası belirtmeniz gerekir. 

Belirttiğiniz yönetici istemci ve kullanıcı istemci sertifikası düğümü düğümü güvenlik için belirttiğiniz birincil ve ikincil sertifikaları farklı olmalıdır.

Yönetim sertifikasını kullanarak kümeye bağlanan istemciler, yönetim özellikleri için tam erişime sahip. Salt okunur kullanıcı istemci sertifikası kullanarak kümeye bağlanan istemciler yönetim özellikleri yalnızca okuma erişimi var. Diğer bir deyişle, bu sertifikalar için RBAC kullanılır.

Bir kümede sertifika güvenliği yapılandırma konusunda bilgi edinmek için [bir Azure Resource Manager şablonu kullanarak bir küme ayarlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

**Azure üzerinde istemci düğümünde Azure Active Directory güvenlik anlama**

Azure üzerinde çalışan kümeler Azure Active Directory (Azure AD) kullanarak yönetim uç noktalarına erişime de güvenliğini sağlayabilirsiniz. Gerekli Azure Active Directory yapıtlar oluşturma, küme oluşturma sırasında doldurmak nasıl ve bu kümeye bağlanma hakkında daha fazla bilgi için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme ayarlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD (kiracılar da bilinir), kuruluşların uygulamalar kullanıcı erişimini yönetmenizi sağlar. Bir web tabanlı oturum açma kullanıcı Arabirimi ile uygulamaları ve yerel istemci bir deneyim uygulamaları vardır.

Service Fabric kümesi yönetim işlevselliği, web tabanlı Service Fabric Explorer ve Visual Studio gibi çeşitli giriş noktalarını sunar. Sonuç olarak, Küme erişimi denetlemek için iki Azure AD uygulamaları oluşturmak: bir web uygulaması ve bir yerel uygulama.

Azure kümeler için istemciler ve sertifikaları düğümü düğümü güvenlik kimlik doğrulaması için Azure AD güvenlik kullanmanızı öneririz.

Windows Server 2012 R2 ve Active Directory ile tek başına Windows sunucu kümeleri için Windows Güvenlik yönetilen Grup hesaplarıyla kullanmanızı öneririz.  Aksi takdirde, Windows güvenliği Windows hesaplarıyla kullanın.

## <a name="understand-monitoring-and-diagnostics-in-azure-service-fabric"></a>İzleme ve tanılama Azure Service Fabric, anlama
[İzleme ve tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) geliştirme, test ve uygulamaları ve Hizmetleri herhangi bir ortamda dağıtmak için kritik öneme sahiptir. İzleme ve uygulamaları ve Hizmetleri yerel geliştirme ortamında veya üretim beklendiği gibi çalıştığından emin olmak için tanılama uyguladığınızda Service Fabric çözümleri en iyi çalışır.

Güvenlik açısından bakıldığında, izleme ve tanılama başlıca amaçları şunlardır:

-   Güvenlik olayı tarafından kaynaklanabilir donanım ve altyapı sorunlarını tanılamak ve algılar.
-   Tehlike (IOC) bir gösterge olabilir yazılım ve uygulama sorunları algılar.
-   Yanlışlıkla hizmet reddi önlemeye yardımcı olmak için kaynak tüketimini anlayın.

Genel iş akışını izleme ve tanılama ve üç adımdan oluşur:

-   **Olay oluşturma:** olay oluşturma altyapı (küme) ve uygulama/hizmet düzeyi olayları (günlüklerini, izlemeleri, özel olaylar) içerir. Daha fazla bilgi edinin [altyapı düzeyi olaylarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) ve [uygulama düzeyi olaylarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) nasıl sağlandığını ve daha fazla izleme eklemek nasıl anlamak için.

-   **Olay toplama:** oluşturulan olaylar gereken toplanacağı ve bunların görüntülenebilmesi bir araya getirilir. Genellikle kullanmanızı öneririz [Azure tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (tabanlı aracı günlük toplama benzer) veya [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (işlemdeki günlük toplama).

-   **Analiz:** olayları görselleştirilmiş ve analiz ve görüntü için izin vermek için bazı biçiminde erişilebilir olması gerekir. İzleme ve tanılama veri Görselleştirme ve analiz için birkaç Platform vardır. Öneririz iki olan [günlük analizi](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) ve [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) nedeniyle Service Fabric ile bunların iyi tümleştirme.

Aynı zamanda [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) Service Fabric kümesi yerleşik Azure kaynaklarını çoğunu izlemek için.

Bir izleme hizmetleri yüklemek ve sistem durumu modeli hiyerarşi içinde herhangi bir şey için sistem durumu raporu durumunu izleyebilirsiniz ayrı bir hizmettir. Bir izleme olaylarını kullanarak tek bir hizmeti görünümü temel alarak algılanmayacağı hataları önlenmesine yardımcı olabilir. 

Watchdogs de iyi (örneğin, belirli zaman aralıklarında depolama günlük dosyalarında temizleniyor) kullanıcı etkileşimi olmadan düzeltici eylemleri gerçekleştirir konak koduna yerlerdir. Örnek izleme hizmeti uygulama konumunda bulabilirsiniz [Azure Service Fabric izleme örnek](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="understand-how-to-secure-communication-by-using-certificates"></a>Sertifikaları kullanılarak güvenli iletişim nasıl anlama
Sertifikaları tek başına Windows kümenizin çeşitli düğümler arasındaki iletişimin güvenliğini sağlamanıza yardımcı olur. X.509 sertifikaları kullanarak, bu kümeye bağlanma istemcileri de doğrulayabilir. Bu, yalnızca yetkili kullanıcılar küme erişebilmesini sağlar. Oluşturduğunuzda küme üzerinde bir sertifika etkinleştirmenizi öneririz.

### <a name="x509-certificates-and-service-fabric"></a>X.509 sertifikaları ve Service Fabric
X.509 dijital sertifikalar, istemcilerin ve sunucuların kimliğini doğrulamak için yaygın olarak kullanılır. Bunlar iletileri dijital olarak imzalamak ve şifrelemek için kullanılır.

Aşağıdaki tabloda, Küme kurulumu gereken sertifikaları listelenmektedir:

|Sertifika bilgilerini ayarlama |Açıklama|
|-------------------------------|-----------|
|ClusterCertificate|    Bu sertifika, bir küme düğümlerinde arasındaki iletişimin güvenliğini sağlamak için gereklidir. İki farklı sertifikaları kullanabilirsiniz: birincil bir sertifika ve bir ikincil yükseltme.|
|ServerCertificate| Bu kümeye bağlanmaya çalıştığında bu sertifikayı istemciye sunulur. İki farklı sunucu sertifikaları kullanabilirsiniz: birincil bir sertifika ve bir ikincil yükseltme.|
|ClientCertificateThumbprints|  Kimliği doğrulanmış istemcilerde yüklenecek sertifikalar kümesidir.|
|ClientCertificateCommonNames|  Bu ortak ilk istemci sertifikası CertificateCommonName için adıdır. Bu sertifika verenin parmak izini CertificateIssuerThumbprint olur.|
|ReverseProxyCertificate|   Bu, güvenli hale getirmek için belirtilen, isteğe bağlı bir sertifikadır, [ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Sertifikaları güvenli hale getirme hakkında daha fazla bilgi için bkz: [X.509 sertifikaları kullanarak Windows'u bir tek başına kümede güvenli](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Rol tabanlı erişim denetimi anlama
Erişim denetimi, belirli küme işlemleri farklı küme böylece daha güvenli hale getirme kullanıcı grupları için erişimi sınırlamak Küme Yöneticisi sağlar. Bir kümeye bağlanan istemciler için iki farklı erişim denetim türleri desteklenir: 

- Yönetici rolü
- Kullanıcı rolü

Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır.

Yönetici ve kullanıcı istemci rolleri küme oluşturma sırasında (Sertifikalar dahil olmak üzere) ayrı kimlikleri sağlayarak her biri için belirtin. Varsayılan erişim denetimi ayarlarını ve varsayılan ayarlarının nasıl değiştirileceği hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-by-using-windows-security"></a>Windows güvenliği kullanarak tek başına küme güvenli hale getirme
Bir Service Fabric kümesi yetkisiz erişimi önlemek için küme güvenlik altına almanız gerekir. Küme üretim iş yükleri çalıştığında güvenlik özellikle önemlidir. Düğüm düğümü ve istemci düğümü güvenliğinin ClusterConfig.JSON dosyasında Windows güvenliği kullanarak nasıl yapılandırılacağı açıklanmaktadır.

**GMSA kullanarak Windows güvenliği yapılandırma**

Service Fabric gMSA altında çalıştırması gerektiğinde, düğümü düğümü güvenlik ayarlayarak yapılandırmanız [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security). Düğümler arasındaki güven ilişkileri oluşturmak için bunlar birbirinden kullanan yapılması gerekir.

İstemcisi düğümü güvenlik ClientIdentities kullanarak yapılandırın. Bir istemci ve küme arasında güven sağlamak için kümenin güven hangi istemci kimlikleri tanımak için yapılandırmanız gerekir.

**Makine grubu kullanarak Windows güvenliği yapılandırma**

Bir Active Directory etki alanı içinde bir makine grubu kullanmak istiyorsanız, düğümü düğümü güvenlik ClusterIdentity ayarlayarak yapılandırın. Daha fazla bilgi için bkz: [Active Directory'de bir makine grubu oluştur](https://msdn.microsoft.com/library/aa545347).

İstemcisi düğümü güvenlik ClientIdentities kullanarak yapılandırın. Bir istemci ve küme arasında güven sağlamak için kümenin küme güvenebileceği istemci kimliklerini tanımak için yapılandırmanız gerekir. İki farklı yolla güven kurabilir:

-   Bağlanabilmesi için etki alanı grubu kullanıcıları belirtin.
-   Bağlanabilmesi için etki alanı düğümü kullanıcıları belirtin.

## <a name="configure-application-security-in-service-fabric"></a>Service Fabric uygulama güvenliğini yapılandırma
### <a name="manage-secrets-in-service-fabric-applications"></a>Service Fabric uygulamaları gizli anahtarları Yönet
Bu yöntem, Service Fabric uygulaması parolalarında yönetmenize yardımcı olur. Gizli depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini olmayan diğer değerleri gibi herhangi bir önemli bilgi olabilir.

Bu yaklaşımı kullanır [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) anahtarları ve gizli anahtarları yönetmek için. Ancak, gizli bir uygulamada bulut platformu belirsiz kullanmaktır. Bu, uygulamaları herhangi bir yerde barındırılan bir kümeye dağıtılabilir anlamına gelir. Bu akış dört ana adım vardır:

-   Veri şifreleme sertifikası alın.
-   Sertifikayı kümenizde yükleyin.
-   Sertifikayla ilgili bir uygulama dağıtırken gizli değerleri şifreler ve bir hizmetin Settings.xml yapılandırma dosyasına Ekle.
-   Settings.xml dışında şifrelenmiş değerler ile aynı şifreleme sertifikası şifresini çözerek okuyun.

>[!NOTE]
>Daha fazla bilgi edinmek [Service Fabric uygulamaları parolalarında yönetme](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Uygulamanıza yönelik güvenlik ilkeleri yapılandırma
Azure Service Fabric güvenlik kullanarak, kümedeki farklı kullanıcı hesabı altında çalışan güvenli uygulamalar yardımcı olabilir. Service Fabric güvenlik aynı zamanda, uygulamalar tarafından kullanıcı hesapları altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları sağlanmasına yardımcı olur. Bu çalışan uygulamaları bile paylaşılan bir barındırılan ortamda, daha güvenli hale getirir.

Adımları içerir:

-   Bir hizmet Kurulum giriş noktası için ilke yapılandırma.
-   PowerShell komutlarını bir Kurulum giriş noktasından başlatılıyor.
-   Yerel hata ayıklama için konsolu yeniden yönlendirmeyi kullanma.
-   Hizmet kodu paketleri için bir ilke yapılandırma.
-   HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Güvenli iletişim için Azure Service Fabric güvenlik hizmetleri
Güvenlik iletişim en önemli yönlerinden birisidir. Güvenilir hizmetler uygulama çerçevesi birkaç önceden oluşturulmuş iletişimi yığınları ve güvenliği geliştirmek için kullanılan araçlar sağlar.

-   [Hizmet remoting kullanırken hizmet korunmasına yardımcı olma](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication)
-   [Bir WCF tabanlı iletişim yığını kullanırken hizmet korunmasına yardımcı olma](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack)

## <a name="next-steps"></a>Sonraki adımlar
- Kavramsal küme güvenliği hakkında bilgi için [Azure Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesi oluştur](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) ve [Azure portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Service Fabric kümesi güvenlik hakkında daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
