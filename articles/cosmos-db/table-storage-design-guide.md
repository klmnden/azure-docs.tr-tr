---
title: "Azure depolama tablo Tasarım Kılavuzu | Microsoft Docs"
description: "Tasarım ölçeklenebilir ve kullanıcı tablolarında Azure tablo depolaması"
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: fadb81e16a6c641ca15efb4f910a51de4fe7c997
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Azure depolama tablo Tasarım Kılavuzu: Ölçeklenebilir tasarlama ve kullanıcı tabloları
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Tasarım ölçeklenebilir ve kullanıcı tabloları bir dizi etkene performans, ölçeklenebilirlik ve maliyet gibi dikkate almanız gerekir. Daha önce şemaları ilişkisel veritabanları için tasarlanmış, bu noktalar size tanıdık olacaktır, ancak Azure Table hizmet depolama modeli ve ilişkisel modelleri arasında bazı benzerlikler olsa da, aynı zamanda birçok önemli farklılıklar vardır. Bu farklılıklar, genellikle, counter-intuitive veya ilişkisel veritabanları ile tanıdık birine yanlış görünebilir, ancak Azure tablo hizmeti gibi bir NoSQL anahtar/değer deposu için tanımlıyorsanız, iyi bir fikir hale getirmeyin çok farklı tasarımları için yol açar. Tasarım farklar çoğunu tablo hizmeti varlıkları (ilişkisel veritabanı terminolojisi bulunan satırlar) veri veya çok yüksek işlem hacmi desteklemelidir veri kümeleri için milyarlarca içerebilir bulut ölçekli uygulamalar destekleyecek şekilde tasarlanmıştır olgu yansıtır: Bu nedenle, farklı verilerinizi depolamak nasıl hakkında düşünün ve tablo hizmeti nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu çok daha (ve daha düşük maliyetle) ölçeklendirmek, çözümünüzün etkinleştirebilirsiniz bir çözümden bir ilişkisel veritabanı kullanır. Bu kılavuz, bu konularda yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure tablo hizmeti hakkında
Bu bölümde bazı performans ve ölçeklenebilirlik için tasarlama yakından ilgili olan anahtar özellikleri Tablo hizmetinin vurgular. Azure Storage ve tablo hizmeti yeniyseniz, ilk okuma [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md) ve [.NET kullanarak Azure Table Storage ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) bu makalenin sonraki bölümlerinde okuma önce. Bu kılavuzun odağı tablo hizmetinde olsa da, bazı tartışma Azure kuyruk ve Blob Hizmetleri ve bir çözüm tablo hizmetinde yanı sıra bunları nasıl kullanacağınızı içerecektir.  

Tablo hizmeti nedir? Tablo hizmeti tablosal biçimde adından beklediğiniz gibi verileri depolamak için kullanır. Standart terminoloji, tablodaki her satır bir varlığı temsil eden ve sütunları varlık çeşitli özelliklerini depolamak. Her varlığın benzersiz şekilde tanımlamak için anahtar çiftini vardır ve tablo hizmeti varlık en son değiştirildiği izlemek için kullandığı bir zaman damgası sütunu güncelleştirilen (Bu otomatik olarak gerçekleşir ve zaman damgası rasgele bir değerle el ile üzerine yazılamıyor). Tablo hizmeti iyimser eşzamanlılık yönetmek için bu son değiştirilen zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemleri aynı zamanda sonuç bir **ETag** son değiştirilen zaman damgası (LMT) türetilen değeri. Aynı temel alınan veri başvurduğundan bu belgede ETag ve LMT terimleri birbirlerinin yerine kullanırız.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıkları depolamak için bir basit Tablo Tasarımı gösterir. Bu kılavuzda gösterilen örneklerin çoğu bu basit tasarım temel alır.  

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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
</tr>
<tr>
<td>Tan</td>
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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
</tr>
<tr>
<td>Haz</td>
<td>Cao</td>
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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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


Şu ana kadar bu zorunlu sütunları ve öğeleri aynı tabloda birden çok varlık türleri Depolama olanağı temel farklılıklar ile ilişkisel bir veritabanındaki bir tablo için çok benzer görünür. Ayrıca, her kullanıcı tarafından tanımlanan özellikler gibi **FirstName** veya **yaş** tamsayı veya dize, yalnızca gibi ilişkisel bir veritabanındaki sütun gibi bir veri türüne sahip. Farklı bir ilişkisel veritabanı içinde tablo hizmeti şema daha az yapısını bir özellik aynı veri her varlığın türü olması gerekmez, ancak. Karmaşık veri türleri tek bir özellikte depolamak için JSON veya XML gibi serileştirilmiş bir format kullanmanız gerekir. Tablo hizmeti gibi desteklenen veri türleri, desteklenen bir tarih aralıkları, adlandırma kuralları ve boyutu kısıtlamaları hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Göreceğiniz gibi tercih ettiğiniz **PartitionKey** ve **RowKey** iyi tablo tasarımı için temel önemdedir. Bir tablodaki her varlığın benzersiz bir birleşimi olmalıdır **PartitionKey** ve **RowKey**. Bir ilişkisel veritabanı tablosundaki gibi anahtarlarla **PartitionKey** ve **RowKey** değerleri hızlı göz atmayı sağlayan bir kümelenmiş dizin oluşturmak için dizine; bunlar yalnızca iki dizin oluşturulmuş özellikleri (daha sonra açıklanan desenleri bazıları Göster görünen bu sınırlamaya geçici nasıl çalışır); bu nedenle ancak, tablo hizmeti tüm ikincil dizinlerin oluşturmaz.  

Bir tablo bir veya daha fazla bölümlerini yapılır ve göreceğiniz gibi birçok tasarım kararları uygun bir seçme geçici bir çözüm olacaktır **PartitionKey** ve **RowKey** çözümünüzü en iyi duruma getirme. Tablonun tüm varlıklar bölümlere düzenlenmiş içeren yalnızca tek bir çözüm oluşabilir, ancak bir çözüm, birden çok tablo genellikle sahip olacaktır. Tablolar, mantıksal olarak varlıklarınızı düzenlemek, erişim denetim listeleri kullanarak veri erişimi yönetmenize yardımcı olmak için Yardım ve tek bir depolama işlemi kullanarak tüm bir tabloyu düşürebilir.  

