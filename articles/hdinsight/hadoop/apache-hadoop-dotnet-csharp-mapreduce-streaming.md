---
title: C# MapReduce hadoop'ta hdınsight'ta - Azure ile kullanma | Microsoft Docs
description: C# Azure hdınsight'ta Hadoop ile MapReduce çözümler oluşturmak için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.custom: hdinsightactive
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 7287972ccf63f33a8cf08065f8d5d30ee1b1afb5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>C# MapReduce hdınsight'ta Hadoop akış ile kullanma

Hdınsight'ta bir MapReduce çözüm oluşturmak için C# kullanmayı öğrenin.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Hadoop akış bir betik veya yürütülebilir dosyası kullanan MapReduce işleri çalıştırmanıza olanak sağlayan bir yardımcı programı kullanılır. Bu örnekte, .NET Eşleyici ve reducer word sayısı çözümü uygulamak için kullanılır.

## <a name="net-on-hdinsight"></a>Hdınsight üzerinde .NET

__Linux tabanlı Hdınsight__ kümeleri kullanım [Mono (https://mono-project.com) ](https://mono-project.com) .NET uygulamalarını çalıştırmak için. Mono sürüm 4.2.1 sürüm 3.6 Hdınsight ile dahil edilir. Hdınsight ile dahil Mono sürümü hakkında daha fazla bilgi için bkz: [Hdınsight bileşen sürümü](../hdinsight-component-versioning.md). Mono belirli bir sürümünü kullanmak için bkz: [yükleme veya güncelleştirme Mono](../hdinsight-hadoop-install-mono.md) belge.

.NET Framework sürümleri Mono uyumluluğu hakkında daha fazla bilgi için bkz: [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop akış nasıl çalışır

Bu belgede akış için kullanılan temel işlem aşağıdaki gibidir:

1. Hadoop veri Eşleyici (Bu örnekte mapper.exe) üzerinde STDIN geçirir.
2. Eşleyici verileri işler ve STDOUT sekmeyle anahtar/değer çiftlerinin yayar.
3. Çıkış, Hadoop tarafından okuyun ve ardından reducer (Bu örnekte reducer.exe) üzerinde STDIN geçirilecek.
4. Reducer sekmeyle anahtar/değer çiftlerini okur, verileri işler ve STDOUT üzerinde sekmeyle anahtar/değer çiftleri olarak sonucu yayar.
5. Çıktı Hadoop tarafından okunabilir ve çıktı dizinine yazılabilir.

Akış ile ilgili daha fazla bilgi için bkz: [Hadoop akış (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Önkoşullar

* Yazma ve .NET Framework 4.5 hedefleyen C# kod oluşturma ile benzer. Bu belgede yer alan adımlar, Visual Studio 2017 kullanın.

* Kümeye .exe dosyaları yüklemek için bir yol. Bu belgede yer alan adımlar, küme için birincil depolama dosyaları yüklemek için Visual Studio için Data Lake araçları kullanın.

* Azure PowerShell veya SSH istemcisi.

* Hdınsight kümesi Hadoop'ta. Küme oluşturma ile ilgili daha fazla bilgi için bkz: [bir Hdınsight kümesi oluşturmayı](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Eşleyici oluşturma

Visual Studio'da yeni bir oluşturma __konsol uygulaması__ adlı __Eşleyici__. Aşağıdaki kod, uygulama için kullanın:

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

Uygulamayı oluşturduktan sonra onu üretmek için yapı `/bin/Debug/mapper.exe` proje dizininde dosya.

## <a name="create-the-reducer"></a>Reducer oluşturma

Visual Studio'da yeni bir oluşturma __konsol uygulaması__ adlı __reducer__. Aşağıdaki kod, uygulama için kullanın:

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

Uygulamayı oluşturduktan sonra onu üretmek için yapı `/bin/Debug/reducer.exe` proje dizininde dosya.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

1. Visual Studio'da açın **Sunucu Gezgini**.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve ardından **oturum**.

4. Bu uygulamayı dağıtmak istediğiniz Hdınsight kümesini genişletin. Metin girişi __(varsayılan depolama hesabı)__ listelenir.

    ![Sunucu Gezgini küme için depolama hesabı gösterme](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/storage.png)

    * Bu girdi şekilde genişletilebilir, kullanmakta olduğunuz bir __Azure depolama hesabı__ kümenin varsayılan depolama. Kümenin varsayılan depolama dosyalarını görüntülemek için giriş genişletin ve ardından __(varsayılan kapsayıcı)__.

    * Bu girdi genişletilemiyor, kullanmakta olduğunuz __Azure Data Lake Store__ kümenin varsayılan depolama. Kümenin varsayılan depolama dosyalarını görüntülemek için çift __(varsayılan depolama hesabı)__ girişi.

5. .Exe dosyaları yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * Kullanılıyorsa bir __Azure depolama hesabı__karşıya yükleme simgesine tıklayın ve sonra gidin **bin\debug** için klasör **Eşleyici** projesi. Son olarak, seçin **mapper.exe** dosya ve tıklayın **Tamam**.

        ![simgeyi karşıya yükleyin](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/upload.png)
    
    * Kullanıyorsanız __Azure Data Lake Store__dosya listesindeki boş bir alana sağ tıklayın ve ardından __karşıya__. Son olarak, seçin **mapper.exe** dosya ve tıklayın **açık**.

    Bir kez __mapper.exe__ karşıya yükleme tamamlandı, karşıya yükleme işlemi için yineleyin __reducer.exe__ dosya.

## <a name="run-a-job-using-an-ssh-session"></a>Bir işi çalıştırın: bir SSH oturumu kullanma

1. SSH Hdınsight kümesine bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. MapReduce işi başlatmak için aşağıdaki komutlardan birini kullanın:

    * Kullanıyorsanız __Data Lake Store__ varsayılan depolama olarak:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Kullanıyorsanız __Azure Storage__ varsayılan depolama olarak:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    Aşağıdaki listede, her bir parametreyi yaptığı açıklanmaktadır:

    * `hadoop-streaming.jar`: Akış MapReduce işlevselliği içerir jar dosyasını.
    * `-files`: Ekler `mapper.exe` ve `reducer.exe` bu iş dosyalarına. `adl:///` Veya `wasb:///` önce her bir dosyanın varsayılan depolama kümesi için kök yolu.
    * `-mapper`: Hangi dosya Eşleyici uygulayan belirtir.
    * `-reducer`: Hangi dosya reducer uygulayan belirtir.
    * `-input`: Giriş verileri.
    * `-output`: Çıktı dizini.

3. MapReduce işi tamamlandığında, sonuçları görüntülemek için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Aşağıdaki metni, bu komutu tarafından döndürülen veri örneğidir:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Bir işi çalıştırın: PowerShell'i kullanma

Bir MapReduce işi çalıştırma ve sonuçları indirmek için aşağıdaki PowerShell betiğini kullanın.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Bu komut dosyası küme oturum açma hesabı adı ve parola, Hdınsight küme adı ile birlikte ister. İş tamamlandığında, çıkış adlı bir dosya indirilir `output.txt`. Aşağıdaki metin verileri örneğidir `output.txt` dosyası:

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

Hdınsight ile MapReduce kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile MapReduce kullanma](hdinsight-use-mapreduce.md).

C# Hive veya Pig kullanma hakkında daha fazla bilgi için bkz: [C# kullanıcı tanımlı bir işlev Hive veya Pig kullanmak](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

C# Hdınsight üzerinde Storm ile kullanma hakkında daha fazla bilgi için bkz: [Hdınsight üzerinde Storm için C# topolojileri](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
