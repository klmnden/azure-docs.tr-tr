---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 0ba379fb14945a59a6a1897118fd21d5f5dee758
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
1. Yeni bir tarayıcı penceresinde [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğine tıklayın.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. **Yeni hesap** sayfasında, yeni Azure Cosmos DB hesabının ayarlarını girin. 
 
    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz bir ad girin*|Bu Azure Cosmos DB hesabını tanımlayan benzersiz bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek ilgili kişi noktanız oluşturulacağından benzersiz ancak tanımlanabilir bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 50 karakterden oluşmalıdır.
    API|Cassandra|API, oluşturulacak hesap türünü belirler. Azure Cosmos DB, uygulamanızın ihtiyaçlarını karşılayacak beş API sunar: Her biri ayrı hesap gerektiren SQL (belge veritabanı), Gremlin (grafik veritabanı), MongoDB (belge veritabanı), Azure Tablosu ve Cassandra. <br><br>Hızlı başlangıçta CQL söz dizimini kullanarak sorgulanabilir bir geniş sütun veritabanı oluşturduğunuzdan **Cassandra**’yı seçin.<br><br>Cassandra (geniş sütun) listenizde görüntülenmiyorsa, Cassandra API önizleme programına [katılmak üzere başvurmanız](../articles/cosmos-db/cassandra-introduction.md#sign-up-now) gerekir.<br><br> [Cassandra API hakkında daha fazla bilgi edinin](../articles/cosmos-db/cassandra-introduction.md)|
    Abonelik|*Aboneliğiniz*|Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|Yeni oluştur<br><br>*Yukarıdaki kimlikte sağlanan benzersiz adın aynısını girin*|**Yeni oluştur**’u seçin ve ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınıza en yakın bölgeyi seçin*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.
    Panoya sabitle | Şunu seçin: | Kolay erişim amacıyla yeni veritabanı hesabınızın portal panonuza eklenmesi için bu kutuyu seçin.

    Sonra **Oluştur**’a tıklayın.

    ![Azure Cosmos DB için yeni hesap sayfası](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. Hesabın oluşturulması birkaç dakika sürer. Portalın **Tebrikler! Azure Cosmos DB hesabınız oluşturuldu** sayfasını görüntülemesini bekleyin.

