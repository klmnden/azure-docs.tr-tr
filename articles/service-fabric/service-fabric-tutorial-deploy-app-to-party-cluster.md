---
title: Azure’da bir Service Fabric uygulamasını bir kümeye dağıtma | Microsoft Docs
description: Uygulamayı Visual Studio'dan bir kümeye dağıtmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: 601742d82c1bd9a0e691de28ff9c4a09f12b538e
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186388"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric uygulamasını kümeye dağıtma

Serinin ikinci kısmı olan bu öğreticide bir Azure Service Fabric uygulamasının Azure’da yeni kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Grup kümesi oluşturma.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * Uygulamayı uzak kümeye dağıtma
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017’yi yükleyin](https://www.visualstudio.com/).
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="publish-to-a-service-fabric-cluster"></a>Service Fabric kümesine yayımlama

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz. [Service Fabric kümesi](https://docs.microsoft.com/en-gb/azure/service-fabric/service-fabric-deploy-anywhere), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir.

Bu öğreticide, Visual Studio kullanarak Voting uygulamasını Service Fabric kümesine dağıtmak için iki seçeneğiniz vardır:

* Deneme (grup) kümesine yayımlama.
* Aboneliğinizde mevcut bir kümeye yayımlama.  [Azure portalı](https://portal.azure.com) aracılığıyla, [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) veya [Azure CLI](./scripts/cli-create-cluster.md) betiklerini kullanarak ya da bir [Azure Resource Manager şablonundan](service-fabric-tutorial-create-vnet-and-windows-cluster.md) Service Fabric kümeleri oluşturabilirsiniz.

> [!NOTE]
> Birçok hizmet birbiriyle iletişim kurmak için ters proxy kullanır. Visual Studio'dan oluşturulan kümelerde ve grup kümelerinde ters proxy varsayılan olarak etkindir.  Mevcut kümelerden birini kullanıyorsanız, [kümede ters proxy'yi etkinleştirmelisiniz](service-fabric-reverseproxy.md#setup-and-configuration).


### <a name="find-the-votingweb-service-endpoint-for-your-azure-subscription"></a>Azure aboneliğiniz için VotingWeb hizmet uç noktasını bulma

Voting uygulamasını kendi Azure aboneliğinize yayımlayacaksanız, ön uç web hizmetinin uç noktasını bulun. Grup kümesi kullanıyorsanız, Voting örneğinin kullandığı 8080 bağlantı noktası otomatik olarak açılır; grup kümenin yük dengeleyicisinde bunu yapılandırmanız gerekmez.

Ön uç web hizmeti belirli bir bağlantı noktasında dinliyor.  Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır.  Gelen trafiğin web hizmetine ulaşabilmesi için bu kümeyle ilgili Azure Yük dengeleyicide bir kural kullanılarak uygulama bağlantı noktası açılmalıdır.  Bağlantı noktası (örneğin, 8080), **Uç Nokta** öğesindeki *VotingWeb/PackageRoot/ServiceManifest.xml* dosyasında bulunabilir:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Azure aboneliğiniz için, Azure'da [PowerShell betiği](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) aracılığıyla Yük dengeleme kuralı kullanarak veya [Azure portalında](https://portal.azure.com) bu kümeye ilişkin Yük dengeleyici yoluyla bu bağlantı noktasını açın.

### <a name="join-a-party-cluster"></a>Grup kümesine katılma

> [!NOTE]
> Uygulamayı kendi Azure aboneliğiniz içinde kendi kümenize yayımlıyorsanız, sonraki bölümün Visual Studio kullanarak uygulamayı dağıtma başlığına atlayın.

Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir. Küme, düğümden düğüme ve istemciden düğüme güvenlik için tek bir otomatik olarak imzalanan sertifika kullanır.

Oturum açın ve [bir Windows kümesine katılın](http://aka.ms/tryservicefabric). **PFX** bağlantısına tıklayarak PFX sertifikasını bilgisayarınıza indirin. **Güvenli Grup kümesine bağlanma** bağlantısına tıklayın ve sertifika parolasını kopyalayın. Aşağıdaki adımlarda sertifika, sertifika parolası ve **Bağlantı uç noktası** değeri kullanılır.

![PFX ve bağlantı uç noktası](./media/service-fabric-quickstart-dotnet/party-cluster-cert.png)

> [!Note]
> Saat başına sınırlı sayıda Grup kümesi vardır. Bir grup kümesine kaydolmaya çalıştığınızda hata alırsanız bir süre bekleyebilir ve tekrar deneyebilirsiniz veya [.NET uygulaması dağıtma](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) öğreticisindeki adımları izleyerek Azure aboneliğinizde bir Service Fabric kümesi oluşturabilir ve bu kümede uygulamayı dağıtabilirsiniz. Mevcut bir Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.
>

Windows makinenizde PFX’i *CurrentUser\My* sertifika deposuna yükleyin.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Sonraki adım için parmak izini unutmayın.

> [!Note]
> Varsayılan olarak, web ön uç hizmeti 8080 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırılmıştır. Grup Kümesinde bağlantı noktası 8080 açıktır.  Uygulama bağlantı noktasını değiştirmeniz gerekiyorsa, bunu Grup Kümesinde açık olan bağlantı noktalarından biriyle değiştirin.
>

### <a name="publish-the-application-using-visual-studio"></a>Visual Studio kullanarak uygulamayı yayımlama

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Çözüm Gezgini'nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. Yayımla iletişim kutusu görüntülenir.

2. Grup kümesi sayfasındaki veya Azure aboneliğinizdeki **Bağlantı Uç Noktası**'nı **Bağlantı Uç Noktası** alanına kopyalayın. Örneğin, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. **Gelişmiş Bağlantı Parametreleri**’ne tıklayın ve *FindValue* ve *ServerCertThumbprint* değerlerinin, önceki adımda grup kümesi veya Azure aboneliğinizle eşleşen sertifika için yüklenen sertifikanın parmak iziyle eşleştiğinden emin olun.

    ![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-dotnet/publish-app.png)

    Kümedeki her uygulamanın benzersiz bir adı olmalıdır.  Bununla birlikte grup kümeleri ortak, paylaşılan bir ortamdır ve mevcut uygulamalardan biriyle çakışma olabilir.  Ad çakışması varsa, Visual Studio projesini yeniden adlandırın ve bir kez daha dağıtın.

3. **Yayımla**’ta tıklayın.

4. Tarayıcıyı açın ve kümede Voting uygulamanıza gitmek için küme adresini yazıp arkasında ':8080' (veya yapılandırıldıysa başka bir bağlantı noktası) ekleyin; örneğin, `http://zwin7fh14scd.westus.cloudapp.azure.com:8080`. Artık Azure'da kümede çalıştırılan uygulamayı görüyor olmalısınız. Voting web sayfasında, oylama seçeneklerini ve bu seçeneklerden en az biri için oylama ekleyip silmeyi deneyin.

    ![Uygulama ön ucu](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Grup kümesi oluşturma.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
