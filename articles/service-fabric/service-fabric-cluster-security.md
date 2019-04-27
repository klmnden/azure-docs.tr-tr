---
title: Bir Azure Service Fabric küme güvenliğini sağlama | Microsoft Docs
description: Bir Azure Service Fabric kümesi ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojileri için güvenlik senaryoları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2018
ms.author: aljo
ms.openlocfilehash: 6d67fa4af031480fda4a91f7356bff69830a654c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60711518"
---
# <a name="service-fabric-cluster-security-scenarios"></a>Service Fabric kümesi güvenlik senaryoları
Bir Azure Service Fabric kümesine sahip olduğunuz bir kaynaktır. Yetkisiz kullanıcıların bunlara bağlanmasını önlemeye yardımcı olmak için kümeleri güvenli hale getirmek için sizin sorumluluğunuzdur. Üretim iş yükleri küme üzerinde çalışan, güvenli bir kümeye özellikle önemlidir. Kümenin yönetim uç noktalarını genel İnternet'e sunarsa, güvenli olmayan bir kümeye oluşturmak mümkün olsa da, anonim kullanıcılar için bağlanabilir. Güvenli olmayan kümelerini üretim iş yükleri için desteklenmez. 

Bu makalede, Azure kümeleri, tek başına kümeler ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojileri için güvenlik senaryolarına genel bakış şöyledir:

* Düğümler için güvenlik
* İstemci düğümü güvenlik
* Rol Tabanlı Erişim Denetimi (RBAC)

## <a name="node-to-node-security"></a>Düğümler için güvenlik
Düğümden düğüme güvenliği bir küme içindeki bilgisayarları ve VM'ler arasında güvenli iletişim yardımcı olur. Bu güvenlik senaryo, kümeye katılmak için yetkili bilgisayar uygulamaları ve Hizmetleri kümedeki barındırma katılabilir sağlar.

![Düğümden düğüme iletişimi diyagramı][Node-to-Node]

Windows üzerinde hem çalışan Azure ve tek başına kümeler üzerinde çalışan kümelerle ya da kullanabilir [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenlik](https://msdn.microsoft.com/library/ff649396.aspx) Windows Server bilgisayarları için.

### <a name="node-to-node-certificate-security"></a>Düğümden düğüme sertifika güvenliği
Service Fabric bir küme oluştururken, düğüm türü yapılandırmasının bir parçası belirttiğiniz X.509 sertifikaları kullanır. Bu makalenin sonunda, bu sertifikaların nedir ve nasıl elde veya bunları oluşturmak kısa bir genel bakış görebilirsiniz.

Küme ya da Azure portalında bir Azure Resource Manager şablonu kullanarak veya bir tek başına JSON şablonunu kullanarak oluşturduğunuz sertifika güvenliği ayarlanır. Service Fabric SDK'ın varsayılan davranışı, dağıtmak ve sertifika en ile gelecekteki sona eren sertifikayı yüklemek için değildir; Klasik davranışını tanımlayan el ile başlatma rollover izin vermek için birincil ve ikincil sertifikaları, izin verilen ve yeni işlevleri üzerinde kullanım için önerilmez. Kullanım süresi dolan tarihe, içine en sahip olacak birincil sertifika için ayarladığınız salt okunur istemci sertifikaları ve yönetici istemci farklı [istemci düğümü güvenlik](#client-to-node-security).

Sertifika Güvenliği bir kümede yedeklemek için Azure hakkında bilgi edinmek için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-arm.md).

Bir kümedeki tek başına Windows Server küme için sertifika güvenliği hakkında bilgi edinmek için bkz: [X.509 sertifikaları kullanarak Windows üzerinde tek başına küme güvenli](service-fabric-windows-cluster-x509-security.md).

### <a name="node-to-node-windows-security"></a>Düğümler için Windows Güvenlik
Bir tek başına Windows Server kümesi için Windows güvenliği hakkında bilgi edinmek için bkz: [tek başına küme Windows üzerinde Windows güvenliğini kullanarak güvenli](service-fabric-windows-cluster-windows-security.md).

