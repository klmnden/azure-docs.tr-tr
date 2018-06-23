---
title: Bilgi Bankası araştırması hizmeti ile çalışmaya başlama | Microsoft Docs
description: Microsoft Bilişsel hizmetler akademik yayınlarda arasında altyapının etkileşimli arama deneyimi oluşturmak için bilgi araştırması hizmet (KES) kullanın.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 02dc9368eef02d6fa507335ef3171e923412acca
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352511"
---
<a name="getting-started"></a>
# <a name="get-started-with-the-knowledge-exploration-service"></a>Bilgi Bankası araştırması hizmeti ile çalışmaya başlama
Bu kılavuzda, etkileşimli arama deneyimi akademik yayınlar için altyapısı oluşturmak için bilgi araştırması hizmet (KES) kullanın. Komut satırı aracı'nı yüklemeden [ `kes.exe` ](CommandLine.md)ve tüm örnek dosyaları [bilgi araştırması hizmeti SDK'sı](https://www.microsoft.com/en-us/download/details.aspx?id=51488).

Akademik yayınlar örnek 1000 akademik raporlar Microsoft araştırmacılarının tarafından yayımlanan bir örneğini içerir.  Her kağıt bir başlık, yayın yıl, yazarlar ve anahtar sözcükler ile ilişkilidir. Her geliştirici bir kimliği, ad ve yayın zaman ilişkisi temsil edilir. Her anahtar sözcüğü (örneğin, "svm" eş anlamlısı ile ilişkili anahtar sözcüğü "Destek vektör makinesi" olabilir) anlamlıları kümesiyle ilişkili olabilir.

<a name="defining-schema"></a>
## <a name="define-the-schema"></a>Şeması Tanımlama
Şema etki alanındaki nesnelerin öznitelik yapısını tanımlar. Her öznitelik için adı ve veri türü bir JSON dosyası biçiminde belirtir. Aşağıdaki örnek dosyayı içeriktir *Academic.schema*.

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

Burada, tanımladığınız *başlık*, *yıl*, ve *anahtar sözcüğü* olarak bir dize, tamsayı ve dize özniteliği, sırasıyla. Yazarları kimliği, adı ve bağlantı tarafından temsil edilen çünkü tanımladığınız *Yazar* üç alt özniteliklerle bileşik özniteliği olarak: *Author.Id*, *yazar.adi*, ve *Author.Affiliation*.

Varsayılan olarak, öznitelikleri kullanılabilir tüm işlemleri kendi veri türü için destek dahil olmak üzere *eşittir*, *starts_with*, ve *is_between*. Yazar Kimliği yalnızca bir tanımlayıcı olarak dahili olarak kullanılır çünkü Varsayılanı geçersiz kılabilir ve belirtin *eşittir* yalnızca dizin oluşturulmuş işlemi.

İçin *anahtar sözcüğü* özniteliği, eş anlamlı dosya belirterek kurallı anahtar değerleriyle eşleşecek şekilde anlamlıları izin *Keyword.syn* öznitelik tanımında. Bu dosya, kurallı listesi ve eş değer çiftleri içerir:

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

Şema tanımı hakkında ek bilgi için bkz: [şema biçimi](SchemaFormat.md).

<a name="generating-data"></a>
## <a name="generate-data"></a>Veri oluştur
Veri dosyası yazıda, öznitelik değerleri belirtme her bir satır ile dizin oluşturmak için yayınlar listesini açıklayan [JSON biçimine](http://json.org/).  Aşağıdaki örnek veri dosyasından tek bir satırdır *Academic.data*okunabilirlik için biçimlendirilmiş:

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

Bu parçacığında, belirttiğiniz *başlık* ve *yıl* özniteliği JSON dizesi ve sayı olarak kağıt sırasıyla. Birden çok değerleri JSON dizileri kullanılarak gösterilir. Çünkü *Yazar* bileşik bir öznitelik alt özniteliklerini içeren bir JSON nesnesi kullanarak her bir değeri temsil edilir. Eksik değerleri ile gibi öznitelikleri *anahtar sözcüğü* bu durumda, JSON gösteriminden dışlandı.

Farklı yazıları olasılığını ayırt etmek için yerleşik kullanarak göreli günlük olasılık belirtin *logprob* özniteliği. Bir olasılık verilen *p* 0 ile 1 arasında günlük olarak günlük olasılık işlem (*p*), log() doğal günlük işlevini olduğu.

Daha fazla bilgi için bkz: [veri biçimi](DataFormat.md).

<a name="building-index"></a>
## <a name="build-a-compressed-binary-index"></a>Sıkıştırılmış bir ikili dizini oluşturun
Bir şema dosyası ve veri dosyasını çalıştırdıktan sonra kullanarak veri nesneleri sıkıştırılmış ikili dizin oluşturabilirsiniz [ `kes.exe build_index` ](CommandLine.md#build_index-command). Bu örnekte, dizin dosyası derleme *Academic.index* giriş şemasını dosyasından *Academic.schema* ve veri dosyası *Academic.data*. Aşağıdaki komutu kullanın:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Azure dışında hızlı prototipi oluşturulurken için [ `kes.exe build_index` ](CommandLine.md#build_index-command) yerel olarak küçük dizinlerini 10.000 nesneler içeren veri dosyalarından oluşturabilirsiniz. Büyük veri dosyaları için ya da komut içinden çalıştırabilirsiniz bir [Azure Windows VM](../../../articles/virtual-machines/windows/quick-create-portal.md), veya uzak bir yapı Azure'da gerçekleştirin. Ayrıntılar için bkz [ölçeklendirmeyi](#scaling-up).

<a name="authoring-grammar"></a>
## <a name="use-an-xml-grammar-specification"></a>Bir XML dilbilgisi belirtimi kullanın
Dilbilgisi hizmet bu doğal dil sorguları anlam sorgusu ifadelere nasıl dönüştürüleceğini yanı sıra yorumlaması için doğal dil sorguları kümesini belirtir. Bu örnekte, kullandığınız belirtilen Dilbilgisi *academic.xml*:

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

Dilbilgisi belirtimi sözdizimi hakkında daha fazla bilgi için bkz: [dilbilgisi biçimini](GrammarFormat.md).

<a name="compiling-grammar"></a>
## <a name="compile-the-grammar"></a>Dilbilgisi derleme
Bir XML dilbilgisi belirtimi aldıktan sonra onu ikili dilbilgisi kullanarak derleyebilir [ `kes.exe build_grammar` ](CommandLine.md#build_grammar-command). Dilbilgisi bir şema alıyorsa, şema dosyası XML dilbilgisi aynı yol bulunması gerektiğini unutmayın. Bu örnekte, ikili dilbilgisi dosyanın derleme *Academic.grammar* giriş XML dilbilgisi dosyasından *Academic.xml*. Aşağıdaki komutu kullanın:

`kes.exe build_grammar Academic.xml Academic.grammar`

<a name="hosting-index"></a>
## <a name="host-the-grammar-and-index-in-a-web-service"></a>Dilbilgisi ve bir web hizmeti dizinde ana bilgisayar
Hızlı prototipi oluşturulurken için bir web hizmeti yerel makinedeki dizinde ve dilbilgisi kullanarak barındırabilirsiniz [ `kes.exe host_service` ](CommandLine.md#host_service-command). Daha sonra hizmeti ile erişebilirsiniz [API'leri web](WebAPI.md) veri doğruluk ve dilbilgisi tasarımı doğrulanacak. Bu örnekte, dilbilgisi dosyası ana bilgisayarı *Academic.grammar* ve dizin dosyası *Academic.index* adresindeki http://localhost:8000/. Aşağıdaki komutu kullanın:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Bu web hizmetini yerel bir örneğini başlatır. Hizmet etkileşimli olarak ziyaret ederek test edebilirsiniz `http::localhost:<port>` bir tarayıcıdan. Daha fazla bilgi için bkz: [hizmetini sınama](#testing-service).

Ayrıca doğrudan çeşitli çağırabilirsiniz [API'leri web](WebAPI.md) doğal dil yorumlama, sorgu tamamlama, yapılandırılmış sorgu değerlendirme ve histogram hesaplama test etmek için. Hizmeti durdurmak için "Çık" girerek `kes.exe host_service` komut istemi veya Ctrl + C tuşlarına basın. İşte bazı örnekler:

* [http://localhost:8000/interpret?query=papers Çiğdem t dumais tarafından](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers d & tam Çiğdem t = 1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='Çiğdem t dumais') & attributes=Title,Year,Author.Name,Author.Id & sayısı = 2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='Çiğdem t dumais'), Yıl > 2013 =) & öznitelikleri = yıl, anahtar sözcüğü & sayısı = 4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Azure dışında [ `kes.exe host_service` ](CommandLine.md#host_service-command) 10.000 nesnelerin dizinlerini sınırlıdır. İşlem otomatik olarak sonlandırılmadan önce diğer sınırları saniyede 10 istekler API oranını ve 1000 isteklerinin toplam içerir. Bu kısıtlamalar atlamak için komutu içinden çalıştırın bir [Azure Windows VM](../../../articles/virtual-machines/windows/quick-create-portal.md), veya bir Azure bulut hizmeti dağıtmanızı [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) komutu. Ayrıntılar için bkz [hizmeti dağıtma](#deploying-service).

<a name="scaling-up"></a>
## <a name="scale-up-to-host-larger-indices"></a>Ana bilgisayar büyük dizinler ölçeği
Ne zaman çalıştırdığınız `kes.exe` Azure dışında dizini 10.000 nesnelere sınırlıdır. Derleme ve Azure kullanarak büyük dizinler barındırır. Kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/). Visual Studio veya MSDN abone olursanız, dilerseniz [abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Bu, her ay bazı Azure kredisi sunar.

İzin vermek için `kes.exe` bir Azure hesabı için erişim [Azure yayımlama ayarları dosyasını indirme](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) Azure portalından. İstenirse, istediğiniz Azure hesaba oturum açın. Dosyayı Farklı Kaydet *AzurePublishSettings.xml* nerede çalışma dizininde `kes.exe` çalıştırır.

Derleme ve büyük dizinler barındırmak için iki yolu vardır. İlk Azure Windows VM şeması ve verisi dosyalarında hazırlamak vardır. Ardından çalıştırın [ `kes.exe build_index` ](#building-index) dizin yerel olarak boyut kısıtlamaları olmadan VM oluşturmak için. Sonuçta ortaya çıkan dizin yerel olarak VM kullanarak barındırılabilir [ `kes.exe host_service` ](#hosting-service) herhangi bir kısıtlama olmadan yeniden hızlı prototipi oluşturulurken için. Ayrıntılı adımlar için bkz: [Azure VM Öğreticisi](../../../articles/virtual-machines/windows/quick-create-portal.md).

Uzak bir Azure yapı kullanarak gerçekleştirmek için ikinci yöntemdir [ `kes.exe build_index` ](CommandLine.md#build_index-command) ile `--remote` parametresi. Bu, bir Azure VM boyutu belirtir. Zaman `--remote` parametresi belirtilirse, bu boyut, geçici bir Azure VM komut oluşturur. Daha sonra VM dizin oluşturur, dizini hedef blob depolama alanına yükler ve VM tamamlanmasından sonra siler. Dizin oluşturulmuş sırada, Azure aboneliğinizin VM maliyeti için ücretlendirilir.

Bu uzak Azure yapı yeteneği verir [ `kes.exe build_index` ](CommandLine.md#build_index-command) herhangi bir ortamda çalıştırılmak üzere. Uzak bir yapı gerçekleştirirken, giriş şeması ve verisi bağımsız değişkenler yerel dosya yolları olabilir veya [Azure blob depolama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) URL'leri. Çıktı dizini bağımsız bir blob depolama URL olması gerekir. Bir Azure depolama hesabı oluşturmak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Dosyaları çok verimli bir şekilde kopyalayın ve blob depolama alanından kullanmak için [AzCopy](../../storage/common/storage-use-azcopy.md) yardımcı programı.

Bu örnekte, aşağıdaki blob depolama kapsayıcısı zaten oluşturuldu varsayabilirsiniz: http://&lt;*hesap*&gt;.blob.core.windows.net/&lt;*kapsayıcı* &gt;/. Bir şema içeriyor *Academic.schema*, başvurulan eş anlamlı dosya *Keywords.syn*ve tam ölçekli veri dosyası *Academic.full.data*. Aşağıdaki komutu kullanarak tam dizin uzaktan oluşturabilirsiniz:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Temporay dizini oluşturmak için VM sağlamak için 5-10 dakika sürebileceğini unutmayın. Hızlı prototipi oluşturulurken için şunları yapabilirsiniz:
- Yerel olarak herhangi bir makinede daha küçük bir veri kümesiyle geliştirin.
- El ile [bir Azure VM oluşturma](../../../articles/virtual-machines/windows/quick-create-portal.md), [bağlanmak](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) Uzak Masaüstü yükleme [bilgi araştırması hizmeti SDK'sı](https://www.microsoft.com/en-us/download/details.aspx?id=51488), çalıştırıp [ `kes.exe` ](CommandLine.md) gelen VM dahilinde.

Disk belleği derleme işlemini yavaşlatır. Disk belleği önlemek için bir VM ile RAM miktarını üç kez dizin oluşturma için giriş veri dosyası boyutu kullanın. Barındırma için dizin boyuttan daha fazla RAM 1 GB olan bir VM kullanın. Kullanılabilir VM boyutları listesi için bkz: [sanal makineler için Boyutlar](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

<a name="deploying-service"></a>
## <a name="deploy-the-service"></a>Hizmeti Dağıt
Dilbilgisi ve dizini oluşturduktan sonra hizmeti bir Azure bulut hizmeti dağıtmak hazır olursunuz. Yeni bir Azure bulut hizmeti oluşturmak için bkz: [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Bir dağıtım paketi bu noktada belirtmeyin.  

Bulut hizmeti oluştururken kullanabileceğiniz [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) hizmeti dağıtmak için. Bir Azure bulut hizmeti iki dağıtım yuvası yok: üretim ve hazırlama. Dinamik kullanıcı trafiği alır bir hizmet için başlangıçta hazırlama yuvasını dağıtmanız gerekir. Hizmetin başlatılması ve kendisini başlatma için bekleyin. Ardından, dağıtımı doğrulamak ve temel testlerini geçtiğini doğrulamak için birkaç istekleri gönderebilirsiniz.

[Takas](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) hazırlama içeriğini yuva üretim yuvasıyla böylece dinamik trafik şimdi yeni dağıtılan hizmete yönlendirilir. Yeni veri hizmetiyle güncelleştirilmiş bir sürümünü dağıtırken bu işlemi yineleyebilirsiniz. Diğer tüm Azure bulut Hizmetleri ile isteğe bağlı olarak Azure Portalı'nı yapılandırmak için kullanabileceğiniz gibi [otomatik ölçeklendirme](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md).

Bu örnekte, dağıttığınız *akademik* var olan bir bulut hizmetiyle hazırlama yuvası dizinine *< vm_size >* VM'ler. Aşağıdaki komutu kullanın:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Kullanılabilir VM boyutları listesi için bkz: [sanal makineler için Boyutlar](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Hizmet dağıttıktan sonra çeşitli çağırabilirsiniz [API'leri web](WebAPI.md) doğal dil yorumlama, sorgu tamamlama, yapılandırılmış sorgu değerlendirme ve histogram hesaplama test etmek için.  

<a name="testing-service"></a>
## <a name="test-the-service"></a>Hizmeti test
Canlı bir hizmette hata ayıklamak için bir web tarayıcısından konak makineye göz atın. Yerel bir hizmet aracılığıyla dağıtılan için [host_service](#hosting-service), ziyaret `http://localhost:<port>/`.  Azure bulut hizmeti aracılığıyla dağıtılan [deploy_service](#deploying-service), ziyaret `http://<serviceName>.cloudapp.net/`.

Bu sayfa temel API çağrısı istatistikleri yanı sıra dilbilgisi ve bu hizmetin barındırılan dizin bilgilerine bir bağlantı içerir. Bu sayfa Ayrıca web API'leri kullanımını gösteren bir etkileşimli arama arabirimi içerir. Sorgu sonuçlarını görmek için arama kutusuna girin [yorumlama](interpretMethod.md), [değerlendirmek](evaluateMethod.md), ve [calchistogram](calchistogramMethod.md) API çağrıları. Temel alınan bu sayfasının HTML kaynağını, ayrıca web API'leri zengin ve etkileşimli arama deneyimi oluşturmak için uygulamaya tümleştirmek nasıl bir örnek olarak görev yapar.


