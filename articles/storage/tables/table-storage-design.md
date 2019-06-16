---
title: Tasarım ölçeklenebilir ve performansa yönelik tablolar Azure tablo depolama. | Microsoft Docs
description: Tasarım ölçeklenebilir ve performansa yönelik tablolar Azure tablo depolama.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 8387e41d57edfa0e54ac930c9462714aca571f2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60848291"
---
# <a name="design-scalable-and-performant-tables"></a>Ölçeklenebilir ve performansa yönelik tablolar tasarlama

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Tasarım ölçeklenebilir ve performansa yönelik tablolar, performans, ölçeklenebilirlik ve maliyet gibi faktörleri dikkate almanız gerekir. Daha önce şemaları ilişkisel veritabanları için tasarlanan ise bu noktalar ilgili bilgi sahibi olduğunuz, ancak ilişkisel modelleri ve Azure tablo hizmeti depolama modelini arasındaki bazı benzerlikler olsa da, ayrıca önemli farklar vardır. Bu farklılıklar counter-intuitive veya ilişkisel veritabanları ile tanıdık birine yanlış görünüyor, ancak Azure tablo hizmeti gibi bir NoSQL anahtar/değer deposu için tasarlıyorsanız mantıklı farklı tasarımlar genellikle neden. Tablo hizmeti veri veya yüksek işlem desteklemelidir veri kümeleri için varlıklar (veya satır ilişkisel veritabanı terminolojisinde) milyarlarca içerebilir bulut ölçekli uygulamaları desteklemek için tasarlanmıştır olgu yansıtacak çoğu, tasarım farkları birimler. Bu nedenle, farklı şekilde, verilerinizi nasıl depolamanın hakkında düşünün ve tablo hizmetinin nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu çok daha ve ilişkisel bir veritabanı kullanan bir çözüme göre daha düşük bir maliyetle ölçeklendirme çözümünüzü etkinleştirebilirsiniz. Bu kılavuz, şu konularda yardımcı olur.  

## <a name="about-the-azure-table-service"></a>Azure tablo hizmeti hakkında
Bu bölümde bazı performans ve ölçeklenebilirlik için tasarlama yakından ilgili olan tablo hizmeti anahtar özelliklerini vurgular. Azure depolama ve tablo hizmeti yeniyseniz, ilk önce okuma [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md) ve [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md) önce bu makalenin geri kalanında okuma . Bu kılavuzun odağı tablo hizmetinde olsa da, Azure kuyruk ve Blob hizmeti ve nasıl bunları tablo hizmeti ile kullanabilir tartışma içerir.  

Tablo hizmeti nedir? Adından, bekleyebileceğiniz gibi tablo hizmeti tablosal verileri depolamak için kullanır. Standart terminolojisinde, tablodaki her satır bir varlığı temsil eder ve bu varlığa çeşitli özelliklerini sütunları depolayın. Her varlık, bir çift anahtar ve varlığın son güncelleştirilme ne zaman açtıklarını izlemek için tablo hizmetini kullanan bir zaman damgası sütunu benzersiz şekilde tanımlamak için vardır. Zaman damgası otomatik olarak uygulanır ve zaman damgası rastgele bir değeri ile el ile üzerine yazılamaz. Tablo hizmeti, iyimser eşzamanlılık yönetmek için bu son değiştirilme zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo hizmeti REST API işlemleri aynı zamanda sonuç bir **ETag** LMT türeyen bir değer. Bunlar aynı temel alınan verilerde başvurduğundan bu belgeyi ETag ve LMT terimleri birbirlerinin yerine kullanır.  
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


Şu ana kadar bu veri zorunlu sütunları ve öğeleri aynı tabloda birden fazla varlık türleri Depolama olanağı temel farklılıklar ilişkisel bir veritabanı tablosuna benzer görünür. Ayrıca, her biri gibi kullanıcı tanımlı Özellikler **FirstName** veya **yaş** tamsayı veya dize, yalnızca gibi ilişkisel bir veritabanındaki bir sütun gibi bir veri türü vardır. Farklı ilişkisel bir veritabanında tablo hizmeti şemasız doğasını bir özelliği aynı veri türüne her varlık üzerinde olması gerekmez, ancak. Karmaşık veri türlerini tek bir özellik depolamak için JSON veya XML gibi bir seri hale getirilmiş biçimi kullanmanız gerekir. Tablo hizmeti gibi desteklenen veri türleri, desteklenen bir tarih aralıkları, adlandırma kuralları ve boyutu sınırlaması hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).

