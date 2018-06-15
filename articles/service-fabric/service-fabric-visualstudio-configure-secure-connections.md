---
title: Güvenli Azure Service Fabric kümesi bağlantılarını yapılandırma | Microsoft Docs
description: Azure Service Fabric kümesi tarafından desteklenen güvenli bağlantıları yapılandırmak için Visual Studio kullanmayı öğrenin.
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: e2772cc2c59b93c7e523eaa0127dcf4ea0bc589e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208694"
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Güvenli bağlantılar Visual Studio'dan bir Service Fabric kümesi için yapılandırın
Azure Service Fabric kümesi yapılandırılmış erişim denetimi ilkeleri ile güvenli bir şekilde erişmek için Visual Studio kullanmayı öğrenin.

## <a name="cluster-connection-types"></a>Küme bağlantı türleri
İki tür bağlantı Azure Service Fabric kümesi tarafından desteklenir: **güvenli olmayan** bağlantıları ve **x509 sertifika tabanlı** güvenli bağlantılar. (Şirket içi, Service Fabric kümeleri barındırılan için **Windows** ve **dSTS** kimlik doğrulamaları de desteklenir.) Küme oluşturulduğunda küme bağlantı türünü yapılandırmanız gerekir. Bağlantı türü, bir kez oluşturulduktan sonra değiştirilemez.

Visual Studio Service Fabric araçları yayımlama bir kümeye bağlanmak için tüm kimlik doğrulama türlerini destekler. Bkz: [Azure portalından bir Service Fabric kümesi ayarlama](service-fabric-cluster-creation-via-portal.md) güvenli Service Fabric küme ayarlama hakkında yönergeler için.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Küme bağlantılarını yapılandırma yayımlama profilleri
Service Fabric projeyi Visual Studio'dan yayımlamak kullanırsanız **Service Fabric uygulaması yayımlama** iletişim kutusu bir Azure Service Fabric kümesi seçin. Altında **bağlantı uç noktasının**, aboneliğinizdeki mevcut bir kümeyi seçin.

![** Yayımlama Service Fabric uygulaması ** iletişim kutusu, Service Fabric bağlantısını yapılandırmak için kullanılır.][publishdialog]

**Service Fabric uygulaması yayımlama** iletişim kutusu otomatik olarak küme bağlantısı doğrular. İstenirse, Azure hesabınızda oturum açın. Doğrulama başarılı olursa, sisteminizin doğru sertifikaların kümeye güvenli bir şekilde bağlanmak için yüklü olan veya güvenli olmayan kümenizi anlamına gelir. Doğrulama hataları, ağ sorunları veya güvenli bir kümeye bağlanmak için doğru yapılandırılmış sisteminizi olmamasından kaynaklanabilir.

![** Yayımlama Service Fabric uygulama iletişim kutusu doğrular var olan ** doğru yapılandırılmış Service Fabric kümesi bağlantısı.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanmak için
1. Bir hedef küme güvenleri istemci sertifikalarının erişebildiğinden emin olun. Sertifika, genellikle bir kişisel bilgi değişimi (.pfx) dosyası olarak paylaşılır. Bkz: [Azure portalından bir Service Fabric kümesi ayarlama](service-fabric-cluster-creation-via-portal.md) için bir istemci erişimi sunucusu nasıl yapılandırılır.
2. Güvenilir sertifika yükleyin. Bunu yapmak için .pfx dosyasını çift tıklatın veya sertifikaları içeri aktarmak için PowerShell betiğini alma PfxCertificate kullanın. Sertifikayı yükleme **Cert: \LocalMachine\My**. Sertifika içeri aktarırken tüm varsayılan ayarları kabul etmek için bir sorun yoktur.
3. Seçin **Yayımla...**  açmak için projesinin kısayol menüsünde komutu **Azure uygulamasını Yayımla** iletişim kutusu ve hedef kümeyi seçin. Aracın otomatik olarak bağlantı çözümler ve güvenli bağlantı parametreleri yayımlama profilinde kaydeder.
4. İsteğe bağlı: Güvenli küme bağlantısını belirtmek için yayımlama profili düzenleyebilirsiniz.
   
   Sertifika bilgilerini belirtmek için sertifika deposunun adını not etmeyi unutmayın yayımlama profili XML dosyasını el ile düzenlediğiniz olduğundan, konum ve sertifika parmak izi depolar. Bu değerleri sertifika deposu için ad ve depolama konumu sağlamanız gerekir. Bkz: [nasıl yapılır: bir sertifikanın parmak izini alma](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) daha fazla bilgi için.
   
   Kullanabileceğiniz *ClusterConnectionParameters* Service Fabric kümeye bağlanırken kullanmak üzere PowerShell parametreleri belirtmek üzere parametreler. Herhangi biri Connect-ServiceFabricCluster cmdlet tarafından kabul edilen geçerli parametreleridir. Bkz: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) kullanılabilir parametrelerin listesi.
   
   Uzak bir kümeye yayımlıyorsanız belirli bu küme için uygun parametreleri belirtmeniz gerekir. Güvenli olmayan bir kümeye bağlanma bir örnek verilmiştir:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Örneği için bir x509 bağlamak için sertifika tabanlı güvenli küme:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Diğer gerekli tüm ayarları, yükseltme parametreler ve uygulama parametre dosyası konumu gibi düzenleyin ve sonra uygulamanızı yayımlamak **Service Fabric uygulaması yayımlama** Visual Studio'da iletişim kutusu.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kümeleri erişme hakkında daha fazla bilgi için bkz: [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
