---
title: Azure Data Factory ile birden çok kapsayıcı dosyaları kopyalama | Microsoft Docs
description: Azure Data Factory ile birden çok kapsayıcı dosyaları kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: aa5f32594c295ab6a8e60af8359370f64f75a72d
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967650"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Azure Data Factory ile birden çok kapsayıcı dosyaları Kopyala

Bu makalede açıklanan çözüm şablonu, birden çok dosya, kapsayıcıları veya demet dosya depolama alanları arasında dosya kopyalamak için yardımcı olur. Örneğin, belki de veri gölü, AWS S3'ten Azure Data Lake Store için geçirmek istediğiniz. Veya belki de her şeyi bir Azure Blob Depolama hesabından başka bir Azure Blob Depolama hesabına çoğaltmak istediğiniz. Bu şablon, bu kullanım için tasarlanmıştır.

Tek kapsayıcı ya da demet dosyaları kopyalamak isterseniz kullanmak daha verimli olur **kopyalama veri aracı** tek bir kopyalama etkinlikli bir işlem hattı oluşturursunuz. Bu şablon çalışması için bu basit kullanmanız daha büyük.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, kaynak depolama deposu kapsayıcılardan numaralandırır ve hedef deponun kopyasını her kaynak depolama kapsayıcılardan depolayın. 

Şablon üç etkinlikleri içerir:
-   A **GetMetadata** , kaynak depolama deposu tarama ve kapsayıcı listesini almak için etkinlik.
-   A **ForEach** kapsayıcı listeden almak için etkinlik **GetMetadata** etkinlik ve liste üzerinde yineleme yapmak ve her kapsayıcı kopyalama etkinliğine geçirin.
-   A **kopyalama** her kapsayıcı kaynak depolama deposundan hedef deposuna kopyalamak için etkinlik.

Şablon iki parametre tanımlar:
-   Parametre *SourceFilePath* nerede alabilirsiniz kapsayıcıları veya demet listesini veri kaynağı deposu yolu. Çoğu durumda, birden çok kapsayıcı klasörleri içeren kök dizin yoludur. Bu parametrenin varsayılan değeri `/`.
-   Parametre *DestinationFilePath* dosyaların kopyalanacağı hedef deponuzda yoludur. Bu parametrenin varsayılan değeri `/`.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu kullanma

1. Şablon Git **birden çok dosya kapsayıcı dosya depoları arasında kopyalama**, oluşturup bir **yeni bağlantı** kaynak depolama deponuza. Kaynak depolama deposunda birden fazla kapsayıcılar veya demet dosyalarını kopyalamak istediğiniz yerdir.

    ![Kaynak için yeni bir bağlantı oluşturun](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Oluşturma bir **yeni bağlantı** hedef depolama deponuza.

    ![Hedef için yeni bir bağlantı oluşturun](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Tıklayın **bu şablonu kullan**.

    ![Bu şablonu kullan](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Aşağıdaki örnekte gösterildiği gibi panelinde kullanılabilir işlem hattı görürsünüz:

    ![İşlem hattı Göster](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Tıklayın **hata ayıklama**, giriş **parametreleri** tıklatıp **son**.

    ![İşlem hattını çalıştırma](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Sonucu gözden geçirin.

    ![Sonucu gözden geçir](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
