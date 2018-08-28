---
title: C# ile HDInsight - Azure Hadoop MapReduce kullanma
description: C# Azure HDInsight, Hadoop ile MapReduce çözümleri oluşturmak için kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: 91558ae4fd592c7490aef0a0f2fd930e970f498c
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049072"
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>C# üzerinde HDInsight Hadoop akışı MapReduce ile kullanma

C# üzerinde HDInsight MapReduce çözüm oluşturmak için kullanmayı öğrenin.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Hadoop akışı MapReduce işlerinizi bir betik veya yürütülebilir dosya kullanarak olanak tanıyan bir aracıdır. Bu örnekte, .NET Eşleyici ve bir sözcük sayımı çözümün için Azaltıcı uygulamak için kullanılır.

## <a name="net-on-hdinsight"></a>HDInsight üzerinde .NET

__Linux tabanlı HDInsight__ kümeleri kullanım [Mono (https://mono-project.com) ](https://mono-project.com) .NET uygulamaları çalıştırmak için. HDInsight sürümü 3.6 ile Mono sürüm 4.2.1 dahildir. HDInsight ile dahil Mono sürümü hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümü](../hdinsight-component-versioning.md). Mono belirli bir sürümünü kullanmak için bkz: [yükleme veya güncelleştirme Mono](../hdinsight-hadoop-install-mono.md) belge.

.NET Framework sürümleri ile Mono uyumluluğu hakkında daha fazla bilgi için bkz. [Mono uyumluluğu](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop akışı nasıl çalışır

Bu belgede akış için kullanılan temel işlem aşağıdaki gibidir:

1. Hadoop veri Eşleyici (Bu örnekte mapper.exe) üzerinde STDIN geçirir.
2. Eşleyici verileri işler ve sekmeyle ayrılmış anahtar/değer çiftleri STDOUT yayar.
3. Çıktısı, Hadoop tarafından okunur ve ardından Azaltıcı (Bu örnekte reducer.exe) üzerinde STDIN geçirilir.
4. Azaltıcı sekmeyle ayrılmış bir anahtar/değer çiftlerini okur, verileri işler ve ardından sonucu, STDOUT üzerinde sekmeyle ayrılmış bir anahtar/değer çiftleri olarak yayar.
5. Çıkış Hadoop tarafından okunur ve çıkış dizinine yazılır.

Akış üzerinde daha fazla bilgi için bkz. [Hadoop akış](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Önkoşullar

* Yazma ve .NET Framework 4.5 hedefleyen C# kod oluşturma bilgisi. Bu belgedeki adımlarda Visual Studio 2017'yi kullanın.

* Kümeye .exe dosyaları karşıya yüklemek için bir yol. Bu belgedeki adımlarda Visual Studio için Data Lake araçları küme için birincil depolama alanına dosya yüklemek için kullanın.

* Azure PowerShell veya SSH istemcisi.

* Bir Hadoop HDInsight kümesi üzerinde. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [bir HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Eşleyici oluşturma

Visual Studio'da yeni bir oluşturma __konsol uygulaması__ adlı __Eşleyici__. Uygulama için aşağıdaki kodu kullanın:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Uygulamayı oluşturduktan sonra bunu oluşturmak için yapı `/bin/Debug/mapper.exe` dosya proje dizininde.

## <a name="create-the-reducer"></a>Azaltıcı oluşturma

Visual Studio'da yeni bir oluşturma __konsol uygulaması__ adlı __Azaltıcı__. Uygulama için aşağıdaki kodu kullanın:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Uygulamayı oluşturduktan sonra bunu oluşturmak için yapı `/bin/Debug/reducer.exe` dosya proje dizininde.

## <a name="upload-to-storage"></a>Depolamaya yükleme

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve ardından **oturum**.

4. Bu uygulamayı dağıtmak istediğiniz bir HDInsight kümesini genişletin. Metin girişi __(varsayılan depolama hesabı)__ listelenir.

    ![Sunucu Gezgini küme için depolama hesabını gösteren](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/storage.png)

    * Bu giriş genişletilebilir, kullanmakta olduğunuz bir __Azure depolama hesabı__ kümenin varsayılan depolama alanı olarak. Kümenin varsayılan depolama dosyalarını görüntülemek için giriş genişletin ve ardından çift __(varsayılan kapsayıcı)__.

    * Bu giriş genişletilemez, kullanmakta olduğunuz __Azure Data Lake Store__ kümenin varsayılan depolama alanı olarak. Kümenin varsayılan depolama alanı dosyaları görüntülemek için çift tıklayın __(varsayılan depolama hesabı)__ girişi.

5. .Exe dosyalarını karşıya yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * Kullanılıyorsa bir __Azure depolama hesabı__, karşıya yükleme simgesine tıklayın ve ardından gözatın **bin\debug** klasör **Eşleyici** proje. Son olarak, seçin **mapper.exe** tıklayın ve dosya **Tamam**.

        ![simgeyi karşıya yükleyin](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/upload.png)
    
    * Kullanıyorsanız __Azure Data Lake Store__dosya listesinde boş bir alana sağ tıklayın ve ardından __karşıya__. Son olarak, seçin **mapper.exe** tıklayın ve dosya **açık**.

    Bir kez __mapper.exe__ karşıya yükleme tamamlandığında, karşıya yükleme işlemi için yineleyin __reducer.exe__ dosya.

## <a name="run-a-job-using-an-ssh-session"></a>Bir işi çalıştırma: bir SSH oturumu kullanma

1. HDInsight kümesine bağlanmak için SSH kullanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. MapReduce işi başlatmak için aşağıdaki komutlardan birini kullanın:

    * Kullanıyorsanız __Data Lake Store__ varsayılan depolama alanı olarak:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Kullanıyorsanız __Azure depolama__ varsayılan depolama alanı olarak:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    Aşağıdaki liste, her parametre ne yaptığını açıklar:

    * `hadoop-streaming.jar`: Akış MapReduce işlevleri içerir jar dosyası.
    * `-files`: Ekler `mapper.exe` ve `reducer.exe` bu proje dosyaları. `adl:///` Veya `wasb:///` önce her dosya kümenin varsayılan depolama alanı kökünde yoludur.
    * `-mapper`: Hangi dosya Eşleyici uygulayan belirtir.
    * `-reducer`: Hangi dosya Azaltıcı uygulayan belirtir.
    * `-input`: Giriş verileri.
    * `-output`: Çıkış dizini.

3. MapReduce işi tamamlandığında, sonuçları görüntülemek için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Bu komut tarafından döndürülen veriler bir örneği aşağıda gösterilmiştir:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Bir işi çalıştırma: PowerShell kullanarak

Bir MapReduce işi çalıştırın ve sonuçları karşıdan yüklemek için aşağıdaki PowerShell betiğini kullanın.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Bu betik, küme oturum açma hesabı adı ve parola, HDInsight küme adıyla birlikte ister. İş tamamlandığında, çıkış adlı bir dosya indirilir `output.txt`. Aşağıdaki metni verileri örneğidir `output.txt` dosyası:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile MapReduce kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md).

Hive ve Pig ile C# ' ı kullanma hakkında daha fazla bilgi için bkz. [Hive ve Pig ile C# kullanıcı tanımlı işlevi kullanma](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

C# HDInsight üzerinde Storm ile kullanma hakkında daha fazla bilgi için bkz. [C# topolojileri geliştirme HDInsight üzerinde Storm için](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
