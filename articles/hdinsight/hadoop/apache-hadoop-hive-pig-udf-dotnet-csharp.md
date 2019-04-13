---
title: Kullanım C# Apache Hive ve HDInsight - Azure, Apache hadoop'un Apache Pig
description: Nasıl kullanacağınızı öğrenin C# kullanıcı tanımlı işlevler (UDF) ile Apache Hive ve Apache Pig Azure HDInsight akış.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: ac2edb4c12e95a915790c1fadfb2dcdcce554aad
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545582"
---
# <a name="use-c-user-defined-functions-with-apache-hive-and-apache-pig-streaming-on-apache-hadoop-in-hdinsight"></a>Kullanım C# Apache Hive ve Apache Pig, HDInsight, Apache Hadoop üzerinde akış ile kullanıcı tanımlı işlevler

Nasıl kullanacağınızı öğrenin C# kullanıcı tanımlı işlevler (UDF) Apache Hive ve HDInsight üzerinde Apache Pig ile.

> [!IMPORTANT]
> Bu belgede yer alan adımlar hem Linux hem de Windows tabanlı HDInsight kümeleri ile çalışma. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Hem Hive ve Pig veri işleme için dış uygulama geçirebilirsiniz. Bu işlem olarak bilinir _akış_. Bir .NET uygulaması kullanırken, veri üzerinde STDIN uygulamaya geçirilir ve uygulama STDOUT sonuçlarını döndürür. STDIN ve STDOUT okuma ve yazma için kullanabileceğiniz `Console.ReadLine()` ve `Console.WriteLine()` konsol uygulamasından.

## <a name="prerequisites"></a>Önkoşullar

