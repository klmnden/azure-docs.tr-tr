---
title: "Azure portalını kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Docs"
description: "Bir Data Lake Analytics hesabı oluşturmak, U-SQL'yi kullanarak Data Lake Analytics işi oluşturmak ve bu işi göndermek için Azure portalının nasıl kullanılacağını öğrenin. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 2722a2d72ed90ea0005362563ecaee30750c040a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Azure portalının nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce bir **Azure aboneliğinizin** olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Şimdi aynı anda bir Data Lake Analytics ve Data Lake Store hesabı oluşturun.  Bu basit bir adımdır ve tamamlanması yaklaşık olarak 60 saniye sürer.

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. **Yeni** >  **Veri + Analiz** > **Data Lake Analytics**’e tıklayın.
3. Aşağıdaki öğeler için değerleri seçin:
   * **Ad**: Data Lake Analytics hesabınızı adlandırın (Yalnızca küçük harf ve sayı kullanılabilir).
   * **Abonelik**: Analytics hesabı için kullanılan Azure aboneliğini seçin.
   * **Kaynak Grubu**. Var olan bir Azure Kaynak Grubu'nu seçin veya yeni bir grup oluşturun.
   * **Konum**. Data Lake Analytics hesabı için bir Azure veri merkezi seçin.
   * **Data Lake Store**: Yeni bir Data Lake Store hesabı oluşturmaya yönelik yönergeyi uygulayın veya var olan bir hesabı seçin. 
4. İsteğe bağlı olarak Data Lake Analytics hesabınıza yönelik bir fiyatlandırma katmanı seçebilirsiniz.
5. **Oluştur**'a tıklayın. 


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

1. Data Lake Analytics hesabında **Yeni İş**'e tıklayın.
2. Yukarıda gösterilen U-SQL betiğinin metnine yapıştırın. 
3. **İşi Gönder**'e tıklayın.   
4. İş durumu **Başarılı** olarak değiştirilene kadar bekleyin.
5. İş başarısız olduysa bkz. [Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
6. **Çıkış** sekmesine ve ardından `data.csv` öğesine tıklayın. 

## <a name="see-also"></a>Ayrıca bkz.

* U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
