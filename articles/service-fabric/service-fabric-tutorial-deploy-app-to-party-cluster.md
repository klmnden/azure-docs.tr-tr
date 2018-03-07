---
title: "Azure Service Fabric uygulamasını bir kümeye dağıtma | Microsoft Docs"
description: "Bu öğreticide, uygulamayı bir Service Fabric kümesine dağıtmayı öğrenirsiniz."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 35ddf77b1e9a9b355ed2cee4731e3c5d87c4a701
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="tutorial-deploy-an-application-to-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric kümesine uygulama dağıtma
Bir serinin ikinci kısmı olan bu öğreticide bir Azure Service Fabric uygulamasının Azure’da çalışan bir kümeye nasıl dağıtılacağı gösterilir.

Bu öğretici serisinin ikinci kısmında şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * Uygulamayı uzak kümeye dağıtma
> * [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak bir uygulamayı uzak bir kümeye dağıtma
> * Service Fabric Explorer kullanarak bir uygulamayı kümeden kaldırma

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

## <a name="set-up-a-party-cluster"></a>Grup Kümesi ayarlama
Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir. Ücretsiz!

Bir Grup Kümesine erişmek için bu siteye göz atın: http://aka.ms/tryservicefabric ve bir kümeye erişmek için yönergeleri izleyin. Bir Grup Kümesine erişmek için bir Facebook veya GitHub hesabı gerekir.

İsterseniz Grup Kümesi yerine kendi kümenizi kullanabilirsiniz.  ASP.NET Core web ön ucu, durum bilgisi olan hizmet arka ucu ile iletişim kurmak için ters ara sunucusu kullanır.  Grup Kümeleri ve yerel geliştirme kümesi varsayılan olarak etkin ters ara sunucuya sahiptir.  Voting örnek uygulamasını kendi kümenize dağıtırsanız, [kümedeki ters ara sunucuyu etkinleştirmeniz](service-fabric-reverseproxy.md#setup-and-configuration) gerekir.

> [!NOTE]
> Grup kümeleri güvenli değildir, bu nedenle uygulamalarınız ve bu kümelere koyduğunuz herhangi bir veri başkalarına görünür olabilir. Başkalarının görmesini istemediğiniz hiçbir şeyi dağıtmayın. Tüm ayrıntılar için Kullanım Koşullarımızı okuduğunuzdan emin olun.

Oturum açın ve [bir Windows kümesine katılın](http://aka.ms/tryservicefabric). **PFX** bağlantısına tıklayarak PFX sertifikasını bilgisayarınıza indirin. Sonraki adımlarda sertifika ve **Bağlantı uç noktası** değeri kullanılır.

![PFX ve bağlantı uç noktası](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

Bir Windows bilgisayarda PFX’i *CurrentUser\My* sertifika deposuna yükleyin.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:
\CurrentUser\My


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```


## <a name="deploy-the-app-to-the-azure"></a>Uygulamayı Azure’a dağıtma
Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan Grup Kümesine dağıtabilirsiniz.

1. Çözüm Gezgini'nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. 

    ![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-containers/publish-app.png)

2. Grup kümesi sayfasındaki **Bağlantı Uç Noktası**'nı **Bağlantı Uç Noktası** alanına kopyalayın. Örneğin, `zwin7fh14scd.westus.cloudapp.azure.com:19000`. **Gelişmiş Bağlantı Parametreleri**’ne tıklayıp ve aşağıdaki bilgileri doldurun.  *FindValue* ve *ServerCertThumbprint* değerleri önceki adımda yüklenen sertifikanın parmak iziyle eşleşmelidir. **Yayımla**’ta tıklayın. 

    Yayımlama tamamlandıktan sonra, bir tarayıcı aracılığıyla uygulamaya bir istek gönderebilirsiniz.

3. Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bağlantı noktası bilgileri olmadan bağlantı uç noktası - örneğin, win1kw5649s.westus.cloudapp.azure.com).

    Uygulamayı yerel olarak çalıştırırken gördüğünüz sonucun aynısını görmeniz gerekir.

    ![Kümeden API Yanıtı](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer kullanarak kümeden uygulamayı kaldırma
Service Fabric Explorer, bir Service Fabric kümesinde bulunan uygulamaları keşfedip yöneten bir grafik kullanıcı arabirimidir.

Uygulamayı Grup Kümesinden kaldırmak için:

1. Grup Kümesi kaydolma sayfasında sunulan bağlantıyı kullanarak Service Fabric Explorer’a gidin. Örneğin, https://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. Service Fabric Explorer’ın sol tarafındaki ağaç görünümünde bulunan **fabric:/Voting** düğümüne gidin.

3. Sağdaki **Essentials** bölmesinde bulunan **Eylem** düğmesine tıklayıp **Uygulamayı Sil** seçeneğini belirleyin. Uygulama örneğinin silinmesini onaylayın. Bu işlem, kümede çalışan uygulama örneğimizi kaldırır.

![Service Fabric Explorer'da Uygulama Silme](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer kullanarak kümeden uygulama türünü kaldırma
Uygulamalar, Service Fabric kümesinde uygulama türleri olarak dağıtılır. Bu sayede kümede çalışan uygulamaların birden fazla örneğine ve sürümüne sahip olursunuz. Uygulamamızın çalışan örneğini kaldırdıktan sonra, dağıtım temizleme işlemini tamamlamak için türü de kaldırabiliriz.

Service Fabric’teki uygulama modeli hakkında daha fazla bilgi için bkz. [Service Fabric’te uygulama modelleme](service-fabric-application-model.md).

1. Ağaç görünümündeki **VotingType** düğümüne gidin.

2. Sağdaki **Essentials** bölmesinde bulunan **Eylem** düğmesine tıklayıp **Türün Sağlamasını Kaldır** seçeneğini belirleyin. Uygulama türünün sağlamasını kaldırma işlemini onaylayın.

![Service Fabric Explorer’da Uygulama Türünün Sağlamasını Kaldırma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Bu, öğreticiyi sonlandırır.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio kullanarak bir uygulamayı uzak bir kümeye dağıtma
> * Service Fabric Explorer kullanarak bir uygulamayı kümeden kaldırma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Visual Studio Team Services kullanarak sürekli tümleştirmeyi ayarlama](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
