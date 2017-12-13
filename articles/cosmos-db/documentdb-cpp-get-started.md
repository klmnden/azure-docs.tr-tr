---
title: "Azure Cosmos DB için C++ öğreticisi | Microsoft Docs"
description: "C++ için Azure Cosmos DB onaylı bir SDK’yı kullanarak bir C++ veritabanı ve konsol uygulaması oluşturan öğretici. Azure Cosmos DB, çok büyük ölçekli bir veritabanı hizmetidir."
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: b9a663387d65ea9413f18599e5cacedd7ed151bf
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-sql-api"></a>Azure Cosmos DB: C++ konsol uygulaması Öğreticisi SQL API'si
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

C++ için SDK Azure Cosmos DB SQL API destekli için C++ Öğreticisine Hoş Geldiniz! Bu öğreticiden yararlandıktan sonra, bir C++ veritabanı dahil olmak üzere Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu hızlı başlangıç kapsar:

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
Lütfen aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Visual Studio 2017](https://www.visualstudio.com/downloads/), C++ dil bileşenlerinin yüklü ile. Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [C++ uygulamanızı kurma](#SetupC++)'ya atlayabilirsiniz.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>2. Adım: C++ uygulamanızı ayarlama
1. Visual Studio’yu açın ve **Dosya** menüsünde **Yeni**’ye, ardından **Proje**’ye tıklayın. 
2. **Yeni Proje** penceresindeki **Yüklü** bölmesinde **Visual C++** seçeneğini genişletin, **Win32**’ye ve ardından **Win32 Konsol Uygulaması**’na tıklayın. Projeyi hellodocumentdb olarak adlandırıp **Tamam**’a tıklayın. 
   
    ![Yeni proje sihirbazının ekran görüntüsü](media/documentdb-cpp-get-started/hello.png)
3. Win32 Uygulama Sihirbazı başlatıldığında **Son**’a tıklayın.
4. Proje oluşturulduktan sonra **Çözüm Gezgini**’nde **hellodocumentdb** projesine sağ tıklayıp **NuGet Paketlerini Yönet**’e tıklayarak NuGet paket yöneticisini açın. 
   
    ![Proje menüsündeki NuGet Paketlerini Yönet seçeneğini gösteren ekran görüntüsü](media/documentdb-cpp-get-started/nuget.png)
5. **NuGet: hellodocumentdb** sekmesinde **Gözat**’a tıklayın ve *documentdbcpp* öğesini aratın. Aşağıdaki ekran görüntüsünde gösterildiği gibi, sonuçlardan DocumentDbCPP’yi seçin. Bu paket, DocumentDbCPP için bir bağımlılık olan C++ REST SDK başvurularını yükler.  
   
    ![DocumentDbCpp paketini vurgulanmış halde gösteren ekran görüntüsü](media/documentdb-cpp-get-started/cpp.png)
   
    Paketler projenize eklendikten sonra biraz kod yazmaya hazırız demektir.   

## <a id="Config"></a>3. Adım: Azure Cosmos DB veritabanınıza yönelik bağlantı ayrıntılarını Azure portaldan kopyalama
[Azure portalını](https://portal.azure.com) açın ve oluşturduğunuz Azure Cosmos DB veritabanı hesabına gidin. C++ kod parçacığımızdan bir bağlantı oluşturmak için bir sonraki adımda Azure portalından alınan URI ve birincil anahtara ihtiyacımız olacak. 

![Azure portalında Azure Cosmos DB URI’si ve anahtarlar](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

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
   
    İstemci başlatmaya yarayacak koda sahip olduğunuza göre Azure Cosmos DB kaynaklarla çalışmak bir bakalım.

## <a id="CreateDBColl"></a>5. Adım: C++ veritabanı ve koleksiyonu oluşturma
Bu adımı gerçekleştirmeden önce, Azure Cosmos DB konusunda acemi olanlar için veritabanı, koleksiyon ve belgelerin nasıl etkileşimde bulunduğundan bahsedelim. [Veritabanı](documentdb-resources.md#databases), koleksiyonlar genelinde bölümlenmiş belge depolama alanının mantıksal bir kapsayıcısıdır. [Koleksiyon](documentdb-resources.md#collections), JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. [Azure Cosmos DB hiyerarşik kaynak modeli ve kavramları](documentdb-resources.md) konusundan Azure Cosmos DB hiyerarşik kaynak modeli ve kavramları hakkında daha fazla bilgi edinebilirsiniz.

Bir veritabanı ve ona karşılık gelen bir koleksiyon oluşturmak için aşağıdaki kodu ana işlevinizin sonuna ekleyin. Bunu yaptığınızda, önceki adımda belirttiğiniz istemci yapılandırması kullanılarak 'FamilyRegistry’ adlı bir veritabanı ve ‘FamilyCollection’ adlı bir koleksiyon oluşturulur.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>6. Adım: Belge oluşturma
[Belgeler](documentdb-resources.md#documents), kullanıcı tanımlı (rastgele) JSON içeriğidir. Artık Azure Cosmos DB'ye bir belge yerleştirebilirsiniz. Aşağıdaki kodu ana işlevin sonuna kopyalayarak bir belge oluşturabilirsiniz. 

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

Özetlemek gerekirse, bu kod bir Azure Cosmos DB veritabanı, koleksiyonu ve belgeleri oluşturur ve bunları Azure portalındaki Belge Gezgini’nde sorgulayabilirsiniz. 

![C++ öğreticisi - Hesap, veritabanı, koleksiyon ve belgeler arasındaki hiyerarşik ilişkiyi gösteren diyagram](media/documentdb-cpp-get-started/docs.png)

## <a id="QueryDB"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](documentdb-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, önceki adımda oluşturduğumuz belgelerde SQL söz dizimi kullanarak gerçekleştirebileceğimiz bir sorguyu gösterir.

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
Farklı Azure Cosmos DB kaynaklarını oluşturmak, sorgulamak, değiştirmek ve silmek için kod ekledik.  Şimdi de bu farklı işlevlere hellodocumentdb.cpp’deki ana işlevimizden çağrıların yanı sıra bazı tanılama iletileri ekleyerek bağlantıları tamamlayalım.

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

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıkışın aşağıdaki görüntüyle eşleşmesi gerekir.

![Azure Cosmos DB C++ uygulama çıktısı](media/documentdb-cpp-get-started/console.png)

Tebrikler! C++ öğreticisini tamamladınız ve ilk Azure Cosmos DB konsol uygulamanızı oluşturdunuz!

## <a id="GetSolution"></a>Tam C++ öğreticisi çözümünü edinin
Bu makaledeki tüm örnekleri içeren GetStarted çözümünü derlemek için aşağıdakilere ihtiyacınız vardır:

* [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [GetStarted](https://github.com/stalker314314/DocumentDBCpp) çözümü.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* [Azure Cosmos DB belgeleri sayfasının](https://azure.microsoft.com/documentation/services/documentdb/) Geliştirme bölümünde programlama modeli hakkında daha fazla bilgi edinin.

[create-account]: create-documentdb-dotnet.md#create-account


