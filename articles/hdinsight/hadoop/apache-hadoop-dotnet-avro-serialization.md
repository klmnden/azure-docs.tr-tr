---
title: Apache Hadoop - Microsoft Avro Library - azure'da verileri seri hale getirme
description: Hadoop bellek, veritabanı veya dosya kalıcı hale getirmek için Microsoft Avro Library kullanarak HDInsight üzerinde verileri seri hale getrime ve öğrenin.
keywords: Avro, hadoop avro
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.custom: hdiseo17may2017
ms.openlocfilehash: 9727a990548977e0b07710d879881669161c7a4c
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015271"
---
# <a name="serialize-data-in-apache-hadoop-with-the-microsoft-avro-library"></a>Microsoft Avro library Apache Hadoop, verileri seri hale getirme

>[!NOTE]
>Avro SDK'sı artık Microsoft tarafından desteklenir. Desteklenen açık kaynak topluluğu kitaplığıdır. Kitaplık kaynaklarını bulunan [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Bu konu nasıl kullanılacağını gösterir [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) bellek, bir veritabanı veya dosya kalıcı olması için akış içine nesneleri ve diğer veri yapılarını serileştirmek için. Ayrıca bunları özgün nesneleri kurtarmak için seri durumdan işlemini de gösterir.

[!INCLUDE [windows-only](../../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> Microsoft.NET ortam için Apache Avro verileri seri hale getirme sistem uygular. Apache Avro sıkıştırılmış ikili veri değişim biçimi serileştirme için sağlar. Kullandığı <a href="http://www.json.org" target="_blank">JSON</a> dil birlikte çalışabilirliğini sağlayan bir dilden şemasını tanımlamak için. Tek bir dilde seri hale getirilmiş verilerin başka bir programda okuyabilirsiniz. Şu anda C, C++, C#, Java, PHP, Python ve Ruby desteklenir. Biçim hakkında ayrıntılı bilgi bulunabilir <a href="https://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro belirtimi</a>. 

>[!NOTE]
>Microsoft Avro Library Bu belirtim uzak yordam çağrılarını (RPC) parçası desteklemez.
>

Avro sistemindeki bir nesne seri hale getirilmiş gösterimini iki bölümden oluşur: şema ve gerçek değer. Avro şemanın JSON ile seri hale getirilmiş verilerin dilden bağımsız veri modelini açıklar. Bu veri ikili gösterimi ile yan yana sunulur. İkili temsilinden ayrı şemasına sahip her nesne serileştirme hızlı ve temsili küçük yapmadan hiçbir değer başına ek yüklerini ile yazılmasına izin verir.

## <a name="the-hadoop-scenario"></a>Hadoop senaryosu
Apache Avro serileştirme biçimi, Azure HDInsight ve Apache Hadoop diğer ortamlarda yaygın olarak kullanılır. Avro Hadoop MapReduce işi karmaşık veri yapılarını temsil etmek için kullanışlı bir yol sağlar. Avro dosyalarının (Avro nesne kapsayıcısı dosyası) biçimi, dağıtılmış MapReduce programlama modelini desteklemek için tasarlanmıştır. Dağıtım sağlayan anahtar dosyaların "bölünebilir" bir dosyada herhangi bir noktaya arama ve böylelikle bloktan itibaren okumaya başlamak özelliğidir.

## <a name="serialization-in-avro-library"></a>Avro Kitaplığı'nda seri hale getirme
Avro için .NET kitaplığı nesneleri serileştirmek iki yöntemle destekler:

* **Yansıma** -türleri için JSON şeması sözleşme öznitelikleri serileştirilecek .NET türleri verilerden otomatik olarak oluşturulur.
* **Genel kayıt** -bir JSON şema tarafından temsil edilen bir kayıttaki belirtilen açıkça [ **AvroRecord** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) sınıfının hiçbir .NET türleri serileştirilecek veriler için şema tanımlamak için mevcut olduğunda.

Veri şeması yazıcı ve akışın Okuyucu için biliniyorsa, veri şemasına gönderilebilir. Bir Avro nesne kontejner soubor kullanıldığında durumlarda şema dosyasında depolanır. Veri sıkıştırma için kullanılan codec gibi diğer parametrelerle belirtilebilir. Bu senaryolar daha ayrıntılı olarak açıklanan ve aşağıdaki kod örneklerinde gösterilmektedir:

## <a name="install-avro-library"></a>Avro Kitaplığı'nı yükleyin
Kitaplık yüklemeden önce aşağıdakiler gereklidir:

* <a href="https://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 veya üzeri)

> [!Note]
> Microsoft Avro Library artık bir NuGet paketi olarak kullanılabilir. Avro kitaplığı kopya kullanmak istiyorsanız, [Microsoft.Hadoop.Avro Github deposu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) ve kod makinenizde derleyin.

## <a name="compile-schemas-using-avro-library"></a>Avro kitaplığı kullanarak şemaları derleme
Microsoft Avro Library önceden tanımlanmış JSON şemasını temel alınarak otomatik olarak C# türleri oluşturma olanak sağlayan bir kod oluşturma yardımcı içerir. Kod oluşturma yardımcı programı, ikili bir yürütülebilir dosya olarak dağıtılmaz, ancak aşağıdaki yordamı kolayca oluşturulabilir:

1. HDInsight SDK kaynak kodundan en son sürümünü içeren .zip dosyasını indirdikten <a href="https://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">için Microsoft .NET SDK'sı Hadoop</a>. (Tıklayın **indirme** simgesine değil **indirir** sekmesini.)
2. HDInsight SDK makinede bir dizine yüklenir ve gerekli bağımlılık NuGet paketlerini karşıdan yüklemek için İnternet'e bağlı .NET Framework 4 ile ayıklayın. Aşağıda, kaynak kodu için C:\SDK ayıklanır varsayılır.
3. C:\SDK\src\Microsoft.Hadoop.Avro.Tools klasöre gidin ve build.bat çalıştırın. (.NET Framework'ün 32-bit dağıtım noktasından MSBuild dosyasını çağırır. Build.bat, dosyanın yorumları takip düzenleyin. 64 bit sürümünü kullanmak istiyorsanız) Derleme başarılı olduğundan emin olun. (Bazı sistemlerde, MSBuild uyarılar oluşturabilir. Derleme hataları var olduğu sürece bu uyarıları yardımcı programı etkilemez.)
4. Derlenmiş yardımcı programı, C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools içinde bulunur.

Komut satırı sözdizimi hakkında bilgi edinmek için kod oluşturma yardımcı programı bulunduğu klasöründen aşağıdaki komutu yürütün: `Microsoft.Hadoop.Avro.Tools help /c:codegen`

Yardımcı program test etmek için kaynak kodu ile sağlanan örnek JSON şema dosyasından C# sınıfları oluşturabilirsiniz. Aşağıdaki komutu yürütün:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Bu iki C# geçerli dizindeki dosyaları üretmek beklenir: SensorData.cs ve Location.cs.

JSON şemasını C# türlerine dönüştürme sırasında kod oluşturma yardımcı programını kullanarak mantıksal anlamak için GenerationVerification.feature C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc içinde bulunan dosyasına bakın.

Ad alanları, önceki paragrafta bahsedilen dosyasında açıklanan mantığı kullanarak JSON şemadan çıkarılır. Ayıklanan Şema ad alanları, ne olursa olsun ile komut satırı yardımcı programını /n parametresinde sağlanan üzerinden öncelik kazanır. Şema içinde yer alan ad alanlarını geçersiz kılmak istiyorsanız, /nf parametresini kullanın. Örneğin, tüm ad alanları için my.own.nspace SampleJSONSchema.avsc değiştirmek için aşağıdaki komutu yürütün:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a>Örnekler hakkında
Bu konuda sağlanan altı örnekler Microsoft Avro Library tarafından desteklenen farklı senaryolar gösterir. Microsoft Avro Library, herhangi bir akışı ile çalışmak üzere tasarlanmıştır. Bu örneklerde, veri veya veritabanları kolaylık ve tutarlılık için bellek akışları yerine dosya akışları aracılığıyla yönetilebilir. Bir üretim ortamında uygulanan yaklaşıma tam senaryosu gereksinimlerini, veri kaynağı ve birim, performans kısıtlamalarına ve diğer faktörlere bağlıdır.

İlk iki örnek, yansıma ve genel kayıtları kullanarak akış arabelleklerini veri seri hale getrime ve gösterilmektedir. Bu iki durumda şemada okuyucular ve yazıcılar arasında paylaşılan varsayılır bant dışı.

Üçüncü ve dördüncü örnekler Avro nesne kapsayıcı dosyalarını kullanarak verileri seri hale getrime ve nasıl gösterir. Veriler bir Avro kapsayıcı dosyasında depolanır, seri durumundan çıkarma için şema paylaşılması gerekir çünkü şeması her zaman birlikte depolanır.

İlk dört örnekler içeren örnek indirilebileceğini <a href="https://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure Kod örnekleri</a> site.

Beşinci örnek özel sıkıştırma codec Avro nesne kapsayıcısı dosyalar için nasıl kullanılacağını gösterir. Bu örnekte indirilebileceğini için kodu içeren bir örnek <a href="https://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure Kod örnekleri</a> site.

Altıncı örnek Avro serileştirme verilerini Azure Blob Depolama'ya yükler ve bir HDInsight (Hadoop) kümesi ile Hive'ı kullanarak çözümlemek için nasıl kullanılacağını gösterir. Dan indirilebilir <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure Kod örnekleri</a> site.

Konu başlığı altında açıklanan altı örnekler için bağlantılar şunlardır:

* <a href="#Scenario1">**Yansıma ile serileştirme** </a> -serileştirilecek türleri için JSON şeması sözleşme öznitelikleri verilerden otomatik olarak oluşturulur.
* <a href="#Scenario2">**Genel kayıt ile serileştirme** </a> -hiçbir .NET türü için yansıma kullanılabilir olduğunda JSON şeması bir kayıtta açıkça belirtilen.
* <a href="#Scenario3">**Yansıma ile nesne kapsayıcısı dosyaları kullanarak serileştirme** </a> -JSON şema otomatik olarak oluşturulur ve paylaşılan bir Avro nesne kapsayıcısı dosyası aracılığıyla serileştirilmiş veriler ile birlikte.
* <a href="#Scenario4">**Genel Kayıt nesne kapsayıcısı dosyaları kullanarak serileştirme** </a> -JSON şeması açıkça serileştirme önce belirtilen ve birlikte bir Avro nesne kapsayıcısı dosyası aracılığıyla verileri paylaşılan.
* <a href="#Scenario5">**Nesne kapsayıcı dosyalarını kullanarak bir özel sıkıştırma codec bileşeni ile serileştirme** </a> -örnek bir Avro nesne kontejner soubor Deflate veri sıkıştırma codec bileşeni ile özelleştirilmiş bir .NET uygulaması oluşturma işlemini gösterir.
* <a href="#Scenario6">**Avro için Microsoft Azure HDInsight hizmeti veri yükleme kullanmayı** </a> -örnek Avro serileştirme HDInsight hizmetiyle nasıl etkileşim kurduğu gösterilmektedir. Bu örneği çalıştırmak için bir etkin Azure aboneliği ve Azure HDInsight kümesine erişim gerekir.

## <a name="Scenario1"></a>Örnek 1: Yansıma serileştirme
Türleri için JSON şeması, otomatik olarak sözleşme öznitelikleri serileştirilecek C# nesne verilerden yansıma aracılığıyla Microsoft Avro Library tarafından oluşturulabilir. Microsoft Avro Library oluşturur bir [ **IAvroSeralizer<T>**  ](https://msdn.microsoft.com/library/dn627341.aspx) serileştirilecek alanlarını tanımlamak için.

Bu örnekte, nesneleri (bir **SensorData** bir üyesine sınıfla **konumu** struct) bir bellek akışınız için seri hale getirilmiş ve bu akışı sırayla seri durumdan. Sonuç, daha sonra onaylamak için ilk örnek karşılaştırılır **SensorData** aynı nesne, kurtarılır.

Bu örnekte şema Avro nesne kapsayıcısı biçimi gerekli olmamasını sağlayacak okuyucular ve yazıcılar, arasında paylaşılacak varsayılır. Şema veri ile paylaşılacak nesne kapsayıcısı biçimiyle yansıma kullanarak veri arabellekleri seri hale getrime ve nasıl bir örnek için bkz <a href="#Scenario3">yansımailenesnekapsayıcısıdosyalarıkullanarakserileştirme</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a>Örnek 2: Serileştirme genel kayıt
Veri .NET sınıflarıyla bir veri anlaşması aracılığıyla gösterilemeyen yansıma kullanılamadığı zaman, bir JSON şeması bir genel kayıtta açıkça belirtilebilir. Bu yöntem, yansıma kullanarak daha yavaştır. Böyle durumlarda veri şemasını da diğer bir deyişle, derleme zamanında bilinen değil dinamik sahip olabilir. Avro biçimi için çalışma zamanında dönüştürülür kadar olan şema bilinmiyor virgülle ayrılmış değerler (CSV) dosyası olarak temsil edilen veri dinamik senaryosu bu tür bir örnektir.

Bu örnek nasıl oluşturup kullanacağınızı gösteren bir [ **AvroRecord** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) bir JSON şeması, verilerle doldurmak nasıl ve ardından bu seri hale getrime ve nasıl açıkça belirtmek için. Sonuç, daha sonra kurtarılmış kaydı aynı olduğundan emin olmak için ilk örnek için karşılaştırılır.

Bu örnekte şema Avro nesne kapsayıcısı biçimi gerekli olmamasını sağlayacak okuyucular ve yazıcılar, arasında paylaşılacak varsayılır. Şema serileştirilmiş verilerle birlikte dahil edilmesi gereken nesne kapsayıcısı biçimiyle genel kayıt kullanarak veri arabellekleri seri hale getrime ve nasıl bir örnek için bkz <a href="#Scenario4">nesne kapsayıcısı dosyalarıyla kullanarak serileştirme Genel kayıt</a> örnek.

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Örnek 3: nesne kapsayıcısı dosyaları ve Serileştirme ile yansıma kullanarak serileştirme
Bu örnek senaryoda benzer <a href="#Scenario1"> ilk örnek</a>, burada şemayı yansıma ile örtük olarak belirtilir. Fark, burada ise, şema, seri durumdan çıkarır okuyucu bilindiği varsayılır değil. **SensorData** serileştirilecek nesneleri ve bunların örtük olarak belirtilen şema tarafından temsil edilen bir Avro nesne kontejner soubor depolanır [ **AvroContainer** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) sınıfı.

Bu örnekte ile verileri seri [ **SequentialWriter<SensorData>**  ](https://msdn.microsoft.com/library/dn627340.aspx) ve seri durumdan çıkarılmış ile [ **SequentialReader<SensorData>**  ](https://msdn.microsoft.com/library/dn627340.aspx). Sonuç, ardından kimlik emin olmak için başlangıç örneklerine karşılaştırılır.

Aracılığıyla varsayılan nesne kapsayıcısı dosyasındaki verilerin sıkıştırılmış [ **Deflate** ] [ deflate-100] .NET Framework 4'ten sıkıştırma codec bileşeni. Bkz: <a href="#Scenario5"> beşinci örnek</a> daha yeni ve daha üst bir sürümünü kullanmayı öğrenmek için bu konudaki [ **Deflate** ] [ deflate-110] sıkıştırma codec bileşeni .NET Framework 4.5 kullanılabilir.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Örnek 4: nesne kapsayıcısı dosyaları ve Serileştirme ile genel kayıt kullanarak serileştirme
Bu örnek senaryoda benzer <a href="#Scenario2"> ikinci örnek</a>, burada şema JSON ile açıkça belirtilir. Fark, burada ise, şema, seri durumdan çıkarır okuyucu bilindiği varsayılır değil.

Test veri kümesini bir liste halinde toplanan [ **AvroRecord** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) nesneleri açıkça tanımlanmış bir JSON şeması ve tarafından temsil edilen nesne kapsayıcısı dosyasında depolanan [  **AvroContainer** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) sınıfı. Bu kapsayıcı dosya, ardından bir dosyaya kaydedilebilir bir bellek akış sıkıştırılmamış veri serileştirmek için kullanılan bir yazıcı oluşturur. [ **Codec.Null** ](https://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) okuyucu oluşturmak için kullanılan parametresi, bu verileri sıkıştırılmaz belirtir.

Ardından veriler dosyadan okunan ve nesnelerin bir koleksiyona seri durumdan. Bu koleksiyon, bunların özdeş olduğunu onaylamak için Avro kayıtları ilk listesine karşılaştırılır.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Örnek 5: bir özel sıkıştırma codec bileşeni ile nesne kapsayıcısı dosyaları kullanarak serileştirme
Beşinci örnek özel sıkıştırma codec Avro nesne kapsayıcısı dosyalar için nasıl kullanılacağını gösterir. Bu örnekte indirilebileceğini için kodu içeren bir örnek [Azure Kod örnekleri](https://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.

[Avro belirtimi](https://avro.apache.org/docs/current/spec.html#Required+Codecs) isteğe bağlı sıkıştırma codec kullanımına izin verir (Ayrıca **Null** ve **Deflate** Varsayılanları). Bu örnekte, Snappy gibi yeni bir codec uygulanmamasının (isteğe bağlı desteklenen codec olarak belirtilen [Avro belirtimi](https://avro.apache.org/docs/current/spec.html#snappy)). .NET Framework 4.5 uygulamasını kullanma işlemini gösterir [ **Deflate** ] [ deflate-110] göre daha iyi bir sıkıştırma algoritması sağlayan codec [zlib ](https://zlib.net/) varsayılan .NET Framework 4 sürümünden sıkıştırma kitaplığı.

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Örnek 6: Avro kullanarak Microsoft Azure HDInsight hizmeti için veri yükleme
Altıncı örnek Azure HDInsight hizmeti ile etkileşim için ilgili bazı programlama teknikleri gösterir. Bu örnekte indirilebileceğini için kodu içeren bir örnek [Azure Kod örnekleri](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.

Örnek, aşağıdaki görevleri gerçekleştirir:

* Mevcut bir HDInsight hizmeti kümesine bağlanır.
* Birden çok CSV dosyalarına serileştirir ve sonuçta Azure Blob depolama alanına yükler. (CSV dosyaları örnek ile birlikte dağıtılan ve ayıklama tarafından dağıtılan AMEX stok geçmiş verilerden temsil [Infochimps](https://www.infochimps.com/) 1970 2010 süre. Örnek CSV dosyası verilerine okur, kayıtları örneğine dönüştürür **hisse senedi** sınıfı ve ardından yansıma kullanarak serileştirir. Stok tür tanımı bir JSON şeması Microsoft Avro Library kod oluşturma yardımcı programı aracılığıyla oluşturulur.
* Adlı yeni bir dış tablo oluşturur **Stocks** Hive ve önceki adımda verileri karşıya bağlantılar.
* Bir sorgu üzerinde Hive'ı kullanarak yürütür **Stocks** tablo.

Ayrıca, örnek önce ve önemli işlemleri gerçekleştirdikten sonra temizleme yordamı gerçekleştirir. Temizliği sırasında tüm klasör ve ilgili Azure Blob veri kaldırılır ve Hive tablo bırakıldı. Örnek komut satırından temizleme yordamı da çağırabilirsiniz.

Örnek, aşağıdaki önkoşulları vardır:

* Etkin bir Microsoft Azure aboneliği ve abonelik kimliğini
* İlgili özel anahtara sahip bir abonelik için yönetim sertifikası. Sertifikayı geçerli kullanıcının özel depolama örneği çalıştırmak için kullanılan makineye yüklenmesi gerekir.
* Etkin bir HDInsight kümesi.
* Bir Azure depolama hesabı, önceki önkoşul, ilgili birincil veya ikincil erişim anahtarı ile birlikte gelen HDInsight kümesine bağlı.

Örneği çalıştırmadan önce tüm önkoşulların bilgileri için örnek yapılandırma dosyası girilmesi gerekir. Bunu yapmanın olası iki yolu vardır:

* Örnek kök dizininde app.config dosyasını düzenleyin ve sonra örneği oluşturmak
* İlk örneği oluşturmak ve ardından yapı dizininde AvroHDISample.exe.config düzenleyin

Her iki durumda da, tüm düzenlemeleri gerçekleştirilmelidir **<appSettings>** ayarları bölümü. Açıklamalar dosyasında izleyin.
Örnek komut satırından aşağıdaki komutu yürüterek çalıştırın (burada .zip dosyasını örnekle C:\AvroHDISample için; ayıklanacak varsa varsayıldı Aksi takdirde, ilgili dosya yolu kullanın):

    AvroHDISample run C:\AvroHDISample\Data

Kümeyi oluşturan temizlemek için aşağıdaki komutu çalıştırın:

    AvroHDISample clean

[deflate-100]: https://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: https://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
