---
title: Azure kapsayıcı durumlarda çalışan kapsayıcılar komutları yürütün
description: Bilgi nasıl Azure kapsayıcı örnekleri şu anda çalışan bir kapsayıcıda komut yürütme
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.openlocfilehash: 43211f620efb16cbcd722d3d386b1bb862fc6280
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Bir çalışan Azure kapsayıcı örneğinde komut yürütme

Azure kapsayıcı örnekleri, bir komut çalışan bir kapsayıcıda yürütülmesini destekler. Bir komut zaten başlatılmış bir kapsayıcıda çalışan uygulama geliştirme ve sorun giderme sırasında özellikle yardımcı olur. Böylece sorunları çalışan bir kapsayıcıda ayıklayabilirsiniz etkileşimli bir kabuk başlatmak için bu özelliğin en yaygın kullanımı içindir.

## <a name="run-a-command-with-azure-cli"></a>Azure CLI ile bir komutu çalıştırın

İle çalışan bir kapsayıcıda komut yürütme [az kapsayıcı exec] [ az-container-exec] içinde [Azure CLI][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Örneğin, bir Nginx kapsayıcısı Bash kabuğunda başlatmak için şunu yazın:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

Aşağıdaki örnek çıktıda Bash kabuğunda, bir terminal sağlayan bir çalışan Linux kapsayıcıda başlatılan `ls` yürütülür:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

Bu örnekte, bir çalışan Nanoserver kapsayıcısında komut istemi başlatılır:

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

Varsa, [kapsayıcı grubu](container-instances-container-groups.md) komutu çalıştırmak bir kapsayıcı adı belirtin, bir uygulama kapsayıcısı ve bir günlük sepet gibi birden çok kapsayıcı `--container-name`.

Örneğin, kapsayıcı grubundaki *mynginx* iki kapsayıcılar olan *nginx uygulama* ve *Günlükçü*. Üzerinde bir kabuk başlatmak için *nginx uygulama* kapsayıcı:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Kısıtlamalar

Azure kapsayıcı örnekleri şu anda destekleyen tek bir işlem başlatılıyor [az kapsayıcı exec][az-container-exec], ve komut bağımsız değişkenlerini geçiremezsiniz. Örneğin, komutları gibi zincirleme yükleyemezsiniz `sh -c "echo FOO && echo BAR"`, veya yürütme `echo FOO`.

## <a name="next-steps"></a>Sonraki adımlar

Diğer sorun giderme araçları ve ortak dağıtım sorunlar hakkında bilgi edinin [Azure kapsayıcı örnekleri de kapsayıcı ve dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure