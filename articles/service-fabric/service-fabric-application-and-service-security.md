---
title: Azure Service Fabric uygulama güvenliği hakkında bilgi edinin | Microsoft Docs
description: Service Fabric'te mikro hizmet uygulamaları güvenli bir şekilde çalışmasına nasıl genel bakış. Farklı güvenlik hesapları altında Hizmetleri ve başlangıç komut dosyasını çalıştırın, kimlik doğrulaması ve kullanıcıları yetkilendirme, uygulama parolalarını yönetme, güvenli hizmet iletişimleri, bekleyen bir API ağ geçidi ve güvenli uygulama verileri kullanma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: aljo
ms.openlocfilehash: cb0f750f4049a1ce652c829f43928a95f30e6973
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66302234"
---
# <a name="service-fabric-application-and-service-security"></a>Service Fabric uygulaması ve hizmet güvenliği
Bir mikro hizmet mimarisi getirebilirsiniz [birçok avantaj](service-fabric-overview-microservices.md). Mikro hizmetler, güvenliğini yönetme ancak sınama ve geleneksel tek parçalı uygulamalarla güvenliğini yönetme farklı olabilir. 

Monoliti, uygulama genellikle bir ağdaki bir veya daha fazla sunucu üzerinde çalışan ve API'ları ve IP adresi ve kullanıma sunulan bağlantı noktalarını tanımlamak kolaydır. Var olan genellikle bir çevre veya sınır ve korumak için bir veritabanı. Bu sistem güvenlik ihlali veya saldırı nedeniyle tehlikedeyse sistemdeki her şey için bir saldırgan kullanılabilir olacağını olasıdır. Mikro hizmetler ile sistem daha karmaşıktır.  Hizmetler merkezi olmayan tercihinizden ve pek çok konak genelinde dağıtılan ve ana bilgisayardan diğerine geçirin.  Uygun güvenlik ile bir saldırganın alabilirsiniz ayrıcalıklarına ve tek bir saldırı kullanılabilir veri miktarı tarafından bir hizmet ihlal sınırlayın.  İletişim iç değildir, ancak bir ağ üzerinden gerçekleşir ve pek çok kullanıma sunulan bağlantı noktaları ve hizmetler arasındaki etkileşimler vardır. Bu hizmet etkileşimleri nedir ve ne zaman meydana bilerek, uygulama güvenliği için önemlidir.

Bu makalede, mikro hizmetler Güvenlik Kılavuzu değil, kullanılabilir birçok kaynak çevrimiçi ancak güvenliğin nasıl farklı yönlerine açıklar Service Fabric'te gerçekleştirilebilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Genellikle, gerekli kaynak ve hizmet tarafından sunulan API'ler belirli güvenilen kullanıcıların veya istemciler için sınırlı olur. Kimlik doğrulaması güvenilir bir şekilde bir kullanıcının kimliğini ascertaining işlemidir.  Yetki API'leri yapan veya kullanılabilir hizmetleri işlemi, bazı kullanıcılar, ancak diğerlerini kimlik doğrulaması.

### <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulama API düzeyinde güven kararları için ilk adımdır. Kimlik doğrulaması güvenilir bir şekilde bir kullanıcının kimliğini ascertaining işlemidir.  Mikro hizmet senaryolarda, kimlik doğrulaması genellikle merkezi olarak yönetilir. Bir API ağ geçidi kullanıyorsanız, yapabilecekleriniz [kimlik doğrulaması boşaltma](/azure/architecture/patterns/gateway-offloading) ağ geçidine. Bu yaklaşımı kullanın, ek güvenlik iletileri kimlik doğrulaması yerinde olup olmadığını ağ geçidinden veya geldikleri olmadıkça tek tek Hizmetleri doğrudan (API ağ geçidi) ulaşılamıyor emin olun.

Hizmetleri doğrudan erişilebilir, kimlik doğrulama hizmeti Azure Active Directory veya bir güvenlik belirteci hizmeti (STS), kullanıcıların kimliklerini doğrulamak için kullanılabilir davranan bir özel kimlik doğrulama mikro hizmet gibi. Güven kararları güvenlik belirteçleri veya tanımlama bilgileri ile hizmetler arasında paylaşılır. 

ASP.NET Core için birincil mekanizma [kullanıcıların kimliğini doğrulama](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) ASP.NET Core kimliği üyelik sistemidir. ASP.NET Core kimliği geliştirici tarafından yapılandırılmış bir veri deposu (oturum açma bilgileri, rolleri ve talep dahil) kullanıcı bilgilerini depolar. ASP.NET Core kimliği iki öğeli kimlik doğrulamayı destekler.  Microsoft, Google, Facebook veya Twitter gibi sağlayıcılarından mevcut kimlik doğrulama işlemleri kullanarak kullanıcıların oturum için Dış kimlik doğrulama sağlayıcılarını da desteklenir.

