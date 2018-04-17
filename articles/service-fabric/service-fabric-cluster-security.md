---
title: Azure Service Fabric kümesi güvenli | Microsoft Docs
description: Azure Service Fabric kümesi ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojileri için güvenlik senaryoları hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2017
ms.author: aljo
ms.openlocfilehash: f60b428ba7fe93713af68851a3e9d246a3b1641b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="service-fabric-cluster-security-scenarios"></a>Service Fabric kümesi güvenlik senaryoları
Azure Service Fabric kümesi sahip olduğunuz bir kaynaktır. Sizin sorumluluğunuzdadır kümelerinizi yetkisiz kullanıcıların kendilerine bağlanmasını önlemek için güvenli değil. Kümede üretim iş yükleri çalıştırırken güvenli küme özellikle önemlidir. Küme genel internet yönetim uç noktalarını kullanıma sunar, güvenli olmayan bir küme oluşturmak mümkün olmasına karşın, anonim kullanıcılar için bağlanabilir. Güvenli olmayan kümeler üretim iş yükleri için desteklenmez. 

Bu makalede Azure kümeler ve tek başına kümeleri ve bunları uygulamak için kullanabileceğiniz çeşitli teknolojileri için güvenlik senaryoları genel bir bakış verilmiştir:

* Düğümü düğümü güvenlik
* İstemcisi düğümü güvenlik
* Rol Tabanlı Erişim Denetimi (RBAC)

## <a name="node-to-node-security"></a>Düğümü düğümü güvenlik
Düğümü düğümü güvenlik VM'ler veya bir kümede bilgisayarlar arasında güvenli iletişim yardımcı olur. Bu güvenlik senaryo, kümeye katılmak için yetkili bilgisayarları uygulamaları ve Hizmetleri kümedeki barındırma katılabilir sağlar.

![Düğümü düğümü iletişim diyagramı][Node-to-Node]

