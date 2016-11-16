---
title: "DC/OS CLI’yi yükleme | Microsoft Belgeleri"
description: "DC/OS CLI’yi yükleyin."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kapsayıcılar, Mikro hizmetler, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: e664ce9426a2852a35dfdade5d41a9ce8b37a3b7
ms.openlocfilehash: a8ea47f158c0d666340815d2e039995c7483257f


---
> [!NOTE]
> DC/OS tabanlı ACS kümeleriyle çalışmak içindir. Swarm tabanlı ACS kümeleri için bunu yapmaya gerek yoktur.
> 
> 

Önce, [DC/OS tabanlı ACS kümenize bağlanın](../articles/container-service/container-service-connect.md). Bunu yaptıktan sonra aşağıdaki komutlarla istemci makinenize DC/OS CLI’yi yükleyebilirsiniz:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Python’un eski bir sürümünü kullanıyorsanız, birkaç "InsecurePlatformWarnings" fark edebilirsiniz. Bunları güvenle yok sayabilirsiniz.

Kabuğunuzu yeniden başlatmadan başlamak için şunu çalıştırın:

```bash
source ~/.bashrc
```

Yeni kabukları başlattığınızda bu adım gerekmeyecektir.

Artık CLI’nin yüklü olduğunu doğrulayabilirsiniz:

```bash
dcos --help
```


<!--HONumber=Nov16_HO2-->


