---
title: "Veri içeri aktarma bir dosyadan Azure Machine Learning Studio'ya | Microsoft Docs"
description: "Sabit eğitim veri dosyasından Azure Machine Learning Studio'ya karşıya nasıl yükleneceğini öğrenin. Bu çalışma alanında bir veri kümesi modül oluşturur."
keywords: "veriler, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 67f66f9b8703f2cab93a2274a90c161a55848c34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Eğitim verileri sabit sürücünüze Machine Learning Studio üzerindeki bir dosyadan içeri aktarma
[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Azure Machine Learning Studio'da eğitim verilerini olarak kullanılacak sabit sürücünüzden veri dosyasını karşıya yükle öğrenin. Veri dosyasını içeri aktararak, çalışma alanınızda kullanıma hazır bir veri kümesi modülü vardır.

## <a name="steps-to-import-data-from-a-local-file"></a>Yerel bir dosyadan veri almak için adımlar
Yerel bir sabit sürücüden veri almak için aşağıdakileri yapın:

1. Tıklatın **+ yeni** Machine Learning Studio penceresinin alt.
2. Seçin **DATASET** ve **yerel DOSYADAN**.
3. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, karşıya yüklemek istediğiniz dosyasına gidin
4. Bir ad girin, veri türünü tanımlayın ve isteğe bağlı olarak bir açıklama girin. Bir açıklama önerilen - verileri gelecekte kullanırken anımsamak istediğiniz verileri hakkında hiçbir özellikleri kaydetmenize olanak sağlar.
5. Onay kutusunu **bu var olan bir dataset yeni sürümüdür** , var olan bir dataset yeni verilerle güncelleştirmenizi sağlar. Bu onay kutusuna tıklayın ve ardından var olan bir veri kümesinin adını girin.

![Yeni bir veri kümesi karşıya yükle](./media/import-data-from-local-file/upload-dataset.png)

Karşıya yükleme sırasında dosyanızı karşıya yüklenen bir ileti görürsünüz. Karşıya yükleme zamanı bağlıdır, verilerin boyutunu ve bağlantınızın hızına hizmet. Dosya uzun sürmesi biliyorsanız, beklerken Machine Learning Studio içinde başka şeyler yapabilirsiniz. Ancak, tarayıcının kapanması verileri karşıya yükleme başarısız olmasına neden olur.

## <a name="dataset-module-is-ready-for-use"></a>Kullanıma hazır olduğundan veri kümesi Modülü
Verileriniz yüklendikten sonra bir veri kümesi modülünde depolanır ve tüm deneme çalışma alanınızdaki kullanılabilir.

Bir deneme düzenlerken, oluşturduğunuz veri kümeleri bulabilirsiniz **My veri kümeleri** altında listesinde **kaydedilen veri kümeleri** modül paletindeki listesi. Daha fazla analizi ve makine öğrenme için veri kümesini kullanmak istediğinizde veri kümesini deneme tuvale sürükleyip bırakabilirsiniz.
