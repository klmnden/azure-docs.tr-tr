---
title: "U-SQL işleri hata ayıklama | Microsoft Docs"
description: "Visual Studio kullanarak bir U-SQL başarısız köşe hata ayıklama öğrenin."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Hata ayıklama başarısız U-SQL işleri için kullanıcı tanımlı C# kodu

U-SQL kullanarak bir özel ayıklayıcısı veya reducer gibi işlevsellikler eklemek için kodunuzu yazabilirsiniz şekilde C# ' ta, genişletilebilirlik modeli sağlar. Daha fazla bilgi için bkz: [U-SQL Programlama Kılavuzu](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). Uygulamada hata ayıklama herhangi bir kod gerekebilir ve büyük veri sistemlerini yalnızca sınırlı çalışma zamanı hata ayıklama günlük dosyaları gibi bilgileri sağlayabilir.

Visual Studio için Azure Data Lake araçları adlı bir özelliği sağlar **köşe hata ayıklama başarısız**, olanak sağlayan, bulutta başarısız bir işi hata ayıklama için yerel makinenize klonlayın. Yerel kopya herhangi bir giriş veri ve kullanıcı kodu da dahil olmak üzere tüm bulut ortamı yakalar.

Aşağıdaki videoda Visual Studio için Azure Data Lake araçları Debug başarısız köşe gösterilmektedir.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Visual Studio zaten yüklü değilse aşağıdaki iki güncelleştirmelerden gerektirir: [Microsoft Visual C++ 2015 Redistributable güncelleştirme 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) ve [Evrensel C çalışma zamanı için Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-to-local-machine"></a>Yerel makineye yükleme başarısız köşe

Visual Studio için Azure Data Lake araçları başarısız bir işi açtığınızda, sarı bir uyarı çubuğu için hata sekmesini ayrıntılı hata iletileri ile bakın.

1. Tıklatın **karşıdan** tüm gerekli kaynakları ve giriş akışları karşıdan yüklemek için. Karşıdan yükleme tamamlanmazsa, tıklatın **yeniden**.

2. Tıklatın **açık** yerel hata ayıklama ortamı oluşturmak için indirme tamamlandıktan sonra. Hata ayıklama çözümü olan yeni bir Visual Studio örneği otomatik olarak oluşturulan ve açılır.

![Azure Data Lake Analytics U-SQL hata ayıklama visual studio indirme köşe](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

İşlerini arka plan kodu kaynak dosyaları veya kayıtlı derlemeler içerebilir ve bu iki türlerinin farklı hata ayıklama senaryolar vardır.

- [Arka plan kodu ile başarısız bir işi hata ayıklama](#debug-job-failed-with-code-behind)
- [Başarısız bir işi derlemeleri ile hata ayıklama](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Hata ayıklama işi arka plan kodu ile başarısız oldu

U-SQL işi başarısız oluyor ve iş kullanıcı kodu içeriyorsa (genellikle adlı `Script.usql.cs` U-SQL projesinde), kaynak kodu hata ayıklama çözüm içine alınır.  Buradan, Visual Studio hata ayıklama araçları (izleme, değişkenleri, vb.) sorunu gidermek için kullanabilirsiniz.

> [!NOTE]
> Hata ayıklama önce kontrol ettiğinizden emin olun **ortak dil çalışma zamanı özel durumları** özel ayarları penceresinde (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL hata ayıklama visual studio ayarı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Tuşuna **F5** arka plan kodu kodu çalıştırmak için. Bir özel durum tarafından durduruluncaya kadar çalışmayacaktır.

2. Açık `ADLTool_Codebehind.usql.cs` dosya ve kesme, tuşuna basarak **F5** adım adım kodun hatalarını ayıklamak için.

    ![Azure Data Lake Analytics U-SQL hata ayıklama özel durumu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Hata ayıklama işi derlemeleri ile başarısız oldu

U-SQL komut dosyanıza kayıtlı derlemeleri kullanırsanız, sistem kaynak kodu otomatik olarak alınamıyor. Bu durumda, el ile derlemeleri kaynak kodu dosyaları çözüme ekleyin.

### <a name="configure-the-solution"></a>Çözümünüzü yapılandırma

1. Sağ **çözüm 'VertexDebug' > Ekle > mevcut proje...**  derlemeler kaynak kodu bulun ve hata ayıklama çözüme proje ekleyin.

    ![Azure Data Lake Analytics U-SQL hata ayıklama proje ekleyin](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Sağ **LocalVertexHost > Özellikler** çözüm kopyalayıp **çalışma dizini** yolu.

3. Sağ **derleme kaynak kod projesi > Özellikleri**seçin **yapı** sekmesinde solda ve kopyalanan yolu olarak Yapıştır **çıkış > çıkış yolu**.

    ![Azure Data Lake Analytics U-SQL hata ayıklama pdb yolunu ayarlama](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Tuşuna **Ctrl + Alt + E**, denetleme **ortak dil çalışma zamanı özel durumları** özel ayarları penceresinde.

### <a name="start-debug"></a>Hata ayıklama Başlat

1. Sağ **derleme kaynak kod projesi > yeniden** çıkış .pdb dosyaları için `LocalVertexHost` çalışma dizini.

2. Tuşuna **F5** ve bir özel durum tarafından durduruluncaya kadar proje çalışır. Güvenle yok sayabilirsiniz aşağıdaki uyarı iletisi görebilirsiniz. Bu işlem birkaç dakika hata ayıklama ekranı için kadar sürebilir.

    ![Azure Data Lake Analytics U-SQL hata ayıklama visual studio uyarı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Kaynak kodunuz açın ve kesme, tuşuna basarak **F5** adım adım kodun hatalarını ayıklamak için.

Visual Studio hata ayıklama araçları (izleme, değişkenleri, vb.) sorunu gidermek için de kullanabilirsiniz.

> [!NOTE]
> Derleme kaynak kodunu projeyi güncelleştirilmiş .pdb dosyaları oluşturmak için kodu değiştirdikten sonra her zaman yeniden oluşturun.

Proje başarıyla tamamlarsa hata ayıklama sonra çıktı penceresinde aşağıdaki iletiyi gösterir:

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL hata ayıklama başarılı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>İş yeniden gönderin

Hata ayıklama tamamladıktan sonra başarısız işi yeniden gönderin.

1. Arka plan kodu çözümleriyle işleri için C# kodu arka plan kodu kaynak dosyaya kopyalayın (genellikle `Script.usql.cs`).
2. Derlemeler ile işleri için güncelleştirilmiş .dll derlemelerine ADLA veritabanınızı kaydedin:
    1. Sunucu Gezgini veya Bulut Gezgini'nden, genişletin **ADLA hesabı > veritabanları** düğümü.
    2. Sağ **derlemeleri** ve yeni .dll derlemeleriniz ADLA veritabanı ile kaydetme: ![Azure Data Lake Analytics U-SQL hata ayıklama derlemesi kaydetme](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. İşinizi yeniden gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL Programlama Kılavuzu](data-lake-analytics-u-sql-programmability-guide.md)
- [Azure Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Öğretici: Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
