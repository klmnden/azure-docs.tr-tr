---
title: Ansible ile Azure kullanarak | Microsoft Docs
description: Bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren için Ansible'ı kullanarak giriş.
keywords: ansible'ı, azure, devops, genel bakış, bulut sağlama, yapılandırma yönetimi, uygulama dağıtımı, ansible modülleri, ansible playbook'ları
ms.topic: overview
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 54d66a0ac425a9a0a63c91317ead0e02ecf9452c
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764138"
---
# <a name="using-ansible-with-azure"></a>Ansible'ı kullanarak Azure ile

[Ansible](https://www.ansible.com) bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren bir açık kaynaklı üründür. Ansible'ı kullanarak sanal makineler, kapsayıcılar ve ağ sağlama ve bulut altyapıları tamamlayın. Ayrıca, Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak sağlar.

Bu makalede, Ansible ile Azure kullanmanın avantajlarından bazıları temel bir bakış sağlar.

## <a name="ansible-playbooks"></a>Ansible playbook'ları

[Ansible playbook'ları](https://docs.ansible.com/ansible/latest/playbooks.html) ortamınızı yapılandırmak için ansible'ı doğrudan olanak sağlar. Playbook'ları kullanarak okunabilir olması için YAML kodlanmıştır. Öğreticiler bölümü, yüklemek ve Azure kaynaklarını yapılandırmak için playbook'ları kullanarak birçok örnekleri sağlar. 

## <a name="ansible-modules"></a>Ansible modülleri

Ansible'ı içeren bir paketi [Ansible modülleri](https://docs.ansible.com/ansible/latest/modules_by_category.html) uzak konaklar üzerinde doğrudan veya aracılığıyla çalıştırılan [playbook'ları](https://docs.ansible.com/ansible/latest/playbooks.html). Kullanıcılar kendi modülleri oluşturabilir. Modüller, hizmetleri, paketler veya dosyaları - gibi sistem kaynaklarının - denetlemek veya sistem komutlarını çalıştırmak için kullanılır.

Azure Hizmetleri ile etkileşim kurmak için ansible'ı dizisi içeren [Ansible bulut modülleri](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure). Bu modüller, azure'daki altyapınız oluşturabilir ve etkinleştirin. 

## <a name="migrate-existing-workload-to-azure"></a>Mevcut iş yükünü Azure'a geçirme

Ansible altyapınızı tanımlamak için kullandığınız bir kez otomatik olarak ölçeği gerektiği gibi ortamınızı Azure izin vererek, uygulamanızın playbook uygulayabilirsiniz. 

## <a name="automate-cloud-native-application-in-azure"></a>Azure'daki bulutta yerel uygulama otomatikleştirin

Ansible'ı bulutta yerel uygulamaları gibi Azure mikro hizmetler kullanılarak azure'da otomatikleştirmenizi sağlar [Azure işlevleri](https://azure.microsoft.com//services/functions/) ve [azure'da Kubernetes](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Dinamik envanterle dağıtımları yönetin

Aracılığıyla kendi [dinamik stok](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) özelliği, Ansible sağlar özelliği çekme stok Azure kaynaklarından. Sonra mevcut Azure dağıtımlarınızı etiketleyin ve bu etiketli dağıtımların Ansible aracılığıyla yönetebilirsiniz.

## <a name="additional-azure-marketplace-options"></a>Ek Azure Marketi seçenekleri

[Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Red Hat Azure Marketi'nde görüntüsüdür. 

Ansible Tower, bir web tabanlı kullanıcı Arabirimi ve Pano için aşağıdaki özelliklere sahip Ansible verilmiştir:

* Rol tabanlı erişim denetimi, iş zamanlama ve grafik envanter yönetimi tanımlamanızı sağlar. 
* Var olan araçlarınız ve süreçlerinizle Tower ekleyebilmek, bir REST API ve CLI'yı içerir. 
* Playbook çalıştırmalarını gerçek zamanlı çıktısını destekler. 
* Kimlik bilgileri açığa çıkarmadan görevleri devredebilirsiniz için kimlik bilgileri - anahtarları - Azure ve SSH gibi şifreler.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Azure için Ansible modülü ve sürüme Matrisi

Ansible, sağlama ve Azure kaynaklarını yapılandırma paketi kullanmak için modüller içerir. Bu kaynaklar, sanal makineleri dahil, Ölçek kümeleri, ağ hizmetleri ve kapsayıcı Hizmetleri. [Ansible matris](./ansible-matrix.md) Ansible modülleri Azure ve hangi sevk Ansible sürümlerini listeler.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure'da CentOS for Ansible çözüm şablonu dağıtma](./ansible-deploy-solution-template.md)
- [Hızlı Başlangıç: Ansible'ı kullanarak Azure'da Linux sanal makineleri yapılandırma](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fazure%2Fansible%2Ftoc.json&bc=%2Fazure%2Fbread%2Ftoc.json)