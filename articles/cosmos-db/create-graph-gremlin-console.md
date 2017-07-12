---
title: "Azure Cosmos DB öğreticisi: Apache TinkerPop tarafından kullanıma sunulan Gremlin Console&quot;da oluşturma, sorgulama ve çapraz geçiş yapma | Microsoft Docs"
description: "Azure Cosmos DB Graph API’sini kullanarak köşe, kenar ve sorgu oluşturmaya yönelik Azure Cosmos DB hızlı başlangıcı."
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 06/10/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: 44972270a13f5ab5b3aa22557b36e80ae406a4a6
ms.contentlocale: tr-tr
ms.lasthandoff: 06/13/2017

---
<a id="azure-cosmos-db-create-query-and-traverse-a-graph-in-the-gremlin-console" class="xliff"></a>

# Azure Cosmos DB: Gremlin konsolunda oluşturma, sorgulama ve çapraz geçiş yapma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta Azure portalını kullanarak bir Azure Cosmos DB hesabı, veritabanı ve grafik (kapsayıcı) oluşturma işlemi ve [Apache TinkerPop](http://tinkerpop.apache.org) tarafından kullanıma sunulan [Gremlin Console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console)'u kullanarak Grafik API'si (önizleme) verileri ile çalışma işlemi anlatılmaktadır. Bu öğreticide köşe ve kenarlar oluşturup sorgulayacak, bir köşe özelliğini güncelleştirecek, köşeleri sorgulayacak, grafiğin çapraz geçişini yapacak ve bir köşeyi bırakacaksınız.

![Apache Gremlin konsolunda Azure Cosmos DB](./media/create-graph-gremlin-console/gremlin-console.png)

