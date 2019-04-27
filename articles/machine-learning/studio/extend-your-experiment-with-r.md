---
title: R ile denemenizi genişletme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da R dili ile R betiği yürütme modülünü kullanarak genişletmek nasıl.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 8c1292d0d36874892a286d91b1e367c7336b99aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60811440"
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
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Paketleri içeri aktarma
Aşağıdaki komutları kullanarak henüz yüklenmemişse paketleri içeri aktarabilirsiniz [R betiği yürütme] [ execute-r-script] Modülü:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Burada `my_favorite_package.zip` paketinizi dosyası içerir.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
