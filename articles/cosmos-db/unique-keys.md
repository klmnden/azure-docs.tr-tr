---
title: Azure Cosmos DB'de benzersiz anahtarlar kullanın
description: Azure Cosmos DB veritabanınıza benzersiz anahtarlar kullanmayı öğrenin
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sngun
ms.openlocfilehash: 73d4ba0c82f26a6249528f2dbef1fd30f99ccedb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475882"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Azure Cosmos DB'de benzersiz anahtar kısıtlamaları

Benzersiz anahtarlar, Cosmos kapsayıcıya bir veri bütünlüğü katmanı ekleme olanağı sunar. Bir Cosmos kapsayıcı oluştururken bir benzersiz anahtar ilkesi oluşturun. Benzersiz anahtarlara sahip benzersiz bir mantıksal bölüm içerisindeki bir veya daha fazla değer sağlamak (başına benzersizliği garanti edebilir [bölüm anahtarı](partition-data.md)). Bir kapsayıcı benzersiz bir anahtar ilke oluşturduktan sonra yeni (veya güncelleştirilmiş) yinelenen öğeleri benzersiz anahtar kısıtlaması tarafından belirtilen mantıksal bölüm içindeki oluşturmadan engeller. Bölüm anahtarı, kapsam içinde bir öğe kapsayıcı benzersiz anahtar garanti benzersizliği ile birleştirilmiş.

Örneğin, bir Cosmos kapsayıcı benzersiz anahtar kısıtlaması olarak e-posta adresiyle göz önünde bulundurun ve `CompanyID` bölüm anahtarı olarak. Kullanıcının e-posta adresi benzersiz bir anahtar yapılandırarak, her öğe içinde benzersiz bir e-posta adresi sahip olduğundan emin olun bir verilen `CompanyID`. İki öğe, yinelenen bir e-posta adreslerine sahip ve aynı bölüm anahtarı değeri ile oluşturulamaz.  

Kullanıcılar aynı e-posta adresi, ancak değil aynı ada, son adı ve e-posta adresi ile birden çok öğe oluşturmak için yeteneği sağlamak istiyorsanız, ek yollar için benzersiz anahtar ilkesi ekleyebilirsiniz. E-posta adresini temel alan benzersiz bir anahtar oluşturmak yerine benzersiz bir anahtar da oluşturabilirsiniz ile birlikte ad, Soyadı ve e-posta adresi (benzersiz bir bileşik anahtarı). Bu durumda, içinde üç her benzersiz birleşimi değerleri bir verilen `CompanyID` izin verilir. Örneğin, kapsayıcı her öğeyi benzersiz anahtar kısıtlaması burada uygularken aşağıdaki değerlere sahip öğeler içerebilir.

|Companyıd|Ad|Soyadı|E-posta adresi|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Çalışan Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Yukarıdaki tabloda listelenen birleşimleri olan başka bir öğe eklemeye çalışırsanız, benzersiz anahtar kısıtlaması karşılanmadığı belirten bir hata alırsınız. Ya da "sahip kaynak belirtilen kimliği veya adı zaten var" alırsınız veya "Zaten var. bir dönüş iletisi olarak belirtilen kimliği, adı ve benzersiz bir dizin ile kaynak".  

## <a name="defining-a-unique-key"></a>Benzersiz bir anahtar tanımlama

Bir Cosmos kapsayıcı oluştururken benzersiz anahtarlar tanımlayabilirsiniz. Benzersiz bir anahtar, mantıksal birime kapsamlıdır. Zipcode üzerinde temel kapsayıcı bölerseniz önceki örnekte, mantıksal her bölümde öğeleri çoğaltılmış yedekleme sona erecek. Aşağıdaki özellikler benzersiz anahtarları oluşturulurken göz önünde bulundurun:

* Farklı bir benzersiz anahtar kullanmak için var olan bir kapsayıcı güncelleştirilemiyor. Diğer bir deyişle, ilke, bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra değiştirilemez.

* Var olan bir kapsayıcı için benzersiz anahtar ayarlamak istiyorsanız, benzersiz anahtar kısıtlaması ile yeni bir kapsayıcı oluşturmak ve verileri mevcut kapsayıcısından yeni kapsayıcısına taşımak için uygun veri geçiş aracı kullanmak zorunda. SQL kapsayıcıları için [veri geçiş aracı](import-data.md) verileri taşımak için. MongoDB kapsayıcıları için [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md) verileri taşımak için.

* En fazla 16 yol değerlerinin benzersiz bir anahtar ilke içerebilir (örneğin: /firstName /lastName, / adres/PostaKodu). Her bir benzersiz anahtar ilkesi, en fazla 10 benzersiz anahtar kısıtlamaları olabilir veya birleşimleri ve her bir benzersiz dizin kısıtlaması birleşik yollarını 60 baytı aşmamalıdır. Önceki örnekte, ad, Soyadı ve e-posta adresi yalnızca bir kısıtlama birbirine ve üç 16 olası yolları kullanır.

* Update ve delete bir öğe kapsayıcı bir benzersiz anahtar ilkesi oluşturmak için istek Birimi'ni (RU) ücretleri olduğunda biraz daha yüksek.

* Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yolu eksik değerler benzersizlik kısıtlaması katılmak null değerler olarak kabul edilir. Bu nedenle, yalnızca olabilir bu kısıtlamasını karşılamak için null değerine sahip tek bir öğe.

* Benzersiz anahtar adları büyük/küçük harfe duyarlıdır. Örneğin, bir kapsayıcı için /address/zipcode ayarlamak benzersiz anahtar kısıtlaması ile düşünün. Verilerinizi ZipCode adlı bir alan varsa, "zipcode" "ZipCode" aynı olduğundan, Cosmos DB "null" benzersiz bir anahtar olarak ekler. "Null" yinelenen anahtar benzersiz kısıtlamayı ihlal çünkü bu büyük/küçük harfe duyarlılık nedeniyle tüm ZipCode kayıtlarıyla eklenemez.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md)
