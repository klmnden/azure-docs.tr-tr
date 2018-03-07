---
title: "Linux üzerinde Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Bu hızlı başlangıçta, Azure Service Fabric üzerinde ilk Linux kapsayıcı uygulamanızı oluşturursunuz.  Uygulamanızla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: f568bdf6ce40ff2d437f3566b66f6dd1478a74fa
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="quickstart-deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Hızlı Başlangıç: Azure’da bir Azure Service Fabric Linux kapsayıcı uygulaması dağıtma
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıçta Linux kapsayıcıları bir Service Fabric kümesine nasıl dağıtacağınız gösterilmektedir. Tamamladığınızda Service Fabric kümesinde çalışan Python web ön ucu ve Redis arka ucundan oluşan bir oy verme uygulamasına sahip olursunuz. 

![quickstartpic][quickstartpic]

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Kapsayıcıları bir Azure Linux Service Fabric kümesine dağıtma
> * Service Fabric'teki kapsayıcıları ölçekleme ve yük devretme

## <a name="prerequisite"></a>Önkoşul
1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

2. Komut satırı arabirimini (CLI) yerel olarak yükleyip kullanmayı seçerseniz Azure CLI 2.0.4 veya sonraki bir sürümünü çalıştırdığınızdan emin olun. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="get-application-package"></a>Uygulama paketini alma
Kapsayıcıları Service Fabric üzerinde dağıtmak için ayrı kapsayıcıları ve uygulamayı açıklayan bildirim dosyası (uygulama tanımı) kümesine ihtiyacınız vardır.

Bulut kabuğunda git kullanarak uygulama tanımının bir kopyasını oluşturun.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```
## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

### <a name="set-up-your-azure-service-fabric-cluster"></a>Azure Service Fabric Kümesi’ni ayarlama
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi oluşturun.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Service Fabric ekibi tarafından çalıştırılan bu kümelere herkes uygulama dağıtarak platform hakkında bilgi alabilir. Bir Grup Kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Güvenli toplu kümede yönetimi işlemleri gerçekleştirmek için Service Fabric Explorer, CLI veya Powershell’i kullanabilirsiniz. Service Fabric Explorer’ı kullanmak için, PFX dosyasını Toplu Küme Web sitesinden indirmeniz ve sertifikayı sertifika deponuza (Windows veya Mac) veya tarayıcının kendisine (Ubuntu) aktarmanız gerekir. Toplu kümeden gelen otomatik olarak imzalanan sertifikaların parolası yoktur. 

Powershell veya CLI ile yönetim işlemleri gerçekleştirmek için PFX (Powershell) veya PEM (CLI) gerekir. PFX dosyasını PEM dosyasına dönüştürmek için lütfen aşağıdaki komutu çalıştırın:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Web ön ucu hizmeti, gelen trafik için 80 numaralı bağlantı noktasını dinlemek üzere yapılandırılmıştır. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. Grup Kümesi kullanıyorsanız bu bağlantı noktası açık durumdadır.
>

### <a name="install-service-fabric-command-line-interface-and-connect-to-your-cluster"></a>Service Fabric Komut Satırı Arabirimi’ni yükleme ve kümenize bağlanma

Azure CLI kullanarak Azure'daki Service Fabric kümesine bağlanın. Uç noktası, kümenizin yönetim uç noktasıdır. Örneğin: `https://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```bash
sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
```

### <a name="deploy-the-service-fabric-application"></a>Service Fabric uygulamasını dağıtma 
Service Fabric kapsayıcı uygulamaları, açıklanan Service Fabric uygulama paketi veya Docker Compose kullanılarak dağıtılabilir. 

#### <a name="deploy-using-service-fabric-application-package"></a>Service Fabric uygulama paketi kullanarak dağıtma
Verilen yükleme betiğini kullanarak oy verme uygulaması tanımını kümeye kopyalayın, uygulama türünü kaydedin ve uygulamanın bir örneğini oluşturun.

```bash
./install.sh
```

#### <a name="deploy-the-application-using-docker-compose"></a>Docker compose kullanarak uygulamayı dağıtma
Aşağıdaki komutla Docker Compose kullanarak Service Fabric kümesinde uygulamayı dağıtın ve yükleyin.
```bash
sfctl compose create --deployment-name TestApp --file-path docker-compose.yml
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
* GitHub'da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-containers) bakın.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
