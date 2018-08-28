---
title: Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
description: Azure portalı kullanarak Azure Data Lake Analytics hesabı oluşturun ve bir U-SQL işi gönderin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.topic: conceptual
ms.date: 03/21/2017
ms.openlocfilehash: 0830f4a6cffb8c1b2dacc8452e52477bb3a3ec91
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43039377"
---
# <a name="get-started-with-azure-data-lake-analytics-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Bu makale, Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Azure portalın nasıl kullanılacağını açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce bir **Azure aboneliğinizin** olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Şimdi aynı anda bir Data Lake Analytics ve Data Lake Store hesabı oluşturun.  Bu basit bir adımdır ve tamamlanması yaklaşık olarak 60 saniye sürer.

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. **Kaynak oluştur** >  **Veri ve Analiz** > **Data Lake Analytics** seçeneğine tıklayın.
3. Aşağıdaki öğeler için değerleri seçin:
   * **Ad**: Data Lake Analytics hesabınızı adlandırın (Yalnızca küçük harf ve sayı kullanılabilir).
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun.
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin.
   * **Data Lake Store**: Yeni bir Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin. 
4. İsteğe bağlı olarak Data Lake Analytics hesabınıza yönelik bir fiyatlandırma katmanı seçebilirsiniz.
5. **Oluştur**’a tıklayın. 


## <a name="your-first-u-sql-script"></a>İlk U-SQL betiğiniz

Aşağıda basit bir U-SQL betiği gösterilmiştir. Betik içinde küçük bir veri kümesinin tanımlanmasına ve ardından söz konusu veri kümesinin varsayılan Data Lake Store'a `/data.csv` adlı bir dosya olarak yazılmasına yarar.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>U-SQL işi gönderme

1. Data Lake Analytics hesabında **Yeni İş**'i seçin.
2. Yukarıdaki U-SQL betiğinin metnine yapıştırın. İşin adı. 
3. İşi başlatmak için **Gönder** düğmesini seçin.   
4. İşin **Durumunu** izleyin ve iş durumu **Başarılı** olana kadar bekleyin.
5. **Veri** sekmesini, ardından **Çıkışlar** sekmesini seçin. `data.csv` adlı çıkış dosyasını seçin ve çıktı verilerini görüntüleyin.

## <a name="see-also"></a>Ayrıca bkz.

* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
