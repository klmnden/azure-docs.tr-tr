---
title: Azure Service Fabric uygulamasını bir kümeye dağıtma | Microsoft Docs
description: Uygulamayı Visual Studio'dan bir kümeye dağıtmayı öğrenin.
services: service-fabric
documentationcenter: .net
-author: rwike77
-manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: f75a05e965a025a3041036679ac06cfe4f1ec8d7
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="tutorial-deploy-an-application-to-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric kümesine uygulama dağıtma
Bir serinin ikinci kısmı olan bu öğreticide bir Azure Service Fabric uygulamasının doğrudan Visual Studio'dan Azure’da yeni bir kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio'dan küme oluşturma
> * Visual Studio kullanarak bir uygulamayı uzak bir kümeye dağıtma


Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * Uygulamayı uzak kümeye dağıtma
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
- **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017’yi yükleyin](https://www.visualstudio.com/).
- [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme
[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma
Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz. [Service Fabric kümesi](/service-fabric/service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir

Dağıtım için Visual Studio içinde iki seçeneğiniz vardır:
- Azure’da Visual Studio'dan küme oluşturma. Bu seçenek doğrudan Visual Studio'dan tercih ettiğiniz yapılandırmalarla güvenli bir küme oluşturmanızı sağlar. Bu tür kümeler test senaryoları için idealdir, çünkü kümeyi oluşturabilir ve ardından Visual Studio'dan doğrudan bu kümenin içine yayımlayabilirsiniz.
- Aboneliğinizde mevcut bir kümeye yayımlama.  [Azure portalı](https://portal.azure.com) aracılığıyla, [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) veya [Azure CLI](./scripts/cli-create-cluster.md) betiklerini kullanarak ya da bir [Azure Resource Manager şablonundan](service-fabric-tutorial-create-vnet-and-windows-cluster.md) Service Fabric kümeleri oluşturabilirsiniz.

Bu öğreticide Visual Studio’dan bir küme oluşturulur. Zaten dağıttığınız bir küme varsa bağlantı uç noktanızı kopyalayıp yapıştırabilir veya aboneliğinizden kümeyi seçebilirsiniz.
> [!NOTE]
> Birçok hizmet birbiriyle iletişim kurmak için ters proxy kullanır. Visual Studio'dan oluşturulan kümelerde ve grup kümelerinde ters proxy varsayılan olarak etkindir.  Mevcut kümelerden birini kullanıyorsanız, [kümede ters proxy'yi etkinleştirmelisiniz](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="find-the-votingweb-service-endpoint"></a>VotingWeb hizmet uç noktasını bulun
İlk olarak ön uç web hizmetinin uç noktasını bulun.  Ön uç web hizmeti belirli bir bağlantı noktasında dinliyor.  Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır.  Gelen trafiğin web hizmetine ulaşabilmesi için Azure yük dengeleyicide uygulama bağlantı noktası açık olmalıdır.  Bağlantı noktası (örneğin, 8080), **Uç Nokta** öğesindeki *VotingWeb/PackageRoot/ServiceManifest.xml* dosyasında bulunabilir:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Sonraki adımda, **Küme oluştur** iletişim kutusunun **Gelişmiş** sekmesinde bu bağlantı noktasını belirtin.  Uygulamayı mevcut bir kümeye dağıtıyorsanız, Azure yük dengeleyicide bu bağlantı noktasını bir [PowerShell betiği](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) kullanarak veya [Azure portalından](https://portal.azure.com) açabilirsiniz.

### <a name="create-a-cluster-in-azure-through-visual-studio"></a>Azure’da Visual Studio aracılığıyla küme oluşturma
Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve **Yayımla**’yı seçin.

Aboneliklerinize erişebilmek için Azure hesabınızı kullanarak oturum açın. Grup kümesi kullanıyorsanız bu adım isteğe bağlıdır.

**Bağlantı Uç Noktası** açılan listesini seçin ve **<Create New Cluster...>** seçeneğini belirtin.
    
![Yayımla İletişim Kutusu](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)
    
**Küme oluştur** iletişim kutusunda aşağıdaki ayarları değiştirin:

1. **Küme Adı** alanında kümenizin adını, ayrıca kullanmak istediğiniz aboneliği ve konumu belirtin.
2. İsteğe bağlı: Düğüm sayısını değiştirebilirsiniz. Varsayılan olarak üç düğümünüz vardır; bu, Service Fabric senaryolarını test etmek için gereken en düşük sayıdır.
3. **Sertifika** sekmesini seçin. Bu sekmede, kümenizin sertifikasını güvenlik altına almak için kullanılacak bir parola yazın. Bu sertifika, kümenizin güvenliğine yardımcı olur. Ayrıca sertifikayı kaydetmek istediğiniz yolu da değiştirebilirsiniz. Visual Studio sertifikayı sizin için içeri aktarabilir, çünkü uygulamayı kümeye yayımlarken bu gerekli bir adımdır.
4. **VM Ayrıntısı** sekmesini seçin. Kümeyi oluşturman Sanal Makineler (VM) için kullanmak istediğiniz parolayı belirtin. Kullanıcı adı ve parola, VM'lere uzaktan bağlanmak için kullanılabilir. Ayrıca VM makine boyutu da seçmelisiniz ve gerekirse VM görüntüsü değiştirebilirsiniz.
5. **Gelişmiş** sekmesinde kümeyle birlikte oluşturulan Azure yük dengeleyicide açılmasını istediğiniz bağlantı noktaları listesinde değişiklik yapabilirsiniz.  Önceki adımlardan birinde keşfettiğiniz VotingWeb hizmet uç noktasını ekleyin. Ayrıca uygulama günlük dosyalarını yönlendirmek için mevcut bir Application Insights anahtarını ekleyebilirsiniz.
6. Ayarları değiştirmeyi bitirdiğinizde **Oluştur** düğmesini seçin. Oluşturma işleminin tamamlanması birkaç dakika sürer; çıkış penceresinde kümenin ne zaman tam olarak oluşturulduğu gösterilir.

![Küme Oluştur İletişim Kutusu](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

## <a name="deploy-the-sample-application"></a>Örnek uygulamayı dağıtma
Kullanmak istediğiniz küme hazır olduğunda, uygulama projesine sağ tıklayın ve **Yayımla**'yı seçin.

Yayımlama tamamlandığında, bir tarayıcı aracılığıyla uygulamaya istek gönderebilirsiniz.

Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bağlantı noktası bilgileri olmadan bağlantı uç noktası - örneğin, win1kw5649s.westus.cloudapp.azure.com).

Uygulamayı yerel olarak çalıştırırken gördüğünüz sonucun aynısını görmeniz gerekir.

![Kümeden API Yanıtı](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio'dan küme oluşturma
> * Visual Studio kullanarak bir uygulamayı uzak bir kümeye dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
