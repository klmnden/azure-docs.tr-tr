---
title: Güvenli Azure Service Fabric küme bağlantıları yapılandırma | Microsoft Docs
description: Visual Studio Azure Service Fabric kümesi tarafından desteklenen güvenli bağlantıları yapılandırmak için kullanmayı öğrenin.
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
ms.openlocfilehash: 8d76a2144234591792359ed8dd4a0779e6a2fc5c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60628311"
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Visual Studio'dan bir Service Fabric kümesine güvenli bağlantı yapılandırma
Güvenli bir şekilde yapılandırılmış erişim denetimi ilkeleri ile bir Azure Service Fabric kümesine erişmek için Visual Studio kullanmayı öğrenirsiniz.

## <a name="cluster-connection-types"></a>Küme bağlantı türleri
İki tür bağlantı, Azure Service Fabric kümesi tarafından desteklenir: **güvenli olmayan** bağlantıları ve **x509 sertifika tabanlı** güvenli bağlantılar. (Service Fabric kümeleri şirket içinde barındırılan için **Windows** ve **dSTS** kimlik doğrulamaları de desteklenir.) Küme bağlantı türü küme oluşturulduğunda yapılandırmanız gerekmez. Bağlantı türü, oluşturulduktan sonra değiştirilemez.

Visual Studio Service Fabric araçları, yayımlama için bir kümeye bağlanmak için tüm kimlik doğrulama türlerini destekler. Bkz: [Azure portalından bir Service Fabric kümesi ayarlama](service-fabric-cluster-creation-via-portal.md) güvenli bir Service Fabric kümesi ayarlama hakkında yönergeler için.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Küme bağlantıları yapılandırma yayımlama profilleri
Bir Service Fabric projesi Visual Studio'dan yayımlama kullanırsanız **Service Fabric uygulamasını Yayımla** iletişim kutusu, bir Azure Service Fabric kümesi seçin. Altında **bağlantı uç noktası**, aboneliğiniz kapsamındaki mevcut bir kümeyi seçin.

![** Yayımlama Service Fabric uygulama ** iletişim kutusunda, bir Service Fabric bağlantısı yapılandırmak için kullanılır.][publishdialog]

**Service Fabric uygulamasını Yayımla** iletişim kutusu, küme bağlantısını otomatik olarak doğrular. İstenirse Azure hesabınızda oturum açın. Doğrulama testlerini geçerse, sisteminizde güvenli bir kümeye bağlanmak için yüklü doğru sertifikalara sahip veya güvenli olmayan kümenizi anlamına gelir. Ağ sorunları veya sisteminizde güvenli bir kümeye bağlanmak için doğru yapılandırılmış olmaması doğrulama hataları neden olabilir.

![** Yayımlama Service Fabric uygulaması iletişim kutusu doğrular mevcut bir ** doğru yapılandırılmış Service Fabric küme bağlantısı.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanmak için
1. Hedef küme güvendiği istemci sertifikalarını birini erişebildiğinden emin olun. Sertifika genellikle bir kişisel bilgi değişimi (.pfx) dosyası olarak paylaşılır. Bkz: [Azure portalından bir Service Fabric kümesi ayarlama](service-fabric-cluster-creation-via-portal.md) için bir istemci erişim sunucusu nasıl yapılandırılır.
2. Güvenilir sertifika yükleyin. Bunu yapmak için .pfx dosyasını çift tıklatın veya sertifikaları almak için Import-PfxCertificate PowerShell betiğini kullanın. Sertifikayı yükleme **Cert: \LocalMachine\My**. Sertifika içeri aktarılırken, tüm varsayılan ayarları kabul etmek için Tamam var.
3. Seçin **Yayımla...**  açmak için projenin kısayol menüsünde komutunu **Azure uygulamasını Yayımla** iletişim kutusu ve hedef kümeyi seçin. Araç otomatik olarak bağlantı giderir ve güvenli bağlantı parametreleri yayımlama profilinde kaydeder.
4. İsteğe bağlı: Güvenli kümeye bağlantı belirtmek için yayımlama profili düzenleyebilirsiniz.
   
   Sertifika bilgileri belirtmek için sertifika deposunun adını not etmeyi unutmayın yayımlama profili XML dosyasını el ile düzenlediğiniz olduğundan, konum ve sertifika parmak izini saklar. Bu değerleri sertifika deposu için adı ve depolama konumu sağlamanız gerekir. Bkz: [nasıl yapılır: Bir sertifikanın parmak izini alma](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) daha fazla bilgi için.
   
   Kullanabileceğiniz *ClusterConnectionParameters* Service Fabric kümeye bağlanırken kullanmak için PowerShell parametreleri belirtmek için parametreleri. Herhangi biri Connect-ServiceFabricCluster cmdlet tarafından kabul edilen geçerli parametrelerdir. Bkz: [Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster) kullanılabilir parametrelerin listesi.
   
   Uzak bir kümeye yayınlıyorsanız, o belirli bir küme için gerekli parametreleri belirtin gerekir. Güvenli olmayan bir kümeye bağlanma örneği verilmiştir:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   İşte bir örnek için x x509 bağlamak için sertifika tabanlı güvenli küme:
   
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
5. Diğer gerekli tüm ayarları, yükseltme parametreleri ve uygulama parametre dosyası konumu gibi düzenleyin ve ardından uygulamanızı yayımlayın **Service Fabric uygulamasını Yayımla** Visual Studio'da iletişim kutusu.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kümeleri erişme hakkında daha fazla bilgi için bkz. [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