Gremlin konsolu, Groovy/Java tabanlıdır ve Linux, Mac ve Windows üzerinde çalışır. Konsolu [Apache TinkerPop sitesinden](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.4/apache-tinkerpop-gremlin-console-3.2.4-bin.zip) indirebilirsiniz.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıca yönelik bir Azure Cosmos DB hesabı oluşturmak için Azure aboneliğinizin olması gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Ayrıca [Gremlin konsolunu](http://tinkerpop.apache.org/) yüklemeniz gerekir. 3.2.4 veya daha yüksek bir sürüm kullanın.

<a id="create-a-database-account" class="xliff"></a>

## Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

<a id="add-a-graph" class="xliff"></a>

## Grafik ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a id="ConnectAppService"></a>Uygulama hizmetinize bağlanma
1. Gremlin Konsolu’nu başlatmadan önce *apache-tinkerpop-gremlin-console-3.2.4/conf* dizininde *remote-secure.yaml* yapılandırma dosyasını oluşturun veya değiştirin.
2. *ana bilgisayar*, *bağlantı noktası*, *kullanıcı adı*, *parola*, *bağlantı havuzu* ve *serileştirici* değerlerini girin:

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Ana bilgisayarlar|***.graphs.azure.com|Grafik hizmeti URI'si değeriniz, Azure portalından alabilirsiniz
    Bağlantı noktası|443|443 olarak ayarlayın
    Kullanıcı adı|*Kullanıcı adınız*|`/dbs/<db>/colls/<coll>` formunun kaynağı.
    Parola|*Birincil ana anahtarınız*|Azure Cosmos DB için birincil ana anahtarınız
    Bağlantı havuzu|{enableSsl: true}|SSL için bağlantı havuzu ayarınız
    Serileştirici|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Bu değere ayarlayın

3. [Gremlin Konsolu](http://tinkerpop.apache.org/docs/3.2.4/tutorials/getting-started/)’nu başlatmak için terminalinizde *bin/gremlin.bat* veya *bin/gremlin.sh* komutunu çalıştırın.
4. Uygulama hizmetinize bağlanmak için terminalinizde *:remote connect tinkerpop.server conf/remote-secure.yaml* komutunu çalıştırın.

Harika! Kurulumu tamamladığımıza göre, bazı konsol komutlarını çalıştırmaya başlayalım.

Basit bir count() komutunu deneyelim. İstendiğinde konsola aşağıdakileri yazın:
```
:> g.V().count()
```

> [!TIP]
> ***:>*** öğesinin g.V().count() text? öğesinden önce yazıldığına dikkat edin 
>
> Bu yazmanız gereken komutun bir parçasıdır. Gremlin Console ile Azure Cosmos DB birlikte kullanıldığında bu kısım önemlidir.  
>
> Bu :> ön ekini attığınızda konsola, komutu yerel olarak (genellikle bellek içi bir grafikte) yürütmesini söylemiş olursunuz.
> ***:>*** ön ekini kullandığınızda, bu örnekte Cosmos DB'de (localhost öykünücüsü veya bir > Azure örneği) olmak üzere, konsola uzaktan komut yürütmesini söylemiş olursunuz.


<a id="create-vertices-and-edges" class="xliff"></a>

## Köşe ve kenar oluşturma

İlk olarak *Thomas*, *Mary Kay*, *Robin*, *Ben* ve *Jack* için beş kişi köşesi ekleyelim.

Giriş (Thomas):

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44).property('userid', 1)
```

Çıktı:

```
==>[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d,label:person,type:vertex,properties:[firstName:[[id:f02a749f-b67c-4016-850e-910242d68953,value:Thomas]],lastName:[[id:f5fa3126-8818-4fda-88b0-9bb55145ce5c,value:Andersen]],age:[[id:f6390f9c-e563-433e-acbf-25627628016e,value:44]],userid:[[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d|userid,value:1]]]]
```
Giriş (Mary Kay):

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39).property('userid', 2)

```

Çıktı:

```
==>[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e,label:person,type:vertex,properties:[firstName:[[id:ea0604f8-14ee-4513-a48a-1734a1f28dc0,value:Mary Kay]],lastName:[[id:86d3bba5-fd60-4856-9396-c195ef7d7f4b,value:Andersen]],age:[[id:bc81b78d-30c4-4e03-8f40-50f72eb5f6da,value:39]],userid:[[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e|userid,value:2]]]]

```

Giriş (Robin):

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield').property('userid', 3)
```

Çıktı:

```
==>[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e,label:person,type:vertex,properties:[firstName:[[id:ec65f078-7a43-4cbe-bc06-e50f2640dc4e,value:Robin]],lastName:[[id:a3937d07-0e88-45d3-a442-26fcdfb042ce,value:Wakefield]],userid:[[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e|userid,value:3]]]]
```

Giriş (Ben):

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller').property('userid', 4)

```

Çıktı:

```
==>[id:ee86b670-4d24-4966-9a39-30529284b66f,label:person,type:vertex,properties:[firstName:[[id:a632469b-30fc-4157-840c-b80260871e9a,value:Ben]],lastName:[[id:4a08d307-0719-47c6-84ae-1b0b06630928,value:Miller]],userid:[[id:ee86b670-4d24-4966-9a39-30529284b66f|userid,value:4]]]]
```

Giriş (Jack):

```
:> g.addV('person').property('firstName', 'Jack').property('lastName', 'Connor').property('userid', 5)
```

Çıktı:

```
==>[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469,label:person,type:vertex,properties:[firstName:[[id:4250824e-4b72-417f-af98-8034aa15559f,value:Jack]],lastName:[[id:44c1d5e1-a831-480a-bf94-5167d133549e,value:Connor]],userid:[[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469|userid,value:5]]]]
```


Ardından, kişilerimiz arasındaki ilişkiler için kenarlar ekleyelim.

Giriş (Thomas -> Mary Kay):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

Çıktı:

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Giriş (Thomas -> Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

Çıktı:

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Giriş (Robin -> Ben):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

Çıktı:

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

<a id="update-a-vertex" class="xliff"></a>

## Köşe güncelleştirme

*Thomas* köşesini yeni yaşı olan *45* ile güncelleştirelim.

Giriş:
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
Çıktı:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

<a id="query-your-graph" class="xliff"></a>

## Grafiğinizi sorgulama

Şimdi de grafiğinize karşı çeşitli sorgular çalıştıralım.

İlk olarak, yalnızca 40 yaşından büyük kişileri döndürecek bir filtre sorgulamayı deneyelim.

Giriş (filtre sorgusu):

```
:> g.V().hasLabel('person').has('age', gt(40))
```

Çıktı:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

Ardından, 40 yaşından büyük kişiler için ad planlaması yapalım.

Giriş (filtre + planlama sorgusu):

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

Çıktı:

```
==>Thomas
```

<a id="traverse-your-graph" class="xliff"></a>

## Grafiğinizi çapraz geçirme

Şimdi grafiği Thomas'ın tüm arkadaşlarını döndürecek şekilde geçirelim.

Giriş (Thomas’ın arkadaşları):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

Çıktı: 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

Ardından, sonraki köşe katmanlarını alalım. Thomas'ın arkadaşlarının tüm arkadaşlarını almak üzere grafiği çapraz geçirin.

Giriş (Thomas’ın arkadaşlarının arkadaşları):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
Çıktı:

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

<a id="drop-a-vertex" class="xliff"></a>

## Köşe bırakma

Şimdi de grafik veritabanından bir köşeyi silelim.

Giriş (Jack köşesini bırakın):

```
:> g.V().hasLabel('person').has('firstName', 'Jack').drop()
```

<a id="clear-your-graph" class="xliff"></a>

## Grafiğinizi temizleme

Son olarak, veritabanındaki tüm köşe ve kenarları temizleyelim.

Giriş:

```
:> g.E().drop()
:> g.V().drop()
```

Tebrikler! Bu Azure Cosmos DB: Grafik API’si öğreticisini tamamladınız!

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:  

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Bu hızlı başlangıçta bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini’ni kullanarak grafik oluşturmayı, köşe ve kenar oluşturmayı ve Gremlin konsolunu kullanarak grafiğinizi çapraz geçirmeyi öğrendiniz. Artık daha karmaşık sorgular derleyebilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)

