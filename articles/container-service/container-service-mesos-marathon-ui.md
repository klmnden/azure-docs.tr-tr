---
title: "Web Kullanıcı Arabirimi aracılığıyla Azure Container Service kapsayıcı yönetimi | Microsoft Belgeleri"
description: "Marathon web kullanıcı arabirimini kullanarak Azure Kapsayıcı Hizmeti küme hizmetine kapsayıcıları dağıtın."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kapsayıcılar, Mikro hizmetler, Mesos, Azure"
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ae2945b0025da3bc933a1e4a8f10f21fd35cb51c


---
# <a name="container-management-through-the-web-ui"></a>Web kullanıcı arabirimi aracılığıyla kapsayıcı yönetimi
DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır.

Çerçeveler çok sayıda yaygın iş yükü için kullanılabilir, ancak bu belgede Marathon ile kapsayıcı dağıtımlarını nasıl oluşturacağınız ve ölçeklendireceğiniz açıklanmıştır. Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesi dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>DC/OS kullanıcı arabirimini keşfetme
Secure Shell (SSH) tüneli oluşturarak, http://localhost/ adresine gidin. Bu, DC/OS web kullanıcı arabirimini yükler ve kullanılan kaynaklar, etkin aracılar ve çalışan hizmetler gibi, küme hakkında bilgileri gösterir.

![DC/OS Kullanıcı Arabirimi](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Marathon kullanıcı arabirimini keşfetme
Marathon kullanıcı arabirimini görmek için http://localhost/Marathon adresine gidin. Bu ekranda, Azure Kapsayıcı Hizmeti DC/OS kümesinde yeni kapsayıcı veya başka bir uygulama başlatabilirsiniz. Kapsayıcıları ve uygulamaları çalıştırma hakkında bilgileri de görebilirsiniz.  

![Marathon kullanıcı arabirimi](media/dcos/dcos3.png)

## <a name="deploy-a-dockerformatted-container"></a>Docker biçimli kapsayıcı dağıtma
Yeni kapsayıcıyı Marathon kullanarak dağıtmak için, **Uygulama Oluştur** düğmesine tıklayın ve forma aşağıdaki bilgileri girin:

| Alan | Değer |
| --- | --- |
| Kimlik |nginx |
| Görüntü |nginx |
| Ağ |Bağlantı |
| Ana Bilgisayar Bağlantı Noktası |80 |
| Protokol |TCP |

![Yeni Uygulama Kullanıcı Arabirimi --Genel](media/dcos/dcos4.png)

![Yeni Uygulama Kullanıcı Arabirimi--Docker Kapsayıcısı](media/dcos/dcos5.png)

![Yeni Uygulama Kullanıcı Arabirimi--Bağlantı Noktaları ve Hizmet Bulma](media/dcos/dcos6.png)

Kapsayıcı bağlantı noktasını statik olarak aracıdaki bağlantı noktasıyla eşlemek istiyorsanız, JSON Modu’nu kullanmalısınız. Bunu yapmak için, geçişi kullanarak Yeni Uygulama Sihirbazı'nı **JSON Modu**’na geçirin. Ardından, uygulama tanımının `portMappings` bölümünün altı aşağıdakini girin. Bu örnek, kapsayıcının 80 numaralı bağlantı noktasını DC/OS aracının 80 numaralı bağlantı noktasına bağlar. Bu değişikliği yaptıktan sonra bu sihirbazı JSON modundan çıkarabilirsiniz.

```none
"hostPort": 80,
```

![Yeni Uygulama Kullanıcı Arabirimi--bağlantı noktası 80 örneği](media/dcos/dcos13.png)

DC/OS kümesi bir grup özel ve ortak aracıyla dağıtılır. Kümenin İnternet’ten uygulamalara erişebilmesi için, uygulamaları ortak aracıya dağıtmanız gerekir. Bunu yapmak için, Yeni Uygulama Sihirbazı'nın **İsteğe Bağlı** sekmesini seçin ve **Kabul Edilen Kaynak Rolleri** için **slave_public** girin.

![Yeni Uygulama Kullanıcı Arabirimi--ortak aracı ayarı](media/dcos/dcos14.png)

Marathon ana sayfasına geri döndüğünüzde, kapsayıcının dağıtımın durumunu görebilirsiniz.

![Marathon ana sayfası kullanıcı arabirimi--kapsayıcı dağıtım durumu](media/dcos/dcos7.png)

DC/OS web kullanıcı arabirimine (http://localhost/) geri döndüğünüzde, bir görevin (örneğimizde, Docker biçimli kapsayıcının) DC/OS kümesinde çalıştığını görürsünüz.

![DC/OS web kullanıcı arabirimi--kümede çalışan görev](media/dcos/dcos8.png)

Üzerinden görevin çalıştığı küme düğümünü de görebilirsiniz.

![DC/OS web kullanıcı arabirimi--görev küme düğümü](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Kapsayıcılarınızı ölçeklendirme
Bir kapsayıcı örnek sayısını ölçeklendirmek için Marathon kullanıcı arabirimi kullanabilirsiniz. Bunu yapmak için, **Marathon** sayfasına gidin, ölçeklendirmek istediğiniz kapsayıcıyı seçin ve **Ölçeklendir** düğmesine tıklayın. **Uygulamayı Ölçeklendir** iletişim kutusuna istediğiniz kapsayıcı örneği sayısını girin ve **Uygulamayı Ölçeklendir**’i seçin.

![Marathon kullanıcı arabirimi--Uygulamayı Ölçeklendir iletişim kutusu](media/dcos/dcos10.png)

Ölçeklendirme işlemi tamamlandıktan sonra, aynı görevin DC/OS aracılarına yayılmış birden çok örneğini görürsünüz.

![DC/OS web kullanıcı arabirimi panosu--aracılara yayılmış görev](media/dcos/dcos11.png)

![DC/OS web kullanıcı arabirimi--düğümler](media/dcos/dcos12.png)

## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS ve Marathon API’si ile çalışma](container-service-mesos-marathon-rest.md)

Mesos ile Azure Container Service’e ilişkin ayrıntılar

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
> 
> 




<!--HONumber=Nov16_HO2-->


