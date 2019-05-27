---
title: DC/OS CLI’yi yükleme | Microsoft Belgeleri
description: DC/OS CLI’yi yükleyin.
services: container-service
documentationcenter: ''
author: rgardler
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: Kapsayıcılar, Mikro hizmetler, DC/OS, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: e2601d5200f0b8ebcfb856bffb871f01b3acb215
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66148940"
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