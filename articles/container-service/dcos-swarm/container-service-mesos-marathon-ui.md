---
title: "Azure DC/OS kümesi Marathon kullanıcı Arabirimi ile yönetme | Microsoft Docs"
description: "Marathon web kullanıcı arabirimini kullanarak Azure Kapsayıcı Hizmeti küme hizmetine kapsayıcıları dağıtın."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kapsayıcılar, Mikro hizmetler, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: b00088bb005519dc5d533433308c0e3e33c7f433
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Marathon web UI üzerinden bir Azure Container Service DC/OS kümesini yönetme
DC/OS, temel donanımı özetlerken, kümelenmiş iş yüklerini dağıtmak ve ölçeklendirmek için ortam sağlar. DC/OS’nin en üstünde, hesaplama iş yüklerini zamanlamayı ve yürütmeyi yöneten bir çerçeve vardır.

Çerçeveler çok sayıda yaygın iş yükü için kullanılabilir, ancak bu belgede Marathon ile kapsayıcı dağıtımına başlamak açıklar. 


## <a name="prerequisites"></a>Ön koşullar
Bu örneklerin üzerinden geçmeden önce, Azure Kapsayıcı Hizmeti’nde yapılandırılan bir DC/OS kümeniz olması gerekir. Bu kümeye uzaktan bağlantınız olması da gerekir. Bu öğeler hakkında daha fazla bilgi için, aşağıdaki makalelere bakın:

* [Azure Container Service kümesi dağıtma](container-service-deployment.md)
* [Azure Container Service kümesine bağlanma](../container-service-connect.md)

> [!NOTE]
> Bu makalede, yerel bağlantı noktası 80 üzerinden DC/OS kümesine tünel varsayar.
>

## <a name="explore-the-dcos-ui"></a>DC/OS kullanıcı arabirimini keşfetme
Secure Shell (SSH) tüneli [oluşturarak](../container-service-connect.md) http://localhost/ adresine gidin. Bu, DC/OS web kullanıcı arabirimini yükler ve kullanılan kaynaklar, etkin aracılar ve çalışan hizmetler gibi, küme hakkında bilgileri gösterir.

![DC/OS Kullanıcı Arabirimi](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Marathon kullanıcı arabirimini keşfetme
Marathon kullanıcı arabirimini görmek için http://localhost/marathon adresine gidin. Bu ekranda, Azure Kapsayıcı Hizmeti DC/OS kümesinde yeni kapsayıcı veya başka bir uygulama başlatabilirsiniz. Kapsayıcıları ve uygulamaları çalıştırma hakkında bilgileri de görebilirsiniz.  

![Marathon kullanıcı arabirimi](./media/container-service-mesos-marathon-ui/dcos3.png)

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

![Yeni Uygulama Kullanıcı Arabirimi --Genel](./media/container-service-mesos-marathon-ui/dcos4.png)

![Yeni Uygulama Kullanıcı Arabirimi--Docker Kapsayıcısı](./media/container-service-mesos-marathon-ui/dcos5.png)

![Yeni Uygulama Kullanıcı Arabirimi--Bağlantı Noktaları ve Hizmet Bulma](./media/container-service-mesos-marathon-ui/dcos6.png)

Kapsayıcı bağlantı noktasını statik olarak aracıdaki bağlantı noktasıyla eşlemek istiyorsanız, JSON Modu’nu kullanmalısınız. Bunu yapmak için, geçişi kullanarak Yeni Uygulama Sihirbazı'nı **JSON Modu**’na geçirin. Ardından, uygulama tanımının `portMappings` bölümünün altına aşağıdaki ayarı girin. Bu örnek, kapsayıcının 80 numaralı bağlantı noktasını DC/OS aracının 80 numaralı bağlantı noktasına bağlar. Bu değişikliği yaptıktan sonra bu sihirbazı JSON modundan çıkarabilirsiniz.

```none
"hostPort": 80,
```

![Yeni Uygulama Kullanıcı Arabirimi--bağlantı noktası 80 örneği](./media/container-service-mesos-marathon-ui/dcos13.png)

Sistem durumu denetimlerini etkinleştirmek istiyorsanız **Sistem Durumu Denetimleri** sekmesinde bir yol belirleyin.

![Yeni Uygulama UI--durum denetimleri](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

DC/OS kümesi bir grup özel ve ortak aracıyla dağıtılır. Kümenin İnternet’ten uygulamalara erişebilmesi için, uygulamaları ortak aracıya dağıtmanız gerekir. Bunu yapmak için, Yeni Uygulama Sihirbazı'nın **İsteğe Bağlı** sekmesini seçin ve **Kabul Edilen Kaynak Rolleri** için **slave_public** girin.

Ardından **Uygulama Oluştur**'a tıklayın.

![Yeni Uygulama Kullanıcı Arabirimi--ortak aracı ayarı](./media/container-service-mesos-marathon-ui/dcos14.png)

Marathon ana sayfasına geri döndüğünüzde, kapsayıcının dağıtımın durumunu görebilirsiniz. Başlangıçta **Dağıtılıyor** durumu görüntülenir. Dağıtım başarıyla tamamlandıktan sonra durum **Çalışıyor** olarak değişir.

![Marathon ana sayfası kullanıcı arabirimi--kapsayıcı dağıtım durumu](./media/container-service-mesos-marathon-ui/dcos7.png)

DC/OS web kullanıcı arabirimine (http://localhost/) geri döndüğünüzde, bir görevin (örneğimizde, Docker biçimli kapsayıcının) DC/OS kümesinde çalıştığını görürsünüz.

![DC/OS web kullanıcı arabirimi--kümede çalışan görev](./media/container-service-mesos-marathon-ui/dcos8.png)

Görevin üzerinde çalıştığı küme düğümünü görmek için **Düğümler** sekmesine tıklayın.

![DC/OS web kullanıcı arabirimi--görev küme düğümü](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a>Kapsayıcı ulaşmak

Bu örnekte, uygulamanın bir ortak aracı düğümünde çalışıyor. Küme FQDN'si aracıya göz atarak internet'ten uygulamasına ulaşması: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, burada:

* **DNSPREFIX** Kümeyi dağıttığınızda sağladığınız DNS önekidir.
* **REGION** kaynak grubunuzun bulunduğu bölgedir.

    ![Internet'ten Nginx](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS ve Marathon API’si ile çalışma](container-service-mesos-marathon-rest.md)

* Mesos ile Azure Container Service’e ilişkin ayrıntılar

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
