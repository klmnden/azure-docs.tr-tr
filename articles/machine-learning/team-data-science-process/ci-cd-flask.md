---
title: "DevOps yapay Intelligence (AI) uygulamaları için: Docker, Kubernetes & Python Flask bir uygulama kullanarak Azure'da sürekli tümleştirme ardışık düzen oluşturma"
description: "DevOps yapay Intelligence (AI) uygulamaları için: Docker ve Kubernetes kullanarak Azure'da sürekli tümleştirme ardışık düzen oluşturma"
services: machine-learning, team-data-science-process
documentationcenter: ''
author: jainr
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jainr
ms.openlocfilehash: 233da393bb9e030d885ce588f4841dc1c707c1cb
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836275"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps yapay Intelligence (AI) uygulamaları için: Docker ve Kubernetes kullanarak Azure'da sürekli tümleştirme ardışık düzeni oluşturma
AI uygulamaya ait iş, makine öğrenimi modellerini ve uygulama oluşturma ve kullanmak için son kullanıcılara gösterme uygulama geliştiriciler derleme veri Bilimcilerine sık iki akışları vardır. Bu makalede, sizi nasıl sürekli tümleştirme (CI) uygulandığını gösteren / sürekli teslim (CD) kanal AI uygulamaya ait. AI uygulama, uygulama kodu pretrained makine öğrenme (ML) bir modelle katıştırılmış birleşimidir. Bu makale için biz pretrained modeli özel Azure blob depolama hesabından getirme, AWS S3 hesabı da olabilir. Makale için bir basit python flask web uygulaması kullanacağız.

> [!NOTE]
> Bu, CI/CD gerçekleştirilen çeşitli yollardan biri. Araçları ve diğer ön koşullar aşağıda belirtilen alternatifleri vardır. Biz ek içerik geliştirdikçe bu yayımlamak.
>
>

## <a name="github-repository-with-document-and-code"></a>Belge ve kod GitHub deposuyla
Kaynak kodundan indirebilirsiniz [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). A [ayrıntılı öğretici](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) da kullanılabilir.

## <a name="pre-requisites"></a>Ön koşullar
Aşağıda açıklanan CI/CD ardışık düzen aşağıdaki ön koşullar verilmiştir:
* [Visual Studio Team Services hesabı](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student)
* [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure kapsayıcı hizmeti (AKS) küme Kubernetes çalıştırma](https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Azure kapsayıcı kayıt (ACR) hesabı](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal)
* [Kubernetes küme karşı komutları çalıştırmak için Kubectl yükleyin.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) ACS kümeden yapılandırma getirmek için gerekli. 
* GitHub hesabınızda depoyu çatallaştırmanız.

## <a name="description-of-the-cicd-pipeline"></a>CI/CD ardışık düzen açıklaması
Kapalı ardışık düzen kicks test paketine test geçişleri en son sürüme sürerse çalıştırmak her yeni yürütülmesi için bir Docker kapsayıcısı paketler. Kapsayıcı, ardından Azure kapsayıcı hizmeti (ACS) kullanılarak dağıtılır ve görüntüleri güvenli bir şekilde (ACR) Azure kapsayıcı kayıt defterinde saklanır. ACS Kubernetes kapsayıcı küme yönetmek için çalışıyor, ancak Docker Swarm veya Mesos seçebilirsiniz.

Uygulamayı bir Azure Storage hesabı ve paketleri en son modelden, uygulamanın bir parçası olarak güvenli bir şekilde çeker. Dağıtılan bir uygulama uygulama kodu ve tek kapsayıcı olarak paketlenmiş ML model sahiptir. Bu, son ML modeliyle en son kodu, üretim uygulama her zaman çalıştığından emin olmak için uygulama geliştiricileri ve veri bilimcilerine, ayrıştırır.

Ardışık düzen mimarisinin aşağıda verilmiştir. 

![Mimari](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>CI/CD ardışık adımları
1. Geliştirici kendi seçtikleri uygulama kodu IDE üzerinde çalışır.
2. Bunlar (VSTS çeşitli kaynağı denetimleri için iyi desteğine sahip) kendi seçtikleri kaynak denetimi için kod tamamlama
3. Ayrıca, veri Bilimcisi iş kendi modeli geliştirme.
4. Bu durumda mutluluk sonra bir model deposuna model yayımlarlar, blob storage hesabı kullanıyoruz. Bu kolayca Azure ML çalışma ekranı'nın Model yönetim hizmeti, REST API'leri aracılığıyla değiştirilebilir.
5. Derleme VSTS github yürütme dayalı olarak koparılan.
6. VSTS yapı ardışık düzen son model Blob kapsayıcısından çeker ve bir kapsayıcı oluşturur.
7. VSTS, Azure kapsayıcı kayıt defterinde özel görüntü deposu için görüntüyü iter.
8. Belirlenmiş bir zamanlamaya göre (gecelik), sürüm ardışık düzen koparılan devre dışı.
9. ACR son görüntüden çekilen ve ACS Kubernetes kümede üzerinden dağıtılabilir.
10. Uygulama için Kullanıcı isteği DNS sunucusu üzerinden gider.
11. DNS sunucusu, yük dengeleyici isteği geçirir ve kullanıcı geri yanıt gönderir.

## <a name="next-steps"></a>Sonraki adımlar
* Başvurmak [öğretici]((https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md)) ayrıntıları izleyin ve kendi CI/CD ardışık düzeni, uygulamanız için uygulamaktır.

## <a name="references"></a>Başvurular
* [Takım veri bilimi işlemi (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/en-us/azure/machine-learning/service/)
* [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/vso/)
* [Azure Kubernetes Hizmetleri (AKS)](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)