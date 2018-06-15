---
title: Azure Service Fabric uygulama güvenliği hakkında bilgi edinme | Microsoft Docs
description: Service Fabric mikro uygulamaları güvenli bir şekilde çalıştırmayı hakkında genel bakış. Farklı güvenlik hesapları altında Hizmetleri ve başlangıç komut dosyasını çalıştırın, kimlik doğrulaması ve kullanıcılara yetki vermek, uygulama parolaları yönetmek, hizmet iletişimlerini güvenli, bekleyen bir API ağ geçidi ve güvenli uygulama verilerini kullanma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: ryanwi
ms.openlocfilehash: fa6d46186ad833b68e60c24f742d210b7845759a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207919"
---
# <a name="service-fabric-application-and-service-security"></a>Service Fabric uygulaması ve hizmet güvenliği
Mikro mimarisi getirebilirsiniz [birçok avantaj](service-fabric-overview-microservices.md). Mikro, güvenliği yönetme ancak sınama ve geleneksel tek yapılı uygulamalarının güvenliğini yönetme farklı olur. 

Bir monolith uygulama genellikle bir ağ içindeki bir veya daha fazla sunucularda çalışan ve API'ları ve IP adresi ve gösterilen bağlantı noktalarını tanımlamak kolaydır. Genellikle bir çevre veya sınır ve yoktur korumak için bir veritabanı. Bu sistem güvenlik ihlali veya saldırı nedeniyle bozulsa sistemi içinde her şeyi saldırganın kullanılabilir olacağını olasıdır. Mikro ile sistem daha karmaşıktır.  Hizmetleri merkeze bağlı ve pek çok ana bilgisayarlar arasında dağıtılmış ve ana bilgisayardan geçirin.  Uygun güvenlik ile bir saldırganın alabilirsiniz ayrıcalıkları ve tek bir saldırı kullanılabilir veri miktarı bir hizmet ihlal tarafından sınırlandırın.  İletişim iç değildir, ancak bir ağ üzerinden olur ve birçok gösterilen bağlantı noktalarını ve Hizmetleri arasındaki etkileşimler vardır. Bu hizmet etkileşimleri nedir ve ne zaman meydana bilerek, uygulama güvenliği için önemlidir.

Bu makalede mikro Güvenlik Kılavuzu değil, yok gibi birçok kaynakları çevrimiçi, ancak güvenlik nasıl farklı yönlerini açıklar Service Fabric gerçekleştirilebilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Kaynakları ve bir hizmet tarafından kullanıma sunulan API'ler belirli güvenilen kullanıcıların veya istemcileri ile sınırlı olması için genellikle gereklidir. Kimlik doğrulama güvenilir bir şekilde bir kullanıcının kimliğini ascertaining işlemidir.  Yetkilendirme işlemidir API'leri yapan veya kullanılabilir hizmetleri bazı kullanıcılar, ancak diğer kimlik doğrulaması.

### <a name="authentication"></a>Kimlik Doğrulama
Kimlik doğrulama API düzeyinde güven kararları için ilk adımdır. Kimlik doğrulama güvenilir bir şekilde bir kullanıcının kimliğini ascertaining işlemidir.  Mikro hizmet senaryolarda, kimlik doğrulama genellikle merkezi olarak işlenir. Bir API ağ geçidi kullanıyorsanız, yapabilecekleriniz [kimlik doğrulaması boşaltma](/azure/architecture/patterns/gateway-offloading) ağ geçidi. Bu yaklaşımı kullanın, ek güvenlik iletilerin kimlik doğrulaması yerinde olup olmadığını Ağ Geçidi'nden veya geldikleri olmadıkça tek tek Hizmetleri doğrudan (API ağ geçidi) ulaşılamıyor emin olun.

Hizmetleri doğrudan erişilebilir değilse, bir kimlik doğrulama hizmeti Azure Active Directory veya bir güvenlik belirteci hizmeti (STS), kullanıcıların kimliklerini doğrulamak için kullanılabilir davranan bir özel kimlik doğrulama mikro hizmet ister. Güven kararları güvenlik belirteçleri veya tanımlama bilgilerini hizmetleriyle arasında paylaşılır. 

ASP.NET Core, birincil mekanizma için [kullanıcıların kimlik doğrulaması](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) ASP.NET Core kimliği üyelik sistemidir. ASP.NET Core kimlik geliştirici tarafından yapılandırılan bir veri deposu (oturum açma bilgileri, rolleri ve talepler dahil) kullanıcı bilgilerini depolar. ASP.NET Core kimlik iki faktörlü kimlik doğrulamasını destekler.  Microsoft, Google, Facebook ve Twitter gibi sağlayıcılarından var olan kimlik doğrulama işlemleri kullanarak kullanıcıların oturum için Dış kimlik doğrulama sağlayıcıları da desteklenir. 

