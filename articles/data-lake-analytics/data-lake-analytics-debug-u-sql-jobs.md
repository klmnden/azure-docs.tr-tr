---
title: Hata ayıklama başarısız Azure Data Lake U-SQL işleri için kullanıcı tanımlı C# kodu | Microsoft Docs
description: Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL başarısız köşe hata ayıklama öğrenin.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/31/2017
ms.author: yanacai
ms.openlocfilehash: b614583079347c2634f8d03531517d1d32c75132
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Hata ayıklama başarısız U-SQL işleri için kullanıcı tanımlı C# kodu

U-SQL C# kullanarak genişletilebilirlik modeli sağlar. U-SQL komut dosyaları, C# işlevleri çağırmak ve SQL benzeri tanımlayıcı dil desteği olmayan analitik işlevleri gerçekleştirmek kolaydır. U-SQL genişletilebilirliği için daha fazla bilgi için bkz: [U-SQL Programlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). 

Uygulamada, herhangi bir kod hata ayıklama gerekebilir, ancak özel kod bulut üzerinde dağıtılmış işlemle sınırlı günlük dosyalarıyla hata ayıklama zordur. [Visual Studio için Azure Data Lake Araçları](http://aka.ms/adltoolsvs) adlı bir özelliği sağlar **köşe hata ayıklama başarısız**, yardımcı olan, daha kolay oluşan hataları özel kodda hata ayıklama. U-SQL işi başarısız olduğunda, hizmet başarısız durumu tutar ve aracı, bulut hatası ortamı hata ayıklama için yerel makineye indirmek için yardımcı olur. Yerel Yükleme herhangi bir giriş veri ve kullanıcı kodu da dahil olmak üzere tüm bulut ortamı yakalar.

Aşağıdaki videoda Visual Studio için Azure Data Lake araçları Debug başarısız köşe gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio bu özelliği kullanmak için aşağıdaki iki güncelleştirmeleri gerektirir: [Microsoft Visual C++ 2015 Redistributable güncelleştirme 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) ve [Evrensel C çalışma zamanı için Windows](https://www.microsoft.com/download/details.aspx?id=50410).
>

## <a name="download-failed-vertex-to-local-machine"></a>Yerel makineye yükleme başarısız köşe

Visual Studio için Azure Data Lake araçları başarısız bir işi açtığınızda, sarı bir uyarı çubuğu için hata sekmesini ayrıntılı hata iletileri ile bakın.

1. Tıklatın **karşıdan** tüm gerekli kaynakları ve giriş akışları karşıdan yüklemek için. Karşıdan yükleme tamamlanmazsa, tıklatın **yeniden**.

2. Tıklatın **açık** yerel hata ayıklama ortamı oluşturmak için indirme tamamlandıktan sonra. Yeni bir hata ayıklama çözüm açılacak ve varolan varsa çözümü Visual Studio'da kaydetmek ve hata ayıklama önce kapatmak emin olun Lütfen açılır.

![Azure Data Lake Analytics U-SQL hata ayıklama visual studio indirme köşe](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>Hata ayıklama ortamı yapılandırma

> [!NOTE]
> Hata ayıklama önce kontrol ettiğinizden emin olun **ortak dil çalışma zamanı özel durumları** özel ayarları penceresinde (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL hata ayıklama visual studio ayarı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

Yeni başlatılan Visual Studio örneğinde olabilir veya kullanıcı tanımlı C# kaynak kodu bulamayabilir:

1. [Kaynak kodumu çözümde bulabilirsiniz](#source-code-is-included-in-debugging-solution)

2. [Kaynak kodumu çözümde bulunamıyor](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Kaynak kodu çözüm hata ayıklama dahil

C# kaynak kodu yakalanır iki durum vardır:

1. Kullanıcı kodu arka plan kod dosyasına tanımlanır (genellikle adlı `Script.usql.cs` bir U-SQL projesi içinde).

2. Kullanıcı kodu C# projesinde sınıf kitaplığı U-SQL uygulaması için tanımlanan ve derleme kayıtlı **hata ayıklama bilgisi**.

Kaynak kodu çözüme aldıysanız, Visual Studio hata ayıklama araçlarını (izleme, değişkenleri, vb.) sorunu gidermek için kullanabilirsiniz:

1. Tuşuna **F5** hata ayıklama başlatılamıyor. Kod bir özel durum tarafından durduruluncaya kadar çalışır.

2. Kaynak kodu dosyasını açın ve kesme, tuşuna basarak **F5** adım adım kodun hatalarını ayıklamak için.

    ![Azure Data Lake Analytics U-SQL hata ayıklama özel durumu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Kaynak kodu çözüm hata ayıklama dahil edilmeyen

Kullanıcı kodu arka plan kod dosyasına dahil edilmeyen veya derleme kaydetmedi **hata ayıklama bilgisi**, kaynak kodu otomatik olarak hata ayıklama çözümde dahil edilmeyen sonra. Bu durumda, kaynak kodu eklemek için ek adımlar gerekir:

1. Sağ **çözüm 'VertexDebug' > Ekle > mevcut proje...**  kaynak kodu derleme bulmak ve hata ayıklama çözüme proje eklemek için.

    ![Azure Data Lake Analytics U-SQL hata ayıklama proje ekleyin](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. İçin proje klasörü yolu **FailedVertexDebugHost** projesi. 

3. Sağ **eklenen derleme kaynak kodunu projeyi > Özellikleri**seçin **yapı** sekmesinde solda ve \bin\debug ile biten kopyalanan yolu yapıştırın **çıkış > çıkış yolu**. Son çıkış yolu benzer "<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\".

    ![Azure Data Lake Analytics U-SQL hata ayıklama pdb yolunu ayarlama](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Bu ayarları sonra ile hata ayıklamayı Başlat **F5** ve kesme noktaları. Visual Studio hata ayıklama araçları (izleme, değişkenleri, vb.) sorunu gidermek için de kullanabilirsiniz.

> [!NOTE]
> Derleme kaynak kodunu projeyi güncelleştirilmiş .pdb dosyaları oluşturmak için kodu değiştirdikten sonra her zaman yeniden oluşturun.

## <a name="resubmit-the-job"></a>İş yeniden gönderin

Proje başarıyla tamamlarsa hata ayıklama sonra çıktı penceresinde aşağıdaki iletiyi gösterir:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Azure Data Lake Analytics U-SQL hata ayıklama başarılı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Başarısız işi yeniden göndermek için:

1. Arka plan kodu çözümleriyle işleri için C# kodu arka plan kodu kaynak dosyaya kopyalayın (genellikle `Script.usql.cs`).

2. İşleri derlemeler ile hata ayıklama çözümü derleme kaynak kodu projeye sağ tıklayın ve Azure Data Lake kataloğunuzu güncelleştirilmiş .dll derlemelerine kaydedin.

3. U-SQL işi yeniden gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL Programlama Kılavuzu](data-lake-analytics-u-sql-programmability-guide.md)
- [Azure Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)
- [Olağan dışı tekrarlayan bir iş ile ilgili sorunları giderme](data-lake-analytics-data-lake-tools-debug-recurring-job.md)