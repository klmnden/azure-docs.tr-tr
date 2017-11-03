---
title: "Azure Service Fabric en iyi güvenlik uygulamaları | Microsoft Docs"
description: "Bu makalede Azure Service Fabric güvenlik için en iyi yöntemler kümesi sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: tomsh
ms.openlocfilehash: 682ad79cc5fe4f08051477b7b90ae80981e5d595
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric en iyi güvenlik uygulamaları
Hızlı, kolay ve düşük maliyetli, Azure'da bir uygulamayı dağıtma. Bulut uygulamanızı üretime dağıtmadan önce uygulamanızda güvenli küme uygulamak için önemli ve önerilen en iyi yöntemler bizim listesini gözden geçirin.

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar. 

En iyi her uygulama için biz açıklamaktadır:

-   Hangi en iyi bir uygulamadır.
-   Neden en iyi uygulama olarak uygulamanız gerekir.
-   En iyi uygulama uygulamaz oluşabileceklerden.
-   Nasıl en iyi uygulama olarak uygulamak bilgi edinebilirsiniz.

En iyi yöntemler aşağıdaki Azure Service Fabric güvenlik öneririz:

-   Azure Resource Manager şablonları ve Service Fabric PowerShell modülü güvenli kümeleri oluşturmak için kullanın.
-   X.509 sertifikaları kullanın.
-   Güvenlik ilkeleri yapılandırın.
-   Reliable Actors güvenlik yapılandırması uygular.
-   SSL için Azure Service Fabric yapılandırın.
-   Ağ yalıtımı ve güvenlik ile Azure Service Fabric kullanın.
-   Azure anahtar kasası için güvenlik yapılandırın.
-   Kullanıcıları rollere atarsınız.


## <a name="best-practices-for-securing-your-clusters"></a>Kümelerinizi güvenliğini sağlamak için en iyi uygulamalar

Her zaman güvenli bir küme kullanın:
-   Küme güvenlik sertifikaları kullanarak uygular.
-   İstemci erişim sağlar (Yönetici ve salt okunur) Azure Active Directory (Azure AD) kullanarak.

Otomatik dağıtımları kullanın:
-   Komut dosyaları oluşturmak, dağıtmak ve gizli anahtarları alma için kullanın.
-   Gizli anahtarları Azure anahtar kasası depolayın ve diğer tüm istemci erişimi için Azure AD kullanın.
-   İnsan erişim gizli anahtarları için kimlik doğrulaması gerektirir.

