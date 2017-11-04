---
title: "Visual Studio ile uzak bir küme için bir uygulama yayımlama | Microsoft Docs"
description: "Visual Studio kullanarak bir uzak service fabric kümesi için bir uygulama yayımlamak öğrenin."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: c440c520d84fc503ff9e705555449e92555d4721
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Dağıtma ve Visual Studio kullanarak uygulamaları kaldırma
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Visual Studio için Azure Service Fabric uzantısı bir Service Fabric kümesi için bir uygulama yayımlamak için kolay, yinelenebilir ve komut dosyalı bir yol sağlar.

## <a name="the-artifacts-required-for-publishing"></a>Yayımlama için gerekli yapıları
### <a name="deploy-fabricapplicationps1"></a>Dağıtma FabricApplication.ps1
Bu bir yayımlama profili yolu yayımlama Service Fabric uygulamaları için parametre olarak kullanan bir PowerShell komut dosyasıdır. Bu komut dosyası uygulamanızın parçası olduğundan, uygulamanız için gerekli olarak değiştirmek Hoş Geldiniz.

### <a name="publish-profiles"></a>Yayımlama profilleri
Adlı bir klasör Service Fabric uygulaması proje **PublishProfiles** gibi bir uygulama yayımlamak için gerekli bilgileri depolamak XML dosyası içeriyor:

* Service Fabric kümesi bağlantı parametreleri
* Bir uygulama parametre dosyasının yolu
* Yükseltme Ayarları

Varsayılan olarak, uygulamanızın üç içerecektir yayımlama profilleri: Local.1Node.xml, Local.5Node.xml ve Cloud.xml. Kopyalama ve yapıştırma varsayılan dosyalarından biri, daha fazla profil ekleyebilirsiniz.

