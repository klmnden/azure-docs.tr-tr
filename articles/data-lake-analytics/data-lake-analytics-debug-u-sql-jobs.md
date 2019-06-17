---
title: Başarısız olan Azure Data Lake U-SQL işleri için kullanıcı tanımlı C# kodu hatalarını ayıklama
description: Bu makalede, Visual Studio için Azure Data Lake Araçları'nı kullanarak U-SQL başarısız köşenin hatalarını ayıklamak açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.topic: conceptual
ms.date: 11/30/2017
ms.openlocfilehash: 5417f66696191cebadc2af9c6d634419a0eb8e5b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615328"
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Başarısız olan U-SQL işleri için kullanıcı tanımlı C# kodu hatalarını ayıklama

U-SQL C# kullanarak bir genişletilebilirlik modeli sağlar. U-SQL betiklerini, C# işlevlerini ve bildirim temelli SQL benzeri dil desteği olmayan analiz işlevleri gerçekleştirmek kolaydır. U-SQL genişletilebilirliği için daha fazla bilgi için bkz. [U-SQL Programlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). 

Uygulamada, hata ayıklama herhangi bir kod gerekebilir, ancak bulutta özel kod ile dağıtılmış bir iş sınırlı günlük dosyalarıyla hata ayıklamak zordur. [Visual Studio için Azure Data Lake Araçları](https://aka.ms/adltoolsvs) adı verilen bir özellik sağlar **başarısız köşe Hatalarını Ayıkla**, yardımcı olan, daha kolay gerçekleşen hataları özel kodunuzda hata ayıklama. U-SQL işi başarısız olduğunda, hata durumu hizmeti kalmasını sağlar ve aracı bulut hatası ortamı hata ayıklama için yerel makinenize indirmek için yardımcı olur. Yerel yükleme, herhangi bir giriş veri ve kullanıcı kodu da dahil olmak üzere tüm bulut ortamı yakalar.

Aşağıdaki videoda başarısız köşe Hatalarını Ayıkla Azure Data Lake araçları, Visual Studio için gösterir.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio, bu özelliği kullanmak için aşağıdaki iki güncelleştirmeleri gerektirir: [Microsoft Visual C++ 2015 yeniden dağıtılabilir güncelleştirme 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) ve [için Windows Evrensel C çalışma zamanı](https://www.microsoft.com/download/details.aspx?id=50410).
>

## <a name="download-failed-vertex-to-local-machine"></a>Başarısız köşenin yerel makineye indirme

Visual Studio için Azure Data Lake araçları başarısız bir işi açtığınızda, bir hata sekmesinde ayrıntılı hata iletileri sarı uyarı çubuğuyla bakın.

1. Tıklayın **indirme** tüm gerekli kaynakları ve giriş akışları indirmek için. İndirme tamamlamazsa tıklayın **yeniden**.

2. Tıklayın **açık** bir yerel hata ayıklama ortamı oluşturmak için indirme tamamlandıktan sonra. Yeni hata ayıklama çözümü açılacak ve hizmetiniz zaten varsa çözüm Visual Studio'da hata ayıklama öncesinde kapatmak emin olun Lütfen açılır.

![Azure Data Lake Analytics U-SQL hata ayıklama visual Studio'yu indirme köşe](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>Hata ayıklama ortamını yapılandırma

> [!NOTE]
> Hata ayıklama öncesinde kontrol ettiğinizden emin olun **ortak dil çalışma zamanı özel durumları** özel durum Ayarları penceresinde (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL hata ayıklama visual Studio'yu ayarlama](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

Yeni başlatılan Visual Studio örneğinde olabilir veya kullanıcı tanımlı C# kaynak kodu bulamayabilir:

1. [Kaynak kodum çözümde bulabilirsiniz](#source-code-is-included-in-debugging-solution)

2. [Kaynak kodum çözümde bulunamıyor](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Kaynak kodu, hata ayıklama çözümü dahildir

C# kaynak kodu yakalanır iki durum vardır:

1. Kullanıcı kodu arka plan kod dosyasında tanımlanır (genellikle adlı `Script.usql.cs` bir U-SQL projesi içinde).

2. Kullanıcı kodu C# sınıf kitaplığı projesi U-SQL uygulaması için tanımlanan ve derleme ile kayıtlı **hata ayıklama bilgisi**.

Kaynak kodu çözüme aldıysanız, Visual Studio hata ayıklama araçları (izleme, değişkenler vb.) sorunu gidermek için kullanabilirsiniz:

1. Hata ayıklamaya başlamak için **F5**'e basın. Kod bir özel durumun durduruluncaya kadar çalışır.

2. Kaynak kodu dosyasını açın ve kesme noktaları, ayarlayın tuşuna **F5** kodda adım adım hata ayıklamak için.

    ![Azure Data Lake Analytics U-SQL hata ayıklama özel durumu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Kaynak kodu hata ayıklama çözümü dahil değildir

Kullanıcı kodu, arka plan kod dosyasında bulunmayan veya derleme ile kaydolamadı **hata ayıklama bilgisi**, sonra da kaynak kodu hata ayıklama çözüm otomatik olarak bulunmaz. Bu durumda, kaynak kodunuzu eklemek için ek adımlar gerekir:

1. Sağ **çözüm 'VertexDebug' > Ekle > var olan proje...**  kaynak kodu derlemeyi bulabilir ve hata ayıklama çözümü projeye ekleyin.

    ![Azure Data Lake Analytics U-SQL hata ayıklama, proje ekleme](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Proje klasör yolu için **FailedVertexDebugHost** proje. 

3. Sağ **eklenen derleme kaynak kod projesi > Özellikleri**seçin **derleme** sekmesinde sol tarafında ve \bin\debug ile biten kopyalanan yolu yapıştırın **çıkış > çıkış yolu**. Son çıkış yolunu benzer `<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\`.

    ![Azure Data Lake Analytics U-SQL hata ayıklama pdb yolunu ayarlayın.](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Bu ayarlar sonra ile hata ayıklamayı Başlat **F5** ve kesme noktaları. Visual Studio hata ayıklama araçları (izleme, değişkenler vb.) sorunu gidermek için de kullanabilirsiniz.

> [!NOTE]
> Derleme kaynak kod projesi güncel .pdb dosyaları oluşturmak için kodu değiştirdikten sonra her zaman yeniden oluşturun.

## <a name="resubmit-the-job"></a>İşi yeniden gönder

Proje başarıyla tamamlanırsa hata ayıklama sonrasında çıkış penceresine aşağıdaki iletiyi gösterir:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Azure Data Lake Analytics U-SQL hata ayıklama başarısız](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Başarısız işi yeniden göndermek için:

1. Arka plan kod çözümleriyle işler için C# kodunu arka plan kod kaynak dosyaya kopyalayın (genellikle `Script.usql.cs`).

2. Derlemeleri işlerle ilgili hata ayıklama çözümü derleme kaynak kod projesine sağ tıklayın ve güncelleştirilmiş .dll derlemelere, Azure Data Lake Kataloğu'na kaydolun.

3. U-SQL işi yeniden gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL Programlama Kılavuzu](data-lake-analytics-u-sql-programmability-guide.md)
- [Azure Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçleri geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)
- [Olağan dışı bir yinelenen iş sorunlarını giderme](data-lake-analytics-data-lake-tools-debug-recurring-job.md)
