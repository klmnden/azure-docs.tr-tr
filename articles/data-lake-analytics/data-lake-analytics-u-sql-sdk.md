---
title: Çalıştırma ve U-SQL işleri için Azure Data Lake U-SQL SDK'sını kullanarak yerel olarak test etme
description: Çalıştırma ve test yerel olarak komut satırını kullanarak ve programlama arabirimleri, yerel iş istasyonunda U-SQL işleri hakkında bilgi edinin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanacai
ms.author: yanacai
manager: kfile
editor: jasonwhowell
ms.topic: conceptual
ms.date: 03/01/2017
ms.openlocfilehash: 11a2bfdcda09a071667cc034ef1ff42794b73a33
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737080"
---
# <a name="run-and-test-u-sql-with-azure-data-lake-u-sql-sdk"></a>Çalıştırma ve U-SQL Azure Data Lake U-SQL SDK'sı ile test etme

U-SQL betiği geliştirmeye çalıştırmak için yaygın bir sorundur ve yerel olarak test U-SQL betiği önce göndermek bulut için. Bu senaryo için Azure Data Lake U-SQL SDK adlı bir Nuget paketi Azure Data Lake sağlar, hangi, kolayca aracılığıyla U-SQL çalıştırma ve test ölçeklendirilebilir. Bu U-SQL test derleme otomatikleştirmek ve test etmek için CI (sürekli tümleştirme) sistemi ile tümleştirmek mümkündür.

İlgilendiğiniz ise nasıl el ile yerel çalıştırın ve Azure Data Lake araçları için Visual Studio için kullanabileceğiniz sonra U-SQL betiği GUI araçları ile hata ayıklama. ' Dan daha fazla bilgi edinebilirsiniz [burada](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Yükleme Azure Data Lake U-SQL SDK'sı

Azure Data Lake U-SQL SDK'sı Al [burada](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) Nuget.org üzerinde. Ve kullanmadan önce bağımlılıklar aşağıdaki gibi olduğundan emin olmak gerekir.

### <a name="dependencies"></a>Bağımlılıklar

Data Lake U-SQL SDK'sı aşağıdaki bağımlılıkları gerektirir:

