---
title: Azure için Ansible modülü ve sürüme Matrisi
description: Azure için Ansible modülü ve sürüme Matrisi
ms.service: ansible
keywords: ansible, roller, matris, sürüm, azure, devops
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: a71f28413028efe3547df7c464ec852fe2161f63
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649950"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible modülü ve sürüme Matrisi

## <a name="ansible-modules-for-azure"></a>Azure modülleri Ansible
Ansible, bir dizi uzak konaklar üzerinde doğrudan veya playbook'ları aracılığıyla yürütülen modül ile birlikte gelir.
Bu makalede, sanal makine, ağ ve container services gibi Azure bulut kaynaklarını sağlayabilirsiniz Azure Ansible modülleri listeler. Bu modüller, Ansible resmi sürümünü veya Microsoft tarafından yayımlanan aşağıdaki playbook rolleri alabilirsiniz.

| Azure için ansible'ı Modülü                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | [Ansible rolü](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **İşlem**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_availabilityset_facts              | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_deployment                         | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_resource                           | -            | -                           | Evet          | Evet          | Evet                                 | 
| azure_rm_resource_facts                     | -            | -                           | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualmachineimage_facts          | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_resourcegroup                      | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_resourcegroup_facts                | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualmachine                     | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_virtualmachine_extension           | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualmachine_scaleset            | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_image                              |              | Evet                         | Evet          | Evet          | Evet                                 | 
| **Ağ**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_virtualnetwork_facts               | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_subnet                             | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_networkinterface                   | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_networkinterface_facts             | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_publicipaddress                    | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_publicipaddress_facts              | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_dnsrecordset                       | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_dnsrecordset_facts                 | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_dnszone                            | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_dnszone_facts                      | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_loadbalancer                       | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_loadbalancer_facts                 | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Evet                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | Evet                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | Evet                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_securitygroup                      | Evet          | Evet                         | Evet          | Evet          | Evet                                 |
| azure_rm_route                              | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_routetable                         | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | Evet          | Evet                                 | 
| **Depolama**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_storageaccount_facts               | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_storageblob                        | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_managed_disk                       | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_managed_disk_facts                 | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| **Kapsayıcılar**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | Evet          | Evet          | Evet                                 | 
| azure_rm_aks_facts                          | -            | -                           | Evet          | Evet          | Evet                                 | 
| azure_rm_acs                                | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_containerinstance                  | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | Evet                                 | 
| azure_rm_containerregistry                  | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Evet                                 | 
| **Azure İşlevleri**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_functionapp_facts                  | Evet          | Evet                         | Evet          | Evet          | Evet                                 | 
| **Veritabanları**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_sqlserver_facts                    | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_sqldatabase                        | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_mysqlserver                        | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_mysqldatabase                      | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_postgresqlserver                   | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_postgresqldatabase                 | -            | Evet                         | Evet          | Evet          | Evet                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Evet          | Evet                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Evet                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Evet                                 | 
| **Anahtar Kasası**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | Evet                         | Evet          | Evet          | Evet                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | Evet                               |
| azure_rm_keyvaultkey                        | -            | Evet                         | Evet          | Evet          | Evet                                 |
| azure_rm_keyvaultsecret                     | -            | Evet                         | Evet          | Evet          | Evet                                 |
| **Web Apps**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | Evet          | Evet                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | Evet          | Evet                                 | 
| **Otomatik ölçeklendirme**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | Evet          | Evet                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | Evet          | Evet                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Azure için playbook rol giriş
[Azure_preview_module playbook rol](https://galaxy.ansible.com/Azure/azure_preview_modules/) en eksiksiz rolü olduğundan, en son tüm Azure modüllerini içerir. Güncelleştirmeler ve hata düzeltmeleri, resmi Ansible sürümden daha fazla zamanında gerçekleştirilir. Sağlama amacıyla Azure kaynağı için Ansible'ı kullanıyorsanız azure_preview_module playbook rolü yüklemek için önerilir.

Azure_preview_module playbook rol üç haftada serbest bırakılır.

## <a name="next-steps"></a>Sonraki adımlar
Playbook rolleri için ilgili daha fazla bilgi [yeniden kullanılabilir playbook'ları oluşturma](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
