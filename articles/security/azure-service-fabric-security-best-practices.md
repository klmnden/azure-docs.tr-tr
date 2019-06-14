---
title: Azure Service Fabric en iyi güvenlik yöntemleri | Microsoft Docs
description: Bu makalede, Azure Service Fabric güvenliği için en iyi yöntemler kümesi sağlar.
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
ms.date: 01/16/2019
ms.author: tomsh
ms.openlocfilehash: 8bafc4a95ca9af4567ed70c190a72f3b351da47c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611518"
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric en iyi güvenlik uygulamaları
Hızlı, kolay ve uygun maliyetli, azure'da bir uygulamayı dağıtma. Bulut uygulamanızı üretime dağıtmadan önce uygulamanızda güvenli kümeleri uygulamak için önemli ve önerilen en iyi yöntemler listemizi gözden geçirin.

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

En iyi her uygulama için biz açıklar:

-   Nelerin iyi bir uygulamadır.
-   Neden en iyi uygulamalıdır.
-   En iyi uygulamayıp ne meydana gelebilir.
-   Nasıl en iyi uygulamak bilgi edinebilirsiniz.

Aşağıdaki Azure Service Fabric güvenlik en iyi öneririz:

-   Güvenli küme oluşturulması için Azure Resource Manager şablonları ve Service Fabric PowerShell modülünü kullanın.
-   X.509 sertifikaları kullanma.
-   Güvenlik ilkeleri yapılandırın.
-   Reliable Actors güvenlik yapılandırmasını uygular.
-   Azure Service Fabric için SSL'yi yapılandırma.
-   Ağ yalıtımını ve güvenlik, Azure Service Fabric ile kullanın.
-   Azure Key Vault için güvenliği yapılandırma.
-   Kullanıcıları rollere atarsınız.


## <a name="best-practices-for-securing-your-clusters"></a>Kümelerinize güvenliğini sağlamaya yönelik en iyi yöntemler

Her zaman güvenli bir küme kullanın:
-   Küme güvenliği sertifikaları kullanarak uygulayın.
-   İstemci erişim sağlamak (Yönetici ve salt okunur) kullanarak Azure Active Directory (Azure AD).

Otomatik dağıtımlar kullanın:
-   Komut dosyaları oluşturmak, dağıtmak ve gizli dizileri almak için kullanın.
-   Gizli dizileri Azure Key Vault'ta Store ve diğer tüm istemci erişimi için Azure AD'yi kullanın.
-   Gizli dizileri İnsan erişim için kimlik doğrulaması gerektirir.

