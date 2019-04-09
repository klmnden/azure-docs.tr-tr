---
title: Azure Resource Manager şablonu örnekleri - Azure Container Instances
description: Azure Container Instances için Azure Resource Manager şablonu örnekleri
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/07/2019
ms.author: danlep
ms.openlocfilehash: 3d73d05c64f4b4867c69a15089c19ab8c320b9a8
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006469"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Azure Container Instances için Azure Resource Manager şablonları

Aşağıdaki örnek şablonları çeşitli yapılandırmalarda kapsayıcı örneklerini dağıtın.

Dağıtım seçenekleri için bkz. [dağıtım](#deployment) bölümü. Azure Container Instances kendi şablonlarınızı oluşturmak istiyorsanız [Resource Manager şablon başvurusu] [ ref] ayrıntıları şablon biçimi ve kullanılabilir özellikler.

## <a name="sample-templates"></a>Örnek şablonlar

| | |
|-|-|
| **Uygulamalar** ||
| [WordPress][app-wp] | Bir kapsayıcı grubunda bir WordPress Web sitesi ve MySQL veritabanı oluşturur. MySQL veritabanı ve WordPress sitesi içeriği bir Azure dosyaları için kalıcı paylaşın. Ayrıca, ortak ağ erişimini WordPress kullanıma sunmak için bir uygulama ağ geçidi oluşturur. |
| [SQL Server ve IIS ile MS NAV][app-nav] | Tek bir Windows kapsayıcısı ile tam özellikli ve kendi başına bir Dynamics NAV dağıtır / Dynamics 365 Business Central ortam. |
| **Birimler** ||
| [emptyDir][vol-emptydir] | EmptyDir birimi paylaşan iki Linux kapsayıcıları dağıtır. |
| [GitRepo][vol-gitrepo] | GitHub deposuna kopyalar ve bir birim olarak bağlar bir Linux kapsayıcı dağıtır. |
| [Gizli anahtarı][vol-secret] | Gizli bir birim takılı bir PFX sertifikası ile bir Linux kapsayıcı dağıtır. |
| **Ağ** ||
| [UDP kullanıma sunulan kapsayıcı][net-udp] | UDP bağlantı noktası kullanıma sunan bir Windows veya Linux kapsayıcı dağıtır. |
| [Genel IP ile Linux kapsayıcısı][net-publicip] | Tek bir Linux kapsayıcısı bir genel IP erişilebilir dağıtır. |
| [Bir sanal ağ (Önizleme) bir kapsayıcı grubu dağıtma][net-vnet] | Yeni sanal ağ, alt ağ, ağ profili ve kapsayıcı grubu dağıtır. |
| **Azure kaynakları** ||
| [Azure depolama hesabı oluşturun ve dosyalarını paylaşın][az-files] | Azure CLI'yı bir depolama hesabı ve bir Azure dosya paylaşımı oluşturmak için bir kapsayıcı örneği kullanır.

## <a name="deployment"></a>Dağıtım

Kaynakları Resource Manager şablonları ile dağıtmak için birkaç seçeneğiniz vardır:

[Azure CLI][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Azure portalı][deploy-portal]

[REST API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[net-vnet]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
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
