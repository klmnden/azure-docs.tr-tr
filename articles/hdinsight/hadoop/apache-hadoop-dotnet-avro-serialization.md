---
title: Hadoop - Microsoft Avro Library - Azure veri seri hale | Microsoft Docs
description: "Seri hale getirmek ve bellek, veritabanı veya dosya kalıcı hale getirmek için Microsoft Avro Library kullanarak hdınsight'ta Hadoop verileri seri durumdan öğrenin."
keywords: Avro, hadoop avro
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c78dc20d-5d8d-4366-94ac-abbe89aaac58
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: jgao
ms.custom: hdiseo17may2017
ms.openlocfilehash: 5bb2ee2b9b838cc9feca60eca6b2c721ca58ed45
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Microsoft Avro Library Hadoop'ta verileri seri hale

>[!NOTE]
>Avro SDK'sı, artık Microsoft tarafından desteklenir. Desteklenen açık kaynak topluluğu kitaplığıdır. Kitaplık için kaynakları bulunur [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Bu konuda nasıl kullanılacağını gösterir [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) belleği, bir veritabanı veya dosya sürdürmek için akışlar içine nesneleri ve diğer veri yapılarını seri hale getirmek için. Ayrıca, onları özgün nesneleri kurtarmak için seri durumdan çıkarılacak nasıl gösterir.

