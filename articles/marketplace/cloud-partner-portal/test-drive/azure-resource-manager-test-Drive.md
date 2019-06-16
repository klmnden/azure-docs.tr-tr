---
title: Azure Resource Manager Test Sürüşü | Azure Market
description: Azure Resource Manager kullanarak bir Market Test Sürüşü oluşturun
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Patrick .Butler
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 92c55c7f15b3f350ad802157bf401f3e75983789
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606443"
---
# <a name="azure-resource-manager-test-drive"></a>Azure Resource Manager Test Sürüşü

Bu makalede, Azure Market veya Appsource'ta kim teklifini sahip olan ancak kendi Test Sürüşü yalnızca Azure kaynakları ile oluşturmak istediğiniz yayımcılar içindir.

Bir Azure Resource Manager (Resource Manager) şablonu, Azure kaynaklarınızın en iyi temsil etmek için çözümünüzün tasarım kodlanmış bir kapsayıcıdır. Bir Resource Manager şablonu durumdayken alışkın değilseniz, okumaya [Resource Manager şablonları anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) ve [Resource Manager şablonları yazma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) derleyip test etmeye nasıl bildiğinizden emin olmak için kendi şablonlarınızı.

Test Sürüşü yapar, sağlanan Resource Manager şablonu alır ve bir kaynak grubunda bu Resource Manager şablonundan gerekli tüm kaynakların dağıtımını yapar olduğu.

Bir Azure Resource Manager Test Sürüşü oluşturmak isterseniz, sizin için gereksinimler şunlardır:

- Derleme, test edin ve ardından Test sürücü Resource Manager şablonunuzu karşıya yükleyin.
- Tüm gerekli meta veriler ve Test Sürüşünüz etkinleştirmek için ayarları yapılandırın.
- Teklifinizi Test Sürüşü etkin yeniden yayımlayın.

## <a name="how-to-build-an-azure-resource-manager-test-drive"></a>Bir Azure Resource Manager Test Sürüşü oluşturma

Bir Azure Resource Manager Test Sürüşü oluşturma işlemi şu şekildedir:

1. Bir Akış Diyagramı içinde yapmak için müşterilerinizin ne istediğini tasarlayın.
1. Hangi deneyimleri müşterilerinizin oluşturmak istediğiniz tanımlayın.
1. Yukarıdaki tanımlarını temel alarak, hangi parçaları ve kaynakları gibi deneyimi gerçekleştirmek, müşteriler için gerekli olan karar verin: Örneğin, D365 örneği veya bir veritabanı ile Web sitesi.
1. Tasarım yerel olarak oluşturun ve deneyimi test edin.
1. Paketi bir ARM şablon dağıtımı ve buradan deneyimi:
    1. Kaynakları hangi bölümlerinin giriş parametreleridir tanımlayın.
    1. Hangi değişkenlerdir;
    1. Hangi çıkışları için Müşteri Deneyimi verilmiştir.
1. Yayımlama, test edin ve yayınlayın.

Bir Azure Resource Manager Test Sürüşü oluşturma konusunda en önemli kısmı, müşterilerinizin deneyimini istediğiniz hangi senaryoları tanımlamaktır. Bir güvenlik duvarı ürünü ve tanıtım ne kadar iyi, betik ekleme saldırılarını işlemek istiyorsanız misiniz? Depolama ürün ve ne kadar hızlı ve kolay bir şekilde çözümünüzü tanıtım istiyorsanız dosyaları sıkıştırır misiniz?

Ürününüzü göstermek için en iyi yollarından nelerdir değerlendirme süresi yeterli miktarda harcayabileceğiniz emin olun. Resource Manager şablonu yeterince daha kolay paketleme getirir özellikle tüm gerekli kaynakları geçici olarak ihtiyacınız.

Güvenlik Duvarı Örneğimizdeki ile devam etmek için mimarisi, güvenlik duvarınızı koruyan bir Web sitesi için hizmetinizin genel bir IP URL'sini ve başka bir genel IP URL gerekiyor olabilir. Her IP bir sanal makinede dağıtılan ve bağlı bir ağ güvenlik grubu + ağ arabirimi ile birlikte.

İstenen paket kaynakları tasarladıktan sonra artık Test sürücü Resource Manager şablonu oluşturma ve yazma içermektedir.