### <a name="authorization"></a>Yetkilendirme
Kimlik doğrulamasından sonra kullanıcı erişim yetkisi vermek veya belirlemek hizmetlerini gerekir. hangi kullanıcı işlemleri gerçekleştirebilirsiniz. Bu işlem, bazı kimliği doğrulanmış kullanıcılar tarafından kullanılabilir, ancak çok tüm API'leri yapmak bir hizmet sağlar. Yetkilendirme, dikey ve bağımsız bir kullanıcısının kim olduğunu ascertaining işlemidir kimlik doğrulaması'ndan gerçekleşir. Kimlik doğrulaması, geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.

[ASP.NET Core yetkilendirme](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) kullanıcıların rollere göre Bitti veya talep veya diğer buluşsal yöntemler inceleyerek içerebilir özel ilkesini temel alarak.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>Kısıtlama ve bir API ağ geçidi kullanarak erişim güvenliğini sağlama
Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. Bir [API ağ geçidi](/azure/architecture/microservices/gateway) istemciler ve hizmetler arasında yer alan ve uygulamanızı sağlayan tüm hizmetler için giriş noktasıdır. Ters Ara sunucu, istemcilerden gelen yönlendirme isteklerini hizmetlerine görür. Ayrıca, kimlik doğrulama ve yetkilendirme, SSL sonlandırma ve oran sınırlandırma gibi çeşitli çapraz görevleri de gerçekleştirebilir. Bir ağ geçidi dağıtmanıza gerek yoktur, istemcileri doğrudan ön uç Hizmetleri için istek göndermeniz gerekir.

Service Fabric'te, bir ağ geçidi herhangi bir durum bilgisi olmayan hizmet gibi olabilir bir [ASP.NET Core uygulaması](service-fabric-reliable-services-communication-aspnetcore.md), veya başka bir hizmet gibi trafik girişi için tasarlanmış [Traefik](https://docs.traefik.io/), [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/), veya [Azure API Management](https://docs.microsoft.com/azure/api-management).

API Management, zengin bir arka uç Service Fabric hizmetleriniz için yönlendirme kuralları kümesiyle API'ler yayımlamanıza olanak tanıyan, Service Fabric ile doğrudan tümleşir.  Arka uç hizmetlerine güvenli erişim, azaltma ile DOS saldırılarını veya API anahtarları, JWT belirteçleri, sertifikaları ve diğer kimlik bilgilerini doğrulayın. Daha fazla bilgi edinmek için [Service Fabric ile Azure API Yönetimi'ne genel bakış](service-fabric-api-management-overview.md).

## <a name="manage-application-secrets"></a>Uygulama parolalarını yönetme
Gizli dizileri depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini değil diğer değerleri gibi herhangi bir önemli bilgi olabilir. Bu makalede, anahtarları ve parolaları yönetmek için Azure anahtar kasası kullanır. Ancak, *kullanarak* gizli bir uygulamadaki herhangi bir yerde barındırılan bir kümeye dağıtılması uygulamalara izin vermek üzere platformdan bağımsız bulut.

Hizmet yapılandırma ayarlarını yönetmek için önerilen yöntem aracılığıyladır [hizmet yapılandırma paketleri][config-package]. Yapılandırma paketlerini tutulan ve yönetilen yükseltmelerini uygunluk durumu doğrulama ve Otomatik geri alma ile üzerinden güncelleştirilebilir. Bu, küresel hizmet kesintisi olasılığını azaltır gibi genel yapılandırma için tercih edilir. Şifrelenmiş gizli dizileri hiçbir özel durumdur. Service Fabric, şifreleme ve şifre çözme sertifikası şifreleme kullanarak bir yapılandırma paketi Settings.xml dosyasının değerleri için yerleşik özelliklere sahiptir.

Aşağıdaki diyagramda bir Service Fabric uygulamasında gizli dizi yönetimi için temel akış gösterilmektedir:

