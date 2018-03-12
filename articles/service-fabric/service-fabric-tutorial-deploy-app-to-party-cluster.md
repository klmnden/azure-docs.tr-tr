---
title: "Azure Service Fabric uygulamasını Visual Studio'dan bir kümeye dağıtma | Microsoft Docs"
description: "Uygulamayı Visual Studio'dan bir kümeye dağıtmayı öğrenin"
services: service-fabric
documentationcenter: .net
-author: mikkelhegn
-manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2018
ms.author: mikkelhegn
ms.custom: mvc
ms.openlocfilehash: 21c991a4e3f9ae19a4ad4a96427fdc1c91c55a1c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
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

## <a name="deploy-the-sample-application"></a>Örnek uygulamayı dağıtma

### <a name="select-a-service-fabric-cluster-to-which-to-publish"></a>Yayımlanacağı Service Fabric kümesini seçme
Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

Dağıtım için iki seçeneğiniz vardır:
- Visual Studio'dan küme oluşturma. Bu seçenek doğrudan Visual Studio'dan tercih ettiğiniz yapılandırmalarla güvenli bir küme oluşturmanızı sağlar. Bu tür kümeler test senaryoları için idealdir, çünkü kümeyi oluşturabilir ve ardından Visual Studio'dan doğrudan bu kümenin içine yayımlayabilirsiniz.
- Aboneliğinizde mevcut bir kümeye yayımlama.

Bu öğreticide, Visual Studio'dan küme oluşturma adımları izlenir. Diğer seçenekler için, bağlantı uç noktanızı kopyalayıp yapıştırabilir veya bunu aboneliğinizden seçebilirsiniz.
> [!NOTE]
> Birçok hizmet birbiriyle iletişim kurmak için ters proxy kullanır. Visual Studio'dan oluşturulan kümelerde ve grup kümelerinde ters proxy varsayılan olarak etkindir.  Mevcut kümelerden birini kullanıyorsanız, [kümede ters proxy'yi etkinleştirmelisiniz](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Uygulamayı Service Fabric kümesine dağıtma

1. Çözüm Gezgini'nde uygulama projesine sağ tıklayın ve **Yayımla**’yı seçin.

2. Aboneliklerinize erişebilmek için Azure hesabınızı kullanarak oturum açın. Grup kümesi kullanıyorsanız bu adım isteğe bağlıdır.

3. **Bağlantı Uç Noktası**'nın açılan listesini seçin ve "<Create New Cluster...>" seçeneğini belirtin.
    
    ![Yayımla İletişim Kutusu](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)
    
4. "Küme oluştur" iletişim kutusunda aşağıdaki ayarları değiştirin:

    1. "Küme Adı" alanında kümenizin adını, ayrıca kullanmak istediğiniz aboneliği ve konumu belirtin.
    2. İsteğe bağlı: Düğüm sayısını değiştirebilirsiniz. Varsayılan olarak üç düğümünüz vardır; bu, Service Fabric senaryolarını test etmek için gereken en düşük sayıdır.
    3. "Sertifika" sekmesini seçin. Bu sekmede, kümenizin sertifikasını güvenlik altına almak için kullanılacak bir parola yazın. Bu sertifika, kümenizin güvenliğine yardımcı olur. Ayrıca sertifikayı kaydetmek istediğiniz yolu da değiştirebilirsiniz. Visual Studio sertifikayı sizin için içeri aktarabilir, çünkü uygulamayı kümeye yayımlarken bu gerekli bir adımdır.
    4. "VM Ayrıntısı" sekmesini seçin. Kümeyi oluşturman Sanal Makineler (VM) için kullanmak istediğiniz parolayı belirtin. Kullanıcı adı ve parola, VM'lere uzaktan bağlanmak için kullanılabilir. Ayrıca VM makine boyutu da seçmelisiniz ve gerekirse VM görüntüsü değiştirebilirsiniz.
    5. İsteğe bağlı: "Gelişmiş" sekmesinde kümeyle birlikte oluşturulacak olan yük dengeleyicide açılmasını istediğiniz bağlantı noktaları listesinde değişiklik yapabilirsiniz. Ayrıca uygulama günlük dosyalarını yönlendirmek için kullanılacak mevcut bir Application Insights anahtarı ekleyebilirsiniz.
    6. Ayarları değiştirmeyi bitirdiğinizde "Oluştur" düğmesini seçin. Oluşturma işleminin tamamlanması birkaç dakika sürer; çıkış penceresinde kümenin ne zaman tam olarak oluşturulduğu gösterilir.
    
    ![Küme Oluştur İletişim Kutusu](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

4. Kullanmak istediğiniz küme hazır olduğunda, uygulama projesine sağ tıklayın ve **Yayımla**'yı seçin.

    Yayımlama tamamlandığında, bir tarayıcı aracılığıyla uygulamaya istek gönderebilirsiniz.

5. Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bağlantı noktası bilgileri olmadan bağlantı uç noktası - örneğin, win1kw5649s.westus.cloudapp.azure.com).

    Uygulamayı yerel olarak çalıştırırken gördüğünüz sonucun aynısını görmeniz gerekir.

    ![Kümeden API Yanıtı](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Visual Studio'dan küme oluşturma
> * Visual Studio kullanarak bir uygulamayı uzak bir kümeye dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Visual Studio Team Services kullanarak sürekli tümleştirmeyi ayarlama](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