## <a name="writing-test-drive-resource-manager-templates"></a>Test sürücü Resource Manager şablonları yazma

Test Sürüşü tam otomatik bir modda ve bu nedenle dağıtımları çalışan, Test Sürüşü şablonları aşağıda açıklanan bazı kısıtlamalar vardır.

### <a name="parameters"></a>Parametreler

Çoğu şablon parametre kümesine sahiptir. Parametreleri ve benzeri kaynak adları, kaynakları boyutları (örneğin, depolama hesaplarını veya sanal makine boyutları türleri), kullanıcı adları ve parolalar, DNS adlarını tanımlar. Azure portalını kullanarak çözümleri dağıttığınızda, el ile bu tüm parametreleri doldurun, kullanılabilen DNS adları veya depolama hesabı adları seçin ve benzeri.

![Bir Azure Kaynak Yöneticisi'nde parametrelerinin listesi](./media/azure-resource-manager-test-drive/param1.png)

Ancak, yalnızca sınırlı sayıda parametre kategorileri destekler, böylece Test Sürüşü insan etkileşimi olmadan tamamen otomatik bir modda çalışır. Test sürücü Resource Manager şablonunda bir parametre desteklenen kategorilerden birine denk olmayan gerekir **Bu parametre bir değişken veya sabit değer ile değiştirin.**

Parametrelerinizi için geçerli bir ad kullanabilir, meta veri türü değeri kullanarak Test Sürüşü parametre kategorisi tanır. **Meta veri türü, her bir şablon parametresi için belirtmelisiniz**, aksi takdirde, şablonunuzu doğrulama geçmez:

```json
"parameters": {
  ...
  "username": {
    "type": "string",
    "metadata": {
      "type": "username"
    }
  },
  ...
}
```

Dikkat etmeniz önemlidir **tüm parametreler isteğe bağlıdır**, bu nedenle görmüyorsanız\'kullanmak istiyorsanız, istemiyorsunuz\'t zorunda.

### <a name="accepted-parameter-metadata-types"></a>Kabul edilen parametresi meta veri türleri

| Meta veri türü   | Parametre türü  | Açıklama     | Örnek değer    |
|---|---|---|---|
| **BaseUri**     | dize          | Taban URI, dağıtım paketi| https:\//\<\..\>.blob.core.windows.net/\<\..\> |
| **Kullanıcı adı**    | dize          | Yeni rastgele kullanıcı adı.| admin68876      |
| **Parola**    | güvenli dize    | Yeni rastgele bir parola | LP! ACS\^2kh     |
| **Oturum kimliği**   | dize          | Benzersiz Test Sürüşü oturum Kimliğini (GUID)    | b8c8693e-5673-449c-badd-257a405a6dee |

#### <a name="username"></a>kullanıcı adı

Test Sürüşü başlatır, bu parametre ile bir **Base URI** paketinize dahil herhangi bir dosya URI'si oluşturmak için bu parametreyi kullanabilmeniz için dağıtım paketi.

```json
"parameters": {
  ...
  "baseuri": {
    "type": "string",
    "metadata": {
      "type": "baseuri",
      "description": "Base Uri of the deployment package."
    }
  },
  ...
}
```

Şablon içinde bu parametre, Test Sürüşü dağıtım paketinden herhangi bir dosyanın bir URI oluşturmak için kullanabilirsiniz. Aşağıdaki örnekte, bağlı şablonun bir URI oluşturmak gösterilmektedir:

```json
"templateLink": {
  "uri": "[concat(parameters('baseuri'),'templates/solution.json')]",
  "contentVersion": "1.0.0.0"
}
```

#### <a name="username"></a>kullanıcı adı

Bu parametre yeni bir rastgele kullanıcı adı ile test Sürüşü başlatır:

```json
"parameters": {
  ...
  "username": {
    "type": "string",
    "metadata": {
      "type": "username",
      "description": "Solution admin name."
    }
  },
  ...
}
```

Örnek değer:

    admin68876

Çözümünüz için rastgele veya sabit kullanıcı adlarını kullanabilirsiniz.

#### <a name="password"></a>password

Bu parametre yeni, rastgele bir parola ile test Sürüşü başlatır:

```json
"parameters": {
  ...
  "password": {
    "type": "securestring",
    "metadata": {
      "type": "password",
      "description": "Solution admin password."
    }
  },
  ...
}
```

Örnek değer:

    Lp!ACS^2kh

Çözümünüz için rastgele veya sabit parolaları kullanabilirsiniz.

#### <a name="session-id"></a>Oturum kimliği

Test Sürüşü Test Sürüşü oturum kimliği temsil eden benzersiz bir GUID ile bu parametreyi başlatın:

```json
"parameters": {
  ...
  "sessionid": {
    "type": "string",
    "metadata": {
      "type": "sessionid",
      "description": "Unique Test Drive session id."
    }
  },
  ...
}
```

Örnek değer:

    b8c8693e-5673-449c-badd-257a405a6dee

Gerekirse Test Sürüşü oturumu benzersiz olarak tanımlanabilmesi için bu parametreyi kullanın.

### <a name="unique-names"></a>Benzersiz adlar

Depolama hesapları veya DNS adları gibi bazı Azure kaynaklarını, genel olarak benzersiz bir ad gerektirir.

Resource Manager şablonu Test Sürüşü dağıtır her zaman oluşturur, yani bir **benzersiz bir ada sahip yeni bir kaynak grubu** tüm kendi\' kaynakları. Bu nedenle kullanmak için gereklidir [uniquestring](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions#uniquestring) kaynak grubu kimlikleri benzersiz rastgele değerler oluşturmak için değişken adları ile birleştirilmiş işlevi:

```json
"variables": {
  ...
  "domainNameLabel": "[concat('contosovm',uniquestring(resourceGroup().id))]",
  "storageAccountName": "[concat('contosodisk',uniquestring(resourceGroup().id))]",
  ...
}
```

Parametre/değişkeni dizelerinizi birleştirme emin olun (\'contosovm\') benzersiz bir dize çıktısı ile (\'resourceGroup () .id\'), bu her bir değişken güvenilirliğini ve benzersizliği garanti eder.

Örneğin, çoğu kaynak adları bir rakamla başlayamaz, ancak bir rakam ile başlayan bir dize benzersiz bir dize işlevi döndürebilir. Bu nedenle, benzersiz bir ham dize çıkış kullanırsanız, dağıtımlar başarısız olur. 

Kaynak adlandırma kuralları ve kısıtlamaları hakkında daha fazla bilgi bulabilirsiniz [bu makalede](https://docs.microsoft.com/azure/guidance/guidance-naming-conventions).

### <a name="deployment-location"></a>Dağıtım konumu

Size, Test Sürüşü kullanılabilir farklı Azure bölgelerinde yapabilirsiniz. Beast kullanıcı deneyimi sunmak için en yakın bölgeyi seçin açmasına izin vermek için olur.

Test Sürüşü Laboratuvar örneğini oluşturduğunda, bölge seçtiğiniz bir kullanıcı tarafından her zaman bir kaynak grubu oluşturur ve ardından bu grubu bağlamında, dağıtım şablonu yürütür. Bu nedenle, şablonunuzu kaynak grubundan dağıtım konumu seçmeniz gerekir:

```json
"variables": {
  ...
  "location": "[resourceGroup().location]",
  ...
}
```

' İ tıklatın ve ardından belirli bir laboratuvar örneği için her kaynak için bu konumu kullanır:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/publicIPAddresses",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/virtualNetworks",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Network/networkInterfaces",
    "location": "[variables('location')]",
    ...
  },
  {
    "type": "Microsoft.Compute/virtualMachines",
    "location": "[variables('location')]",
    ...
  }
]
```

Aboneliğiniz her seçmiş olursunuz bölgelerin dağıtmak istediğiniz tüm kaynakları dağıtma izni olduğunu emin olmanız gerekir. Sanal makine görüntülerinizi etkinleştirmek için seçeceğiz tüm bölgelerde kullanılabilir olduğundan emin olmanız gerekir de, aksi takdirde, dağıtım şablonu için bazı bölgelerde çalışmaz.

### <a name="outputs"></a>Çıkışlar

Normalde Resource Manager şablonları ile herhangi bir çıktı üretmeden dağıtabilirsiniz. Şablon parametreleri doldurmak için kullandığınız tüm değerleri biliyor olmasıdır ve her zaman el ile herhangi bir kaynağın özelliklerini inceleyebilirsiniz.

Test sürücü Resource Manager şablonları için ancak bunu\'Test Sürüşü için bir laboratuvar (Web sitesi bir URI'leri, sanal makine ana bilgisayar adları, kullanıcı adları ve parolalar) erişmek için gerekli tüm bilgileri döndürmek önemlidir. Bu değişkenler müşteriye sunulduğundan, tüm çıkış adları okunabilir olduğundan emin olun.

Şablon çıktıları ilgili bir kısıtlama yoktur. Yalnızca unutmayın, tüm çıktı değerlerini Test Sürüşü dönüştürür **dizeleri**, çıktıyı bir nesne gönderirseniz, kullanıcı JSON görürsünüz dize.

Örnek:

```json
"outputs": {
  "Host Name": {
    "type": "string",
    "value": "[reference(variables('pubIpId')).dnsSettings.fqdn]"
  },
  "User Name": {
    "type": "string",
    "value": "[parameters('adminName')]"
  },
  "Password": {
    "type": "string",
    "value": "[parameters('adminPassword')]"
  }
}
```

### <a name="subscription-limits"></a>Abonelik limitleri

Daha fazla şeyi dikkate atmanız aboneliktir ve hizmet sınırları. Örneğin, en fazla on 4 çekirdekli sanal makineler dağıtmak istiyorsanız, Laboratuvarınız için kullandığınız abonelik 40 çekirdek kullanmanıza izin verdiğinden emin olmak gerekir.

Azure aboneliği ve hizmet sınırları hakkında daha fazla bilgi bulabilirsiniz [bu makalede](https://docs.microsoft.com/azure/azure-subscription-service-limits). Aynı anda birden çok Test Sürüşleri alınabilir gibi aboneliğinizin işleyebileceğini doğrulayın \# eş zamanlı Test gerçekleştirilebilecek sürücüleri, toplam sayı ile çarpılan çekirdek.

### <a name="what-to-upload"></a>Ne yüklemek için

Adlı bir dosya çeşitli dağıtım yapıları içeren bir zip dosyası, ancak gerektiğinde test sürücü Resource Manager şablonu karşıya **ana template.json**. Azure Resource Manager dağıtım şablonu bu dosyasıdır ve Test Sürüşü bir laboratuvar oluşturmak için kullanır.

Bu dosya ek kaynaklar varsa, şablonu içinde dış bir kaynak olarak başvuru veya zip dosyasında kaynak içerebilir.

Yayımlama sertifika sırasında Test Sürüşü dağıtım paketinizi unzips ve içeriğini bir iç Test Sürüşü blob kapsayıcısına yerleştirir. Bir kapsayıcı yapısı, dağıtım paketi yapısını gösterir:

| Package.zip                       | Test sürücü blob kapsayıcısı         |
|---|---|
| Ana template.json                | https:\//\<\.... \>.blob.core.windows.net/\<\.... \>/main-template.json  |
| Templates/Solution.JSON           | https:\//\<\.... \>.blob.core.windows.net/\<\.... \>/templates/solution.json |
| scripts/warmup.ps1                | https:\//\<\.... \>.blob.core.windows.net/\<\.... \>/scripts/warmup.ps1  |


Bu blob kapsayıcısında URI temel URI'sini diyoruz. Blob kapsayıcısı laboratuvarınızın her değişiklik vardır ve bu nedenle, her değişiklik laboratuvarınızın kendi taban URI'sine sahip. Test Sürüşü sıkıştırması dağıtım paketinizi temel URI'sini şablonunuzu şablon parametreleri aracılığıyla içine geçirebilirsiniz.

## <a name="transforming-template-examples-for-test-drive"></a>Test Sürüşü için dönüştürme şablon örnekleri

Bir Test sürücü Resource Manager şablonuna bir mimari kaynakların kapatma işlemin göz korkutucu olabilir. Bu işlem kolaylaştırılmasına yardımcı olmak için biz\'en iyi nasıl yaptıysanız örnekler [burada geçerli dağıtım şablonlarını Dönüştür](./transforming-examples-for-test-drive.md).

## <a name="how-to-publish-a-test-drive"></a>Bir Test sürüşüne yayımlama

Oluşturulan Test Sürüşünüz olduğuna göre bu bölümde, başarılı bir şekilde Test Sürüşünüz yayımlamak gerekli alanların her biri açıklanmaktadır.

![Test Sürüşü kullanıcı arabiriminde etkinleştirme](./media/azure-resource-manager-test-drive/howtopub1.png)

İlk ve en önemli alan teklifiniz için Etkin Test Sürüşü isteyip istemediğinizi geçiş yapmak için kullanılır. Seçtiğinizde, **Evet,** formun tüm gerekli alanları ile geri kalanı doldurmak size sunulur. Seçtiğinizde, **Hayır** form devre dışı kalır ve devre dışı Test Sürüşü ile yeniden yayımlarsanız, Test Sürüşünüz üretimden kaldırılır.

Not: Etkin kullanıcılar tarafından kullanılan sürücüleri tüm testleri vardır, bu Test Sürüşleri oturumun süresi dolana kadar çalışmaya devam eder.

### <a name="details"></a>Ayrıntılar

Doldurmak için sonraki bölüme, Teklif Ayrıntıları, Test Sürüşü hakkında ' dir.

![Test Sürüşü ayrıntılı bilgi](./media/azure-resource-manager-test-drive/howtopub2.png)

**Açıklama -** *gerekli* budur burada Test Sürüşünüz nedir hakkında temel bir açıklama yazın. Müşteri, hangi senaryolarını Test Sürüşünüz ürününüzü hakkında kapsayan okumak için buraya gelir. 

**Kullanıcı el ile -** *gerekli* Test Sürüşü deneyiminizin ayrıntılı izlenecek yolu budur. Müşteri, bu açılır ve tam olarak kendi Test Sürüşü yapmasını istediğiniz aracılığıyla size yol. Bu içeriği kolayca izleyin ve anlamak önemlidir! (.Pdf dosyası olması gerekir)

**Test sürücü tanıtım videosu -** *önerilen* benzer kullanıcı el ile Test Sürüşü deneyiminizin videosu dahil en iyisidir. Müşteri, bu önceki ya da kendi Test Sürüşü sırasında izleyecek ve tam olarak kendi Test Sürüşü yapmasını istediğiniz aracılığıyla size yol. Bu içeriği kolayca izleyin ve anlamak önemlidir!

- **Ad** -videonuzu başlığı
- **Bağlantı** -boru veya video katıştırılmış bir URL olmalıdır. Katıştırılmış URL'sini alma hakkında bir örnek aşağıda verilmiştir:
- **Küçük resim** -yüksek kaliteli görüntü (533 x 324) piksel olmalıdır. Test Sürüşü deneyiminizi kısmı görüntüsü burada olması önerilir.

Nasıl bu alanlar, müşteri için Test Sürüşü deneyimlerini sırasında görünmesini aşağıdadır.

![Test Sürüşü alanların Market teklifi konumu](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Teknik yapılandırma

Burada Test sürücü Resource Manager şablonunuzu karşıya yükleyin ve nasıl özellikle tanımlamak doldurmak için sonraki bölüme olan Test Sürüşünüz iş örnekler.

![](./media/azure-resource-manager-test-drive/howtopub5.png)

**Örnekleri -** *gerekli* kaç örnekler yapılandıracağınız budur istiyorsanız, hangi bölgeler içinde ve müşterilerin Test Sürüşü ne kadar hızlı alabilirsiniz.

- **Örnekleri** -bölgeleri seçin olduğu yeri de Test sürücü Resource Manager şablonunuzu dağıtıldığı seçin. Yalnızca tek bir bölge müşterilerinizin adresindedir olması en burada beklediğiniz seçmek için önerilir.
- **Sık erişimli** -dağıtılan ve bekleniyor zaten olan sayı, Test Sürüşü yapmasını örneklere erişmek seçili bölge başına. Müşteriler, bir dağıtım için beklemek zorunda yerine bu Test Sürüşleri anında erişebilirsiniz. Artırabilen maliyeti daha büyük bir çalışma süresi tabi şekilde, bu örneklerin her zaman Azure aboneliğinize göre çalıştığını ' dir. Olması önemle tavsiye edilir **en az bir etkin örnek**, müşterilerin çoğu tam dağıtımları için beklemek istemiyorsanız gibi ve müşteri kullanımı bir bırakma için.
- **Orta Gecikmeli** - örnekler dağıtılan bölge başına sayı, Test Sürüşü yapmasını ve ardından VM olduğundan durduruldu ve Azure Depolama'da depolanan. Orta Gecikmeli örnekleri için bekleme süresi sık erişimli örnekleri yavaştır, ancak çalışma süresi depolama maliyetini de daha ucuz.
- **Soğuk** -büyük olasılıkla dağıtılabilir bölge başına sayı, Test Sürüşü yapmasını örnekleri. Soğuk örnekleri zaman Test Sürüşü sık erişimli veya sıcak örnekleri yavaştır isteyen bir müşteri dağıtımı aracılığıyla gitmek için tüm Test sürücü Resource Manager şablonu gerektirir. Ancak, karşılıklı avantaj ve dezavantajlarını, yalnızca Test Sürüşü süresi için ödeme yapmayı olmasıdır.

Şu anda olası eş zamanlı Test seçeceğiz sürücüleri toplam sayısını hesaplar kullanılabilir hale getirmek ve bu eşzamanlı tutar, aboneliğinize ilişkin kota sınırınıza işleyebileceğini doğrulayın:

**(Süre seçili bölgeleri sık erişimli örnekleri x) + (sayı, seçili bölgeleri sıcak örnekleri x) + (sayı, seçili bölgeleri soğuk örnekleri x)**

**Test sürücü süresi (saat) -** *gerekli* ne kadar Test Sürüşü içinde etkin kalacak süre \# saat. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer.

**Test sürücü Resource Manager şablonu -** *gerekli* , Resource Manager şablonunuzu karşıya yükleyin. Bu, yukarıdaki önceki bölümde oluşturulan dosyasıdır. Ana şablon dosyası adı: "ana template.json" ve Resource Manager şablonunuzu çıkış parametreleri için gerekli olan anahtar değişkenleri içerdiğinden emin olun. (Bir .zip dosyası olması gerekir)

**Erişim bilgileri -** *gerekli* bir müşteri, Test Sürüşü aldıktan sonra erişim bilgileri kullanıcılara sunulur. Bu yönergeler, Test sürücü Resource Manager şablonunuzu yararlı çıkış parametrelerini paylaşmak için yöneliktir. Çıktı parametreleri eklemek için çift kaşlı ayraçlar kullanın (örneğin, **{{outputname}}** ), ve konumda doğru eklenir. (HTML biçimlendirme dizesi burada ön uç işleme için önerilir).

### <a name="test-drive-deployment-subscription-details"></a>Test Sürüşü dağıtım Abonelik Ayrıntıları

Doldurmak için son bölümü, Test sürücüleri otomatik olarak Azure aboneliğinizi ve Azure Active Directory (AD) bağlanarak dağıtabilmesini sağlamaktır.

![Test Sürüşü dağıtım Abonelik Ayrıntıları](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure abonelik kimliği -** *gerekli* bu Azure hizmetlerini ve Azure portalına erişim verir. Burada kullanım raporlama ve Hizmetleri faturalandırılır aboneliktir. Zaten yoksa bir **ayrı** Azure aboneliği için Test Sürüşleri yalnızca bir tane biri olun. Azure abonelik kimlikleri, Azure portalında oturum açıyorsanız ve sol taraftaki menüyü Aboneliklerde giderek bulabilirsiniz. (Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure abonelikleri](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD Kiracı kimliği -** *gerekli* bir kiracı bulabilirsiniz altındaki özellikler - kimliği zaten mevcut varsa\> dizin kimliği

![Azure Active Directory özellikleri](./media/azure-resource-manager-test-drive/subdetails3.png)

Aksi takdirde, Azure Active Directory'de yeni bir kiracı oluşturun.

![Azure Active Directory listesi Kiracı](./media/azure-resource-manager-test-drive/subdetails4.png)

![Azure AD kiracınız için kuruluş, etki alanı ve ülke/bölge tanımlayın](./media/azure-resource-manager-test-drive/subdetails5.png)

![Seçimi onaylayın](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD uygulama kimliği -** *gerekli* oluşturmak ve yeni bir uygulamayı kaydetmek için bir sonraki adım olacaktır. Test Sürüşü örneğinizin işlemleri gerçekleştirmek için bu uygulamayı kullanacağız.

1. Yeni oluşturduğunuz dizine gidin veya dizin zaten var ve Azure Active directory içinde Filtre bölmesini seçin.
2. "Uygulama kayıtları" ve "Ekle" ye tıklayın.
3. Bir uygulama adı sağlayın.
4. Türü olarak seçin "Web uygulaması / API'si"
5. Oturum açma URL'si herhangi bir değer girin, biz de kazandık\'t bu alanı kullanıyor.
6. Oluştur'a tıklayın.
7. Uygulama oluşturulduktan sonra - özelliklerine gidin\> çok kiracılı uygulama ayarlayın ve Kaydet'e basın.

Kaydet’e tıklayın. Son adım, bu kayıtlı uygulama için uygulama Kimliğini alın ve burada Test Sürüşü alana yapıştırın sağlamaktır.

![Azure AD kimliği ayrıntısı](./media/azure-resource-manager-test-drive/subdetails7.png)

Verilen kullanıyoruz uygulamayı aboneliğinize dağıtmak için biz uygulamanın abonelik üzerinde katkıda bulunan olarak eklemeniz gerekir. Bu yönergeleri olarak olan aşağıda:

1. Abonelikler dikey penceresine gidin ve yalnızca Test Sürüşü için kullanmakta olduğunuz uygun aboneliği seçin.
1. Tıklayın **erişim denetimi (IAM)** .
1. Tıklayın **rol atamaları** sekmesi.  ![Yeni bir erişim denetimi sorumlusu ekleme](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. Tıklayın **rol ataması Ekle**.
1. Rol olarak ayarla **katkıda bulunan**.
1. Azure AD uygulama adını yazın ve rol atamak için uygulamayı seçin.
    ![İzin Ekle](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. **Kaydet**’e tıklayın.

**Azure AD uygulama anahtarı -** *gerekli* bir kimlik doğrulama anahtarını oluşturmak için son alandır. Anahtarı altında anahtarı bir açıklama ekleyin, ardından süresiz olarak süresini Kaydet'i belirleyin. Bu **önemli** süresi dolmuş zorunda kalmamak için anahtar, hangi test sürüşünüz üretimde çalışmamasına neden olur. Bu değeri kopyalayın ve gerekli Test Sürüşü alanına yapıştırın.

![Azure AD uygulaması için anahtarlar gösterir](./media/azure-resource-manager-test-drive/subdetails8.png)

## <a name="next-steps"></a>Sonraki adımlar

Test Sürüşü alanlarınızı doldurulan sahip olduğunuza göre üzerinden geçmek ve **yeniden yayımlamanız** teklifinizi. Test Sürüşünüz sertifika geçtikten sonra gitmesi gereken bir müşteri deneyimini Java'da test **Önizleme** teklifinizin. Test Sürüşü kullanıcı Arabiriminde başlatın ve ardından Azure aboneliğinizi Azure portalın içinde açın ve Test Sürüşleri tam olarak doğru şekilde dağıtıldığını doğrulayın.

![Azure portal](./media/azure-resource-manager-test-drive/subdetails9.png)

Bir müşteri ile tamamlandıktan sonra Test Sürüşü hizmeti otomatik olarak bu kaynak gruplarını temizler, böylece müşterileriniz için hazırlanan gibi herhangi bir Test Sürüşü örneği silmeyin dikkat edin önemlidir.

Şimdi Önizleme teklifinizle birlikte hissedene sonra durumuna gelir **yayınlayın**! Teklif çift yayımlanan onay uçtan uca deneyiminin tamamı silindikten sonra Microsoft son gözden geçirme işleminden yoktur. Herhangi bir nedenden dolayı teklifini reddetti, teklifinizi ne düzeltilecek gerekenleri mühendislik birimi ilgili kişisi için size bildirim göndereceğiz.

Lütfen Git başka sorularım varsa, sorun giderme önerilerine aradığınız veya Test Sürüşünüz daha da başarılı hale getirmek istediğiniz [SSS, sorun giderme ve en iyi](./marketing-and-best-practices.md).
