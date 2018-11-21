---
title: Verileri içeri aktarma bir dosyadan Azure Machine Learning Studio'ya | Microsoft Docs
description: Azure Machine Learning Studio'da eğitim veri dosyasına sabit sürücünüzden karşıya yüklemeyi öğrenin. Bu çalışma alanında bir veri kümesi modülü oluşturur.
keywords: verileri, veri biçimi, veri türleri, veri kaynakları, eğitim verilerini içeri aktarma
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 057142911d8179fac0ad3e47563a4f49a9ae8d60
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263868"
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Machine Learning Studio'ya sabit sürücünüzde bir dosyadan eğitim verilerini içeri aktarma

Azure Machine Learning Studio'da eğitim verilerini olarak kullanılacak sabit sürücünüzden bir veri dosyası karşıya yüklemeyi öğrenin. Veri dosyasını içeri aktararak, çalışma alanınızda kullanıma hazır bir veri kümesi modülü vardır.

## <a name="steps-to-import-data-from-a-local-file"></a>Yerel bir dosyadan veri içeri aktarma adımları
Bir yerel sabit sürücünüzden verileri içeri aktarmak için aşağıdakileri yapın:

1. Tıklayın **+ yeni** Machine Learning Studio penceresinin alt kısmındaki.
2. Seçin **veri KÜMESİ** ve **yerel DOSYADAN**.
3. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, karşıya yüklemek istediğiniz dosyaya göz atın
4. Bir ad girin, veri türünü tanımlar ve isteğe bağlı olarak bir açıklama girin. Bir açıklama önerilen - veri gelecekte kullanırken unutmayın istediğiniz veriler hakkında herhangi bir özelliği kaydetmenize olanak sağlar.
5. Onay kutusunu **bu var olan bir dataset yeni sürümüdür** , var olan bir dataset yeni veriler ile güncelleştirmenizi sağlar. Bu onay kutusuna tıklayın ve ardından var olan bir veri kümesinin adını girin.

![Yeni bir veri kümesi karşıya yükleme](./media/import-data-from-local-file/upload-dataset.png)

Karşıya yükleme sırasında dosyanız karşıya yükleniyor bir ileti görürsünüz. Karşıya yükleme, veri boyutu ve hizmete bağlantınızın hızına bağlı zaman. Dosya uzun sürmesi biliyorsanız beklerken Machine Learning Studio içinde başka şeyler yapabilirsiniz. Ancak, tarayıcının kapanması verileri karşıya yükleme başarısız olmasına neden olur.

## <a name="dataset-module-is-ready-for-use"></a>Veri kümesi modülü kullanıma hazırdır
Verilerinizi karşıya yüklendikten sonra bir veri kümesi modülde depolanır ve çalışma alanınızdaki tüm deneme sunulur.

Bir deney düzenlerken, oluşturduğunuz veri kümeleri bulabilirsiniz **My veri kümeleri** altında listesinde **kaydedilmiş veri kümeleri** modül paletindeki listesi. Daha fazla analiz ve makine öğrenimi için veri kümesini kullanmak istediğinizde veri kümesini deneme tuvaline sürükleyip bırakabilirsiniz.
