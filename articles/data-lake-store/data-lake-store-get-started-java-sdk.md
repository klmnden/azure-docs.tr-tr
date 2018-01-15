---
title: "Java SDK: Azure Data Lake Store'daki dosya sistemi işlemleri | Microsoft Docs"
description: "Data Lake Store'da klasör oluşturma gibi dosya sistemi işlemlerini gerçekleştirmek için Azure Data Lake Store Java SDK'sını kullanın."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: fdae36c3cbdda16f9392a113502c7c6c62b25534
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="filesystem-operations-on-data-lake-store-using-java-sdk"></a>Data Lake Store'da Java SDK'sı kullanılarak gerçekleştirilen dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Klasör oluşturma, veri dosyalarını karşıya yükleme ve indirme gibi temel işlemleri gerçekleştirmek için Azure Data Lake Store Java SDK’sını kullanma hakkında bilgi edinin. Data Lake hakkında daha fazla bilgi için bkz. [Azure Data Lake Store](data-lake-store-overview.md).

Azure Data Lake Store için Java SDK API belgelerine, [Azure Data Lake Store Java API belgelerinden](https://azure.github.io/azure-data-lake-store-java/javadoc/) erişebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Java Development Kit (Java sürüm 1.7 veya üzerini kullanan JDK 7 ya da üzeri)
* Azure Data Lake Store hesabı. [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın.
* [Maven](https://maven.apache.org/install.html). Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.
* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
[GitHub’da](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) bulunan kod örneği, depoda dosya oluşturma, dosyaları birleştirme, dosya indirme ve depodaki bazı dosyaları silme işlemlerinde size yol gösterir. Makalenin bu bölümü, kodun ana bölümlerinde sizi yönlendirir.

1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın. 

2. Maven **pom.xml** dosyanıza aşağıdaki bağımlılıkları ekleyin. Aşağıdaki kod parçacığını **\</project>** etiketinin önüne ekleyin:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    İlk bağımlılık, maven deposundan Data Lake Store SDK’sını (`azure-data-lake-store-sdk`) kullanmaktır. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake Store SDK’sı; log4j, Java günlük kaydı, logback gibi birçok popüler günlük altyapısından birini seçmenizi veya günlük kaydı seçmemenizi sağlayan [slf4j](http://www.slf4j.org/) günlük cephesini kullanır. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](http://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

        import java.io.*;
        import java.util.Arrays;
        import java.util.List;

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için bkz. [Java kullanarak Data Lake Store'da son kullanıcı kimlik doğrulaması gerçekleştirme](data-lake-store-end-user-authenticate-java-sdk.md).
* Uygulamanızın servisler arası kimlik doğrulaması için bkz. [Java kullanarak Data Lake Store'da servisler arası kimlik doğrulaması gerçekleştirme](data-lake-store-service-to-service-authenticate-java.md).

## <a name="create-an-azure-data-lake-store-client"></a>Azure Data Lake Store istemcisi oluşturma
[ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) nesnesi oluşturmak için Data Lake Store hesap adını ve Data Lake Store ile kimlik doğrulaması gerçekleştirdiğinizde ([Kimlik doğrulaması](#authentication) bölümüne bakın) oluşturduğunuz belirteç sağlayıcısını belirtmeniz gerekir. Data Lake Store hesap adının tam etki alanı adı olması gerekir. Örneğin, **BURAYI DOLDURUN** alanını **mydatalakestore.azuredatalakestore.net** gibi bir etki alanı adı değiştirin.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

Aşağıdaki bölümlerde yer alan kod parçacıkları bazı ortak dosya sistemi işlemlerine örnekler içermektedir. Diğer işlemleri görmek için **ADLStoreClient** nesnesinin tüm [Data Lake Store Java SDK API belgelerine](https://azure.github.io/azure-data-lake-store-java/javadoc/) bakabilirsiniz.

## <a name="create-a-directory"></a>Dizin oluşturma

Aşağıdaki kod parçacığı belirttiğiniz Data Lake Store hesabının kök dizininde bir dizin yapısı oluşturur.

    // create directory
    client.createDirectory("/a/b/w");
    System.out.println("Directory created.");

## <a name="create-a-file"></a>Dosya oluşturma

Aşağıdaki kod parçacığı dizin yapısında bir dosya (c.txt) oluşturur ve dosyaya veri yazar.

    // create file and write some content
    String filename = "/a/b/c.txt";
    OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
    PrintStream out = new PrintStream(stream);
    for (int i = 1; i <= 10; i++) {
        out.println("This is line #" + i);
        out.format("This is the same line (%d), but using formatted output. %n", i);
    }
    out.close();
    System.out.println("File created.");

Bayt dizisi kullanarak da dosya (d.txt) oluşturabilirsiniz.

    // create file using byte arrays
    stream = client.createFile("/a/b/d.txt", IfExists.OVERWRITE);
    byte[] buf = getSampleContent();
    stream.write(buf);
    stream.close();
    System.out.println("File created using byte array.");

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır. 

## <a name="append-to-a-file"></a>Dosyanın sonuna ekleme

Aşağıdaki kod parçacığı var olan bir dosyaya içerik ekler.

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();
    System.out.println("File appended.");

Yukarıdaki kod parçacığında kullanılan `getSampleContent` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="read-a-file"></a>Dosya okuma

Aşağıdaki kod parçacığı Data Lake Store hesabındaki bir dosyadan içerik okur.

    // Read File
    InputStream in = client.getReadStream(filename);
    BufferedReader reader = new BufferedReader(new InputStreamReader(in));
    String line;
    while ( (line = reader.readLine()) != null) {
        System.out.println(line);
    }
    reader.close();
    System.out.println();
    System.out.println("File contents read.");

## <a name="concatenate-files"></a>Dosyaları birleştirme

Aşağıdaki kod parçacığı Data Lake Store hesabındaki iki dosyayı birleştirir. İşlem başarılı olursa birleştirilmiş dosya, var olan iki dosyanın yerini alır.

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);
    System.out.println("Two files concatenated into a new file.");

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma

Aşağıdaki kod parçacığı Data Lake Store hesabındaki bir dosyayı yeniden adlandırır.

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");
    System.out.println("New file renamed.");

## <a name="get-metadata-for-a-file"></a>Dosyanın meta verilerini alma

Aşağıdaki kod parçacığı Data Lake Store hesabındaki bir dosyanın meta verilerini alır.

    // get file metadata
    DirectoryEntry ent = client.getDirectoryEntry(filename);
    printDirectoryInfo(ent);
    System.out.println("File metadata retrieved.");

## <a name="set-permissions-on-a-file"></a>Dosyanın izinlerini belirleme

Aşağıdaki kod parçacığı önceki bölümde oluşturduğunuz dosyanın izinlerini belirler.

    // set file permission
    client.setPermission(filename, "744");
    System.out.println("File permission set.");

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Aşağıdaki kod parçacığı dizin içeriğini yinelemeli olarak listeler.

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }
    System.out.println("Directory contents listed.");

Yukarıdaki kod parçacığında kullanılan `printDirectoryInfo` işlevinin tanımı [GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/)'daki örnekle birlikte sunulmaktadır.

## <a name="delete-files-and-folders"></a>Dosyaları ve klasörleri silme

Aşağıdaki kod parçacığı Data Lake Store hesabındaki dosyaları ve klasörleri yinelemeli olarak siler.

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");
    System.out.println("All files and folders deleted recursively");
    promptEnterKey();

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma
1. Bir IDE içinden çalıştırmak için **Çalıştır** düğmesini bulup basın. Maven’den çalıştırmak [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)’i kullanın.
2. Komut satırından çalıştırabileceğiniz tek başına bir jar oluşturmak için jar’ı [Maven derleme eklentisini](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html) kullanarak dahil edilen tüm bağımlılıklarla birlikte derleyin. [GitHub'daki örnek kaynak kodda](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) bulunan pom.xml, bir örnek içerir.

## <a name="next-steps"></a>Sonraki adımlar
* [Java SDK için JavaDoc’u keşfedin](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)


