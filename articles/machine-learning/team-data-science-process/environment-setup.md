---
title: Azure'da veri bilimi ortamlarını ayarlama | Microsoft Docs
description: Veri bilimi ortamlarını Team Data Science Process içinde kullanım için Azure üzerinde ayarlayın.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/29/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: c8154400621fae719c7097e36c8e14d3f946d7c2
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445916"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Team Data Science Process içinde kullanım için veri bilimi ortamlarını ayarlama
Team Data Science Process, depolama, işleme ve veri analizi için çeşitli veri bilimi ortamlarını kullanır. Bunlar, Azure Blob Depolama, birden fazla Azure sanal makineleri, HDInsight (Hadoop) kümeleri ve Azure Machine Learning çalışma alanlarını içerir. Kullanmak için hangi ortamı modellenmesi için türü ve veri miktarını bağlıdır ve buluttaki veriler için hedef hakkında karar. 

* Bu karar verirken dikkate almanız gereken sorular hakkında yönergeler için bkz [planlama bilgisayarınızı Azure Machine Learning veri bilimi ortamını](plan-your-environment.md). 
* Bazı gelişmiş analiz yaparken çalıştırdığınızca senaryolarını kataloğu için bkz: [Team Data Science Process için senaryolar](plan-sample-scenarios.md)

Aşağıdaki makaleler Team Data Science Process tarafından kullanılan çeşitli veri bilimi ortamlarını ayarlama açıklanır.

* [Azure depolama hesabı](../../storage/common/storage-quickstart-create-account.md)
* [HDInsight (Hadoop) kümesi](customize-hadoop-cluster.md)
* [Azure Machine Learning Studio çalışma alanı](../studio/create-workspace.md)

**Microsoft Veri bilimi sanal makinesi (DSVM)** bir Azure sanal makine (VM) görüntüsü olarak kullanılabilir. Bu VM önceden yüklenmiş ve yapılandırılmış veri analizi ve makine öğrenimi için yaygın olarak kullanılan çeşitli popüler araçlarla. DSVM, hem Windows hem de Linux üzerinde kullanılabilir. Daha fazla bilgi için [Linux ve Windows için bulut tabanlı veri bilimi sanal makinesi giriş](../data-science-virtual-machine/overview.md).

Oluşturmayı öğrenin:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [Derin öğrenme](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
