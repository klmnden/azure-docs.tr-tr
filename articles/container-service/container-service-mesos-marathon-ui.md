---
title: "Marathon UI ile bir Azure Container Service kümesini yönetme | Microsoft Belgeleri"
description: "Marathon web kullanıcı arabirimini kullanarak Azure Kapsayıcı Hizmeti küme hizmetine kapsayıcıları dağıtın."
services: container-service
documentationcenter: 
author: dlepow
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
ms.date: 12/02/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: c472963048e842e795e17df39567a8380d8543a9
ms.openlocfilehash: b076fabd964cfb583bb4049d1373268d0d2762fb


---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Marathon web UI üzerinden bir Azure Container Service DC/OS kümesini yönetme
DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır.

Çerçeveler çok sayıda yaygın iş yükü için kullanılabilir, ancak bu belgede Marathon ile kapsayıcı dağıtımlarını nasıl oluşturacağınız ve ölçeklendireceğiniz açıklanmıştır. 


## <a name="prerequisites"></a>Ön koşullar
Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesi dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>DC/OS kullanıcı arabirimini keşfetme
Secure Shell (SSH) tüneli [oluşturarak](container-service-connect.md) http://localhost/ adresine gidin. Bu, DC/OS web kullanıcı arabirimini yükler ve kullanılan kaynaklar, etkin aracılar ve çalışan hizmetler gibi, küme hakkında bilgileri gösterir.

![DC/OS Kullanıcı Arabirimi](./media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Marathon kullanıcı arabirimini keşfetme
Marathon kullanıcı arabirimini görmek için http://localhost/Marathon adresine gidin. Bu ekranda, Azure Kapsayıcı Hizmeti DC/OS kümesinde yeni kapsayıcı veya başka bir uygulama başlatabilirsiniz. Kapsayıcıları ve uygulamaları çalıştırma hakkında bilgileri de görebilirsiniz.  

![Marathon kullanıcı arabirimi](./media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Docker biçimli kapsayıcı dağıtma
Yeni kapsayıcıyı Marathon kullanarak dağıtmak için **Uygulama Oluştur**'a tıklayın ve form sekmelerine aşağıdaki bilgileri girin:

| Alan | Değer |
| --- | --- |
| Kimlik |nginx |
| Bellek | 32 |
| Görüntü |nginx |
| Ağ |Bağlantı |
| Ana Bilgisayar Bağlantı Noktası |80 |
| Protokol |TCP |

![Yeni Uygulama Kullanıcı Arabirimi --Genel](./media/dcos/dcos4.png)

![Yeni Uygulama Kullanıcı Arabirimi--Docker Kapsayıcısı](./media/dcos/dcos5.png)

![Yeni Uygulama Kullanıcı Arabirimi--Bağlantı Noktaları ve Hizmet Bulma](./media/dcos/dcos6.png)

Kapsayıcı bağlantı noktasını statik olarak aracıdaki bağlantı noktasıyla eşlemek istiyorsanız, JSON Modu’nu kullanmalısınız. Bunu yapmak için, geçişi kullanarak Yeni Uygulama Sihirbazı'nı **JSON Modu**’na geçirin. Ardından, uygulama tanımının `portMappings` bölümünün altına aşağıdaki ayarı girin. Bu örnek, kapsayıcının 80 numaralı bağlantı noktasını DC/OS aracının 80 numaralı bağlantı noktasına bağlar. Bu değişikliği yaptıktan sonra bu sihirbazı JSON modundan çıkarabilirsiniz.

```none
"hostPort": 80,
```

![Yeni Uygulama Kullanıcı Arabirimi--bağlantı noktası 80 örneği](./media/dcos/dcos13.png)

Sistem durumu denetimlerini etkinleştirmek istiyorsanız **Sistem Durumu Denetimleri** sekmesinde bir yol belirleyin.

![Yeni Uygulama UI--durum denetimleri](./media/dcos/dcos_healthcheck.png)

DC/OS kümesi bir grup özel ve ortak aracıyla dağıtılır. Kümenin İnternet’ten uygulamalara erişebilmesi için, uygulamaları ortak aracıya dağıtmanız gerekir. Bunu yapmak için, Yeni Uygulama Sihirbazı'nın **İsteğe Bağlı** sekmesini seçin ve **Kabul Edilen Kaynak Rolleri** için **slave_public** girin.

Ardından **Uygulama Oluştur**'a tıklayın.

![Yeni Uygulama Kullanıcı Arabirimi--ortak aracı ayarı](./media/dcos/dcos14.png)

Marathon ana sayfasına geri döndüğünüzde, kapsayıcının dağıtımın durumunu görebilirsiniz. Başlangıçta **Dağıtılıyor** durumu görüntülenir. Dağıtım başarıyla tamamlandıktan sonra durum **Çalışıyor** olarak değişir.

![Marathon ana sayfası kullanıcı arabirimi--kapsayıcı dağıtım durumu](./media/dcos/dcos7.png)

DC/OS web kullanıcı arabirimine (http://localhost/) geri döndüğünüzde, bir görevin (örneğimizde, Docker biçimli kapsayıcının) DC/OS kümesinde çalıştığını görürsünüz.

![DC/OS web kullanıcı arabirimi--kümede çalışan görev](./media/dcos/dcos8.png)

Görevin üzerinde çalıştığı küme düğümünü görmek için **Düğümler** sekmesine tıklayın.

![DC/OS web kullanıcı arabirimi--görev küme düğümü](./media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Kapsayıcılarınızı ölçeklendirme
Bir kapsayıcı örnek sayısını ölçeklendirmek için Marathon kullanıcı arabirimi kullanabilirsiniz. Bunu yapmak için **Marathon** sayfasına gidin, ölçeklendirmek istediğiniz kapsayıcıyı seçin ve **Uygulamayı Ölçeklendir**'e tıklayın. **Uygulamayı Ölçeklendir** iletişim kutusuna istediğiniz kapsayıcı örneği sayısını girin ve **Uygulamayı Ölçeklendir**'e tıklayın.

![Marathon kullanıcı arabirimi--Uygulamayı Ölçeklendir iletişim kutusu](./media/dcos/dcos10.png)

Ölçeklendirme işlemi tamamlandıktan sonra, aynı görevin DC/OS aracılarına yayılmış birden çok örneğini görürsünüz.

![DC/OS web kullanıcı arabirimi panosu--aracılara yayılmış görev](./media/dcos/dcos11.png)

![DC/OS web kullanıcı arabirimi--düğümler](./media/dcos/dcos12.png)

## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS ve Marathon API’si ile çalışma](container-service-mesos-marathon-rest.md)

* Mesos ile Azure Container Service’e ilişkin ayrıntılar

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 



<!--HONumber=Dec16_HO1-->


