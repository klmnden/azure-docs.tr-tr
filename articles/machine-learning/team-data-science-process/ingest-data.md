---
title: Analiz için Azure depolama ortamlara veri yükleme | Microsoft Docs
description: Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 369b06ee22b30977affb3642f0d321e5d92e6c41
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme
Takım veri bilimi işlemi veri veya alınan işlenen ya da işlemin her aşamasında en uygun şekilde analiz için farklı depolama ortamları çeşitli yüklenen olmasını gerektirir. İşleme için yaygın olarak kullanılan veri hedefleri Azure Blob Storage, SQL Azure veritabanının SQL Server Azure VM, Hdınsight (Hadoop) ve Azure Machine Learning içerir. 

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

Bu **menü** bunları veri alma nasıl açıklayan konulara bağlantılar hedef burada veri depolanabilir ve işlenebilir ortamları.

Teknik ve iş gereksinimlerinize yanı sıra, ilk konum biçimlendirin ve içine veri çözümleme hedeflerinize ulaşmak için alınan gerekiyor hedef ortamları verilerinizi boyutunu belirler. Tahmine dayalı bir modeli oluşturmak için gereken görevler çeşitli elde etmek için çeşitli ortamlar arasında taşınacak veri gerektirecek şekilde bir senaryo için seyrek değil. Bu görev dizisi, örneğin, veri keşfi, ön işleme, temizleme, aşağı örnekleme ve model eğitim içerebilir.