### <a name="application-parameter-files"></a>Uygulama parametre dosyaları
Adlı bir klasör Service Fabric uygulaması proje **ApplicationParameters** kullanıcı tarafından belirtilen uygulama bildirim parametre değerleri için XML dosyası içeriyor. Dağıtım ayarları için farklı değerler kullanabilmesi için uygulama bildirim dosyaları parametreli olabilir. Uygulamanızı kümesini parametreleştirme hakkında daha fazla bilgi için bkz: [Service Fabric birden çok ortamlarında yönetme](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Aktör Hizmetleri için ilk önce dosyayı bir düzenleyicide veya Yayımla iletişim kutusu üzerinden düzenlemeye çalışılırken proje oluşturması gerekir. Bu durum, bildirim dosyaları parçası derleme sırasında oluşturulur çünkü.

## <a name="to-publish-an-application-using-the-publish-service-fabric-application-dialog-box"></a>Service Fabric uygulaması yayımlama iletişim kutusunu kullanarak uygulama yayımlama
Aşağıdaki adımları kullanarak uygulama yayımlama göstermek **Service Fabric uygulaması yayımlama** Visual Studio Service Fabric araçları tarafından sağlanan iletişim kutusu.

1. Service Fabric uygulaması proje kısayol menüsünden seçin **Yayımla...** görüntülemek için **Service Fabric uygulaması yayımlama** iletişim kutusu.
   
    ![** Yayımlama Service Fabric uygulaması ** iletişim kutusu][0]
   
    Seçilen dosyanın **hedef profil** açılır liste kutusunda nerede olduğundan tüm ayarlar dışında **bildirim sürümleri**, kaydedilir. Varolan bir profile yeniden veya seçerek yeni bir tane oluşturun **< profillerini yönetme... >** içinde **hedef profil** açılır liste kutusu. Bir yayımlama profili seçtiğinizde, iletişim kutusunun karşılık gelen alanlara içeriğinin görünür. Herhangi bir zamanda yaptığınız değişiklikleri kaydetmek üzere seçim yapın **profili Kaydet** bağlantı.    
2. İçinde **bağlantı uç noktasının** bölümünde, bir yerel veya uzak Service Fabric kümenin yayımlama uç noktası belirtin. Eklemek veya bağlantı uç noktasının değiştirmek için tıklayın **bağlantı uç noktasının** açılır liste. Liste kullanılabilir Service Fabric kümesi yayımlayabilmeniz için bağlantı uç Azure Abonelikleriniz göre gösterir. Zaten Visual Studio'ya günlüğe kaydedilir, bunu yapmak için istenir olduğunu unutmayın.
   
    Kullanılabilir abonelikler ve kümeleri kümesinden seçmek için küme seçimi iletişim kutusunu kullanın.
   
    ![** Seçin Service Fabric kümesi ** iletişim kutusu][1]
   
   > [!NOTE]
   > Rastgele bir uç nokta (örneğin, bir taraf kümesi) yayımlamak istiyorsanız, bkz: **bir rastgele küme uç noktası yayımlama** bölümüne bakın.
   > 
   > 
   
    Bir uç nokta seçildikten sonra Visual Studio seçili Service Fabric kümesi bağlantıyı doğrular. Küme güvenli değilse, Visual Studio için hemen bağlanabilir. Ancak, küme güvenli ise, devam etmeden önce yerel bilgisayarınızda bir sertifika yüklemeniz gerekir. Bkz: [güvenli bağlantılarını yapılandırma](service-fabric-visualstudio-configure-secure-connections.md) daha fazla bilgi için. İşiniz bittiğinde seçin **Tamam** düğmesi. Seçili küme görünür **Service Fabric uygulaması yayımlama** iletişim kutusu.
3. İçinde **uygulama parametre dosyası** açılır liste kutusunda, bir uygulama parametre dosyasına gidin. Bir uygulama parametre dosyası parametrelerinin değerleri kullanıcı tarafından belirtilen uygulama bildirim dosyasında tutar. Eklemek veya bir parametre değiştirmek istediğiniz **Düzenle** düğmesi. Girin veya parametrenin değeri değiştirin **parametreleri** kılavuz. İşiniz bittiğinde seçin **kaydetmek** düğmesi.
   
    ![** Düzenle parametreleri ** iletişim kutusu][2]
4. Kullanım **uygulama yükseltme** Bu eylem yayımlama olup olmadığını belirtmek için onay kutusunu olduğu yükseltme. Yükseltme yayımlama eylemleri normal farklı eylemler yayımlayın. Bkz: [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md) farklar listesi. Yükseltme ayarlarını yapılandırmak için tercih **yükseltme ayarlarını yapılandır** bağlantı. Yükseltme parametre Düzenleyicisi görüntülenir. Bkz: [Service Fabric uygulaması yükseltmesini yapılandırmak](service-fabric-visualstudio-configure-upgrade.md) yükseltme parametreleri hakkında daha fazla bilgi edinmek için.
5. Seçin **bildirim sürümleri...** görüntülemek için düğmesini **Düzenle sürümleri** iletişim kutusu. Uygulama ve hizmet sürümleri gerçekleşmesi için yükseltme için güncelleştirmeniz gerekir. Bkz: [Service Fabric uygulama yükseltme öğretici](service-fabric-application-upgrade-tutorial.md) uygulama ve hizmet bildirimi sürümlerine yükseltme işlemini nasıl etkiler öğrenin.
   
    ![** Düzenle sürümleri ** iletişim kutusu][3]
   
    1.0.0 veya sayısal değerleri gibi anlamsal sürüm 1.0.0.0 biçiminde kullanıyorsanız uygulama ve hizmet sürümleri seçin **otomatik olarak uygulama ve hizmet sürümleri güncelleştirme** seçeneği. Ne zaman, bu seçeneği, hizmet ve kod, yapılandırma veya veri Paket sürümü her uygulama sürüm numaralarını otomatik olarak güncelleştirilir güncelleştirilir. Sürümleri el ile düzenlemek tercih ederseniz, bu özellik devre dışı bırakmak için onay kutusundaki işareti kaldırın.
   
   > [!NOTE]
   > Aktör projesinde görünmesi tüm paket girişleri için önce Service Manifest dosyalarında girişleri oluşturmak için projeyi oluşturun.
   > 
   > 
6. Bitirdiğinizde tüm gerekli ayarları belirtme, seçin **Yayımla** seçili Service Fabric kümesi uygulamanızı yayımlamak için düğmesi. Belirttiğiniz ayarları için yayımlama işlemi uygulanır.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>(Taraf kümeleri dahil) bir rastgele küme uç noktası yayımlama
Visual Studio deneyimi yayımlama yayımlama, Azure aboneliklerinize biriyle ilişkili uzak kümeleri için optimize edilmiştir. Ancak, yayımlama profili XML doğrudan düzenleyerek rasgele Uç noktalara (örneğin, Service Fabric taraf kümeler) yayımlamak için mümkündür. Yukarıda açıklandığı gibi üç yayımlama profillerini varsayılan olarak--sağlanan**Local.1Node.xml**, **Local.5Node.xml**, ve **Cloud.xml**--ancak oluşturmak Hoş Geldiniz farklı ortamlar için ek profiller. Örneğin, belki de adlı taraf kümelerine yayımlama için bir profil oluşturmak isteyebilirsiniz **Party.xml**.

Güvenli olmayan bir kümeye bağlanılıyorsa, gerekli olan tek şey küme bağlantı uç noktasının gibi `partycluster1.eastus.cloudapp.azure.com:19000`. Bu durumda, yayımlama profilindeki bağlantı uç noktasının şunun gibi görünür:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Güvenli bir kümeye bağlanılıyorsa, istemci sertifikası kimlik doğrulaması için kullanılacak yerel deposundan ayrıntılarını sağlamanız gerekir. Daha fazla ayrıntı için bkz: [Service Fabric kümesi için güvenli bağlantılarını yapılandırma](service-fabric-visualstudio-configure-secure-connections.md).

  Yayımlama profilinizi ayarladıktan sonra aşağıda gösterildiği gibi bunu Yayımla iletişim kutusunda başvurabilirsiniz.

  ![Yeni Yayımlama profili Yayımla iletişim kutusu][4]

  Bu durumda, yeni yayımlama profili varsayılan uygulama parametreleri dosyalarını birine gösterdiğini unutmayın. Bu, aynı uygulama yapılandırması ortamlarında sayıya yayımlamak isterseniz uygundur. Bunun aksine, yayımlamak istediğiniz her bir ortamda farklı yapılandırmalara sahip istediğiniz durumlarda, karşılık gelen bir uygulama parametre dosyası oluşturmak için anlamlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Sürekli Tümleştirme ortamında yayımlama işlemini otomatikleştirmek öğrenmek için bkz: [Service Fabric sürekli tümleştirme kurup](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