* Yazma ve .NET Framework 4.5 hedefleyen C# kod oluşturma bilgisi.

    * İstediğiniz herhangi bir IDE kullanın. Öneririz [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, veya [Visual Studio Code](https://code.visualstudio.com/). Bu belgedeki adımlarda Visual Studio 2017'yi kullanın.

* Kümeyi ve çalışma Pig ve Hive işleri için .exe dosyaları karşıya yüklemek için bir yol. Data Lake araçları Visual Studio, Azure PowerShell ve klasik Azure CLI'yı öneririz. Bu belge kullanım dosyaları yüklemek ve örneği çalıştırmak Visual Studio için Data Lake Araçları'ndaki adımları Hive sorgusu.

    Çalıştırmak için diğer yollar hakkında bilgi için sorgular ve Pig işleri Hive, aşağıdaki belgelere bakın:

    * [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)

    * [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)

* Bir Hadoop HDInsight kümesi üzerinde. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [bir HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="net-on-hdinsight"></a>HDInsight üzerinde .NET

* __Linux tabanlı HDInsight__ kullanarak kümeleri [Mono (https://mono-project.com) ](https://mono-project.com) .NET uygulamaları çalıştırmak için. HDInsight sürümü 3.6 ile Mono sürüm 4.2.1 dahildir.

    .NET Framework sürümleri ile Mono uyumluluğu hakkında daha fazla bilgi için bkz. [Mono uyumluluğu](https://www.mono-project.com/docs/about-mono/compatibility/).

* __Windows tabanlı HDInsight__ kümeleri .NET uygulamaları çalıştırmak için Microsoft .NET CLR kullanın.

HDInsight sürümleri dahil Mono ve .NET framework sürümü hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümü](../hdinsight-component-versioning.md).

## <a name="create-the-c-projects"></a>C oluşturma\# projeleri

### <a name="apache-hive-udf"></a>Apache Hive UDF

1. Visual Studio'yu açın ve bir çözüm oluşturun. Proje türü için **konsol uygulaması (.NET Framework)** ve yeni proje adını **HiveCSharp**.

    > [!IMPORTANT]
    > Seçin __.NET Framework 4.5__ bir Linux tabanlı HDInsight kümesi kullanıyorsanız. .NET Framework sürümleri ile Mono uyumluluğu hakkında daha fazla bilgi için bkz. [Mono uyumluluğu](https://www.mono-project.com/docs/about-mono/compatibility/).

2. Öğesinin içeriğini değiştirin **Program.cs** aşağıdaki kod ile:

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

### <a name="apache-pig-udf"></a>Apache Pig UDF

1. Visual Studio'yu açın ve bir çözüm oluşturun. Proje türü için **konsol uygulaması**ve yeni proje adını **PigUDF**.

2. Öğesinin içeriğini değiştirin **Program.cs** dosyasındaki kodu aşağıdaki kodla:

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

    Bu kod, Pig gönderilen satırlarının ayrıştırır ve ile başlayan satırlar yeniden biçimlendirir `java.lang.Exception`.

3. Kaydet **Program.cs**ve ardından projeyi derleyin.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve ardından **oturum**.

4. Bu uygulamayı dağıtmak istediğiniz bir HDInsight kümesini genişletin. Metin girişi __(varsayılan depolama hesabı)__ listelenir.

    ![Sunucu Gezgini küme için depolama hesabını gösteren](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Bu giriş genişletilebilir, kullanmakta olduğunuz bir __Azure depolama hesabı__ kümenin varsayılan depolama alanı olarak. Kümenin varsayılan depolama dosyalarını görüntülemek için giriş genişletin ve ardından çift __(varsayılan kapsayıcı)__.

    * Bu giriş genişletilemez, kullanmakta olduğunuz __Azure Data Lake Storage__ kümenin varsayılan depolama alanı olarak. Kümenin varsayılan depolama alanı dosyaları görüntülemek için çift tıklayın __(varsayılan depolama hesabı)__ girişi.

6. .Exe dosyalarını karşıya yüklemek için aşağıdaki yöntemlerden birini kullanın:

   * Kullanılıyorsa bir __Azure depolama hesabı__, karşıya yükleme simgesine tıklayın ve ardından gözatın **bin\debug** klasör **HiveCSharp** proje. Son olarak, seçin **HiveCSharp.exe** tıklayın ve dosya **Tamam**.

       ![simgeyi karşıya yükleyin](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
   * Kullanıyorsanız __Azure Data Lake Storage__dosya listesinde boş bir alana sağ tıklayın ve ardından __karşıya__. Son olarak, seçin **HiveCSharp.exe** tıklayın ve dosya **açık**.

     Bir kez __HiveCSharp.exe__ karşıya yükleme tamamlandığında, karşıya yükleme işlemi için yineleyin __PigUDF.exe__ dosya.

## <a name="run-an-apache-hive-query"></a>Apache Hive sorgusu çalıştırma

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. Dağıttığınız kümeye sağ **HiveCSharp** uygulamasını ve ardından **Hive sorgusu yaz**.

4. Aşağıdaki metni için Hive sorgusu kullanın:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen1
    -- add file adl:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen2
    -- add file abfs:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > Açıklamadan çıkarın `add file` kümeniz için kullanılan varsayılan depolama alanı türü ile eşleşen bir ifade.

    Bu sorgu seçer `clientid`, `devicemake`, ve `devicemodel` alanlarını `hivesampletable`ve alanların HiveCSharp.exe uygulamaya geçirir. Uygulama olarak depolanan üç alan döndürmek için sorguyu bekliyor `clientid`, `phoneLabel`, ve `phoneHash`. Sorgu, varsayılan depolama kapsayıcısını kökündeki HiveCSharp.exe bulmak de bekliyor.

5. Tıklayın **Gönder** işi HDInsight kümesine göndermek için. **Hive işi özeti** penceresi açılır.

6. Tıklayın **Yenile** kadar özeti yenilemek için **iş durumu** değişikliklerini **tamamlandı**. İş çıkışını görüntülemek için tıklayın **iş çıktısı**.

## <a name="run-an-apache-pig-job"></a>Apache Pig işi çalıştırma

1. HDInsight kümenize bağlanmak için SSH kullanın. Örneğin, `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Daha fazla bilgi için [SSH kullanma withHDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Pig komut satırında başlatmak için aşağıdaki komutu kullanın:

        pig

    > [!IMPORTANT]
    > Windows tabanlı bir küme kullanıyorsanız aşağıdaki komutları kullanın:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    A `grunt>` istemi görüntülenir.

3. .NET Framework uygulamasını kullanan bir Pig işi çalıştırmak için şunu girin:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    `DEFINE` Deyimi oluşturur, bir diğer ad `streamer` pigudf.exe uygulamaların ve `CACHE` kümenin varsayılan depolama biriminden yükler. Daha sonra `streamer` ile kullanılan `STREAM` işlem GÜNLÜĞÜNDE yer alan tek satır ve sütun bir dizi olarak verileri döndürmek için işleci.

    > [!NOTE]
    > Akış için kullanılan uygulama adı tarafından alınmalıdır \` (vurgulamasını belirtir) ne zaman karakter diğer adı, ve ' (tek tırnak işareti) ile kullanıldığında `SHIP`.

4. Son satırı girdikten sonra iş başlamanız gerekir. Çıktı aşağıdaki metne benzer döndürür:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight üzerinde Hive ve Pig bir .NET Framework uygulamasından kullanmayı öğrendiniz. Hive ve Pig ile Python kullanma konusunda bilgi almak istiyorsanız, bkz. [Apache Hive ve HDInsight, Apache Pig ile Python kullanma](python-udf-hdinsight.md).

Diğer yolları için Pig ve Hive kullanmaya ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
