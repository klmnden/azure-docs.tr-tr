---
title: "Jenkins ve Azure genel bakış | Microsoft Docs"
description: "Jenkins yapı konağı ve Azure Otomasyon sunucusu dağıtmak ve dağıtım ve sürekli tümleştirme (CI/CD) ardışık düzen genişletmek için Azure işlem ve depolama kaynaklarını kullanır."
services: jenkins
author: rloutlaw
manager: justhe
ms.service: jenkins
ms.devlang: NA
ms.topic: article
ms.workload: na
ms.date: 08/22/2017
ms.author: routlaw
ms.custom: mvc
ms.openlocfilehash: daa202ddf0dc934c491ead3951ddc4fdc3dd819c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-and-jenkins"></a>Azure ve Jenkins

[Jenkins](https://jenkins.io/) sürekli tümleştirme ve yazılım projeleriniz için teslim (CI/CD) ayarlamak için kullanılan bir popüler açık kaynak Otomasyon sunucusu. Jenkins dağıtımınızda Azure ana bilgisayar veya Azure kaynakları kullanarak var olan Jenkins yapılandırmanızı genişletin. Jenkins eklentileri CI/CD uygulamalarınızın Azure basitleştirmek de kullanılabilir.

Bu makalede Azure ile Jenkins kullanmaya giriş, çekirdek ayrıntılı Azure Jenkins kullanıcıların kullanımına sunar. Azure kendi Jenkins sunucunuzu kullanmaya başlamak için bkz: bizim [Hızlı Başlangıç](install-jenkins-solution-template.md).

## <a name="host-your-jenkins-servers-in-azure"></a>Jenkins sunucularınızı Azure ana bilgisayar

Ana bilgisayar Jenkins yapı automation'ınızı merkezileştirmek ve yazılım projelerinizi ihtiyaçlarınız arttıkça dağıtımınız ölçeklendirmek için azure'da. Jenkins Azure kullanarak dağıtabilirsiniz:
 
- [Jenkins çözüm şablonu](install-jenkins-solution-template.md) Azure markette.
- [Azure sanal makineleri](/azure/virtual-machines/linux/overview). Bkz: bizim [öğretici](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) Jenkins örneği üzerinde bir VM oluşturmak için.
- Üzerinde bir Kubernetes kümesinin [Azure kapsayıcı hizmeti](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough), bkz: bizim [nasıl yapılır](/azure/container-service/kubernetes/container-service-kubernetes-jenkin).

İzleme ve Azure Jenkins kullanarak dağıtımı yönetme [günlük analizi](/azure/log-analytics/log-analytics-overview), [Operations Management Suite](/azure/operations-management-suite/operations-management-suite-overview)ve [Azure CLI] (/ CLI/azure/genel bakış).

## <a name="scale-your-build-automation-on-demand"></a>Yapı automation'ınızı ölçeğini isteğe bağlı olarak

Derleme aracıları Jenkins yapı kapasitenizi derlemeleri sayısı ve karmaşıklığı işlerinizin olarak ölçeklendirmek için mevcut Jenkins dağıtımınızı ekleyin ve ardışık düzen artırın. Bunlar çalıştırabilirsiniz kullanarak Azure sanal makinelerde yapı aracıları [Azure VM Aracısı eklentisi](jenkins-azure-vm-agents.md). Bkz: bizim [öğretici](/azure/jenkins/jenkins-azure-vm-agents) daha fazla ayrıntı için.

Bir kez ile yapılandırılmış bir [Azure hizmet sorumlusu](/azure/azure-resource-manager/resource-group-overview), Jenkins işler ve ardışık düzen bu kimlik bilgilerini kullanabilirsiniz:

- Güvenli şekilde depolayın ve Arşiv yapı yapıları içinde [Azure Storage](/azure/storage/common/storage-introduction) kullanarak [Azure depolama eklentisi](https://plugins.jenkins.io/windows-azure-storage). Gözden geçirme [Jenkins depolama yapılır](/azure/storage/common/storage-java-jenkins-continuous-integration-solution) daha fazla bilgi için.
- Yönetme ve Azure kaynakları ile yapılandırma [Azure CLI](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Kodunuzu Azure hizmetlerine dağıtma

Jenkins eklentileri uygulamalarınızı Azure Jenkins CI/CD hatlarınızı bir parçası olarak dağıtmak için kullanın. Uygulamasına dağıtma [Azure App Service](/azure/app-service/) ve [Azure kapsayıcı hizmeti](/azure/container-service/kubernetes/) altyapının yönetmek zorunda kalmadan aşaması, test ve yayın güncelleştirmeleri uygulamalarınıza olanak sağlar.

 Eklentiler aşağıdaki hizmetleri ve ortamlar dağıtmak kullanılabilir:

- [Azure Web uygulaması Linux'ta](/azure/app-service/containers/app-service-linux-intro). Bkz: [öğretici](java-deploy-webapp-tutorial.md) başlamak için.
- [Azure Web uygulaması](/azure/app-service/app-service-web-overview). Bkz: [nasıl yapılır](deploy-Jenkins-app-service-plugin.md) başlamak için.

