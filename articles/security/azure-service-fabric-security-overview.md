---
title: Azure Service Fabric güvenliğine genel bakış | Microsoft Docs
description: Bu makalede, Azure Service Fabric güvenliğine genel bakış sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: c5b5f80a43530fe6d0b90e65c3aef89a815157e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610899"
---
# <a name="azure-service-fabric-security-overview"></a>Azure Service Fabric güvenliğine genel bakış
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) paketlemeyi, dağıtmayı ve ölçeklenebilir ve güvenilir mikro hizmetleri sağlayan bir dağıtılmış sistemler platformudur. Service Fabric, bulut uygulamalarını geliştirme ve yönetme, sorunlarını ele alır. Geliştiriciler ve Yöneticiler, karmaşık altyapı sorunlarını çözmeye ve ölçeklenebilir ve güvenilir bir iş açısından kritik, zorlu iş yüklerini uygulamaya odaklanmasına.

Bu makalede güvenlik konuları Service Fabric dağıtımı için bir genel bakıştır.

## <a name="secure-your-cluster"></a>Kümenizin güvenliğini sağlama
Azure Service Fabric, bir makine kümesindeki Hizmetleri düzenler. Kümeler, yetkisiz kullanıcıların özellikle, üretim iş yükleri çalıştırırken, bağlanmasını önlemek için güvenli hale getirilmelidir. Güvenli olmayan bir kümeye oluşturmak mümkün olsa da, bu (genel İnternet'e yönetim uç noktalarını sunarsa) kümeye bağlanmak anonim kullanıcılar izin verebilir.

Tek başına çalışan kümeler için ya da Azure ile ilgili dikkate alınması gereken iki senaryo düğümler için güvenlik ve istemci düğümü güvenlik ' dir. Bu senaryoları uygulamak için çeşitli teknolojileri kullanabilirsiniz.

### <a name="node-to-node-security"></a>Düğümler için güvenlik
Düğümden düğüme güvenliği, VM veya kümede makineler arasındaki iletişim için uygulanır. Düğümden düğüme güvenlik ile uygulamaları ve Hizmetleri kümedeki barındırma Kümeye katılma yetkisi olan bilgisayarları katılabilir.

Windows üzerinde çalışan Azure veya tek başına kümeler üzerinde çalışan kümeler ya da kullanabilir [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenlik](https://msdn.microsoft.com/library/ff649396.aspx) için Windows Server makineleri.

#### <a name="node-to-node-certificate-security"></a>Düğümden düğüme sertifika güvenliği

Service Fabric bir küme oluştururken belirttiğiniz X.509 sertifikaları kullanır. Bu sertifikalar nelerdir ve nasıl elde veya bunları oluşturmak Hızlı bakış için bkz: [Working with certificates](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Azure portalı, Azure Resource Manager şablonları ya da bir tek başına JSON şablon aracılığıyla küme oluşturduğunuzda sertifika güvenliğini yapılandırın. Birincil sertifika ve sertifika rollover için kullanılan isteğe bağlı ikincil bir sertifika belirtebilirsiniz. Belirttiğiniz birincil ve ikincil sertifikaları için belirttiğiniz salt okunur istemci sertifikaları ve yönetici istemci farklı [istemci düğümü güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>İstemci düğümü güvenlik
İstemci düğümü güvenlik, istemci kimliklerini kullanarak yapılandırın. Bir istemci ile bir küme arasında güven oluşturmak için hangi istemci kimlikleri buna güvenmesi bilmek küme yapılandırmanız gerekir.

Service Fabric, bir Service Fabric kümesine bağlanan istemciler için iki erişim denetim türlerini destekler:

-   **Yönetici**: Okuma/yazma özellikleri dahil olmak üzere, yönetim özelliklerine tam erişim.
-   **Kullanıcı**: Yönetim yetenekleri (örneğin, sorgu işlevleri) yalnızca okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı.

Küme yöneticileri, erişim denetimi kullanarak, belirli türde bir küme işlemlerini erişimi sınırlayabilirsiniz. Bu kümeye daha güvenli hale getirir.

#### <a name="client-to-node-certificate-security"></a>İstemci düğümü sertifika güvenliği

Azure portalı, Resource Manager şablonları ya da bir tek başına JSON şablon aracılığıyla bir küme oluşturduğunuzda istemci düğümü sertifika güvenliğini yapılandırın. Bir yönetici istemci sertifikası ve/veya bir kullanıcı istemci sertifikası belirtmeniz gerekir. Bu sertifikalar, düğümden düğüme güvenlik için belirttiğiniz birincil ve ikincil sertifikaları farklı olduğundan emin olun.

Yönetici sertifikayı kullanarak kümeye bağlanma istemcileri yönetim özelliklerine tam erişime sahiptir. Salt okunur kullanıcı istemci sertifikası kullanarak kümeye bağlanma istemcileri yönetim özellikleri yalnızca okuma erişimi var. Diğer bir deyişle, bu sertifikalar, rol tabanlı erişim denetimi (RBAC) için kullanılır.

Bir kümede sertifika güvenliği yapılandırma konusunda bilgi için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

#### <a name="client-to-node-azure-active-directory-security"></a>Azure Active Directory güvenlik istemci düğümü

Azure üzerinde çalışan kümeler Azure Active Directory (Azure AD) kullanarak yönetim uç noktalarına erişimi de güvenliğini sağlayabilirsiniz. Gereken Azure Active Directory yapıtlarını oluşturma işlemini nasıl küme oluşturma sırasında doldurulacağı ve bu kümeye bağlanma hakkında daha fazla bilgi için bkz. [bir Azure Resource Manager şablonu kullanarak bir küme oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Bir web tabanlı oturum açma kullanıcı Arabirimi ile uygulamaları ve yerel istemci deneyimi ile uygulamaları vardır.

Service Fabric kümesi, birden çok giriş noktası için web tabanlı Service Fabric Explorer ve Visual Studio da dahil olmak üzere Yönetim işlevselliğini sunar. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulamaları oluşturduğunuz: bir web uygulaması ve bir yerel uygulama.

Azure kümelerinde, istemciler ve düğümden düğüme güvenliği için sertifika kimlik doğrulaması için Azure AD güvenlik kullanmanızı öneririz.

Windows Server 2012 R2 ve Active Directory ile tek başına Windows Server kümeleri için Windows Güvenlik Grup yönetilen hizmet hesapları ile (gmsa'ları) kullanmanızı öneririz. Aksi takdirde, Windows Güvenlik ile Windows hesapları kullanın.

## <a name="understand-monitoring-and-diagnostics-in-service-fabric"></a>İzleme ve tanılama Service fabric'te anlama
[İzleme ve tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) geliştirme, test ve uygulamaları ve Hizmetleri her türlü ortamda dağıtmak için kritik öneme sahiptir. Service Fabric çözümleri, izleme ve tanılama uygulamaları ve Hizmetleri yerel geliştirme ortamında veya üretim beklendiği gibi çalıştığından emin olmak için uygularken en iyi çalışır.

Güvenlik açısından bakıldığında, başlıca amaçlarından biri, izleme ve tanılama şunlardır:

-   Algılar ve bir güvenlik olayı tarafından kaynaklanabilir donanım ve altyapı sorunları tanılayın.
-   (IOC) güvenlik ihlali göstergesi olabilecek yazılım ve uygulama sorunları algılayın.
-   Yanlışlıkla bir hizmet reddi önlemeye yardımcı olmak için kaynak tüketimi anlayın.

İş akışı izleme ve tanılama üç adımdan oluşur:

1.  **Olay oluşturma**: Olay oluşturma (küme) altyapı düzeyinde ve uygulama/hizmet düzeyinde hem olayları (günlüklerini, izlemeleri, özel olaylar) içerir. Daha fazla bilgi edinin [altyapı düzeyinde olaylar](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) ve [uygulama düzeyinde olaylar](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) nasıl sağlandığını ve daha fazla izleme ekleme anlamak için.

2.  **Olay toplama**: Oluşturulan olayları toplanacak ve bunlar görüntülenebilmesi toplanan gerekir. Genellikle kullanmanızı öneririz [Azure tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (aracı tabanlı günlük toplama benzer) veya [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (işlem içi günlük toplama).

3.  **Analiz**: Olayları görselleştirilmiş ve analiz ve görüntüleme için izin vermek için bazı biçiminde erişilebilir olması gerekir. İzleme ve Tanılama verileri görselleştirmesini ve analiz için çeşitli platformları vardır. Öneririz [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) ve [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) çünkü bunlar da Service Fabric ile tümleştirme.

Ayrıca [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) bir Service Fabric kümesi oluşturulan Azure kaynaklarını birçoğu izlemek için.

Bir izleme sistem durumu izleme ve sistem durumu modeli hiyerarşi içinde herhangi bir şey için farklı hizmet ve rapor durumu yük ayrı bir hizmettir. Bir izleme kullanarak, tek bir hizmette bir görünümü temel alarak değil algılanır hatalarını engellemeye yardımcı olabilir. 

Watchdogs de iyi bir kullanıcı etkileşimi olmadan düzeltici eylemler gerçekleştirir konak koda yerdir. Örnek Depolama günlük dosyalarını belirli aralıklarla temizleme. Örnek izleme hizmeti uygulaması konumunda bulabilirsiniz [Azure Service Fabric izleme örnek](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-communication-by-using-certificates"></a>Sertifikaları kullanarak güvenli iletişim
Sertifikaları, tek başına Windows kümenizin çeşitli düğümler arasındaki iletişimi güvenli hale getirmenize yardımcı olur. X.509 sertifikaları kullanarak bu kümeye bağlanma istemcileri doğrulayabilir. Bu, yalnızca yetkili kullanıcıların küme erişmesini sağlar. Oluşturduğunuzda, küme üzerinde bir sertifika etkinleştirmenizi öneririz.

X.509 dijital sertifikalar, istemciler ve sunucular kimliğini doğrulamak için yaygın olarak kullanılır. İletileri dijital olarak imzalamak ve şifrelemek için de kullanılırlar.

Aşağıdaki tabloda, küme kurulumunuzu gereken sertifikaları listelenmektedir:

|Sertifika bilgileri ayarlama |Açıklama|
|-------------------------------|-----------|
|ClusterCertificate|    Bu sertifika, bir kümedeki düğümlerden arasındaki iletişimin güvenliğini sağlamak için gereklidir. İki küme sertifikaları kullanabilirsiniz: birincil sertifikada yanı sıra, ikincil bir yükseltme için.|
|ServerCertificate| Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. İki sunucu sertifikaları kullanabilirsiniz: birincil sertifikada yanı sıra, ikincil bir yükseltme için.|
|ClientCertificateThumbprints|  Bu, kimliği doğrulanmış istemcilerde yüklenecek sertifikalar kümesidir.|
|ClientCertificateCommonNames|  İlk istemci sertifikası ortak adı için CertificateCommonName budur. Bu sertifika verenin parmak izi CertificateIssuerThumbprint olur.|
|ReverseProxyCertificate|   Bu, güvenli hale getirmek için belirtebilirsiniz, isteğe bağlı bir sertifikadır, [ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Sertifikaları güvenli hale getirme hakkında daha fazla bilgi için bkz. [X.509 sertifikaları kullanarak Windows üzerinde tek başına küme güvenli](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Rol tabanlı erişim denetimini anlama
Yönetici ve kullanıcı istemci rolleri küme oluşturma sırasında ayrı kimlikleri (Sertifikalar dahil olmak üzere) sağlayarak her biri için belirtin. Varsayılan erişim denetimi ayarlarını ve varsayılan ayarlarının nasıl değiştirileceği hakkında daha fazla bilgi için bkz. [Service Fabric istemciler için rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-clusters-by-using-windows-security"></a>Tek başına kümeler Windows güvenliğini kullanarak güvenli hale getirme
Bir Service Fabric kümesine yetkisiz erişimi önlemek için küme güvenlik altına almanız gerekir. Üretim iş yükleri küme çalıştırdığında, güvenlik özellikle önemlidir. Düğümden düğüme ve düğümden istemci güvenlik, ClusterConfig.JSON dosyasında Windows güvenliğini kullanarak yapılandırın.

Service Fabric bir gMSA altında çalıştırılması gerektiğinde, düğümden düğüme güvenlik ayarlayarak yapılandırdığınız [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security). Düğümler arasındaki güven ilişkileri oluşturmak için birbiriyle uyumlu yapmanız gerekir.

Bir Active Directory etki alanı içinde bir makine grubu kullanmak istiyorsanız, düğümden düğüme güvenlik ClusterIdentity ayarlayarak yapılandırın. Daha fazla bilgi için [Active Directory'de bir makine grubu oluştur](https://msdn.microsoft.com/library/aa545347).

İstemci düğümü güvenlik, ClientIdentities kullanarak yapılandırın. Güven hangi istemci kimlikleri tanımak için küme yapılandırmanız gerekir. Güven iki şekilde oluşturabilirsiniz:

-   Bağlanabilir etki alanı grubu kullanıcıları belirtin.
-   Bağlanabilen etki alanı düğümü kullanıcıları belirtin.

## <a name="configure-application-security-in-service-fabric"></a>Service Fabric'te uygulama güvenliğini yapılandırma
### <a name="manage-secrets-in-service-fabric-applications"></a>Service Fabric uygulamaları gizli dizileri Yönet
Gizli dizileri depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini değil diğer değerleri gibi herhangi bir önemli bilgi olabilir.

Kullanabileceğiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) anahtarları ve parolaları yönetmek için. Ancak, bir uygulama gizli kullanımı belirli bir bulut platformunda içermez. Uygulamaları herhangi bir yerde barındırılan bir kümeye dağıtabilirsiniz. Bu akışta dört ana adım vardır:

1.  Veri şifreleme sertifikası alın.
2.  Sertifikayı kümenizde yükleyin.
3.  Sertifika ile bir uygulama dağıtırken gizli değerleri şifreler ve bunları bir hizmetin Settings.xml yapılandırma dosyasına yerleştirir.
4.  Settings.xml dışında şifrelenmiş değerler ile aynı şifreleme sertifikası şifresini çözerek okuyun.

Daha fazla bilgi için [Service Fabric uygulamaları gizli dizileri Yönet](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-an-application"></a>Bir uygulama için güvenlik ilkelerini yapılandırma
Azure Service Fabric güvenliği kullanarak kümedeki farklı bir kullanıcı hesabı altında çalışan güvenli uygulamalar yardımcı olabilir. Service Fabric güvenliği, örneğin uygulamalar kullanıcı hesaplarını altında--dağıtım zamanında kullanan kaynakları, dosyalar, dizinler ve sertifikaları güvenli da yardımcı olur. Bu çalışan uygulamalar da paylaşılan bir barındırılan ortamda, daha güvenli hale getirir.

Güvenlik ilkelerini yapılandırmak için görevler aşağıdakileri içerir:

-   İlke için bir hizmet Kurulumu giriş noktasını yapılandırma
-   PowerShell komutlarını bir Kurulum giriş noktasından başlatılıyor
-   Konsol yönlendirmesi, yerel hata ayıklama için kullanma
-   Hizmet kodu paketleri için bir ilke yapılandırma
-   HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atama

## <a name="secure-communication-for-services"></a>Hizmetleri güvenli iletişim
Güvenlik iletişim en önemli yönlerinden birisidir. Reliable Services uygulaması çerçevesi, birkaç önceden oluşturulmuş iletişim yığınlarını ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Daha fazla bilgi için [güvenli bir hizmet için hizmet uzaktan iletişimini iletişimleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).

## <a name="next-steps"></a>Sonraki adımlar
- Küme güvenliği hakkında kavramsal bilgiler için bkz. [Azure Resource Manager'ı kullanarak bir Service Fabric kümesi oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) ve [Azure portalını kullanarak bir Service Fabric kümesi oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Service Fabric küme güvenliği hakkında daha fazla bilgi için bkz: [Service Fabric kümesi güvenlik senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).
