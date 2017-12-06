---
title: "C# Hive veya Pig hadoop'ta hdınsight'ta - Azure ile kullanma | Microsoft Docs"
description: "Hive veya Pig Azure Hdınsight'ta akış ile C# kullanıcı tanımlı işlevler (UDF) kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: 1ad6ba7126b210ddc671026244c4c614d7010000
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>C# kullanıcı tanımlı işlevler Hive veya Pig hdınsight'ta Hadoop akış ile kullanma

Apache Hive ve Hdınsight üzerinde Pig C# kullanıcı tanımlı işlevler (UDF) kullanmayı öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux tabanlı hem de Windows tabanlı Hdınsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Her ikisi de Hive ve Pig işleme için dış uygulamalara veri geçirebilirsiniz. Bu işlem olarak bilinir _akış_. .NET uygulaması kullanırken, veriler üzerinde STDIN uygulamaya gönderilir ve uygulama STDOUT üzerinde sonuçları döndürür. STDIN ve STDOUT okuma ve yazma için kullanabileceğiniz `Console.ReadLine()` ve `Console.WriteLine()` bir konsol uygulamasından.

## <a name="prerequisites"></a>Ön koşullar

* Yazma ve .NET Framework 4.5 hedefleyen C# kod oluşturma ile benzer.

    * İstediğiniz ne olursa olsun IDE kullanın. Öneririz [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, veya [Visual Studio Code](https://code.visualstudio.com/). Bu belgede yer alan adımlar, Visual Studio 2017 kullanın.

* .Exe dosyaları küme ve çalışma Pig ve Hive işi karşıya yüklemek için bir yol. Visual Studio, Azure PowerShell ve Azure CLI için Data Lake araçları ile öneririz. Bu belge kullanımı dosyaları karşıya yükleme ve örneği çalıştırmak Visual Studio için Data Lake Araçları'ndaki adımları sorgu yığını.

    Çalıştırmak için diğer yöntemler hakkında bilgi için sorgular ve Pig işleri Hive, aşağıdaki belgelere bakın:

    * [Apache Hive Hdınsight ile kullanma](hdinsight-use-hive.md)

    * [Hdınsight ile Apache Pig kullanma](hdinsight-use-pig.md)

* Hdınsight kümesi Hadoop'ta. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [bir Hdınsight kümesi oluşturmayı](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="net-on-hdinsight"></a>Hdınsight üzerinde .NET

* __Linux tabanlı Hdınsight__ kullanarak kümeleri [Mono (https://mono-project.com)](https://mono-project.com) .NET uygulamalarını çalıştırmak için. Mono sürüm 4.2.1 Hdınsight sürüm 3.5 ile dahil edilir.

    .NET Framework sürümleri Mono uyumluluğu hakkında daha fazla bilgi için bkz: [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/).

    Mono belirli bir sürümünü kullanmak için bkz: [yükleme veya güncelleştirme Mono](../hdinsight-hadoop-install-mono.md) belge.

* __Windows tabanlı Hdınsight__ kümeleri .NET uygulamalarını çalıştırmak için Microsoft .NET CLR kullanın.

Hdınsight sürümleri dahil Mono ve .NET framework sürümü hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü](../hdinsight-component-versioning.md).

## <a name="create-the-c-projects"></a>C oluşturmak\# projeleri

### <a name="hive-udf"></a>UDF yığını

1. Visual Studio'yu açın ve bir çözüm oluşturun. Proje türü olarak seçin **konsol uygulaması (.NET Framework)**ve yeni proje adı **HiveCSharp**.

    > [!IMPORTANT]
    > Seçin __.NET Framework 4.5__ Linux tabanlı Hdınsight kümesi kullanıyorsanız. .NET Framework sürümleri Mono uyumluluğu hakkında daha fazla bilgi için bkz: [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/).

2. Değiştir **Program.cs** aşağıdaki kod ile:

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. Projeyi derleyin.

### <a name="pig-udf"></a>Pig UDF

1. Visual Studio'yu açın ve bir çözüm oluşturun. Proje türü olarak seçin **konsol uygulaması**ve yeni proje adı **PigUDF**.

2. Değiştir **Program.cs** aşağıdaki kod ile dosya:

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    Bu kod Pig gönderilen satırları ayrıştırır ve ile başlayan satırlar yeniden biçimlendirir `java.lang.Exception`.

3. Kaydet **Program.cs**ve projeyi oluşturun.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve ardından **oturum**.

4. Bu uygulamayı dağıtmak istediğiniz Hdınsight kümesini genişletin. Metin girişi __(varsayılan depolama hesabı)__ listelenir.

    ![Sunucu Gezgini küme için depolama hesabı gösterme](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Bu girdi şekilde genişletilebilir, kullanmakta olduğunuz bir __Azure depolama hesabı__ kümenin varsayılan depolama. Kümenin varsayılan depolama dosyalarını görüntülemek için giriş genişletin ve ardından __(varsayılan kapsayıcı)__.

    * Bu girdi genişletilemiyor, kullanmakta olduğunuz __Azure Data Lake Store__ kümenin varsayılan depolama. Kümenin varsayılan depolama dosyalarını görüntülemek için çift __(varsayılan depolama hesabı)__ girişi.

6. .Exe dosyaları yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * Kullanılıyorsa bir __Azure depolama hesabı__karşıya yükleme simgesine tıklayın ve sonra gidin **bin\debug** için klasör **HiveCSharp** projesi. Son olarak, seçin **HiveCSharp.exe** dosya ve tıklayın **Tamam**.

        ![simgeyi karşıya yükleyin](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Kullanıyorsanız __Azure Data Lake Store__dosya listesindeki boş bir alana sağ tıklayın ve ardından __karşıya__. Son olarak, seçin **HiveCSharp.exe** dosya ve tıklayın **açık**.

    Bir kez __HiveCSharp.exe__ karşıya yükleme tamamlandı, karşıya yükleme işlemi için yineleyin __PigUDF.exe__ dosya.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. Dağıttığınız kümesine sağ **HiveCSharp** için uygulama ve ardından **Hive sorgusu yaz**.

4. Aşağıdaki metni Hive sorgusu için kullanın:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > Açıklamadan çıkarın `add file` kümeniz için kullanılan varsayılan depolama türüyle eşleşen deyimi.

    Bu sorgu seçer `clientid`, `devicemake`, ve `devicemodel` alanlarını `hivesampletable`ve alanları HiveCSharp.exe uygulamaya geçirir. Sorgu olarak depolanan üç alanlarını döndürmek için uygulama bekler `clientid`, `phoneLabel`, ve `phoneHash`. Sorgu, varsayılan depolama kapsayıcısını kök dizininde HiveCSharp.exe bulmak de bekliyor.

5. Tıklatın **gönderme** bir Hdınsight kümesine işi göndermek için. **Hive işi Özet** penceresi açılır.

6. Tıklatın **yenileme** kadar özeti yenilemek için **iş durumu** değişikliklerini **tamamlandı**. İş çıktısı görüntülemek için **iş çıktısı**.

## <a name="run-a-pig-job"></a>Pig işi çalıştırma

1. Hdınsight kümenize bağlanmak için aşağıdaki yöntemlerden birini kullanın:

    * Kullanıyorsanız bir __Linux tabanlı__ Hdınsight küme, SSH kullanın. Örneğin, `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Daha fazla bilgi için bkz: [SSH kullanma withHDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Kullanıyorsanız bir __Windows tabanlı__ Hdınsight kümesi [Uzak Masaüstü kullanarak kümeye Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. Pig komut satırında başlatmak için aşağıdaki komutu kullanın:

        pig

    > [!IMPORTANT]
    > Windows tabanlı bir küme kullanıyorsanız, bunun yerine aşağıdaki komutları kullanın:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    A `grunt>` komut istemi görüntülenir.

3. .NET Framework uygulama kullanan bir Pig işi çalıştırmak için şunu girin:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    `DEFINE` Deyimi bir diğer ad oluşturur `streamer` pigudf.exe uygulamalar ve `CACHE` kümenin varsayılan depolama biriminden yükler. Daha sonra `streamer` ile kullanılan `STREAM` işlem GÜNLÜĞÜNDE yer alan tek satırları ve sütunları bir dizi olarak veri döndürmek için işleci.

    > [!NOTE]
    > Akış için kullanılan uygulama adı tarafından alınmalıdır \` (backtick) ne zaman karakter diğer adı, ve ' (tek tırnak) ile kullanıldığında `SHIP`.

4. Son satırı girdikten sonra iş başlamanız gerekir. Çıktı aşağıdaki metni benzer döndürür:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Hdınsight'ta Hive veya Pig .NET Framework uygulamasından kullanmayı öğrendiniz. Hive veya Pig Python kullanmayı öğrenmek istiyorsanız, bkz: [kullanmak Python Hive veya Pig hdınsight'ta ile](python-udf-hdinsight.md).

Diğer yolları için Pig ve Hive kullanmaya ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