Windows üzerinde hem çalışan Azure ve tek başına kümeleri üzerinde çalışan kümelerle kullanabilirsiniz [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenliği](https://msdn.microsoft.com/library/ff649396.aspx) Windows Server bilgisayarları için.

### <a name="node-to-node-certificate-security"></a>Düğümü düğümü sertifika güvenliği
Service Fabric Küme oluşturduğunuzda, belirttiğiniz düğüm türü yapılandırmasının bir parçası X.509 sunucu sertifikaları kullanır. Bu makalenin sonunda, bu sertifikalar nedir ve nasıl elde veya bunları oluşturmanız kısa bir genel bakış görebilirsiniz.

Küme ya da Azure portalında bir Azure Resource Manager şablonu kullanarak veya tek başına JSON şablonunu kullanarak oluşturduğunuz sertifika güvenliği ayarlanır. Birincil bir sertifika ve sertifika rollover için kullanılan bir isteğe bağlı ikincil sertifika ayarlayabilirsiniz. Ayarladığınız birincil ve ikincil sertifikalar için ayarladığınız salt okunur istemci sertifikalarını ve yönetici istemci farklı [istemcisi düğümü güvenlik](#client-to-node-security).

Azure için sertifika güvenliği kümedeki ayarlamak öğrenmek için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme ayarlama](service-fabric-cluster-creation-via-arm.md).

Sertifika Güvenliği tek başına Windows Server kümesi için bir kümedeki ayarlamak öğrenmek için bkz: [X.509 sertifikalarını kullanarak Windows tek başına kümede güvenli](service-fabric-windows-cluster-x509-security.md).

### <a name="node-to-node-windows-security"></a>Düğümü düğümü Windows güvenliği
Bir tek başına Windows Server kümesi için Windows güvenliği ayarlama hakkında bilgi edinmek için bkz: [Windows güvenliği kullanarak Windows tek başına kümede güvenli](service-fabric-windows-cluster-windows-security.md).

## <a name="client-to-node-security"></a>İstemcisi düğümü güvenlik
İstemcisi düğümü güvenlik, istemcilerin kimliğini doğrular ve bir istemci ve bireysel düğümleri arasında güvenli iletişim yardımcı olur. Bu tür güvenlik küme ve kümedeki dağıtılan uygulamalar yalnızca yetkili kullanıcılar erişebildiğinden emin olun yardımcı olur. İstemciler, Windows güvenlik kimlik bilgilerini veya sertifika güvenlik kimlik bilgilerini benzersiz şekilde tanımlanır.

![İstemci düğüme iletişim diyagramı][Client-to-Node]

Windows üzerinde hem çalışan Azure ve tek başına kümeleri üzerinde çalışan kümelerle kullanabilirsiniz [sertifika güvenlik](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows Güvenliği](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>İstemci düğüm sertifika güvenliği
Resource Manager şablonu kullanarak veya tek başına JSON şablonunu kullanarak kümeyi Azure portalında ya da oluşturduğunuzda istemcisi düğümü sertifika güvenliği ayarlayın. Sertifikayı oluşturmak için bir yönetici istemci sertifikası veya bir kullanıcı istemci sertifikası belirtin. En iyi uygulama, belirttiğiniz yönetici istemci ve kullanıcı istemci sertifikalarını belirtmek için birincil ve ikincil sertifikalardan farklı olmalıdır [düğümü düğümü güvenlik](#node-to-node-security). Varsayılan olarak, küme sertifikaları düğümü düğümü güvenlik için izin verilen istemci yönetim sertifikalar listesine eklenir.

Yönetim sertifikasını kullanarak kümeye bağlanan istemciler, yönetim özellikleri için tam erişime sahip. Salt okunur kullanıcı istemci sertifikası kullanarak kümeye bağlanan istemciler yönetim özellikleri yalnızca okuma erişimi var. Bu sertifikalar, bu makalenin sonraki bölümlerinde açıklanan RBAC için kullanılır.

Azure için sertifika güvenliği kümedeki ayarlamak öğrenmek için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme ayarlama](service-fabric-cluster-creation-via-arm.md).

Sertifika Güvenliği tek başına Windows Server kümesi için bir kümedeki ayarlamak öğrenmek için bkz: [X.509 sertifikalarını kullanarak Windows tek başına kümede güvenli](service-fabric-windows-cluster-x509-security.md).

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Azure üzerinde istemci düğümünde Azure Active Directory güvenliği
Azure üzerinde çalışan kümeler için Azure Active Directory (Azure AD) kullanarak da Yönetim uç noktalarının erişimin güvenliğini sağlayabilirsiniz. Gerekli oluşturmayı öğrenmek için Azure AD yapıları nasıl küme ve daha sonra kümelerine bağlanmak nasıl oluşturduğunuzda, bunları doldurmak için bkz: [bir Azure Resource Manager şablonu kullanarak bir küme ayarlama](service-fabric-cluster-creation-via-arm.md).

## <a name="security-recommendations"></a>Güvenlik önerileri
Düğümü düğümü güvenlik için Azure kümeleri için istemcileri ve sertifika kimlik doğrulaması için Azure AD güvenlik kullanmanızı öneririz.

Windows Server 2012 R2 ve Windows Active Directory, varsa, tek başına Windows sunucu kümeleri için Windows Güvenlik Grup yönetilen hizmet hesapları ile kullanmanızı öneririz. Aksi takdirde, Windows güvenliği Windows hesaplarıyla kullanın.

## <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Erişim denetimi, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlamak için kullanabilirsiniz. Bu, küme daha güvenli olmasına yardımcı olur. Bir kümeye bağlanan istemciler için desteklenen iki erişim denetimi türleri: Yönetici rolü ve kullanıcı rolü.

Yönetici rolüne atanan kullanıcıların tam yönetim özellikleri dahil olmak üzere okuma ve yetenekleri yazma erişimi. Varsayılan olarak, kullanıcı rolüne atanan kullanıcıların yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi var. Uygulamalar ve hizmetler de çözebilirsiniz.

Kümeyi oluşturduğunuzda yönetici ve kullanıcı istemci rolleri ayarlayın. Rolleri (ayrı kimlikleri örneğin, sertifikalar veya Azure AD kullanarak) her rol türü için sağlayarak atayın. Varsayılan erişim denetimi ayarlarını ve varsayılan ayarlarının nasıl değiştirileceği hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509 sertifikaları ve Service Fabric
X.509 dijital sertifikalar, istemcilerin ve sunucuların kimliğini doğrulamak için yaygın olarak kullanılır. Bunlar iletileri dijital olarak imzalamak ve şifrelemek için kullanılır. X.509 dijital sertifikalar hakkında daha fazla bilgi için bkz: [sertifikalarla çalışma](http://msdn.microsoft.com/library/ms731899.aspx).

Dikkate alınması gereken bazı önemli noktalar:

* Üretim iş yükleri çalıştıran kümeler için sertifikalar oluşturmak için doğru yapılandırılmış bir Windows Server sertifika hizmeti ya da bir onaylanan birinden kullanın [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Hiçbir zaman her geçici kullanın veya bir üretim ortamında MakeCert.exe gibi araçları kullanarak oluşturduğunuz sertifikaları test et.
* Ancak yalnızca bir test kümede kendinden imzalı bir sertifika kullanabilirsiniz. Kendinden imzalı bir sertifika üretimde kullanmayın.

### <a name="server-x509-certificates"></a>X.509 sertifikaları
Sunucu sertifikaları istemcilere bir sunucu (düğüm) kimlik doğrulaması ya da bir sunucu (düğüm) için bir sunucu (düğüm) kimlik doğrulaması birincil görev vardır. Bir istemci veya düğüm bir düğüm doğruladığında, biri de, ilk ortak ad değeri **konu** alan. Bu ortak adı veya sertifikalar ilgili alternatif adlarına (SAN'lar) biri izin verilen Ortak adların listesinde olmalıdır.

SAN'lara sahip sertifikalarını oluşturmak öğrenmek için bkz: [güvenli LDAP sertifika için bir konu alternatif adı eklemek nasıl](http://support.microsoft.com/kb/931351).

**Konu** alan birden çok değer olabilir. Her değer, değer türü belirtmek için bir başlatma öneki. Genellikle, başlatma işlemi **CN** (için *ortak ad*); Örneğin, **CN = www.contoso.com**. **Konu** alan boş olamaz. Varsa isteğe bağlı **konu diğer adı** alanı doldurulur, sertifika ve SAN her bir giriş'ın ortak adı olması gerekir. Bunlar olarak girilir **DNS adı** değerleri.

Değeri **Hedeflenen amaçlar** sertifikası alanı içermelidir uygun bir değer gibi **sunucu kimlik doğrulaması** veya **istemci kimlik doğrulaması**.

### <a name="client-x509-certificates"></a>İstemci X.509 sertifikaları
İstemci sertifikaları genellikle bir üçüncü taraf CA tarafından verilen değil. Bunun yerine, geçerli kullanıcının konuma kişisel deposuna var olan bir kök yetkilisi tarafından yerleştirilen istemci sertifikaları genellikle içeren bir **Hedeflenen amaçlar** değerini **istemci kimlik doğrulaması**. Karşılıklı kimlik doğrulaması gerekli olduğunda istemci bu sertifikayı kullanabilirsiniz.

> [!NOTE]
> Service Fabric kümesi üzerindeki tüm yönetim işlemlerinin sunucu sertifikaları gerektirir. İstemci sertifikalarını yönetimi için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar
* [Resource Manager şablonu kullanarak Azure'da bir küme oluşturma](service-fabric-cluster-creation-via-arm.md) 
* [Azure portalını kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