Ayrıca, aşağıdaki yapılandırma seçeneklerini göz önünde bulundurun:
-   Çevre ağı (sivil bölge, DMZ'ler ve denetimli alt ağ olarak da bilinir) Azure ağ güvenlik grupları (Nsg'ler) kullanarak oluşturun.
-   Küme sanal makineleri (VM'ler) erişim veya Uzak Masaüstü Bağlantısı ile atlama sunucularını kullanarak kümenizi yönetebilirsiniz.

Yetkisiz kullanıcılar bağlanmak, özellikle bir küme üretimde çalışırken engellemek için kümelerinizi güvenli hale getirilmelidir. Güvenli olmayan bir küme oluşturmak mümkün olsa da, küme yönetim uç noktalarının genel internet gösterir, anonim kullanıcılar kümenize bağlanabilir.

Üç [senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) çeşitli teknolojiler kullanılarak küme güvenlik uygulamak için:

-   Düğümü düğümü güvenlik: Bu senaryo VM'ler ve kümedeki bilgisayarların arasındaki iletişimin güvenliğini sağlar. Bu form güvenlik uygulamaları ve Hizmetleri kümedeki Kümeye katılma yetkisi olan bilgisayarları barındırabilir sağlar.
Bu senaryo, Azure üzerinde çalıştığı kümesi ya da Windows üzerinde çalışan tek başına kümeleri kullanabilirsiniz [sertifika güvenlik](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) veya [Windows Güvenliği](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) Windows Server makinelerini için.
-   İstemcisi düğümü güvenlik: Bu senaryo bir Service Fabric istemcisi ve bireysel düğümleri arasındaki iletişimin güvenliğini sağlar.
-   Rol tabanlı erişim denetimi (RBAC): Bu senaryo, ayrı kimlikleri kullanır (Sertifikalar, Azure AD, vb.) küme erişen her yönetici ve kullanıcı istemci rolü için. Kümeyi oluşturduğunuzda rol kimlikleri belirtin.

>[!NOTE]
>**Azure kümeleri için güvenlik öneri:** istemcileri için ve sertifikaları düğümü düğümü güvenlik kimlik doğrulaması için kullanım Azure AD güvenlik.

Bir tek başına Windows kümesi yapılandırmak için bkz: [tek başına Windows kümesi yapılandırmak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Azure Resource Manager şablonları ve Service Fabric PowerShell modülü güvenli bir küme oluşturmak için kullanın.
Azure Resource Manager şablonları kullanarak güvenli bir Service Fabric kümesi oluşturmak adım adım yönergeler için bkz: [bir Service Fabric kümesi oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure Resource Manager şablonu kullanın:
-   VM sanal sabit diskler (VHD) için yönetilen depolama yapılandırmak için şablonu kullanarak kümenizi özelleştirin.
-   Sürücü kaynak grubunuz için kolay bir yapılandırma yönetim şablonu kullanarak ve denetim değiştirir.

Küme yapılandırmasını kodu olarak kabul eder:
-   Dağıtım yapılandırmalarınızı denetlerken kapsamlı olabilir.
-   Doğrudan kaynaklarınızı değiştirmek için örtük komutları kullanmaktan kaçının.

Pek çok görünüşünün [Service Fabric uygulama yaşam döngüsü](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) otomatikleştirilebilir. [Service Fabric PowerShell Modülü](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) dağıtma, yükseltme, kaldırma ve Azure Service Fabric uygulamaları test etmek için ortak görevleri otomatik hale getirir. Yönetilen API'ler ve HTTP API'leri uygulama yönetimi için de kullanılabilir.

## <a name="use-x509-certificates"></a>X.509 sertifikaları kullan
Her zaman kümelerinizi X.509 sertifikaları ya da Windows güvenliği kullanarak güvenli hale getirin. Güvenlik, yalnızca küme oluşturma sırasında yapılandırılır. Küme oluşturulduktan sonra güvenliği etkinleştirmek mümkün değildir.

Belirtmek için bir [küme sertifika](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), değerini **ClusterCredentialType** X509 özelliğine. Dış bağlantılar için bir sunucu sertifikası belirtmek için ayarlayın **ServerCredentialType** X509 özelliğine.

Ayrıca, bu uygulamaları izleyin:
-   Üretim kümeleri için sertifikaları doğru yapılandırılmış bir Windows Server sertifika hizmeti kullanarak oluşturun. Ayrıca bir onaylanmış sertifika yetkilisinden (CA) sertifikaları elde edebilirsiniz.
-   Sertifika MakeCert.exe veya benzer bir araç kullanılarak oluşturulduysa, sertifikayı üretim kümeleri için test veya hiçbir zaman bir geçici kullanın.
-   Test kümelerindeki, ancak üretim kümeleri için otomatik olarak imzalanan bir sertifika kullanın.

Küme güvensiz ise, herkes kümeye anonim olarak bağlanmasına ve yönetim işlemleri. Bu nedenle, her zaman üretim kümelerine X.509 sertifikaları ya da Windows güvenliği kullanarak güvenli hale getirme.

X.509 sertifikaları kullanma hakkında daha fazla bilgi edinmek için [Service Fabric kümesi için ekleme veya kaldırma sertifikaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Güvenlik ilkelerini yapılandırma
Service Fabric, ayrıca uygulamaları tarafından kullanılan kaynakların güvenliğini sağlar. Kaynak dosyaları, dizinleri, ister ve uygulama dağıtıldığında, sertifikaları kullanıcı hesapları altında depolanır. Bu özellik çalışan uygulamaları bile paylaşılan barındırılan bir ortamda birbirinden, daha güvenli hale getirir.

-   Bir Active Directory etki alanı grubu veya kullanıcı kullanın: Active Directory kullanıcı veya grup hesabı için kimlik bilgileri altında hizmetini çalıştırmak. Active Directory şirket içi etki alanı ve Azure Active Directory içinde kullandığınızdan emin olun. Etki alanındaki bir etki alanı kullanıcısı veya grubu kullanarak izinleri verilmiş diğer kaynaklara erişim. Örneğin, dosya paylaşımları gibi kaynakları.

-   HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama: belirtin **SecurityAccessPolicy** uygulamak için özellik bir **RunAs** hizmet bildirimi uç noktası kaynakları bildirir, bir hizmete İlkesi HTTP ile. HTTP uç noktaları için ayrılmış bağlantı hizmetinin altında çalıştığı RunAs kullanıcı hesabı için doğru erişim kontrol listeleri noktalarıdır. İlke ayarlanmamışsa, http.sys hizmet için erişime sahip değil ve istemciden çağrıları hataları alabilirsiniz.

Güvenlik ilkeleri bir Service Fabric kümesi kullanmayı öğrenmek için bkz: [uygulamanız için güvenlik ilkelerini yapılandırmak](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="implement-the-reliable-actors-security-configuration"></a>Reliable Actors güvenlik yapılandırmasını uygulamak
Service Fabric Reliable Actors aktör tasarım deseni uygulamasıdır. Tüm yazılım tasarım deseni gibi belirli bir desene kullanmaya karar yazılım sorunu düzeni uygun üzerinde temel alır.

Genel olarak, aşağıdaki yazılım sorunları veya güvenlik senaryoları için model çözümleri yardımcı olması için aktör tasarım deseni kullanın:
-   Çok sayıda sorunu alanınızı içerir (binlerce veya daha fazla) küçük, bağımsız ve yalıtılmış birim durumu ve mantığı sayısı.
-   Dış bileşenlerinden aktörler kümesi boyunca durumu sorgulama dahil olmak üzere önemli etkileşim gerektirmeyen tek iş parçacıklı nesnelerle çalışıyoruz.
-   Aktör örneklerinizi g/ç işlemleri vererek beklenmeyen gecikme ile arayanlar engelleme.

Service Fabric içinde aktörler Reliable Actors uygulama çerçevesi uygulanır. Bu çerçeve aktör deseni temel alınarak ve üstünde yerleşik [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Yazdığınız her güvenilir aktör hizmeti bir bölümlenmiş durum bilgisi olan güvenilir hizmetidir.

Her aktör .NET türünün bir örneği bir .NET nesnesidir şekilde özdeş bir aktör türünün bir örneği olarak tanımlanır. Örneğin, bir **aktör türü** uygulayan çeşitli düğümlerde küme genelinde dağıtılan birçok aktörler bu tür bir hesap makinesi işlevselliğini sahip olabilir. Her dağıtılmış aktörler benzersiz olarak aktör tanımlayıcı tarafından belirlenir.

[Çoğaltıcı güvenlik yapılandırmalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) çoğaltma sırasında kullanılır ve iletişim kanalının güvenliğini sağlamak için kullanılır. Bu yapılandırma Hizmetleri birbirlerinin çoğaltma trafiğini görmesini engeller ve yüksek oranda kullanılabilir veri güvenli olmasını sağlar. Varsayılan olarak, bir boş güvenlik yapılandırması bölümü çoğaltma güvenlik engeller.
Çoğaltıcı yapılandırmaları aktör durumu sağlayıcısı durumu yüksek oranda güvenilir yapmaktan sorumlu çoğaltıcı yapılandırın.

## <a name="configure-ssl-for-azure-service-fabric"></a>Azure Service Fabric için SSL yapılandırma
Sunucu kimlik doğrulama işlemi [kimliğini doğrulayan](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) bir yönetim istemcisi küme yönetim Uç noktalara. Yönetim istemcisi ardından gerçek kümeye Bahsediyor tanır. Bu sertifika ayrıca sağlar bir [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) HTTPS yönetim API'si ve Service Fabric Explorer HTTPS üzerinden.
Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir sertifika yetkilisinden bir sertifika istediğinde, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Bir uygulama için SSL yapılandırmak için önce bir CA tarafından imzalanmış bir SSL sertifikası edinmeniz gerekir. SSL güvenlik amacıyla sertifikaları veren güvenilen bir üçüncü taraf CA olduğunda. Bir SSL sertifikası yoksa, bir SSL sertifikalarını sattığı bir şirketten edinmeniz gerekir.

Sertifika Azure SSL sertifikaları için aşağıdaki gereksinimleri karşılamalıdır:
-   Sertifika bir özel anahtar içermelidir.

-   Sertifika anahtar değişimi için oluşturulmuş olması gerekir ve bir kişisel bilgi değişimi (.pfx) dosyası için verilebilir.

-   Sertifikanın konu adı bulut hizmetinize erişmek için kullanılan etki alanı adı eşleşmelidir.

    - Bulut hizmetinize erişmek için kullanılacak özel etki alanı adını alın.
    - Hizmetinizin özel etki alanı adıyla eşleşen bir konu adına sahip bir CA'dan bir sertifika isteyin. Örneğin, özel etki alanı adınızı ise __contoso__**.com**, CA'nız sertifikadan konu adı olmalıdır **. contoso.com** veya __www__ **. contoso.com**.

    >[!NOTE]
    >Bir CA için bir SSL sertifikası elde edemiyor __cloudapp__**.net** etki alanı.
    
-   En az 2.048 bit şifreleme sertifikası kullanması gerekir.

HTTP protokolü güvenli olmayan ve gizli dinleme saldırılarına maruz ' dir. HTTP üzerinden aktarılan veriler web sunucusuna veya diğer uç noktalar arasında web tarayıcısından düz metin olarak gönderilir. Saldırganlar kesecek ve HTTP üzerinden gönderilen kredi kartı ayrıntıları ve hesap oturum açma bilgileri gibi hassas verileri görüntüleyebilir. Veri gönderilen ya da bir tarayıcı yoluyla HTTPS üzerinden gönderilen SSL hassas bilgileri şifrelenmiş ve kişiler tarafından ele güvenli olmasını sağlar.

SSL sertifikalarının kullanımı hakkında daha fazla bilgi için bkz: [Azure uygulamaları için SSL yapılandırma](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="use-network-isolation-and-security-with-azure-service-fabric"></a>Ağ yalıtımı ve güvenlik ile Azure Service Fabric kullanın
3 nodetype güvenli küme kullanarak ayarlama [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) örnek olarak. Ağ güvenlik grupları ve şablonu kullanarak gelen ve giden ağ trafiğini denetler.

Şablon bir NSG her sanal makine ölçek kümeleri için sahip ve giden trafiği filtrelemek kümesi denetlemek için kullanılır. Kuralları sistem hizmetlerini ve şablonda belirtilen uygulama bağlantı noktaları için gerekli tüm trafiğine izin vermek için varsayılan olarak yapılandırılır. Bu kuralları gözden geçirin ve uygulamalarınız için yeni kuralları ekleme gibi gereksinimlerinize uyacak şekilde değişiklikleri yapın.

Daha fazla bilgi için bkz: [Azure Service Fabric genel ağ senaryoları](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-azure-key-vault-for-security"></a>Azure anahtar kasası için güvenlik ayarlama
Service Fabric, kimlik doğrulaması ve şifreleme bir küme ve kendi uygulamalarına güvenli hale getirmek için sertifikaları kullanır.

Bir küme güvenli ve uygulama güvenlik özellikleri sağlamak için Service Fabric X.509 sertifikaları kullanır. Azure anahtar Kasası'na kullandığınız [sertifikaları yönetme](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) Azure Service Fabric kümeleri için. Kümeleri oluşturur Azure kaynak sağlayıcısı, bir anahtar kasası sertifikalardan çeker. Azure üzerinde Küme dağıtıldığında sağlayıcı sanal makinelerin sonra sertifikaları yükler.

Arasında sertifika bir ilişki var. [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), Service Fabric kümesi ve sertifikaları kullanan kaynak sağlayıcısı. Küme oluşturulduğunda, sertifika ilişki hakkında bilgi anahtar kasasında depolanır.

Bir anahtar kasasını ayarlamak için iki temel adımlar şunlardır:
1. Özel anahtar kasanız için bir kaynak grubu oluşturun.

    Anahtar kasası, kendi kaynak grubunda put öneririz. Bu eylem diğer kaynak gruplarının, depolama, hesaplama veya kümenizi içeren grubu gibi kaldırılırsa anahtarları ve gizli anahtarları kaybını önlemeye yardımcı olur. Anahtar kasanızı içeren kaynak grubu tarafından kullanıldığı kümesi ile aynı bölgede olması gerekir.

2. Bir anahtar kasası yeni kaynak grubu oluşturun.

    Anahtar kasası dağıtım için etkinleştirilmesi gerekir. İşlem kaynak sağlayıcısı kasadan sertifikaları almak ve bunları VM örneklerinde yükleyin.

Bir anahtar kasasını oluşturup hakkında daha fazla bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama
Kümenizin temsil etmek üzere uygulamalar oluşturduktan sonra kullanıcılarınızın Service Fabric tarafından desteklenen rolleri atayın: salt okunur ve yönetici Azure Portalı'nı kullanarak bu roller atayabilirsiniz.

>[!NOTE]
> Service Fabric rolleri kullanma hakkında daha fazla bilgi için bkz: [Service Fabric istemciler için rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric bağlanan istemciler için iki erişim denetim türlerini destekler bir [Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): Yönetici ve kullanıcı. Küme Yöneticisi, erişim denetimi, belirli küme işlemleri farklı kullanıcı grupları için erişimi sınırlamak için kullanabilirsiniz. Erişim denetimi küme daha güvenli hale getirir.

## <a name="next-steps"></a>Sonraki adımlar
- , Service Fabric kümesi [geliştirme ortamı](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Hakkında bilgi edinin [Service Fabric destek seçenekleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).
