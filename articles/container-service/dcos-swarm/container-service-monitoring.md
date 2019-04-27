---
title: (KULLANIM DIŞI) Azure DC/OS kümesi - Datadog ile izleme
description: Datadog ile bir Azure Container Service kümesini izleme. Datadog aracıları kümenize dağıtmak için DC/OS web kullanıcı arabirimini kullanın.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: a094369d467b3b1f3d5fe93f870dccc9eae7519c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60508124"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>(KULLANIM DIŞI) Bir Azure Container Service DC/OS kümesini Datadog ile izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlere Datadog aracıları dağıtacağız. Bu yapılandırma için Datadog sahip bir hesap gerekir. 

## <a name="prerequisites"></a>Önkoşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Git [ https://datadoghq.com ](https://datadoghq.com) Datadog hesabı ayarlamak için. 

## <a name="datadog"></a>Datadog
Datadog gelen Azure Container Service kümenizdeki kapsayıcıları izleme verilerini toplayan bir izleme hizmetidir. Datadog Docker tümleştirmesi, kapsayıcılarınızın belirli ölçümleri görebileceğiniz bir Pano vardır. Kapsayıcılardan toplanan ölçümler, CPU, bellek, ağ ve g/ç tarafından düzenlenir. Datadog ölçümleri kapsayıcılar ve görüntüleri halinde böler. Kullanıcı Arabirimi için CPU kullanımını nasıl göründüğüne ilişkin bir örnek aşağıda verilmiştir.

![Datadog kullanıcı Arabirimi](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Datadog dağıtım Marathon ile yapılandırma
Bu adımları, yapılandırma ve kümenize Marathon ile Datadog uygulamaları dağıtma gösterilmektedir. 

DC/OS kullanıcı arabiriminize erişin [ http://localhost:80/ ](http://localhost:80/). Bir kez ", sol altta bulunan Evreni" DC/OS Arabiriminde gidin ve ardından "Datadog" için arama yapın ve "Yükle" seçeneğine tıklayın.

![DC/OS Evreni içinde Datadog paket](./media/container-service-monitoring/datadog1.png)

Şimdi yapılandırmasını tamamlamak için Datadog hesap veya ücretsiz bir deneme hesabı gerekir. Ardından sol Datadog Web sitesi görünümü için oturum açtınız ve tümleştirmeler gidin -> [API'leri](https://app.datadoghq.com/account/settings#api). 

![Datadog API anahtarı](./media/container-service-monitoring/datadog2.png)

Ardından DC/OS Evreni içinde Datadog yapılandırma API anahtarınızı girin. 

![DC/OS evreninde Datadog yapılandırma](./media/container-service-monitoring/datadog3.png) 

Yukarıdaki yapılandırmada örnekleri 10000000 için ayarlanır. Bunu yeni bir düğüm kümeye Datadog otomatik olarak bu düğüme bir aracı dağıtacağınız eklendiğinde. Bu geçici bir çözümdür. Tekrar Datadog Web sitesine gidin ve bulmak paketi yükledikten sonra "[panolar](https://app.datadoghq.com/dash/list)." Buradan, özel ve tümleştirme panoları görürsünüz. [Docker Pano](https://app.datadoghq.com/screen/integration/docker) kümenizi izlemek için ihtiyacınız olan tüm kapsayıcı ölçümleri olacaktır. 

