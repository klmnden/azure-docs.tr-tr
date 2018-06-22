---
title: Azure Cosmos DB için C++ öğreticisi | Microsoft Docs
description: C++ için Azure Cosmos DB onaylı bir SDK’yı kullanarak bir C++ veritabanı ve konsol uygulaması oluşturan öğretici. Azure Cosmos DB, çok büyük ölçekli bir veritabanı hizmetidir.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: cpp
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: sngun
ms.openlocfilehash: 0e142eaf4182331e0a5803c54d2cc1284e21b221
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807184"
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-sql-api"></a>Azure Cosmos DB: SQL API’si için C++ konsol uygulaması öğreticisi
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
> 

C++ için Azure Cosmos DB SQL API’si onaylı SDK için C++ öğreticisine hoş geldiniz! Bu öğreticiden yararlandıktan sonra, bir C++ veritabanı dahil olmak üzere Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu hızlı başlangıç aşağıdakileri kapsar:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Uygulamanızı kurma
* C++ Azure Cosmos DB veritabanı oluşturma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* Bir belgeyi değiştirme
* Bir belgeyi silme
* C++ Azure Cosmos DB veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/stalker314314/DocumentDBCpp)'da bulabilirsiniz. Hızlı yönergeler için bkz. [Eksiksiz çözüm edinme](#GetSolution).

Şimdi başlayalım!