### <a name="table-partitions"></a>Tablo bölümleri
Tablo adı, hesap adı ve **PartitionKey** birlikte tablo hizmeti, varlığı depoladığı depolama hizmeti içinde bölümü tanımlayın. Adres düzeni varlıklar için parçası olmasının yanı sıra, bölüm işlemleri için kapsam tanımlayın (bkz [varlık Grup hareketleri](#entity-group-transactions) aşağıda), ve tablo hizmeti nasıl ölçeklendirir temeli oluştururlar. Bölümleri hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../storage/common/storage-scalability-targets.md).  

Tablo hizmetinde hizmetlerini tek tek bir düğüm tek tek ya da daha fazla tamamlamak bölümler ve hizmet ölçekler dinamik Yük Dengeleme tarafından düğümleri arasında bölümler. Bir düğümü yük altında ise, tablo hizmeti için *bölme* bölümleri aralığını hizmet farklı düğümleri üzerine bu düğüm tarafından; trafiği subsides hizmet yapabilirsiniz *birleştirme* sessiz düğümleri bölüm aralığından tek bir düğüme geri.  

Daha fazla bilgi için iç Ayrıntılar tablo hizmetinin ve özellikle bölümleri hizmet yöneten nasıl incelemesine bakın [Microsoft Azure Storage: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Varlık Grup işlemleri
Tablo hizmetinde varlık Grup hareketleri (EGTs) birden çok varlık arasında atomik güncelleştirmeleri gerçekleştirmek için yalnızca yerleşik sistemdir. EGTs de denir *toplu işlemler* bazı belgelerde. Bu varlıkların aynı bölümde olduğundan emin olmak için gereken birden çok varlık arasında atomik işlem davranışı gereken herhangi bir zamanda EGTs yalnızca aynı bölümünde (paylaşım verilen bir tablodaki aynı bölüm anahtarı) depolanan varlıklar üzerinde bunu çalıştırabilir. Genellikle birden fazla tablo farklı varlık türlerine yönelik kullanmayan ve birden çok varlık türleri aynı tablonun (ve bölüm) tutulması için bir neden de budur. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşleme için birden çok eşzamanlı EGTs gönderirseniz bu EGTs, aksi takdirde işleme Gecikmeli olarak EGTs arasında ortak olan varlıklar üzerinde çalıştırmayın emin olmak önemlidir.

EGTs de tasarımınızda değerlendirebilmeniz için olası dengelemeyi tanıtmak: daha fazla bölüm kullanarak artıracaktır ölçeklenebilirlik, uygulamanızın Azure düğümleri arasında dengelemesini istekleri için daha fazla fırsatlarını sahiptir, ancak bu atomik işlemleri gerçekleştirmek ve verileriniz için güçlü tutarlılık sağlamak için uygulamanızın özelliğini sınırlayabilir. Ayrıca, vardır belirli ölçeklenebilirlik hedefleri için tek bir düğüm beklediğiniz işlemleri verimini sınırlayabilir bir bölümün düzeyinde: Azure depolama hesapları ve tablo hizmeti ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../storage/common/storage-scalability-targets.md). Bu kılavuzun sonraki bölümlerde ele çeşitli yardımcı stratejileri dengelemeler bunun gibi yönetmek ve istemci uygulamanızı belirli gereksinimlerine bağlı olarak, bölüm anahtarı seçmek için en iyi şekilde nasıl ele tasarım.  

### <a name="capacity-considerations"></a>Kapasite dikkat edilecek noktalar
Aşağıdaki tabloda bazı ne zaman, bir tablo hizmeti çözümü tasarlarken bulundurmanız anahtar değerleri içerir:  

| Bir Azure depolama hesabının toplam kapasite | 500 TB |
| --- | --- |
| Bir Azure depolama hesabı tablo sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Bir tabloda bölüm sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Bir bölümdeki varlıkların sayısı |Depolama hesabı kapasitesini yalnızca sınırlıdır |
| Tek bir varlık boyutu |En çok 1 MB ile en fazla 255 özellikleri (de dahil olmak üzere **PartitionKey**, **RowKey**, ve **zaman damgası**) |
| Boyutunu **PartitionKey** |Dizenin en çok 1 KB boyutunda |
| Boyutunu **RowKey** |Dizenin en çok 1 KB boyutunda |
| Bir varlık grubu işlem boyutu |Bir işlem en fazla 100 varlık içerebilir ve yükü boyutu 4 MB'den az olmalıdır. Bir EGT yalnızca bir kez bir varlık güncelleştirebilirsiniz. |

Daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Maliyet dikkat edilecek noktalar
Tablo depolama oldukça masrafsız, ancak kapasite kullanımı ve hareketlerinin miktarı için maliyet tahminleri değerlendirmenize tablo hizmeti kullanan herhangi bir çözümünün bir parçası olarak içermelidir. Ancak, geliştirmek için Normalleştirilmemiş veya yinelenen veri depolama birçok senaryolarda performansı veya ölçeklenebilirliği çözümünüzün yapılacak bir geçerli yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Tablo Tasarımı için yönergeler
Bu listeler tablolarınızı tasarlarken göz önünde bulundurmanız gereken anahtar yönergeleri bazıları özetlemek ve bu kılavuzda bunları daha ayrıntılı daha sonra da değerlendirecektir. Bu yönergeler, ilişkisel veritabanı tasarımı için genellikle izlersiniz yönergeleri çok farklıdır.  

Olması için tablo hizmeti çözümünüzü tasarlama *okuma* verimli:

* ***Okuma ağır uygulamalarda sorgulamaya tasarlayın.*** Tabloları tasarlarken, yürütecek sorguları hakkında (özellikle önemli olanları gecikme) düşündüğünüz varlıklarınızı nasıl güncelleştirecektir hakkında düşünmek önce. Bu genellikle bir verimli ve kullanıcı çözümde sonuçlanır.  
* ***PartitionKey ve RowKey sorgularınızda belirtin.*** *Noktası sorguları* en verimli tablo hizmeti sorguları bunlar gibi.  
* ***Varlıkları yinelenen kopyalarını depolamayı düşünün.*** Tablo depolama ucuz böylece daha verimli sorguları etkinleştirmek için birden çok kez (anahtarları farklı) aynı varlığa depolama göz önünde bulundurun.  
* ***Verilerinizi denormalizing göz önünde bulundurun.*** Tablo depolama ucuz böylece verilerinizi denormalizing göz önünde bulundurun. Örneğin, veri toplama için sorgular yalnızca tek bir varlık erişmesi gereken şekilde Özet varlıklar depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Sahip olduğunuz yalnızca anahtarlar **PartitionKey** ve **RowKey**. Örneğin, bileşik anahtar değerlerinden varlıklar için alternatif anahtarlı erişim yolları etkinleştirmek için kullanın.  
* ***Sorgu projeksiyon kullanın.*** Gereksinim alanları seçin sorgularını kullanarak ağ üzerinden aktarım veri miktarını azaltır.  

Olması için tablo hizmeti çözümünüzü tasarlama *yazma* verimli:  

* ***Sık kullanılan bölümleri oluşturmayın.*** İsteklerinizi birden çok bölüm zaman herhangi bir noktada üzerinden yayılan sağlayan anahtarları'i seçin.  
* ***Ani trafiğinin kaçının.*** Trafiği makul bir süre boyunca kesintisiz ve ani trafiğinin kaçının.
* ***Her varlık türü için ayrı bir tablo mutlaka oluşturmayın.*** Varlık türlerine atomik işlemleri gerektirdiğinde bu birden çok varlık türleri aynı tabloda aynı bölüm depolayabilirsiniz.
* ***En yüksek verimlilik elde gerekir göz önünde bulundurun.*** Tablo hizmeti ölçeklenebilirlik hedefleri farkında olmanız ve tasarımınızı, bunları aşmasına neden olmanız gerekir.  

Bu kılavuzu okumadan gibi tüm bu ilkeler uygulamaya koymanın örnekler görürsünüz.  

## <a name="design-for-querying"></a>Sorgulama için Tasarım
Tablo hizmeti çözümleri yoğun, yazma yoğun veya ikisinin bir karışımı okuyabilirsiniz. Bu bölümde, okuma işlemleri verimli bir şekilde desteklemek için tablo hizmeti tasarlarken de hesaba katılmalıdır gerekenler odaklanır. Genellikle, desteklediği işlemleri verimli bir şekilde okuma tasarım de yazma işlemleri için verimli olur. Ancak, sonraki bölümde açıklanan yazma işlemlerini desteklemek için tasarlarken göz önünde ayı gereken ek noktalar vardır [veri değişikliği için tasarım](#design-for-data-modification).

"Sorguları Uygulamam tablo hizmetinden ihtiyacı olan verileri almak için yürütme gerekecek mi?" sormak için veri etkin şekilde okumanızı sağlamak için tablo hizmeti çözümünüzü tasarlamak için iyi bir başlangıç noktası değil  

> [!NOTE]
> Tablo hizmeti ile zor ve pahalı daha sonra değiştirmeye olduğundan tasarım doğru Önden almak önemlidir. Örneğin, bir ilişkisel veritabanı genellikle dizinler yalnızca mevcut bir veritabanına ekleyerek performans sorunlarına yönelik mümkündür: Bu tablo hizmeti ile bir seçenek değil.  
> 
> 

Bu bölümde sorgulamak için tabloları tasarlarken çözülmesi gereken anahtar durumlara odaklanmaktadır. Bu bölümde yer alan konuları içerir:

* [PartitionKey ve RowKey seçiminizi sorgu performansını nasıl etkilediğini](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için en iyi duruma getirme sorguları](#optimizing-queries-for-the-table-service)
* [Tablo hizmetinde verileri sıralama](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçiminizi sorgu performansını nasıl etkilediğini
Aşağıdaki örneklerde, tablo hizmetinde şu yapıda çalışan varlıkları depolamak varsayılmaktadır (örnekler çoğunu atlayın **zaman damgası** özelliği daha anlaşılır olması için):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **PartitionKey** (bölüm adı) |Dize |
| **RowKey** (çalışan kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **geçerlilik süresi** |Tamsayı |
| **EmailAddress** |Dize |

Önceki bölümde [Azure Table hizmetine genel bakış](#overview) bazı Azure tablo Hizmeti sorgu tasarlama üzerinde doğrudan etkisi olan temel özellikleri açıklanmaktadır. Bunlar, tablo hizmeti sorguları tasarlamak için aşağıdaki genel yönergeleri sonuçlanır. Daha fazla bilgi için tablo hizmetinden REST API'si, aşağıdaki örneklerde kullanılan filtresi sözdizimi olduğuna dikkat edin [sorgu varlıklar](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

* A ***noktası sorgusu*** kullanmak için en verimli arama ve yüksek hacimli aramaları veya en düşük gecikme gerektiren aramalar için kullanılacak önerilir. Böyle bir sorguyu her ikisi de belirterek tek bir varlık çok verimli bir şekilde bulmak için dizinler kullanabilir **PartitionKey** ve **RowKey** değerleri. Örneğin: $filter (PartitionKey eq 'Satış') = ve (RowKey eq '2')  
* İkinci en iyi olan bir ***aralık sorgusu*** kullanan **PartitionKey** ve bir dizi filtreleri **RowKey** birden fazla varlık döndürülecek değer. **PartitionKey** değer belirli bir bölüm tanımlar ve **RowKey** değerleri o bölümün varlıklarda kümesini tanımlayın. Örneğin: $filter PartitionKey eq 'Satış ve RowKey ge'nın ' ve RowKey lt 'T ='  
* Üçüncü iyi bir ***bölüm tarama*** kullanan **PartitionKey** ve başka bir anahtar dışı özelliği ve, filtreleri, birden fazla varlık döndürebilir. **PartitionKey** değer belirli bir bölüm tanımlar ve değerlerini özellik o bölümün varlıklarda alt kümeleri için seçin. Örneğin: $filter PartitionKey eq 'Satış' ve LastName eq 'Smith' =  
* A ***tablo tarama*** içermemesi **PartitionKey** ve tüm tablonuzu sırayla eşleşen tüm varlıkların oluşturan bölümlerin arar çok verimli değildir. Olsun veya olmasın, filtre kullanır, bağımsız olarak bir tablo taraması gerçekleştirecek **RowKey**. Örneğin: $filter LastName eq 'Can' =  
* Birden çok varlık döndüren sorgular bunları içinde sıralanmış dönmek **PartitionKey** ve **RowKey** sırası. Varlıklar istemci yeniden ayırma önlemek için seçin bir **RowKey** en yaygın sıralama düzenini tanımlar.  

Bu kullanarak not bir "**veya**" dayalı bir filtre belirtmek için **RowKey** değerleri bir bölüm tarama sonuçları ve aralığı sorgu olarak işlenmez. Bu nedenle, filtreleri gibi kullanan sorguları kaçınmanız gerekir: $filter PartitionKey eq 'Satış' ve RowKey eq '121' = (veya RowKey eq '322')  

Verimli sorgularını yürütmek için depolama istemci kitaplığı kullanan istemci-tarafı kod örnekleri için bkz:  

* [Depolama istemci kitaplığı kullanılarak noktası sorgusu yürütme](#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlık alma](#retrieving-multiple-entities-using-linq)
* [Server-side projection](#server-side-projection)  

Birden çok varlık türleri aynı tabloda depolanan işleyebilir istemci-tarafı kod örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
Tercih ettiğiniz **PartitionKey** (tutarlılığını sağlamak için) EGTs kullanımını etkinleştirmek için gereken varlıklarınızı (ölçeklenebilir bir çözüm sağlamak için) birden çok bölüm arasında dağıtmak için gereksinim karşı dengelemeniz.  

Bir extreme adresindeki tüm varlıkları tek bir bölüm saklayabilirsiniz, ancak bu çözümünüzü ölçeklenebilirliğini sınırlayabilir ve tablo hizmeti Yük Dengeleme isteklerini engellemek. Diğer uçta bu yüksek oranda ölçeklenebilir olur ve Yük Dengeleme isteklerini tablo hizmetine sağlar, ancak hangi, varlık Grup hareketleri kullanmasını önler bölüm başına tek bir varlık saklayabilirsiniz.  

İdeal **PartitionKey** verimli sorguları kullanmanıza olanak sağlayan ve çözümünüzü ölçeklenebilir olduğundan emin olmak için yeterli bölümleri olan biridir. Genellikle, varlıklarınızı varlıklarınızı yeterli bölümler dağıtır uygun bir özellik olduğunu bulabilirsiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar ilgili bilgileri depoladığı bir sistemde, iyi PartitionKey UserID olabilir. Bölüm anahtarı olarak belirli bir kullanıcı kimliği kullanan birden fazla varlık sahip. Bir kullanıcı hakkında verilerini depolayan her varlığın tek bir bölümde gruplandırılır ve bu nedenle bu varlıkları hala yüksek oranda ölçeklenebilir devam ederken varlık Grup hareketleri erişilebilir.
> 
> 

Tercih ettiğiniz hususlar vardır **PartitionKey** ilişkili nasıl, ekleme, güncelleştirme ve varlıkları silmek için: bölümüne bakın [veri değişikliği için tasarım](#design-for-data-modification) aşağıda.  

### <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için en iyi duruma getirme sorguları
Tablo hizmeti otomatik olarak kullanarak, varlıklarınızı dizinler **PartitionKey** ve **RowKey** tek bir kümelenmiş dizin, bu nedenle sorguları noktası neden değerler kullanmak için en etkili. Ancak, vardır kümelenmiş dizini dışında hiçbir dizinler **PartitionKey** ve **RowKey**.

Çoğu tasarımları birden çok ölçüte dayalı varlıkların aramasını etkinleştirmek için gereksinimleri karşılaması gerekir. Örneğin, e-postalar, temel çalışan varlıkları bulma çalışan kimliği ya da son adı. Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) gereksinim bu tür adres ve tablo hizmetinde ikincil dizinler sağlamaz olgu çözümüne yolları açıklanmaktadır:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerlerini (aynı bölüm) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak diğer sıralamalar **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerleri de, bölümler ayrı veya içinde hızlı ve verimli aramaları ve diğer sıralama etkinleştirmek için tabloları ayırın farklı kullanarak siparişleri **RowKey** değerleri.  
* [Dizin varlıkları düzeni](#index-entities-pattern) -varlıklar listesi Döndür verimli aramalar etkinleştirmek için dizin varlıkları korumak.  

### <a name="sorting-data-in-the-table-service"></a>Tablo hizmetinde verileri sıralama
Tablo hizmeti göre artan düzende sıralandı varlıklar döndürüyor **PartitionKey** ve sonra **RowKey**. Bu anahtarları dize değerlerini ve sayısal değerleri doğru sıralamak emin olmak için sabit bir uzunluğa Dönüştür sıfırlarla doldurur. Örneğin, çalışan kimlik değeri olarak kullanırsanız, **RowKey** bir tamsayı değeri çalışan kimliği dönüştürmeniz gerekir **123** için **00000123**.  

Birçok uygulama farklı siparişler sıralanmış veri kullanımı için gereksinimler vardır: Örneğin, çalışanlar ada göre ya da tarih katılarak sıralama. Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) sıralamalar varlıklarınızı için alternatif nasıl adresi:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) - hızlı etkinleştirmek için (aynı bölümde) farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı RowKey değerleri kullanarak.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı tablolarda ayrı bölümlerdeki farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı RowKey değerleri kullanarak .
* [Günlük tail düzeni](#log-tail-pattern) -almak *n* kullanılarak bir bölüm için en son eklenen varlıklar bir **RowKey** ters tarihi ve saati sipariş sıralar değeri.  

## <a name="design-for-data-modification"></a>Veri değişikliği için Tasarım
Bu bölümde ekler, güncelleştirmeleri, en iyi duruma getirme için tasarım konuları odaklanır ve siler. Bazı durumlarda (Tasarım dengelemeler yönetmek için kullanılan teknikleri ilişkisel bir veritabanında farklı olmasına rağmen) tasarımlarına ilişkisel veritabanları için yaptığınız gibi veri değişikliği için en iyi duruma getirme tasarımlarını karşı sorgulamak için en iyi duruma getirme tasarımları arasındaki dengelemeyi değerlendirmek gerekir. Bölüm [tablo Tasarım desenleri](#table-design-patterns) tablo hizmeti için bazı ayrıntılı tasarım desenleri açıklar ve bazı bu dengelemeler vurgular. Uygulamada, varlıkları iyi değiştirmek için varlıkları sorgulamak için en iyi duruma getirilmiş çoğu tasarımları da iş bulacaksınız.  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a>Performansı en iyi duruma getirme, ekleme, güncelleştirme ve silme işlemleri
Güncelleştirmek veya bir varlığı silmek için kullanarak tanımlayabilir olmalıdır **PartitionKey** ve **RowKey** değerleri. Bu açısından, tercih ettiğiniz **PartitionKey** ve **RowKey** varlıklar değiştirme benzer ölçütleri varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istediğiniz için noktası sorguları desteklemek için tercih ettiğiniz için izlemeniz gereken için. Verimsiz bir bölüm veya tablo taraması bulmak üzere bir varlık bulmak için kullanmak istediğiniz değil **PartitionKey** ve **RowKey** değerleri güncelleştirmek veya silmek için gerekir.  

Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) adres iyileştirme performans veya ekleme, güncelleştirme ve silme işlemleri:  

* [Yüksek hacimli silme düzeni](#high-volume-delete-pattern) -kendi ayrı tabloda eşzamanlı silme işlemi için tüm varlıkları depolayarak varlıklar yüksek hacimli silinmesini sağlar; tablo silerek varlıklarını silme.  
* [Veri serisi deseni](#data-series-pattern) -deposu tam veri serisi içindeki yaptığınız istek sayısını en aza indirmek için tek bir varlık.  
* [Geniş varlıklar düzeni](#wide-entities-pattern) -birden çok 252 özellik sahip mantıksal varlık depolamak için birden çok fiziksel varlık kullanın.  
* [Büyük varlıklar düzeni](#large-entities-pattern) -büyük özellik değerlerini depolamak için blob depolama kullanın.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Saklı varlıklarınızı tutarlılık sağlama
Veri değişiklikleri iyileştirmek için anahtarların tercih ettiğiniz etkileyen diğer anahtar atomik işlemleri kullanarak tutarlılığını sağlamak nasıl faktördür. Yalnızca bir EGT aynı bölümünde depolanan varlıklar üzerinde çalışması için de kullanabilirsiniz.  

Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) tutarlılık yönetme adresi:  

* [İçi bölüm ikincil dizin düzeni](#intra-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerlerini (aynı bölüm) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak diğer sıralamalar **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlere veya ayrı tablolarda farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı kullanarak**RowKey** değerleri.  
* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern) -etkinleştirmek sonuçta tutarlı davranış bölüm sınırları veya depolama sistemi sınırları boyunca Azure sıraları kullanarak.
* [Dizin varlıkları düzeni](#index-entities-pattern) -varlıklar listesi Döndür verimli aramalar etkinleştirmek için dizin varlıkları korumak.  
* [Denormalization deseni](#denormalization-pattern) -birleştirme ilgili verileri birlikte tek nokta sorguyla gereken tüm verileri almak üzere etkinleştirmek için tek bir varlık olarak.  
* [Veri serisi deseni](#data-series-pattern) -deposu tam veri serisi içindeki yaptığınız istek sayısını en aza indirmek için tek bir varlık.  

Varlık Grup işlemler hakkında daha fazla bilgi için bkz [varlık Grup hareketleri](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Tasarımınız etkili değişiklikler için sağlama verimli sorguları kolaylaştırır
Çoğu durumda, bunun belirli senaryonuz için geçerli olup olmadığını tasarım etkili değişiklikler, ancak sorgulama verimli sonuçlar için her zaman değerlendirmelisiniz. Bazı bölümünde desenleri [tablo Tasarım desenleri](#table-design-patterns) açıkça sorgulama ve varlıkları değiştirme arasındaki dengelemeler değerlendirin ve her zaman her türden işlem sayısını dikkate almanız.  

Bölümünde aşağıdaki desenleri [tablo Tasarım desenleri](#table-design-patterns) adres için verimli sorguları tasarlama ve verimli veri değişikliği için tasarlama arasındaki dengelemeler:  

* [Bileşik anahtar düzeni](#compound-key-pattern) -kullanım bileşik **RowKey** tek nokta sorgu ile ilgili veri aramak bir istemci etkinleştirmek için değerleri.  
* [Günlük tail düzeni](#log-tail-pattern) -almak *n* kullanılarak bir bölüm için en son eklenen varlıklar bir **RowKey** ters tarihi ve saati sipariş sıralar değeri.  

## <a name="encrypting-table-data"></a>Tablo verileri şifreleme
.NET Azure Storage istemci kitaplığı dizesi varlık özellikleri şifrelenmesi için INSERT destekler ve değiştirme işlemlerini. Şifrelenmiş dizelerin hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra dizelere geri dönüştürülür.    

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenecek özelliklerini belirtmeniz gerekir. Bu, ya da bir [EncryptProperty] özniteliği (için TableEntity türetilen POCO varlıklar) veya bir şifreleme çözümleyici isteği seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarı, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilci ' dir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikler nasıl şifrelenir geçici mantığı olasılığı için de sağlar. (Örneğin, X ise ardından özelliği A şifrelemek; Aksi takdirde özellikleri A ve b şifreleme) Bu bilgileri okurken veya varlıkları sorgulamak için gerekli olmadığını göz önünde bulundurun.

Bu birleştirme şu anda desteklenmeyen unutmayın. Bir alt özellikler kümesini daha önce farklı bir anahtar kullanılarak şifrelenmiş olabilecek olduğundan, sadece yeni özellikleri birleştirme ve meta verilerini güncelleştirme veri kaybına neden olur. Ya da birleştirme önceden var olan varlık hizmetinden okumak için fazladan hizmeti çağrıları yapma gerektirir veya yeni bir anahtar özellik başına kullanarak, her ikisi de performansı artırmak için uygun değildir.     

Tablo verisi şifreleme hakkında daha fazla bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../storage/common/storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>İlişkileri modelleme
Etki alanı model oluşturmaya, karmaşık sistemler tasarımında önemli bir adımdır. Genellikle, varlıkları ve ilişkileri aralarında iş etki alanını anlamak ve sisteminizin tasarımını bildirmek için bir yol olarak tanımlamak için modelling işlemini kullanın. Bu bölüm, nasıl sizin tasarımlar tablo hizmeti için etki alanı modellerine bulunan ortak ilişki türlerinden bazıları çevirebilir üzerinde odaklanmıştır. İlişkisel veritabanı tasarlarken kullanılan oldukça farklı bir fiziksel dayalı NoSQL veri modeli için bir mantıksal veri modelinden eşlemesinin işlemidir. İlişkisel veritabanları tasarımı, genellikle artıklık – ve nasıl uyarlamasını veritabanı işleyişi soyutlar bildirim temelli bir sorgulama yeteneği en aza indirmek için en iyi duruma getirilmiş veri normalleştirme işlemi varsayar.  

### <a name="one-to-many-relationships"></a>Bir-çok ilişkileri
İş etki alanı nesneler arasındaki bir-çok ilişkileri ortaya çok sık: Örneğin, bir bölümü çok sayıda çalışan vardır. Tablo hizmetinde her Artıları ve eksileri belirli senaryoya ilgili olabilecek ile bir-çok ilişkileri uygulamak için birkaç yolu vardır.  

Büyük çok Ulusal corporation örneği on binlerce Departmanlar ve her departman çok sayıda çalışan ve her çalışan belirli bir bölüm ile ilişkili olarak sahip olduğu çalışan varlıkları ile göz önünde bulundurun. Ayrı departmanı ve bunlar gibi çalışan varlıkları depolamak için bir yaklaşım ise:  

![][1]

Bu örnek, temel türleri arasında örtük bir-çok ilişkisi gösterir **PartitionKey** değeri. Her bölüm çok sayıda çalışan olabilir.  

Bu örnek ayrıca aynı bölümde departmanı varlık ve ilgili çalışan varlıklarını gösterir. Farklı bölümleri, tablolar veya hatta depolama hesapları farklı varlık türleri için kullanılacak tercih edebilirsiniz.  

Verilerinizi denormalize ve yalnızca çalışan varlıklarıyla aşağıdaki örnekte gösterildiği gibi Normalleştirilmemiş Departman verileri depolamak için alternatif bir yaklaşım var. Bunu yapmak için her çalışan departmanındaki güncelleştirmeniz gerekir çünkü bir bölüm Yöneticisi'ni ayrıntılarını değiştirebilmesi gereksinimi varsa bu belirli bir senaryoda, en iyi Normalleştirilmemiş bu yaklaşım olmayabilir.  

![][2]

Daha fazla bilgi için bkz: [Denormalization düzeni](#denormalization-pattern) bu kılavuzda daha sonra.  

Aşağıdaki tabloda Artıları ve eksileri her çalışan ve bir-çok ilişkisi departmanı varlıkları depolamak için yukarıda özetlenen yaklaşımlardan biri özetlenmektedir. Çeşitli işlemleri ne sıklıkta yapılacağı düşünmelisiniz: pahalı bir işlem bu işlemi yalnızca seyrek durumda içeren bir tasarıma sahip kabul edilebilir olabilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Uzmanları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı bir varlık türleri, aynı bölüm, aynı tablosu</td>
<td>
<ul>
<li>Tek bir işlemle bir departman varlık güncelleştirebilirsiniz.</li>
<li>Bir EGT departman varlığı değiştirmek için bir gereksinim duyarsanız tutarlılığın korunmasını için kullanabileceğiniz olduğunda, güncelleştirme/ekleme/silme çalışan varlık. Örneğin, bir departman çalışan sayısı her bölüm için sahipseniz.</li>
</ul>
</td>
<td>
<ul>
<li>Bir çalışan ve bazı istemci etkinlikler için bir bölüm varlık almak gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimlerinin, bu bir etkin nokta neden olabilir.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüm taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı bir varlık türleri, farklı bölümleri veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Tek bir işlemle bir departman varlık veya çalışan varlık güncelleştirebilirsiniz.</li>
<li>Yüksek işlem birimlerinin, bu yük daha fazla bölüm yayılan yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Bir çalışan ve bazı istemci etkinlikler için bir bölüm varlık almak gerekebilir.</li>
<li>Tutarlılık sağlamak için EGTs kullanamazsınız olduğunda, güncelleştirme/ekleme/silme bir çalışan ve güncelleştirme bir bölüm. Örneğin, bir çalışan sayısı departmanı varlıktaki güncelleştiriliyor.</li>
<li>Bir çalışan bir EGT kullanarak yeni bir bölüm taşınamıyor.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne denormalize</td>
<td>
<ul>
<li>Tek bir istekle gereken tüm bilgileri alabilir.</li>
</ul>
</td>
<td>
<ul>
<li>(Bu, bir departmandaki tüm çalışanlar güncelleştirmek gerekir) departmanı bilgileri güncelleştirmek gereksinim duyarsanız tutarlılığın korunmasını pahalı olabilir.</li>
</ul>
</td>
</tr>
</table>

Nasıl Bu seçenekler ve Artıları ve eksileri hangisinin en önemli, belirli bir uygulama senaryolarınızı bağlıdır arasında seçin. Örneğin, ne sıklıkta, departman varlıklar değiştirmeyin; Tüm çalışan sorguları ek departman bilgi gerekiyor; ölçeklenebilirlik sınırları, bölüm veya depolama hesabınız için ne kadar yakın misiniz?  

### <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri varlıklar arasındaki birebir ilişkileri içerebilir. Tablo hizmetinde bire bir ilişki uygulamanız gerekiyorsa, her ikisinin de almanız gerektiğinde, iki ilgili varlık bağlanma seçmeniz gerekir. Bu bağlantıyı biçiminde bir bağlantı depolayarak örtük, bir kural anahtar değerlerine göre veya açık olabilir **PartitionKey** ve **RowKey** ilgili varlık için her bir varlıkta değerleri. Bölüm olup aynı bölümde ilgili varlıkları saklamalısınız bir tartışma için bkz [-çok ilişkileri](#one-to-many-relationships).  

Olduğunu da tablo hizmetinde bire bir ilişkiler uygulamak için yol açabilecek uygulama konuları göz önünde bulundurun:  

* Büyük varlıklar işleme (daha fazla bilgi için bkz: [büyük varlıklar düzeni](#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için bkz: [paylaşılan erişim imzaları ile erişimi denetleme](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>İstemci katılma
Tablo hizmetinde ilişkileri modellemek için yollar olsa da, tablo hizmeti kullanmak için iki prime nedenler ölçeklenebilirlik ve performans olduğunu unuttunuz değil. Performans ve ölçeklenebilirlik çözümünüzün tehlikeye birçok ilişki modelleme bulursanız, tablo tasarımınıza tüm veri ilişkileri oluşturmak gerekli olup olmadığını kendiniz istemeniz gerekir. Tasarım basitleştirmek ve istemci uygulamanızı gerekli tüm birleştirmeler gerçekleştirme izin verirseniz çözümünüzün performansını ve ölçeklenebilirliğini artırmak mümkün olabilir.  

Örneğin, çok sık değişmeyen veriler içeren küçük tablolar sahip yaparsanız, ardından bu verileri bir kez ve istemcide önbelleğe. Bu, aynı veri almak için yinelenen gidiş-dönüş önleyebilirsiniz. Bu kılavuzda inceledik örneklerde, küçük ve seyrek veri ara olarak istemci uygulamasını bir kez indirebilirsiniz verileri ve önbellek için iyi bir aday yapmadan değiştirmek küçük bir kuruluşta Departmanlar kümesi olasıdır.  

### <a name="inheritance-relationships"></a>Devralma ilişkisi
İstemci uygulamanızın iş varlıkları temsil etmek için bir devralma ilişkisi parçasını sınıflar kümesini kullanıyorsa, bu varlıkların tablo hizmetinde kolayca devam edebilir. Örneğin, aşağıdaki istemci uygulamanızda tanımlanan sınıflar kümesini olabilir nerede **kişi** bir Özet sınıf.

![][3]

İki somut sınıfların görünüm bu gibi varlıkları kullanarak tek bir kişi tablosunu kullanarak tablo hizmetinde örneklerini devam edebilir:  

![][4]

İstemci kodu aynı tablodaki birden çok varlık türleri ile çalışma hakkında daha fazla bilgi için bkz [heterojen varlık türleriyle çalışma](#working-with-heterogeneous-entity-types) bu kılavuzda daha sonra. Bu, istemci kodu varlık türünde tanımak nasıl örnekleri sağlar.  

## <a name="table-design-patterns"></a>Tablo Tasarım desenleri
Önceki bölümlerde bazı tartışmalar tablo tasarımınızı ekleme, güncelleştirme ve varlık verileri siliniyor ve sorguları kullanarak her iki varlık verileri için en iyi duruma getirme hakkında ayrıntılı gördünüz. Bu bölümde tablo hizmeti çözümleri ile kullanım için uygun bazı desenleri açıklar. Ayrıca, nasıl, pratikte bazı sorunlar ve bu kılavuzda daha önce oluşturulan dengelemeler karşılayabilirsiniz görürsünüz. Aşağıdaki diyagramda farklı desenleri arasındaki ilişkileri özetlenmektedir:  

![][5]

Yukarıdaki desenini eşleme desenleri (mavi) ve bu kılavuzda belgelenen koruma desenleri (turuncu) arasında bazı ilişkiler vurgular. Elbette dikkate değer olan diğer birçok desenleri de vardır. Örneğin, tablo hizmeti için ana senaryoları kullanmak için biri [gerçekleştirilip görünüm düzeni](https://msdn.microsoft.com/library/azure/dn589782.aspx) gelen [komutu sorgu sorumluluk ayrımı (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) düzeni.  

### <a name="intra-partition-secondary-index-pattern"></a>Bölüm içi ikincil dizin düzeni
Her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerleri (aynı bölüm) etkinleştir hızlı ve verimli aramalarını kullanarak diğer sıralamalar farklı **RowKey** değerleri. Güncelleştirmeleri kopyaları arasında saklanması tutarlı EGT'ın kullanma.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti otomatik olarak kullanarak varlıkları dizinler **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde bu değerleri kullanarak bir varlık almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısı kullanarak bir istemci uygulaması noktası sorgu bölüm adını ve çalışan kimliği kullanarak bir tek çalışan varlık almak için kullanabilirsiniz ( **PartitionKey** ve **RowKey** değerleri). Bir istemci, her bölüm içinde çalışan kimliğine göre sıralanmış varlıklar de alabilirsiniz.

![][6]

Ayrıca e-posta adresi gibi başka bir özelliğin değerini dayalı bir çalışan varlık bulamaz istiyorsanız bir eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmetinde ikincil dizinler sağlamadığından budur. Ayrıca, çalışanların farklı bir düzende sıralanmış bir listesini istemek için bir seçenek yoktur **RowKey** sırası.  

#### <a name="solution"></a>Çözüm
İkincil dizinler eksikliği olarak çözmek için farklı bir kullanarak her kopyayla her varlığın birden çok kopya depolayabilir **RowKey** değeri. Aşağıda gösterilen yapıları içeren bir varlık depolarsanız, e-posta adresi veya çalışan kimliği temel alarak çalışan varlıkları verimli bir şekilde alabilir. Önek değerleri **RowKey**, "empid_" ve "email_" tek bir çalışan veya bir dizi çalışanlar için bir aralığı e-posta adresleri ya da çalışan kimlikleri kullanarak sorgulama olanak tanır.  

![][7]

(Bir çalışan kimliği ve bir e-posta adresine göre arayarak bakan) aşağıdaki iki filtre ölçütü her ikisi de noktası sorguları belirtin:  

* $filter (PartitionKey eq 'Satış') = ve (RowKey eq 'empid_000223')  
* $filter (PartitionKey eq 'Satış') = ve (RowKey eq 'email_jonesj@contoso.com')  

Çalışan varlıklar için bir aralığı sorgu, çalışan kimliği düzende sıralanmış bir aralık veya uygun önekle varlıklar için sorgulayarak e-posta adresi düzende sıralanmış bir aralığı belirtebilirsiniz **RowKey**.  

* Satış departmanında çalışan kimliği aralığı 000100 için 000199 kullanımda olan tüm çalışanlar bulmak için: $filter (PartitionKey eq 'Satış') = ve (RowKey ge 'empid_000100') ve (RowKey le 'empid_000199')  
* Tüm çalışanlar satış departmanında 'bir' kullanımı harfle başlayan bir e-posta adresiyle bulmak için: $filter (PartitionKey eq 'Satış') = ve (RowKey ge 'email_a') ve (RowKey lt 'email_b')  
  
  Daha fazla bilgi için tablo hizmetinden REST API, Yukarıdaki örneklerde kullanılan filtresi sözdizimi olduğuna dikkat edin [sorgu varlıklar](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolama yinelenen veri depolamanın maliyeti yükünü başlıca sorunlardan olmamalıdır şekilde kullanmak görece ucuz ' dir. Ancak, her zaman öngörülen depolama gereksinimlerinize göre tasarımınızı maliyetini değerlendirmek ve yalnızca istemci uygulamanızı yürütecek sorguları desteklemek için yinelenen varlıkları ekleyin.  
* İkincil dizin varlıkları özgün varlıkları aynı bölümünde depolandığından tek tek bir bölüm için ölçeklenebilirlik hedefleri aşmamasını sağlayın.  
* Varlık iki kopyasını otomatik olarak güncelleştirmek için EGTs kullanarak, yinelenen varlıklarınızı birbiriyle tutarlı tutabilirsiniz. Bu, bir varlığın tüm kopyaları aynı bölümde saklamalısınız anlamına gelir. Daha fazla bilgi için bkz [kullanarak varlık Grup hareketleri](#entity-group-transactions).  
* İçin kullanılan değer **RowKey** her bir varlık için benzersiz olmalıdır. Bileşik anahtar değerlerinden kullanmayı düşünün.  
* Sayısal değerler doldurma **RowKey** (örneğin, çalışan kimliği 000223), sıralama ve filtreleme göre üst ve alt sınırlarını etkinleştirir düzeltin.  
* Mutlaka, varlığın tüm özellikleri yinelenen gerekmez. Örneğin, sorguları, e-posta kullanarak varlıkları aramanın adres **RowKey** hiçbir zaman, çalışanın yaşı gerekir, bu varlıkları aşağıdaki yapısına sahip olabilir:

![][8]

* Yinelenen verileri depolamak ve tek bir sorgu ile gereken tüm verileri alabilir sağlamak genellikle daha iyi bir varlık ve gerekli verileri aramak için başka bulmak için bir sorgu kullanımı çok.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İstemci uygulamanızın istemci farklı sıralamalar varlıklarda almak gerektiğinde farklı anahtarlar, çeşitli kullanarak varlık almaya gerektiğinde ve benzersiz değerler çeşitli kullanarak her bir varlık tanımlayabilirsiniz bu deseni kullanır. Ancak, farklı kullanarak varlık arama yaparken bölüm ölçeklenebilirlik sınırları aşmadığından emin olmalıdır **RowKey** değerleri.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [İkincil dizin arası bölüm düzeni](#inter-partition-secondary-index-pattern)
* [Bileşik anahtar düzeni](#compound-key-pattern)
* [Varlık Grup işlemleri](#entity-group-transactions)
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>İkincil dizin arası bölüm düzeni
Her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerleri de, bölümler ayrı veya tablolara etkinleştir hızlı ve verimli aramaları ve kullanarak diğer sıralamalar farklı'de ayrı **RowKey** değerleri.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti otomatik olarak kullanarak varlıkları dizinler **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde bu değerleri kullanarak bir varlık almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısı kullanarak bir istemci uygulaması noktası sorgu bölüm adını ve çalışan kimliği kullanarak bir tek çalışan varlık almak için kullanabilirsiniz ( **PartitionKey** ve **RowKey** değerleri). Bir istemci, her bölüm içinde çalışan kimliğine göre sıralanmış varlıklar de alabilirsiniz.  

![][9]

Ayrıca e-posta adresi gibi başka bir özelliğin değerini dayalı bir çalışan varlık bulamaz istiyorsanız bir eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmetinde ikincil dizinler sağlamadığından budur. Ayrıca, çalışanların farklı bir düzende sıralanmış bir listesini istemek için bir seçenek yoktur **RowKey** sırası.  

Bu varlıklar karşı işlemleri çok yüksek hacimli bekleme ve tablo hizmeti istemci azaltma riskini en aza indirmek istediğiniz.  

#### <a name="solution"></a>Çözüm
İkincil dizinler eksikliği olarak çözmek için birden çok kopyası her kopyalama kullanarak her bir varlık farklı depolayabilir **PartitionKey** ve **RowKey** değerleri. Aşağıda gösterilen yapıları içeren bir varlık depolarsanız, e-posta adresi veya çalışan kimliği temel alarak çalışan varlıkları verimli bir şekilde alabilir. Önek değerleri **PartitionKey**, "empid_" ve "email_" bir sorgu için kullanmak istediğiniz hangi dizin tanımlamak etkinleştirin.  

![][10]

(Bir çalışan kimliği ve bir e-posta adresine göre arayarak bakan) aşağıdaki iki filtre ölçütü her ikisi de noktası sorguları belirtin:  

* $filter (PartitionKey eq ' empid_Sales') = ve (RowKey eq '000223')
* $filter (PartitionKey eq ' email_Sales') = ve (RowKey eq 'jonesj@contoso.com')  

Çalışan varlıklar için bir aralığı sorgu, çalışan kimliği düzende sıralanmış bir aralık veya uygun önekle varlıklar için sorgulayarak e-posta adresi düzende sıralanmış bir aralığı belirtebilirsiniz **RowKey**.  

* Çalışan kimliği aralığında Satış departmanındaki tüm çalışanlar bulmak için **000100** için **000199** çalışan kimliği sipariş kullanımda sıralanır: $filter (PartitionKey eq ' empid_Sales') = ve (RowKey ge '000100') ve (RowKey le '000199')  
* E-posta adresi sipariş kullanımda sıralanmış 'a' ile başlayan e-posta adresi olan Satış departmanındaki tüm çalışanlar bulmak için: $filter (PartitionKey eq ' email_Sales') = ve (RowKey ge 'bir') ve (RowKey lt 'b')  

Daha fazla bilgi için tablo hizmetinden REST API, Yukarıdaki örneklerde kullanılan filtresi sözdizimi olduğuna dikkat edin [sorgu varlıklar](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Kullanarak, yinelenen varlıklarınızı birbiriyle sonunda tutarlı tutabilirsiniz [sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern) birincil ve ikincil dizin varlıkları korumak için.  
* Tablo depolama yinelenen veri depolamanın maliyeti yükünü başlıca sorunlardan olmamalıdır şekilde kullanmak görece ucuz ' dir. Ancak, her zaman öngörülen depolama gereksinimlerinize göre tasarımınızı maliyetini değerlendirmek ve yalnızca istemci uygulamanızı yürütecek sorguları desteklemek için yinelenen varlıkları ekleyin.  
* İçin kullanılan değer **RowKey** her bir varlık için benzersiz olmalıdır. Bileşik anahtar değerlerinden kullanmayı düşünün.  
* Sayısal değerler doldurma **RowKey** (örneğin, çalışan kimliği 000223), sıralama ve filtreleme göre üst ve alt sınırlarını etkinleştirir düzeltin.  
* Mutlaka, varlığın tüm özellikleri yinelenen gerekmez. Örneğin, sorguları, e-posta kullanarak varlıkları aramanın adres **RowKey** hiçbir zaman, çalışanın yaşı gerekir, bu varlıkları aşağıdaki yapısına sahip olabilir:
  
  ![][11]
* Yinelenen verileri depolamak ve ikincil dizin ve arama gerekli verileri için başka birincil dizinde kullanarak bir varlık bulmak için bir sorgu kullanımı çok tek bir sorgu ile gereken tüm verileri alabilir sağlamak genellikle daha iyi olur.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İstemci uygulamanızın istemci farklı sıralamalar varlıklarda almak gerektiğinde farklı anahtarlar, çeşitli kullanarak varlık almaya gerektiğinde ve benzersiz değerler çeşitli kullanarak her bir varlık tanımlayabilirsiniz bu deseni kullanır. Varlık aramalarını farklı kullanarak gerçekleştirirken bölüm ölçeklenebilirlik sınırları aşmamak istediğinizde bu deseni kullanın **RowKey** değerleri.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern)  
* [Bölüm içi ikincil dizin düzeni](#intra-partition-secondary-index-pattern)  
* [Bileşik anahtar düzeni](#compound-key-pattern)  
* [Varlık Grup işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Sonuçta tutarlı işlemleri düzeni
Sonuçta tutarlı davranışı, Azure sıraları kullanarak bölüm sınırları veya depolama sistemi sınırları boyunca etkinleştirin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
EGTs atomik işlemleri aynı bölüm anahtarına paylaşan birden çok varlık arasında etkinleştirin. Performans ve ölçeklenebilirlik için ayrı bölümlere veya ayrı bir depolama sistemi tutarlılık gereksinimlerin varlıkları depolamak karar verebilirsiniz: Böyle bir senaryoda EGTs tutarlılığını korumak için kullanamazsınız. Örneğin, arasında nihai tutarlılık sağlamak için bir zorunluluk olabilir:  

* Varlık aynı tablodaki farklı tablolar, iki farklı bölümlere farklı depolama hesaplarında depolanır.  
* Tablo hizmetinde depolanan bir varlık ve Blob hizmetinde depolanan bir blob.  
* Tablo hizmeti ve bir dosya bir dosya sisteminde depolanan bir varlık.  
* Tablo hizmeti varlık deposunda henüz Azure Search hizmeti kullanarak dizin.  

#### <a name="solution"></a>Çözüm
Azure sıralar kullanarak, iki veya daha fazla bölüm ya da depolama sistemleri arasında nihai tutarlılık teslim bir çözüm uygulayabilirsiniz.
Bu yaklaşım göstermek için eski çalışan varlıkları arşivlemek için gereksinim varsayalım. Eski çalışan varlıkları nadiren seçmeleri istenir ve geçerli çalışanlar ile ilgili tüm etkinlikleri dışında tutulması. Bu gereksinim uygulamak için etkin çalışanlara depolamak **geçerli** tablo ve eski çalışanların **arşiv** tablo. Bir çalışan arşivleme gerektirir varlıktan silmek **geçerli** tablo ve varlığa ekleyin **arşiv** tablosu, ancak bir EGT iki bu işlemleri gerçekleştirmek için kullanamazsınız. Bir hata görünmesi bir varlık hem veya hiçbiri tablolarda neden riskini önlemek için arşiv işlemi sonunda tutarlı olması gerekir. Aşağıdaki sıralı diyagramı adımlar bu işlemde açıklanır. Daha fazla ayrıntı metin aşağıdaki özel durum yollarında sağlanır.  

![][12]

Bir istemci üzerinde çalışan #456 arşivlemek için bu örnek bir Azure sırada bir ileti koyarak Arşiv işlemi başlatır. Çalışan rolü yeni iletiler için sıra yoklar; bir bulduğunda, iletiyi okur ve gizli bir kopyasını sıra üzerinde bırakır. Çalışan rolü sonraki varlıktan kopyasını getirir **geçerli** tablo, bir kopya ekler **arşiv** tablo ve özgün siler **geçerli** tablo. Son olarak, önceki adımlarda hata olsaydı, çalışan rolü gizli ileti kuyruktan siler.  

Bu örnekte, 4. adım çalışanı ekler **arşiv** tablo. Blob hizmetinde bir blob veya bir dosya sistemindeki bir dosyaya çalışan ekleyebilirsiniz.  

#### <a name="recovering-from-failures"></a>Hatalardan kurtarma
Önemlidir, adımları işlemlerinde **4** ve **5** olmalıdır *ıdempotent* durumda çalışan rolü Arşiv işleminin yeniden başlatılması gerekiyor. Adım için tablo hizmeti kullanıyorsanız, **4** "Ekle veya Değiştir" işlemi kullanmanız gerekir; adım için **5** kullanması gereken bir "silin var" işlemi kullanmakta olduğunuz istemci Kitaplığı'nda. Başka bir depolama sistemi kullanıyorsanız, uygun ıdempotent işlemi kullanmanız gerekir.  

Çalışan rolü asla adım tamamlarsa **6**, sonra da bir zaman aşımından sonra ileti yeniden görünür, sıranın onu yeniden işlemek denemek çalışan rolü için hazır. Çalışan rolü sıraya bir ileti okumak ve gerekirse, bayrak açıldı kaç kez denetleyebilirsiniz ayrı bir sıraya göndermek tarafından araştırma için "zarar" iletisi olduğu. Kuyruk iletileri okumak ve dequeue sayısı denetimi hakkında daha fazla bilgi için bkz: [iletileri almak](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Bazı hatalar tablo ve kuyruk Hizmetleri geçici hataları ve bunları işlemek için uygun yeniden deneme mantığı istemci uygulamanızı içermelidir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm için işlem yalıtım sağlamaz. Örneğin, bir istemci okuyabilir **geçerli** ve **arşiv** tabloları arasında adımları çalışan rolü olduğu zaman **4** ve **5**ve verileri tutarlı bir görünümünü bakın. Veri sonunda tutarlı olacağını unutmayın.  
* Adım 4 ve 5 nihai tutarlılık sağlamak için ıdempotent olduğundan emin olmanız gerekir.  
* Birden çok kuyruklar ve çalışan rolü örnekleri kullanarak çözüm ölçeklendirebilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı bölümleri veya tablo mevcut varlıklar arasında nihai tutarlılığı garanti istediğinizde bu deseni kullanır. Nihai tutarlılık işlemleri için tablo ve Blob hizmeti ve diğer olmayan - Azure Storage veritabanı veya dosya sistemi gibi veri kaynakları sağlamak için bu deseni genişletebilirsiniz.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık Grup işlemleri](#entity-group-transactions)  
* [Birleştirme ya da değiştirme](#merge-or-replace)  

> [!NOTE]
> İşlem yalıtım çözümünüz için önemliyse, EGTs kullanmanızı sağlamak için tablolarınız yeniden düşünmelisiniz.  
> 
> 

### <a name="index-entities-pattern"></a>Dizin varlıkları düzeni
Varlıklar listesi Döndür verimli aramalar etkinleştirmek için dizin varlıkları korur.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo hizmeti otomatik olarak kullanarak varlıkları dizinler **PartitionKey** ve **RowKey** değerleri. Bu, verimli bir şekilde noktası sorgusu kullanarak bir varlık almak bir istemci uygulaması sağlar. Örneğin, aşağıda gösterilen tablo yapısı kullanarak, bir istemci uygulaması verimli bir şekilde bir tek çalışan varlık bölüm adını ve çalışan kimliği kullanarak alabilirsiniz ( **PartitionKey** ve **RowKey**).  

![][13]

Ayrıca son kullanıcıların adı gibi başka bir benzersiz olmayan özelliğinin değeri göre çalışan varlıklar listesini almak istiyorsanız doğrudan aramak için bir dizin kullanmak yerine eşleşmeleri bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Tablo hizmetinde ikincil dizinler sağlamadığından budur.  

#### <a name="solution"></a>Çözüm
Yukarıda gösterilen varlık yapısına sahip son ada göre arama etkinleştirmek için çalışan kimlikleri listesini bulundurmanız gerekir. Can gibi belirli bir son adı çalışan varlıklarıyla almak istiyorsanız, önce kendi son adıyla Etikan olan çalışanlar için çalışan kimliklerinin listesi bulun ve bu çalışan varlıkların almak gerekir. Çalışan kimlikleri listesini depolamak için üç ana seçeneğiniz vardır:  

* BLOB storage'ı kullanın.  
* Çalışan varlıkları aynı bölüme dizin varlıklar oluşturun.  
* Dizin varlıkları ayrı bölüm ya da tablo oluşturun.  

<u>#1. seçenek: Blob storage'ı kullanma</u>  

İlk seçenek için bir listesini her benzersiz soyadı ve her blob Mağazası'nda bir blob oluşturun **PartitionKey** (bölüm) ve **RowKey** son bu ada sahip çalışanlar için (çalışan kimliği) değerleri. Eklemek veya bir çalışan sildiğinizde ilgili blob içeriği çalışan varlıklarıyla sonuçta tutarlı olduğundan emin olmanız gerekir.  

<u>#2. seçenek:</u> aynı bölümde dizin varlıkları oluşturma  

İkinci seçenek için aşağıdaki veri depolayan dizin varlıklar kullanın:  

![][14]

**EmployeeIDs** özelliği depolanan Soyadı ile çalışanlar için çalışan kimlikleri listesini içeren **RowKey**.  

Aşağıdaki adımları ikinci seçeneği kullanıyorsanız, yeni bir çalışan ekleme yapıyorsanız izlemeniz gereken sürecini özetlemektedir. Bu örnekte, bir çalışan kimliği 000152 ve satış departmanında son adıyla Etikan ekliyoruz:  

1. Dizin varlıkla almak bir **PartitionKey** "Satış" değeri ve **RowKey** "Can" değeri 2. adımda kullanmak üzere bu varlığın ETag kaydedin.  
2. Yeni bir çalışan varlık ekleyen bir varlık Grup hareketi (diğer bir deyişle, toplu işlem) oluşturun (**PartitionKey** "Satış" değeri ve **RowKey** değer "000152") ve dizin varlığı güncelleştirir (**PartitionKey** "Satış" değeri ve **RowKey** "Can" değer) EmployeeIDs alan listesinde yeni çalışan kimliği ekleyerek. Varlık Grup işlemler hakkında daha fazla bilgi için bkz: [varlık Grup hareketleri](#entity-group-transactions).  
3. Varlık Grup hareketi (birisi yalnızca dizin varlık değiştirdi) iyimser eşzamanlılık hata nedeniyle başarısız olursa, 1. adımda yeniden başlatabilirsiniz gerekir.  

İkinci seçeneği kullanıyorsanız, bir çalışan silmek için benzer bir yaklaşım kullanabilirsiniz. Bir çalışanın soyadını değiştirmek, biraz daha karmaşık çünkü üç varlık güncelleştirmeleri bir varlık Grup işlemi yürütmek gerekecektir: çalışan varlık, dizin varlık eski soyadı ve yeni Soyadı dizin varlık. Ardından iyimser eşzamanlılık kullanarak güncelleştirmeleri gerçekleştirmek için kullanabileceğiniz ETag değerleri almak için herhangi bir değişiklik yapmadan önce her bir varlık almanız gerekir.  

Aşağıdaki adımları verilen Soyadı bir departmandaki tüm çalışanlarla ikinci seçeneği kullanılıyorsa bakmak gerektiğinde izlemeniz gereken sürecini özetlemektedir. Bu örnekte, satış departmanında son adıyla Etikan tüm çalışanlar yukarı arıyoruz:  

1. Dizin varlıkla almak bir **PartitionKey** "Satış" değeri ve **RowKey** "Can" değeri  
2. Çalışan EmployeeIDs alanında kimlikleri listesini ayrıştırılamıyor.  
3. (Örneğin, e-postaları) bu çalışanların her hakkında ek bilgi gerekirse, her kullanarak çalışan varlıkları almak **PartitionKey** "Satış" değeri ve **RowKey** 2. adımda elde ettiğiniz çalışanların listesini değerleri.  

<u>#3. seçenek:</u> ayrı bölüm veya tablo içinde dizin varlıkları oluşturma  

Üçüncü seçenek için aşağıdaki veri depolayan dizin varlıklar kullanın:  

![][15]

**EmployeeIDs** özelliği depolanan Soyadı ile çalışanlar için çalışan kimlikleri listesini içeren **RowKey**.  

Üçüncü seçenek dizin varlıkları çalışan varlıklardaki ayrı bir bölüme olduğundan tutarlılık sağlamak için EGTs kullanamazsınız. Dizin varlıkları çalışan varlıklarıyla sonuçta tutarlı olduğundan emin olun.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm eşleşen varlıkları almak için en az iki sorgular gerektirir: bir listesini almak için dizin varlıkları sorgulamak için **RowKey** değerler ve listedeki her varlık almak için sorgular.  
* Tek bir varlık 1 MB maksimum boyuta sahip koşuluyla, seçeneği #2 ve seçeneği #3 çözümdeki verilen Soyadı çalışanı kimliklerinin listesi hiçbir zaman 1 MB'den daha büyük olduğu varsayılmaktadır. Çalışan kimliklerinin listesi boyutu 1 MB'den büyük olasılıkla ise #1 seçeneğini kullanın ve blob depolama alanına dizin verileri depolar.  
* Seçenek #2 kullanırsanız, işlem hacmi belli bir bölüm ölçeklenebilirlik sınırları yaklaşımını, (ekleme ve çalışanlar silme ve bir çalışanın soyadını değiştirme işlemek için EGTs kullanarak), değerlendirmeniz gerekir. Bu durumda, güncelleştirme isteklerini işlemek için kuyrukları kullanan ve ayrı bir bölüme çalışan varlıklardaki dizin varlıklarınızı depolamanıza olanak sağlar sonuçta tutarlı çözümünü (#1 veya seçeneği #3) düşünmelisiniz.  
* Bu çözüm #2 seçeneğinde varsayar bir bölüm içinde son ada göre arama yapmak istediğiniz: Örneğin, son adıyla Etikan satış departmanında çalışan listesini almak istiyorsunuz. Son adıyla Etikan tüm kuruluş genelinde tüm çalışanlar aramak istiyorsanız, seçeneğini #1 veya #3 seçeneğini kullanın.
* Nihai tutarlılık sunar sıra tabanlı bir çözüm uygulayabilirsiniz (bkz [sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern) daha fazla ayrıntı için).  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tüm son adıyla Etikan tüm çalışanlar gibi ortak bir özellik değeri paylaşır varlık kümesi için arama yapmak istediğinizde bu deseni kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar düzeni](#compound-key-pattern)  
* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern)  
* [Varlık Grup işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalization deseni
İlgili verileri içeren bir tek nokta sorgu gereken tüm verileri almak üzere etkinleştirmek için tek bir varlık birleştirmek.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında genellikle birden çok tablodan veri sorgularda kaynaklanan çoğaltma kaldırmak için veri normalleştirin. Verilerinizi Azure tablolardaki normalleştirin gerekirse, birden çok gidiş dönüş ilgili verileri almak için sunucuya istemciden yapmanız gerekir. Örneğin, aşağıda gösterilen tablosu yapısına sahip bir bölüm ayrıntılarını almak için iki gidiş dönüş gerekir: bir yöneticisinin kimliği ve bir çalışan varlık Yöneticisi'nin ayrıntılar getirmek için başka bir istek içerir departmanı varlık getirilemedi.  

![][16]

#### <a name="solution"></a>Çözüm
Verileri iki ayrı varlıklarda depolayarak, yerine verileri denormalize ve departman varlıkta Yöneticisi'nin ayrıntılar kopyasını tutun. Örneğin:  

![][17]

Bu özellikleri depolanan departmanı varlıklarıyla noktası sorgusu kullanarak bir bölüm hakkında gereken tüm ayrıntıları alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Ek yükü bazı verileri iki kez saklamanın bazı maliyet yoktur. Depolama maliyetleri Marjinal artırma (daha az isteklerden kaynaklanan depolama birimi hizmeti) performans avantajı genellikle ağır (ve bu maliyet kısmen bir bölüm ayrıntılarını getirmek için gereken işlem sayısı azalmasına tarafından uzaklığı).  
* Yöneticileri hakkında bilgi depolamak iki varlık tutarlılığını bulundurmanız gerekir. Tek bir atomik işlemle birden çok varlık güncelleştirileceğini EGTs kullanarak tutarlılık sorunu işleyebilir: Bu durumda, departman varlık ve bölüm Yöneticisi'ni çalışan varlık aynı bölümünde depolanır.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sık ilgili bilgi aramak ihtiyacınız olduğunda bu deseni kullanır. Bu deseni istemciniz gerektirdiği verileri almak üzere yapmalısınız olan sorgu sayısını azaltır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar düzeni](#compound-key-pattern)  
* [Varlık Grup işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Bileşik anahtar düzeni
Kullanım bileşik **RowKey** tek nokta sorgu ile ilgili veri aramak bir istemci etkinleştirmek için değerleri.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında tek bir sorgu istemcisinde veri ilgili parçalarını dönmek için sorguları birleşimlerde kullanmak için oldukça doğal. Örneğin, bu çalışan için verileri gözden geçirin ve performans içeren ilgili varlıklar listesi bakmak için çalışan kimliği kullanabilirsiniz.  

Aşağıdaki yapısını kullanarak tablo hizmetinde çalışan varlıkları depolayan varsayın:  

![][18]

Ayrıca incelemeler ve performans çalışanın çalıştığı her yıl, kuruluşunuz için ilgili geçmiş verileri depolamak gerekir ve bu bilgileri yıla göre erişebilmeleri gerekir. Bir seçenek varlıkların şu yapıda depolayan başka bir tablo oluşturmaktır:  

![][19]

Bu yaklaşımda, bazı bilgiler (örneğin, ad ve Soyadı) yinelenen karar verebilir, tek bir istek verilerinizle almanıza olanak sağlamak için yeni bir varlık olarak dikkat edin. Ancak, iki varlık otomatik olarak güncelleştirmek için bir EGT kullanamadığından güçlü tutarlılık korunamaz.  

#### <a name="solution"></a>Çözüm
Yeni bir varlık türü aşağıdaki yapısıyla varlıkları kullanarak özgün tablonuzu saklayın:  

![][20]

Bildirim nasıl **RowKey** olan şimdi çalışan kimliği ve çalışanın performans almak ve verileri tek bir varlık için tek bir istekle gözden sağlar gözden geçirme verilerini yılın oluşan bir bileşik anahtarı.  

Aşağıdaki örnek, belirli bir çalışan (örneğin, satış departmanında çalışan 000123) için tüm gözden geçirme verilerini nasıl alabilirsiniz özetlenmektedir:  

$filter (PartitionKey eq 'Satış') = ve (RowKey ge 'empid_000123') ve (RowKey lt 'empid_000124') & $select = RowKey, Yöneticisi derecelendirme, eş derecelendirme, Yorumlar  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Ayrıştırılacak kolaylaştıran bir uygun ayırıcı karakter kullanması gereken **RowKey** değeri: Örneğin, **000123_2012**.  
* Bu varlık, aynı bölüme EGTs güçlü tutarlılık sağlamak için kullanabileceğiniz anlamına gelir aynı çalışan için ilgili verileri içeren diğer varlıklar olarak da depoluyorsanız.
* Bu desen uygun olup olmadığını belirlemek için verileri ne sıklıkla sorgulayacak göz önünde bulundurmalısınız.  Örneğin, gözden geçirme seyrek ve ana çalışan verilere genellikle erişecekse bunları olarak ayrı varlıklar tutmalısınız.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu deseni, bir saklamanız gerekir veya ilgili daha fazla varlıklar, sorgu sık kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık Grup işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#working-with-heterogeneous-entity-types)  
* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Günlük tail düzeni
Alma *n* kullanılarak bir bölüm için en son eklenen varlıklar bir **RowKey** ters tarihi ve saati sipariş sıralar değeri.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Ortak bir gereksinim en son oluşturulan varlıkları alabilir, örneğin on en son çalışan tarafından gönderilen talepleri gider. Tablo sorguları destek bir **$top** sorgu işleminin ilk döndürmesi için *n* bir kümesindeki varlıkların: kümesindeki son n varlıkları döndürme eşdeğer sorgu işlem yok.  

#### <a name="solution"></a>Çözüm
Kullanarak varlıkları depolayan bir **RowKey** doğal sıralar böylece en son giriş kullanarak geri tarih olduğunu her zaman Birinci tablodaki.  

Örneğin, bir çalışan tarafından gönderilen en son on gider talep almak için geçerli tarih/saat türetilmiş bir ters onay değer kullanabilirsiniz. Aşağıdaki C# kod örneği için uygun bir "çizgilerine ters" değer oluşturmanın yollarından gösterir bir **RowKey** , en son sıralar eski için:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Aşağıdaki kodu kullanarak tarih saat değerine geri alabilirsiniz:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Tablo sorgusu şöyle görünür:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dize değeri beklendiği gibi sıralar emin olmak için sıfır eklenerek ters onay değeri paneli gerekir.  
* Bir bölümün düzeyinde ölçeklenebilirlik hedefleri farkında olması gerekir. Dikkatli olun etkin nokta bölümleri oluşturma değil.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Ters tarih sırası veya en son eklenen varlıklar erişim gerektiğinde varlıklarda erişmeye ihtiyacınız olduğunda bu deseni kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Başına / koruma düzeni ekleme](#prepend-append-anti-pattern)  
* [Varlıkları alma](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Yüksek hacimli delete düzeni
Yüksek hacimli varlıkların silinmesini kendi ayrı tabloda eşzamanlı silme işlemi için tüm varlıkları depolayarak etkinleştirme; Tablo silerek varlıkları silin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Birçok uygulama, artık bir istemci uygulaması kullanılabilir olması gerekir veya uygulamayı başka bir depolama ortamına arşivlenmiş eski verileri silin. Genellikle bu tür veriler tarihe göre belirleyin: Örneğin, 60 günden eski olan tüm oturum açma isteklerinin kayıtları silmek için gereksinim.  

Bir olası tasarım kullanmaktır tarih ve saat oturum açma isteğinin **RowKey**:  

![][21]

Uygulama eklemek ve ayrı bir bölüme her kullanıcı için oturum açma varlıkları silmek için bu yaklaşım bölüm etkin önler. Ancak, bu yaklaşım pahalı ve zaman alıcı ilk silmek için tüm varlıkları tanımlamak üzere bir tablo taraması gerçekleştirmeniz gerekir ve ardından eski her varlık silmeniz gerekir çünkü çok sayıda varlık varsa olabilir. Not EGTs birden çok silme isteklerinin'toplu işleme eski varlıkları silmek için gerekli sunucuya gidiş dönüş sayısını azaltabilirsiniz.  

#### <a name="solution"></a>Çözüm
Oturum açma denemesi her gün için ayrı bir tablo kullanın. Etkin noktalarına varlıklar ekleme ve eski varlıkları silmek şimdi her gün bir tablo silme yalnızca bir soru olduğunda kaçınmak için yukarıdaki varlık tasarım kullanabilirsiniz (tek bir depolama işlemi) bulma ve her gün yüzlerce ve tek tek oturum açma varlıklar binlerce silme yerine.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tasarımınızın, uygulamanızın belirli varlıklar, diğer veri ya da oluşturma toplama bilgisi ile bağlama bakarak gibi verileri kullanacak diğer yolları destekliyor mu?  
* Yeni varlıklar eklerken tasarımınızı etkin noktalar önlenir?  
* Aynı tablo adı sildikten sonra yeniden kullanmak istiyorsanız bir gecikme bekler. Her zaman benzersiz tablo adları kullanmak en iyisidir.  
* Tablo hizmeti erişim desenlerini öğrenir ve bölümleri düğümleri arasında dağıtır sırasında yeni bir tablo ilk kullandığınızda, bazı azaltma bekler. Yeni tablo oluşturmak gereken ne sıklıkta göz önünde bulundurmalısınız.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Aynı anda silmelisiniz varlıklar hacmi yüksek olduğunda bu deseni kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık Grup işlemleri](#entity-group-transactions)
* [Varlıkları değiştirme](#modifying-entities)  

### <a name="data-series-pattern"></a>Veri serisi deseni
Tam veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık deposu.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Yaygın bir senaryo, genellikle aynı anda almak için gereken verileri bir dizi depolamak bir uygulamadır. Örneğin, uygulamanız her çalışan her saat gönderir kaç anlık ileti iletileri kaydetmek ve ardından bu bilgileri kaç iletileri çizmek için önceki 24 saat içinde gönderilen her bir kullanıcı kullanın. Her çalışan için 24 varlıkları depolamak için bir tasarım olabilir:  

![][22]

Bu tasarım ile kolayca bulmanıza ve uygulama ileti sayısı değerini güncelleştirmeniz gerektiğinde her bir çalışana güncelleştirilecek varlık güncelleştirin. Ancak, önceki 24 saat için etkinliğin bir grafiği çizmek için bilgi almak için 24 varlıklar almanız gerekir.  

#### <a name="solution"></a>Çözüm
Aşağıdaki tasarım sahip ayrı bir özellik ileti sayısı için her bir saat depolamak için kullanın:  

![][23]

Bu tasarımla, bir çalışanın ileti sayısı için belirli bir saat güncelleştirmek için bir birleştirme işlemi kullanabilirsiniz. Şimdi, tek bir varlık için bir istek kullanarak grafiği çizmek gereken tüm bilgileri alabilir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tam veri dizileriniz (bir varlık en çok 252 özellik olabilir) tek bir varlık uygun değilse, alternatif veri deposu blob gibi kullanın.  
* Bir varlık aynı anda güncelleştirme birden fazla istemciniz varsa kullanmanız gerekecektir **ETag** iyimser eşzamanlılık uygulamak için. Birçok istemciniz varsa, yüksek çakışma karşılaşabilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Güncelleştirme ve tek bir varlık ile ilişkili bir veri serisi almak ihtiyacınız olduğunda bu deseni kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Büyük varlıklar düzeni](#large-entities-pattern)  
* [Birleştirme ya da değiştirme](#merge-or-replace)  
* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern) (, veri serisi bir blob'a depoladığınız varsa)  

### <a name="wide-entities-pattern"></a>Geniş varlıklar düzeni
Birden çok 252 özellik sahip mantıksal varlık depolamak için birden çok fiziksel varlık kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık (zorunlu sistem özellikleri dışında) en çok 252 özellik olabilir ve birden fazla 1 MB veri toplama depolanamıyor. İlişkisel bir veritabanında, genellikle bir satır boyutu üzerinde herhangi bir sınır round yeni bir tablo ekleyerek ve aralarında 1-1 ilişkisi zorlamayı elde edebileceğiniz.  

#### <a name="solution"></a>Çözüm
Tablo hizmeti kullanarak, birden çok 252 özellik tek büyük iş nesnesiyle temsil etmek için birden çok varlık depolayabilirsiniz. Örneğin, son 365 gün için her çalışan tarafından gönderilen IM iletilerin sayısını saklamak isterseniz, iki varlık farklı şemalarda kullanan aşağıdaki tasarım kullanabilirsiniz:  

![][24]

Onları korumak için her iki varlıkları güncelleştirme birbirleri ile eşitlenen gerektiren bir değişiklik yapmanız gerekirse bir EGT kullanabilirsiniz. Aksi takdirde, ileti sayısı için belirli bir gün güncelleştirmek için bir tek birleştirme işlemi kullanabilirsiniz. Tek bir çalışan için tüm verileri almak için her ikisini de kullanmanız iki verimli isteği ile yapabileceğiniz her iki varlığa alma bir **PartitionKey** ve **RowKey** değeri.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Eksiksiz bir mantıksal varlık alma, en az iki depolama işlemleri içerir: biri her fiziksel varlık almak için.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu desen ne zaman kullanın, boyutunu veya sayısını özelliklerinin tablo hizmetinde tek bir varlık için sınırlarını aşıyor varlıkları depolamak gerekir.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık Grup işlemleri](#entity-group-transactions)
* [Birleştirme ya da değiştirme](#merge-or-replace)

### <a name="large-entities-pattern"></a>Büyük varlıklar düzeni
Büyük özellik değerlerini depolamak için BLOB Depolama kullanır.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık 1 MB'tan fazla veri toplama depolanamıyor. Bir veya birkaç özelliklerinizin bu değeri aşacak varlığınız toplam boyutu neden değerleri depolarsanız, tablo hizmetinde tüm varlık depolanamıyor.  

#### <a name="solution"></a>Çözüm
Bir veya daha fazla özellikleri büyük miktarda veri içerdiğinden varlığınız 1 MB boyutunda aşarsa, Blob hizmetinde verileri depolamak ve ardından bir özellikte varlık blob adresi depolamak. Örneğin, bir çalışanın fotoğrafı blob storage'da depolamak ve fotoğraf bağlantı depolamak **fotoğraf** çalışan varlığınız özelliği:  

![][25]

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo hizmeti varlıkta ve verileri Blob hizmeti arasında nihai tutarlılık sağlamak için kullanmak [sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern) varlıklarınızı korumak için.
* Eksiksiz bir varlık alma, en az iki depolama işlemleri içerir: bir varlık ve bir blob verileri almak üzere alınamadı.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Büyüklüğü tablo hizmetinde tek bir varlık için sınırlarını aşıyor varlıkları depolamak ihtiyacınız olduğunda bu deseni kullanır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonuçta tutarlı işlemleri düzeni](#eventually-consistent-transactions-pattern)  
* [Geniş varlıklar düzeni](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Koruma deseni başına ve ekleme
Birden çok bölüm arasında eklemeleri yayarak eklemeleri hacmi yüksek olduğunda ölçeklenebilirliği artırır.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Eklenmesini veya varlıklar saklı varlıklarınızı genellikle sonuna ekleme sırası bölümlerinin ilk veya son bölümü için yeni varlıklar ekleme uygulama sonuçlanır. Bu durumda, tüm eklemeleri verilen herhangi bir zamanda birden çok düğümüne ekler Dengeleme ve büyük olasılıkla bölüm ölçeklenebilirlik hedefleri isabet uygulamanıza neden yük tablo hizmetinden engelleyen bir etkin nokta oluşturma aynı bölüme yerinde sürüyor. Örneğin, bir uygulamanız varsa ağ kaydeder ve kaynak erişimini çalışanlar, aşağıda gösterildiği gibi daha sonra bir varlık yapısı tarafından birimin işlemlerinin tek bir bölüm için ölçeklenebilirlik hedef ulaşırsa bir etkin nokta olmadan geçerli saatlik bölüm neden olabilir:  

![][26]

#### <a name="solution"></a>Çözüm
Aşağıdaki alternatif varlık yapısını belirli bölümlerinin etkin nokta uygulama günlükleri olayları ortadan kaldırır:  

![][27]

Bu örnekle nasıl hem bildirimi **PartitionKey** ve **RowKey** bileşik anahtarlar. **PartitionKey** günlüğe birden çok bölüm arasında dağıtmak için hem Bölüm hem de çalışana kimliğini kullanır.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dinamik bölümleri ekler üzerinde verimli bir şekilde oluşturulmasını engeller alternatif anahtar yapısı istemci uygulamanızı yapar sorgularını destekliyor mu?  
* Beklenen biriminiz işlemlerinin tek bir bölüm için ölçeklenebilirlik hedefleri erişmek ve depolama hizmeti tarafından kısıtlanan büyük olasılıkla anlama geliyor?  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Biriminiz işlemlerinin büyük bir olasılıkla dinamik bir bölüm eriştiğinizde depolama hizmeti tarafından azaltma neden olduğunda prepend ve append koruma düzeni kaçının.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzeni uygularken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar düzeni](#compound-key-pattern)  
* [Günlük tail düzeni](#log-tail-pattern)  
* [Varlıkları değiştirme](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Günlük veri koruma düzeni
Genellikle, Blob hizmeti yerine tablo hizmeti günlük verilerini depolamak için kullanmanız gerekir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Özel tarih aralığı için günlük girişlerini seçimi almak için günlük verileri için ortak bir kullanım örneği: Örneğin, tüm hata ve kritik iletileri 15:04 15:06 belirli bir tarihte arasındaki uygulamanızı günlüğe bulmak istediğiniz. Günlük varlıklara kaydettiğiniz bölüm belirlemek için tarih ve saat günlük iletisi kullanmak istiyor musunuz: belirli bir zamanda, tüm günlük varlıkları aynı paylaşacak çünkü etkin bir bölümünde sonuçlarının **PartitionKey** değeri (bölümüne bakın [Prepend ve append koruma düzeni](#prepend-append-anti-pattern)). Örneğin, uygulamanın tüm günlük iletilerini bölüm için geçerli tarih ve saat için yazdığından aşağıdaki varlık şemanın bir günlük iletisi için etkin bir bölümünde sonuçları:  

![][28]

Bu örnekte, **RowKey** tarih ve saat günlük iletilerini tarih sırasına koyulmuş depolanan emin olmak için günlük iletisinin içerir ve birden çok günlük iletilerini aynı tarih ve saat paylaşmak durumunda bir ileti kimliği içerir.  

Başka bir yaklaşım kullanmaktır bir **PartitionKey** sağlar uygulama bölümleri aralığı iletileri yazar. Örneğin, günlüğü kaynağı iletisi iletilerini birçok bölüm arasında dağıtmak için bir yol sağlar, aşağıdaki varlık şema kullanabilirsiniz:  

![][29]

Belirli bir süre için tüm günlük iletileri almak için her bölüm tablosunda arama gerekir, ancak bu şemayı sorun olur.

#### <a name="solution"></a>Çözüm
Önceki bölümde günlük girişlerini ve önerilen iki, yetersiz, tasarımları depolamak için tablo hizmeti kullanmayı denemek sorunu vurgulanır. Günlük iletileri yazma performansının düşük riskini etkin bir bölüm için bir çözüm öncülük; başka bir çözümü her bölüm tablosundaki belirli bir süre için günlüğü iletileri almak için tarama gereksinimi nedeniyle zayıf sorgu performansı sonuçlandı. BLOB storage bu tür senaryosu için daha iyi bir çözüm sunar ve bu, nasıl Azure depolama çözümlemeleri depoları günlük verilerini toplar.  

Bu bölümde, nasıl Storage Analytics bir aralık tarafından genellikle sorgu verileri depolamak için bu yaklaşım çizimi olarak blob depolama alanına günlük verilerini depolar özetlenmektedir.  

Storage Analytics günlük iletilerini birden çok BLOB'ları sınırlandırılmış biçimde depolar. Sınırlandırılmış biçimde günlük iletisi verileri ayrıştırmak bir istemci uygulaması kolaylaştırır.  

Depolama çözümlemeleri için aradığınız günlük iletilerini içeren blob bulmanızı sağlar BLOB'ları (veya BLOB'ları) için adlandırma kuralı kullanır. Örneğin, "queue/2014/07/31/1800/000001.log" adlı bir blob, kuyruk hizmeti 18:00 31 Temmuz 2014 üzerinde başlayan bir saat ile ilgili günlük iletilerini içerir. "000001" Bu ilk günlük dosyası bu dönem için gösterir. Storage Analytics ayrıca blob'un meta verilerinin bir parçası dosyasında depolanan ilk ve son günlük iletilerini damgalarının kaydeder. Blob depolama etkinleştirir adı ön ekini temel alarak bir kapsayıcıdaki blobları bulmanıza API'si: 18: 00'dan başlayarak saat için sıraya günlük verilerini içeren tüm BLOB'lar bulmak için "sıra/2014/07/31/1800." öneki kullanabilir  

Depolama Analytics arabellekleri iletileri dahili olarak oturum açın ve düzenli aralıklarla uygun blob güncelleştirir veya en son toplu günlük girişlerinin ile yeni bir tane oluşturur. Blob hizmeti gerçekleştirmelisiniz yazma sayısını azaltır.  

Benzer bir çözüm, kendi uygulamanızda uyguluyorsanız, yönetme ve güvenilirlik (olduğu sürece her günlük girişinin blob depolama alanına yazılmasını) ve Maliyet (güncelleştirmeler, uygulamanızda arabelleğe alma ve blob depolama yığınlardaki yazma) ölçeklenebilirlik arasındaki dengelemeyi dikkate almanız gerekir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Günlük verilerini depolamak nasıl karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Olası dinamik bölümleri önler bir tablo tasarımı oluşturursanız, günlük verilerinizi verimli bir şekilde erişemiyor bulabilirsiniz.  
* Günlük verileri işlemek için bir istemci genellikle çok sayıda kayıt yüklemesi gerekir.  
* Günlük verileri genellikle yapılandırılmış olsa da, blob depolama daha iyi bir çözüm olabilir.  

### <a name="implementation-considerations"></a>Uygulama konuları
Bu bölümde önceki bölümlerde açıklanan desenleri uyguladığınızda de hesaba katılmalıdır üzere konuları bazıları açıklanmaktadır. Bu bölümde çoğunu depolama istemci kitaplığı (sürüm 4.3.0 zaman yazma) kullanarak C# ile yazılmış örnekler kullanır.  

### <a name="retrieving-entities"></a>Varlıkları alma
Bölümünde açıklandığı gibi [sorgulamak için tasarım](#design-for-querying), en verimli sorgudur noktası sorgusu. Ancak, bazı senaryolarda birden çok varlık almak gerekebilir. Bu bölüm için depolama istemci kitaplığı kullanılarak varlıkları almak için bazı ortak yaklaşımlar açıklar.  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a>Depolama istemci kitaplığı kullanılarak noktası sorgusu yürütme
Noktası sorguyu yürütmek için en kolay yolu kullanmaktır **almak** sahip bir varlık alan aşağıdaki C# kod parçacığında gösterildiği gibi işlemi tablo bir **PartitionKey** "Satış" değerinin ve **RowKey** "212" değerinin:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Bu örnek varlık nasıl bekliyor fark türünde olmasını alır. **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>LINQ kullanarak birden çok varlık alma
LINQ ile depolama istemci kitaplığı kullanılarak ve bir sorgu belirterek birden çok varlık almak bir **burada** yan tümcesi. Bir tablo taraması önlemek için her zaman içermelidir **PartitionKey** değeri WHERE yan tümcesi ve mümkünse **RowKey** tablo ve bölüm taramalar önlemek için değer. Tablo hizmeti nerede kullanılacak Karşılaştırma işleçleri (büyük, daha büyük veya eşit, daha az daha küçüktür veya eşit, eşit ve eşit değildir), sınırlı bir kümesini destekler yan tümcesi. Aşağıdaki C# kod parçacığını tüm çalışanlar, son adı ile başlayan, "B" bulur (varsayılarak **RowKey** Soyadı depolar) satış departmanında (varsayılarak **PartitionKey** bölüm adını depolar):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Sorgu her ikisi de nasıl belirtiyor fark bir **RowKey** ve **PartitionKey** daha iyi bir performans sağlamak için.  

Aşağıdaki kod örneği fluent API kullanılarak eşdeğer işlevselliğini göstermektedir (genel olarak, akıcı API'ler hakkında daha fazla bilgi için bkz [Fluent API tasarlamak için en iyi yöntemler](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

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
> Birden çok örnek katmandan **CombineFilters** üç filtre koşulları dahil etmek için yöntemler.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Sorgudan çok sayıda varlık alma
En iyi sorgu göre tek bir varlık döndüren bir **PartitionKey** değeri ve **RowKey** değeri. Bununla birlikte, bazı senaryolarda aynı bölüme ya da birçok bölüm birçok varlıkları döndürme zorunluluğu olabilir.  

Her zaman tam gibi senaryolarda, uygulamanızın performansını test etmeniz gerekir.  

Tablo hizmetinde bir sorgu en çok 1.000 varlıkları aynı anda döndürebilir ve en fazla beş saniyede için yürütür. Sonuç kümesi sorgu beş saniye içinde tamamlanmadı 1000'den fazla varlıklar varsa veya sorgu bölüm sınır kestiği, tablo hizmeti sonraki varlıkları kümesinin istemek istemci uygulaması etkinleştirmek için devamlılık belirteci döndürür. İş devamlılığı nasıl belirteçleri hakkında daha fazla bilgi için bkz: [sorgu zaman aşımı ve sayfa numaralandırma](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Depolama istemcisi kitaplığı kullanıyorsanız, tablo hizmetinden varlıklar döndürüyor gibi otomatik olarak devamlılık belirteçleri sizin için işleyebilir. Tablo hizmeti bunları bir yanıt döndürürse depolama istemci kitaplığı kullanılarak otomatik olarak aşağıdaki C# kod örneği devamlılık belirteçleri işler:  

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

Aşağıdaki C# kodu devamlılık belirteçleri açıkça işler:  

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

Devamlılık belirteçleri açıkça kullanarak, uygulamanızın veri sonraki kesimin zaman alır kontrol edebilirsiniz. Örneğin, istemci uygulamanız kullanıcıların tablo içinde saklanan varlıkları aracılığıyla sayfasında olanak sağlayan, bir kullanıcı, uygulamanızın, kullanıcının geçerli kesime alanındaki tüm varlıkları sayfalama bittiğinde sonraki segment almak için devamlılık belirteci yalnızca kullanırsınız şekilde sorgu tarafından alınan tüm varlıklar aracılığıyla sayfası değil karar verebilirsiniz. Bu yaklaşımın birkaç avantaj vardır:  

* Tablo hizmetinden almak için veri miktarını sınırlamak sağlar ve ağ üzerinden taşıması.  
* . NET'te zaman uyumsuz g/ç gerçekleştirmenizi sağlar.  
* Bir uygulama çökmesi durumunda devam edebilmek için kalıcı depolama birimine devamlılık belirteci seri hale getirmek sağlar.  

> [!NOTE]
> Daha az olabilir ancak bir devamlılık belirteci 1.000 varlık içeren bir kesimi genellikle döndürür. Bir sorgunun döndürdüğü kullanarak girdi sayısı sınırlarsa da böyledir **ele** arama ölçütlerinizle eşleşen ilk n varlıklar döndürülecek: Tablo hizmeti kalan varlık almaya sağlamak için devamlılık belirteci ile birlikte n varlıklar taneden içeren bir segment döndürebilir.  
> 
> 

Aşağıdaki C# kod içinde bir kesim döndürülen varlıkların sayısını değiştirmek nasıl gösterir:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Server-side projection
Tek bir varlık, en fazla 255 özelliklere sahip ve en çok 1 MB boyutunda olmalıdır. Tabloyu sorgulamak ve varlıkları almak, tüm özellikler gerekli değildir ve gereksiz yere (gecikme süresi ve maliyetini azaltmaya yardımcı olmak üzere) veri aktarımı önleyebilirsiniz. Sunucu tarafı projeksiyon gereksinim özellikleri aktarmak için kullanabilirsiniz. Aşağıdaki örnek alır olduğundan yalnızca **e-posta** özelliği (ile birlikte **PartitionKey**, **RowKey**, **zaman damgası**, ve **ETag**) sorgu tarafından seçilen gelen.  

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

Bildirim nasıl **RowKey** değeri, almak için özellikler listesinde eklenmemiş olsa da kullanılabilir.  

### <a name="modifying-entities"></a>Varlıkları değiştirme
Depolama istemcisi kitaplığı, ekleme, silme ve varlıkları güncelleştirme tablo hizmetinde depolanan varlıklarınızı değiştirmenize olanak sağlar. Birden çok ekleme, güncelleştirme ve silme işlemleri birlikte gerekli gidiş dönüş sayısını azaltmak için batch ve çözümünüzün performansını artırmak için EGTs kullanabilirsiniz.  

Depolama istemcisi kitaplığı bir EGT genellikle yürüttüğünde oluşturulan özel durumları batch başarısız olmasına neden varlık dizinini içerdiğini unutmayın. EGTs kullanan kodu ayıklarken bu yararlıdır.  

İstemci uygulamanızı eşzamanlılık ve güncelleştirme işlemlerinin nasıl işlediğini tasarımınızı nasıl etkileyeceğini de dikkate almalısınız.  

#### <a name="managing-concurrency"></a>Eşzamanlılık yönetme
Varsayılan olarak tablo hizmeti iyimser eşzamanlılık denetler ayrı varlıklar için düzeyinde uygulayan **Ekle**, **birleştirme**, ve **silmek** işlemleri, bir istemci bu denetimleri atlamak için tablo hizmeti zorlamak mümkün olsa. Tablo hizmeti eşzamanlılık nasıl yönettiği hakkında daha fazla bilgi için bkz: [yönetme eşzamanlılık Microsoft Azure depolama alanında](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Birleştirme ya da değiştirme
**Değiştir** yöntemi **TableOperation** sınıfı her zaman tam varlık tablo hizmetinde yerini alır. Bu özellik saklı varlıkta mevcut olduğunda, bir özellik istekte içermiyorsa, istek bu özellik saklı varlıktan kaldırır. Bir özellik açıkça depolanan bir varlıktan kaldırmak istemediğiniz sürece, istekte her özellik eklemeniz gerekir.  

Kullanabileceğiniz **birleştirme** yöntemi **TableOperation** bir varlığı güncelleştirmek istediğiniz zaman tablo hizmetine gönderdiğiniz veri miktarını azaltmak için sınıf. **Birleştirme** yöntemi istekte bulunan varlık özellik değerleriyle saklı varlıktaki tüm özelliklerini değiştirir, ancak istekte dahil edilmeyen saklı varlık özelliklerinde dokunmaz. Büyük varlıklar varsa ve yalnızca küçük bir istek özelliklerinde çeşitli güncelleştirmeniz gerektiğinde kullanışlıdır.  

> [!NOTE]
> **Değiştir** ve **birleştirme** yöntemler başarısız varlık mevcut değilse. Alternatif olarak, kullandığınız **Yerleştir veya Değiştir** ve **InsertOrMerge** yoksa, yeni varlık oluşturma yöntemleri.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Heterojen varlık türleri ile çalışma
Tablo hizmeti bir *şema daha az* tasarımınızda büyük esneklik sağlayan birden çok tür varlıklarının tek bir tabloyu depolayabilir anlamına gelir tablo deposu. Aşağıdaki örnek, çalışan ve departman varlıkları depolayan bir tablo gösterir:  

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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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

Her varlığın hala olmalıdır Not **PartitionKey**, **RowKey**, ve **zaman damgası** değerleri, ancak herhangi bir özellikler kümesi olabilir. Ayrıca, bir şey yok Bu bilgileri bir yerde saklamak seçmediğiniz sürece bir varlık türünü belirtmek için. Varlık türü tanımlamak için iki seçenek vardır:  

* Varlık türü başına **RowKey** (veya büyük olasılıkla **PartitionKey**). Örneğin, **EMPLOYEE_000123** veya **DEPARTMENT_SALES** olarak **RowKey** değerleri.  
* Aşağıdaki tabloda gösterildiği gibi kayıt varlık türü için ayrı özelliğini kullanın.  

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
<th>EntityType</th>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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
<th>EntityType</th>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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
<th>EntityType</th>
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
<th>EntityType</th>
<th>FirstName</th>
<th>Soyadı</th>
<th>Yaş</th>
<th>E-posta</th>
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

Varlık eklenmesini ilk seçenek türü için **RowKey**, farklı türlerde iki varlık aynı anahtar değerine sahip olabileceğiniz olasılığı varsa faydalıdır. Ayrıca, aynı türde birlikte bölümündeki varlıklar gruplandırır.  

Bu bölümde açıklanan teknikleri tartışmaya yakından ilgili olan [devralma ilişkileri](#inheritance-relationships) bölümünde bu kılavuzun önceki bölümlerinde [ilişkileri modelleme](#modelling-relationships).  

> [!NOTE]
> İstemci uygulamaların POCO nesneleri gelişmesi ve farklı sürümleri ile çalışmasını sağlamak için varlık türü değeri bir sürüm numarası dahil olmak üzere göz önünde bulundurmalısınız.  
> 
> 

Bu bölüm geri kalanı aynı tablodaki birden çok varlık türü ile çalışmayı kolaylaştırmak için depolama istemci kitaplığı özelliklerinde bazıları açıklanmaktadır.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Heterojen varlık türleri alınıyor
Depolama istemcisi kitaplığı kullanıyorsanız, birden çok varlık türleri ile çalışmak için üç seçeneğiniz vardır.  

Belirli bir ile depolanan varlık türünü biliyorsanız **RowKey** ve **PartitionKey** değerleri varlık türünde varlıklar almak önceki iki örneklerde gösterildiği gibi aldığınızda varlık türünü belirtebilirsiniz ardından **EmployeeEntity**: [için depolama istemci kitaplığı kullanılarak noktası sorgu yürütülürken](#executing-a-point-query-using-the-storage-client-library) ve [LINQ kullanarak birden çok varlık alma](#retrieving-multiple-entities-using-linq).  

İkinci seçenek kullanmaktır **DynamicTableEntity** türü (bir özellik paketi) yerine (Bu seçenek ayrıca performansı iyileştirebilir seri hale getirmek ve seri durumdan .NET türleri varlığa gerek yoktur çünkü) somut bir POCO varlık türü. Aşağıdaki C# kodu potansiyel olarak farklı türlerde birden çok varlık tablosundan alır, ancak tüm varlıklar olarak döndürür **DynamicTableEntity** örnekleri. Daha sonra kullanır **EntityType** özelliği her bir varlık türünü belirlemek için:  

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

Kullanmalısınız diğer özelliklerini almak için unutmayın **TryGetValue** yöntemi **özellikleri** özelliği **DynamicTableEntity** sınıfı.  

Kullanarak birleştirmek için üçüncü seçenek olan **DynamicTableEntity** türü ve bir **Varlıkçözümleyicisi** örneği. Bu, birden çok POCO tür aynı sorguda çözümlemek sağlar. Bu örnekte, **Varlıkçözümleyicisi** temsilci kullanarak **EntityType** sorgunun döndürdüğü varlık iki tür arasında ayrım yapmak için özellik. **Gidermek** yöntemi kullanan **çözümleyici** çözümlemek için temsilci **DynamicTableEntity** için örnekler **TableEntity** örnekleri.  

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
Silmek için bir varlık türünü bilmeniz gerek yoktur ve bu eklediğinizde her zaman bir varlık türünü bilmeniz. Ancak, kullanabileceğiniz **DynamicTableEntity** türünü bilerek ve bir POCO varlık sınıfı kullanarak olmadan bir varlığı güncelleştirmek için türü. Aşağıdaki kod örneği, tek bir varlık alır ve denetler **EmployeeCount** özellik var güncelleştirmeden önce.  

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
İstemci uygulamaları doğrudan tablo hizmeti ile kimlik doğrulaması gerek kalmadan doğrudan tablo varlıkları değiştirmek (ve sorgulamak) etkinleştirmek için paylaşılan erişim imzası (SAS) belirteçleri kullanabilirsiniz. Genellikle, uygulamanızda SAS kullanarak için üç ana faydası vardır:  

* Erişim ve tablo hizmeti varlıklarda değiştirmek o aygıtı izin vermek üzere depolama hesabı anahtarınızı güvenli olmayan bir platforma (örneğin, bir mobil cihaz) dağıtmak gerekmez.  
* Web ve çalışan rolleri varlıklarınızı son kullanıcı bilgisayarlar gibi istemci cihazları ve mobil aygıtlara yönetilmesindeki gerçekleştirmek işinin bir kısmını boşaltabilir.  
* Bir kısıtlanmış atayabilir ve izinleri (örneğin, belirli kaynaklara salt okunur erişime izin verme) istemciye zaman sınırlıdır.  

SAS belirteci tablo hizmeti ile kullanma hakkında daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Ancak, yine bir istemci uygulaması tablo hizmeti varlıklarla vermek SAS belirteci oluşturmanız gerekir: depolama hesabı anahtarlarını güvenli erişimi olan bir ortamda yapmalısınız. Genellikle, web veya çalışan rolü SAS belirteçleri oluşturmak ve bunları varlıklarınızı erişmesi gereken istemci uygulamaları için teslim etmek için kullanılır. Olduğundan hala bir ek yük oluşturma ve SAS belirteci istemciler için en iyi şekilde nasıl düşünmelisiniz yüksek hacimli senaryolarda özellikle bu yükünü azaltmak için teslim etmek dahil edilen.  

Bir alt tabloda bulunan varlıklara erişim veren bir SAS belirteci oluşturmak mümkündür. Varsayılan olarak, tüm tablo için bir SAS belirteci oluşturur, ancak SAS belirteci bir dizi ya da erişim izni belirtmek mümkündür **PartitionKey** değerleri veya bir dizi **PartitionKey** ve **RowKey** değerleri. Her kullanıcının SAS belirteci yalnızca kendi varlıklarına erişmek için tablo hizmetinde veren şekilde sisteminizin bireysel kullanıcılar için SAS belirteci oluşturmak isteyebilirsiniz.  

### <a name="asynchronous-and-parallel-operations"></a>Zaman uyumsuz ve paralel işlemleri
İsteklerinizi birden çok bölüm arasında yayılmak sağlanan zaman uyumsuz veya paralel sorguları kullanarak verimlilik ve istemci yanıt hızını artırabilir.
Örneğin, tablolarınızı paralel erişme iki veya daha fazla çalışan rolü örneklerine sahip olabilir. Ayrı ayrı çalışan rolleri bölümler belirli kümeleri için sorumlu olan veya yalnızca birden çok çalışan rolü örnekleri, her bir tablodaki tüm bölümleri erişim olanağına sahip.  

Bir istemci örneği içinde depolama işlemlerini zaman uyumsuz olarak yürüterek üretilen işi artırabilir. Depolama istemcisi kitaplığı zaman uyumsuz sorgular ve değişiklikleri yazma kolay hale getirir. Örneğin, aşağıdaki C# kodu gösterildiği gibi bir bölümdeki tüm varlıklar alır zaman uyumlu yöntemi ile başlayabilirsiniz:  

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

Sorgu zaman uyumsuz şekilde olarak çalışır, böylece bu kodu kolayca değiştirebilirsiniz:  

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

Zaman uyumsuz Bu örnekte, zaman uyumlu sürümünden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası artık içerir **zaman uyumsuz** değiştiricisini ve döndürür bir **görev** örneği.  
* Çağırmak yerine **ExecuteSegmented** sonuçları, yöntemi şimdi almak için yöntemini çağırır **ExecuteSegmentedAsync** yöntemi ve kullandığı **await** sonuçlarını zaman uyumsuz olarak almak için değiştiricisi.  

İstemci uygulaması birden çok kez bu yöntemi çağırabilmeniz (farklı değerlere sahip **departmanı** parametresi), ve her sorgu ayrı bir iş parçacığı üzerinde çalışır.  

Hiçbir zaman uyumsuz sürümü olduğuna dikkat edin **yürütme** yönteminde **TableQuery** çünkü sınıf **IEnumerable** arabirimi zaman uyumsuz numaralandırması desteklemez.  

Ekle, Güncelleştir ve varlıkları zaman uyumsuz olarak silin. Aşağıdaki C# örnek eklemek veya çalışan varlığı değiştirmek için basit, zaman uyumlu bir yöntemi gösterir:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Güncelleştirmeyi zaman uyumsuz şekilde olarak çalışır kolayca bu kodu değiştirebilirsiniz:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Zaman uyumsuz Bu örnekte, zaman uyumlu sürümünden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası artık içerir **zaman uyumsuz** değiştiricisini ve döndürür bir **görev** örneği.  
* Çağırmak yerine **yürütme** yöntemi şimdi varlık güncelleştirileceğini yöntemini çağırır **ExecuteAsync** yöntemi ve kullandığı **await** sonuçlarını zaman uyumsuz olarak almak için değiştiricisi.  

İstemci uygulaması bunun gibi birden çok zaman uyumsuz yöntemleri çağırabilir ve her yöntem çağırma ayrı bir iş parçacığı üzerinde çalışır.  

### <a name="credits"></a>KREDİLERİ
Aşağıdaki üyeleri Azure ekibi Katkıları için teşekkür ederiz isteriz: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah ve Serdar Ozler yanı sıra Microsoft DX gelen zel Hollander. 

Biz de gözden geçirme döngüsü sırasında aşağıdaki Microsoft MVP ait değerli görüşlerini için teşekkür ederiz ister misiniz: Igor Papirov ve Edward Bakker.

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