![Gizli dizi Yönetimi'ne genel bakış][overview]

Bu akışta dört ana adım vardır:

1. Veri şifreleme sertifikası alın.
2. Sertifika, kümenizin yükleyin.
3. Sertifika ile bir uygulama dağıtırken gizli değerleri şifreler ve bunları bir hizmetin Settings.xml yapılandırma dosyasına yerleştirir.
4. Settings.xml dışında şifrelenmiş değerler ile aynı şifreleme sertifikası şifresini çözerek okuyun. 

[Azure Key Vault] [ key-vault-get-started] burada Azure Service Fabric kümelerinde yüklü sertifikaları almak için bir yol ve sertifika için bir güvenli depolama konumu olarak kullanılır. Azure'a dağıtmıyor, Service Fabric uygulamaları içindeki gizli verileri yönetmek için Key Vault'u kullanın gerekmez.

Bir örnek için bkz. [uygulama parolalarını yönetme](service-fabric-application-secret-management.md).

## <a name="secure-the-hosting-environment"></a>Barındırma ortamının güvenliğini sağlama
Azure Service Fabric kullanarak kümedeki farklı bir kullanıcı hesabı altında çalışan uygulamaların güvenliğini sağlayabilirsiniz. Service Fabric uygulamaları tarafından kullanıcı hesaplarını altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları güvenli yardımcı olur. Bu çalışan uygulamalar da paylaşılan bir barındırılan ortamda, diğerinden daha güvenli hale getirir.

Uygulama bildirimi, hizmetler ve güvenli kaynaklara gerekli güvenlik Sorumlular (kullanıcılar ve gruplar) çalıştırın bildirir.  Bu güvenlik sorumluları İlkeleri'nde, çalıştırma olarak uç nokta bağlama örneğin başvurulan paket paylaşımı veya güvenlik erişim ilkeleri.  İlkeleri sonra hizmet kaynaklarını uygulandığı **Servicemanifestımport** uygulama bildiriminin.

İlkeleri bildirirken tanımlayabilir ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilecek her gruba eklenebilir. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilir olan belirli ortak ayrıcalıklara sahip olması için ihtiyaç duydukları zaman yararlıdır.

Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesabın altına çalıştırın. Service Fabric uygulamaları bir yerel kullanıcı hesabı veya uygulama bildirimini içinde belirtilen yerel sistem hesabı altında çalıştırma özelliği de sağlar. Daha fazla bilgi için [yerel kullanıcı hesabı veya yerel sistem hesabı olarak bir hizmet çalıştırma](service-fabric-application-runas-security.md).  Ayrıca [yerel kullanıcı veya sistem hesabı olarak bir hizmet başlangıcı komut dosyası çalıştırma](service-fabric-run-script-at-service-startup.md).

Service Fabric Windows tek başına küme üzerinde çalıştırırken, altında bir hizmeti çalıştırabileceğiniz [Active Directory etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya [Grup yönetilen hizmet hesapları](service-fabric-run-service-as-gmsa.md).

## <a name="secure-containers"></a>Güvenli kapsayıcılar
Service Fabric Hizmetleri (sürüm 5.7 veya daha yüksek) bir Windows veya Linux küme düğümlerinde yüklü bir sertifika erişmek için bir kapsayıcı içinde bir mekanizma sağlar. Bu PFX sertifika kimlik doğrulaması, uygulama veya hizmet veya diğer hizmetleri ile güvenli iletişim için kullanılabilir. Daha fazla bilgi için [sertifika bir kapsayıcıya alma](service-fabric-securing-containers.md).

Ayrıca, Service Fabric Windows kapsayıcıları için gmsa'yı (Grup yönetilen hizmet hesapları) da destekler. Daha fazla bilgi için [Windows kapsayıcıları için gMSA ayarlama](service-fabric-setup-gmsa-for-windows-containers.md).

## <a name="secure-service-communication"></a>Hizmet iletişimi güvenli hale getirme
Service Fabric'te hizmet genellikle birden çok VM arasında dağıtılmış bir Service Fabric kümesindeki herhangi bir yerde çalışır. Service Fabric hizmeti iletişimlerin güvenliğini sağlamak için çeşitli seçenekler sunar.

HTTPS uç noktalarını etkinleştirebilirsiniz, [ASP.NET Core veya Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) web hizmetleri.

Bu nedenle bir uçtan uca güvenli kanal etkinleştirme ters proxy ve hizmetler arasında güvenli bağlantı kurabilirsiniz. Ters proxy HTTPS üzerinde dinleyecek şekilde yapılandırıldığında güvenli hizmetlere bağlanma desteklenir. Ters proxy yapılandırma hakkında daha fazla bilgi için okuma [ters proxy Azure Service fabric'te](service-fabric-reverseproxy.md).  [Güvenli bir hizmete bağlanabilir](service-fabric-reverseproxy-configure-secure-communication.md) ters proxy ve hizmetler arasında güvenli bağlantı kurmak açıklar.

Reliable Services uygulaması çerçevesi, birkaç önceden oluşturulmuş iletişim yığınlarını ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Uzaktan hizmet iletişimini kullanırken, Güvenliği geliştirmeyi öğrenin (içinde [ C# ](service-fabric-reliable-services-secure-communication.md) veya [Java](service-fabric-reliable-services-secure-communication-java.md)) veya bu adı kullanıyor [WCF](service-fabric-reliable-services-secure-communication-wcf.md).

## <a name="encrypt-application-data-at-rest"></a>Bekleyen uygulama verilerini şifrele
Her [düğüm türü](service-fabric-cluster-nodetypes.md) bir Service Fabric kümesinde çalışan tarafından yedeklenen bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz.  Hizmetlerinizi bir bağlı veri diski verileri kaydederseniz, yapabilecekleriniz [söz konusu veri disklerini şifreler](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) uygulama verilerinizi korumak için.

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Hizmetin başlatılması sırasında bir kurulum betiğini çalıştırın](service-fabric-run-script-at-service-startup.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [Küme güvenliği hakkında bilgi edinin](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-overview.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png