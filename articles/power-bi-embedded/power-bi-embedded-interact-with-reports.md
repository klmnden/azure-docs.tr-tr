<properties
   pageTitle="JavaScript API’sini kullanarak raporlarla etkileşim kurma | Microsoft Azure"
   description="Power BI Embedded, JavaScript API’si kullanarak raporlarla etkileşim kurma"
   services="power-bi-embedded"
   documentationCenter=""
   authors="mgblythe"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="08/26/2016"
   ms.author="mblythe"/>


# JavaScript API’sini kullanarak Power BI raporlarıyla etkileşim kurma

Power BI JavaScript API’’si Power BI raporlarını uygulamalarınıza kolaylıkla eklemenizi sağlar. API ile uygulamalarınız sayfalar ve filtreler gibi farklı rapor öğeleri ile programlı bir şekilde etkileşim kurabilir. Bu etkileşim Power BI raporlarını uygulamanızın daha tümleşik bir parçası yapar.

Bir Power BI raporunu uygulamanıza katıştırmak için uygulamanın bir parçası olarak barındırılan bir iframe kullanın. Aşağıdaki görüntüde görebileceğiniz gibi bu iframe uygulamanızla rapor arasında sınır işlevi görür. 

![Javascript API’si olmadan Power BI embedded iframe](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

iframe katıştırma işlemini çok daha kolaylaştırır, ancak JavaScript API’si olmadan rapor ve uygulamanız birbiriyle etkileşim kuramaz. Bu etkileşimin kurulamaması raporun uygulamanıza gerçekten ait olmadığını hissettirebilir. Rapor ve uygulama aşağıdaki görüntüde olduğu gibi geri ve ileri doğru iletişim kurmalıdır.

![Javascript API’si ile Power BI embedded iframe](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Power BI JavaScript API’si iframe sınırından güvenli bir şekilde geçebilecek bir kod yazmanızı sağlar. Bunun yapılması uygulamanızın bir raporda eylemi programlı bir şekilde gerçekleştirmesini ve kullanıcıların rapor içinde yaptığı eylemlerden olayları dinlemesini sağlar.

## Power BI JavaScript API’si ile neler yapabilirsiniz?
JavaScript API’si ile raporları yönetebilir, bir rapordaki sayfalarda gezinebilir, raporu filtreleyebilir ve katıştırma olaylarını gerçekleştirebilirsiniz. Aşağıdaki diyagramda API’nin yapısı gösterilmektedir.

![Power BI JavaScript API’si diyagramı](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### Raporları yönetme
Javascript API’si rapor ve sayfa düzeyindeki davranışı yönetmenizi sağlar:

- Belirli bir Power BI Raporunu uygulamanıza güvenli bir şekilde katıştırma - [katıştırma demo uygulamasını](http://azure-samples.github.io/powerbi-angular-client/#/scenario1) deneyin
  - Erişim belirteci ayarlama
- Raporu yapılandırma
  - Filtre bölmesini ve sayfa gezinti bölmesini etkinleştirme ve devre dışı bırakma - [ayar güncelleştirme demo uygulamasını](http://azure-samples.github.io/powerbi-angular-client/#/scenario6) deneyin
  - Sayfalar ve filtreler için varsayılanları ayarlama - [varsayılanları ayarlama demosunu](http://azure-samples.github.io/powerbi-angular-client/#/scenario5) deneyin
- Tam ekran moduna giriş ve çıkış

[Bir raporu katıştırma hakkında daha fazla bilgi edinin](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### Rapordaki sayfalarda gezinme
JavaScript API’si bir rapordaki tüm sayfaları bulmanızı ve geçerli sayfayı ayarlamanızı sağlar. [Gezinti demo uygulamasını](http://azure-samples.github.io/powerbi-angular-client/#/scenario3) deneyin.

[Sayfa gezintisi hakkında daha fazla bilgi edinin](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### Bir raporu filtreleme
JavaScript API’si katıştırılmış raporlar ve rapor sayfaları için temel ve gelişmiş filtreleme özellikleri sağlar. [Filtreleme demo uygulamasını](http://azure-samples.github.io/powerbi-angular-client/#/scenario4) deneyin ve giriş niteliğindeki bazı kodları burada gözden geçirin.  


#### Temel filtreler
Temel filtre bir sütuna veya hiyerarşi düzeyine yerleştirilir ve dahil edilecek ya da hariç tutulacak değerler listesini içerir.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### Gelişmiş filtreler
Gelişmiş filtreler AND veya OR mantıksal işlecini kullanır ve her biri kendi işlecine ve değerine sahip bir ya da iki koşulu kabul eder. Desteklenen koşullar şunlardır:

- None
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Contains
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Filtreleme hakkında daha fazla bilgi edinin](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### Olayları işleme
iframe’e bilgi göndermeye ek olarak uygulamanız iframe’den gelen aşağıdaki olaylara ilişkin bilgi de alabilir:

- Embed
  - loaded
  - error
- Reports
  - pageChanged
  - dataSelected (yakında)

[Olayları işleme hakkında daha fazla bilgi edinin](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## Sonraki adımlar
Power BI JavaScript API’si hakkında daha fazla bilgi için aşağıdaki bağlantılara göz atın:

- [JavaScript API Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Nesne modeli başvurusu](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Örnekler
  - [Angular](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Canlı tanıtım](https://microsoft.github.io/PowerBI-JavaScript/demo/)



<!--HONumber=Sep16_HO3-->


