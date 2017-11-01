---
title: "Linux üzerinde Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Azure Service Fabric üzerinde ilk Linux kapsayıcı uygulamanızı oluşturun.  Uygulamanızla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.openlocfilehash: 7c1ac13d50180909bbe55b01f47721387d1195d7
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Azure'da bir Azure Service Fabric Linux kapsayıcı uygulaması dağıtma
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıçta Linux kapsayıcıları bir Service Fabric kümesine nasıl dağıtacağınız gösterilmektedir. Tamamladığınızda Service Fabric kümesinde çalışan Python web ön ucu ve Redis arka ucundan oluşan bir oy verme uygulamasına sahip olursunuz. 

![quickstartpic][quickstartpic]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Kapsayıcıları bir Azure Linux Service Fabric kümesine dağıtma
> * Service Fabric'teki kapsayıcıları ölçekleme ve yük devretme

## <a name="prerequisite"></a>Önkoşul
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/en-us/free/) oluşturun.
  
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Komut satırı arabirimini (CLI) yerel olarak yükleyip kullanmayı seçerseniz Azure CLI 2.0.4 veya sonraki bir sürümünü çalıştırdığınızdan emin olun. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="get-application-package"></a>Uygulama paketini alma
Kapsayıcıları Service Fabric üzerinde dağıtmak için ayrı kapsayıcıları ve uygulamayı açıklayan bildirim dosyası (uygulama tanımı) kümesine ihtiyacınız vardır.

Bulut kabuğunda git kullanarak uygulama tanımının bir kopyasını oluşturun.

```azurecli-interactive
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/Voting
```

## <a name="deploy-the-containers-to-a-service-fabric-cluster-in-azure"></a>Kapsayıcıları Azure'daki bir Service Fabric kümesine dağıtma
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi veya bir Grup kümesi kullanın.

> [!Note]
> Uygulamanın yerel geliştirme makinenizdeki Service Fabric kümesine değil Azure'daki bir kümeye dağıtılması gerekir. 
>

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Bakımı Service Fabric ekibi tarafından yapılan bu kümelere herkes uygulama dağıtarak platform hakkında bilgi alabilir. Bir Grup kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da ilk Service Fabric kümenizi oluşturma](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Web ön uç hizmeti 80 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırılmıştır. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. Grup kümesi kullanıyorsanız bu bağlantı noktası açık durumdadır.
>

### <a name="deploy-the-application-manifests"></a>Uygulama bildirimlerini dağıtma 
[Service Fabric CLI (sfctl)](service-fabric-cli.md) öğesini CLI ortamınıza yükleyin

```azurecli-interactive
pip3 install --user sfctl 
export PATH=$PATH:~/.local/bin
```
Azure CLI kullanarak Azure'daki Service Fabric kümesine bağlanın. Uç noktası, kümenizin yönetim uç noktasıdır. Örneğin: `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```azurecli-interactive
sfctl cluster select --endpoint http://linh1x87d1d.westus.cloudapp.azure.com:19080
```

Verilen yükleme betiğini kullanarak oy verme uygulaması tanımını kümeye kopyalayın, uygulama türünü kaydedin ve uygulamanın bir örneğini oluşturun.

```azurecli-interactive
./install.sh
```

Bir tarayıcı penceresi açın ve http://\<my-azure-service-fabric-cluster-url>:19080/Explorer - örneğin, `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer` adresini izleyerek Service Fabric Explorer'a gidin. Uygulamalar düğümünü genişlettiğinizde oluşturduğunuz Oy verme uygulama türü ve örneği için bir giriş oluşturulduğunu göreceksiniz.

![Service Fabric Explorer][sfx]

Çalışan kapsayıcıya bağlanın.  Bir web tarayıcısı penceresi açarak kümenizin URL'sine gidin, örneğin: `http://linh1x87d1d.westus.cloudapp.azure.com:80`. Oy verme uygulamasını tarayıcıda görmeniz gerekir.

![quickstartpic][quickstartpic]

## <a name="fail-over-a-container-in-a-cluster"></a>Kümedeki bir kapsayıcıya yük devretme
Service Fabric, kapsayıcı örneklerinizin bir arıza durumunda kümedeki diğer düğümlere otomatik olarak taşınmasını sağlar. Bir düğümü kapsayıcılar için el ile boşaltabilir ve kümedeki diğer düğümlere taşıyabilirsiniz. Hizmetlerinizi ölçeklendirmek için kullanabileceğiniz birden fazla yöntem vardır. Bu örnekte Service Fabric Explorer'ı kullanacağız.

Ön uç kapsayıcısında yük devretmek için aşağıdaki adımları uygulayın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin. Ağaç görünümünde kapsayıcının üzerinde çalıştığı düğümleri gösteren düğüm adına dikkat edin. Örneğin: `_nodetype_4`
3. Ağaç görünümünde **Düğümler** düğümünü genişletin. Kapsayıcıyı çalıştıran düğümün yanındaki üç noktaya tıklayın.
4. İlgili düğümü yeniden başlatmak için **Yeniden Başlat**'ı seçin ve yeniden başlatma eylemini onaylayın. Yeniden başlatma durumunda kapsayıcıdan kümedeki başka bir düğüme yük devretme gerçekleştirilir.

![sfxquickstartshownodetype][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme
Hizmet yükünü karşılamak için bir kümedeki Service Fabric hizmetleri kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz.

Web ön uç hizmetini ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![containersquickstartscale][containersquickstartscale]

  Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![containersquickstartscaledone][containersquickstartscaledone]

    Hizmetin artık iki örneği olduğunu görebilirsiniz. Ağaç görünümünde örneklerin üzerinde çalıştığı düğümleri görebilirsiniz.

Bu basit yönetim görevi sayesinde ön uç hizmetinizde kullanıcı yükünü işleyecek kaynak sayısını iki katına çıkarmış olduk. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="clean-up"></a>Temizleme
Kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın. Bu komutun örneği temizlemesi zaman alacağından 'install'sh' komutu bu betiğin hemen ardından çalıştırılmamalıdır. 

```bash
./uninstall.sh
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:
> [!div class="checklist"]
> * Azure'a bir Linux kapsayıcı uygulaması dağıtma
> * Service Fabric kümesindeki bir kapsayıcıda yük devretme
> * Service Fabric kümesindeki bir kapsayıcıyı ölçeklendirme

* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub'da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-dotnet-containers) bakın.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