## <a name="client-to-node-security"></a>İstemci düğümü güvenlik
İstemci düğümü güvenlik, istemcilerin kimliğini doğrular ve bir istemci ve kümedeki tek tek düğümler arasında güvenli iletişim yardımcı olur. Bu tür bir güvenlik, küme ve kümede dağıtılan uygulamalar yalnızca yetkili kullanıcıların erişebildiğinden emin olun yardımcı olur. İstemcileri, Windows güvenlik kimlik bilgilerini ya da sertifika güvenlik kimlik bilgilerini üzerinden benzersiz şekilde tanımlanır.

![İstemci-düğüm iletişimi diyagramı][Client-to-Node]

Windows üzerinde hem çalışan Azure ve tek başına kümeler üzerinde çalışan kümelerle ya da kullanabilir [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenlik](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>İstemci düğümü sertifika güvenliği
Resource Manager şablonu kullanarak veya bir tek başına JSON şablonunu kullanarak bir kümeyi ya da Azure portalında oluşturduğunuzda düğüm için istemci sertifikası güvenliği ayarlayın. Sertifika oluşturmak için bir yönetici istemci sertifikası veya bir kullanıcı istemci sertifikası belirtin. En iyi uygulama, belirttiğiniz yönetici istemci ve kullanıcı istemci sertifikalarını belirtmek için birincil ve ikincil sertifikaları arasından farklı olmalıdır [düğümden düğüme güvenlik](#node-to-node-security). Varsayılan olarak, düğümden düğüme güvenlik için küme sertifikaları izin verilen istemci yönetim sertifikaları listesine eklenir.

Yönetici sertifikayı kullanarak kümeye bağlanma istemcileri yönetim özelliklerine tam erişime sahiptir. Salt okunur kullanıcı istemci sertifikası kullanarak kümeye bağlanma istemcileri yönetim özellikleri yalnızca okuma erişimi var. Bu sertifikalar, bu makalenin sonraki bölümlerinde açıklanan RBAC için kullanılır.

Sertifika Güvenliği bir kümede yedeklemek için Azure hakkında bilgi edinmek için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-arm.md).

Bir kümedeki tek başına Windows Server küme için sertifika güvenliği hakkında bilgi edinmek için bkz: [X.509 sertifikaları kullanarak Windows üzerinde tek başına küme güvenli](service-fabric-windows-cluster-x509-security.md).

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Azure'da istemci düğümü Azure Active Directory güvenlik
Azure AD (kiracılar bilinir), kuruluşların uygulamalara kullanıcı erişimini yönetmenizi sağlar. Uygulamaları olan web tabanlı oturum açma kullanıcı Arabirimi hem de yerel istemci deneyimi ile ayrılır. Zaten bir kiracı oluşturmadıysanız okuyarak başlamanız [bir Azure Active Directory kiracısı edinme][active-directory-howto-tenant].

Service Fabric kümesi birden çok giriş noktası için web tabanlı dahil olmak üzere Yönetim işlevselliğini sunar [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] ve [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulaması, bir web uygulaması ve yerel bir uygulama oluşturun.

Azure üzerinde çalışan kümeler için Azure Active Directory (Azure AD) kullanarak da Yönetim uç noktalarına erişimi güvenliğini sağlayabilirsiniz. Gerekli oluşturma hakkında bilgi edinmek için bkz: Azure AD yapıtları ve Küme oluşturduğunuzda bunları doldurmak nasıl [istemcilerin kimliğini doğrulamak için Azure AD'yi ayarlarken ayarlamak](service-fabric-cluster-creation-setup-aad.md).

## <a name="security-recommendations"></a>Güvenlik önerileri
Azure üzerinde barındırılan ortak bir ağda dağıtılmış Service Fabric kümeleri için istemci düğümü karşılıklı kimlik doğrulaması için önerilir:
*   Azure Active Directory istemci kimliği için kullan
*   Sunucu kimliği ve http iletişim SSL şifrelemesi için bir sertifika

Service Fabric kümeleri için Azure'da barındırılan bir ortak ağda dağıtılan düğümden düğüme güvenlik için düğümleri kimliğini doğrulamak için bir küme sertifikası kullanmak için önerilir. 


Windows Server 2012 R2 ve Windows Active Directory, varsa, tek başına Windows Server kümeleri için Windows Güvenlik Grup yönetilen hizmet hesapları ile kullanmanızı öneririz. Aksi takdirde, Windows Güvenlik ile Windows hesapları kullanın.

## <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Erişim denetimi, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlamak için kullanabilirsiniz. Bu, küme daha güvenli olmasına yardımcı olur. İstemciler bir kümeye bağlanmak için iki erişim denetim türleri desteklenir: Yönetici rolü ve kullanıcı rolü.

Yönetici rolüne atanan kullanıcıların, okuma ve yazma özellikleri dahil olmak üzere yönetim özelliklerine tam erişime sahiptir. Varsayılan olarak, kullanıcı rolüne atanan kullanıcıların yalnızca okuma erişimi yönetim özelliklerine (örneğin, sorgu işlevleri) sahip. Uygulamalar ve hizmetler de çözebilirsiniz.

Kümeyi oluşturduğunuzda, yönetici ve kullanıcı istemci rolleri ayarlayın. Rolleri ayrı kimlikleri (örneğin, sertifikalar veya Azure AD kullanarak) için her rol türü sağlayarak atayın. Varsayılan erişim denetimi ayarları ve varsayılan ayarlarının nasıl değiştirileceği hakkında daha fazla bilgi için bkz. [rol tabanlı Access Control Service Fabric istemciler için](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509 sertifikaları ve Service Fabric
X.509 dijital sertifikalar, istemciler ve sunucular kimliğini doğrulamak için yaygın olarak kullanılır. Bunlar iletileri dijital olarak imzalamak ve şifrelemek için kullanılır. Service Fabric küme güvenliğini sağlama ve uygulama güvenlik özellikler sağlamak için X.509 sertifikaları kullanır. X.509 dijital sertifikalar hakkında daha fazla bilgi için bkz. [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx). Kullandığınız [Key Vault](../key-vault/key-vault-overview.md) azure'da Service Fabric kümelerine ait sertifikaları yönetmek için.

Dikkate alınması gereken bazı önemli noktalar:

* Üretim iş yüklerinde çalışan kümeler için sertifikaları oluşturmak için doğru şekilde yapılandırılmış bir Windows Server sertifika hizmeti veya onaylanan bir diğerine kullanın [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Hiçbir zaman herhangi bir geçici kullanın veya bir üretim ortamında MakeCert.exe gibi araçları kullanarak, oluşturduğunuz sertifikalar test edebilirsiniz.
* Ancak yalnızca bir sınama kümesi otomatik olarak imzalanan bir sertifika kullanabilirsiniz. Kendinden imzalı sertifika üretim ortamında kullanmayın.
* Sertifika parmak izini oluştururken, SHA1 parmak izi'ı oluşturduğunuzdan emin olun. İstemci ve küme sertifikası parmak izleri yapılandırırken kullandığınız kadar SHA1 değeridir.

### <a name="cluster-and-server-certificate-required"></a>Küme ve sunucu sertifikası (gerekli)
Bu sertifikalar (birincil ve isteğe bağlı olarak ikincil) küme güvenliğini sağlama ve yetkisiz erişimi önlemek için gereklidir. Bu sertifikalar, küme ve sunucu kimlik doğrulaması sağlar.

Küme kimlik doğrulaması, düğümden düğüme iletişim için küme Federasyon kimliğini doğrular. Bu sertifika ile kimliğini kanıtlamak düğüm kümesine katılabilirsiniz. Yönetim istemci gerçek bir küme ve olmayan bir 'ortadaki adam' Bahsediyor bilebilmesi sunucu kimlik doğrulaması, bir yönetim istemcisinde küme yönetimi Uç noktalara kimliğini doğrular. Bu sertifika aynı zamanda bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar. Bir istemci veya düğüm düğüm doğruladığında, biri de, ilk ortak ad değeri **konu** alan. Bu ortak adı veya sertifikalarının konu alternatif adları (SAN), izin verilen ortak adlar listesinde mevcut olmalıdır.

Sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika özel anahtar içermelidir. Bu sertifikalar, genellikle uzantıları .pfx veya .pem çalıştırılır  
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olması gerekir.
* **Sertifikanın konu adı, Service Fabric kümesine erişmek için kullandığınız etki alanı eşleşmelidir**. Bu eşleşen bir SSL kümenin HTTPS yönetim uç noktası ve Service Fabric Explorer için sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor *. cloudapp.azure.com etki alanı. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Dikkate alınması gereken diğer işlemlerden bazıları:

* **Konu** alan birden çok değere sahip olabilir. Her değer, değer türü belirtmek için bir başlatma ile önekidir. Genellikle, başlatma, **CN** (için *ortak ad*); Örneğin, **CN = www\.contoso.com**. 
* **Konu** alanı boş olabilir. 
* İsteğe bağlı **konu alternatif adı** alanın doldurulduğundan, sertifika ve SAN başına bir girişe ortak adı olması gerekir. Bunlar olarak girilir **DNS adı** değerleri. SAN'lara sahip bir sertifika oluşturma konusunda bilgi almak için bkz: [konu alternatif adı için bir güvenli LDAP sertifikası ekleme](https://support.microsoft.com/kb/931351).
* Değerini **Hedeflenen amaçlar** sertifikasının alanı içermelidir uygun bir değer gibi **sunucu kimlik doğrulaması** veya **istemci kimlik doğrulaması**.

### <a name="application-certificates-optional"></a>Uygulama sertifikaları (isteğe bağlı)
Uygulama güvenlik nedenleriyle bir kümedeki herhangi bir sayıda ek sertifikalar yüklenebilir. Kümenizi oluşturmadan önce düğümler üzerinde gibi yüklenmesi için bir sertifika gerektiren uygulama güvenliği senaryoları göz önünde bulundurun:

* Şifreleme ve şifre çözme uygulama yapılandırma değerlerini.
* Çoğaltma sırasında düğümler üzerinden verilerin şifrelenmesi.

Güvenli kümeleri oluşturma kavramı, bunlar Linux veya Windows kümelerinde aynıdır.

### <a name="client-authentication-certificates-optional"></a>İstemci kimlik doğrulama sertifikaları (isteğe bağlı)
Herhangi bir sayıda ek sertifikalar, yönetici veya kullanıcı istemci işlemleri için belirtilebilir. Karşılıklı kimlik doğrulaması gerekli olduğunda, istemci bu sertifikayı kullanabilirsiniz. İstemci sertifikaları genellikle bir üçüncü taraf CA tarafından verilen değil. Bunun yerine, geçerli kullanıcının konuma alanı kişisel mağazasında genellikle var. bir kök yetkilisi tarafından yerleştirilen istemci sertifikaları içerir. Sertifika olmalıdır bir **Hedeflenen amaçlar** değerini **istemci kimlik doğrulaması**.  

Varsayılan olarak, küme sertifikası Yöneticisi istemci ayrıcalıklarına sahiptir. Bu ek istemci sertifikalarını kümeye yüklü olmamalıdır, ancak küme yapılandırmasında izin olarak belirtilir.  Ancak, istemci sertifikalarını kümeye bağlanın ve herhangi bir işlemi gerçekleştirmek için istemci bilgisayarlarında yüklü olması gerekir.

> [!NOTE]
> Tüm yönetim işlemlerini bir Service Fabric kümesinde sunucu sertifikaları gerektirir. İstemci sertifikaları Yönetim için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar
* [Resource Manager şablonu kullanarak Azure'da bir küme oluşturma](service-fabric-cluster-creation-via-arm.md) 
* [Azure portalını kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
