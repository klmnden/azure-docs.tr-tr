---
title: "Örnek: Alma başlatıldı - bilgi keşfetme hizmeti API'si"
titlesuffix: Azure Cognitive Services
description: Akademik yayınlar arasında etkileşimli bir arama deneyimi altyapısı oluşturmak için Bilgi Keşfetme Hizmeti'ni (KES) kullanın.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: sample
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 00c5ed3e3ea5c083f727d06c2ed305fe35ed03db
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523333"
---
# <a name="get-started-with-the-knowledge-exploration-service"></a>Bilgi Keşfetme Hizmeti'ni kullanmaya başlama

Bu yol gösteren kılavuzda, Bilgi Keşfetme Hizmeti'ni (KES) kullanarak akademik yayınlar arasında etkileşimli bir arama deneyimi altyapısı oluşturuyorsunuz. [`kes.exe`](CommandLine.md) komut satırı aracını ve tüm örnek dosyaları [Bilgi Keşfetme Hizmeti SDK'sından](https://www.microsoft.com/en-us/download/details.aspx?id=51488) yükleyebilirsiniz.

Akademik yayınlar örneğinde, Microsoft'taki araştırmacıların yayımladığı 1000 tane örnek akademik çalışma vardır.  Her çalışma bir başlık, yayın tarihi, yazarlar ve anahtar sözcüklerle ilişkilendirilmiştir. Her yazar bir kimlik, ad ve yayın tarihindeki ilişki durumuyla gösterilir. Her anahtar sözcük bir dizi eş anlamlı sözcükleri ilişkilendirilebilir (örneğin, "destek vektör makinesi" anahtar sözcüğü "svm" eş anlamlısıyla ilişkilendirilebilir).

## <a name="define-the-schema"></a>Şemayı tanımlama

Şema, etki alanındaki nesnelerin öznitelik yapısını açıklar. JSON dosya biçimindeki her özniteliğin adını ve veri türünü belirtir. Aşağıdaki örnek, *Academic.schema* dosyasının içeriğidir.

```json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Burada, *Title*, *Year* ve *Keyword* özniteliklerini sırasıyla dize, tamsayı ve dize özniteliği olarak tanımlarız. Yazarlar kimliği, adı ve bağlantı tarafından temsil edilir çünkü tanımladığınız *Yazar* üç alt özniteliklere sahip bir bileşik özniteliği olarak: *Author.Id*, *yazar.adi*, ve *Author.Affiliation*.

Varsayılan olarak, öznitelikler *equals*, *starts_with* ve *is_between* de dahil olmak üzere kendi veri türünde kullanılabilen tüm işlemleri destekler. Yazar kimliği yalnızca dahili olarak bir tanımlayıcı olarak kullanıldığından, varsayılan değeri geçersiz kılın ve dizini oluşturulan tek işlem olarak *equals* işlemini belirtin.

*Keyword* özniteliği için, öznitelik tanımında *Keyword.syn* eş anlamlılar dosyasını belirterek eş anlamlıların kurallı anahtar sözcük değerleriyle eşleştirilmesine izin verin. Bu dosya kurallı ve eş anlamlı değer çiftlerinin listesini içerir:

```json
...
["support vector machine","support vector machines"]
["support vector machine","support vector networks"]
["support vector machine","support vector regression"]
["support vector machine","support vector"]
["support vector machine","svm machine learning"]
["support vector machine","svm"]
["support vector machine","svms"]
["support vector machine","vector machine"]
...
```

Şema tanımı hakkında daha fazla bilgi için bkz. [Şema Biçimi](SchemaFormat.md).

## <a name="generate-data"></a>Verileri oluşturma

Veri dosyası, dizini oluşturulacak yayınlar listesini açıklar. Her satırda çalışmanın öznitelik değerleri [JSON biçiminde](https://json.org/) belirtilir.  Aşağıdaki örnek, *Academic.data* veri dosyasından okunabilir olması için biçimlendirilmiş tek bir satırdır:

```
...
{
    "logprob": -16.714,
    "Title": "the world wide telescope",
    "Year": 2001,
    "Author": [
        {
            "Id": 717694024,
            "Name": "alexander s szalay",
            "Affiliation": "microsoft"
        },
        {
            "Id": 2131537204,
            "Name": "jim gray",
            "Affiliation": "microsoft"
        }
    ]
}
...
```

Bu kod parçacığında, çalışmanın *Title* ve *Year* özniteliklerini sırasıyla JSON dizesi ve sayı olarak belirtirsiniz. JSON dizileri kullanılarak birden çok değer gösterilir. *Author* bir bileşik öznitelik olduğundan, her değer bu bileşik özniteliğin alt özniteliklerinden oluşturulmuş bir JSON nesnesi kullanılarak gösterilir. Bu örnekteki *Keyword* gibi değerleri eksik olan öznitelikler, JSON gösteriminin dışında tutulabilir.

Farklı çalışmaların benzerliğini ayırt etmek için, yerleşik *logprob* özniteliğini kullanarak göreli logaritmik olasılığı belirtin. 0 ile 1 arasında bir *p* olasılık değeri verildiğinde, logaritmik olasılığı log(*p*) olarak hesaplarsınız; burada log(), doğal logaritma işlevidir.

Daha fazla bilgi için bkz. [Veri Biçimi](DataFormat.md).

## <a name="build-a-compressed-binary-index"></a>Sıkıştırılmış ikili dizin oluşturma

Bir şema dosyanız ve veri dosyanız olduktan sonra, [`kes.exe build_index`](CommandLine.md#build_index-command) kullanarak veri nesnelerinin sıkıştırılmış ikili dizinini oluşturabilirsiniz. Bu örnekte, *Academic.schema* giriş şema dosyasından ve *Academic.data* veri dosyasından *Academic.index* dizin dosyasını oluşturuyorsunuz. Aşağıdaki komutu kullanın:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Azure dışında hızla bir prototip oluşturmak için, [`kes.exe build_index`](CommandLine.md#build_index-command) en çok 10.000 nesne içeren veri dosyalarından yerel olarak küçük dizinler oluşturabilir. Daha büyük veri dosyaları için, komutu [Azure'da Windows VM'sinin](../../../articles/virtual-machines/windows/quick-create-portal.md) içinden çalıştırabilir veya Azure'da uzaktan derleme yapabilirsiniz. Ölçeklendirme yukarı Ayrıntılar için bkz.

## <a name="use-an-xml-grammar-specification"></a>XML dil bilgisi belirtimi kullanma

Dil bilgisi, hem hizmetin yorumlayabildiği bir dizi doğal dil sorgusu hem de bu doğal dil sorgularının anlam sorgusu ifadelerine nasıl çevrildiğini belirtir. Bu örnekte, *academic.xml* dosyasında belirtilen dil bilgisini kullanırsınız:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="Academic.schema" name="academic"/>

  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>

    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>

    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>

      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>

        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>

        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>

  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="lt" name="year"/>
          </item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="gt" name="year"/>
          </item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

Dil bilgisi belirtimi söz dizimi hakkında daha fazla bilgi için, bkz. [Dil Bilgisi Biçimi](GrammarFormat.md).

## <a name="compile-the-grammar"></a>Dil bilgisini derleme

Bir XML söz dizimi belirtiminiz olduktan sonra, [`kes.exe build_grammar`](CommandLine.md#build_grammar-command) kullanarak bunu bir ikili dil bilgisine derleyebilirsiniz. Dil bilgisinin bir şemayı içeri aktarması durumunda, şema dosyasının dil bilgisi XML dosyasıyla aynı yolda bulunması gerektiğini unutmayın. Bu örnekte, *Academic.xml* giriş XML dil bilgisi dosyasından *Academic.grammar* ikili dil bilgisi dosyasını derliyorsunuz. Aşağıdaki komutu kullanın:

`kes.exe build_grammar Academic.xml Academic.grammar`

## <a name="host-the-grammar-and-index-in-a-web-service"></a>Dil bilgisini ve dizini web hizmetinde barındırma

Hızlı bir prototip için, [`kes.exe host_service`](CommandLine.md#host_service-command) kullanarak dil bilgisini ve dizini yerel makinedeki bir web hizmetinde barındırabilirsiniz. Ardından veri doğruluğunu ve dil bilgisi tasarımını doğrulamak için [web API'leri](WebAPI.md) yoluyla hizmete erişebilirsiniz. Bu örnekte, *Academic.grammar* dil bilgisi dosyasını ve *Academic.index* dizin dosyasını `http://localhost:8000/` konumunda barındırıyorsunuz. Aşağıdaki komutu kullanın:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Bu, web hizmetinin yerel bir örneğini başlatır. Tarayıcıdan `http::localhost:<port>` adresini ziyaret ederek hizmeti etkileşimli olarak test edebilirsiniz. Hizmeti test etmek daha fazla bilgi için bkz.

Doğal dil yorumunu, sorgu tamamlamayı, yapılandırılmış sorgu gelişimini ve histogram hesaplamasını test etmek için çeşitli [web API'lerini](WebAPI.md) doğrudan çağırmanız da mümkündür. Hizmeti durdurmak için, `kes.exe host_service` komut istemine "quit" girin veya Ctrl+C tuşlarına basın. İşte bazı örnekler:

* [http://localhost:8000/interpret?query=papers by susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers by susan t d&complete=1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais')&attributes=Title,Year,Author.Name,Author.Id&count=2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'),Year>=2013)&attributes=Year,Keyword&count=4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Azure dışında, [`kes.exe host_service`](CommandLine.md#host_service-command) en çok 10.000 nesnenin dizinleriyle sınırlıdır. Diğer sınırlar saniyede 10 istek olan API hızı ve işlem otomatik olarak sonlandırılmadan önce toplam 1000 istek sınırıdır. Bu kısıtlamaları aşmak için, komutu [Azure'da Windows VM'si](../../../articles/virtual-machines/windows/quick-create-portal.md) içinden çalıştırın veya [`kes.exe deploy_service`](CommandLine.md#deploy_service-command) komutunu kullanarak Azure bulut hizmetine dağıtın. Ayrıntılar için bkz: hizmet dağıtma.

## <a name="scale-up-to-host-larger-indices"></a>Daha büyük dizinleri barındırmak için ölçeklendirme

`kes.exe` dosyasını Azure'un dışında çalıştırdığınızda, dizin 10.000 nesneyle sınırlıdır. Azure kullanarak daha büyük dizinler oluşturabilir ve barındırabilirsiniz. [Ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/) için kaydolun. Alternatif olarak, Visual Studio veya MSDN'ye abone olursanız [abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Bunlar her ay bir miktar Azure kredisi sunar.

Azure hesabına `kes.exe` erişimi izni vermek için, Azure portalından [Azure Yayımlama Ayarları dosyasını indirin](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade). İstenirse, dilediğiniz Azure hesabıyla oturum açın. Dosyayı *AzurePublishSettings.xml* adıyla `kes.exe` dosyasının çalıştırıldığı çalışma dizinine kaydedin.

Büyük dizinleri oluşturmanın ve barındırmanın iki yolu vardır. İlk yol, şema ve veri dosyalarını Azure'da Windows VM'sinde hazırlayın. Ardından çalıştırın `kes.exe build_index` dizin yerel olarak herhangi bir boyut kısıtlama olmadan bir VM oluşturmak için. Sonuçta elde edilen dizin yerel olarak VM'de kullanarak barındırılabilir `kes.exe host_service` için herhangi bir kısıtlama olmadan yeniden hızlı prototip oluşturma. Ayrıntılı adımlar için bkz. [Azure VM öğreticisi](../../../articles/virtual-machines/windows/quick-create-portal.md).

İkinci yöntem, [`kes.exe build_index`](CommandLine.md#build_index-command) komutunu `--remote` parametresiyle kullanarak uzaktan Azure derlemesi yapmaktır. Bu, Azure VM boyutunu belirtir. `--remote` parametresi belirtildiğinde, komut bu boyutta geçici bir Azure VM oluşturur. Ardından VM'de dizini oluşturur, dizini hedef blob depolamaya yükler ve işlem tamamlandıktan sonra VM'yi siler. Dizin oluşturulurken Azure aboneliğiniz VM'nin maliyeti tutarında ücretlendirilir.

Bu uzaktan derleme özelliği [`kes.exe build_index`](CommandLine.md#build_index-command) komutunun herhangi bir ortamda çalıştırılmasına olanak tanır. Uzaktan derleme yaparken, giriş şema ve veri bağımsız değişkenleri yerel dosya yolları veya [Azure blob depolama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) URL'leri olabilir. Çıkış dizini bağımsız değişkeni blob depolama URL'si olmalıdır. Azure depolama hesabı oluşturmak için bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md). Dosyaları blob depolamasına ve blob depolamasından verimli bir yolla kopyalamak için, [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programını kullanın.

Bu örnekte, şu blob depolama kapsayıcısının oluşturulduğunu varsayabilirsiniz: http://&lt;*hesap*&gt;.blob.core.windows.net/&lt;*kapsayıcı*&gt;/. Bu kapsayıcı *Academic.schema* şemasını, başvurulan *Keywords.syn* eş anlamlılar dosyasını ve tam ölçekli *Academic.full.data* veri dosyasını barındırır. Aşağıdaki komutu kullanarak tam dizini uzaktan oluşturabilirsiniz:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Dizini oluşturmak için geçici bir VM sağlama için 5-10 dakika sürebilir unutmayın. Hızlı bir prototip oluşturmak için şunları yapabilirsiniz:
- Herhangi bir makinede yerel olarak daha küçük bir veri kümesiyle geliştirme.
- El ile [bir Azure VM oluşturma](../../../articles/virtual-machines/windows/quick-create-portal.md), Uzak Masaüstü yoluyla [bu VM'ye bağlanma](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), [Bilgi Keşfetme Hizmeti SDK'sını](https://www.microsoft.com/en-us/download/details.aspx?id=51488) yükleme ve VM'nin içinden [`kes.exe`](CommandLine.md) komutunu çalıştırma.

Derleme işlemi sırasında sayfalama yavaşlar. Sayfalamayı önlemek için, dizin oluşturmanın giriş veri dosyası olarak RAM miktarının üç katı kadar bir VM kullanın. Barındırma için dizin boyutundan 1 GB daha fazla RAM içeren bir VM kullanın. Kullanılabilir VM boyutları listesi için bkz. [Sanal makinelerin boyutları](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

## <a name="deploy-the-service"></a>Hizmeti dağıtma

Dil bilginiz ve dizininiz olduktan sonra, artık hizmeti bir Azure bulut hizmetine dağıtmaya hazırsınız demektir. Yeni bir Azure bulut hizmeti oluşturmak için bkz. [Bulut hizmetini oluşturma ve dağıtma](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Bu noktada bir dağıtım paketi belirtmeyin.  

Bulut hizmetini oluşturduğunuzda, hizmeti dağıtmak için [`kes.exe deploy_service`](CommandLine.md#deploy_service-command) kullanabilirsiniz. Azure bulut hizmetinin iki dağıtım yuvası vardır: üretim ve hazırlama. Canlı kullanıcı trafiği alan bir hizmet için, başlangıçta bir hazırlama yuvasına dağıtım yapmanız gerekir. Hizmetin kendini başlatmasını bekleyin. Ardından, dağıtımı doğrulamak için birkaç istek gönderebilir ve temel testleri geçtiğini doğrulayabilirsiniz.

Hazırlama yuvasını içeriğini üretim yuvasıyla [değiştirin](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md); böylelikle, canlı trafik artık yeni dağıtılan hizmete yönlendirilir. Hizmetin yeni verilerle güncelleştirilmiş sürümünü dağıtırken de bu işlemi yineleyebilirsiniz. Aynı diğer Azure bulut hizmetlerinde olduğu gibi, isterseniz Azure portalını kullanarak [otomatik ölçeklendirmeyi](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md) yapılandırabilirsiniz.

Bu örnekte, dağıttığınız *akademik* dizin ile bir bulut hizmetinin hazırlık yuvasına  *\<vm_size >* VM'ler. Aşağıdaki komutu kullanın:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Kullanılabilir VM boyutları listesi için bkz. [Sanal makinelerin boyutları](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Hizmeti dağıttıktan sonra, doğal dil yorumunu, sorgu tamamlamayı, yapılandırılmış sorgu gelişimini ve histogram hesaplamasını test etmek için çeşitli [web API'lerini](WebAPI.md) çağırabilirsiniz.  

## <a name="test-the-service"></a>Hizmeti test etme

Canlı hizmetin hatalarını ayıklamak için, web tarayıcısından konak makinesine göz atın. Host_service dağıtılan bir yerel hizmet için ziyaret `http://localhost:<port>/`.  Deploy_service dağıtılan bir Azure bulut hizmeti için ziyaret `http://<serviceName>.cloudapp.net/`.

Bu sayfada hem temel API çağrısı istatistikleri hakkındaki bilgilerin bağlantısı hem de bu hizmette barındırılan dil bilgisi ve dizin yer alır. Bu sayfa ayrıca web API'lerinin kullanımını gösteren etkileşimli bir arama arabirimi de içerir. Arama kutusuna sorguları girin ve [interpret](interpretMethod.md), [evaluate](evaluateMethod.md) ve [calchistogram](calchistogramMethod.md) API çağrılarının sonuçlarına bakın. Bu sayfanın temel HTML kaynağı, zengin, etkileşimli bir arama deneyimi oluşturmak için web API'lerini bir uygulamaya tümleştirme örneği işlevi de görür.


