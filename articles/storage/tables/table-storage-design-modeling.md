---
title: Azure depolama Tablo Tasarımı ilişkilerde modelleme | Microsoft Docs
description: Modelleme işlemini tablo depolama çözümünüzü tasarlarken anlayın.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 561281375273135009a956fd27dfe9f193ab92ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661071"
---
# <a name="modeling-relationships"></a>Modelleme ilişkileri
Bu makalede, Azure Table depolama çözümleri tasarlamanıza yardımcı olması için model oluşturma işlemi açıklanır.

Etki alanı model oluşturmaya, karmaşık sistemler tasarımında önemli bir adımdır. Genellikle, varlıkları ve ilişkileri aralarında iş etki alanını anlamak ve sisteminizin tasarımını bildirmek için bir yol olarak tanımlamak için modelleme işlemini kullanın. Bu bölüm, nasıl sizin tasarımlar tablo hizmeti için etki alanı modellerine bulunan ortak ilişki türlerinden bazıları çevirebilir üzerinde odaklanmıştır. İlişkisel veritabanı tasarlarken kullanılır farklı bir fiziksel dayalı NoSQL veri modeli için bir mantıksal veri modelinden eşlemesinin işlemidir. İlişkisel veritabanları tasarımı, genellikle artıklık – ve nasıl uyarlamasını veritabanı işleyişi soyutlar bildirim temelli bir sorgulama yeteneği en aza indirmek için en iyi duruma getirilmiş veri normalleştirme işlemi varsayar.  

## <a name="one-to-many-relationships"></a>Bir-çok ilişkileri
İş etki alanı nesneler arasındaki bir-çok ilişkileri ortaya sık: Örneğin, bir bölümü çok sayıda çalışan vardır. Tablo hizmetinde her Artıları ve eksileri belirli senaryoya ilgili olabilecek ile bir-çok ilişkileri uygulamak için birkaç yolu vardır.  

Büyük çok Ulusal corporation örneği on binlerce Departmanlar ve her departman çok sayıda çalışan ve her çalışan belirli bir bölüm ile ilişkili olarak sahip olduğu çalışan varlıkları ile göz önünde bulundurun. Ayrı departmanı ve bunlar gibi çalışan varlıkları depolamak için bir yaklaşım ise:  


![Depolama ayrı departman ve çalışan varlıklar](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Bu örnek, temel türleri arasında örtük bir-çok ilişkisi gösterir **PartitionKey** değeri. Her bölüm çok sayıda çalışan olabilir.  

Bu örnek ayrıca aynı bölümde departmanı varlık ve ilgili çalışan varlıklarını gösterir. Farklı bölümleri, tablolar veya hatta depolama hesapları farklı varlık türleri için kullanılacak tercih edebilirsiniz.  

Verilerinizi denormalize ve yalnızca çalışan varlıklarıyla aşağıdaki örnekte gösterildiği gibi Normalleştirilmemiş Departman verileri depolamak için alternatif bir yaklaşım var. Bunu yapmak için her çalışan departmanındaki güncelleştirmeniz gerekir çünkü bir bölüm Yöneticisi'ni ayrıntılarını değiştirebilmesi gereksinimi varsa bu belirli bir senaryoda, en iyi Normalleştirilmemiş bu yaklaşım olmayabilir.  

![çalışan varlık](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Daha fazla bilgi için bkz: [Denormalization düzeni](table-storage-design-patterns.md#denormalization-pattern) bu kılavuzda daha sonra.  

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

## <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri varlıklar arasındaki birebir ilişkileri içerebilir. Tablo hizmetinde bire bir ilişki uygulamanız gerekiyorsa, her ikisinin de almanız gerektiğinde, iki ilgili varlık bağlanma seçmeniz gerekir. Bu bağlantıyı biçiminde bir bağlantı depolayarak örtük, bir kural anahtar değerlerine göre veya açık olabilir **PartitionKey** ve **RowKey** ilgili varlık için her bir varlıkta değerleri. Bölüm olup aynı bölümde ilgili varlıkları saklamalısınız bir tartışma için bkz [-çok ilişkileri](#one-to-many-relationships).  

Ayrıca, tablo hizmetinde bire bir ilişkiler uygulamak için yol açabilecek uygulama noktalar vardır:  

* Büyük varlıklar işleme (daha fazla bilgi için bkz: [büyük varlıklar düzeni](table-storage-design-patterns.md#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için bkz: [paylaşılan erişim imzaları ile erişimi denetleme](#controlling-access-with-shared-access-signatures)).  

## <a name="join-in-the-client"></a>İstemci katılma
Tablo hizmetinde ilişkileri modellemek için yollar olsa da, tablo hizmeti kullanmak için iki prime nedenler ölçeklenebilirlik ve performans olduğunu unuttunuz değil. Performans ve ölçeklenebilirlik çözümünüzün tehlikeye birçok ilişki modelleme bulursanız, tablo tasarımınıza tüm veri ilişkileri oluşturmak gerekli olup olmadığını kendiniz istemeniz gerekir. Tasarım basitleştirmek ve istemci uygulamanızı gerekli tüm birleştirmeler gerçekleştirme izin verirseniz çözümünüzün performansını ve ölçeklenebilirliğini artırmak mümkün olabilir.  

Örneğin, sıklıkla değişmeyen veriler içeren küçük tablolar sahip yaparsanız, ardından bu verileri bir kez ve istemcide önbelleğe. Bu, aynı veri almak için yinelenen gidiş-dönüş önleyebilirsiniz. Bu kılavuzda inceledik örneklerde, küçük ve seyrek veri ara olarak istemci uygulamasını bir kez indirebilirsiniz verileri ve önbellek için iyi bir aday yapmadan değiştirmek küçük bir kuruluşta Departmanlar kümesi olasıdır.  

## <a name="inheritance-relationships"></a>Devralma ilişkisi
İstemci uygulamanızın iş varlıkları temsil etmek için bir devralma ilişkisi parçasını sınıflar kümesini kullanıyorsa, bu varlıkların tablo hizmetinde kolayca devam edebilir. Örneğin, aşağıdaki istemci uygulamanızda tanımlanan sınıflar kümesini olabilir nerede **kişi** bir Özet sınıf.

![Soyut kişi sınıfı](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

İki somut sınıfların görünüm bu gibi varlıkları kullanarak tek bir kişi tablosunu kullanarak tablo hizmetinde örneklerini devam edebilir:  

![Kişi tablosu](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

İstemci kodu aynı tablodaki birden çok varlık türleri ile çalışma hakkında daha fazla bilgi için bkz [heterojen varlık türleriyle çalışma](#working-with-heterogeneous-entity-types) bu kılavuzda daha sonra. Bu, istemci kodu varlık türünde tanımak nasıl örnekleri sağlar.  


## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verileri şifrele](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)