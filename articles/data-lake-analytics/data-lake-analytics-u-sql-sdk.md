---
title: Çalıştırma ve yerel olarak Azure Data Lake U-SQL SDK'sını kullanarak U-SQL işlerini test etme
description: Çalıştırma ve komut satırını kullanarak yerel olarak ve programlama arabirimleri yerel iş istasyonunuzda U-SQL işlerini test etme hakkında bilgi edinin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanacai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 03/01/2017
ms.openlocfilehash: 14908225e78b79cb748e712ae23643ddde4a4242
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813503"
---
# <a name="run-and-test-u-sql-with-azure-data-lake-u-sql-sdk"></a>Çalıştırma ve U-SQL Azure Data Lake U-SQL SDK'sı ile test etme

U-SQL betiği geliştirirken çalıştırmak için genel ve yerel olarak test U-SQL betiği önce göndermek, bulut. Azure Data Lake, Azure Data Lake U-SQL SDK'sı olarak adlandırılan bu senaryo için bir Nuget paketi sunar, U-SQL çalıştırın ve test, kolayca yapabilecekleriniz aracılığıyla ölçeklendirin. Bu U-SQL test derleme otomatikleştirin ve test etmek için CI (sürekli tümleştirme) sistemiyle tümleştirmek mümkündür.

