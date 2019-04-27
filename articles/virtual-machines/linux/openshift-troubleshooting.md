---
title: Azure'da OpenShift dağıtım sorunlarını giderme | Microsoft Docs
description: Azure'da OpenShift dağıtım sorunlarını giderin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: af6746e7246b8783e5bdbef34cf1b57427aa7ebb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60771286"
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>Azure'da OpenShift dağıtım sorunlarını giderme

OpenShift kümesi başarıyla değil dağıtıyorsanız, Azure portalında hata çıktısı sağlar. Çıkış sorunu tanımlamak zorlaştıran okunması zor olabilir. Bu çıktı, çıkış kodu 3, 4 veya 5 için hızlı tarama. Aşağıda, bu üç çıkış kodları bilgileri sağlar:

- Çıkış kodu 3: Red Hat aboneliğinizin kullanıcı adı / parola veya kuruluş kimliği / etkinleştirme anahtarı hatalı
- Çıkış kodu 4: Red Hat Havuz kimliği yanlış veya hiçbir destek hakları vardır
- Çıkış kodu 5: Docker ölçülü kaynak havuzu birimini açamadığından

Diğer tüm çıkış kodları için konaklarında ssh günlük dosyalarını görüntülemek için bağlanın.

**OpenShift kapsayıcı platformu**

SSH ansible playbook konağa. Kale ana bilgisayarı, şablonu veya Market teklifi için kullanın. Savunma (ana, ınfra, CNS, işlem) kümedeki diğer tüm düğümler için SSH kullanabilirsiniz. Günlük dosyalarını görüntülemek için kök olması gerekir. Kök SSH erişim için SSH köküne diğer düğümlere kullanmayın için varsayılan olarak devre dışıdır.

**OKD**

SSH ansible playbook konağa. Ana-0 konak OKD şablonu (sürüm 3,9 ve öncesi) kullanın. Kale ana bilgisayarı OKD şablonu (sürüm 3.10 ve üstü) kullanın. Ansible playbook konaktan (ana, ınfra, CNS, işlem) kümedeki diğer tüm düğümler için SSH kullanabilirsiniz. Günlük dosyalarını görüntülemek için kök (sudo su-) olması gerekir. Kök SSH erişim için SSH köküne diğer düğümlere kullanmayın için varsayılan olarak devre dışıdır.

## <a name="log-files"></a>Günlük dosyaları

Konak hazırlama komut dosyaları için günlük dosyaları (stderr ve stdout) bulunan `/var/lib/waagent/custom-script/download/0` tüm konaklarda. Konak hazırlanırken bir hata oluştu, hata belirlemek için bu günlük dosyaları görüntüleyin.

Hazırlama betikleri başarıyla çalıştırılmışsa sonra günlük dosyalarını `/var/lib/waagent/custom-script/download/1` ansible playbook ana dizin incelenmesi gerekiyor. Stdout dosyası, OpenShift gerçek yüklenirken hata oluştu, hata görüntülenir. Daha fazla yardım için desteğe başvurmak için bu bilgileri kullanın.

Örnek çıktı

