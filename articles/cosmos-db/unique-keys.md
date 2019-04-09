---
title: Azure Cosmos DB'de benzersiz anahtarlar kullanın
description: Azure Cosmos veritabanınızda benzersiz anahtarlar kullanmayı öğrenin
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.reviewer: sngun
ms.openlocfilehash: 3c5e8a2c85898175772dc353258e77fc8e0a74f2
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263239"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Azure Cosmos DB'de benzersiz anahtar kısıtlamaları

Benzersiz anahtarlar bir veri bütünlüğü katmanı için bir Azure Cosmos kapsayıcısı ekleyin. Bir Azure Cosmos kapsayıcı oluştururken bir benzersiz anahtar ilkesi oluşturun. Benzersiz anahtarlara sahip bir veya daha fazla değer mantıksal bölüm içindeki benzersiz olduğundan emin olun. Ayrıca başına benzersizliği garanti [bölüm anahtarı](partition-data.md). 

Bir kapsayıcı benzersiz bir anahtar ilke oluşturduktan sonra yeni bir oluşturulmasını veya yinelenen bir mantıksal bölüm içinde bunun sonucunda var olan bir öğenin bir güncelleştirme engellendiğinde, benzersiz anahtar kısıtlaması tarafından belirtildiği gibi. Benzersiz bir anahtarla birlikte bölüm anahtarı kapsayıcının kapsamı içindeki bir öğenin benzersizliği garanti eder.

Örneğin, bir Azure Cosmos kapsayıcı benzersiz anahtar kısıtlaması olarak e-posta adresiyle göz önünde bulundurun ve `CompanyID` bölüm anahtarı olarak. Kullanıcının e-posta adresi benzersiz bir anahtar ile yapılandırdığınızda, her öğe içinde benzersiz bir e-posta adresi olan bir verilen `CompanyID`. İki öğe, yinelenen bir e-posta adreslerine sahip ve aynı bölüm anahtarı değeri ile oluşturulamaz. 

Aynı e-posta ile öğeleri oluşturmak için benzersiz anahtar ilkesi için daha yollarını adresi, ancak değil aynı ad, Soyadı ve e-posta adresi ekleyin. Yalnızca e-posta adresini temel alan benzersiz bir anahtar oluşturmak yerine benzersiz bir anahtar da oluşturabilirsiniz ile birlikte ad, Soyadı ve e-posta adresi. Bu anahtarı bileşik benzersiz bir anahtar olarak bilinir. Bu durumda, içinde üç her benzersiz birleşimi değerleri bir verilen `CompanyID` izin verilir. 

Örneğin, kapsayıcı öğeleri burada her öğeyi benzersiz anahtar kısıtlaması geliştirir aşağıdaki değerlerle içerebilir.

|Companyıd|Ad|Soyadı|E-posta adresi|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Çalışan Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Önceki tabloda listelenen birleşimleri olan başka bir öğe eklemeye çalışırsanız, bir hata alırsınız. Benzersiz anahtar kısıtlaması'nin karşılanmadığı durumların hata gösterir. Ya da aldığınız `Resource with specified ID or name already exists` veya `Resource with specified ID, name, or unique index already exists` dönüş iletisi. 

## <a name="define-a-unique-key"></a>Benzersiz bir anahtar tanımlayın

Yalnızca bir Azure Cosmos kapsayıcısı oluşturduğunuzda, benzersiz anahtarlar tanımlayabilirsiniz. Benzersiz bir anahtar, mantıksal birime kapsamlıdır. POSTA koduna göre kapsayıcı bölümlemeniz halinde önceki örnekte, mantıksal her bölümdeki yinelenen öğeler ile sonlanır. Benzersiz anahtarlar oluşturduğunuzda aşağıdaki özellikleri göz önünde bulundurun:

* Farklı bir benzersiz anahtar kullanmak için var olan bir kapsayıcı güncelleştirilemiyor. Diğer bir deyişle, ilke, bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra değiştirilemez.

* Var olan bir kapsayıcı için benzersiz bir anahtar kümesi için benzersiz anahtar kısıtlaması ile yeni bir kapsayıcı oluşturun. Yeni kapsayıcı için mevcut kapsayıcıdan verileri taşımak için uygun veri geçiş aracını kullanın. SQL kapsayıcıları için [veri geçiş aracı](import-data.md) verileri taşımak için. MongoDB kapsayıcıları için [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md) verileri taşımak için.

* En fazla 16 yol değerlerinin bir benzersiz anahtar ilkesi olabilir. Örneğin, değerleri olabilir `/firstName`, `/lastName`, ve `/address/zipCode`. Her bir benzersiz anahtar ilkesi, en fazla 10 benzersiz anahtar kısıtlamaları veya birleşimleri olabilir. Her benzersiz dizin kısıtlaması birleşik yollarını 60 baytı aşmamalıdır. Önceki örnekte, ad, Soyadı ve e-posta adresi bir kısıtlama birleştirilir. Bu kısıtlama, 3 16 olası yolları kullanır.

* Bir kapsayıcı benzersiz bir anahtar ilke olduğunda [istek birimi (RU)](request-units.md) ücretleri oluşturmak için güncelleştirme ve devre dışı bir öğeyi silmek biraz daha yüksektir.

* Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yolu eksik değerler benzersizlik kısıtlaması katılmak null değerler olarak kabul. Bu nedenle, yalnızca tek bir öğe bu kısıtlamasını karşılamak için null değerine sahip olabilir.

* Benzersiz anahtar adları büyük/küçük harfe duyarlıdır. Örneğin, bir kapsayıcı ayarlamak benzersiz anahtar kısıtlaması ile göz önünde `/address/zipcode`. Verilerinizi adlı bir alan varsa `ZipCode`, Azure Cosmos DB ekler "benzersiz anahtar null" çünkü `zipcode` aynı olmayan `ZipCode`. Bu büyük/küçük harfe duyarlılık nedeniyle, "null" yinelenen benzersiz anahtar kısıtlamasını ihlal ettiğinden ZipCode diğer tüm kayıtları eklenemez.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
