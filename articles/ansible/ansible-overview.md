---
title: Azure ile Ansible kullanma
description: Ansible otomatikleştiren bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını kullanmayı giriş.
ms.service: ansible
keywords: ansible, azure, devops, genel bakış, bulut sağlama, yapılandırma yönetimi, uygulama dağıtımı, ansible modülleri, ansible playbooks
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: a7ce3c239a50462a9af137eb958268f72dbf79d1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28212109"
---
# <a name="ansible-with-azure"></a>Azure ile Ansible

[Ansible](http://www.ansible.com) bulut sağlama, yapılandırma yönetimi ve uygulama dağıtımlarını otomatikleştiren bir açık kaynaklı üründür. Ansible kullanarak sanal makineler, kapsayıcıları ve ağ sağlamak ve bulut altyapılarının tamamlayın. Ayrıca, Ansible dağıtımını ve yapılandırmasını, ortamınızdaki kaynakların otomatikleştirmenizi sağlar.

Bu makalede Azure ile Ansible kullanmanın avantajları bazıları temel bir bakış sağlar.

## <a name="ansible-playbooks"></a>Ansible playbooks

[Ansible playbooks](http://docs.ansible.com/ansible/latest/playbooks.html) Ansible'nın yapılandırma, dağıtım ve orchestration dil. Bunlar uzak sistemlerinizi zorlamak için istediğiniz bir ilke veya genel BT işlemindeki adımlar açıklanmaktadır. Bir playbook oluşturduğunuzda, bu nedenle, bir yapılandırma veya bir işlem modeli tanımlayan YAML kullanarak yapın.

## <a name="ansible-modules"></a>Ansible modülleri

Ansible içeren bir paketi olan [Ansible modülleri](http://docs.ansible.com/ansible/latest/modules_by_category.html) , yürütülebilir doğrudan uzak ana bilgisayarların veya aracılığıyla [playbooks](http://docs.ansible.com/ansible/latest/playbooks.html). Kullanıcılar kendi modüllerini de oluşturabilirsiniz. Modüller, hizmetleri, paketler veya dosyaları - gibi sistem kaynaklarının - denetlemek veya sistem komutlarını çalıştırmak için kullanılabilir.

Azure Hizmetleri ile etkileşmek için Ansible dizisi içerir [Ansible bulut modülleri](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) kolayca oluşturun ve Azure altyapınızda düzenlemek için araçlar sağlar. 

## <a name="migrate-existing-workload-to-azure"></a>Mevcut iş yükü Azure'a geçirme

Ansible altyapınızı tanımlamak için kullandığınız sonra Azure ortamınıza gerektiğinde otomatik olarak ölçekleme izin vererek, uygulamanızın playbook uygulayabilirsiniz. 

## <a name="automate-cloud-native-application-in-azure"></a>Azure bulut yerel uygulamada otomatikleştirme

Ansible Azure mikro gibi kullanılarak Azure bulut yerel uygulamalarda otomatikleştirmenizi sağlar [Azure işlevleri](https://azure.microsoft.com//services/functions/) ve [azure'da Kubernetes](https://azure.microsoft.com/services/container-service/kubernetes/)).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Dağıtımları dinamik stok ile yönetme
Aracılığıyla kendi [dinamik stok](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) özelliği, Ansible sağlar özelliği çekme stok Azure kaynaklarından. Ardından, var olan Azure dağıtımlarınızı etiketi ve Ansible aracılığıyla etiketli bu dağıtımlarını yönetin.

## <a name="additional-azure-marketplace-options"></a>Ek Azure Marketi seçenekleri
[Ansible kule](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Azure Market görüntüsü Red Hat ile BT Otomasyon ölçekleme ve karmaşık dağıtımlar arasında fiziksel, sanal yönetmek ve bulut altyapılarının kuruluşlara yardımcı olur. Ansible kule görünürlük, Denetim, güvenlik ve verimlilik bugünün kuruluşlar için gerekli ek düzeyleri sağlayan özellikler içerir. Böylece kimlik bilgilerinizi açığa çıkma riskini olmadan daha az deneyimli çalışanlara işleri devredebilirsiniz Ansible kule Azure ve SSH anahtarları gibi kimlik bilgilerini şifreler.

## <a name="next-steps"></a>Sonraki adımlar
- [Ansible yapılandırın](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Bir Linux VM oluşturma](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)