---
title: Bulutta Kubernetes geliştirme alanı oluşturma | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: quickstart
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
ms.openlocfilehash: eebec24702456ec1062a1ac4b3cb9bc6d6580c29
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705081"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-net-core-and-visual-studio"></a>Hızlı Başlangıç: Azure Dev Spaces ile bir Kubernetes geliştirme alanı oluşturma (.NET Core ve Visual Studio)

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
- Visual Studio kullanarak kapsayıcılarda yinelemeli kod geliştirin.
- Bu kümede çalıştırılan kodda hata ayıklaması yapın.

> [!Note]
> Herhangi bir zamanda **kilitlenirseniz** [Sorun giderme](troubleshooting.md) bölümüne başvurun veya bu sayfada bir yorum paylaşın. Daha ayrıntılı bir [öğreticiyi](get-started-netcore-visualstudio.md) de deneyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral veya CanadaEast bölgesinde Kubernetes 1.9.6 veya üzerini çalıştıran, Http Uygulama Yönlendirmesi etkinleştirilmiş bir Kubernetes kümesi.

  ![Http Uygulama Yönlendirmesi'nin etkinleştirildiğinden emin olun.](media/common/Kubernetes-Create-Cluster-3.PNG)

- Web Geliştirme iş yükünün yüklendiği Visual Studio 2017. Yüklü değilse, [buradan](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) indirin.

## <a name="set-up-azure-dev-spaces"></a>Azure Dev Spaces'i ayarlama

[Kubernetes için Visual Studio Araçları](https://aka.ms/get-vsk8stools)'nı yükleyin.

## <a name="connect-to-a-cluster"></a>Kümeye bağlanma

Ardından, Azure Dev Spaces için bir proje oluşturur ve yapılandırırsınız.

### <a name="create-an-aspnet-web-app"></a>ASP.NET web uygulaması oluşturma

Visual Studio 2017’de yeni bir proje oluşturun. Şu anda, projenin bir **ASP.NET Core Web Uygulaması** olması gerekir. Projeyi **webfrontend** olarak adlandırın.

**Web Uygulaması (Model-Görünüm-Denetleyici)** şablonunu seçin ve **.NET Core** ile **ASP.NET Core 2.0**’ı hedeflediğinizden emin olun.

### <a name="enable-dev-spaces-for-an-aks-cluster"></a>AKS kümesi için Azure Dev Spaces'i etkinleştirme

Yeni oluşturduğunuz projede, aşağıda gösterildiği gibi başlatma ayarları açılır listesinden **Azure Dev Spaces** seçeneğini belirleyin.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Daha sonra gösterilen iletişim kutusunda, uygun hesapla oturum açtığınızdan emin olun ve ardından mevcut bir kümeyi seçin.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

**Alan** açılır listesini şimdilik ayarlanmış olduğu `default` değerinde bırakın. Web uygulamasına bir genel uç noktadan erişilebilmesi için **Genel Erişime Açık** onay kutusunu işaretleyin.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Kümeyi seçmek veya oluşturmak için **Tamam**’a tıklayın.

Azure Dev Spaces ile çalışacak şekilde yapılandırılmamış bir küme seçerseniz, yapılandırmak isteyip istemediğinizi soran bir ileti görürsünüz.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

**Tamam**’ı seçin. 

### <a name="look-at-the-files-added-to-project"></a>Projeye eklenen dosyalara bakın
Geliştirme alanının oluşturulmasını beklerken, Azure Dev Spaces kullanmayı seçmeniz sırasında projenize eklenmiş dosyalara bakın.

- `charts` adlı bir klasörün eklenmiş ve bu klasörün içinde uygulamanız için bir [Helm grafiği](https://docs.helm.sh) oluşturulmuştur. Bu dosyalar, uygulamanızı geliştirme alanına dağıtmak için kullanılır.
- `Dockerfile`, uygulamanızı standart Docker biçiminde paketlemek için gereken bilgileri içerir.
- `azds.yaml`, geliştirme alanının ihtiyaç duyduğu geliştirme zamanı yapılandırmasını içerir.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Kubernetes’te bir kapsayıcının hatalarını ayıklama
Geliştirme alanı başarıyla oluşturulduktan sonra uygulamanızda hata ayıklayabilirsiniz. Kodda bir kesme noktası oluşturun. Örneğin, `Message` değişkeninin ayarlandığı `HomeController.cs` dosyasının 20. satırında. Hata ayıklamaya başlamak için **F5**’e tıklayın. 

Visual Studio, uygulamayı derleyip dağıtmak için geliştirme alanıyla iletişim kurar ve sonra web uygulaması çalışır durumdayken bir tarayıcı açar. Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamında çalışıyordur. Localhost adresinin nedeni, Azure Dev Spaces’in AKS’de çalışan kapsayıcıya geçici bir SSH tüneli oluşturmasıdır.

Kesme noktasını tetiklemek için sayfanın üst kısmındaki **Hakkında** bağlantısına tıklayın. Kodun yerel olarak yürütülmesi durumunda olduğu gibi, çağrı yığını, yerel değişkenler, özel durum bilgileri vb. hata ayıklama bilgilerine tam erişiminiz vardır.


## <a name="iteratively-develop-code"></a>Kodu yinelemeli geliştirme

Azure Dev Spaces yalnızca kodu Kubernetes’te çalıştırmaya yönelik değildir; aynı zamanda kod değişikliklerinizin buluttaki bir Kubernetes ortamında uygulandığını hızlıca ve yinelenerek görmenizi sağlar.

### <a name="update-a-content-file"></a>İçerik dosyası güncelleştirme
1. `./Views/Home/Index.cshtml` dosyasını bulun ve HTML dosyasında bir düzenleme yapın. Örneğin, `<h2>Application uses</h2>` olan 70. satırı `<h2>Hello k8s in Azure!</h2>` benzeri bir değerle değiştirin.
1. Dosyayı kaydedin.
1. Tarayıcınıza gidip sayfayı yenileyin. Web sayfasında güncelleştirilmiş HTML’in gösterildiğini görürsünüz.

Ne oldu? HTML ve CSS gibi içerik dosyalarında düzenleme yapılması için bir .NET Core web uygulamasında yeniden derleme yapılması gerekmez; bu nedenle, etkin bir F5 oturumu değiştirilmiş içerik dosyalarını AKS’deki çalışan kapsayıcı ile otomatik olarak eşitler ve böylece içerik düzenlemelerinizi hemen görebilirsiniz.

### <a name="update-a-code-file"></a>Kod dosyasını güncelleştirme
.NET Core uygulamasının güncelleştirilmiş uygulama ikili dosyalarını yeniden derleyip oluşturması gerektiğinden, kod dosyalarının güncelleştirilmesi biraz daha fazla iş gerektirir.

1. Visual Studio'daki hata ayıklayıcısını durdurun.
1. `Controllers/HomeController.cs` adlı kod dosyasını açın ve Hakkında sayfasında gösterilen iletiyi düzenleyin: `ViewData["Message"] = "Your application description page.";`
1. Dosyayı kaydedin.
1. Hata ayıklamaya yeniden başlamak için **F5**'e basın. 

Azure Dev Spaces, her kod düzenlemesi yapıldığında yeni bir kapsayıcı görüntüsünü yeniden derleme ve yeniden dağıtmayı içeren uzun süreli işlem yerine, mevcut kapsayıcı içindeki kodu artımlı bir şekilde yeniden derleyerek daha hızlı bir düzenleme/hata ayıklama döngüsü sağlar.

Tarayıcıda web uygulamasını yenileyin ve Hakkında sayfasına gidin. Özel iletinizin kullanıcı arabiriminde görüntülendiğini görürsünüz.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](team-development-netcore-visualstudio.md)