### <a name="authorization"></a>Yetkilendirme
Kullanıcı erişim yetkisi vermek veya belirlemek kimlik doğrulamasından sonra hizmetlerini gerekir hangi kullanıcı yapabilirsiniz. Bu işlem, bazı kimliği doğrulanmış kullanıcılar için kullanılabilir, ancak değil tüm API'leri yapmak bir hizmet sağlar. Yetkilendirme resme ve bağımsız olan bir kullanıcı ascertaining işlemidir kimlik doğrulaması gerçekleşir. Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.

[ASP.NET Core yetkilendirme](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) kullanıcıların rollerini tabanlı Bitti veya, talep veya diğer buluşsal yöntemler inceleniyor içerebilen özel ilkesini temel alarak.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>Kısıtlama ve güvenli bir API ağ geçidi kullanarak erişim
Bulut uygulamalarının normalde kullanıcılar, cihazlar ve diğer uygulamalara tek giriş noktası sağlamak için bir ön uç ağ geçidine ihtiyacı vardır. Bir [API ağ geçidi](/azure/architecture/microservices/gateway) istemcileri ve hizmetleri arasında bulunur ve uygulamanızı sağlayan tüm hizmetlere giriş noktasıdır. Yönlendirme isteklerine Hizmetleri istemcilerinden gelen ters bir proxy işlevi görür. Ayrıca, kimlik doğrulama ve yetkilendirme, SSL sonlandırma ve hız sınırlaması gibi çeşitli arası kesme görevleri de gerçekleştirebilir. Bir ağ geçidi dağıtırsanız yok, istemcileri doğrudan ön uç Hizmetleri isteği göndermeniz gerekir.

