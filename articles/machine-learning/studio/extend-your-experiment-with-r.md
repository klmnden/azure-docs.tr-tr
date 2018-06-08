---
title: R ile denemenizi genişletme | Microsoft Docs
description: R betiği yürütün modülünü kullanarak Azure Machine Learning Studio işlevselliğini R diliyle genişletmek nasıl.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
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
ms.openlocfilehash: 1f05119d94611df2e75afc3a56d9682d1149326c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834483"
---
# <a name="extend-your-experiment-with-r"></a>R ile denemenizi genişletme
Kullanarak Azure Machine Learning Studio işlevselliğini R diliyle genişletebilirsiniz [R betiği yürütün] [ execute-r-script] modülü.

Bu modül, birden çok giriş veri kümeleri kabul eder ve tek bir veri kümesini çıktı olarak üretir. Bir R betiği içine yazabilirsiniz **R betiği** parametresinin [R betiği yürütün] [ execute-r-script] modülü.

Her giriş bağlantı noktası modülün aşağıdakine benzer bir kod kullanarak erişebilirsiniz:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Şu anda yüklü olan tüm paketleri listeleme
Yüklü olan paketlerin listesini değiştirebilirsiniz. Şu anda yüklü olan paketlerin listesini bulunabilir [R paketleri Azure Machine Learning tarafından desteklenen](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Aşağıdaki kodu girerek yüklü paketleri tam, güncel listesini alabilirsiniz [R betiği yürütün] [ execute-r-script] Modülü:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Bu paket listesini çıkış bağlantı noktasına gönderir [R betiği yürütün] [ execute-r-script] modülü.
Connect gibi bir dönüştürme modülü paket listesini görüntülemek için [CSV'ye Dönüştür] [ convert-to-csv] sol çıktısı için [R betiği yürütün] [ execute-r-script] modülü denemeyi çalıştırın, çıktı dönüştürme modülü seçin ve ardından **karşıdan**. 

!["CSV Dönüştür" modülü çıktısını indirme](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Paket alma
Aşağıdaki komutları kullanarak henüz yüklenmemişse paketleri içeri aktarabilirsiniz [R betiği yürütün] [ execute-r-script] Modülü:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Burada `my_favorite_package.zip` dosyası paketinizi içerir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
