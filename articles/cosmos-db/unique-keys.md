---
title: Azure Cosmos DB'de benzersiz anahtarlar kullanın
description: Azure Cosmos veritabanınızda benzersiz anahtarlar kullanmayı öğrenin
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sngun
ms.openlocfilehash: 3a7133d9c092ab8ad8a4bc585e3b0df2b8ca1234
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55999275"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Azure Cosmos DB'de benzersiz anahtar kısıtlamaları

Benzersiz anahtarlar bir veri bütünlüğü katmanı için bir Azure Cosmos kapsayıcısı ekleyin. Bir Azure Cosmos kapsayıcı oluştururken bir benzersiz anahtar ilkesi oluşturun. Benzersiz anahtarlara sahip bir veya daha fazla değer mantıksal bölüm içindeki benzersiz olduğundan emin olun. Ayrıca başına benzersizliği garanti [bölüm anahtarı](partition-data.md). 

Bir kapsayıcı benzersiz bir anahtar ilke oluşturduktan sonra mantıksal bir bölümü içindeki yeni veya güncelleştirilmiş yinelenen öğeler oluşturulmasını engellendiğinde, benzersiz anahtar kısıtlaması tarafından belirtildiği gibi. Benzersiz bir anahtarla birlikte bölüm anahtarı kapsayıcının kapsamı içindeki bir öğenin benzersizliği garanti eder.

Örneğin, bir Azure Cosmos kapsayıcı benzersiz anahtar kısıtlaması olarak e-posta adresiyle göz önünde bulundurun ve `CompanyID` bölüm anahtarı olarak. Kullanıcının e-posta adresi benzersiz bir anahtar ile yapılandırdığınızda, her öğe içinde benzersiz bir e-posta adresi olan bir verilen `CompanyID`. İki öğe, yinelenen bir e-posta adreslerine sahip ve aynı bölüm anahtarı değeri ile oluşturulamaz. 

Aynı e-posta ile öğeleri oluşturmak için benzersiz anahtar ilkesi için daha yollarını adresi, ancak değil aynı ad, Soyadı ve e-posta adresi ekleyin. E-posta adresini temel alan benzersiz bir anahtar oluşturmak yerine benzersiz bir anahtar da oluşturabilirsiniz ile birlikte ad, Soyadı ve e-posta adresi. Bu anahtarı bileşik benzersiz bir anahtar olarak bilinir. Bu durumda, içinde üç her benzersiz birleşimi değerleri bir verilen `CompanyID` izin verilir. 

Örneğin, kapsayıcı öğeleri burada her öğeyi benzersiz anahtar kısıtlaması geliştirir aşağıdaki değerlerle içerebilir.

|Companyıd|Ad|Soyadı|E-posta adresi|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Çalışan Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Önceki tabloda listelenen birleşimleri olan başka bir öğe eklemeye çalışırsanız, bir hata alırsınız. Benzersiz anahtar kısıtlaması'nin karşılanmadığı durumların hata gösterir. Aldığınız ya da "sahip kaynak belirtilen kimliği veya adı zaten var" veya "Zaten var. bir dönüş iletisi olarak belirtilen kimliği, adı ve benzersiz bir dizin ile kaynak". 

## <a name="define-a-unique-key"></a>Benzersiz bir anahtar tanımlayın

Yalnızca bir Azure Cosmos kapsayıcısı oluşturduğunuzda, benzersiz anahtarlar tanımlayabilirsiniz. Benzersiz bir anahtar, mantıksal birime kapsamlıdır. POSTA koduna göre kapsayıcı bölümlemeniz halinde önceki örnekte, mantıksal her bölümdeki yinelenen öğeler ile sonlanır. Benzersiz anahtarlar oluşturduğunuzda aşağıdaki özellikleri göz önünde bulundurun:

* Farklı bir benzersiz anahtar kullanmak için var olan bir kapsayıcı güncelleştirilemiyor. Diğer bir deyişle, ilke, bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra değiştirilemez.

* Var olan bir kapsayıcı için benzersiz bir anahtar kümesi için benzersiz anahtar kısıtlaması ile yeni bir kapsayıcı oluşturun. Yeni kapsayıcı için mevcut kapsayıcıdan verileri taşımak için uygun veri geçiş aracını kullanın. SQL kapsayıcıları için [veri geçiş aracı](import-data.md) verileri taşımak için. MongoDB kapsayıcıları için [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md) verileri taşımak için.

* En fazla 16 yol değerlerinin bir benzersiz anahtar ilkesi olabilir. Örneğin, değerleri /firstName /lastName ve /address/zipCode olabilir. Her bir benzersiz anahtar ilkesi, en fazla 10 benzersiz anahtar kısıtlamaları veya birleşimleri olabilir. Her benzersiz dizin kısıtlaması birleşik yollarını 60 baytı aşmamalıdır. Önceki örnekte, ad, Soyadı ve e-posta adresi bir kısıtlama birleştirilir. Bu kısıtlama, 3 16 olası yolları kullanır.

* Update ve delete bir öğe kapsayıcı bir benzersiz anahtar ilkesi oluşturmak için istek birimi (RU) ücretleri olduğunda biraz daha yüksek.

* Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yolu eksik değerler benzersizlik kısıtlaması katılmak null değerler olarak kabul. Bu nedenle, yalnızca tek bir öğe bu kısıtlamasını karşılamak için null değerine sahip olabilir.

* Benzersiz anahtar adları büyük/küçük harfe duyarlıdır. Örneğin, bir kapsayıcı için /address/zipcode ayarlamak benzersiz anahtar kısıtlaması ile düşünün. Verilerinizi ZipCode adlı bir alan varsa, "zipcode" "ZipCode." ile aynı olmadığından Azure Cosmos DB "null" olarak benzersiz bir anahtar ekler Bu büyük/küçük harfe duyarlılık nedeniyle, "null" yinelenen benzersiz anahtar kısıtlamasını ihlal ettiğinden ZipCode diğer tüm kayıtları eklenemez.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [mantıksal bölümler](partition-data.md).
