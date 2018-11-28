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
ms.openlocfilehash: fe6df20d294a3b1802d396085c36a6587dc45730
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249090"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric uygulamasını kümeye dağıtma

Bu öğretici, bir dizinin ikinci bölümüdür. Azure Service Fabric uygulamasının Azure’da yeni kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Grup kümesi oluşturma.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md).
> * Uygulamayı uzak kümeye dağıtma.
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Azure Pipelines kullanarak CI/CD yapılandırın](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017’yi yükleyin](https://www.visualstudio.com/).
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki kodu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

## <a name="publish-to-a-service-fabric-cluster"></a>Service Fabric kümesine yayımlama

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz. [Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir.

Bu öğreticide, Visual Studio kullanarak Voting uygulamasını Service Fabric kümesine dağıtmak için iki seçeneğiniz vardır:

* Deneme (grup) kümesine yayımlama. 
* Aboneliğinizde mevcut bir kümeye yayımlama. [Azure portal](https://portal.azure.com) aracılığıyla, [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) veya [Azure CLI](./scripts/cli-create-cluster.md) betiklerini kullanarak ya da bir [Azure Resource Manager şablonundan](service-fabric-tutorial-create-vnet-and-windows-cluster.md) Service Fabric kümeleri oluşturabilirsiniz.

> [!NOTE]
> Birçok hizmet birbiriyle iletişim kurmak için ters proxy kullanır. Visual Studio'dan oluşturulan kümelerde ve grup kümelerinde ters proxy varsayılan olarak etkindir. Mevcut kümelerden birini kullanıyorsanız, [kümede ters proxy'yi etkinleştirmelisiniz](service-fabric-reverseproxy-setup.md).


### <a name="find-the-voting-web-service-endpoint-for-your-azure-subscription"></a>Azure aboneliğiniz için Voting web hizmeti uç noktasını bulma

Voting uygulamasını kendi Azure aboneliğinize yayımlamak için, ön uç web hizmetinin uç noktasını bulun. Grup kümesi kullanıyorsanız otomatik olarak açılan Voting örneğini kullanarak 8080 numaralı bağlantı noktasına bağlanın. Grup kümesinin yük dengeleyicisinde yapılandırma yapmanız gerekmez.

Ön uç web hizmeti belirli bir bağlantı noktasında dinliyor. Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır. Kümenin Azure yük dengeleyicisinde kural kullanarak uygulama bağlantı noktasının açılması gerekir. Açık bağlantı noktası, gelen trafiği web hizmeti üzerinden gönderir. Bağlantı noktası **VotingWeb/PackageRoot/ServiceManifest.xml** dosyasının **Endpoint** öğesinde bulunur. Örnek olarak 8080 numaralı bağlantı noktası kullanılabilir.

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Azure aboneliğiniz için, Azure'da [PowerShell betiği](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) aracılığıyla yük dengeleme kuralı kullanarak veya [Azure portalda](https://portal.azure.com) bu kümeye ilişkin yük dengeleyici yoluyla bu bağlantı noktasını açın.

### <a name="join-a-party-cluster"></a>Grup kümesine katılma

> [!NOTE]
>  Uygulamayı bir Azure aboneliği içindeki kendi kümenize yayımlıyorsanız, [Visual Studio kullanarak uygulamayı yayımlama](#publish-the-application-by-using-visual-studio) bölümüne geçin. 

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleri olup Service Fabric ekibi tarafından çalıştırılır. Uygulama dağıtma ve platform hakkında bilgi edinme işlemleri herkes tarafından gerçekleştirilebilir. Küme, düğümden düğüme ve istemciden düğüme güvenlik için tek bir otomatik olarak imzalanan sertifika kullanır.

Oturum açın ve [bir Windows kümesine katılın](https://aka.ms/tryservicefabric). PFX sertifikasını bilgisayarınıza indirmek için **PFX** bağlantısını seçin. **Güvenli Grup kümesine bağlanma** bağlantısını seçin ve sertifika parolasını kopyalayın. Aşağıdaki adımlarda sertifika, sertifika parolası ve **Bağlantı uç noktası** değeri kullanılır.

![PFX ve bağlantı uç noktası](./media/service-fabric-quickstart-dotnet/party-cluster-cert.png)

> [!Note]
> Saat başına sınırlı sayıda grup kümesi vardır. Grup kümesi kaydı sırasında hatayla karşılaşırsanız bir süre bekleyip yeniden deneyin. Veya [.NET uygulaması dağıtma](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) öğreticisindeki adımları uygulayarak Azure aboneliğinizde bir Service Fabric kümesi oluşturun ve uygulamayı ona dağıtın. Mevcut bir Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.
>

Windows makinenizde PFX’i **CurrentUser\My** sertifika deposuna yükleyin.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Sonraki adım için parmak izini unutmayın.

> [!Note]
> Varsayılan olarak, web ön uç hizmeti 8080 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırılmıştır. Grup kümesinde 8080 numaralı bağlantı noktası açıktır. Uygulama bağlantı noktasını değiştirmeniz gerekiyorsa, bunu grup kümesinde açık olan bağlantı noktalarından biriyle değiştirin.
>

### <a name="publish-the-application-by-using-visual-studio"></a>Visual Studio kullanarak uygulamayı yayımlama

Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Çözüm Gezgini'nde **Voting**’e sağ tıklayın. **Yayımla**’yı seçin. **Yayımla** iletişim kutusu görüntülenir.

2. Grup kümesi sayfasındaki veya Azure aboneliğinizdeki **Bağlantı Uç Noktası**'nı **Bağlantı Uç Noktası** alanına kopyalayın. `zwin7fh14scd.westus.cloudapp.azure.com:19000` bunun bir örneğidir. **Gelişmiş Bağlantı Parametreleri**'ni seçin.  **FindValue** ve **ServerCertThumbprint** değerlerinin, önceki adımda grup kümesi veya Azure aboneliğinizle eşleşen sertifika için yüklenen sertifikanın parmak iziyle eşleştiğinden emin olun.

    ![Service Fabric uygulamasını yayımlama](./media/service-fabric-quickstart-dotnet/publish-app.png)

    Kümedeki her uygulamanın benzersiz bir adı olmalıdır. Grup kümeleri ortak, paylaşılan bir ortamdır ve mevcut uygulamalardan biriyle çakışma olabilir. Ad çakışması varsa, Visual Studio projesini yeniden adlandırın ve bir kez daha dağıtın.

3. **Yayımla**’yı seçin.

4. Kümedeki Voting uygulamanıza erişmek için bir tarayıcı açın ve küme adresini yazıp sonuna **:8080** ekleyin. Veya başka bir bağlantı noktası yapılandırdıysanız onu girin. `http://zwin7fh14scd.westus.cloudapp.azure.com:8080` bunun bir örneğidir. Artık Azure'da kümede çalıştırılan uygulamayı görüyor olmalısınız. Voting web sayfasında, oylama seçeneklerini ve bu seçeneklerden en az biri için oylama ekleyip silmeyi deneyin.

    ![Service Fabric Voting örneği](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