[!INCLUDE [windows-only](../../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> Microsoft.NET ortamı için Apache Avro verileri seri hale getirme sistemi uygular. Apache Avro sıkıştırılmış ikili veri değişim biçimi serileştirme için sağlar. Kullandığı <a href="http://www.json.org" target="_blank">JSON</a> dil birlikte çalışabilirliğini sağlayan bir dilden bağımsız şemasını tanımlamak için. Tek bir dilde seri verilerini başka bir programda okuyabilir. Şu anda C, C++, C#, Java, PHP, Python ve Ruby desteklenir. Biçim hakkında ayrıntılı bilgi bulunabilir <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro belirtimi</a>. 

>[!NOTE]
>Microsoft Avro Library bu belirtimi uzaktan yordam çağrılarını (RPC) parçası desteklemez.
>

Avro sistemindeki bir nesne seri hale getirilmiş gösterimini iki bölümden oluşur: şema ve gerçek değer. Avro şemasının JSON serileştirilmiş verilerle dilden bağımsız veri modelinin açıklar. Veri ikili gösterimidir yan yana sunulur. İkili gösterimden ayrı şemasına sahip her bir nesne seri hale getirme hızlı ve temsili küçük hale hiçbir değer başına ek yüklerine karşılık ile yazılacak izin verir.

## <a name="the-hadoop-scenario"></a>Hadoop senaryosu
Apache Avro seri hale getirme biçimi, Azure Hdınsight hem de diğer Apache Hadoop ortamlarında yaygın olarak kullanılır. Avro Hadoop MapReduce işi karmaşık veri yapılarını temsil etmek için kolay bir yol sağlar. Avro dosyalarının (Avro nesne kapsayıcısı dosyası) biçimi, dağıtılmış MapReduce programlama modelini desteklemek üzere tasarlanmıştır. Dağıtımına olanak sağlayan anahtar dosyaların "bölünebilir" olması bir dosyada herhangi bir noktaya arama ve böylelikle belirli bir bloktan itibaren okumaya başlamanız herkese açık olmasını özelliğidir.

## <a name="serialization-in-avro-library"></a>Avro Kitaplığı'nda seri hale getirme
Avro için .NET kitaplığı biçimlendiricisi nesnelerinin iki yolla destekler:

* **Yansıma** -türleri için JSON şeması serileştirilmesi için .NET türleri sözleşme özniteliklerini verilerinden otomatik olarak oluşturulur.
* **Genel kayıt** -A JSON şeması tarafından temsil edilen bir kayıttaki belirtilen açıkça [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) sınıf hiçbir .NET türleri verilerin serileştirilmesi şema açıklamak için mevcut olduğunda.

Veri şeması yazıcı ve akış Okuyucu için bilindiğinde veri olmadan, şema gönderilebilir. Avro nesne kapsayıcısı dosyası kullanıldığında durumlarda şema dosyasında depolanır. Veri sıkıştırma için kullanılan codec gibi diğer parametrelerle belirtilebilir. Bu senaryolar daha ayrıntılı olarak özetlenen ve aşağıdaki kodu örneklerde gösterilmiştir:

## <a name="install-avro-library"></a>Avro kitaplığını yükle
Kitaplık yüklemeden önce aşağıdakiler gereklidir:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 veya üzeri)

Not Newtonsoft.Json.dll bağımlılık Microsoft Avro Library yüklemesiyle otomatik olarak yüklenir. Yordam aşağıdaki bölümde sağlanır:

Visual Studio'dan aşağıdaki yordamı yüklenebilir bir NuGet paketi olarak Microsoft Avro Library dağıtılır:

1. Seçin **proje** sekmesi -> **NuGet paketlerini Yönet...**
2. Arama "Microsoft.Hadoop.Avro için" **arama çevrimiçi** kutusu.
3. Tıklatın **yükleme** düğmesine **Microsoft Azure Hdınsight Avro Kitaplığı**.

Unutmayın Newtonsoft.Json.dll (> = 6.0.4) bağımlılık da karşıdan otomatik olarak Microsoft Avro Library ile.

Microsoft Avro Library kaynak kodunu şu adresten edinilebilir [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Avro kitaplığını kullanarak şemaları derleme
Microsoft Avro Library, önceden tanımlanmış JSON şemasını temel alarak otomatik olarak C# türleri oluşturma sağlayan bir kod oluşturma yardımcı içerir. Kod oluşturma yardımcı programı bir ikili yürütülebilir dosya dağıtılmaz ancak aşağıdaki yordamı kolayca oluşturulabilir:

1. Hdınsight SDK kaynak kodundan en son sürümü ile .zip dosyasını indirdikten <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">için Microsoft .NET SDK Hadoop</a>. (Tıklatın **karşıdan** simgesi değil **indirir** sekmesi.)
2. .NET Framework yüklü ve gerekli bağımlılık NuGet paketlerini indirmek için İnternete bağlı 4 ile Hdınsight SDK makinede bir dizine ayıklayın. Aşağıda, kaynak kodu için C:\SDK ayıklanır varsayalım.
3. C:\SDK\src\Microsoft.Hadoop.Avro.Tools klasörüne gidin ve build.bat çalıştırın. (.NET Framework'ün 32-bit dağıtım noktasından MSBuild dosyasını çağırır. İçinde dosya yorumlar aşağıdaki build.bat düzenleyin. 64-bit sürümünü kullanmak istiyorsanız) Yapı başarılı olduğundan emin olun. (Bazı sistemlerinde MSBuild uyarılar oluşturabilir. Derleme hataları var olduğu sürece bu uyarılar yardımcı programı etkilemez.)
4. Derlenmiş yardımcı programı C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools içinde bulunur.

Komut satırı sözdizimi hakkında bilgi edinmek için kod oluşturma yardımcı programı bulunduğu klasöründen aşağıdaki komutu yürütün: `Microsoft.Hadoop.Avro.Tools help /c:codegen`

Yardımcı program sınamak için kaynak kodu ile sağlanan örnek JSON şema dosyasından C# sınıfları oluşturabilirsiniz. Aşağıdaki komutu yürütün:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Bu iki C# geçerli dizindeki dosyaları üretmek beklenir: SensorData.cs ve Location.cs.

C# türleri için JSON şeması dönüştürülürken kod oluşturma yardımcı programını kullanarak mantığını anlamak için GenerationVerification.feature C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc içinde bulunan dosyasına bakın.

Ad alanları, önceki paragrafta bahsedilen dosyasında açıklanan mantığı kullanarak JSON şemadan ayıklanır. Ad alanları şemadan ayıklanan ne olursa olsun yardımcı programı komut satırında /n parametresiyle sağlanan üzerinden önceliklidir. Şemanın içinde bulunan ad alanlarını geçersiz kılmak istiyorsanız, /nf parametresini kullanın. Örneğin, tüm ad alanlarını my.own.nspace için SampleJSONSchema.avsc değiştirmek için aşağıdaki komutu yürütün:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a>Örnekleri hakkında
Bu konuda sağlanan altı örnekleri Microsoft Avro Library tarafından desteklenen farklı senaryolar gösterilmektedir. Microsoft Avro Library akış ile çalışmak üzere tasarlanmıştır. Bu örneklerde, veri veya bellek akışları yerine dosya akışları için veritabanları Basitlik ve tutarlılık aracılığıyla yönetilebilir. Bir üretim ortamında uygulanan yaklaşıma, tam senaryo gereksinimleri, veri kaynağı ve birim, performans ile ilgili kısıtlamalar ve diğer etkenlere bağlıdır.

İlk iki örnek, seri hale getirmek ve yansıma ve genel kayıtları kullanarak veri akışı arabelleklerini seri durumdan gösterilmektedir. Bu iki durumlarda şema yazıcılarının ve okuyucular arasında paylaşılan varsayılır bant dışı.

Üçüncü ve dördüncü örnekler, seri hale getirmek ve Avro nesne kapsayıcısı dosyaları kullanarak verileri seri durumdan gösterilmektedir. Veriler bir Avro kapsayıcı dosyasında depolanır, şema çıkarma için paylaşılan gerekir çünkü şemasına her zaman ile depolanır.

İlk dört örnekleri içeren örnek yüklenebilir <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure Kod örnekleri</a> site.

Beşinci örnek özel sıkıştırma codec Avro nesne kapsayıcısı dosyalar için nasıl kullanılacağını gösterir. Bu örnek yüklenebilir için kodu içeren bir örnek <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure Kod örnekleri</a> site.

Altıncı örnek Avro serileştirme verileri Azure Blob depolama alanına yüklemek ve bir Hdınsight (Hadoop) kümesini Hive kullanarak çözümlemek için nasıl kullanılacağını gösterir. Adresten yüklenebilir <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure Kod örnekleri</a> site.

Bağlantılar konuda tartışılan altı örnekleri şunlardır:

* <a href="#Scenario1">**Yansıma ile seri hale getirme** </a> -türleri serileştirilmesi JSON şeması sözleşme öznitelikleri verilerinden otomatik olarak oluşturulur.
* <a href="#Scenario2">**Seri hale getirme Genel kaydıyla** </a> -.NET türü yansıma için kullanılabilir olduğunda JSON şeması bir kayıtta açıkça belirtilen.
* <a href="#Scenario3">**Yansıma ile nesne kapsayıcısı dosyaları kullanarak serileştirme** </a> -JSON şeması otomatik olarak oluşturulur ve bir Avro nesne kapsayıcısı dosyası aracılığıyla serileştirilmiş verileri birlikte paylaşılan.
* <a href="#Scenario4">**Genel kaydıyla nesne kapsayıcısı dosyaları kullanarak serileştirme** </a> -JSON şeması açıkça belirtilen önce seri hale getirme ve Avro nesne kapsayıcısı dosyası aracılığıyla verileri birlikte paylaşılan.
* <a href="#Scenario5">**Özel sıkıştırma codec ile nesne kapsayıcısı dosyaları kullanarak serileştirme** </a> -örnek bir Avro nesne kapsayıcısı dosyası Deflate veri sıkıştırma codec ile bir özelleştirilmiş .NET uygulaması oluşturmak nasıl gösterir.
* <a href="#Scenario6">**Microsoft Azure Hdınsight hizmeti için verileri karşıya yüklemeye avro kullanarak** </a> -örnek Avro serileştirme Hdınsight hizmeti ile nasıl etkileşim kurduğu gösterilmektedir. Bu örneği çalıştırmak için bir etkin Azure aboneliği ve Azure Hdınsight kümesi erişimi gerekir.

## <a name="Scenario1"></a>Örnek 1: Yansıma seri hale getirme
JSON şeması türleri için otomatik olarak serileştirilmesi için C# nesnelerini sözleşme özniteliklerini verilerden yansıma aracılığıyla Microsoft Avro Library tarafından oluşturulabilir. Microsoft Avro Library oluşturur bir [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) serileştirilmesi için alanları tanımlamak için.

Bu örnekte, nesneleri (bir **SensorData** üyesi sınıfıyla **konumu** yapısı) bir bellek akış için sıralanmış ve bu akış sırayla seri. Sonuç, daha sonra onaylamak için ilk örnek karşılaştırılır **SensorData** kurtarılan nesne asıl aynıdır.

Bu örnekte şema Avro nesne kapsayıcısı biçimi gerekli olmamasını sağlayacak şekilde okuyucuları ve yazıcıları, arasında paylaşılacak varsayılır. Seri hale getirmek ve şema verilerle paylaşılması nesne kapsayıcısı biçimiyle yansıma kullanarak bellek arabelleği veri serisi nasıl bir örnek için bkz: <a href="#Scenario3">yansıma ile nesne kapsayıcısı dosyaları kullanarak serileştirme</a>.

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


## <a name="sample-2-serialization-with-a-generic-record"></a>Örnek 2: Genel bir kayıtla seri hale getirme
Veriler veri sözleşmesi ile .NET sınıfları aracılığıyla gösterilemez yansıma kullanılamaz bir JSON şeması açıkça bir genel kayıtta belirtilebilir. Bu yöntem, yansıma kullanmaktan daha yavaştır. Böyle durumlarda, veri şeması de başka bir deyişle, derleme zamanında bilinmiyor dinamik olabilir. Çalışma zamanında Avro biçimine dönüştürülür kadar şema bilinmiyor virgülle ayrılmış değerler (CSV) dosyaları olarak gösterilen veriler dinamik senaryo bu tür bir örnektir.

Bu örnekte, oluşturma ve kullanma gösterilmektedir bir [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) JSON şeması, verilerle doldurmak nasıl ve seri hale getirmek ve bu seri nasıl açıkça belirtmek için. Sonuç sonra kurtarılmış kayıt asıl özdeş olduğunu onaylamak için ilk örnek karşılaştırılır.

Bu örnekte şema Avro nesne kapsayıcısı biçimi gerekli olmamasını sağlayacak şekilde okuyucuları ve yazıcıları, arasında paylaşılacak varsayılır. Seri hale getirmek ve şema serileştirilmiş verilerle dahil edilmelidir nesne kapsayıcısı biçimiyle genel kaydı kullanarak arabelleklerini veri serisi nasıl bir örnek için bkz: <a href="#Scenario4">genel kaydıyla nesne kapsayıcısı dosyaları kullanarak serileştirme</a> örnek.

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


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Örnek 3: Serileştirme nesne kapsayıcısı dosyaları ve seri hale getirme yansıma ile kullanma
Bu örnek senaryoda benzer <a href="#Scenario1"> ilk örnek</a>burada şema yansıma ile örtük olarak belirtilir. Fark, işte, şema, seri durumdan çıkarır okuyucu bilindiği varsayılır değil. **SensorData** nesneleri seri hale ve bunların örtük olarak belirtilen şema tarafından temsil edilen bir Avro nesne kapsayıcısı dosyasında depolanır [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) sınıfı.

Bu örnekte ile verileri seri [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) ve seri durumdan çıkarılmış ile [ **SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Sonuç kimlik emin olmak için ilk örneklerine karşılaştırılır.

Nesne kapsayıcısı dosyasındaki verilerin varsayılan sıkıştırılmış [ **Deflate** ] [ deflate-100] sıkıştırma codec .NET Framework 4. Bkz: <a href="#Scenario5"> beşinci örnek</a> daha yeni ve üstün bir sürümü kullanmayı öğrenmek için bu konudaki [ **Deflate** ] [ deflate-110] sıkıştırma codec .NET Framework 4. 5 ' kullanılabilir.

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


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Örnek 4: nesne kapsayıcısı dosyaları ve seri hale getirme Genel kaydıyla kullanarak seri hale getirme
Bu örnek senaryoda benzer <a href="#Scenario2"> ikinci örnek</a>burada şema JSON ile açıkça belirtilir. Fark, işte, şema, seri durumdan çıkarır okuyucu bilindiği varsayılır değil.

Sınama veri kümesi bir liste halinde toplanan [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) nesneleri açıkça tanımlanmış bir JSON şeması ve tarafından temsil edilen bir nesne kapsayıcısı dosyasında depolanan [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) sınıfı. Bu kapsayıcı dosyayı sıkıştırılmamış bir dosyaya ardından kaydedilen bellek akış verilerini seri hale getirmek için kullanılan bir yazıcı oluşturur. [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) okuyucu oluşturmak için kullanılan parametresi, bu verileri sıkıştırılmaz belirtir.

Veri sonra dosyadan okunan ve nesneleri koleksiyona seri. Bu koleksiyon, bunların özdeş olduğunu onaylamak için Avro kayıt ilk listesine karşılaştırılır.

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




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Örnek 5: özel sıkıştırma codec ile nesne kapsayıcısı dosyaları kullanarak seri hale getirme
Beşinci örnek özel sıkıştırma codec Avro nesne kapsayıcısı dosyalar için nasıl kullanılacağını gösterir. Bu örnek yüklenebilir için kodu içeren bir örnek [Azure Kod örnekleri](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.

[Avro belirtimi](http://avro.apache.org/docs/current/spec.html#Required+Codecs) bir isteğe bağlı sıkıştırma codec kullanımına izin verir (ek olarak **Null** ve **Deflate** Varsayılanları). Bu örnekte yeni codec Snappy gibi uygulama değil (desteklenen isteğe bağlı codec olarak belirtilen [Avro belirtimi](http://avro.apache.org/docs/current/spec.html#snappy)). .NET Framework 4.5 uygulaması kullanmayı gösterir [ **Deflate** ] [ deflate-110] göre daha iyi bir sıkıştırma algoritması sağlayan codec [zlib](http://zlib.net/) varsayılan .NET Framework 4 sürümünden sıkıştırma kitaplığı.

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

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Örnek 6: Microsoft Azure Hdınsight hizmeti için verileri karşıya yüklemek Avro kullanma
Altıncı örnek Azure Hdınsight hizmetiyle etkileşim için ilgili bazı programlama tekniklerinin gösterir. Bu örnek yüklenebilir için kodu içeren bir örnek [Azure Kod örnekleri](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.

Örnek aşağıdaki görevleri gerçekleştirir:

* Varolan bir Hdınsight hizmet kümeye bağlanır.
* Birkaç CSV dosyaları serileştirir ve sonuç Azure Blob depolama alanına yükler. (CSV dosyaları örnek ile birlikte dağıtılır ve AMEX stok geçmiş verileri tarafından dağıtılan bir ayıklama temsil [Infochimps](http://www.infochimps.com/) 1970'ten 2010 süre. Örnek CSV dosyası verilerini okur ve örneklerine kayıtları dönüştürür **hisse senedi** sınıfı ve ardından yansıma kullanarak serileştirir. Stok tür tanımı JSON şeması Microsoft Avro Library kod oluşturma yardımcı programı aracılığıyla oluşturulur.
* Adlı yeni bir dış tablo oluşturur **istediğiniz hisse** Hive ve önceki adımda verileri karşıya bağlantılar.
* Üzerinden Hive kullanarak bir sorguyu yürüten **istediğiniz hisse** tablo.

Ayrıca, örnek önce ve sonra ana işlemlerini gerçekleştirme temizleme yordamı gerçekleştirir. Temizlemenin sırasında tüm ilgili Azure Blob verileri ve klasörleri kaldırılır ve Hive tablosu bırakılır. Örnek komut satırından temizleme yordamı da çağırabilirsiniz.

Örnek aşağıdaki önkoşullar vardır:

* Etkin bir Microsoft Azure aboneliği ve abonelik kimliği
* İlgili özel anahtara sahip bir abonelik için bir yönetim sertifikası. Sertifika geçerli kullanıcı özel depolama örneği çalıştırmak için kullanılan makineye yüklenmesi gerekir.
* Etkin bir Hdınsight kümesi.
* Bir Azure Storage hesabı, önceki önkoşul karşılık gelen birincil veya ikincil erişim anahtarı ile birlikte gelen Hdınsight kümesine bağlı.

Örneği çalıştırmadan önce tüm bilgileri önkoşullardan örnek yapılandırma dosyasına girilmesi gerekir. Bunu yapmanın iki olası yolu vardır:

* Örnek kök dizininde app.config dosyasını düzenleyin ve örnek oluşturma
* İlk örneği oluşturmak ve ardından AvroHDISample.exe.config yapı dizininde düzenleyin

Her iki durumda da, tüm düzenlemeleri yapılmalıdır '  **<appSettings>**  ayarları bölümü. Dosya açıklamaları izleyin.
Örnek komut satırından şu komutu yürüterek çalıştırın (burada örnekle .zip dosyası için C:\AvroHDISample; ayıklanacak kabul Aksi halde, ilgili dosya yolunu kullan):

    AvroHDISample run C:\AvroHDISample\Data

Kümeyi temizlemek için aşağıdaki komutu çalıştırın:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