```json
TASK [openshift_storage_glusterfs : Load heketi topology] **********************
fatal: [mycluster-master-0]: FAILED! => {"changed": true, "cmd": ["oc", "--config=/tmp/openshift-glusterfs-ansible-IbhnUM/admin.kubeconfig", "rsh", "--namespace=glusterfs", "deploy-heketi-storage-1-d9xl5", "heketi-cli", "-s", "http://localhost:8080", "--user", "admin", "--secret", "VuoJURT0/96E42Vv8+XHfsFpSS8R20rH1OiMs3OqARQ=", "topology", "load", "--json=/tmp/openshift-glusterfs-ansible-IbhnUM/topology.json", "2>&1"], "delta": "0:00:21.477831", "end": "2018-05-20 02:49:11.912899", "failed": true, "failed_when_result": true, "rc": 0, "start": "2018-05-20 02:48:50.435068", "stderr": "", "stderr_lines": [], "stdout": "Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2\n\tAllowing file volumes on cluster.\n\tAllowing block volumes on cluster.\n\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4\n\t\tAdding device /dev/sdd ... OK\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131\n\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdd ... OK\n\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f\n\t\tAdding device /dev/sde ... OK\n\t\tAdding device /dev/sdf ... OK\n\t\tAdding device /dev/sdd ... OK", "stdout_lines": ["Creating cluster ... ID: 794b285745b1c5d7089e1c5729ec7cd2", "\tAllowing file volumes on cluster.", "\tAllowing block volumes on cluster.", "\tCreating node mycluster-cns-0 ... ID: 45f1a3bfc20a4196e59ebb567e0e02b4", "\t\tAdding device /dev/sdd ... OK", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\tCreating node mycluster-cns-1 ... ID: 596f80d7bbd78a1ea548930f23135131", "\t\tAdding device /dev/sdc ... Unable to add device: Unable to execute command on glusterfs-storage-4zc42:   Device /dev/sdc excluded by a filter.", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdd ... OK", "\tCreating node mycluster-cns-2 ... ID: 42c0170aa2799559747622acceba2e3f", "\t\tAdding device /dev/sde ... OK", "\t\tAdding device /dev/sdf ... OK", "\t\tAdding device /dev/sdd ... OK"]}

PLAY RECAP *********************************************************************
mycluster-cns-0       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-1       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-cns-2       : ok=146  changed=57   unreachable=0    failed=0   
mycluster-infra-0     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-1     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-infra-2     : ok=143  changed=55   unreachable=0    failed=0   
mycluster-master-0    : ok=502  changed=198  unreachable=0    failed=1   
mycluster-master-1    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-master-2    : ok=348  changed=140  unreachable=0    failed=0   
mycluster-node-0      : ok=143  changed=55   unreachable=0    failed=0   
mycluster-node-1      : ok=143  changed=55   unreachable=0    failed=0   
localhost                  : ok=13   changed=0    unreachable=0    failed=0   

INSTALLER STATUS ***************************************************************
Initialization             : Complete (0:00:39)
Health Check               : Complete (0:00:24)
etcd Install               : Complete (0:01:24)
Master Install             : Complete (0:14:59)
Master Additional Install  : Complete (0:01:10)
Node Install               : Complete (0:10:58)
GlusterFS Install          : In Progress (0:03:33)
    This phase can be restarted by running: playbooks/openshift-glusterfs/config.yml

Failure summary:

  1. Hosts:    mycluster-master-0
     Play:     Configure GlusterFS
     Task:     Load heketi topology
     Message:  Failed without returning a message.
```

Yükleme sırasında en yaygın hatalar şunlardır:

1. Parola özel anahtarı yok
2. Özel anahtar ile anahtar kasası gizli dizi doğru biçimde oluşturulmadıysa
3. Hizmet sorumlusu kimlik bilgileri yanlış girilmiş
4. Hizmet sorumlusu, kaynak grubuna katkıda bulunan erişimi yok

### <a name="private-key-has-a-passphrase"></a>Özel anahtara sahip bir parola

İçin ssh izni bir hata görürsünüz. SSH özel anahtarı bir parola olup olmadığını denetlemek için ansible playbook konağa.

### <a name="key-vault-secret-with-private-key-wasnt-created-correctly"></a>Özel anahtar ile anahtar kasası gizli dizi doğru biçimde oluşturulmadıysa

Özel anahtar, ansible playbook ana bilgisayar - ~/.ssh/id_rsa kopyalanır. Bu dosyayı doğru olduğunu onaylayın. Ansible playbook konaktan küme düğümlerinden biri için bir SSH oturumu açıp test edin.

### <a name="service-principal-credentials-were-entered-incorrectly"></a>Hizmet sorumlusu kimlik bilgileri yanlış girilmiş

Şablon veya Market teklifi girişi sağlanırken hatalı bilgileri sağlanmadı. Hizmet sorumlusu doğru AppID (ClientID) ve parolayı (clientSecret) kullandığınızdan emin olun. Aşağıdaki azure CLI komutu göndererek doğrulayın.

```bash
az login --service-principal -u <client id> -p <client secret> -t <tenant id>
```

### <a name="service-principal-doesnt-have-contributor-access-to-the-resource-group"></a>Hizmet sorumlusu, kaynak grubuna katkıda bulunan erişimi yok

Azure bulut sağlayıcısı etkin olduğunda kullanılan hizmet sorumlusunun kaynak grubuna katkıda bulunan erişimi olması gerekir. Aşağıdaki azure CLI komutu göndererek doğrulayın.

```bash
az group update -g <openshift resource group> --set tags.sptest=test
```

## <a name="additional-tools"></a>Ek araçlar

Bazı hataları daha fazla bilgi için aşağıdaki komutları kullanabilirsiniz:

1. systemctl durumu \<hizmet >
2. journalctl -xe