## <a name="prerequisites-for-the-c-tutorial"></a>C++ öğreticisi için önkoşullar
Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* C++ dil bileşenleri yüklü [Visual Studio 2017](https://www.visualstudio.com/downloads/). Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [C++ uygulamanızı kurma](#SetupC++)'ya atlayabilirsiniz.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>2. Adım: C++ uygulamanızı ayarlama
1. Visual Studio’yu açın ve **Dosya** menüsünde **Yeni**’ye, ardından **Proje**’ye tıklayın. 
2. **Yeni Proje** penceresindeki **Yüklü** bölmesinde **Visual C++** seçeneğini genişletin, **Win32**’ye ve ardından **Win32 Konsol Uygulaması**’na tıklayın. Projeyi hellodocumentdb olarak adlandırıp **Tamam**’a tıklayın. 
   
    ![Yeni proje sihirbazının ekran görüntüsü](media/sql-api-cpp-get-started/hello.png)
3. Win32 Uygulama Sihirbazı başlatıldığında **Son**’a tıklayın.
4. Proje oluşturulduktan sonra **Çözüm Gezgini**’nde **hellodocumentdb** projesine sağ tıklayıp **NuGet Paketlerini Yönet**’e tıklayarak NuGet paket yöneticisini açın. 
   
    ![Proje menüsündeki NuGet Paketlerini Yönet seçeneğini gösteren ekran görüntüsü](media/sql-api-cpp-get-started/nuget.png)
5. **NuGet: hellodocumentdb** sekmesinde **Gözat**’a tıklayın ve *documentdbcpp* öğesini aratın. Aşağıdaki ekran görüntüsünde gösterildiği gibi, sonuçlardan DocumentDbCPP’yi seçin:   
   
    ![DocumentDbCpp paketini vurgulanmış halde gösteren ekran görüntüsü](media/sql-api-cpp-get-started/cpp.png)
   
    Bu paket, DocumentDbCPP için bir bağımlılık olan C++ REST SDK başvurularını yükler. Paketler projenize eklendikten sonra kod yazmaya hazırsınız demektir.   

## <a id="Config"></a>3. Adım: Azure Cosmos DB veritabanınıza yönelik bağlantı ayrıntılarını Azure portaldan kopyalama
[Azure portalını](https://portal.azure.com) açın ve oluşturduğunuz Azure Cosmos DB hesabına gidin. C++ kod parçacığından bir bağlantı oluşturmak için bir sonraki adımda Azure portalından alınan URI ve birincil anahtara ihtiyacınız olacak. 

![Azure portalında Azure Cosmos DB URI’si ve anahtarlar](media/sql-api-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>4. Adım: Azure Cosmos DB hesabına bağlanma
1. Aşağıdaki üst bilgileri ve ad alanlarını kaynak kodunuza `#include "stdafx.h"` ifadesinden sonra gelecek şekilde ekleyin.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Ardından, aşağıdaki kodu ana işlevinize ekleyin ve hesap yapılandırması ile birincil anahtarı 3. adımdaki Azure Cosmos DB ayarlarınızla eşleşecek şekilde değiştirin. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Artık istemciyi başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya göz atalım.

## <a id="CreateDBColl"></a>5. Adım: C++ veritabanı ve koleksiyonu oluşturma
Bu adımı gerçekleştirmeden önce, Azure Cosmos DB konusunda acemi olanlar için veritabanı, koleksiyon ve belgelerin nasıl etkileşimde bulunduğundan bahsedelim. [Veritabanı](sql-api-resources.md#databases), koleksiyonlar genelinde bölümlenmiş belge depolama alanının mantıksal bir kapsayıcısıdır. [Koleksiyon](sql-api-resources.md#collections), JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. [Azure Cosmos DB hiyerarşik kaynak modeli ve kavramları](sql-api-resources.md) konusundan Azure Cosmos DB hiyerarşik kaynak modeli ve kavramları hakkında daha fazla bilgi edinebilirsiniz.

Bir veritabanı ve ona karşılık gelen bir koleksiyon oluşturmak için aşağıdaki kodu ana işlevinizin sonuna ekleyin. Bunu yaptığınızda, önceki adımda belirttiğiniz istemci yapılandırması kullanılarak 'FamilyRegistry’ adlı bir veritabanı ve ‘FamilyCollection’ adlı bir koleksiyon oluşturulur.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>6. Adım: Belge oluşturma
[Belgeler](sql-api-resources.md#documents), kullanıcı tanımlı (rastgele) JSON içeriğidir. Artık Azure Cosmos DB'ye bir belge yerleştirebilirsiniz. Aşağıdaki kodu ana işlevin sonuna kopyalayarak bir belge oluşturabilirsiniz. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

Özetlemek gerekirse, bu kod bir Azure Cosmos DB veritabanı, koleksiyonu ve belgeleri oluşturur ve bunları Azure portalındaki Veri Gezgini’nde sorgulayabilirsiniz. 

![C++ öğreticisi - Hesap, veritabanı, koleksiyon ve belgeler arasındaki hiyerarşik ilişkiyi gösteren diyagram](media/sql-api-cpp-get-started/docs.png)

## <a id="QueryDB"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, önceki adımda oluşturduğumuz belgelerde SQL söz dizimi kullanarak gerçekleştirebileceğiniz bir sorguyu gösterir.

Bu işlev, veritabanı ve koleksiyonun yanı sıra belge istemcisinin benzersiz tanımlayıcısı ve kaynak kimliğini bağımsız değişkenler olarak alır. Bu kodu ana işlevden önce ekleyin.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>8. Adım: Bir belgeyi değiştirme
Azure Cosmos DB, aşağıdaki kodda gösterildiği gibi JSON belgelerinin değiştirilmesini destekler. Bu kodu executesimplequery işlevinden sonra ekleyin.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>9. Adım: Bir belgeyi silme
Azure Cosmos DB JSON belgelerinin silinmesini destekler; aşağıdaki kodu kopyalayıp replacedocument işlevinden sonra yapıştırarak bunu gerçekleştirebilirsiniz. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>10. Adım: Bir veritabanını silme
Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (koleksiyonlar, belgeler vb.) kaldırılır.

Veritabanını ve tüm alt kaynaklarını kaldırmak için aşağıdaki kod parçacığını (cleanup işlevi) kopyalayıp deletedocument işlevinden sonra yapıştırın.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>11. Adım: C++ uygulamanızı hep birlikte çalıştırın!
Şimdi farklı Azure Cosmos DB kaynaklarını oluşturmak, sorgulamak, değiştirmek ve silmek için kod eklediniz.  Şimdi de bu farklı işlevlere hellodocumentdb.cpp’deki ana işlevden çağrıların yanı sıra bazı tanılama iletileri de ekleyerek bağlantıları tamamlayın.

Bunu, uygulamanızın ana işlevini aşağıdaki kodla değiştirerek gerçekleştirebilirsiniz. Bu işlem 3. adımda koda kopyaladığınız account_configuration_uri ve primary_key değerlerinin üzerine yazacağından, bu satırı kaydedin veya değerleri yeniden portaldan kopyalayın. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

Artık F5’e basarak veya alternatif olarak terminal penceresinden uygulamayı bulup yürütülebilir dosyayı çalıştırarak Visual Studio’da kendi kodunuzu derleyip çalıştırabilmeniz gerekiyor. 

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı aşağıdaki ekran görüntüsüyle aynı olmalıdır:

![Azure Cosmos DB C++ uygulama çıktısı](media/sql-api-cpp-get-started/console.png)

Tebrikler! C++ öğreticisini tamamladınız ve ilk Azure Cosmos DB konsol uygulamanızı oluşturdunuz!

## <a id="GetSolution"></a>Tam C++ öğreticisi çözümünü edinin
Bu makaledeki tüm örnekleri içeren GetStarted çözümünü derlemek için aşağıdakilere ihtiyacınız vardır:

* [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [GetStarted](https://github.com/stalker314314/DocumentDBCpp) çözümü.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)’daki bir örnek veri kümesinde sorgular çalıştırın.
* [Azure Cosmos DB belgeleri sayfasının](https://azure.microsoft.com/documentation/services/cosmos-db/) Geliştirme bölümünde programlama modeli hakkında daha fazla bilgi edinin.

[create-account]: create-sql-api-dotnet.md#create-account


