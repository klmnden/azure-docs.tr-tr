---
title: Azure'da veri bilimi ortamlarını ayarlama | Microsoft Docs
description: Veri bilimi ortamlarını Team Data Science Process içinde kullanım için Azure üzerinde ayarlayın.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 6c28a64830afeb19c6a9264888b296c3b99990d1
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39262574"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Team Data Science Process içinde kullanım için veri bilimi ortamlarını ayarlama
Team Data Science Process, depolama, işleme ve veri analizi için çeşitli veri bilimi ortamlarını kullanır. Bunlar, Azure Blob Depolama, birden fazla Azure sanal makineleri, HDInsight (Hadoop) kümeleri ve Azure Machine Learning çalışma alanlarını içerir. Kullanmak için hangi ortamı modellenmesi için türü ve veri miktarını bağlıdır ve buluttaki veriler için hedef hakkında karar. 

* Bu karar verirken dikkate almanız gereken sorular hakkında yönergeler için bkz [planlama bilgisayarınızı Azure Machine Learning veri bilimi ortamını](plan-your-environment.md). 
* Bazı gelişmiş analiz yaparken çalıştırdığınızca senaryolarını kataloğu için bkz: [Team Data Science Process için senaryolar](plan-sample-scenarios.md)

Team Data Science Process tarafından kullanılan çeşitli veri bilimi ortamlarını ayarlama nasıl açıklayan konulara bu menü bağlar.

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

**Microsoft Veri bilimi sanal makinesi (DSVM)** bir Azure sanal makine (VM) görüntüsü olarak kullanılabilir. Bu VM önceden yüklenmiş ve yapılandırılmış veri analizi ve makine öğrenimi için yaygın olarak kullanılan çeşitli popüler araçlarla. DSVM, hem Windows hem de Linux üzerinde kullanılabilir. Daha fazla bilgi için [Linux ve Windows için bulut tabanlı veri bilimi sanal makinesi giriş](../data-science-virtual-machine/overview.md).

Oluşturmayı öğrenin:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [Derin öğrenme](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