Tercih ettiğiniz **PartitionKey** ve **RowKey** iyi tablo tasarımı için temeldir. Tabloda depolanan her varlık eşsiz bir bileşimiyle olmalıdır **PartitionKey** ve **RowKey**. Anahtarları gibi bir ilişkisel veritabanı tablosundaki **PartitionKey** ve **RowKey** hızlı göz atmayı etkinleştirmek için kümelenmiş bir dizin oluşturmak için değerleri dizin bulunur. Ancak, tablo hizmeti herhangi bir ikincil dizinler, bu nedenle oluşturmaz **PartitionKey** ve **RowKey** tek Dizinli Özellikler. Açıklanan düzenlerden bazılarını [tablo Tasarım desenleri](table-storage-design-patterns.md) görünen bu sınırlamayı nasıl çalıştığını göstermek.  

Bir tabloyu bir veya daha fazla bölüm oluşur ve birçok tasarım kararları uygun bir geçici bir çözüm seçme olacak **PartitionKey** ve **RowKey** çözümünüzü en iyi duruma getirme. Bir çözümün tüm varlıklarınızı bölümler halinde düzenlenmiş içeren tek bir tablonun oluşabilir, ancak genellikle birden çok tablo bir çözümü vardır. Tablolar, mantıksal olarak varlıklarınızı düzenleyebilir, erişim denetim listeleri kullanarak veri erişimi yönetmenize yardımcı olmak için Yardım ve tek bir depolama işlemi kullanarak tüm bir tabloyu bırakabilirsiniz.  

## <a name="table-partitions"></a>Tablo bölümleri
Tablo adı, hesap adı ve **PartitionKey** birlikte depolama hizmetindeki tablo hizmeti, varlık depoladığı bölüm tanımlar. Adres düzeni varlıklar için bir parçası olmanın yanı sıra, bölümler işlemleri için bir kapsam tanımlama (bkz [varlık grubu işlemleri](#entity-group-transactions) aşağıda), ve tablo hizmeti nasıl ölçeklendirilir temeli oluştururlar. Bölümleri hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md).  

Tablo hizmetinde Hizmetleri tek bir düğüm bir veya daha fazla tamamlamak bölümleri ve hizmet ölçekler dinamik olarak yük dengeleyici tarafından düğümleri arasında bölümler. Bir düğümü yük altında ise, tablo hizmeti için *bölme* bölümler çeşitli hizmet, farklı düğümlerde düğüme tarafından; trafiği kısalana hizmet yapabilirsiniz *birleştirme* sessiz düğümlerden bölüm aralığı geri tek bir düğüme.  

Daha fazla bilgi için iç ayrıntıları tablo hizmeti ve belirli bölümler hizmet yönetir nasıl incelemeye bakın [Microsoft Azure Depolama: Güçlü tutarlılık ile yüksek oranda kullanılabilir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Varlık grubu işlemleri
Tablo hizmeti, varlık grubu işlemleri (EGTs) birden fazla varlıkta atomik güncelleştirmeleri gerçekleştirmek için yalnızca yerleşik mekanizmasıdır. EGTs bazen de denir *toplu işlemler*. EGTs yalnızca çalışabilir varlıklar aynı bölümde depolanır (diğer bir deyişle, bir tablodaki aynı bölüm anahtarına paylaşma). Bu nedenle birden fazla varlıkta atomik işlem davranışı gerektiren her zaman, bu varlıklar aynı bölümde olduğunu emin olmalısınız. Genellikle birden çok varlık türleri aynı tablonun (ve bölüm) tutulması ve birden fazla tablo farklı varlık türleri için kullanmayan bir nedeni de budur. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşleme için birden çok eş zamanlı EGTs gönderirseniz, söz konusu EGTs EGTs arasında ortak olan varlıklar üzerinde çalıştırmayın sağlamak önemlidir; Aksi takdirde, işlem ertelenebilir.

EGTs sıra, tasarımınızın değerlendirmek olası bir denge uygular. Diğer bir deyişle, daha fazla bölüm kullanarak Azure istekleri düğümleri arasında yük dengelemesi için daha fazla fırsat sahip olduğundan, uygulamanızın ölçeklenebilirliği artırır. Ancak daha fazla bölüm kullanarak atomik işlemler gerçekleştirmek ve verileriniz için güçlü tutarlılık sağlamak için uygulamanızın özelliğini sınırlayabilir. Ayrıca, vardır belirli ölçeklenebilirlik hedefleri düzeyinde bir bölümünün işlemlerinin tek bir düğüm için beklediğiniz aktarım hızını sınırlayabilir. Azure depolama hesapları ve tablo hizmeti için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Kapasite konuları
Aşağıdaki tabloda bir tablo hizmeti çözümü yaptığınızda tasarlarken dikkat etmeniz anahtar değerlerden bazıları açıklanmaktadır:  

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

## <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo depolama oldukça Hesaplı, ancak kapasite kullanımı hem de işlem miktarını için maliyet tahminlerini değerlendirmenizi herhangi bir tablo hizmeti çözümünün bir parçası olarak içermelidir. Ancak, birçok senaryoda çözümünüzün ölçeklenebilirlik ve performansı iyileştirmek için normalleştirilmişlikten çıkarılmış veya yinelenen verileri saklama bir geçerli bir yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)
