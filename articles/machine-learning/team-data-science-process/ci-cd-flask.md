---
title: 'Yapay zeka (AI) uygulamalar için DevOps: Docker, Kubernetes ve Python Flask uygulaması kullanarak Azure üzerinde sürekli tümleştirme işlem hattı oluşturma'
description: "Yapay zeka (AI) uygulamalar için DevOps: Azure'da Docker ve Kubernetes kullanarak sürekli tümleştirme işlem hattı oluşturma"
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
ms.openlocfilehash: 4d95fc25ed6f2f2efec8313e5b208b3cccbb619f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968800"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>Yapay zeka (AI) uygulamalar için DevOps: Azure'da Docker ve Kubernetes kullanarak sürekli tümleştirme işlem hattı oluşturma
Yapay ZEKA uygulaması için iş, veri Bilimcileri makine öğrenimi modelleri ve bir uygulama oluşturmak ve kullanmak için son kullanıcılara gösterme uygulama geliştiriciler genellikle iki akışlarını vardır. Bu makalede, biz nasıl sürekli tümleştirme (CI) uygulanacağını gösteren / sürekli teslim (CD) işlem hattı için yapay ZEKA uygulama. Yapay ZEKA uygulaması, uygulama kodu kullanan machine learning (ML) bir modelle katıştırılmış birleşimidir. Bu makalede, biz pretrained modeli özel Azure blob depolama hesabından getiriliyor, AWS S3 hesabı da olabilir. Makale için bir basit bir python flask web uygulaması kullanacağız.

> [!NOTE]
> Bu, CI/CD gerçekleştirilebilir yollarından biridir. Araçlar ve diğer Önkoşullar aşağıda belirtilen alternatifleri vardır. Ek İçerik ekibiyiz gibi bu yayımlarız.
>
>

## <a name="github-repository-with-document-and-code"></a>Belge ve kod ile GitHub deposu
Kaynak kodundan indirebileceğiniz [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). A [ayrıntılı öğretici](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) de kullanılabilir.

## <a name="pre-requisites"></a>Ön koşullar
Aşağıda açıklanan CI/CD işlem hattı izleyen için ön koşullar şunlardır:
* [Visual Studio Team Services hesabı](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Kubernetes çalıştıran azure Container Service (AKS) kümesi](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Azure Container kayıt defteri (ACR) hesabı](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)
* [Kubernetes kümesine göre komutları çalıştırmak için Kubectl yükleyin.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Biz bu ACS kümeden yapılandırma getirmek gerekir. 
* GitHub hesabınıza depo çatalı oluşturma.

## <a name="description-of-the-cicd-pipeline"></a>CI/CD işlem hattı açıklaması
İşlem hattı kicks kapatmak için test geçişleri en son sürüme sürerse test paketi çalıştırmak, her yeni işleme bir Docker kapsayıcısında paketler. Azure container service (ACS) kullanarak kapsayıcı dağıtılır ve görüntüler Azure container Registry'de (ACR) güvenli bir şekilde depolanır. ACS Kubernetes kapsayıcı kümesini yönetmek için çalışıyor, ancak Docker Swarm veya Mesos seçebilirsiniz.

Uygulamayı bir Azure depolama hesabı ve paketlerin en son modelden, uygulamanın bir parçası olarak güvenli bir şekilde çeker. Dağıtılan bir uygulama, uygulama kodu ve tek kapsayıcı olarak paketlenmiş ML model vardır. Bu, en son ML model ile en son kodu, üretim uygulamalarının her zaman çalıştığından emin olmak için uygulama geliştiriciler ve veri uzmanları, birbirinden ayırır.

İşlem hattı mimari aşağıda verilmiştir. 

![Mimari](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>CI/CD işlem hattının adımları
1. Geliştirici uygulama kodu kendi tercih ettiğiniz IDE'de çalışın.
2. Bunlar kaynak denetimine (VSTS çeşitli kaynak denetimleri iyi desteği olan) kendi seçtikleri kod işle
3. Ayrıca, veri uzmanı iş modellerindeki geliştirmeye.
4. Bu durumda mutlu sonra bunlar için bir model deposu modeli yayımlayın, blob depolama hesabı kullanıyoruz. Bu kolayca Azure ML Workbench'ın Model Yönetimi hizmeti, REST API'ler aracılığıyla ile değiştirilebilir.
5. Bir derleme vsts'de github'da işleme dayalı başlatılır.
6. VSTS derleme işlem hattı, en son modele Blob kapsayıcısından çeker ve bir kapsayıcı oluşturur.
7. VSTS görüntüyü Azure Container Registry'de özel görüntü deposuna gönderir.
8. Belirlenmiş bir zamanlamaya göre (gecelik), yayın işlem hattı başlatılır.
9. ACR son görüntüden çekilen ve ACS Kubernetes kümesinde arasında dağıtılır.
10. Uygulama için kullanıcıların isteği, DNS sunucusu üzerinden gider.
11. DNS sunucusu, yük dengeleyici isteği geçirir ve kullanıcı geri yanıt gönderir.

## <a name="next-steps"></a>Sonraki adımlar
* Başvurmak [öğretici]((https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md)) ayrıntılarını izleyin ve kendi uygulamanız için CI/CD ardışık uygulamak için.

## <a name="references"></a>Başvurular
* [Team Data Science Process (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/azure/machine-learning/service/)
* [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/vso/)
* [Azure Kubernetes hizmeti (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)