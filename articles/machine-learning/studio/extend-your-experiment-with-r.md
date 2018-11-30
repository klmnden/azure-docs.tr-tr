---
title: R - Azure Machine Learning Studio ile denemenizi genişletme | Microsoft Docs
description: Azure Machine Learning Studio'da R dili ile R betiği yürütme modülünü kullanarak genişletmek nasıl.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: df6453f1cf6e8b372239d528067a7258d645518d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311017"
---
# <a name="azure-machine-learning-studio-extend-your-experiment-with-r"></a>Azure Machine Learning Studio: R ile denemenizi genişletme 
Kullanarak Azure Machine Learning Studio'da R diliyle işlevselliğini genişletebilirsiniz [R betiği yürütme] [ execute-r-script] modülü.

Bu modül, birden çok giriş veri kümeleri kabul eder ve tek bir veri kümesi çıktı olarak verir. Bir R betiği yazabilirsiniz **R betiği** parametresinin [R betiği yürütme] [ execute-r-script] modülü.

Her giriş bağlantı noktasına modülü, aşağıdakine benzer kod kullanarak erişin:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Şu anda yüklü tüm paketlerin listesi
Yüklü paketler listesini değiştirebilirsiniz. Şu anda yüklü paketler listesini bulunabilir [R paketleri Azure Machine Learning Studio tarafından desteklenen](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Aşağıdaki kodu girerek yüklü paketleri tam, güncel listesini alabilirsiniz [R betiği yürütme] [ execute-r-script] Modülü:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Bu çıkış bağlantı noktasına paketleri listesi gönderir [R betiği yürütme] [ execute-r-script] modülü.
Connect gibi bir dönüştürme modülü paket listesini görüntülemek için [CSV'ye Dönüştür] [ convert-to-csv] sol çıktısına [R betiği yürütme] [ execute-r-script] Modülü denemeyi çalıştırın ve sonra dönüştürme modülün çıkışına tıklayın ve seçin **indirme**. 

!["CSV dönüştürme" modülü çıkışı indirme](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Paketleri içeri aktarma
Aşağıdaki komutları kullanarak henüz yüklenmemişse paketleri içeri aktarabilirsiniz [R betiği yürütme] [ execute-r-script] Modülü:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Burada `my_favorite_package.zip` paketinizi dosyası içerir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