Verdiğiniz ise nasıl el ile yerel çalıştırın ve Azure Data Lake araçları Visual Studio için kullanabileceğiniz sonra U-SQL betiği GUI araçları ile hata ayıklama. Daha fazla bilgi [burada](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Yükleme Azure Data Lake U-SQL SDK'sı

Azure Data Lake U-SQL SDK'sı alabilirsiniz [burada](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) Nuget.org üzerinde. Ve kullanmadan önce aşağıdaki gibi bağımlılıkları olduğundan emin olmak gerekir.

### <a name="dependencies"></a>Bağımlılıkları

Data Lake U-SQL SDK'sı, aşağıdaki bağımlılıkları gerektirir:

- [Microsoft .NET Framework 4.6 veya daha yeni](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 ve Windows SDK'sı 10.0.10240.0 veya daha yeni (adlandırılan CppSDK bu makalede). CppSDK almanın iki yolu vardır:

  - Yükleme [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Program dosyaları klasörü altında--örneğin C:\Program Files (x86) \Windows Kits\10\ \Windows Kits\10 klasörüne sahip olacaksınız. Ayrıca Windows 10 SDK sürüm \Windows Kits\10\Lib altında bulabilirsiniz. Bu klasörleri görmüyorsanız, Visual Studio'yu yeniden yükleyin ve yükleme sırasında Windows 10 SDK'yı seçtiğinizden emin olun. Visual Studio ile yüklenir bu varsa, U-SQL yerel Derleyici bunu otomatik olarak bulur.

    ![Visual Studio için Data Lake araçları yerel Windows 10 SDK'yı çalıştırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

  - Yükleme [Visual Studio için Data Lake Araçları](https://aka.ms/adltoolsvs). Önceden paketlenmiş Visual C++ ve Windows SDK'sı dosyaları C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK bulabilirsiniz. Bu durumda, U-SQL yerel derleyici bağımlılıkları otomatik olarak bulunamıyor. İçin CppSDK yolunu belirtmeniz gerekir. Dosyaları başka bir konuma kopyalayın veya olduğu gibi kullanabilirsiniz.

## <a name="understand-basic-concepts"></a>Temel kavramlarını anlama

### <a name="data-root"></a>Veri kökü

Veri kökü "yerel depolama" yerel işlem hesabı klasördür. Data Lake Analytics hesabı Azure Data Lake Store hesabına eşdeğerdir. Farklı veri kök klasörüne değiştirme gibi farklı depolama hesabına yalnızca geçiş olur. Farklı veri kök klasörleri genellikle Paylaşılan verilere erişmek istiyorsanız, komut dosyalarınızda mutlak yollar kullanmanız gerekir. Veya, dosya sistemi simgesel bağlantılar oluştur (örneğin, **mklink** NTFS) için paylaşılan bir veri noktası için verileri kök klasörü altında.

Veri kök klasör için kullanılır:

- Veritabanı, tablolar, tablo değerli işlev (Tvf) ve derlemeleri de dahil olmak üzere, yerel meta verileri Store.
- U-SQL göreli yollar olarak tanımlanan giriş ve çıkış yol arayın. Göreli yollar kullanarak U-SQL projelerinizi Azure'a dağıtmanızı kolaylaştırır.

### <a name="file-path-in-u-sql"></a>U-SQL dosyası yolu

U-SQL betiklerini, hem göreli bir yol hem de yerel bir mutlak yol kullanabilirsiniz. Belirtilen verileri kök klasörü yol göreli yoludur. Kullanmanızı öneririz "/" betiklerinizi sunucu tarafı ile uyumlu hale getirmek için yol ayırıcı olarak. Göreli yollar ve bunların eşdeğer mutlak yollar bazı örnekleri aşağıda verilmiştir. Bu örneklerde C:\LocalRunDataRoot veri kök klasördür.

|Göreli yolu|Mutlak yol|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Çalışma dizini

U-SQL betiğini yerel olarak çalışırken, bir çalışma dizini geçerli çalışan dizin altında derleme sırasında oluşturulur. Derleme çıktıları ek olarak, bu çalışma dizini için gölge yerel yürütme için gerekli çalışma zamanı dosyaları olacaktır. Çalışma dizini kök klasörü "ScopeWorkDir" olarak adlandırılır ve çalışma dizini altındaki dosyaları aşağıdaki gibidir:

|Dizin/dosya|Dizin/dosya|Dizin/dosya|Tanım|Açıklama|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Karma dize çalışma zamanı sürümü|Gölge kopyasını yerel yürütme için gerekli çalışma zamanı dosyaları|
| |Script_66AE4909AA0ED06C| |Betik adı + betik yolu dizenin karma|Derleme çıkışlarını ve yürütme günlüğü adım|
| | |\_betik\_.abr|Derleyici çıkışı|Cebir dosyası|
| | |\_ScopeCodeGen\_.*|Derleyici çıkışı|Oluşturulan yönetilen kod|
| | |\_ScopeCodeGenEngine\_. *|Derleyici çıkışı|Üretilen yerel kod|
| | |Başvurulan derlemeler|Bütünleştirilmiş kod başvurusu|Başvurulan bütünleştirilmiş kod dosyaları|
| | |deployed_resources|Kaynak dağıtımı|Kaynak dağıtım dosyaları|
| | |xxxxxxxx.xxx[1..n]\_\*.*|Yürütme günlüğü|Günlük yürütme adımları|


## <a name="use-the-sdk-from-the-command-line"></a>Komut satırından SDK'sını kullanma

### <a name="command-line-interface-of-the-helper-application"></a>Yardımcı uygulama komut satırı arabirimi

SDK'sı directory\build\runtime altında LocalRunHelper.exe en yaygın olarak kullanılan yerel çalıştırma işlevlerin arabirimlerine sağlayan komut satırı yardımcı uygulamasıdır. Komut ve bağımsız değişken anahtarları hem büyük küçük harfe duyarlı olduğunu unutmayın. Onu çağırmak için:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Bağımsız değişkenler olmadan veya LocalRunHelper.exe çalıştırmak **yardımcı** anahtar Yardım bilgilerini görüntülemek için:

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

-  **Komut** , komutun adını verir.  
-  **Bağımsız değişkeni gerekli** sağlanmalıdır bağımsız değişkenleri listeler.  
-  **İsteğe bağlı bağımsız değişkeni** varsayılan değerlerle isteğe bağlı bağımsız değişkenleri listeler.  İsteğe bağlı Boolean bağımsız parametreleri olmayan ve, görünümlerini varsayılan değerlerine negatif anlamına gelir.

### <a name="return-value-and-logging"></a>Dönüş değeri ve günlüğe kaydetme

Yardımcı uygulama döndürür **0** başarı için ve **-1** hatası. Varsayılan olarak, yardımcı geçerli konsola istediğiniz tüm iletileri gönderir. Ancak, komutların çoğu Destek **- MessageOut path_to_log_file** çıktı bir günlük dosyasına bağlama yeniden yönlendirmeleri isteğe bağlı bağımsız değişkeni.

### <a name="environment-variable-configuring"></a>Ortam değişkeni yapılandırma

U-SQL yerel ihtiyaçlarını bağımlılıklar için belirtilen CppSDK yolu yanı sıra yerel depolama hesabı olarak belirtilen veri kökü çalıştırma. Her ikisi de bağımsız komut satırı veya ayarlanan ortam değişkeninde kendileri için ayarlayabilirsiniz.

- Ayarlama **SCOPE_CPP_SDK** ortam değişkeni.

    Visual Studio için Data Lake araçları yükleyerek Microsoft Visual C++ ile Windows SDK'sı alırsanız, şu klasörü olduğunu doğrulayın:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Adlı yeni bir ortam değişkeni tanımlamak **SCOPE_CPP_SDK** bu dizine işaret edecek şekilde. Veya klasör için başka bir konuma kopyalayın ve belirtin **SCOPE_CPP_SDK** olarak.

    Ortam değişkeni ayarlamanın yanı sıra belirtebilmeniz için **- CppSDK** kullandığınızda, komut satırı bağımsız değişkeni. Bu bağımsız değişken varsayılan CppSDK ortam değişkeninize üzerine yazar.

- Ayarlama **LOCALRUN_DATAROOT** ortam değişkeni.

    Adlı yeni bir ortam değişkeni tanımlamak **LOCALRUN_DATAROOT** veri kök dizinine işaret eder.

    Ortam değişkeni ayarlamanın yanı sıra belirtebilmeniz için **- DataRoot** bağımsız değişkenle bir komut satırı kullanırken veri kök yolu. Bu bağımsız değişken varsayılan veri kökü ortam değişkeninize üzerine yazar. Bu bağımsız değişken, tüm işlemler için varsayılan veri kökü ortam değişkenini üzerine böylece çalıştırdığınız her komut satırı eklemeniz gerekir.

### <a name="sdk-command-line-usage-samples"></a>SDK komut satırı kullanım örnekleri

#### <a name="compile-and-run"></a>Derleyin ve çalıştırın

**Çalıştırma** komutu, kodu derlemek ve ardından derlenmiş sonuçları yürütmek için kullanılır. Bu, komut satırı bağımsız değişkenlerini birleşimidir **derleme** ve **yürütme**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **çalıştırmak**:


|Bağımsız Değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-CodeBehind|False|Arka plan kod .cs betiği sahiptir|
|-CppSDK| |CppSDK dizini|
|-DataRoot| DataRoot ortam değişkeni|DataRoot için yerel çalıştırma, varsayılan olarak 'LOCALRUN_DATAROOT' ortam değişkeni|
|-MessageOut| |Konsolunda bir dosyaya ileti dökümü|
|-Paralel|1|Plan ile belirtilen paralellik çalıştırın|
|-Başvurular| |Ek başvuru bütünleştirilmiş kodları veya kod arkasında, veri dosyaları tarafından ayrılmış yollar listesi ';'|
|-UdoRedirect|False|Udo derleme yeniden yönlendirme yapılandırması oluşturma|
|-UseDatabase|ana|Geçici bir bütünleştirilmiş kod kaydı arka plan kod için kullanılacak veritabanı|
|-Verbose|False|Ayrıntılı çalışma zamanı çıkışları gösterme|
|-WorkDir|Geçerli dizin|Derleyici kullanımı ve çıkış dizini|
|-RunScopeCEP|0|Kullanılacak ScopeCEP modu|
|-ScopeCEPTempPath|Temp|Akış verileri için kullanılacak geçici yol.|
|-OptFlags| |İyileştirici bayrakları virgülle ayrılmış listesi|


Bir örneği aşağıda verilmiştir:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Birleştirme yanı sıra **derleme** ve **yürütme**, derleyin ve ayrı olarak derlenen yürütülebilir yürütün.

#### <a name="compile-a-u-sql-script"></a>Derleme bir U-SQL betiği

**Derleme** komutu bir U-SQL betiği yürütülebilir dosyaları için derleme için kullanılır.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **derleme**:


|Bağımsız Değişken|Açıklama|
|--------|-----------|
| -CodeBehind [varsayılan değer 'False']|Arka plan kod .cs betiği sahiptir|
| -CppSDK [varsayılan değer '']|CppSDK dizini|
| -DataRoot [varsayılan değer 'DataRoot ortam değişkeni']|DataRoot için yerel çalıştırma, varsayılan olarak 'LOCALRUN_DATAROOT' ortam değişkeni|
| -MessageOut [varsayılan değer '']|Konsolunda bir dosyaya ileti dökümü|
| -Başvuran [varsayılan değer '']|Ek başvuru bütünleştirilmiş kodları veya kod arkasında, veri dosyaları tarafından ayrılmış yollar listesi ';'|
| -Basit [varsayılan değer 'False']|Basit bir derleme|
| -UdoRedirect [varsayılan değer 'False']|Udo derleme yeniden yönlendirme yapılandırması oluşturma|
| -UseDatabase [varsayılan değer 'master']|Geçici bir bütünleştirilmiş kod kaydı arka plan kod için kullanılacak veritabanı|
| -WorkDir [varsayılan değer: 'Geçerli dizin']|Derleyici kullanımı ve çıkış dizini|
| -RunScopeCEP [varsayılan değer '0']|Kullanılacak ScopeCEP modu|
| -ScopeCEPTempPath [varsayılan değer 'temp']|Akış verileri için kullanılacak geçici yol.|
| -OptFlags [varsayılan değer '']|İyileştirici bayrakları virgülle ayrılmış listesi|


Bazı kullanım örnekleri aşağıda verilmiştir.

U-SQL komut dosyası derleme:

    LocalRunHelper compile -Script d:\test\test1.usql

U-SQL betiği derleyin ve veri kök klasörünü ayarlayın. Bu ortam değişken Ayarla üzerine unutmayın.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

U-SQL betiği derlemek ve bir çalışma dizini, başvuru bütünleştirilmiş kodu ve veritabanı ayarlayın:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Derlenmiş sonuçları yürütün

**Yürütme** komutu derlenmiş sonuçları yürütmek için kullanılır.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

İsteğe bağlı bağımsız değişkenleri şunlardır **yürütme**:

|Bağımsız Değişken|Varsayılan değer|Açıklama|
|--------|-------------|-----------|
|-DataRoot | '' |Meta veri yürütme için veri kökü. Varsayılan **LOCALRUN_DATAROOT** ortam değişkeni.|
|-MessageOut | '' |Konsolunda bir dosyaya ileti dökümü.|
|-Paralel | '1' |Belirtilen paralellik düzeyi ile oluşturulan yerel çalıştırma adımları çalışmasını göstergesi.|
|-Verbose | 'False' |Ayrıntılı göstermek için göstergesinin çalışma zamanını şuradan çıkarır.|

Kullanım örneği aşağıda verilmiştir:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Programlama arabirimleri SDK'sını kullanma

Programlama arabirimleri tüm LocalRunHelper.exe içinde yer alır. U-SQL betiğini yerel test ölçeklendirmek için U-SQL SDK'sı ve C# test çerçevesi işlevselliğini tümleştirmek için bunları kullanabilirsiniz. Bu makalede, bu arabirimler, U-SQL betiğini sınamak için nasıl kullanılacağını göstermek için standart C# birim testi projesi kullanacağım.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>1\. adım: Oluşturma C# birim testi projesi ve yapılandırma

- Bir C# birim testi projesi dosyası aracılığıyla oluşturma > Yeni > Proje > Visual C# > Test > birim testi projesi.
- Proje için bir başvuru olarak LocalRunHelper.exe ekleyin. Nuget paketinde \build\runtime\LocalRunHelper.exe LocalRunHelper.exe bulunur.

    ![Azure Data Lake U-SQL SDK'sı başvurusu ekleme](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL SDK'sı **yalnızca** destek x64 ortamı, yapı platform hedefi x64 ayarladığınızdan emin olun. Bu proje özelliği ayarlayabilirsiniz > derleme > Platform hedefi.

    ![Azure Data Lake U-SQL SDK'sını yapılandırmak x64 proje](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Test ortamınızı x64 ayarladığınızdan emin olun. Visual Studio'da Test içinde ayarlayabilirsiniz > Test Ayarları > Varsayılan İşlemci mimarisi > x64.

    ![Azure Data Lake U-SQL SDK'sını yapılandırmak x64 Test Ortamı](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Çalışma dizini, genellikle ProjectFolder\bin\x64\Debug altında olan proje NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyaları kopyalamak emin olun.

### <a name="step-2-create-u-sql-script-test-case"></a>2\. adım: U-SQL betiği test çalışması oluştur

U-SQL betiği test için örnek kod aşağıda verilmiştir. Test etmek için betikleri, girdi dosyalarını ve beklenen Çıkış dosyalarını hazırlamanız gerekir.

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

LocalRunHelper.exe programlama arabirimleri U-SQL yerel derleme çalıştırın, vb. için sağlar. Arabirimler aşağıda listelenmiştir.

**Oluşturucusu**

Genel LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parametre|Tür|Açıklama|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|Çıkış iletileri için konsolunu kullanmak için null olarak|

**Özellikleri**

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|AlgebraPath|string|Cebir dosyası yolu (Cebir dosyası olan bir derleme sonuçları)|
|CodeBehindReferences|string|Komut dosyası başvuruları arkasında ek kodu varsa, ile ayrılmış yolları belirtin. ';'|
|CppSdkDir|string|CppSDK dizini|
|CurrentDir|string|Geçerli dizin|
|DataRoot|string|Veri kök yolu|
|DebuggerMailPath|string|Hata ayıklayıcı yuvası yolu|
|GenerateUdoRedirect|bool|Derleme yükleme oluşturmak isterseniz yeniden yönlendirme geçersiz yapılandırma|
|HasCodeBehind|bool|Betik arkasındaki kodu varsa|
|InputDir|string|Giriş verileri için dizin|
|MessagePath|string|İleti döküm dosyasının yolu|
|OutputDir|string|Çıktı verileri için dizin|
|Paralellik|int|Cebir çalıştırmak için paralellik|
|ParentPid|int|Hizmet çıkmak için izleyen üst PID 0 olarak ayarlayın veya yok saymak için negatif|
|ResultPath|string|Sonuç döküm dosyası yolu|
|RuntimeDir|string|Çalışma Zamanı Modülü dizini|
|ScriptPath|string|Betik nerede bulacağını|
|Yüzeysel|bool|Derleme veya basit|
|TempDir ayarını|string|Geçici dizin|
|UseDataBase|string|Geçici bir bütünleştirilmiş kod kaydı, varsayılan olarak ana arka plan kod için kullanmak istediğiniz veritabanını belirtin|
|Workdır|string|Tercih edilen çalışma dizini|


**Yöntemi**

|Yöntem|Açıklama|döndürülecek|Parametre|
|------|-----------|------|---------|
|public bool DoCompile()|U-SQL betiği derleme|Başarılı olma durumunda true| |
|Genel bool DoExec()|Derlenen sonuçtaki yürütün|Başarılı olma durumunda true| |
|public bool DoRun()|U-SQL betiği (derleme + yürütme) çalıştırın|Başarılı olma durumunda true| |
|Genel bool IsValidRuntimeDir (dize yolu)|Belirtilen yol geçerli çalışma zamanı yolu olup olmadığını denetleyin|TRUE geçerli|Çalışma zamanı dizinin yolu|


## <a name="faq-about-common-issue"></a>Yaygın sorun hakkında SSS

### <a name="error-1"></a>1\. hata:
E_CSC_SYSTEM_INTERNAL: İç hata! Dosya veya derleme 'ScopeEngineManaged.dll' veya bağımlılıklarından biri yüklenemedi. Belirtilen modül bulunamadı.

Lütfen aşağıdakileri denetleyin:

- X64 sahip olduğunuzdan emin olun. ortam. Derleme hedef platform ve test ortamı x64 olması, sorun **1. adım: Oluşturma C# birim testi projesi ve yapılandırma** yukarıda.
- Çalışma dizini proje NugetPackage\build\runtime\ altındaki tüm bağımlılık dosyaları kopyaladığınızdan emin olun.


## <a name="next-steps"></a>Sonraki adımlar

* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Tanılama bilgilerini günlüğe kaydetmek için bkz: [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md).
* Daha karmaşık bir sorgu görmek için bkz: [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
* İş ayrıntılarını görüntülemek için bkz: [kullanımı iş tarayıcı ve Azure Data Lake Analytics işleri için iş görünümünü](data-lake-analytics-data-lake-tools-view-jobs.md).
* Köşe yürütme görünümünü kullanma hakkında bilgi için bkz: [Visual Studio için Data Lake araçlarına köşe yürütme görünümünü kullanma](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