- [Microsoft .NET Framework 4.6 veya daha yeni](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 ve Windows SDK 10.0.10240.0 ya da daha yeni (adlandırılan CppSDK bu makalede). CppSDK almanın iki yolu vardır:

    - Yükleme [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Program dosyaları klasörü altında--Örneğin, C:\Program Files (x86) \Windows Kits\10\ \Windows Kits\10 klasörü sahip olacaksınız. Ayrıca, Windows 10 SDK sürüm \Windows Kits\10\Lib altında bulabilirsiniz. Bu klasörler görmüyorsanız, Visual Studio'yu yeniden yükleyin ve yükleme sırasında Windows 10 SDK'yı seçtiğinizden emin olun. Bu Visual Studio ile yüklü varsa, U-SQL yerel derleyici onu otomatik olarak bulur.

    ![Visual Studio için Data Lake araçları yerel Windows 10 SDK çalıştırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Yükleme [Visual Studio için Data Lake Araçları](http://aka.ms/adltoolsvs). Paketlenmiş Visual C++ ve Windows SDK dosyaları C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK bulabilirsiniz. Bu durumda, U-SQL yerel derleyici bağımlılıkları otomatik olarak bulunamıyor. Bunun için CppSDK yolunu belirtmeniz gerekir. Dosyaları başka bir konuma kopyalayın veya olduğu gibi kullanın.

## <a name="understand-basic-concepts"></a>Temel kavramları anlama

### <a name="data-root"></a>Veri kökü

Veri kök klasörü "yerel depolama" yerel işlem hesabıdır. Bir Data Lake Analytics hesabı Azure Data Lake Store hesabına eşdeğerdir. Farklı bir veri kök klasörüne değiştirme yalnızca farklı depolama hesabına geçişi gibi olur. Farklı bir veri kök klasörlerle yaygın olarak Paylaşılan verilere erişmek istiyorsanız, komut dosyalarınızı mutlak yollar kullanmanız gerekir. Veya dosya sistemi sembolik bağlantılar oluşturun (örneğin, **mklink** NTFS) için paylaşılan veri noktası için veri kök klasörü altında.

Veri kök klasör için kullanılır:

- Veritabanları, tablolar, tablo değerli işlevler (Tvf) ve derlemeleri de dahil olmak üzere, yerel meta verileri depolar.
- U-SQL göreli yolda olarak tanımlanan giriş ve çıkış yol arayın. Göreli yollar kullanarak U-SQL projelerinizi Azure'a dağıtmak kolaylaştırır.

### <a name="file-path-in-u-sql"></a>U-SQL dosya yolu

U-SQL betikleri göreli bir yol ve yerel bir mutlak yolu kullanabilirsiniz. Göreli yolu göreli belirtilen veri kök klasör yoludur. Kullanmanızı öneririz "/" komut dosyalarınızı sunucu tarafı ile uyumlu hale getirmek için yol ayırıcısı olarak. Göreli yollar ve eşdeğer mutlak yollarına bazı örnekleri aşağıda verilmiştir. Bu örneklerde, C:\LocalRunDataRoot veri kök klasörüdür.

|Göreli yol|Mutlak yolu|
|-------------|-------------|
|/ABC/DEF/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Çalışma dizini

U-SQL betiği yerel olarak çalışırken, bir çalışma dizini geçerli çalışma dizini altında derleme sırasında oluşturulur. Derleme çıktı yanı sıra yerel yürütme için gerekli çalışma zamanı dosyalarını bu çalışma dizini gölge olacaktır. Çalışma dizini kök klasörü "ScopeWorkDir" adı verilir ve çalışma dizini altındaki dosyalar aşağıdaki gibidir:

|Dizin/dosya|Dizin/dosya|Dizin/dosya|Tanım|Açıklama|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Karma dize çalışma zamanı sürümü|Çalışma zamanı dosyalarını yerel yürütme için gerekli gölge kopyası|
| |Script_66AE4909AA0ED06C| |Ad script + betik yolu dizesi karma|Derleme çıktı ve yürütme günlüğü adım|
| | |\_komut dosyası\_.abr|Derleyici çıktısı|Cebiri dosyası|
| | |\_ScopeCodeGen\_.*|Derleyici çıktısı|Oluşturulan yönetilen kod|
| | |\_ScopeCodeGenEngine\_. *|Derleyici çıktısı|Oluşturulan yerel kod|
| | |Başvurulan derlemeler|Derleme başvurusu|Başvurulan derleme dosyaları|
| | |deployed_resources|Kaynak dağıtma|Kaynak dağıtım dosyaları|
| | |xxxxxxxx.xxx[1..n]\_\*. *|Yürütme günlüğü|Günlük yürütme adımları|


## <a name="use-the-sdk-from-the-command-line"></a>Komut satırından SDK'yı kullanma

### <a name="command-line-interface-of-the-helper-application"></a>Yardımcı uygulamasının komut satırı arabirimi

SDK directory\build\runtime altında LocalRunHelper.exe en yaygın olarak kullanılan yerel çalıştırma işlevlerin arabirimlerine sağlayan komut satırı yardımcı uygulamasıdır. Komut ve değişken anahtarları büyük küçük harfe duyarlı olduğunu unutmayın. Bunu çağırmak için:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Bağımsız değişkenler olmadan veya LocalRunHelper.exe çalıştırın **yardımcı** Yardım bilgilerini göstermek için anahtar:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

Yardım bilgileri:

-  **Komut** komut adını verir.  
-  **Bağımsız değişkeni gerekli** sağlanmalıdır bağımsız değişkenleri listeler.  
-  **İsteğe bağlı bağımsız değişkeni** varsayılan değerlerle isteğe bağlı bağımsız değişkenler listelenmiştir.  Boole isteğe bağlı bağımsız değişkenler parametreleri yoktur ve, görünümlerini varsayılan değerlerine negatif anlamına gelir.

### <a name="return-value-and-logging"></a>Dönüş değeri ve günlüğe kaydetme

Yardımcı uygulama döndürür **0** başarı için ve **-1** hatası. Varsayılan olarak, yardımcı geçerli konsola tüm iletileri gönderir. Ancak, komutları çoğunu destekler **- MessageOut path_to_log_file** çıkışları bir günlük dosyasına yönlendirir isteğe bağlı bağımsız değişkeni.

### <a name="environment-variable-configuring"></a>Ortam değişkeni yapılandırma

U-SQL yerel ihtiyaçlarını bağımlılıklar için belirtilen CppSDK yolu yanı sıra yerel depolama hesabı olarak belirtilen veri kök çalıştırın. Her ikisi de bağımsız değişkeni komut satırı veya ayarlanan ortam değişkeninde bunları için ayarlayabilirsiniz.

- Ayarlama **SCOPE_CPP_SDK** ortam değişkeni.

    Visual Studio için Data Lake araçları yükleyerek Microsoft Visual C++ ve Windows SDK'sı alırsanız aşağıdaki klasörü sahip olduğunu doğrulayın:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Adlı yeni bir ortam değişkeni tanımlamak **SCOPE_CPP_SDK** bu dizinine yönlendirin. Veya klasör başka bir konuma kopyalayın ve belirtin **SCOPE_CPP_SDK** olarak.

    Ortam değişkeni ayarlamaya ek olarak, belirtebilirsiniz **- CppSDK** komut satırı kullanırken bağımsız değişkeni. Bu bağımsız değişken varsayılan CppSDK ortam değişkeni üzerine yazar.

- Ayarlama **LOCALRUN_DATAROOT** ortam değişkeni.

    Adlı yeni bir ortam değişkeni tanımlamak **LOCALRUN_DATAROOT** veri kök dizinine işaret eder.

    Ortam değişkeni ayarlamaya ek olarak, belirtebilirsiniz **- DataRoot** bağımsız değişkeni ile komut satırını kullanırken veri kök yolu. Bu bağımsız değişken varsayılan veri kök ortam değişkeni üzerine yazar. Böylece tüm işlemleri için varsayılan veri kök ortam değişkeni üzerine çalıştırdığınız her komut satırı bu bağımsız değişken eklemeniz gerekir.

### <a name="sdk-command-line-usage-samples"></a>SDK komut satırı kullanım örnekleri

#### <a name="compile-and-run"></a>Derleyin ve çalıştırın

**Çalıştırmak** komutu, komut dosyasını derleyin ve derlenmiş sonuçları yürütmek için kullanılır. Komut satırı bağımsız değişkenlerini olanlardan birleşimidir **derleme** ve **yürütme**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **çalıştırmak**:


|Bağımsız değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-Arkasındaki koda|False|Arka plan .cs kod komut dosyası var|
|-CppSDK| |CppSDK dizini|
|-DataRoot| DataRoot ortam değişkeni|Yerel çalıştırma, 'LOCALRUN_DATAROOT' ortam değişkeni varsayılan DataRoot|
|-MessageOut| |Bir dosya konsola iletilerde dökümü|
|-Paralel|1|Plan ile belirtilen paralellik çalıştırın|
|-Başvurular| |Ek başvuru derlemeleri veya arka plan, kod veri dosyaları için ayrılmış yollar listesi ';'|
|-UdoRedirect|False|Udo derleme yeniden yönlendirme yapılandırması oluşturma|
|-UseDatabase|ana|Geçici derleme kaydı arka plan kod için kullanılacak veritabanı|
|-Verbose|False|Çalışma zamanı ayrıntılı çıkışlarından Göster|
|-WorkDir|Geçerli dizin|Derleyici kullanım ve çıktı dizini|
|-RunScopeCEP|0|ScopeCEP modunu kullanmak için|
|-ScopeCEPTempPath|Temp|Veri akış için kullanılacak geçici yol|
|-OptFlags| |İyileştirici bayrakların virgülle ayrılmış listesi|


Bir örneği aşağıda verilmiştir:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Birleştirme yanı sıra **derleme** ve **yürütme**, derleyip derlenmiş yürütülebilir dosyalar ayrı ayrı çalıştırın.

#### <a name="compile-a-u-sql-script"></a>U-SQL komut dosyası derleme

**Derleme** komutu yürütülebilir dosyalar için bir U-SQL betiği derlemek için kullanılır.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **derleme**:


|Bağımsız değişken|Açıklama|
|--------|-----------|
| -Arkasındaki koda [varsayılan değer 'False']|Arka plan .cs kod komut dosyası var|
| -CppSDK [varsayılan değer '']|CppSDK dizini|
| -DataRoot [varsayılan değer 'DataRoot ortam değişkeni']|Yerel çalıştırma, 'LOCALRUN_DATAROOT' ortam değişkeni varsayılan DataRoot|
| -MessageOut [varsayılan değer '']|Bir dosya konsola iletilerde dökümü|
| -Başvuran [varsayılan değer '']|Ek başvuru derlemeleri veya arka plan, kod veri dosyaları için ayrılmış yollar listesi ';'|
| -Basit [varsayılan değer 'False']|Basit derleme|
| -UdoRedirect [varsayılan değer 'False']|Udo derleme yeniden yönlendirme yapılandırması oluşturma|
| -UseDatabase [varsayılan değer 'master']|Geçici derleme kaydı arka plan kod için kullanılacak veritabanı|
| -WorkDir [varsayılan değer 'Geçerli dizin']|Derleyici kullanım ve çıktı dizini|
| -RunScopeCEP [varsayılan değer '0']|ScopeCEP modunu kullanmak için|
| -ScopeCEPTempPath [varsayılan değer 'temp']|Veri akış için kullanılacak geçici yol|
| -OptFlags [varsayılan değer '']|İyileştirici bayrakların virgülle ayrılmış listesi|


Bazı kullanım örnekleri aşağıda verilmiştir.

U-SQL komut dosyası derleme:

    LocalRunHelper compile -Script d:\test\test1.usql

U-SQL komut dosyasını derleyin ve veri kök klasörünü ayarlayın. Bu ayarla ortam değişkeni üzerine unutmayın.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

U-SQL betiği derlemek ve bir çalışma dizini, referans derlemesini ve veritabanı ayarlayın:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Derlenmiş sonuçları yürütme

**Yürütme** komutu derlenmiş sonuçları yürütmek için kullanılır.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **yürütme**:

|Bağımsız değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-DataRoot | '' |Meta veri yürütme için veri kökü. İçin varsayılan olarak **LOCALRUN_DATAROOT** ortam değişkeni.|
|-MessageOut | '' |İletiler bir dosyaya konsolunda dökümü.|
|-Paralel | '1' |Belirtilen paralellik düzeyi ile oluşturulan yerel çalıştırma adımlarını çalıştırmak için göstergesi.|
|-Verbose | 'False' |Ayrıntılı göstermek için gösterge çalışma zamanını şuradan çıkarır.|

Kullanım örneği aşağıdadır:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Programlama arabirimleri SDK'yi kullanın

Programlama arabirimleri tüm LocalRunHelper.exe bulunur. U-SQL SDK'sı ve C# test çerçevesi, U-SQL komut dosyası yerel test ölçeklendirmek için işlevselliğini tümleştirmek için bunları kullanabilirsiniz. Bu makalede, bu arabirimleri, U-SQL betiğini sınamak için nasıl kullanılacağını göstermek için standart C# birim testi projesi kullanacağım.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>1. adım: C# birim testi projesi ve yapılandırma oluşturma

- Dosyası aracılığıyla bir C# birim testi projesi oluşturma > Yeni > Proje > Visual C# > Test > birim testi projesi.
- Proje için bir başvuru olarak LocalRunHelper.exe ekleyin. LocalRunHelper.exe \build\runtime\LocalRunHelper.exe Nuget paketi bulunur.

    ![Azure Data Lake U-SQL SDK'sı başvurusu ekleme](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL SDK **yalnızca** destek x64 ortamı yapı platform hedefi x64 ayarladığınızdan emin olun. Bu proje özelliği üzerinden ayarlayabilirsiniz > Yapı > Platform hedefi.

    ![Azure Data Lake U-SQL SDK'sını yapılandırmak x64 proje](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Test ortamınızı x64 ayarladığınızdan emin olun. Visual Studio'da Test ayarlayabilirsiniz > Test Ayarları > Varsayılan İşlemci mimarisi > x64.

    ![Azure Data Lake U-SQL SDK'sını yapılandırmak x64 Test Ortamı](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Çalışma dizini altında ProjectFolder\bin\x64\Debug genellikle olan projeye NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyaları kopyaladığınızdan emin olun.

### <a name="step-2-create-u-sql-script-test-case"></a>2. adım: U-SQL betiği test çalışması oluşturma

U-SQL betiği testi için örnek kod aşağıda verilmiştir. Test etmek için komut dosyaları, giriş dosyaları ve beklenen çıktı dosyalarını hazırlamanız gerekir.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget to close MessageOutput to get logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>LocalRunHelper.exe programlama arabirimleri

LocalRunHelper.exe programlama arabirimleri çalıştırmak U-SQL yerel derleme, vb. için sağlar. Arabirimler aşağıda listelenmiştir.

**Oluşturucusu**

Ortak LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parametre|Tür|Açıklama|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|Çıktı iletileri için konsol kullanmak için sıfıra ayarlayın|

**özellikleri**

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|AlgebraPath|dize|Cebiri dosyasının yolunu (cebiri dosya biridir derleme sonuçları)|
|CodeBehindReferences|dize|Komut dosyası başvuruları arkasında ek kod varsa, ile ayrılmış yollar belirtin ';'|
|CppSdkDir|dize|CppSDK dizini|
|CurrentDir|dize|Geçerli dizin|
|DataRoot|dize|Veri kök yolu|
|DebuggerMailPath|dize|Hata ayıklayıcı yuvası yolu|
|GenerateUdoRedirect|bool|Biz yeniden yönlendirmeyi geçersiz kılma config yüklenirken derleme oluşturmak istiyorsanız|
|HasCodeBehind|bool|Komut dosyası arka plan kodu varsa|
|InputDir|dize|Giriş verileri için dizin|
|MessagePath|dize|İleti döküm dosyası yolu|
|OutputDir|dize|Çıktı verileri için dizin|
|Paralellik|Int|Paralellik cebiri çalıştırmak için|
|ParentPid|Int|Hizmet çıkmak için izleyen üst PID 0 olarak ayarlayın veya yoksaymak için negatif|
|ResultPath|dize|Sonuç döküm dosyası yolu|
|RuntimeDir|dize|Çalışma zamanı dizini|
|ScriptPath|dize|Komut dosyası nerede|
|Basit|bool|Derleme veya basit|
|TempDir|dize|Geçici dizin|
|UseDataBase|dize|Geçici derleme kaydı, varsayılan olarak ana arka plan kod için kullanmak istediğiniz veritabanını belirtin|
|WorkDir|dize|Tercih edilen çalışma dizini|


**Yöntemi**

|Yöntem|Açıklama|Dönüş|Parametre|
|------|-----------|------|---------|
|Ortak bool DoCompile()|U-SQL komut dosyası derleme|Başarı true| |
|Ortak bool DoExec()|Derlenmiş sonuç yürütme|Başarı true| |
|Ortak bool DoRun()|U-SQL betiği (derleme + Execute) çalıştırın|Başarı true| |
|Ortak bool IsValidRuntimeDir (dize yolu)|Belirtilen yolun geçerli çalışma zamanı yolu olup olmadığını denetleyin|TRUE olarak geçerli|Çalışma zamanı dizinin yolu|


## <a name="faq-about-common-issue"></a>Yaygın sorun hakkında SSS

### <a name="error-1"></a>1. hata:
E_CSC_SYSTEM_INTERNAL: İç hata oluştu! Dosya veya derleme 'ScopeEngineManaged.dll' ya da bağımlılıklarından biri yüklenemedi. Belirtilen modül bulunamadı.

Lütfen aşağıdakileri denetleyin:

- X64 olduğundan emin olun ortamı. Derleme hedef platformu ve test ortamı x64 olması, başvurmak **1. adım: oluşturmak C# birim testi projesi ve yapılandırma** üstünde.
- Çalışma dizini projeye NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyaları kopyalandığından emin olun.


## <a name="next-steps"></a>Sonraki adımlar

* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Tanılama bilgileri günlüğe kaydetmek için bkz: [Azure Data Lake Analytics için tanılama günlükleri erişme](data-lake-analytics-diagnostic-logs.md).
* Daha karmaşık bir sorgu görmek için bkz: [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
* İş ayrıntılarını görüntülemek için bkz: [kullanım iş tarayıcı ve Azure Data Lake Analytics işleri için iş görünümünde](data-lake-analytics-data-lake-tools-view-jobs.md).
* Köşe yürütme görünümü kullanmak için bkz: [köşe yürütme görünümü Visual Studio için Data Lake araçları kullanmak](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
