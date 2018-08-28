---
title: Ansible'ı kullanarak Azure ile
description: Bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren için Ansible'ı kullanarak giriş.
ms.service: ansible
keywords: ansible'ı, azure, devops, genel bakış, bulut sağlama, yapılandırma yönetimi, uygulama dağıtımı, ansible modülleri, ansible playbook'ları
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: e710770131c844598762feebe09ba50dc120de0c
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106909"
---
# <a name="ansible-with-azure"></a>Ansible ile Azure

[Ansible](http://www.ansible.com) bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren bir açık kaynaklı üründür. Ansible'ı kullanarak sanal makineler, kapsayıcılar ve ağ sağlama ve bulut altyapıları tamamlayın. Ayrıca, Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak sağlar.

Bu makalede, Ansible ile Azure kullanmanın avantajlarından bazıları temel bir bakış sağlar.

## <a name="ansible-playbooks"></a>Ansible playbook'ları

[Ansible playbook'ları](http://docs.ansible.com/ansible/latest/playbooks.html) Ansible'nın yapılandırma, dağıtım ve düzenleme dil. Bunlar, uzak sistemlere uygulamak istediğiniz ilke ya da genel bir BT süreç adımlarda bir kümesini tanımlayabilirsiniz. Playbook oluşturduğunuzda bir model, bir yapılandırma veya bir işlemi tanımlayan bir YAML kullanarak bunu.

## <a name="ansible-modules"></a>Ansible modülleri

Ansible'ı içeren bir paketi [Ansible modülleri](http://docs.ansible.com/ansible/latest/modules_by_category.html) , yürütülebilir aracılığıyla ya da uzak konaklar üzerinde doğrudan [playbook'ları](http://docs.ansible.com/ansible/latest/playbooks.html). Kullanıcılar ayrıca kendi modülleri oluşturabilir. Modüller, hizmetleri, paketler veya dosyaları - gibi sistem kaynaklarının - denetlemek veya sistem komutlarını çalıştırmak için kullanılabilir.

Azure Hizmetleri ile etkileşim kurmak için ansible'ı dizisi içeren [Ansible bulut modülleri](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) kolayca oluşturabilir ve azure'daki altyapınız için araçlar sağlar. 

## <a name="migrate-existing-workload-to-azure"></a>Mevcut iş yükünü Azure'a geçirme

Ansible altyapınızı tanımlamak için kullandığınız bir kez otomatik olarak ölçeği gerektiği gibi ortamınızı Azure izin vererek, uygulamanızın playbook uygulayabilirsiniz. 

## <a name="automate-cloud-native-application-in-azure"></a>Azure'daki bulutta yerel uygulama otomatikleştirin

Ansible'ı bulutta yerel uygulamaları gibi Azure mikro hizmetler kullanılarak azure'da otomatikleştirmenizi sağlar [Azure işlevleri](https://azure.microsoft.com//services/functions/) ve [azure'da Kubernetes](https://azure.microsoft.com/services/container-service/kubernetes/)).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Dinamik envanterle dağıtımları yönetin
Aracılığıyla kendi [dinamik stok](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) özelliği, Ansible sağlar özelliği çekme stok Azure kaynaklarından. Sonra mevcut Azure dağıtımlarınızı etiketleyin ve bu etiketli dağıtımların Ansible aracılığıyla yönetebilirsiniz.

## <a name="additional-azure-marketplace-options"></a>Ek Azure Marketi seçenekleri
[Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Azure Market görüntüsü Red Hat, kuruluşların BT otomasyonunu ölçeklendirin ve karmaşık dağıtımları arasında fiziksel, sanal yönetmek ve bulut altyapılarının yardımcı olur. Ansible Tower görünürlük, Denetim, güvenlik ve verimlilik günümüzde kuruluşlar için gerekli ek düzeyleri sağlayan özellikler içerir. Ansible Tower Azure ve SSH anahtarları gibi kimlik bilgilerini şifreler; böylece kimlik bilgilerinizi ifşa eden riski olmadan daha az deneyimli çalışanlar için işleri devredebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Ansible'ı yapılandırma](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Linux VM oluşturma](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)