---
title: "Veri Hizmeti Market oluşturmaya Kılavuzu | Microsoft Docs"
description: "Oluşturma, sertifika ve bir veri hizmeti için dağıtma konusunda ayrıntılı yönergeler Azure Market satın alın."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: a853b4dbd1952ba4ea8ee68ea3ca98f588bb71a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Var olan bir web hizmetini CSDL aracılığıyla OData eşleme
> [!IMPORTANT]
> **Şu anda artık ekleme duyuyoruz herhangi yeni veri hizmeti yayımcılar. Yeni dataservices listesine onaylanmamış.** Üzerinde AppSource yayımlamak istediğiniz bir SaaS iş uygulaması varsa daha fazla bilgi bulabilirsiniz [burada](https://appsource.microsoft.com/partners). Bir Iaas uygulamalar varsa veya Azure marketi, yayımlamak istediğiniz Geliştirici hizmet, daha fazla bilgi bulabilirsiniz [burada](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Bu makalede bir CSDL var olan bir hizmeti bir OData uyumlu hizmetine eşlemek için nasıl kullanılacağı hakkında genel bir bakış sağlar. Nasıl bir hizmet aramasıyla istemciden giriş isteğinde dönüştüren eşleme belge (CSDL) oluşturmak için ve çıkış (veri) ile uyumlu bir OData akışı istemciye geri açıklanmaktadır. Microsoft Azure Marketi'nde OData protokolünü kullanarak son kullanıcılara hizmetleri gösterir. (Verileri sahiplerinin) içerik sağlayıcıları tarafından sunulan hizmetler, formlar, REST, SOAP, vb. gibi çeşitli sunulur.

## <a name="what-is-a-csdl-and-its-structure"></a>Bir CSDL ve yapısını nedir?
CSDL (kavramsal şema tanım dili) web hizmeti veya Azure Marketi'nde ortak XML duyuruları veritabanı hizmetinin açıklamak nasıl tanımlayan bir özelliğidir.

Basit genel bakış **istek akışı:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Veri akışı** ters yönde değil:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Şekil 1** diyagramları nasıl bir istemci verilerini bir içerik sağlayıcı (hizmetinizi) Azure Market üzerinden giderek elde edebilirsiniz.  İçerik sağlayıcının hizmetlere ve istekte bulunan istemci arasında veri iletmek ve CSDL isteği işlemek üzere eşleme/dönüştürme bileşen tarafından kullanılır.

*Şekil 1: Azure Market üzerinden içerik sağlayıcı için istekte bulunan istemciden ayrıntılı akış*

  ![Çizim](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Gözden geçirme Atom arka planda için lütfen Atom Pub ve bağlı Azure Marketi uzantıları yapı, OData protokolü: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Bağlantı yukarıdaki alıntı: *"amacı (bundan böyle OData adlandırılır) açık veri protokolü, REST tabanlı bir protokol veri hizmetleri sunulan kaynaklara karşı stili CRUD işlemleri (oluşturma, okuma, güncelleştirme ve silme) sağlamaktır. "Veri hizmeti" bir uç noktadır koleksiyonlardan bir veya daha fazla"" her ", oluşur sıfır veya daha fazla girişlerini" ile kullanıma sunulan veri olduğu adlı-değer çiftleri belirtilmiş. Böylece isteyen herkes derleme sunucuları, istemcileri veya Araçlar telif hakkı veya kısıtlamaları olmadan OData OASIS (terfi yapılandırılmış bilgi standartlar için kuruluş) standartları altında Microsoft tarafından yayımlanan."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Üç kritik CSDL tarafından tanımlanmış olması parçalar şunlardır:
* **Endpoint** , servis sağlayıcı Web adresi (URI) hizmetinin
* **Veri parametreleri** veri türü için içerik sağlayıcının hizmetine gönderilen parametreleri tanımlarını giriş hizmet sağlayıcısı olarak geçirilen.
* **Şema** isteyen hizmete içerik sağlayıcının hizmeti tarafından teslim edilmesini veri ve şema döndürülen veriler kapsayıcı, koleksiyonları/tabloları, değişkenleri/sütunları ve veri türleri dahil.

Aşağıdaki diyagramda burada sonuçlar/verileri geri almak için istemci OData deyimi (içerik sağlayıcının web hizmeti çağrısı) girer gelen akış özetini gösterir.

  ![Çizim](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>adımlar:
1. İstemci isteği hizmeti çağrısı aracılığıyla Azure Marketi XML tanımlanan giriş parametreleri ile tam gönderir
2. CSDL hizmeti çağrısı doğrulamak için kullanılır.
   * Biçimlendirilmiş hizmetini çağırmak için içerik sağlayıcıları hizmeti Azure Marketi tarafından gönderilir
3. Web hizmeti yürütür ve Http Fiili eylem preforms (yani GET) verileri Azure Marketi (varsa) istenen verileri istemciye XML biçiminde kullanıma sunar nerede CSDL tanımlı eşlemesi kullanarak döndürülür.
4. İstemci veriler (varsa) XML veya JSON biçiminde gönderilir

## <a name="definitions"></a>Tanımlar
### <a name="odata-atom-pub"></a>OData ATOM pub
Her girişin bir sonucu olarak bir satır temsil ettiği ATOM pub uzantısı ayarlayın. Giriş içerik parçası satırın – anahtar değer çiftleri olarak değerleri içerecek şekilde geliştirilmiştir. Daha fazla bilgi aşağıda bulunan: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - kavramsal şema tanım dili
İşlevler (SPROCs) tanımlanmasına olanak sağlar ve bir veritabanı yoluyla kullanıma sunulan varlıklar. Burada bulunan daha fazla bilgi: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Tıklatın **diğer sürümleri** açılır ve makale görmüyorsanız, bir sürümü seçin.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - giriş veri modeli
* Genel Bakış: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Önizleme: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Veri türleri: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

Ayrıntılı soldan sağa akış istemci OData deyimi (içerik sağlayıcının web hizmeti çağrısı) burada girer gelen sonuçları/verileri almak için aşağıdaki gösterildiği yedekleyin:

  ![Çizim](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>CSDL temelleri
CSDL (kavramsal şema tanım dili) web hizmeti veya Azure Marketi'nde ortak XML duyuruları veritabanı hizmetinin açıklamak nasıl tanımlayan bir özelliğidir. CSDL kritik açıklar, parça **veri geçirme veri kaynağından Azure Marketi mümkün kılar.** Temel Parçalar aşağıda açıklanmıştır:

* Tüm genel kullanıma açık işlevleri (Functionımport düğümü) açıklayan bilgileri arabirimi
* Veri türü bilgileri tüm ileti requests(input) ve ileti responses(outputs) (EntitySet/EntityContainer/EntityType düğümler)
* Aktarım Protokolü şekilde hakkında bilgi bağlama (üstbilgi düğümü) kullanılır
* Belirtilen hizmet (tabanURI özniteliği) yerini adresi bilgileri

Buna koysalar CSDL hizmet istek sahibi ve hizmet sağlayıcısı arasında bir platform ve dilden bağımsız sözleşme temsil eder. Bir istemci, CSDL'ı kullanarak bir web hizmeti/veritabanı hizmetini bulun ve herhangi bir genel kullanıma açık işlevlerini çağırma.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Bir veritabanı veya bir koleksiyon için bir CSDL ilgili
**CSDL belirtimi**

CSDL açıklayan bir web hizmeti için XML dilbilgisi ' dir. Belirtimi 4 ana öğeye ayrılmıştır: EntitySet, Functionımport; Ad alanı ve EntityType.

Bu soyutlama sağlar anlamak daha kolay hale getirmek için bir tabloya bir CSDL ilgilidir.

Unutmayın;

  CSDL temsil eden bir platform ve dilden bağımsız sözleşme arasında **hizmet istek sahibi** ve **hizmet sağlayıcısı**. CSDL, kullanarak bir **istemci** bulabilir bir **web hizmeti/veritabanı hizmeti** ve kendi genel kullanıma açık hiçbirini çağırma **işlevleri.**

Veri Hizmeti için bir CSDL dört bölümden, veritabanı, tablo, sütun ve deposu yordamı bakımından değerlendirilebilir.

Bu gibi bir veri hizmeti ile ilgili:

* EntityContainer ~ = veritabanı
* EntitySet ~ = tablosu
* EntityType ~ sütunları =
* Functionımport ~ saklı yordam =

**İzin verilen HTTP fiilleri**

* GET-(bir koleksiyon döndürür) DB'den değerler verir
* POST – veri ve isteğe bağlı dönüş değerleri (oluşturma koleksiyonu, dönüş kimliği/URI'si yeni bir giriş) DB'den iletmek için kullanılan
* – (Koleksiyonu siler) veritabanından siler verileri Sil
* PUT – bir Veritabanına veri güncelleştirme (bir koleksiyon değiştirin veya bir tane oluşturun)

## <a name="metadatamapping-document"></a>Meta veri/eşleme belge
Meta veri/eşleme belge, böylece Azure Marketi sistem tarafından bir OData web hizmeti olarak verilebilen bir içerik sağlayıcının varolan web hizmetleri eşlemek için kullanılır. Implements REST ihtiyaçlarını karşılamak için CSDL birkaç uzantıları Azure Market üzerinden kullanıma sunulan web hizmetleri tabanlı ve CSDL üzerinde temel alır. Uzantıları bulunan [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) ad alanı.

CSDL örneği izler: (Kopyala ve Yapıştır örnek CSDL bir XML Düzenleyicisi'ni ve değişiklik hizmetinizi eşleştirmek için aşağıda.  CSDL eşleme DataService sekmesi altında hizmetinizi oluştururken yapıştırın [Azure Marketi'nde yayımlama portalında](https://publish.windowsazure.com)).

**Koşulları:** CSDL ilgili koşulları için [yayımlama portalında](https://publish.windowsazure.com) UI (PPUI) koşulları.

* PPUI "Title" MyWebOffer için ilgili teklif
* PPUI Şirketim ilişkili **yayımcı görünen adı** içinde [Microsoft Developer Center'da](http://dev.windows.com/registration?accountprogram=azure) kullanıcı Arabirimi
* Bir Web veya veri hizmeti (PPUI planında) API'nizi öğesiyle ilişkili

**Hiyerarşi:** tanesine olan tekliflerini sahibinin şirket (içerik sağlayıcısı), yani service(s), bir API ile hangi hizalamak.

### <a name="webservice-csdl-example"></a>Web hizmeti CSDL örneği
Bir web uygulaması uç noktası (örneğin, C# uygulaması) gösterme hizmetine bağlanır

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Daha fazla CSDL Web hizmeti örnekleri makaleyi görüntülemek [var olan eşleme örnekleri web CSDLs aracılığıyla OData hizmeti](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>DataService CSDL örneği
Temel veriler için iki API'leri API (tablolar yerine görünümleri kullanabilirsiniz) CSDL dayalı bir uç nokta örnek aşağıda gösterildiği gibi bir veritabanı tablosu veya görünümü gösterme bir hizmete bağlanır.

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Ayrıca Bkz.
* Öğrenme ve belirli düğümler ve bunların parametrelerini anlama ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme düğümleri](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) tanımları ve açıklamalar, örnekler ve kullanım örneği bağlamı.
* Örnekler gözden geçirme ilgileniyorsanız, bu makaleyi okuyun [veri hizmeti OData eşleme örnekler](marketplace-publishing-data-service-creation-odata-mapping-examples.md) örnek kodu görmek ve kod sözdizimi ve bağlamı anlamak için.
* Veri Hizmeti Azure Marketinde yayımlama için belirtilen yol dönmek için bu makaleyi okuyun [veri hizmeti yayımlama Kılavuzu](marketplace-publishing-data-service-creation.md).