Ayrıca, aşağıdaki yapılandırma seçeneklerini göz önünde bulundurun:
-   Çevre ağları (arındırılmış bölge, DMZ'ler ve denetimli alt ağ olarak da bilinir), Azure ağ güvenlik grupları (Nsg'ler) kullanarak oluşturun.
-   Küme sanal makinelerine (VM'ler) erişebilir veya atlama sunucuları ile Uzak Masaüstü bağlantısı kullanarak kümenize yönetin.

Kümeleriniz, yetkisiz kullanıcıların bağlanma, özellikle de bir küme üretim ortamında çalışırken engellemek için güvenli hale getirilmelidir. Güvenli olmayan bir kümeye oluşturmak mümkün olsa da, küme yönetim uç noktalarını genel İnternet'e sunarsa, anonim kullanıcılar kümenize bağlanabilir.

Üç [senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) çeşitli teknolojiler kullanarak küme güvenliği uygulamak için:

-   Düğümden düğüme güvenlik için: Bu senaryo, Vm'leri kümedeki bilgisayarlar arasındaki iletişimi korur. Bu formu güvenlik uygulamaları ve Hizmetleri kümedeki Kümeye katılma yetkisi olan bilgisayarları barındırabilir sağlar.
Bu senaryo, Azure'da çalıştırılan kümeleri veya Windows üzerinde çalışan tek başına kümeler kullanabilirsiniz [sertifika güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) veya [Windows Güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) için Windows Server makineleri.
-   İstemci düğümü güvenlik: Bu senaryo, bir Service Fabric istemci kümedeki tek tek düğümler arasındaki iletişimi korur.
-   Rol tabanlı erişim denetimi (RBAC): Bu senaryo ayrı kimlikleri kullanır (Sertifikalar, Azure AD, vb.) kümeye erişen her yönetici ve kullanıcı istemci rolü için. Kümeyi oluşturduğunuzda rol kimliklerini belirtin.

>[!NOTE]
>**Azure kümeleri için güvenlik önerisi:** İstemciler ve düğümden düğüme güvenliği için sertifika kimlik doğrulaması için Azure AD güvenlik kullanın.

Tek başına bir Windows kümesi yapılandırmak için bkz [tek başına bir Windows kümesi için ayarları yapılandırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Güvenli bir küme oluşturmak için Azure Resource Manager şablonları ve Service Fabric PowerShell modülünü kullanın.
Azure Resource Manager şablonlarını kullanarak güvenli bir Service Fabric kümesi oluşturmak adım adım yönergeler için bkz: [bir Service Fabric kümesi oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure Resource Manager şablonu kullanın:
-   VM sanal sabit diskleri (VHD'ler) için yönetilen depolama yapılandırmak için bir şablon kullanarak kümenize özelleştirin.
-   Sürücü, şablon için kolay yapılandırma yönetimi kullanarak ve denetim, kaynak grubunuza değiştirir.

Küme yapılandırmanızı kod olarak işleyin:
-   Dağıtım yapılandırmalarınızı denetlenirken kapsamlı olabilir.
-   Örtük komutları doğrudan kaynaklarınızı değiştirmek için kullanmaktan kaçının.

Birçok yönden [Service Fabric uygulama yaşam döngüsü](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) otomatikleştirilebilir. [Service Fabric PowerShell modülünü](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) dağıtma, yükseltme, kaldırma ve Azure Service Fabric uygulamalarını test etme için ortak görevleri otomatik hale getirir. Yönetilen API'ler ve uygulama yönetimi için HTTP API'lerini de mevcuttur.

## <a name="use-x509-certificates"></a>X.509 sertifikaları kullanma
Her zaman kümelerinizi X.509 sertifikaları veya Windows güvenliği kullanılarak güvenli hale getirin. Güvenlik yalnızca küme oluşturma sırasında yapılandırılır. Küme oluşturulduktan sonra güvenliği etkinleştirmek mümkün değildir.

Belirtmek için bir [küme sertifikası](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), değerini **ClusterCredentialType** X509 özelliği. Dış bağlantıları için bir sunucu sertifikası belirtmek için ayarlayın **ServerCredentialType** X509 özelliği.

Ayrıca, bu uygulamaları izleyin:
-   Üretim kümeleri için sertifikaları doğru şekilde yapılandırılmış bir Windows Server sertifika hizmetini kullanarak oluşturun. Ayrıca, onaylanmış bir sertifika yetkilisinden (CA) sertifikaları edinebilirsiniz.
-   Hiçbir zaman geçici kullanın veya sertifika MakeCert.exe veya benzer bir araç kullanarak oluşturduysanız sertifika üretim kümeleri için test edebilirsiniz.
-   Test kümeleri için ancak üretim kümeleri için otomatik olarak imzalanan bir sertifika kullanın.

Küme güvenli değildir, herkes anonim olarak kümeye bağlanın ve yönetim işlemleri. Bu nedenle, her zaman üretim kümeleri X.509 sertifikaları veya Windows güvenliği kullanılarak güvenli hale getirin.

X.509 sertifikaları kullanma hakkında daha fazla bilgi edinmek için [bir Service Fabric kümesi ekleme veya kaldırma sertifikalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Güvenlik ilkelerini yapılandırma
Service Fabric ayrıca uygulamalar tarafından kullanılan kaynakların güvenliğini sağlar. Kaynak dosyalar, dizinler, ister ve uygulama dağıtıldığında, sertifikaları kullanıcı hesapları altında depolanır. Bu özellik çalışan uygulamalar da paylaşılan barındırılan bir ortamda birbirinden, daha güvenli hale getirir.

-   Bir Active Directory etki alanı grubu veya kullanıcı kullanın: Bir Active Directory kullanıcı veya grup hesabı için kimlik bilgileri altında hizmet çalıştırın. Active Directory şirket içi Azure Active Directory ve etki alanı içinde kullandığınızdan emin olun. Etki alanındaki bir etki alanı kullanıcısı veya grubu kullanarak izinleri verilmiş olması diğer kaynaklara erişin. Örneğin, dosya paylaşımları gibi kaynaklar.

-   HTTP ve HTTPS Uç noktalara yönelik güvenlik erişim ilkesi atayın: Belirtin **SecurityAccessPolicy** özelliği uygulamak için bir **RunAs** hizmet bildirimi HTTP uç noktası kaynaklarla bildirir, bir hizmet ilkesi. İçin HTTP uç noktalarını ayrılmış bağlantı noktaları doğru erişim kontrol altında çalışacağı RunAs kullanıcı hesabı için listeleridir. İlke ayarlanmamış, http.sys Erişim hizmetine sahip değil ve istemciden çağrıları sahip olan hatalar alabilirsiniz.

Bir Service Fabric kümesi güvenlik ilkelerini kullanma konusunda bilgi almak için bkz: [uygulamanıza yönelik güvenlik ilkeleri yapılandırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="implement-the-reliable-actors-security-configuration"></a>Reliable Actors güvenlik yapılandırmasını uygulayın
Service Fabric Reliable Actors aktör tasarım deseni uygulamasıdır. Tüm yazılım tasarım deseni ile gibi belirli bir desene karar yazılım sorunu deseni uygun üzerinde temel alır.

Genel olarak, model çözüm aşağıdaki yazılım sorunlarının veya güvenlik senaryoları için yardımcı olmak için aktör tasarım deseni kullanın:
-   Çok sayıda sorunu alanınızı içerir (binlerce veya daha fazlası) küçük, bağımsız ve ayrı birim durumu ve mantığı.
-   Dış bileşenlerinden durumu aktörler kümesi sorgulama dahil olmak üzere önemli etkileşim gerektirmeyen tek iş parçacıklı nesnelerle çalışıyoruz.
-   Aktör örneklerinizi çağıranlar beklenmeyen gecikmeler ile g/ç işlemleri göndererek engellemez.

Service Fabric'te Reliable Actors uygulaması framework aktörler uygulanır. Bu çerçeve aktör deseni temel alınarak ve üst kısmındaki yerleşik [Service Fabric güvenilir hizmetler](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Yazdığınız her güvenilir aktör bölümlenmiş bir durum bilgisi olan reliable Services hizmetidir.

Her aktör, bir .NET nesnesini .NET türünün bir örneği olduğu şekilde özdeş bir aktör türü örneği olarak tanımlanır. Örneğin, bir **aktör türü** uygulayan bir hesap makinesi işlevselliğini çeşitli düğümler bir küme genelinde dağıtılan birçok aktör türü olabilir. Her bir dağıtılmış aktörleri benzersiz bir aktör tanımlayıcısı tarafından belirlenir.

[Çoğaltıcı güvenlik yapılandırmalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) çoğaltma sırasında kullanılan iletişim kanalını güvenli hale getirmek için kullanılır. Bu yapılandırma Hizmetleri birbirlerini çoğaltma trafiğini görmesini engelleyen ve yüksek oranda kullanılabilir verileri güvenli olmasını sağlar. Varsayılan olarak, bir boş güvenlik yapılandırma bölümü çoğaltma güvenlik engeller.
Çoğaltma, yüksek oranda güvenilir aktör durumu sağlayıcısı durumu yapmaktan sorumlu çoğaltıcı yapılandırmaları.

## <a name="configure-ssl-for-azure-service-fabric"></a>Azure Service Fabric için SSL'yi yapılandırma
Sunucu kimlik doğrulama işlemi [doğrular](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) küme yönetim uç noktalar için bir yönetim istemcisi. Yönetim istemci ardından gerçek bir küme Bahsediyor tanır. Bu sertifika ayrıca sağlar bir [SSL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) ve Service Fabric Explorer HTTPS üzerinden HTTPS yönetim API'si için.
Kümeniz için özel bir etki alanı adı edinmeniz gerekir. Sertifikanın konu adı, bir sertifika yetkilisinden bir sertifika talep ettiğinizde, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Bir uygulama için SSL'yi yapılandırmak için önce bir CA tarafından imzalanmış bir SSL sertifikası edinmeniz gerekir. CA güvenlik amacıyla SSL sertifikaları veren güvenilen bir üçüncü taraf'dır. Bir SSL sertifikası zaten yoksa, bir SSL sertifikaları satan bir şirketten edinmeniz gerekir.

Sertifika, azure'da SSL sertifikaları için aşağıdaki gereksinimleri karşılaması gerekir:
-   Sertifika özel anahtar içermelidir.

-   Sertifika, anahtar değişimi için oluşturulmuş olması gerekir ve bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir.

-   Sertifikanın konu adı, bulut hizmetine erişmek için kullanılan etki alanı adı eşleşmelidir.

    - Bulut hizmetinize erişmek için kullanılacak özel etki alanı adını alın.
    - Hizmetinizin özel etki alanı adıyla eşleşen bir konu adına sahip bir CA'dan bir sertifika isteyin. Örneğin, özel etki alanı adınızı ise __contoso__ **.com**, Sertifika yetkilinizden sertifikası konu adı olmalıdır **. contoso.com** veya __www__ **. contoso.com**.

    >[!NOTE]
    >Bir CA için bir SSL sertifikası alınamıyor __cloudapp__ **.net** etki alanı.

-   Sertifikanın en az 2.048 bit şifreleme kullanmanız gerekir.

HTTP protokolü, güvenli ve İleri gizli dinleme saldırılarına maruz. HTTP üzerinden aktarılan veriler web sunucusuna veya diğer uç noktalar arasında web tarayıcısından düz metin olarak gönderilir. Saldırganlar kesebilir ve kredi kartı Detayları ve hesap oturum açma bilgileri gibi HTTP üzerinden gönderilen hassas verileri görüntüleyebilecek. Veri gönderildikten veya HTTPS aracılığıyla bir tarayıcı aracılığıyla gönderilen, SSL hassas bilgileri şifrelenmiş ve durdurma güvenli olmasını sağlar.

SSL sertifikalarının kullanılması hakkında daha fazla bilgi için bkz: [Azure uygulamaları için SSL yapılandırma](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="use-network-isolation-and-security-with-azure-service-fabric"></a>Ağ yalıtımını ve güvenlik, Azure Service Fabric ile kullanma
Bir 3 nodetype güvenli kullanarak küme oluşturma [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) örnek olarak. Ağ güvenlik grupları ve şablon kullanarak gelen ve giden ağ trafiğini denetler.

Şablon, her sanal makine ölçek kümeleri için bir NSG bulunur ve kümenin içine ve dışına trafiği denetlemek için kullanılır. Varsayılan olarak tüm gerekli sistem hizmetleri ve şablonda belirtilen uygulama bağlantı noktaları için izin verecek şekilde yapılandırılmış bir kural. Bu kuralları gözden geçirin ve uygulamalarınız için yeni kurallar ekleme dahil olmak üzere, kendi gereksinimlerinize uyacak şekilde herhangi bir değişiklik yapın.

Daha fazla bilgi için [Azure Service Fabric genel ağ senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-azure-key-vault-for-security"></a>Güvenlik için Azure anahtar kasası ayarlama
Service Fabric, kimlik doğrulaması ve şifreleme için bir küme ile uygulamalarının güvenliğini sağlamak için sertifikaları kullanır.

Service Fabric küme güvenliğini sağlama ve uygulama güvenlik özellikleri sağlamak için X.509 sertifikaları kullanır. Azure anahtar Kasası'na kullandığınız [sertifikaları yönetme](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) azure'da Service Fabric kümeleri için. Kümeleri oluşturan Azure kaynak sağlayıcısı sertifikaları key vault çeker. Azure'da küme dağıtıldığında sağlayıcı sertifikaları Vm'lerde ardından yükler.

Bir sertifika ilişki arasında mevcut [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), Service Fabric kümesine ve sertifikaları kullanan kaynak sağlayıcısı. Sertifika ilişki hakkında bilgi, küme oluşturulduğunda bir anahtar Kasası'nda depolanır.

Bir anahtar kasası ayarlama için iki temel adım vardır:
1. Özel anahtar kasanız için bir kaynak grubu oluşturun.

    Anahtar Kasası'nı kendi kaynak grubuna ekleyin öneririz. Bu eylem, diğer kaynak gruplarını, depolama, işlem veya kümenizi içeren grubun gibi kaldırılırsa anahtarlarınızı ve gizli bilgilerinizi kaybı önlemeye yardımcı olur. Anahtar kasanızı içeren kaynak grubunu kullanıyorsa kümeyle aynı bölgede olması gerekir.

2. Yeni kaynak grubunda bir anahtar kasası oluşturun.

    Anahtar kasası dağıtım için etkinleştirilmesi gerekir. İşlem kaynak sağlayıcısı sertifikaları kasadan almak ve bunları kullanarak VM örneklerine yükleyin.

Bir anahtar kasası ayarlama hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-to-roles"></a>Kullanıcı rollerine atama
Kümenizi temsil etmek için uygulamaları oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen roller atama: salt okunur ve yönetici Azure portalını kullanarak bu roller atayabilirsiniz.

>[!NOTE]
> Service Fabric'te rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı Access Control Service Fabric istemciler için](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric, bağlı istemciler için iki erişim denetim türleri destekleyen bir [Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): Yönetici ve kullanıcı. Küme Yöneticisi, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlandırmak için erişim denetimini kullanabilirsiniz. Erişim denetimi, küme daha güvenli hale getirir.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric güvenlik denetim listesi](azure-service-fabric-security-checklist.md)
- Service fabric'i ayarlama [geliştirme ortamı](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Hakkında bilgi edinin [Service Fabric destek seçenekleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).
