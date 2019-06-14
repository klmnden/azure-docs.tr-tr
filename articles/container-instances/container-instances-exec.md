---
title: Azure Container Instances'da kapsayıcı çalıştırma komutları yürütün.
description: Bilgi nasıl Azure Container Instances'da çalışmakta olan bir kapsayıcıdaki bir komut yürütün
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: danlep
ms.openlocfilehash: 577e2386c352798bc21a2c78b22726128ac7cf0a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60579755"
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Bir çalışan Azure container Instance üzerinde bir komut yürütün

Azure Container Instances, çalışan bir kapsayıcıda bir komutun yürütülmesini destekler. Zaten başlatılmış bir kapsayıcıda çalıştırarak uygulama geliştirme ve sorun giderme sırasında özellikle yardımcı olur. Bu özelliğin en yaygın kullanımı bir çalışmakta olan kapsayıcıyı sorunları hatalarını ayıklayabilir, etkileşimli bir kabuk başlatılmasıdır.

## <a name="run-a-command-with-azure-cli"></a>Azure CLI ile bir komut çalıştırın

İle çalışan bir kapsayıcıdaki bir komut yürütün [az container exec] [ az-container-exec] içinde [Azure CLI][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Örneğin, bir Bash kabuğunda Ngınx kapsayıcısı başlatmak için şunu yazın:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

Aşağıdaki örnek çıktıda bir terminalde sağlayan bir çalışan Linux kapsayıcısı içinde Bash kabuğunu başlatılan `ls` çalıştırılır:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

Bu örnekte, bir çalışan Nanoserver kapsayıcıda komut istemi başlatılır:

```console
$ az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Birden çok kapsayıcılı gruplar

Varsa, [kapsayıcı grubu](container-instances-container-groups.md) komutu çalıştırmak için kapsayıcının adını belirtin, bir uygulama kapsayıcısı ve günlüğe kaydetme sepet, gibi birden çok kapsayıcı `--container-name`.

Örneğin, kapsayıcı grubundaki *mynginx* iki kapsayıcı *ngınx uygulamasını* ve *Günlükçü*. Üzerinde bir Shell'i başlatmak için *ngınx uygulamasını* kapsayıcı:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Kısıtlamalar

Azure Container Instances şu anda destekleyen tek bir işlem başlatma [az container exec][az-container-exec], ve komut satırı bağımsız değişkenlerini geçirilemez. Örneğin, komutları gibi zincirleme yükleyemezsiniz `sh -c "echo FOO && echo BAR"`, veya yürütme `echo FOO`.

## <a name="next-steps"></a>Sonraki adımlar

Diğer sorun giderme araçları ve sık karşılaşılan dağıtım sorunlarını hakkında [Azure Container ınstances'da kapsayıcı ve dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure