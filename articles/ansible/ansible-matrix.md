---
title: Azure için Ansible modülü ve sürüm Matrisi
description: Azure için Ansible modülü ve sürüm Matrisi
ms.service: ansible
keywords: ansible, roller, matris, sürüm, azure, devops
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 03/25/2018
ms.topic: article
ms.openlocfilehash: 011cb173ffdecc7a22c2e470209719ccaf6bda58
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "30834155"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible modülü ve sürüm Matrisi

## <a name="ansible-modules-for-azure"></a>Azure için Ansible modülleri
Ansible doğrudan uzak ana bilgisayarların veya playbooks aracılığıyla yürütülen modül sayısı ile birlikte gelir.
Bu makalede, Azure bulut kaynakları gibi sanal makine, ağ ve kapsayıcı hizmetlerini sağlamak Azure Ansible modüllerini listeler. Bu modüller Ansible resmi sürümünü veya Microsoft tarafından yayımlanan aşağıdaki playbook rolleri alabilirsiniz.

| Azure için Ansible Modülü                   |  Ansible 2.4 |  Ansible 2,5 |  Playbook Role [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|-----------------------------|-------------------------------------| 
| **İşlem**                    |           |                          |                                  | 
| azure_rm_availabilityset                    | Evet          | Evet                         | Evet                                 | 
| azure_rm_availabilityset_facts              | Evet          | Evet                         | Evet                                 | 
| azure_rm_deployment                         | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualmachineimage_facts          | Evet          | Evet                         | Evet                                 | 
| azure_rm_resourcegroup                      | Evet          | Evet                         | Evet                                 | 
| azure_rm_resourcegroup_facts                | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualmachine                     | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualmachine_extension           | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualmachine_scaleset            | Evet          | Evet                         | Evet                                 | 
| azure_rm_image                              |              | Evet                         | Evet                                 | 
| **Ağ**                    |           |                          |                                  | 
| azure_rm_virtualnetwork                     | Evet          | Evet                         | Evet                                 | 
| azure_rm_virtualnetwork_facts               | Evet          | Evet                         | Evet                                 | 
| azure_rm_subnet                             | Evet          | Evet                         | Evet                                 | 
| azure_rm_networkinterface                   | Evet          | Evet                         | Evet                                 | 
| azure_rm_networkinterface_facts             | Evet          | Evet                         | Evet                                 | 
| azure_rm_publicipaddress                    | Evet          | Evet                         | Evet                                 | 
| azure_rm_publicipaddress_facts              | Evet          | Evet                         | Evet                                 | 
| azure_rm_dnsrecordset                       | Evet          | Evet                         | Evet                                 | 
| azure_rm_dnsrecordset_facts                 | Evet          | Evet                         | Evet                                 | 
| azure_rm_dnszone                            | Evet          | Evet                         | Evet                                 | 
| azure_rm_dnszone_facts                      | Evet          | Evet                         | Evet                                 | 
| azure_rm_loadbalancer                       | Evet          | Evet                         | Evet                                 | 
| azure_rm_loadbalancer_facts                 | Evet          | Evet                         | Evet                                 | 
| azure_rm_appgw                              | -            | -                           | Evet                                 | 
| azure_rm_appgwroute                         | -            | -                           | Evet                                 | 
| azure_rm_appgwroute                         | -            | -                           | Evet                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | Evet                                 |
| azure_rm_appgwroutetable                    | -            | -                           | Evet                                 |
| azure_rm_securitygroup                      | Evet          | Evet                         | Evet                                 | 
| azure_rm_appgwroutetable_facts              | -            | -                           | Evet                                 | 
| **Depolama**                    |           |                          |                                  | 
| azure_rm_storageaccount                     | Evet          | Evet                         | Evet                                 | 
| azure_rm_storageaccount_facts               | Evet          | Evet                         | Evet                                 | 
| azure_rm_storageblob                        | Evet          | Evet                         | Evet                                 | 
| azure_rm_managed_disk                       | Evet          | Evet                         | Evet                                 | 
| azure_rm_managed_disk_facts                 | Evet          | Evet                         | Evet                                 | 
| **Kapsayıcılar**                    |           |                          |                                  | 
| azure_rm_acs                                | Evet          | Evet                         | Evet                                 | 
| azure_rm_containerinstance                  | -            | Evet                         | Evet                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | Evet                                 | 
| azure_rm_containerregistry                  | -            | Evet                         | Evet                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | Evet                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | Evet                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | Evet                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | Evet                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | Evet                                 | 
| **Azure İşlevleri**                    |           |                          |                                  | 
| azure_rm_functionapp                        | Evet          | Evet                         | Evet                                 | 
| azure_rm_functionapp_facts                  | Evet          | Evet                         | Evet                                 | 
| **Veritabanları**                    |           |                          |                                  | 
| azure_rm_sqlserver                          | -            | Evet                         | Evet                                 | 
| azure_rm_sqlserver_facts                    | -            | Evet                         | Evet                                 | 
| azure_rm_sqldatabase                        | -            | Evet                         | Evet                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | Evet                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | Evet                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | Evet                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | Evet                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | Evet                                 | 
| azure_rm_mysqlserver                        | -            | Evet                         | Evet                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | Evet                                 | 
| azure_rm_mysqldatabase                      | -            | Evet                         | Evet                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | Evet                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | Evet                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | Evet                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | Evet                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | Evet                                 | 
| azure_rm_postgresqlserver                   | -            | Evet                         | Evet                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | Evet                                 | 
| azure_rm_postgresqldatabase                 | -            | Evet                         | Evet                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | Evet                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | Evet                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | Evet                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | Evet                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | Evet                                 | 
| **Anahtar Kasası**                    |           |                          |                                  | 
| azure_rm_keyvault                           | -            | Evet                         | Evet                                 |
| azure_rm_keyvault_facts                     | -            | -                           | Evet                                 |
| azure_rm_keyvaultkey                        | -            | Evet                         | Evet                                 |
| azure_rm_keyvaultsecret                     | -            | Evet                         | Evet                                 |


## <a name="introduction-to-playbook-role-for-azure"></a>Azure playbook rolü için giriş
[Azure_preview_module playbook rol](https://galaxy.ansible.com/Azure/azure_preview_modules/) en eksiksiz rolü ve tüm son Azure modüller içerir. Güncelleştirmeleri ve hata düzeltmeleri resmi Ansible yayın'den daha güncel bir şekilde yapılır. Azure kaynak amacıyla sağlama Ansible kullanırsanız, azure_preview_module rolü yüklemek için önerilir.

Azure_preview_module playbook rol üç haftada yayımlanır.

## <a name="next-steps"></a>Sonraki adımlar
Playbook rollere ilgili daha fazla bilgi [oluşturma yeniden kullanılabilir Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