Service Fabric bir ağ geçidi herhangi bir durum bilgisiz hizmeti gibi olabilir bir [ASP.NET Core uygulama](service-fabric-reliable-services-communication-aspnetcore.md), veya başka bir hizmeti gibi trafik giriş için tasarlanmış [Træfik](https://docs.traefik.io/), [olay hub'ları](https://docs.microsoft.com/azure/event-hubs/), [IOT hub'ı](https://docs.microsoft.com/azure/iot-hub/), veya [Azure API Management](https://docs.microsoft.com/azure/api-management).

API Management Service Fabric, arka uç Service Fabric hizmetlerinize yönlendirme kuralları zengin bir dizi API yayımlamanıza olanak tanıyan ile doğrudan tümleşir.  Arka uç hizmetlerine erişim güvenliğini, azaltma ile DOS saldırılarını veya API anahtarları, JWT belirteçleri, sertifikaları ve diğer kimlik bilgilerini doğrulayın. Daha fazla bilgi edinmek için okuma [Service Fabric Azure API Management genel bakış ile](service-fabric-api-management-overview.md).

## <a name="manage-application-secrets"></a>Uygulama parolalarını yönetme
Gizli depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini olmayan diğer değerleri gibi herhangi bir önemli bilgi olabilir. Bu makalede Azure anahtar kasası anahtarları ve gizli anahtarları yönetmek için kullanır. Ancak, *kullanarak* gizli bir uygulamada herhangi bir yerde barındırılan bir kümeye dağıtılacak uygulamalar izin vermek için platform belirsiz bulut.

Hizmet yapılandırma ayarlarını yönetmek için önerilen yöntem aracılığıyladır [hizmet yapılandırma paketleri][config-package]. Yapılandırma paketleri sürümlü ve sistem durumu doğrulama ve Otomatik geri alma ile yönetilen yükseltmelerini aracılığıyla güncelleştirilebilir. Genel hizmet kesintisi olasılığını azaltır olarak bu genel yapılandırma için tercih edilir. Şifrelenmiş parolalar hiçbir istisnadır. Service Fabric şifreleme ve şifre çözme sertifikası şifreleme kullanarak bir yapılandırma paketi Settings.xml dosyasındaki değerleri için yerleşik özellikler vardır.

Aşağıdaki diyagramda Service Fabric uygulaması gizli yönetimi için temel akışı gösterilmektedir:

![Gizli yönetimine genel bakış][overview]

Bu akış dört ana adım vardır:

1. Veri şifreleme sertifikası alın.
2. Sertifikayı kümenizdeki yükleyin.
3. Sertifikayla ilgili bir uygulama dağıtırken gizli değerleri şifreler ve bir hizmetin Settings.xml yapılandırma dosyasına Ekle.
4. Settings.xml dışında şifrelenmiş değerler ile aynı şifreleme sertifikası şifresini çözerek okuyun. 

[Azure anahtar kasası] [ key-vault-get-started] burada Azure Service Fabric kümeleri yüklü sertifikaları almak için bir yol ve sertifikalar için bir güvenli depolama konumu olarak kullanılır. Azure'a dağıtma değil, anahtar kasası Service Fabric uygulamaları parolalarında yönetmek üzere kullanmak gerekmez.

Bir örnek için bkz: [uygulama parolaları yönetme](service-fabric-application-secret-management.md).

## <a name="secure-the-hosting-environment"></a>Barındırma ortamı güvenli
Azure Service Fabric kullanarak, kümedeki farklı kullanıcı hesabı altında çalışan uygulamaları güvenliğini sağlayabilirsiniz. Service Fabric uygulamaları tarafından kullanıcı hesapları altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları güvenli yardımcı olur. Bu çalışan uygulamaları bile paylaşılan bir barındırılan ortamda, diğerinden daha güvenli hale getirir.

Uygulama bildirimini hizmetlere ve güvenli kaynaklar gerekli olan güvenlik sorumlularının (kullanıcılar ve gruplar) çalıştıran bildirir.  Bu güvenlik sorumluları ilkelerinde, örneğin bağlama, Çalıştır, uç nokta başvurulan paylaşımı ya da güvenlik erişim ilkelerini paketi.  İlkeleri sonra uygulanır hizmet kaynaklarını **ServiceManifestImport** uygulama bildirimi bölümü.

Asıl adlar bildirirken tanımlayabilir ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilecek her gruba eklenebilir. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklarına sahip olmanız gerekir yararlıdır.

Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Service Fabric de bir yerel kullanıcı hesabı veya uygulama bildirimi içinde belirtilen yerel sistem hesabı altında uygulamaları çalıştırmak için yeteneği sağlar. Daha fazla bilgi için bkz: [bir hizmet bir yerel kullanıcı hesabı veya yerel sistem hesabı olarak çalıştırılması](service-fabric-application-runas-security.md).  Ayrıca [hizmet başlangıç komut dosyasını bir yerel kullanıcı veya sistem farklı çalıştır hesabı](service-fabric-run-script-at-service-startup.md).

Windows tek başına kümede Service Fabric çalıştırırken altında bir hizmeti çalıştırabilirsiniz [Active Directory etki alanı hesapları](service-fabric-run-service-as-ad-user-or-group.md) veya [Grup yönetilen hizmet hesapları](service-fabric-run-service-as-gmsa.md).

## <a name="secure-containers"></a>Güvenli kapsayıcılar
Service Fabric kümesindeki düğümlere bir Windows veya Linux (sürüm 5.7 veya üstü) yüklü bir sertifika erişmek için bir kapsayıcı içindeki hizmetler için bir mekanizma sağlar. Bu PFX sertifika kimlik doğrulaması, uygulama veya hizmet ya da diğer hizmetleri ile güvenli iletişim için kullanılabilir. Daha fazla bilgi için bkz: [bir kapsayıcıya sertifika alma](service-fabric-securing-containers.md).

Ayrıca, Service Fabric Windows kapsayıcıları için de gMSA (Grup yönetilen hizmet hesapları) destekler. Daha fazla bilgi için bkz: [gMSA Windows kapsayıcıları için ayarlama](service-fabric-setup-gmsa-for-windows-containers.md).

## <a name="secure-service-communication"></a>Hizmet iletişimin güvenliğini sağlama
Service Fabric içinde bir hizmet birden çok VM genellikle dağıtılmış bir Service Fabric kümesindeki herhangi bir yerde çalışır. Service Fabric hizmet iletişimleri güvenli hale getirmek için çeşitli seçenekler sağlar.

HTTPS uç noktalarını etkinleştirebilirsiniz, [ASP.NET Core veya Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) web hizmetleri.

Bu nedenle bir uçtan uca güvenli kanal etkinleştirme ters proxy hizmetleri arasında güvenli bağlantı kurabilirsiniz. Ters proxy HTTPS üzerinde dinleme yapılandırıldığında, güvenli hizmetlerine bağlanan desteklenir. Ters proxy yapılandırma hakkında daha fazla bilgi için okuma [ters proxy Azure Service Fabric](service-fabric-reverseproxy.md).  [Güvenli bir hizmete bağlanmak](service-fabric-reverseproxy-configure-secure-communication.md) ters proxy hizmetleri arasında güvenli bağlantı kurmak açıklar.

Güvenilir hizmetler uygulama çerçevesi birkaç önceden oluşturulmuş iletişimi yığınları ve güvenliği geliştirmek için kullanabileceğiniz araçlar sağlar. Hizmet remoting kullanırken güvenlik geliştirmeyi öğrenin (içinde [C#](service-fabric-reliable-services-secure-communication.md) veya [Java](service-fabric-reliable-services-secure-communication-java.md)) veya kullanarak [WCF](service-fabric-reliable-services-secure-communication-wcf.md).

## <a name="encrypt-application-data-at-rest"></a>REST uygulama verilerini şifrele
Her [düğüm türü](service-fabric-cluster-nodetypes.md) bir Service Fabric kümesi Azure'da çalışan tarafından yedeklenen bir [sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz.  Hizmetlerinizin ekli veriler diske veri kaydederseniz, yapabilecekleriniz [bu veri diskleri şifrelemek](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) uygulama verilerinizi korumak için.

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Hizmetin başlatılması sırasında bir kurulum komut dosyası çalıştırma](service-fabric-run-script-at-service-startup.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [Küme güvenliği hakkında bilgi edinin](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png