---
title: Azure Resource Manager şablonu örnekler - Azure kapsayıcı örnekleri
description: Azure kapsayıcı örnekleri için Azure Resource Manager şablon örnekleri
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/17/2018
ms.author: marsma
ms.openlocfilehash: fcc2e6c52e773d95bcdfe43d881fce036fae6513
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260255"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure kapsayıcı örnekleri için Azure Resource Manager şablonları

Aşağıdaki örnek şablonları çeşitli yapılandırmaları kapsayıcı örnekleri dağıtabilirsiniz.

Dağıtım seçenekleri için bkz: [dağıtım](#deployment) bölümü. Azure kapsayıcı örnekleri kendi şablonlarınızı oluşturmak isteyip istemediğinizi [Resource Manager şablonu başvurusu] [ ref] ayrıntıları şablon biçimi ve kullanılabilir özellikler.

## <a name="sample-templates"></a>Örnek şablonları

| | |
|-|-|
| **Uygulamalar** ||
| [Wordpress][app-wp] | Bir WordPress Web sitesi ve MySQL veritabanının bir kapsayıcı örneği oluşturur. WordPress sitesi içeriği ve MySQL veritabanı için bir Azure dosyaları kalıcı paylaşın. |
| [MS NAV SQL Server ve IIS ile][app-nav] | Tam özellikli bir müstakil Dynamics NAV ile tek bir Windows kapsayıcısı dağıtır / Dynamics 365 iş merkezi ortamı. |
| **Birimleri** ||
| [emptyDir][vol-emptydir] | Bir emptyDir birimi paylaşan iki Linux kapsayıcı dağıtır. |
| [GitRepo][vol-gitrepo] | GitHub depo klonlar ve bir birim olarak bağlar Linux kapsayıcı dağıtır. |
| [Gizli][vol-secret] | Gizli bir birim olarak oluşturulmuş bir PFX sertifikası ile bir Linux kapsayıcı dağıtır. |
| **Ağ** ||
| [UDP kullanıma sunulan kapsayıcısı][net-udp] | UDP bağlantı noktası kullanıma sunan bir Windows veya Linux kapsayıcı dağıtır. |
| [Genel IP ile Linux kapsayıcısı][net-publicip] | Tek bir Linux kapsayıcısı genel bir IP erişilebilir dağıtır. |
| **Azure kaynakları** ||
| [Azure depolama hesabı oluşturun ve dosyaları paylaşma][az-files] | Azure CLI, bir depolama hesabı ve bir Azure dosya paylaşımı oluşturmak için bir kapsayıcı örneği kullanır.

## <a name="deployment"></a>Dağıtım

Resource Manager şablonları kaynaklarla dağıtmak için birkaç seçeneğiniz vardır:

[Azure CLI][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure portalı][deploy-portal]

[REST API'Sİ][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/resource-group-template-deploy-cli.md
[deploy-portal]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/resource-group-template-deploy.md
[deploy-rest]: ../azure-resource-manager/resource-group-template-deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups