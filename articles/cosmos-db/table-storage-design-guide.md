---
title: Azure Cosmos DB tablo ölçekleme ve performans desteklemek için Tasarım
description: "Azure depolama Tablo Tasarımı Kılavuzu: Tasarlama ölçeklenebilir ve performansa yönelik tablolar Azure Cosmos DB'de ve Azure depolama tablosu"
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: conceptual
ms.date: 05/21/2019
author: wmengmsft
ms.author: wmeng
ms.custom: seodec18
ms.openlocfilehash: af155b5adb2e4b45412a8b84818852ed1b1c5e72
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966104"
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Azure depolama Tablo Tasarımı Kılavuzu: Tasarlama ölçeklenebilir ve performansa yönelik tablolar

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Tasarım ölçeklenebilir ve performansa yönelik tablolar performans, ölçeklenebilirlik ve maliyet gibi bir dizi etkene dikkate almanız gerekir. Şemaları ilişkisel veritabanları için daha önce tasarladık, bu konuları size tanıdık gelecektir ancak ilişkisel modelleri ve Azure tablo hizmeti depolama modelini arasındaki bazı benzerlikler olsa da, aynı zamanda birçok önemli farklar vardır. Bu farklılıklar, counter-intuitive ya da yanlış ilişkisel veritabanları ile tanıdık bir kişiye görünebilir, ancak Azure tablo hizmeti gibi bir NoSQL anahtar/değer deposu için tasarlıyorsanız, iyi oluşturamayacağınızdan anlamlı farklı tasarım genellikle neden. Tablo hizmeti (ilişkisel veritabanı terminolojisinde satır), yüksek işlem desteklemelidir veri kümeleri için veri ve varlıkların milyarlarca içerebilir bulut ölçekli uygulamaları desteklemek için tasarlanmıştır olgu, tasarım farkları birçoğu yansıtır birimler: Bu nedenle, farklı şekilde, verilerinizi nasıl depolamanın hakkında düşünün ve tablo hizmetinin nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu çok daha (ve daha düşük bir maliyetle) ölçeklendirmek, çözümünüzün etkinleştirebilirsiniz, ilişkisel veritabanını kullanan bir çözüme göre. Bu kılavuz, şu konularda yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure tablo hizmeti hakkında
Bu bölümde bazı performans ve ölçeklenebilirlik için tasarlama yakından ilgili olan tablo hizmeti anahtar özelliklerini vurgular. Azure depolama ve tablo hizmeti için yeni başladıysanız, ilk önce okuma [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md) ve [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) önce bu makalenin geri kalanında okuma . Bu kılavuzun odağı tablo hizmetinde olsa da, bazı tartışma Azure kuyruk ve Blob hizmet ve çözüm tablo hizmetinde yanı sıra bunları nasıl kullanacağınızı dahil edilir.  

Tablo hizmeti nedir? Adından, bekleyebileceğiniz gibi tablo hizmeti tablosal verileri depolamak için kullanır. Standart terminolojisinde, tablodaki her satır bir varlığı temsil eder ve bu varlığa çeşitli özelliklerini sütunları depolayın. Her varlık, bir çift tanımlanabilmesi için kendisiyle anahtar içerir ve varlık son ne zaman açtıklarını izlemek için tablo hizmetini kullanan bir zaman damgası sütunu güncelleştirildi (zaman damgası alanı otomatik olarak eklenir ve zaman damgası rastgele bir değeri ile el ile üzerine yazılamaz). Tablo hizmeti, iyimser eşzamanlılık yönetmek için bu son değiştirilme zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemleri aynı zamanda sonuç bir **ETag** son değiştirilme itibaren zaman damgası (LMT) türetilen değeri. Birbirinin yerine bunlar aynı temel alınan verilerde başvurduğundan bu belgede ETag ve LMT koşullarını göreceksiniz.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıkları depolamak için bir basit Tablo Tasarımı gösterir. Bu kılavuzda gösterilen örneklerden birçoğu, bu basit bir tasarım üzerinde temel alır.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td>Pazarlama</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Haz</td>
<td>CAO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>Bölüm</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Bölüm adı</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satış</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Şu ana kadar bu tasarım temel farklılıklar zorunlu sütunları ve öğeleri aynı tabloda birden fazla varlık türleri Depolama olanağı ile ilişkisel bir veritabanındaki tabloya benzer. Ayrıca, her biri gibi kullanıcı tanımlı Özellikler **FirstName** veya **yaş** tamsayı veya dize, yalnızca gibi ilişkisel bir veritabanındaki bir sütun gibi bir veri türü vardır. Farklı ilişkisel bir veritabanında tablo hizmeti şemasız doğasını bir özelliği aynı veri türüne her varlık üzerinde olması gerekmez, ancak. Karmaşık veri türlerini tek bir özellik depolamak için JSON veya XML gibi bir seri hale getirilmiş biçimi kullanmanız gerekir. Tablo hizmeti gibi desteklenen veri türleri, desteklenen bir tarih aralıkları, adlandırma kuralları ve boyutu sınırlaması hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).

Göreceğiniz gibi tercih ettiğiniz **PartitionKey** ve **RowKey** iyi tablo tasarımı için temeldir. Tabloda depolanan her varlık eşsiz bir bileşimiyle olmalıdır **PartitionKey** ve **RowKey**. Anahtarları gibi bir ilişkisel veritabanı tablosundaki **PartitionKey** ve **RowKey** hızlı göz atmayı sağlayan kümelenmiş bir dizin oluşturmak için değerleri dizin bulunur; ancak, tablo hizmeti herhangi oluşturmaz (daha sonra açıklanan düzenlere bazıları nasıl görünen Bu sınırlandırma çerçevesinde çalışabilirsiniz Göster) yalnızca iki Dizinli Özellikler bunlar için ikincil dizinler.  

Bir tablo, bir veya daha fazla bölümlerini oluşur ve göreceğiniz gibi birçok tasarım kararları uygun bir geçici bir çözüm seçme olacaktır **PartitionKey** ve **RowKey** çözümünüzü en iyi duruma getirme. Bir çözümü içeren tüm varlıkları bölümler halinde düzenlenmiş yalnızca tek bir tablonun oluşabilir, ancak genellikle bir çözüm birden çok tablo sahip olacaktır. Tablolar, mantıksal olarak varlıklarınızı düzenleyebilir, erişim denetim listeleri kullanarak veri erişimi yönetmenize yardımcı olmak için Yardım ve tek bir depolama işlemi kullanarak tüm bir tabloyu bırakabilirsiniz.  

### <a name="table-partitions"></a>Tablo bölümleri
Tablo adı, hesap adı ve **PartitionKey** birlikte depolama hizmetindeki tablo hizmeti, varlık depoladığı bölüm tanımlar. Adres düzeni varlıklar için bir parçası olmanın yanı sıra, bölümler işlemleri için bir kapsam tanımlama (bkz [varlık grubu işlemleri](#entity-group-transactions) aşağıda), ve tablo hizmeti nasıl ölçeklendirilir temeli oluştururlar. Bölümleri hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](../storage/common/storage-scalability-targets.md).  

Tablo hizmetinde Hizmetleri tek bir düğüm bir veya daha fazla tamamlamak bölümleri ve hizmet ölçekler dinamik olarak yük dengeleyici tarafından düğümleri arasında bölümler. Bir düğümü yük altında ise, tablo hizmeti için *bölme* bölümler çeşitli hizmet, farklı düğümlerde düğüme tarafından; trafiği kısalana hizmet yapabilirsiniz *birleştirme* sessiz düğümlerden bölüm aralığı geri tek bir düğüme.  

Daha fazla bilgi için iç ayrıntıları tablo hizmeti ve belirli bölümler hizmet yönetir nasıl incelemeye bakın [Microsoft Azure Depolama: Güçlü tutarlılık ile yüksek oranda kullanılabilir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Varlık grubu işlemleri
Tablo hizmeti, varlık grubu işlemleri (EGTs) birden fazla varlıkta atomik güncelleştirmeleri gerçekleştirmek için yalnızca yerleşik mekanizmasıdır. EGTs de denir *toplu işlemler* bazı belgelerde. Bu varlıklar aynı bölümde olmasını sağlamak için ihtiyacınız olan birden çok varlık arasında atomik işlem davranışı gereken herhangi bir zamanda EGTs yalnızca aynı bölümde (paylaşım bir tablodaki aynı bölüm anahtarı), saklanan varlıklar üzerinde bunu çalışabilir. Genellikle birden çok varlık türleri aynı tablonun (ve bölüm) tutulması ve birden fazla tablo farklı varlık türleri için kullanmayan bir nedeni de budur. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşleme için birden çok eş zamanlı EGTs gönderirseniz, aksi takdirde işleme Gecikmeli olarak EGTs arasında ortak olan varlıklar söz konusu EGTs çalıştırmayın sağlamak önemlidir.

Sıra uygular, tasarımınızın değerlendirmek olası bir denge EGTs: daha fazla bölüm kullanarak artacaktır ölçeklenebilirlik, uygulamanızın Azure düğümleri, ancak bu arasında Yük Dengeleme isteklerini özelliğini sınırlayabilir için daha fazla fırsat olduğundan Uygulamanızı atomik işlemler gerçekleştirmek ve verileriniz için güçlü tutarlılık sağlamak için. Ayrıca, vardır belirli ölçeklenebilirlik hedefleri işlemleri için tek bir düğüm bekleyebileceğiniz verimini sınırlayabilir bir bölüm düzeyinde: Azure depolama hesapları ve tablo hizmeti için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Azure depolama ölçeklenebilirlik ve performans hedefleri](../storage/common/storage-scalability-targets.md). Bu kılavuzun sonraki bölümlerde ele çeşitli yardımcı olacak stratejileri gibi bu stillerden yönetmek ve özel istemci uygulamanızın gereksinimlerine göre bölüm anahtarı seçmek için en iyi şekilde nasıl ele tasarım.  

### <a name="capacity-considerations"></a>Kapasite konuları
Aşağıdaki tablo, bir tablo hizmeti çözümü yaptığınızda tasarlarken dikkat etmeniz anahtar değerlerinden bazılarını içerir:  

| Bir Azure depolama hesabının toplam kapasite | 500 TB |
| --- | --- |
| Bir Azure depolama hesabındaki tablolar sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Bir tablodaki bölümlerin sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Bir bölümdeki varlıkların sayısı |Depolama hesabının kapasite yalnızca sınırlıdır |
| Tek bir varlık boyutu |En fazla 255 özelliklerinin en çok 1 MB (dahil olmak üzere **PartitionKey**, **RowKey**, ve **zaman damgası**) |
| Boyutu **PartitionKey** |Bir dize boyutu 1 KB'a kadar |
| Boyutu **RowKey** |Bir dize boyutu 1 KB'a kadar |
| Bir varlık grubu işlem boyutu |Bir işlem en fazla 100 varlık içerebilir ve yükü boyutu 4 MB'tan küçük olmalıdır. Bir EGT varlığın yalnızca bir kez güncelleştirebilirsiniz. |

Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo depolama oldukça Hesaplı, ancak kapasite kullanımı hem de işlem miktarını için maliyet tahminlerini değerlendirmenizi tablo hizmeti kullanan herhangi bir çözümün bir parçası olarak içermelidir. Ancak iyileştirmek için normalleştirilmişlikten çıkarılmış veya yinelenen verileri saklama birçok senaryoda performansı veya ölçeklenebilirliği çözümünüzün yararlanmak için geçerli bir yaklaşım olur. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler
Bu listeler anahtar yönergeleri tablolarınızı tasarlarken dikkat etmeniz gereken bazı özetlemenize ve bu kılavuzda bunları daha ayrıntılı daha sonra da ele alınacaktır. Bu yönergeleri için ilişkisel veritabanı tasarımından genellikle izlersiniz yönergeleri farklıdır.  

Olması için tablo hizmeti çözümünüzü tasarlama *okuma* verimli:

* ***Okuma yoğunluklu uygulamalar sorgulama için tasarlayın.*** Tablolarınızı tasarlarken, yürütülen sorguları hakkında (özellikle önemli olanları gecikme) düşünme önce varlıklarınızın nasıl güncelleştirilir hakkında düşünün. Bu genellikle, verimli ve yüksek performanslı bir çözümde de sonuçlanır.  
* ***PartitionKey ve RowKey hem sorgularınızdaki belirtin.*** *İşaret sorguları* en verimli tablo hizmeti sorguları bunlar gibi.  
* ***Varlıkları yinelenen kopyalarını depolamayı düşünün.*** Tablo depolama ucuz, bu nedenle aynı varlık daha verimli sorgular etkinleştirmek için birden çok kez (anahtarları farklı) depolama göz önünde bulundurun.  
* ***Verilerinizi normal durumdan çıkarmayı düşünün.*** Tablo depolama ucuz, böylece verilerinizi normal durumdan çıkarmayı düşünün. Örneğin, veri toplama için sorgular yalnızca tek bir varlık erişmesi gereken şekilde Özet varlıkları depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Sahip olduğunuz yalnızca anahtarları **PartitionKey** ve **RowKey**. Örneğin, varlıklara alternatif anahtarlı erişim yolları etkinleştirmek için bileşik bir anahtar değerlerini kullanın.  
* ***Sorgu projeksiyon kullanın.*** Yalnızca istediğiniz alanları seçin sorgularını kullanarak ağ üzerinden aktarım veri miktarını azaltabilirsiniz.  

Olması için tablo hizmeti çözümünüzü tasarlama *yazma* verimli:  

* ***Etkin bölümler oluşturmayın.*** Birden çok bölüm zaman herhangi bir noktada isteklerinizi yayılmış olanak tanıyan tuşlarını seçin.  
* ***Trafiğindeki ani artışları kaçının.*** Trafik makul bir süre kesintisiz ve ani trafik kaçının.
* ***Her varlık türü için ayrı bir tabloda mutlaka oluşturmayın.*** Varlık türlerinde atomik işlemler ihtiyaç duyduğunuzda, bu varlık türleri aynı tablodaki aynı bölümde depolayabilirsiniz.
* ***En fazla aktarım hızı elde etmek gerekir göz önünde bulundurun.*** Tablo hizmeti için ölçeklenebilirlik hedefleri bilmeniz ve tasarımınızı, bunları aşmasına neden olmayacak emin olmanız gerekir.  

Bu kılavuzu okumadan gibi tüm bu ilkeleri uygulamaya koymanın örnekler göreceksiniz.  

## <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo hizmeti çözümleri, yoğun, yazma yoğun veya ikisinin bir karışımı okunabilir. Bu bölümde, okuma işlemleri verimli bir şekilde destekleyin, tablo hizmeti tasarlarken göz önünde işlenmesi için gereken noktalar ele alınmaktadır. Genellikle, desteklediği işlemleri verimli bir şekilde okuyun bir tasarım de yazma işlemleri için verimli olur. Ancak, sonraki bölümde tartışılacağı yazma işlemlerini desteklemek için tasarlarken göz önünde işlenmesi için ek hususlar vardır [veri değişikliği için tasarım](#design-for-data-modification).

"Sorguları tablo hizmetinden ihtiyacı olan verileri almak için yürütmek uygulamamı gerekir?" isteyebilir, verimli bir şekilde veri okumanızı sağlamak için tablo hizmeti çözümünüzü tasarlama için iyi bir başlangıç noktası olan  

> [!NOTE]
> Tablo hizmeti ile zor ve daha sonra değiştirmek pahalı olduğundan tasarım doğru Önden almak önemlidir. Örneğin, ilişkisel bir veritabanında genellikle yalnızca mevcut bir veritabanına dizinleri ekleyerek performans sorunlarını gidermek için mümkündür: Bu tablo hizmeti ile bir seçenek değildir.  
> 
> 

Bu bölümde tablolarınızı sorgulamak için tasarlarken çözülmesi gereken önemli sorunları ele alınmaktadır. Bu bölümde ele alınan konular:

* [PartitionKey ve RowKey seçiminizi sorgu performansı nasıl etkiler](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için en iyi duruma getirme sorguları](#optimizing-queries-for-the-table-service)
* [Tablo hizmeti verileri sıralama](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçiminizi sorgu performansı nasıl etkiler
Aşağıdaki örneklerde, tablo hizmeti aşağıdaki yapıya sahip çalışan varlıkları depolamak varsayılmaktadır (örneklerin çoğu atlamak **zaman damgası** açıklık için özellik):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **PartitionKey** (bölüm adı) |Dize |
| **RowKey** (çalışan kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **Geçerlilik süresi** |Tamsayı |
| **EmailAddress** |String |

Önceki bölümde Azure tablo hizmeti genel bakış için sorgu tasarlama, doğrudan bir etkisi yoktur anahtar Azure tablo hizmeti özelliklerinin bazılarını açıklar. Bunlar, tablo hizmeti sorguları tasarlamaya yönelik aşağıdaki genel yönergeleri sonuçlanır. Aşağıdaki örneklerde kullanılan filtre söz dizimi hakkında daha fazla bilgi için REST API, tablo hizmetinden olup [varlıkları sorgulayın](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

* A ***noktası sorgusu*** kullanılacak en verimli arama ve yüksek hacimli aramaları veya en düşük gecikme gerektiren aramalar için kullanılması önerilir. Böyle bir sorguyu her ikisi de belirterek tek bir varlık verimli bir şekilde bulmak için dizinleri kullanabilirsiniz **PartitionKey** ve **RowKey** değerleri. Örneğin: $filter = (PartitionKey eq 'Satış') ve (RowKey eq '2')  
* İkinci en iyi olan bir ***aralık sorgusu*** kullanan **PartitionKey** ve bir dizi filtreleri **RowKey** birden fazla varlık döndürülecek değer. **PartitionKey** değer belirli bir bölüm tanımlar ve **RowKey** değerleri bu bölümdeki varlıkları kümesini belirleyin. Örneğin: $filter PartitionKey eq 'Satışları'değerine ve RowKey ge'nin' ve RowKey lt 'T ='  
* Üçüncü iyi bir ***bölüm tarama*** kullanan **PartitionKey** ve başka bir anahtar olmayan özellik ve bu filtreler, birden fazla varlık döndürebilir. **PartitionKey** değeri belirli bir bölüm tanımlar ve özellik değerleri için bu bölümdeki varlıkları kümesini seçin. Örneğin: $filter PartitionKey eq 'Satış' ve LastName eq 'Smith' =  
* A ***tablo tarama*** içermemesi **PartitionKey** ve tüm tablonuzda sırayla eşleşen tüm varlıkların oluşturan bölümlerin arar verimli değildir. Olsun veya olmasın, filtre kullanır, bağımsız olarak bir tablo taraması gerçekleştirecek **RowKey**. Örneğin: $filter LastName eq 'Jones' =  
* Azure tablo depolama, birden çok varlık döndüren sorgular onları sıralanmış olarak döndürür **PartitionKey** ve **RowKey** sırası. Maksimum istemci varlıklarda önlemek için seçin bir **RowKey** , en yaygın sıralama düzenini tanımlar. Azure Cosmos DB, Azure tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarı veya satır anahtarı sıralı değildir. Özellik farkları ayrıntılı bir listesi için bkz. [Azure Cosmos DB ile Azure tablo depolama tablo API'SİNDE arasındaki farklar](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior).

Kullanarak bir "**veya**" dayalı bir filtre belirtmek için **RowKey** değerleri bölüm tarama sonuçları ve bir aralık sorgusu işlenmez. Bu nedenle, aşağıdaki gibi filtreler kullanan sorguları kaçınmanız gerekir: $filter PartitionKey eq 'Satış' ve RowKey eq '121' = (veya RowKey eq '322')  

Verimli sorgular çalıştırmak için depolama istemcisi kitaplığı kullanan istemci tarafı kod örnekleri için bkz:  

* [Depolama istemci kitaplığı kullanılarak noktası sorgu yürütme](#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlık alma](#retrieving-multiple-entities-using-linq)
* [Sunucu tarafı projeksiyonu](#server-side-projection)  

Aynı tabloda depolanan birden fazla varlık türleri işleyebilir istemci tarafı kod örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
Tercih ettiğiniz **PartitionKey** EGTs (tutarlılık sağlamak üzere) kullanımını etkinleştirmek için gereken karşı varlıklarınızı (ölçeklenebilir bir çözüm sağlamak üzere) birden çok bölümler arasında dağıtmak için ihtiyaç dengelemeniz.  

Bir üst düzey, tek bir bölümde tüm varlıklarınızı depolayabilir, ancak bu çözümünüzü ölçeklenebilirliğini sınırlandırabilir ve tablo hizmeti, Yük Dengeleme isteklerini airdrop önler. Diğer uçta, yüksek oranda ölçeklenebilir olması ve istek yükünü dengelemek için tablo hizmetini sağlar, ancak bu, varlık grubu işlemleri kullanmasını önler bölüm başına tek bir varlık olarak depolayabilir.  

İdeal **PartitionKey** verimli sorgular kullanmanıza olanak sağlar ve çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli bölümleri olan biridir. Genellikle, varlıklarınızı varlıklarınızı yeterli bölümler arasında dağıtır. uygun bir özellik olduğunu bulabilirsiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgileri depolayan bir sistemde iyi PartitionKey UserID olabilir. Bölüm anahtarı olarak belirli bir kullanıcı kimliği kullanan çeşitli varlıklara olabilir. Bir kullanıcı hakkında daha fazla veri depolar her varlığın tek bir bölümde gruplandırılır ve bu nedenle bu varlıkları hala yüksek oranda ölçeklenebilir olmanın yanı sıra varlık grubu işlemleri erişilebilir.
> 
> 

Kendi seçtiğiniz ek hususlar vardır **PartitionKey** nasıl, ekleyecek, güncelleştirme ve varlıkları silmek üzere ilişkili: bakın [veri değişikliği için tasarım](#design-for-data-modification) aşağıda.  

### <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için en iyi duruma getirme sorguları
Tablo hizmeti kullanarak varlıklarınızı otomatik olarak dizinleyen **PartitionKey** ve **RowKey** tek bir kümelenmiş dizin, bu nedenle sorguları noktası nedeni değerler kullanılacak en verimli. Ancak, vardır kümelenmiş dizini dışındaki hiçbir dizinler **PartitionKey** ve **RowKey**.

Birçok tasarımı birden çok ölçüte bağlı varlıkların aramasını etkinleştirmek için gereksinimleri karşılaması gerekir. Örneğin, e-postasına erişmelerini göre çalışan varlık bulma çalışan kimliği veya son adı. Bölümüne şu desenlerden [tablo Tasarım desenleri](#table-design-patterns) bu tür bir gereksinimi ele ve tablo hizmeti ikincil dizinler sağlamaz olgu etrafında çalışma yollarını açıklar:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak alternatif sıralama düzenleri **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerleri bölüm'de ayrı veya içinde tablolar, hızlı ve verimli aramalar ve alternatif sıralama etkinleştirmek için ayrı farklı kullanarak siparişler **RowKey** değerleri.  
* [Dizin varlıklarını düzeni](#index-entities-pattern) -varlıklar listesi döndüren verimli aramalar etkinleştirmek için dizin varlıklarını korumak.  

### <a name="sorting-data-in-the-table-service"></a>Tablo hizmeti verileri sıralama

Sorgu sonuçları tablosu hizmet tarafından döndürülen göre artan düzende sıralanır **PartitionKey** göre ve ardından **RowKey**.

> [!NOTE]
> Sorgu sonuçlarını Azure DB'de Azure tablo API'si tarafından döndürülen bölüm anahtarı veya satır anahtarı sıralı değildir. Özellik farkları ayrıntılı bir listesi için bkz. [Azure Cosmos DB ile Azure tablo depolama tablo API'SİNDE arasındaki farklar](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior).

Dize değerlerini Azure depolama tablosundaki anahtarlarıdır ve sayısal değerleri doğru sıralamak emin olmak için sabit uzunluk için bunları dönüştürmeniz ve bunları sıfır ile doldurur. Örneğin, çalışan kimliği değeri olarak kullanırsanız **RowKey** bir tamsayı değeri olan çalışan kimliği dönüştürmelisiniz **123** için **00000123**. 

Çoğu uygulama farklı sırada sıralanmış veri kullanmak için gereksinimler vardır: Örneğin, çalışanlar ada göre ya da tarih katılarak sıralama. Bölümüne şu desenlerden [tablo Tasarım desenleri](#table-design-patterns) nasıl alternatif sıralama düzenleri varlıklarınızı için adres:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) - hızlı etkinleştirmek için (aynı bölümde) farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı RowKey değerlerini kullanarak.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlerde ayrı tablolarda farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı RowKey değerlerini kullanarak .
* [Günlük kuyruğu deseni](#log-tail-pattern) -almak *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  

## <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu bölümde, ekleme, güncelleştirme iyileştirmek için tasarım konuları üzerinde odaklanır ve siler. Bazı durumlarda (Tasarım stillerden yönetme teknikleri olmasına rağmen tasarımlarında ilişkisel veritabanları için yaptığınız gibi veri değişikliği için en iyi duruma getirme tasarımları karşı sorgulama için en iyi duruma getirme tasarımları arasındaki dengeyi değerlendirin gerekir farklı ilişkisel bir veritabanındaki). Bölüm [tablo Tasarım desenleri](#table-design-patterns) tablo hizmeti için bazı ayrıntılı tasarım desenleri açıklar ve bazı bu stillerden vurgular. Uygulamada, varlıklar da değiştirmek için birçok tasarım varlıkları sorgulama için en iyi duruma getirilmiş ayrıca iş bulabilirsiniz.  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a>Performansı en iyi duruma getirme, ekleme, güncelleştirme ve silme işlemleri
Bir varlığı silme ve güncelleştirmek için kullanarak tanımlayamayabilir olmalıdır **PartitionKey** ve **RowKey** değerleri. Tercih ettiğiniz bu açısından **PartitionKey** ve **RowKey** varlıkları değiştirme benzer ölçütleri için varlıklar olarak tanımlamak istediğiniz çünkü noktası sorguları desteklemek için tercih ettiğiniz izlemelisiniz için mümkün olduğunca verimli şekilde. Bulmak için bir varlık bulmaya verimsiz bir bölüm veya tablo taraması kullanmak istemediğiniz **PartitionKey** ve **RowKey** güncelleştirmek veya silmek için gereken değerleri.  

Bölümüne şu desenlerden [tablo Tasarım desenleri](#table-design-patterns) adres iyileştirme performans veya ekleme, güncelleştirme ve silme işlemleri:  

* [Yüksek hacimli düzeni Sil](#high-volume-delete-pattern) -tablo silerek varlıklarını silme; eşzamanlı silinmesi için tüm varlıkları kendi ayrı bir tabloda depolayarak yüksek hacimli varlıkları silinmesini etkinleştirme.  
* [Veri serisi deseni](#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  
* [Geniş bir varlıklar deseni](#wide-entities-pattern) -mantıksal varlıklarla birden çok 252 özellik depolamak için birden çok fiziksel varlıkları kullanın.  
* [Büyük varlıklar deseni](#large-entities-pattern) -büyük özellik değerlerini depolamak için blob depolama alanını kullanın.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Saklı varlıklarınızı tutarlılık sağlama
Seçtiğiniz veri değişiklikleri iyileştirmek için anahtarların etkileyen diğer önemli atomik bir işlem kullanarak tutarlılık sağlamak nasıl faktördür. Yalnızca bir EGT varlıklar aynı bölümde depolanır üzerinde çalışması için de kullanabilirsiniz.  

Bölümüne şu desenlerden [tablo Tasarım desenleri](#table-design-patterns) tutarlılık yönetme adresi:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak alternatif sıralama düzenleri **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlerde veya farklı tablolarda farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı kullanarak**RowKey** değerleri.  
* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern) -etkinleştirme sonunda tutarlı bir davranış bölüm sınırları veya depolama sistemi sınırları arasında Azure kuyrukları kullanılarak.
* [Dizin varlıklarını düzeni](#index-entities-pattern) -varlıklar listesi döndüren verimli aramalar etkinleştirmek için dizin varlıklarını korumak.  
* [Normalleştirilmişlikten çıkarma deseni](#denormalization-pattern) -birleştirme ilgili verilerin birlikte tek nokta sorgu ile ihtiyacınız olan tüm verileri almanızı etkinleştirmek için tek bir varlık içinde.  
* [Veri serisi deseni](#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  

Varlık grubu işlemleri hakkında daha fazla bilgi için konudaki [varlık grubu işlemleri](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Tasarımınızı etkili değişiklikler için sağlama verimli sorgular kolaylaştırır
Bu durumda, belirli bir senaryoya yönelik olup olmadığını çoğu durumda, her zaman etkili değişiklikler, ancak verimli sorgulama sonuçları için bir tasarım değerlendirmelidir. Bazı bölümünde desenlerinin [tablo Tasarım desenleri](#table-design-patterns) açıkça sorgulama ve değiştirme varlıklar arasında denge değerlendirin ve her zaman her işlem türü sayısını dikkate almanız.  

Bölümüne şu desenlerden [tablo Tasarım desenleri](#table-design-patterns) verimli sorgular için tasarlama ve verimli veri değişikliği için tasarlama arasında denge adresi:  

* [Bileşik anahtar deseni](#compound-key-pattern) -kullanım bileşik **RowKey** aramak bir istemci etkinleştirmek için değerler tek nokta sorgu verilerle ilgili.  
* [Günlük kuyruğu deseni](#log-tail-pattern) -almak *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  

## <a name="encrypting-table-data"></a>Tablo verilerini şifreleme
.NET Azure depolama istemci kitaplığı, yerleştirme için şifreleme dize varlık özelliklerini destekler ve işlemleri değiştirin. Şifrelenmiş dizeleri hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra geri dizelere dönüştürülür.    

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenmiş özelliklerini belirtmeniz gerekir. Bu, ya da (TableEntity türetilen POCO varlık için) bir [EncryptProperty] özniteliği ya da bir şifreleme çözümleyici istek seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarını, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilcidir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikleri nasıl şifrelenir etrafında mantıksal olasılığı için de sağlar. (X, örneğin, daha sonra özellik A şifrelemek; Aksi takdirde özellik A ve b şifreleme) Okurken veya varlıkları sorgulayarak bu bilgiyi sağlamak gerekli değildir.

Birleştirme şu anda desteklenmiyor. Bir özellik alt kümesi daha önce farklı bir anahtarla şifrelenmiş olabilecek olduğundan, yalnızca birleştirme yeni özellikleri ve meta verilerini güncelleştirme veri kaybına yol açar. Özellik başına yeni bir anahtar kullanarak, ikisi için de performansla ilgili nedenlerden dolayı uygun değil veya hizmetten önceden var olan bir varlığa okumak için ek hizmet çağrıları yapma ya da birleştirme gerektirir.     

Tablo verilerini şifreleme hakkında daha fazla bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../storage/common/storage-client-side-encryption.md).  

## <a name="modeling-relationships"></a>İlişkileri modelleme
Etki alanı modellerini oluşturma karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, varlıklar ve iş etki alanını anlamak ve sisteminizin tasarımına bildirmek için bir yol olarak arasındaki ilişkileri tanımlamak için modelleme işlemi kullanın. Bu bölüm, nasıl, bazı yaygın ilişki türleri tasarımı için tablo hizmeti için etki alanı modelleri bulunan çevirebilir üzerinde odaklanır. İlişkisel bir veritabanı tasarlarken kullanılan farklı bir fiziksel NoSQL tabanlı veri modeli için bir mantıksal veri modelinden eşleme işlemidir. İlişkisel veritabanları tasarım yedeklilik – ve nasıl uygulanması veritabanı işleyişi soyutlayan bir bildirim temelli sorgulanırken özelliği en aza indirmek için en iyi duruma getirilmiş veri normalleştirme işlemi genellikle varsayar.  

### <a name="one-to-many-relationships"></a>Bire çok ilişkileri
İş etki alanı nesneler arasındaki bire çok ilişkileri ortaya sık: Örneğin, bir departmandaki çalışanların çoğu vardır. Tablo hizmetinde her Artıları ve eksileri belirli senaryoya ilgili olabilecek ile bire çok ilişkileri uygulamak için birkaç yolu vardır.  

On binlerce Departmanlar ve çalışan varlıkların her departman çok sayıda çalışan ve her çalışana belirli bir bölümle ilişkili olarak sahip olduğu büyük bir Uluslararası Şirket örneği göz önünde bulundurun. Ayrı bir departman ve bunlar gibi çalışan varlıkları depolamak için bir yaklaşım şöyledir:  

![Bölüm ve çalışan varlık][1]

Bu örnek, temel türleri arasında örtük bir-çok ilişkisi gösterir **PartitionKey** değeri. Her departmandaki çalışanların çoğu olabilir.  

Bu örnek ayrıca departmanı varlık ve ilgili çalışan varlıkları aynı bölümde gösterir. Farklı bölümleri, tablolar veya hatta depolama hesapları farklı varlık türleri için kullanılacak seçebilirsiniz.  

Verileri normalleştirilmişlikten çıkarmak ve yalnızca aşağıdaki örnekte gösterildiği gibi normalleştirilmişlikten çıkarılmış departman verilerine sahip çalışan varlıklar depolamak için alternatif bir yoludur. Bunu yapmak için her çalışanın departmanındaki güncelleştirmeye gerek duyduğunuz olduğundan Bölüm Yöneticisi'nin ayrıntılar değişiklik yapabilmek için bir gereksinimi varsa bu belirli bir senaryoda en iyi normalleştirilmişlikten çıkarılmış bu yaklaşım olmayabilir.  

![Çalışan varlık][2]

Daha fazla bilgi için [normalleştirilmişlikten çıkarma deseni](#denormalization-pattern) bu kılavuzun sonraki.  

Aşağıdaki tabloda, Artıları ve eksileri her çalışan ve bir-çok ilişkisi departmanı varlıkları depolamak için yukarıda açıklanan yaklaşımlardan biri özetlenmektedir. Ne sıklıkta çeşitli işlemler gerçekleştirmek beklediğiniz düşünmelisiniz: Bu işlem yalnızca sık meydana gelirse, pahalı bir işlem içeren bir tasarım için kabul edilebilir olabilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Uzmanları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı varlık türleri, aynı bölüme, aynı tabloya</td>
<td>
<ul>
<li>Tek bir işlemle bir departman varlık güncelleştirebilirsiniz.</li>
<li>Bir EGT departmanı varlığı değiştirmek için bir gereksinimi varsa tutarlılık sağlamak için kullanabileceğiniz olduğunda, güncelleştirme/ekleme/silme çalışan varlık. Örneğin, bir departman çalışan sayısı her departman için korur.</li>
</ul>
</td>
<td>
<ul>
<li>Hem çalışan hem de bazı istemci etkinlikleri için bir bölüm varlık almanız gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimleri, bir etkin nokta bu neden olabilir.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüme taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman veya çalışan bir varlıktan tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem hacmi, bu yükü daha fazla bölümler arasında yaymak yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Hem çalışan hem de bazı istemci etkinlikleri için bir bölüm varlık almanız gerekebilir.</li>
<li>Tutarlılık sağlamak için EGTs kullanılamaz olduğunda, güncelleştirme/ekleme/silme bir çalışan ve güncelleştirme bir bölüm. Örneğin, bir çalışan sayısı departmanı varlıklardaki güncelleştiriliyor.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüme taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne normalleştirmeyi geri alın</td>
<td>
<ul>
<li>Tek bir istekle gereken tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Bölüm bilgileri (Bu, bir departmandaki tüm çalışanlar güncelleştirmenizi gerektirir) güncelleştirmeniz gerekiyorsa tutarlılık sağlamak pahalı olabilir.</li>
</ul>
</td>
</tr>
</table>

Nasıl Bu seçenekler ve Artıları ve eksileri hangisinin en önemli, belirli bir uygulama senaryolarınız bağlıdır arasında seçin. Örneğin, ne sıklıkta, departman varlıkları değiştirmeyin; Tüm çalışan sorguları ek departman bilgi gerekiyor; ölçeklenebilirlik sınırları, bölüm veya depolama hesabınız için ne kadar yakın misiniz?  

### <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri varlıklar arasında bire bir ilişkiler içerebilir. Tablo hizmetinde bire bir ilişki uygulamak gerekiyorsa, her ikisinin de almanız gerektiğinde, iki ilişkili varlık bağlama da seçmeniz gerekir. Bu bağlantıyı bağlantı biçiminde depolayarak örtük, anahtar değerlerinin bir kurala göre veya açık olabilir **PartitionKey** ve **RowKey** her varlık kendi ilgili varlık için değerler. İlgili varlıkları aynı bölümde olup saklamalısınız bir açıklaması için konudaki [bire çok ilişkileri](#one-to-many-relationships).  

Tablo hizmetinde bire bir ilişkiler uygulamak için yol açabilecek bir uygulama konuları vardır:  

* Büyük varlıklar işleme (daha fazla bilgi için [büyük varlıklar deseni](#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için [paylaşılan erişim imzaları ile erişimi denetleme](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>İstemci katılın
Tablo hizmetinde ilişkileri modellemek için yol olsa da, tablo hizmeti kullanarak iki ana nedeni ölçeklenebilirlik ve performans olduğunu unutmayın. Performans ve ölçeklenebilirlik çözümünüzün tehlikeye çok ilişkileri modelleme fark ederseniz, tablo tasarımınızla tüm veri ilişkileri oluşturmak gerekli olup olmadığını kendiniz istemeniz gerekir. Tasarımınızı basitleştirmek ve istemci uygulamanızı gerekli tüm birleştirmeler gerçekleştirme izin verirseniz, çözümünüzün performansını ve ölçeklenebilirliğini artırmak mümkün olabilir.  

Örneğin, genellikle değiştirmez veriler içeren küçük tablolar varsa, daha sonra bu verileri bir kez alabilir ve istemcide önbellek. Bu, aynı verileri almak için yinelenen gidiş-dönüş önleyebilirsiniz. Bu kılavuzda inceledik örneklerde, küçük ve istemci uygulama bir kez indirebilirsiniz veri ve arama verisi olarak önbelleği için iyi bir adaydır getirerek seyrek değiştirmek küçük bir kuruluş departmanlara kümesini olasıdır.  

### <a name="inheritance-relationships"></a>Kalıtım ilişkileri
İstemci uygulamanız bir dizi iş varlığı temsil eden bir devralma ilişkisi parçasını sınıfı kullanıyorsa, bu varlıkların tablo hizmetindeki kolayca kalıcı hale getirebilirsiniz. Örneğin, aşağıdaki istemci uygulamanızda tanımlanan sınıflar kümesini olabilir. burada **kişi** bir Özet sınıf.

![ER kalıtım ilişkileri diyagramı][3]

Bu görünüm bu gibi varlıkları kullanarak tek bir kişi tablosunu kullanarak tablo hizmetindeki iki somut sınıfların örneklerini kalıcı hale getirebilirsiniz:  

![Müşteri varlığı ve çalışan varlık diyagramı][4]

İstemci kodu aynı tabloda birden fazla varlık türleri ile çalışma hakkında daha fazla bilgi için bkz [heterojen varlık türleri ile çalışmaktan](#working-with-heterogeneous-entity-types) bu kılavuzun sonraki. Bu varlık türünde istemci kodu anlamayı örnekleri sağlar.  

## <a name="table-design-patterns"></a>Tablo Tasarım desenleri
Önceki bölümlerde bazı tartışmalar tablo tasarımınızı ekleme, güncelleştirme ve varlık verilerini silme ve sorguları kullanarak her iki varlık verileri için en iyi duruma getirme hakkında ayrıntılı gördünüz. Bu bölümde, tablo hizmeti çözümleri ile kullanım için uygun olan bazı desenleri açıklar. Ayrıca, nasıl, pratikte bazı sorunlar ve dezavantajlarına bu kılavuzda daha önce oluşturulan ele görürsünüz. Aşağıdaki diyagramda, farklı düzenlerinin arasındaki ilişkileri özetlenmektedir:  

![Tablo Tasarım desenleri görüntüsü][5]

Yukarıdaki desen eşleme desenleri (mavi) ve bu kılavuzda belirtilmiştir ters desenler (turuncu) bazı ilişkiler vurgular. Elbette, dikkate değer olan diğer birçok desen vardır. Örneğin, tablo hizmeti için ana senaryoları biri kullanmaktır [gerçekleştirilmiş görünüm düzeni](https://msdn.microsoft.com/library/azure/dn589782.aspx) gelen [komut sorgu sorumluluğu ayrımı (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) deseni.  

### <a name="intra-partition-secondary-index-pattern"></a>İçi bölüm ikincil dizin düzeni
Her varlık kullanan birden çok kopyalarını farklı Store **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve alternatif sıralama düzenleri kullanarak farklı **RowKey** değerleri. Kopya arasında güncelleştirmeleri saklanır tutarlı EGTs kullanma.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmetini kullanarak varlıkları otomatik olarak dizinleyen **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde bu değerleri kullanarak bir varlığı almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısı kullanarak bir istemci uygulaması noktası sorgu bölüm adını ve çalışan kimliği kullanarak tek çalışan varlık almak için kullanabilirsiniz ( **PartitionKey** ve **RowKey**  değerler). Bir istemci, varlıkların her departman içinde çalışan kimlik ölçütü de alabilirsiniz.

![Çalışan varlık][6]

Ayrıca e-posta adresi gibi başka bir özelliğin değerini temel alan bir çalışan varlık bulamaz istiyorsanız bir eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmeti, ikincil dizinler sağlamaz olmasıdır. Ayrıca, çalışanların farklı bir düzende sıralanmış bir listesini istemek için bir seçenek yoktur **RowKey** sırası.  

#### <a name="solution"></a>Çözüm
İkincil dizinler eksikliği geçici olarak çözmek için farklı bir kullanarak her kopya her varlığın birden çok kopyasını depolayabilirsiniz **RowKey** değeri. Aşağıda gösterilen yapıları ile varlıkta depolarsanız, verimli bir şekilde e-posta adresi veya çalışan kimliği temel alarak çalışan varlıkları alabilirsiniz. Ön ek değerleri **RowKey**, "empid_" ve "email_" tek bir çalışan veya çalışan bir dizi için bir dizi e-posta adresleri veya çalışan kimlikleri kullanarak sorgulamak etkinleştirin.  

![Çalışan varlık ile RowKey değer Çeşitleme uygulanıyor][7]

(Bir çalışan kimliği ve e-posta adresine göre bir arayan tarafından bakan) aşağıdaki iki filtre ölçütleri, her ikisi de nokta sorgu belirtin:  

* $filter = (PartitionKey eq 'Satış') ve (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Satış') ve (RowKey eq 'email_jonesj@contoso.com')  

Bir dizi çalışan varlıklar için sorgu, çalışan kimliği sırasına koyulmuş bir aralık veya uygun önekle varlıkları sorgulayarak e-posta adresi sırasına koyulmuş bir aralık belirtebilirsiniz **RowKey**.  

* Çalışan kimliği aralığı için 000100 000199 kullanımda olan Satış departmanındaki tüm çalışanlar bulmak için: $filter = (PartitionKey eq 'Satış') ve (RowKey ge 'empid_000100') ve (RowKey le 'empid_000199')  
* Satış departmanındaki tüm çalışanlar ' bir ' harfi ile başlayan bir e-posta adresiyle bulunacak: $filter = (PartitionKey eq 'Satış') ve (RowKey ge 'email_a') ve (RowKey lt 'email_b')  
  
  Yukarıdaki örneklerde kullanılan filtre söz dizimi hakkında daha fazla bilgi için REST API, tablo hizmetinden olup [varlıkları sorgulayın](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolama, yinelenen veri depolamanın maliyeti yükü büyük kaygısı olmaması gerekir kullanmak görece ucuz. Ancak, her zaman öngörülen depolama gereksinimlerinize göre tasarımınızı maliyetini değerlendirmek ve yalnızca istemci uygulamanızı yürütecek sorguları desteklemek için yinelenen varlıkları ekleyin.  
* İkincil dizin varlıkları aynı bölümde özgün varlıklar olarak depolandığından, tek bir bölüm için ölçeklenebilirlik hedefleri aşmamasını sağlayın.  
* Varlığın iki kopyasını atomik olarak güncelleştirmek için EGTs kullanarak yinelenen varlıklarınızı birbiriyle tutarlı kalmasını sağlayabilirsiniz. Bu, bir varlığın tüm kopyaları aynı bölümde saklamalısınız anlamına gelir. Daha fazla bilgi için konudaki [varlık grubu işlemleri kullanılarak](#entity-group-transactions).  
* İçin kullanılan değer **RowKey** her varlık için benzersiz olmalıdır. Bileşik anahtar değerleri kullanmayı düşünün.  
* Sayısal değerler doldurma **RowKey** göre üst ve alt sınırları filtreleme ve sıralama sağlar (örneğin, çalışan kimliği 000223), düzeltin.  
* Mutlaka varlığınızın tüm özelliklerini yinelenen gerekmez. Örneğin, e-posta kullanarak varlıkları bir şekilde sorguları içinde adresi **RowKey** hiçbir zaman çalışanın yaş'ı gerekir, bu varlıkları aşağıdaki yapıya sahip olabilir:

![Çalışan varlık][8]

* Yinelenen verileri depolamak ve bir varlık ile diğer gerekli verileri aramak için bulmak için bir sorgu kullanımı çok tek bir sorgu ile ihtiyacınız olan tüm verileri alabilirsiniz emin olmak genellikle iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı anahtarlar, çeşitli istemci farklı sıralamalar varlıkları alma gerektiğinde kullanarak varlıkları almak istemci uygulamanız gerektiğinde ve benzersiz değerler çeşitli kullanarak her bir varlık tanımlayabilirsiniz bu düzeni kullanın. Ancak, farklı kullanarak varlık aramaları gerçekleştirilirken bölüm ölçeklenebilirlik sınırları aşmadığından emin olmalıdır **RowKey** değerleri.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern)
* [Bileşik anahtar deseni](#compound-key-pattern)
* [Varlık grubu işlemleri](#entity-group-transactions)
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>İkincil dizin arası bölüm düzeni
Her varlık kullanan birden çok kopyalarını farklı Store **RowKey** değerleri bölüm'de ayrı veya etkinleştir hızlı ve verimli aramalar ve alternatif sıralama düzenleri kullanarak farklı tablolara'de ayrı **RowKey**değerleri.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmetini kullanarak varlıkları otomatik olarak dizinleyen **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde bu değerleri kullanarak bir varlığı almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısı kullanarak bir istemci uygulaması noktası sorgu bölüm adını ve çalışan kimliği kullanarak tek çalışan varlık almak için kullanabilirsiniz ( **PartitionKey** ve **RowKey**  değerler). Bir istemci, varlıkların her departman içinde çalışan kimlik ölçütü de alabilirsiniz.  

![Çalışan varlık][9]

Ayrıca e-posta adresi gibi başka bir özelliğin değerini temel alan bir çalışan varlık bulamaz istiyorsanız bir eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmeti, ikincil dizinler sağlamaz olmasıdır. Ayrıca, çalışanların farklı bir düzende sıralanmış bir listesini istemek için bir seçenek yoktur **RowKey** sırası.  

Yüksek hacimli işlemler bu varlıkları öngörerek ve istemcinizi sınırlama tablo hizmeti oranı riskini en aza indirmek istiyorsunuz.  

#### <a name="solution"></a>Çözüm
İkincil dizinler eksikliği geçici olarak çözmek için her kopya kullanarak her varlık, birden çok kopyasının farklı depolayabilirsiniz **PartitionKey** ve **RowKey** değerleri. Aşağıda gösterilen yapıları ile varlıkta depolarsanız, verimli bir şekilde e-posta adresi veya çalışan kimliği temel alarak çalışan varlıkları alabilirsiniz. Ön ek değerleri **PartitionKey**, "empid_" ve "email_" bir sorgu için kullanmak istediğiniz hangi dizini belirlemek etkinleştirin.  

![Birincil dizin ve ikincil dizin ile çalışan entity çalışan varlık][10]

(Bir çalışan kimliği ve e-posta adresine göre bir arayan tarafından bakan) aşağıdaki iki filtre ölçütleri, her ikisi de nokta sorgu belirtin:  

* $filter = (PartitionKey eq ' empid_Sales') ve (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') ve (RowKey eq 'jonesj@contoso.com')  

Bir dizi çalışan varlıklar için sorgu, çalışan kimliği sırasına koyulmuş bir aralık veya uygun önekle varlıkları sorgulayarak e-posta adresi sırasına koyulmuş bir aralık belirtebilirsiniz **RowKey**.  

* Bir aralıkta çalışan kimliği ile Satış departmanındaki tüm çalışanlar bulunacak **000100** için **000199** çalışan kimliği kullanmak sıralanır: $filter = (PartitionKey eq ' empid_Sales') ve (RowKey ge '000100') ve (RowKey le '000199')  
* E-posta adresi kullanmak sıralanmış 'a' ile başlayan bir e-posta adresiyle Satış departmanındaki tüm çalışanlar bulmak için: $filter = (PartitionKey eq ' email_Sales') ve (RowKey ge 'bir') ve (RowKey lt 'b')  

Yukarıdaki örneklerde kullanılan filtre söz dizimi hakkında daha fazla bilgi için REST API, tablo hizmetinden olup Not [varlıkları sorgulayın](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Kullanarak, yinelenen varlıklarınızı birbiriyle tutarlı sonunda tutabilirsiniz [sonunda tutarlı işlemler deseni](#eventually-consistent-transactions-pattern) birincil ve ikincil dizin varlıklarını korumak için.  
* Tablo depolama, yinelenen veri depolamanın maliyeti yükü büyük kaygısı olmaması gerekir kullanmak görece ucuz. Ancak, her zaman öngörülen depolama gereksinimlerinize göre tasarımınızı maliyetini değerlendirmek ve yalnızca istemci uygulamanızı yürütecek sorguları desteklemek için yinelenen varlıkları ekleyin.  
* İçin kullanılan değer **RowKey** her varlık için benzersiz olmalıdır. Bileşik anahtar değerleri kullanmayı düşünün.  
* Sayısal değerler doldurma **RowKey** göre üst ve alt sınırları filtreleme ve sıralama sağlar (örneğin, çalışan kimliği 000223), düzeltin.  
* Mutlaka varlığınızın tüm özelliklerini yinelenen gerekmez. Örneğin, sorgular, arama, e-posta kullanarak varlıkları adres **RowKey** hiçbir zaman çalışanın yaş'ı gerekir, bu varlıkları aşağıdaki yapıya sahip olabilir:
  
  ![İkincil dizin ile çalışan entity][11]
* Yinelenen verileri depolamak ve ikincil dizin ve başka bir arama gerekli verileri birincil dizin kullanarak bir varlık bulmak için bir sorgu kullanımı çok tek bir sorgu ile ihtiyacınız olan tüm verileri alabilirsiniz emin olmak genellikle iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı anahtarlar, çeşitli istemci farklı sıralamalar varlıkları alma gerektiğinde kullanarak varlıkları almak istemci uygulamanız gerektiğinde ve benzersiz değerler çeşitli kullanarak her bir varlık tanımlayabilirsiniz bu düzeni kullanın. Farklı kullanarak varlık aramaları gerçekleştirilirken bölüm ölçeklenebilirlik sınırlarını aşmamak istediğinizde bu düzeni kullanın **RowKey** değerleri.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern)  
* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern)  
* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Son tutarlılık işlemleri deseni
Azure kuyrukları kullanılarak bölüm sınırları veya depolama sistemi sınırları arasında sonunda tutarlı bir davranış etkinleştirin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
EGTs atomik işlemler aynı bölüm anahtarı paylaşan birden fazla varlıkta etkinleştirin. Performans ve ölçeklenebilirlik için ayrı bölümlerde veya ayrı depolama sistemi tutarlılığı gereksinimlerine sahip bir varlık depolamaya karar verebilirsiniz: Böyle bir senaryoda EGTs tutarlılık sağlamak için kullanamazsınız. Örneğin, nihai tutarlılık arasında sağlamak için bir gereksinim olabilir:  

* Varlıklar, iki farklı bölümleri aynı tabloda, farklı tablolardaki veya farklı depolama hesaplarında depolanır.  
* Tablo hizmetinde depolanan bir varlık ile Blob hizmetinde depolanan bir blob.  
* Tablo hizmeti ve bir dosya içinde bir dosya sisteminde depolanmış varlığa.  
* Tablo hizmetinde bir varlık deposu henüz Azure Search hizmetini kullanarak dizini.  

#### <a name="solution"></a>Çözüm
Azure kuyrukları kullanılarak, son tutarlılık bölümler veya depolama sistemleri arasında iki veya daha fazla sunan bir çözüm uygulayabilirsiniz.
Bu yaklaşım göstermek için eski çalışan varlıkları arşivlemek için gereksinim varsayılır. Eski çalışan varlıkları nadiren sorgulanan ve geçerli çalışanlar ile ilgili herhangi bir etkinlik dışında tutulacak. Bu gereksinimin uygulanması için etkin depolamak **geçerli** tablo ve eski çalışanları **arşiv** tablo. Bir çalışan arşivleme, gerektirir varlıktan silmenize **geçerli** tablo ve varlığa ekleme **arşiv** tablosu, ancak bir EGT iki bu işlemleri gerçekleştirmek için kullanamazsınız. Bir hata görünür bir varlık veya hiçbir tablolarında neden riskini önlemek için arşiv işlemi sonunda tutarlı olması gerekir. Aşağıdaki sıralama diyagramı, bu işlem adımları açıklanmaktadır. Metin aşağıdaki özel durum yolları için daha fazla ayrıntı sağlanır.  

![Son tutarlılık için çözüm diyagramı][12]

Bir istemci üzerinde çalışan #456 arşivlemek için bu örnekte bir Azure kuyruğuna bir ileti yerleştirerek Arşiv işlemi başlatır. Bir çalışan rolü, kuyruğu yeni iletiler için yoklama; bir bulduğunda, iletiyi okur ve gizli bir kopyasını kuyrukta bırakır. Çalışan rolü sonraki varlıktan bir kopyasını getirir **geçerli** tablo, bir kopya ekler **arşiv** tablosuna sağ tıklayıp ardından özgün siler **geçerli** tablo. Son olarak, önceki adımlarda hata varsa, çalışan rolü gizli iletiyi kuyruktan siler.  

Bu örnekte, 4. adım çalışanı ekler **arşiv** tablo. Çalışan, Blob hizmetindeki blob veya dosya sistemindeki bir dosya ekleyebilirsiniz.  

#### <a name="recovering-from-failures"></a>Hatalardan kurtarma
Önemli olduğu, adımları işlemlerinde **4** ve **5** olmalıdır *ıdempotent* durumda çalışan rolü Arşiv işlemi bilgisayarın yeniden başlatılması gerekiyor. Adımı için tablo hizmetini kullanıyorsanız, **4** bir "Ekle veya Değiştir" işlemi kullanmanız gerekir; adım **5** kullanması gereken bir "silin var" kullanmakta olduğunuz istemci Kitaplığı'nda işlemi. Başka bir depolama sistemi kullanıyorsanız, uygun bir kez etkili işlemi kullanmanız gerekir.  

Çalışan rolü asla adım tamamlanırsa **6**, sonra da bir zaman aşımından sonra bunu yeniden işlemek denemek çalışan rolü için hazır kuyruğa ileti yeniden görünür. Çalışan rolü, kuyruktaki bir iletiyi okumak ve gerekirse, bayrak oluştu kaç kez denetleyebilirsiniz araştırma için bir "zehirli" ileti için ayrı bir kuyruk göndererek olduğu. Kuyruk iletileri okumak ve kuyruktan çıkarma sayımı denetimi hakkında daha fazla bilgi için bkz. [iletileri alma](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Geçici hatalar tablo ve kuyruk hizmetlerine bazı hataları olan ve istemci uygulamanızı bunları işlemesi için uygun bir yeniden deneme mantığı içermelidir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm, işlem yalıtım için sağlamaz. Örneğin, bir istemci okuyabilir **geçerli** ve **arşiv** tablolar adımları çalışan rolü olduğu zaman **4** ve **5**ve bir Veri Görünümü tutarsız. Veri sonunda tutarlı olur.  
* Adım 4 ve 5 nihai tutarlılığı sağlamak için etkili olduğundan emin olması gerekir.  
* Çözüm, birden fazla kuyruk ve çalışan rolü örnekleri kullanarak ölçeklendirebilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı bölümler veya tablo mevcut varlıklar arasındaki son tutarlılık garantisi istediğinizde bu düzeni kullanın. Nihai tutarlılık işlemleri için tablo hizmeti ve Blob hizmetine ve diğer olmayan - Azure depolama üzerinde veritabanı veya dosya sistemi gibi veri kaynakları sağlamak için bu düzeni genişletebilirsiniz.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Birleştirme veya değiştirme](#merge-or-replace)  

> [!NOTE]
> İşlem yalıtım çözümünüze önemliyse tablolarınızı EGTs kullanmanızı sağlamak için yeniden tasarlanmasını düşünmelisiniz.  
> 
> 

### <a name="index-entities-pattern"></a>Dizin varlıklarını düzeni
Varlıkların listesini döndüren verimli aramalar etkinleştirmek için dizin varlıkları korur.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmetini kullanarak varlıkları otomatik olarak dizinleyen **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde noktası sorgusu kullanarak bir varlığı almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısını kullanarak, bir istemci uygulaması verimli bir şekilde ayrı ayrı çalışan varlığın bölüm adını ve çalışan kimliği kullanarak alabilirsiniz ( **PartitionKey** ve **RowKey**).  

![Çalışan varlık][13]

Ayrıca, son kullanıcıların adı gibi başka bir benzersiz olmayan özelliği değere göre çalışan varlıkların listesini almak istiyorsanız doğrudan aramak için bir dizini kullanmak yerine eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmeti, ikincil dizinler sağlamaz olmasıdır.  

#### <a name="solution"></a>Çözüm
Yukarıda gösterilen varlık yapısı ile soyadına göre aramasını etkinleştirmek için çalışan kimlikleri listesi sürdürmeniz gerekir. Jones gibi belirli bir son adı ile çalışan varlıkları almak istiyorsanız önce Jones, son adı olarak olan çalışanlar için çalışan kimlikleri listesini bulmak ve ardından bu çalışan varlıkları alma gerekir. Çalışan kimliklerinin listelerini depolamak için üç ana seçeneğiniz vardır:  

* BLOB Depolama kullanır.  
* Çalışan varlıklar aynı bölümde dizin varlıklar oluşturun.  
* Dizin varlıklar bir ayrı bölüm ya da tablo oluşturun.  

<u>#1. seçenek: Blob storage'ı kullanma</u>  

Birinci seçenek için bir listesini her benzersiz son adı ve her blob deposuna blob oluşturma **PartitionKey** (bölüm) ve **RowKey** son bu ada sahip çalışanların (çalışan kimliği) değerleri. Ekleme ya da bir çalışan silme ilgili blobun içeriğini çalışan varlıklarla sonunda tutarlı olduğundan emin olmanız gerekir.  

<u>#2. seçenek:</u> Dizin varlıklar aynı bölümde oluşturun  

İkinci seçenek için aşağıdaki verilerini depolayan dizin varlıkları kullanın:  

![Çalışan kimliklerini son aynı ada sahip bir listesini içeren dize ile çalışan entity][14]

**EmployeeIDs** özelliği depolanan Soyadı ile çalışanlar için çalışan kimlikleri listesini içeren **RowKey**.  

Aşağıdaki adımlar, ikinci seçenek kullanıyorsanız yeni bir çalışan ekleme yapıyorsanız izlemeniz gereken işlem özetlemektedir. Bu örnekte, bir çalışan kimliği 000152 ve satış departmanında son adı Jones ekliyoruz:  

1. Dizin varlıkla almak bir **PartitionKey** "Sales" değerini ve **RowKey** değeri "Jones." Bu varlık Etag'i 2. adımda kullanmak üzere kaydedin.  
2. Yeni bir çalışan varlık ekleyen bir varlık grubu işlem (diğer bir deyişle, bir toplu işlem) oluşturmak (**PartitionKey** "Sales" değerini ve **RowKey** değeri "000152") ve dizin varlığı güncelleştiren (**PartitionKey** "Sales" değerini ve **RowKey** "Jones" değeri) EmployeeIDs alan listesinde yeni çalışan kimliği ekleyerek. Varlık grubu işlemleri hakkında daha fazla bilgi için bkz: [varlık grubu işlemleri](#entity-group-transactions).  
3. Varlık grubu işlem (başkası dizin varlık değiştirildi) iyimser eşzamanlılık hatası nedeniyle başarısız olursa, adım 1 yeniden baştan başlatmak gerekir.  

İkinci seçenek kullanıyorsanız, bir çalışan silmek için benzer bir yaklaşım kullanabilirsiniz. Bir çalışanın soyadı değiştirme biraz daha karmaşık üç varlığı güncelleştiren bir varlık grubu işlem çalıştırmak gerekeceğinden: çalışan varlık, dizin varlık için eski son adı ve dizin varlık için yeni soyadı. Ardından iyimser eşzamanlılık kullanarak güncelleştirmeleri gerçekleştirmek için kullanabileceğiniz ETag değerleri almak için herhangi bir değişiklik yapmadan önce her varlığın alınması gerekir.  

Aşağıdaki adımlar, bir departmandaki belirli bir soyadı ile tüm çalışanlar ikinci seçenek kullanıyorsanız aramak, ihtiyacınız olduğunda uygulamanız gereken işlem özetlemektedir. Bu örnekte, Yukarı satış departmanında Jones son ada sahip tüm çalışanlar arıyoruz:  

1. Dizin varlıkla almak bir **PartitionKey** "Sales" değerini ve **RowKey** değeri "Jones."  
2. Çalışan EmployeeIDs alanında kimlikleri listesini ayrıştırılamıyor.  
3. Her biri (örneğin, e-posta adreslerini) bu çalışanların hakkında ek bilgi gerekiyorsa, her birini kullanarak çalışan varlıkları almak **PartitionKey** "Sales" değerini ve **RowKey** değerlerini 2. adımda elde ettiğiniz çalışanların listesi.  

<u>#3. seçenek:</u> Dizin varlıkları ayrı bölüm veya tablo oluşturma  

Aşağıdaki veri depolayan dizin varlıklar için üçüncü bir seçenek olarak kullanın:  

![Çalışan kimliklerini son aynı ada sahip bir listesini içeren dize ile çalışan entity][15]

**EmployeeIDs** özelliği depolanan Soyadı ile çalışanlar için çalışan kimlikleri listesini içeren **RowKey**.  

Üçüncü seçenek ayrı bir bölüme çalışan varlıklardaki dizin varlık olduğundan tutarlılık sağlamak için EGTs kullanamazsınız. Dizin varlıkları çalışan varlıklarla sonunda tutarlı olduğundan emin olmanız gerekir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm eşleşen varlıkları almak için en az iki sorgular gerekir: bir listesini almak için dizin varlıklarını sorgulamak için **RowKey** değerleri ve her varlıkta bir listesini almak için sorgular.  
* Koşuluyla, tek bir varlık Boyut 1 MB, #2. seçenek ve seçeneği üst sınırı olan #3'te çözüm verilen Soyadı çalışan kimliklerinin listesi hiçbir zaman 1 MB'den büyük olduğu varsayılmaktadır. Çalışan kimliklerinin listesi boyutu 1 MB'tan büyük olması olasılığı varsa, #1. seçenek kullanın ve dizin verileri blob depolama alanında depolayın.  
* Seçenek #2 kullanıyorsanız işlemlerinin belli bir bölüm ölçeklenebilirlik sınırları yaklaşımını, (bir çalışanın soyadı değiştirme ekleme ve çalışanlar silme işlemek için EGTs kullanarak), değerlendirilmesi gerekir. Bu durumda, ayrı bir bölüme çalışan varlıklardaki dizin varlıklarınızı depolamanızı sağlar ve güncelleştirme isteklerini işlemek için kuyrukları kullanan bir son tutarlılık çözümü (#1. seçenek veya seçenek #3) dikkate almanız gerekir.  
* Bu çözüm #2 seçeneğinde varsayar, bir bölümdeki son adına göre aramak istediğiniz: Örneğin, bir soyadı Jones Satış departmanındaki çalışanlara listesini almak istediğiniz. Tüm kuruluş çapında Jones son ada sahip tüm çalışanlar aramak istiyorsanız, #1. seçenek veya seçenek #3 kullanın.
* Son tutarlılık sunan kuyruk tabanlı bir çözüm uygulayabilirsiniz (bkz [sonunda tutarlı işlemler deseni](#eventually-consistent-transactions-pattern) daha fazla ayrıntı için).  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bir dizi Soyadı Jones sahip bütün çalışanlarla gibi ortak bir özellik değeri herkes varlık arama istediğinizde bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Normalleştirilmişlikten çıkarma deseni
İlgili verileri içeren bir tek nokta sorgu ihtiyacınız olan tüm verileri almanıza olanak sağlamak için tek bir varlık birleştirmek.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında genellikle birden çok tablodan veri alan sorgular bunun sonucunda çoğaltmayı kaldırmak için veri Normalleştir. Verilerinizi Azure tabloları Normalleştir gerekirse, birden çok gidiş dönüş ilgili verilerinizi almak için sunucuya istemciden yapmanız gerekir. Örneğin, tablo yapısı, gösterilen bir departman ayrıntılarını almak için iki gidiş dönüş gerekir: bir yöneticisinin kimliği ve çalışan varlıktaki manager'ın ayrıntılarını getirmek için başka bir istek içeren bölüm varlık getirilemedi.  

![Departman varlığı ile çalışan varlığı][16]

#### <a name="solution"></a>Çözüm
İki ayrı varlık içinde veri depolama, yerine verileri normalleştirilmişlikten çıkarmak ve departman varlıkta manager'ın ayrıntılarını bir kopyasını tutun. Örneğin:  

![Departman varlık normalleştirilmişlikten çıkarılmış ve birleştirilmiş][17]

Bu özelliklerle depolanan departmanı varlıklarla noktası sorgusu kullanarak bir departman hakkında tüm ayrıntıları alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bazı verileri iki kez depolama ile ilişkili ek maliyeti yoktur. (Daha az istekleri depolama hizmetine kaynaklanan) kazanılan performans avantajını genellikle depolama maliyetlerini Marjinal artış kaybettirebilir (ve bu maliyet kısmen bir bölümü ayrıntılarını almak için ihtiyaç duyduğunuz işlem sayısını azalmasına tarafından uzaklığı ).  
* Yöneticileri hakkında bilgi depolamak iki varlık tutarlılığını sürdürmeniz gerekir. Tek bir atomik işlemde birden çok varlık güncelleştirilecek EGTs kullanarak tutarlılık sorunu işleyebilir: Bu durumda, departman varlık ve çalışan varlık için Bölüm Yöneticisi'ni aynı bölümde depolanır.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sık ilgili bilgi aramak gerektiğinde bu düzeni kullanın. Bu düzen gerektiriyorsa verileri almak için istemcinizi yapmalısınız sorguların sayısını azaltır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Bileşik anahtar deseni
Kullanım bileşik **RowKey** tek nokta sorgu ile ilgili verileri aramak bir istemci etkinleştirmek için değerler.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında tek bir sorguda istemciye ilgili veri parçasını döndürülecek sorguları birleştirme kullanılacak doğaldır. Örneğin, çalışan kimliği performans içeren ve verileri gözden geçirmek için bu çalışan ilgili varlıklar listesi aramak için kullanabilirsiniz.  

Çalışan varlıkları şu yapıyı kullanarak tablo hizmetinde depolanması varsayın:  

![Çalışan varlık][18]

Ayrıca incelemeler ve çalışan yapmış her yıl kuruluşunuz için performans ile ilgili geçmiş verileri depolamak gerekir ve bu bilgileri yıla göre erişebilmesi gerekir. Aşağıdaki yapıya sahip varlıklar depolayan başka bir tablo oluşturma tek seçenektir:  

![Çalışan gözden geçirme varlığı][19]

Bu yaklaşımda, bazı bilgiler (örneğin, ad ve Soyadı) oluşturmaya karar verebilir, verilerinizi tek bir istekle almak sağlamak için yeni bir varlık içinde dikkat edin. Ancak, iki varlık atomik olarak güncelleştirmek için bir EGT kullanamadığından güçlü tutarlılık korunamaz.  

#### <a name="solution"></a>Çözüm
Varlıklar ile şu yapıyı kullanarak özgün tablosunda yeni bir varlık türü Store:  

![Bileşik anahtar ile çalışan entity][20]

Bildirim nasıl **RowKey** , artık çalışan kimliği ve yıllık çalışanın performans almak ve verileri tek bir varlığın tek bir istekle gözden sağlayan gözden geçirme veri oluşan bir bileşik anahtarı.  

Aşağıdaki örnek nasıl (örneğin, satış departmanında çalışan 000123) belirli bir çalışan için tüm gözden geçirme verilerini alabilirsiniz özetlenmektedir:  

$filter = (PartitionKey eq 'Satış') ve (RowKey ge 'empid_000123') ve (RowKey lt 'empid_000124') & $select RowKey, Manager değerlendirme, eş derecelendirme, açıklama =  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Ayrıştırılacak kolaylaştıran bir uygun ayırıcı karakter kullanması gereken **RowKey** değer: Örneğin, **000123_2012**.  
* Bu varlık, EGTs güçlü tutarlılık sağlamak için kullanabileceğiniz anlamına gelir aynı çalışan için ilgili verileri içeren diğer varlıklar aynı bölümde de ayrıca depoladığını.
* Bu düzen uygun olup olmadığını belirlemek için verilerin ne sıklıkla sorgulanacağı dikkate almanız gerekir.  Örneğin, gözden geçirme verileri seyrek ve sık sık ana çalışan verilerini erişecekse bunları olarak ayrı varlıklar tutmalısınız.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzen, bir saklamanız gerekir veya ilgili daha fazla varlık sorguladığınız sık kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  
* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Günlük kuyruğu deseni
Alma *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  

> [!NOTE]
> Sorgu sonuçlarını Azure DB'de Azure tablo API'si tarafından döndürülen bölüm anahtarı veya satır anahtarı sıralanmaz. Bu nedenle, bu düzen Azure tablo depolama ve Azure Cosmos DB için uygundur. Özellik farkları ayrıntılı bir listesi için bkz. [Azure Cosmos DB tablo API'si ile Azure tablo depolama arasındaki farklar](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior).

#### <a name="context-and-problem"></a>Bağlam ve sorun
Sık karşılaşılan bir gereksinimdir en son oluşturulan varlıkları almak için örneğin on en son bir çalışan tarafından gönderilen talepleri gider. Tablo sorguları destek bir **$top** sorgulama işlemi ilk döndürülecek *n* bir kümesindeki varlıkların: küme içindeki son n varlıkları döndürülecek eşdeğer sorgu işlemi yok.  

#### <a name="solution"></a>Çözüm
Store kullanarak varlıkları bir **RowKey** doğal olarak bu nedenle en son giriş kullanarak ters tarih/saat sırada sıralar olduğunu her zaman Birinci tablodaki.  

Örneğin, bir çalışan tarafından gönderilen en son on gider talep alabilmesi için geçerli tarih/saat türetilmiş bir ters değer çizgisi değeri kullanabilirsiniz. Aşağıdaki C# kod örneği "ticks ters" için uygun bir değer oluşturmak için bir yol gösteren bir **RowKey** , en son sıralar Yeniye:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Aşağıdaki kodu kullanarak tarih saat değerine geri dönebilirsiniz:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Tablo sorgusu şöyle görünür:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dize değeri beklenen şekilde sıralar emin olmak için sıfır eklenerek ters değer çizgisi değeri paneli gerekir.  
* Bir bölüm düzeyinde ölçeklenebilirlik hedefleri farkında olması gerekir. Dikkatli olun etkin nokta bölümler oluşturma.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Ters tarih/saat düzeni veya en son eklenen varlıkları erişim gerektiğinde varlıklara erişim gerektiğinde bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Önüne ekleyin / koruma desen Ekle](#prepend-append-anti-pattern)  
* [Varlıkları alma](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Yüksek hacimli silme deseni
Silme işlemini varlıkları yüksek hacimli eşzamanlı silinmesi için tüm varlıkları kendi ayrı bir tabloda depolamak tarafından etkinleştirme; varlıklar, tablonun silerek silin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Birçok uygulama, artık bir istemci uygulaması için kullanılabilir olması gerekir veya uygulamayı başka bir depolama ortamına arşivlenmiş eski verileri silin. Genellikle bu tür veriler tarihe göre belirleyin: Örneğin, 60 günden eski olan tüm oturum açma isteklerinin kayıtları silmek için gereksinim.  

Olası bir tasarım olduğu tarihi ve saati oturum açma isteğinde kullanılacak **RowKey**:  

![Oturum açma denemesi varlık][21]

Uygulama ekleme ve oturum açma ayrı bir bölüme her kullanıcı için varlıklar silmek için bu yaklaşım bölüm sıcak önler. Ancak, bu yaklaşım, pahalı ve zaman alıcı ilk silmek tüm varlıkları tanımlamak için bir tablo taraması gerçekleştirmeniz gerekir ve ardından eski her varlık silmeniz gerekir çünkü çok sayıda varlık varsa olabilir. EGTs birden fazla delete isteğinin toplu işleme eski varlıkları silmek için gerekli sunucuya gidiş dönüş sayısını azaltabilir.  

#### <a name="solution"></a>Çözüm
Oturum açma denemesi her günü için ayrı bir tabloda kullanın. Yukarıdaki varlık tasarım sıcak varlıklar eklemek ve eski varlıkları silmek, artık her gün bir tablo silme yalnızca bir soru önlemek için kullanabilirsiniz (tek bir depolama işlemi) yerine bulma ve yüzlerce ve binlerce kişinin silme varlıkları her gün işaretleri.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tasarımınızı belirli varlıklar, diğer veri ya da oluşturma toplama bilgisi ile bağlama bakarak verileri uygulamanızın kullanacağı diğer yolları destekliyor mu?  
* Yeni varlıklar eklerken tasarımınızı etkin noktalardan kaçınacak mu?  
* Aynı tablo adı sildikten sonra yeniden kullanmak isterseniz gecikme bekler. Her zaman benzersiz tablo adları kullanılması daha iyidir.  
* Tablo hizmeti erişim düzenlerini öğrenir ve bölümleri düğümlere dağıtır ilk kez yeni bir tablo kullandığınızda bazı oran sınırlandırma bekler. Sıklıkla yeni tablolar oluşturmak için ihtiyacınız düşünmelisiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Aynı anda silmelisiniz varlıkları yüksek hacimli varsa bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)
* [Varlıkları değiştirme](#modifying-entities)  

### <a name="data-series-pattern"></a>Veri serisi deseni
Tam veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık Store.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Bir dizi genellikle aynı anda almak için gereken verileri depolamak bir uygulama için bir yaygın senaryodur. Örneğin, uygulamanız her çalışan her saat gönderir anlık ileti sayısını kaydedin ve ardından bu bilgileri ileti sayısını çizmek için önceki 24 saat içinde gönderilen her bir kullanıcı kullanın. Her bir çalışanın 24 varlıkları depolamak için bir tasarım olabilir:  

![İleti istatistikleri varlık][22]

Bu tasarım ile kolayca bulun ve uygulamanın ileti sayısı değerini güncelleştirmek gerektiğinde her bir çalışan için güncelleştirilecek varlık güncelleştirin. Ancak, önceki 24 saat için bir etkinlik grafiğini çizmek için bilgi almak için 24 varlıkları almanız gerekir.  

#### <a name="solution"></a>Çözüm
Aşağıdaki tasarım, her saat için ileti sayısı depolamak için ayrı bir özellik kullanın:  

![Ayrılmış özelliklere sahip ileti istatistikleri varlık][23]

Bu tasarımla, bir çalışanın ileti sayısı için belirli bir saat güncelleştirmek için bir birleştirme işlemi kullanabilirsiniz. Artık, tek bir varlık için bir istek kullanarak grafiği çizmek gereken tüm bilgileri alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tam veri seri (varlık en çok 252 özellik olabilir) tek bir varlığa uygun değilse, bir blob gibi bir alternatif bir veri deposunu kullanın.  
* Bir varlık aynı anda güncelleştiren birden çok istemci varsa kullanmanız gerekecektir **ETag** iyimser eşzamanlılık uygulamak için. Birden çok istemci varsa, yüksek çakışma yaşayabilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Güncelleştirme ve tek bir varlık ile ilişkili bir veri serisi almak gerektiğinde bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Büyük varlıklar deseni](#large-entities-pattern)  
* [Birleştirme veya değiştirme](#merge-or-replace)  
* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern) (, veri serisi içinde blob depoladığınız varsa)  

### <a name="wide-entities-pattern"></a>Geniş bir varlıklar deseni
Birden çok 252 özellik ile mantıksal varlıkların depolamak için birden çok fiziksel varlıkları kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık (zorunlu sistem özellikleri hariç) en çok 252 özellik olabilir ve toplam 1 MB'tan fazla veri depolanamıyor. İlişkisel bir veritabanında, genellikle herhangi bir satırın boyutu sınırı round yeni bir tablo ekleyerek ve aralarında 1-1 ilişki zorlamayı almalıdır.  

#### <a name="solution"></a>Çözüm
Tablo hizmeti kullanarak, birden çok 252 özellik tek bir büyük iş nesnesiyle temsil etmek için birden çok varlık depolayabilirsiniz. Örneğin, son 365 gün için her bir çalışan tarafından gönderilen anlık ileti sayısını depolamak istiyorsanız, aşağıdaki iki varlık farklı şemalarla kullanan aşağıdaki tasarım kullanabilirsiniz:  

![İleti istatistikleri varlıkla Rowkey 01 ve ileti durumu varlığı ile Rowkey 02][24]

Bunları birbirleri ile eşitlenmiş tutmak için iki varlıkları güncelleştirme gerektiren bir değişiklik yapmanız gerekirse, bir EGT kullanabilirsiniz. Aksi takdirde, ileti sayısı için belirli bir günde güncelleştirileceği tek birleştirme işlemi kullanabilirsiniz. Tek bir çalışan için tüm verileri almak için her ikisi de kullanan iki verimli istekleri yapabileceğiniz iki varlıkları almak bir **PartitionKey** ve **RowKey** değeri.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Eksiksiz bir mantıksal varlık alma, en az iki depolama işlemleri kapsar: biri her fiziksel varlığını almak için.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzeni aşağıdaki durumlarda kullanın, boyutunu veya sayısını özelliklerinin limitlerini tablo hizmetinin tek bir varlık için varlık depolamaya gerekir.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)
* [Birleştirme veya değiştirme](#merge-or-replace)

### <a name="large-entities-pattern"></a>Büyük varlıklar deseni
BLOB Depolama, büyük özellik değerlerini nasıl depolayacağınızı öğrenin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık toplam 1 MB'tan fazla veri depolanamıyor. Bir veya birkaç özelliklerinizin toplam boyutu varlığınız bu değerini aşmasına neden değerleri depolarsanız, tüm varlık tablo hizmetinde depolanamıyor.  

#### <a name="solution"></a>Çözüm
Bir veya daha fazla özellik büyük miktarda veri içerdiğinden varlık 1 MB boyutunda aşarsa, Blob hizmetinde verileri depolamak ve ardından varlık özelliğinde blob adresini depolar. Örneğin, bir çalışanın fotoğraf blob storage'da depolamak ve fotoğrafın bağlantısını depolamak **fotoğraf** çalışan varlık özelliği:  

![Blob depolama alanına işaret eden fotoğraf dizesiyle çalışan varlık][25]

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo hizmeti, varlık ve verileri Blob hizmetinde arasında nihai tutarlılık sağlamak için kullanın [sonunda tutarlı işlemler deseni](#eventually-consistent-transactions-pattern) varlıklarınızı korumak için.
* Eksiksiz bir varlık alma, en az iki depolama işlemleri kapsar: bir varlık ve blob verilerini almak için alınacak.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Boyutu limitlerini tablo hizmetinin tek bir varlık için varlık depolamaya ihtiyacınız varsa bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Son tutarlılık işlemleri deseni](#eventually-consistent-transactions-pattern)  
* [Geniş bir varlıklar deseni](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Koruma düzeni önüne ekleyin ve ekleme
Birden çok bölümde ekler yayarak ekler yüksek hacimli olduğunda ölçeklenebilirliği artırabilirsiniz.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Bölümler bir dizi ilk veya son bölümü için yeni varlıklar ekleme uygulama eklenmesini ya da saklı varlıklarınızı varlıklar ekleme, genellikle sonuçlanır. Bu durumda, tüm eklemeleri verilen herhangi bir zamanda gerçekleşen birden fazla düğüme ve büyük olasılıkla uygulamanız için ölçeklenebilirlik hedefleri isabet neden aynı bölümde ekler Tablo hizmeti Yük Dengeleme dan engelleyen bir etkin nokta oluşturma bölüm. Günlükleri ağ ve kaynak çalışanlar tarafından erişen bir uygulamanız varsa, örneğin, ardından aşağıda gösterildiği gibi bir varlık yapısı bir etkin nokta işlemlerinin ölçeklenebilirlik hedefi ulaşırsa olma geçerli saat bölüm neden olabilir bir tek tek bölüm:  

![Çalışan varlık][26]

#### <a name="solution"></a>Çözüm
Aşağıdaki alternatif varlık yapısı herhangi bir bölümü etkin nokta uygulama günlükleri olayları ortadan kaldırır:  

![Yıl, ay, gün, saat ve olay kimliği birleşmesi, kullanılması RowKey ile çalışan entity][27]

Bu örnek nasıl hem bildirim **PartitionKey** ve **RowKey** bileşik anahtarlar. **PartitionKey** günlüğe birden çok bölümler arasında dağıtmak için hem Bölüm hem de çalışana kimliğini kullanır.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Etkin bölümler ekler üzerinde verimli bir şekilde oluşturulmasını engeller alternatif anahtar yapısı, istemci uygulamanın yaptığı sorguları destekliyor mu?  
* Beklenen toplu işlem, depolama hizmeti tarafından kısıtlanabilir ve tek bir bölüm için ölçeklenebilirlik hedefleri ulaşmak büyük olasılıkla anlama geliyor?  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Toplu işlem hızını sıcak bölüm eriştiğinizde depolama hizmeti tarafından sınırlama neden olabilir olduğunda prepend ve ekleme koruma düzeni kaçının.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Günlük kuyruğu deseni](#log-tail-pattern)  
* [Varlıkları değiştirme](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Günlük veri koruma deseni
Genellikle, Blob hizmeti yerine tablo hizmeti günlük verilerini depolamak için kullanmanız gerekir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Belirli bir tarih/zaman aralığı için günlük girişlerini seçimi almak için günlük verileri için ortak bir kullanım örneği: Örneğin, tüm hataları ve kritik iletileri 15:04 15:06 belirli bir tarihte arasındaki uygulamanızı günlüğe bulmak istediğiniz. Günlük varlıklara kaydettiğiniz bölüm belirlemek için tarih ve saat günlük ileti kullanmak istiyor musunuz: herhangi bir zamanda tüm günlük varlıkları aynı paylaşacak için sık erişimli bir bölümünde sonuçlarının **PartitionKey** değeri (bkz: bölüm [Prepend ve ekleme koruma düzeni](#prepend-append-anti-pattern)). Örneğin, geçerli tarih ve saat için tüm günlük iletilerini uygulama bölümüne Yazar nedeniyle aşağıdaki varlık şemasında bir günlük iletisi için sık erişimli bir bölümü oluşur:  

![Günlük ileti varlık][28]

Bu örnekte, **RowKey** tarih ve saat günlük iletinin tarih sırasına koyulmuş günlük iletilerini depolandığından emin olun içerir ve aynı tarih ve saat birden çok günlük iletilerini paylaşma durumunda bir ileti kimliği içerir.  

Başka bir yaklaşım kullanmaktır bir **PartitionKey** sağlar bir uygulama bir aralığını bölümler arasında ileti yazar. Örneğin, günlük iletisi kaynağını iletilerini birçok bölümler arasında dağıtmak için bir yol sağlar, aşağıdaki varlık şemasında kullanabilirsiniz:  

![Günlük ileti varlık][29]

Belirli bir süre için tüm günlük iletilerini almak için her bölüm tablosunda arama gerekir, ancak bu şema ile ilgili bir sorun olabilir.

#### <a name="solution"></a>Çözüm
Önceki bölümde günlük girişlerini ve önerilen iki, yetersiz, tasarımları depolamak için tablo hizmetini kullanmayı denemek sorunu vurgulanır. Bir çözümü; risk kötü performans günlük iletileri yazma ile sık erişimli bir bölüme yönlendirdi diğer çözüm tablodaki belirli bir süre için günlük iletilerini almak için her bölümü taramak için gereksinim nedeniyle zayıf sorgu performansı sonuçlandı. BLOB Depolama, bu tür bir senaryo için daha iyi bir çözüm sunar ve bu Azure depolama analizi depoları günlük verileri toplar.  

Bu bölümde, depolama analizi'nın bu yaklaşım genellikle aralığına göre sorgu verilerini depolamak için bir gösterimi olarak günlük verileri blob depolama alanında nasıl depoladı özetlenmektedir.  

Depolama analizi günlük iletilerini birden çok BLOB'ları sınırlandırılmış biçimde depolar. Ayrılmış biçimi için günlük verileri ayrıştırmak bir istemci uygulamasını kolaylaştırır.  

Depolama analizi, aradığınız günlük iletilerini içeren bir adlandırma kuralı, blob bulmanızı sağlar BLOB'ları (veya BLOB'ları) için kullanır. Örneğin, "queue/2014/07/31/1800/000001.log" adlı bir blob, kuyruk hizmeti 31 Temmuz 2014'te 18:00 başlangıç saati ile ilgili günlük iletilerini içerir. "000001", bu ilk günlük dosyası bu döneme ait olduğunu gösterir. Depolama analizi, ayrıca blob meta verilerini bir parçası olarak dosyasında saklanan ilk ve son günlük iletilerini damgalarının kaydeder. API için bir adı ön ekini temel alarak bir kapsayıcıdaki blobları bulmanıza blob depolama alanı sağlar: 18: 00'da bir saat kuyruk günlük verilerini içeren tüm blobları bulmak için "kuyruk/2014/07/31/1800." önekini kullanabilirsiniz  

Depolama analizi arabellekleri iletileri dahili olarak oturum açın ve ardından düzenli aralıklarla uygun blobu Güncelleştir veya en son toplu günlük girişlerinin ile yeni bir tane oluşturur. Bu, blob hizmetine gerçekleştirmelidir yazma sayısını azaltır.  

Kendi uygulamanıza benzer bir çözüm gerçekleştiriyorsanız, (olduğu sürece her günlük girişinin blob depolama alanına yazılmasını) güvenilirlik ve maliyet ve ölçeklenebilirlik (uygulama ve yazma güncelleştirmeleri arabelleğe alma arasındaki dengeyi yönetmek nasıl dikkate almanız gerekir bunları BLOB depolamaya gruplar halinde).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Günlük verilerini depolamak nasıl karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Olası etkin bölümler engelleyen bir tablo tasarımı oluşturursanız, günlük verilerinizi verimli bir şekilde erişemiyor bulabilirsiniz.  
* Günlük verileri işlemek için bir istemci genellikle çok sayıda kayıt yüklemesi gerekir.  
* Günlük verilerini genellikle yapılandırılmış olsa da, blob depolama, daha iyi bir çözüm olabilir.  

### <a name="implementation-considerations"></a>Uygulama konuları
Bu bölümde önceki bölümlerde açıklanan desenleri uygularken göz önünde işlenmesi için dikkat edilmesi gerekenler bazıları açıklanmaktadır. Bu bölümde çoğu C# dilinde yazılmış olan ve depolama istemci Kitaplığı'nı (sürüm 4.3.0 zaman yazma) kullanan örnekler kullanır.  

### <a name="retrieving-entities"></a>Varlıkları alma
Bölümünde açıklandığı gibi [sorgulamak için tasarım](#design-for-querying)en verimli sorgudur noktası sorgusu. Ancak, bazı senaryolarda birden çok varlıkları alma gerekebilir. Bu bölümde, depolama istemci kitaplığı kullanarak varlıkları almak için bazı genel yaklaşımları açıklanmaktadır.  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a>Depolama istemci kitaplığı kullanılarak noktası sorgu yürütme
En kolay yolu noktası sorguyu kullanmaktır **almak** tablo işlemi ile varlık alan aşağıdaki C# kod parçacığında gösterildiği gibi bir **PartitionKey** "Sales" değerinin ve  **RowKey** "212" değerinin:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Bu örnekte varlık nasıl bekliyor fark türünde olmasını alır **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>LINQ kullanarak birden çok varlık alma
Depolama istemci kitaplığı ile LINQ kullanarak ve bir sorgu belirten birden fazla varlık almak bir **burada** yan tümcesi. Bir tablo taraması önlemek için her zaman içermelidir **PartitionKey** değer WHERE yan tümcesi ve eğer mümkünse **RowKey** tablo ve bölüm taramalar önlemek için değer. Tablo hizmeti nerede kullanılacak Karşılaştırma işleçleri (büyüktür, büyük veya buna eşit, az daha az veya eşit, eşit ve eşit değil) sınırlı bir kümesini destekler yan tümcesi. Aşağıdaki C# kod parçacığı tüm çalışanlar, son adı şununla başlar "B" bulur (varsayarak **RowKey** Soyadı depolar) satış departmanında (varsayılarak **PartitionKey** depolar Bölüm adı):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Sorgu her ikisi de nasıl belirtiyor fark bir **RowKey** ve **PartitionKey** daha iyi performans sağlamak için.  

Aşağıdaki kod örneği fluent API'sini kullanarak eşdeğer bir işlevselliği gösterir (genel olarak, fluent API'ler hakkında daha fazla bilgi için bkz [Fluent bir API tasarlamaya ilişkin en iyi yöntemler](https://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Birden çok örnek katmandan **CombineFilters** üç filtre koşulları dahil etmek için yöntemleri.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Bir sorgudan çok sayıda varlık alma
En iyi bir sorgu dayalı tek bir varlık döndüren bir **PartitionKey** değer ve bir **RowKey** değeri. Ancak, bazı senaryolarda aynı bölüme veya daha fazla sayıda varlık döndürmek için bir gereksinim olabilir.  

Böyle senaryolarda, uygulamanızın performansını her zaman tam olarak test etmeniz gerekir.  

Tablo hizmetinde bir sorgu en fazla 1.000 varlıkları tek seferde döndürebilir ve en fazla beş saniye için yürütür. Sorgu beş saniye içinde tamamlanmadı, 1. 000'den fazla varlıklar, sonuç kümesini içerir ya da sorgu bölüm sınırının aşması durumunda, tablo hizmeti, sonraki dizi varlık istemek istemci uygulamasını etkinleştirmek için bir devamlılık belirteci döndürür. Nasıl iş devamlılığı belirteçleri hakkında daha fazla bilgi için bkz. [sorgu zaman aşımı ve sayfalandırma](https://msdn.microsoft.com/library/azure/dd135718.aspx).  

Depolama istemci kitaplığı kullanıyorsanız, tablo hizmetinden varlıklar döndürüyor gibi otomatik olarak devamlılık belirteçleri için işleyebilirsiniz. Depolama istemci kitaplığı kullanılarak otomatik olarak aşağıdaki C# kod örneği, tablo hizmeti bunları bir yanıt döndürürse devamlılık belirteçlerini işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

Aşağıdaki C# kodu açıkça devamlılık belirteçlerini işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Açıkça devamlılık belirteçleri kullanarak, ne zaman uygulamanızı veri sonraki segmentini alır kontrol edebilirsiniz. İstemci uygulamanız kullanıcıların bir tablodaki varlıkların aracılığıyla sayfasında olanak tanır, örneğin, bir kullanıcı, uygulamanızın sonraki almak için yalnızca bir devamlılık belirteci kullanacağınız için sorgu tarafından alınan tüm varlıkları sayfasında değil karar verebilir ne zaman segmentlere ayırın. kullanıcının geçerli kesime alanındaki tüm varlıkları sayfalama bitti. Bu yaklaşım, birçok faydası vardır:  

* Tablo hizmetinden almak için veri miktarını sınırlamak sağlar ve ağ üzerinden taşıması.  
* . NET'te zaman uyumsuz g/ç gerçekleştirmenize olanak sağlar.  
* Bir uygulama çökmesi durumunda devam edebilmesi devamlılık belirteci kalıcı depolama için seri hale getirmek sağlar.  

> [!NOTE]
> Daha az olabilir ancak bir devamlılık belirteci 1.000 varlıkları içeren bir segment genellikle döndürür. Bir sorgunun döndürdüğü kullanarak girdi sayısı sınırlıyorsa da böyledir **ele** arama ölçütlerinizle eşleşen ilk n varlıkları döndürülecek: Tablo hizmeti ile birlikte n varlıklara göre daha az içeren bir segment döndürebilir bir Kalan varlıkları alma sağlamak için devamlılık belirteci.  
> 
> 

Aşağıdaki C# kod bir segment döndürülen varlık sayısını değiştirmek nasıl gösterir:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Sunucu tarafı projeksiyonu
Tek bir varlık, en fazla 255 özelliklere sahip ve en çok 1 MB boyutunda olmalıdır. Bir tabloyu sorgulamak ve varlıkları alma, tüm özellikler gerekli değildir ve gereksiz yere (gecikme süresini ve maliyetini azaltmaya yardımcı olmak için) veri aktarımı önleyebilirsiniz. Sunucu tarafı projeksiyonu yalnızca gereksinim duyduğunuz özellikleri aktarmak için kullanabilirsiniz. Aşağıdaki örnek yalnızca alır **e-posta** özelliği (ile birlikte **PartitionKey**, **RowKey**, **zaman damgası**ve **ETag**) gelen sorgu tarafından seçilen varlıklar.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

Bildirim nasıl **RowKey** değeri, özellikleri almak için listede eklenmemiş olsa bile kullanılabilir.  

### <a name="modifying-entities"></a>Varlıkları değiştirme
Depolama istemcisi kitaplığı, ekleme, silme ve varlıkları güncelleştirme tablo hizmetinde depolanan varlıklarınızı değiştirmenize olanak sağlar. Birden çok ekleme, güncelleştirme ve silme işlemleri birlikte gerekli gidiş dönüş sayısını azaltmak için batch ve çözümünüzün performansını EGTs kullanabilirsiniz.  

Depolama istemcisi kitaplığı bir EGT genellikle yürütüldüğünde oluşturulan özel durumlar, batch başarısız olmasına neden olan varlık dizinini içerir. EGTs kullanan kod hata ayıklaması yapıyorsanız, bu yararlıdır.  

Ayrıca, istemci uygulamanızın eşzamanlılık ve güncelleştirme işlemlerinin nasıl işlediğini tasarımınıza nasıl etkilediğini dikkate almalısınız.  

#### <a name="managing-concurrency"></a>Eşzamanlılığı yönetme
Tablo hizmeti varsayılan olarak, iyimser eşzamanlılık denetler için tek bir varlık düzeyinde uygulayan **Ekle**, **birleştirme**, ve **Sil** işlemleri olsa da, Bu denetimleri atlamak için tablo hizmeti zorlamak bir istemci için mümkündür. Tablo hizmeti eşzamanlılık nasıl yönettiği hakkında daha fazla bilgi için bkz. [Microsoft Azure Depolama'da eşzamanlılığı yönetme](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Birleştirme veya değiştirme
**Değiştirin** yöntemi **TableOperation** sınıfı her zaman tam varlık tablo hizmetindeki değiştirir. Bu özellik depolanan varlık içinde mevcut olduğunda, bir özellik istekte eklemezseniz, istek saklı varlıktan bu özelliği kaldırır. Bir özelliği açıkça depolanmış bir varlıktan kaldırmak istemediğiniz sürece, istekte her bir özellik içermelidir.  

Kullanabileceğiniz **birleştirme** yöntemi **TableOperation** bir varlığı güncelleştirmek istediğiniz zaman tablo hizmetine gönderdiğiniz veri miktarını azaltmak için sınıf. **Birleştirme** yöntemi depolanan varlık içinde herhangi bir özelliği istekte bulunan varlık özellik değerlerini değiştirir, ancak istekte yer almayan depolanan varlık özelliklerinde dokunmaz. Sahip büyük varlıklar ve az sayıda istek özelliklerini güncelleştirmek yalnızca ihtiyacınız varsa, bu yararlıdır.  

> [!NOTE]
> **Değiştirin** ve **birleştirme** yöntemler, varlık mevcut değilse başarısız. Alternatif olarak, kullandığınız **Insertorreplace** ve **InsertOrMerge** yöntemleri, yoksa yeni bir varlık oluşturun.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Heterojen varlık türleri ile çalışma
Tablo hizmeti bir *şemasız* tablo deposu, tek bir tabloyu tasarımınızda büyük esneklik sağlayan birden çok tür varlık depolayabilirsiniz anlamına gelir. Aşağıdaki örnek, hem çalışan hem de bölüm varlıkları depolamak için bir tablo gösterir:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Bölüm adı</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Her varlığın hala olmalıdır **PartitionKey**, **RowKey**, ve **zaman damgası** değerleri, ancak herhangi bir özellikler kümesi olabilir. Ayrıca, hiçbir şey yoktur yere bu bilgileri depolamak seçmediğiniz sürece, bir varlık türünü belirtmek için. Varlık türü tanımlamak için iki seçenek vardır:  

* Varlık türü için önüne ekleyin **RowKey** (veya büyük olasılıkla **PartitionKey**). Örneğin, **EMPLOYEE_000123** veya **DEPARTMENT_SALES** olarak **RowKey** değerleri.  
* Aşağıdaki tabloda gösterildiği gibi kayıt varlık türü için ayrı bir özelliğini kullanın.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>entityType</th>
<th>Bölüm adı</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Bölüm</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

İlk seçenek, varlık eklenmesini türünü **RowKey**, farklı türden iki varlık aynı anahtar değerine sahip olabileceğiniz bir olasılık varsa yararlı olur. Ayrıca, aynı türde birlikte bölümündeki varlıkları gruplandırır.  

Bu bölümde açıklanan olan tekniklerle tartışmaya özellikle ilgilendiren [kalıtım ilişkileri](#inheritance-relationships) bölümünde bu kılavuzun başlarındaki ilişkileri modelleme.  

> [!NOTE]
> İstemci uygulamaların POCO nesneleri gelişmesine ve farklı sürümleri ile çalışmasını sağlamak için varlık türü değeri bir sürüm numarası dahil olmak üzere düşünmelisiniz.  
> 
> 

Bu bölümün geri kalanında bazı depolama istemci Kitaplığı'nda aynı tabloda birden fazla varlık türleri ile çalışmayı kolaylaştırmak özelliklerini açıklar.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Heterojen varlık türleri alınıyor
Depolama istemci kitaplığı kullanıyorsanız, birden çok varlık türleriyle çalışmak için üç seçeneğiniz vardır.  

Belirli bir ile depolanan varlık türünü biliyorsanız **RowKey** ve **PartitionKey** değerleri önceki iki örnekte gösterildiği gibi varlık aldığınızda, varlık türünü belirtebilirsiniz ardından, türündeki varlıkları alma **EmployeeEntity**: [Depolama istemci kitaplığı kullanılarak noktası Sorguyu yürüten](#executing-a-point-query-using-the-storage-client-library) ve [LINQ kullanarak birden çok varlık alma](#retrieving-multiple-entities-using-linq).  

İkinci seçenek kullanmaktır **DynamicTableEntity** türü (bir özellik paketi) yerine (Bu seçenek ayrıca artırabilir performans .NET türlerine varlık seri hale getrime ve gerek olmadığından) somut bir POCO varlık türü. Aşağıdaki C# kodu potansiyel olarak farklı türlerde birden çok varlık tablosundan alır, ancak tüm varlıklar olarak döndürür **DynamicTableEntity** örnekleri. Ardından kullanır **EntityType** özelliği her varlık türünü belirlemek için:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Diğer özelliklerini almak için kullanmanız gerekir **TryGetValue** metodunda **özellikleri** özelliği **DynamicTableEntity** sınıfı.  

Kullanan birleştirileceğini üçüncü seçenek olmasına **DynamicTableEntity** türü ve bir **EntityResolver** örneği. Bu, aynı sorguda birden çok POCO türleri çözümlemeye sağlar. Bu örnekte, **EntityResolver** temsilci kullanarak **EntityType** iki sorgunun döndürdüğü varlık türleri arasında ayrım yapmak için özellik. **Çözmek** yöntemi kullanan **çözümleyici** çözmek için temsilci **DynamicTableEntity** için örnekler **TableEntity** örnekleri.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a>Heterojen varlık türlerini değiştirme
Silmek için bir varlık türünü bilmeniz gerekmez ve onu eklediğinizde bir varlık türü her zaman bildirin. Ancak, kullanabileceğiniz **DynamicTableEntity** türünü bilmek ve bir POCO varlık sınıfı kullanarak olmadan bir varlığı güncelleştirmek için türü. Aşağıdaki kod örneği, tek bir varlığı alır ve denetimleri **EmployeeCount** özelliği var. güncelleştirmeden önce.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile erişimi denetleme
İstemci uygulamaların doğrudan tablo hizmeti ile kimlik doğrulaması yapmak zorunda kalmadan doğrudan tablo varlıklarını değiştirme (ve sorgulamak) sağlamak için paylaşılan erişim imzası (SAS) belirteçleri kullanabilirsiniz. Genellikle, uygulamanızda SAS kullanarak üç ana avantajları vardır:  

* Güvenli bir platforma (örneğin, bir mobil cihazı) depolama hesabı anahtarınızı erişmek ve tablo hizmeti varlıklarda değiştirmek cihazın izin vermek üzere dağıtmak gerekmez.  
* Web ve çalışan rolleri varlıklarınızı son kullanıcı bilgisayarlar gibi istemci cihazları ve mobil cihazları yönetme konusunda gerçekleştiren işinin bir kısmını boşaltabilirsiniz.  
* Kısıtlanmış bir atayabilir ve saat (örneğin, belirli kaynaklara salt okunur erişime izin verme) istemciye izinler kümesini sınırlıdır.  

Tablo hizmeti ile SAS belirteçlerini kullanma hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Ancak, yine de bir istemci uygulama varlıkları tablo hizmetindeki vermek SAS belirteçlerini oluşturmanız gerekir: Bu depolama hesap anahtarlarınızı güvenli erişimi olan bir ortamda yapın. Genellikle, SAS belirteçleri oluşturmak ve varlıklarınızın erişmesi gereken istemci uygulamaları sunmak için bir web veya çalışan rolü kullanın. Olduğundan hala bir ek yük oluşturma ve SAS belirteçlerini istemciler için en iyi şekilde nasıl dikkate almanız gereken yüksek hacimli senaryolarda özellikle bu yükünü azaltmak için teslim katılan.  

Bir tablodaki varlıkların bir alt kümesinde erişim veren bir SAS belirteci oluşturmak mümkündür. Varsayılan olarak tüm bir tablo için SAS belirteci oluşturur, ancak bir SAS belirteci, bir dizi ya da erişim izni belirtmek mümkündür **PartitionKey** değerleri ya da bir dizi **PartitionKey** ve **RowKey** değerleri. Her kullanıcının SAS belirteci yalnızca kendi varlıklara erişimi tablo hizmetinde sağlar, sisteminizin bireysel kullanıcılar için SAS belirteçleri oluşturmak isteyebilirsiniz.  

### <a name="asynchronous-and-parallel-operations"></a>Zaman uyumsuz ve paralel işlemleri
Birden çok bölümde isteklerinizi yayılmasını sağlanan zaman uyumsuz veya paralel sorgular kullanarak aktarım hızı ve istemci yanıt hızını artırabilirsiniz.
Örneğin, paralel tablolara erişen iki veya daha fazla çalışan rolü örnekleri olabilir. Sorumlu belirli bölümleri kümeleri için tek çalışan rolleri veya yalnızca birden çok çalışan rolü örnekleri, her bir tablodaki tüm bölümleri erişim olanağına sahip.  

Bir istemci örneği içinde depolama işlemleri zaman uyumsuz olarak yürüterek üretilen işi artırabilir. Depolama istemcisi kitaplığı, zaman uyumsuz sorgular ve değişiklikleri yazma kolaylaştırır. Örneğin, aşağıdaki C# kodu gösterildiği gibi bir bölümdeki tüm varlıkları getiren bir zaman uyumlu yöntem ile başlayabilirsiniz:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Sorgu zaman uyumsuz olarak aşağıdaki gibi çalışır, böylece bu kodu kolayca değiştirebilirsiniz:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

Bu zaman uyumsuz bir örnekte, zaman uyumlu bir sürümünü aşağıdaki değişiklikleri görebilirsiniz:  

* Artık yöntem imzası içerir **zaman uyumsuz** değiştiricisi ve döndürür bir **görev** örneği.  
* Çağırmak yerine **ExecuteSegmented** yöntemi artık sonuçları almak için gereken yöntemini çağırır **ExecuteSegmentedAsync** yöntemi ve kullanımları **await** değiştiriciyi sonuçlarını zaman uyumsuz olarak alır.  

İstemci uygulaması bu yöntem, birden çok kez çağırabilir (için farklı değerlerle **departmanı** parametresi), ve her sorgu ayrı bir iş parçacığında çalışır.  

Hiçbir zaman uyumsuz sürümü **yürütme** yönteminde **TableQuery** çünkü **IEnumerable** arabirimi, zaman uyumsuz numaralandırma desteklemez.  

Ekleme, güncelleştirme ve zaman uyumsuz olarak varlıkları silin. Aşağıdaki C# örneği eklemek veya çalışan varlığı değiştirmek için basit, zaman uyumlu bir yöntemi gösterir:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Güncelleştirmeyi zaman uyumsuz olarak aşağıdaki gibi çalışır, böylece bu kodu kolayca değiştirebilirsiniz:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Bu zaman uyumsuz bir örnekte, zaman uyumlu bir sürümünü aşağıdaki değişiklikleri görebilirsiniz:  

* Artık yöntem imzası içerir **zaman uyumsuz** değiştiricisi ve döndürür bir **görev** örneği.  
* Çağırmak yerine **yürütme** yöntemi artık varlığı güncelleştirmek için gereken yöntemini çağırır **ExecuteAsync** yöntemi ve kullanımları **await** sonuçları almak için değiştiricisi zaman uyumsuz olarak.  

İstemci uygulama, bunun gibi birden çok zaman uyumsuz yöntemler çağırabilir ve her yöntem çağırma ayrı bir iş parçacığında çalışır.  

### <a name="credits"></a>Jenerik
Aşağıdaki üyeleri Azure ekibi Katkıları için teşekkür ederiz istiyoruz: Dominic Betts Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah ve Serdar Ozler yanı, Tom Hollander DX Microsoft gelen. 

Ayrıca aşağıdaki Microsoft MVPs değerli görüşlerini için teşekkür ederiz gözden geçirme döngüsü sırasında istiyoruz: Igor Papirov ve Edward Bakker.

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

