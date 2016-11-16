---
title: "NodeJS’de Azure Search kullanmaya başlama | Microsoft Belgeleri"
description: "Programlama diliniz olarak NodeJS kullanarak, Azure&quot;da barındırılan bir bulut arama hizmeti üzerinde arama uygulaması derleme konusunu inceleyin."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 0625dc1b-9db6-40d5-ba9a-4738b75cbe19
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8a66c8f6079671b16c1c60467e6d458ed54be5af


---
# <a name="get-started-with-azure-search-in-nodejs"></a>NodeJS'de Azure Search kullanmaya başlama
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Arama deneyimi için Azure Search kullanan özel bir NodeJS arama uygulaması derlemeyi öğrenin. Bu öğretici, bu alıştırmada kullanılan nesneleri ve işlemleri oluşturmak için [Azure Search Hizmeti REST API'si](https://msdn.microsoft.com/library/dn798935.aspx)'ni kullanır.

Bu kodu geliştirmek ve test etmek için [NodeJS](https://nodejs.org) ve NPM, [Sublime Text 3](http://www.sublimetext.com/3) ve Windows 8.1'de Windows PowerShell kullandık.

Bu örneği çalıştırmak için, [Azure Portal](https://portal.azure.com)'da oturum açabileceğiniz bir Azure Search hizmetine sahip olmanız gerekir. Adım adım yönergeler için bkz. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md).

## <a name="about-the-data"></a>Veriler hakkında
Bu örnek uygulama, [Birleşik Devletler Jeoloji Hizmetleri (USGS)](http://geonames.usgs.gov/domestic/download_data.htm)'nin, veri kümesi boyutunu küçültmek için Rhode Island eyaletinde filtrelenen verilerini kullanır. Akarsular, göller ve zirveler gibi jeolojik özelliklerin yanı sıra, hastaneler ve okullar gibi önemli binaları döndüren bir arama uygulaması derlemek için bu verileri kullanacağız.

Bu uygulamada **DataIndexer** programı, filtrelenmiş USGS veri kümesini ortak bir Azure SQL Database'den alan bir [Oluşturucu](https://msdn.microsoft.com/library/azure/dn798918.aspx) yapısı kullanarak dizini derler ve yükler. Veri kaynağına yönelik kimlik bilgileri ve bağlantı bilgileri program kodu içinde sağlanır. Ek yapılandırma gerekli değildir.

> [!NOTE]
> Ücretsiz fiyatlandırma katmanının 10.000 belge limiti altında kalmak için, bu veri kümesi üzerinde filtre uyguladık. Standart katmanı kullanırsanız bu limit uygulanmaz. Her fiyatlandırma katmanının kapasitesi hakkında ayrıntılı bilgi için bkz: [Search hizmet limitleri](search-limits-quotas-capacity.md).
> 
> 

<a id="sub-2"></a>

## <a name="find-the-service-name-and-apikey-of-your-azure-search-service"></a>Azure Search hizmetinizin hizmet adını ve api anahtarını bulma
Hizmeti oluşturduktan sonra, URL'yi veya `api-key`'i almak için portala dönün. Search hizmetinize yönelik bağlantılar, çağrı kimliğini doğrulamak için hem URL hem de `api-key` sahibi olmanızı gerektirir.

1. [Azure Portal](https://portal.azure.com)'da oturum açın.
2. Atlama çubuğunda, aboneliğiniz için sağlanan tüm Azure Search hizmetlerini listelemek için **Search hizmeti**'ne tıklayın.
3. Kullanmak istediğiniz hizmeti seçin.
4. Hizmet panosunda, yönetici anahtarlarına erişim için anahtar simgesinin yanı sıra önemli bilgiler için kutucuklar görürsünüz.
   
      ![][3]
5. Hizmet URL'sini, bir yönetici anahtarını ve bir sorgu anahtarını kopyalayın. Üçüne de daha sonra, bunları config.js. dosyasına eklerken ihtiyacınız olacak.

## <a name="download-the-sample-files"></a>Örnek dosyalarını indirme
Örneği indirmek için aşağıdaki yaklaşımlardan birini kullanın.

1. [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo)'ya gidin.
2. **ZIP'i İndir**'e tıklayın, .zip dosyasını kaydedin ve ardından içerdiği tüm dosyaları ayıklayın.

Sonraki tüm dosya değişiklikleri ve çalıştırma deyimleri bu klasördeki dosyalara uygulanır.

## <a name="update-the-configjs-with-your-search-service-url-and-apikey"></a>Config.js dosyasını Search hizmetinizin URL'si ve api anahtarı ile güncelleştirme
Önceden kopyaladığınız URL'yi ve api anahtarını kullanarak yapılandırma dosyasında URL'yi, yönetici anahtarını ve sorgu anahtarını belirtin.

Yönetici anahtarları, dizin oluşturma ve silme ile belge yükleme de dahil olmak üzere hizmet işlemleri üzerinde tam denetim hakkı verir. Buna karşılık sorgu anahtarları, genellikle Azure Search'e bağlanan istemci uygulamaları tarafından kullanılan salt okunur işlemler içindir.

Sorgu anahtarını istemci uygulamalarında kullanma konusunda en iyi deneyimi pekiştirmek amacıyla, bu örneğe sorgu anahtarını dahil ediyoruz.

Aşağıdaki ekran görüntüsü, bir metin düzenleyicide açılan **config.js** dosyasını ilgili girişler çerçeve içine alınmış şekilde gösterir. Böylece, dosyanın hangi kısmını arama hizmetiniz için geçerli değerlerle güncelleştireceğinizi görebilirsiniz.

![][5]

## <a name="host-a-runtime-environment-for-the-sample"></a>Örnek için çalışma zamanı ortamı barındırma
Örnek, genel olarak npm kullanarak yükleyebileceğiniz bir HTTP sunucusu gerektirir.

Aşağıdaki komutlar için PowerShell penceresi kullanın.

1. **Package.json** dosyasını içeren klasöre gidin.
2. `npm install` yazın.
3. `npm install -g http-server` yazın.

## <a name="build-the-index-and-run-the-application"></a>Dizini derleme ve uygulamayı çalıştırma
1. `npm run indexDocuments` yazın.
2. `npm run build` yazın.
3. `npm run start_server` yazın.
4. Tarayıcınızı `http://localhost:8080/index.html` adresine yönlendirin

## <a name="search-on-usgs-data"></a>USGS verilerinde arama
USGS veri kümesi, Rhode Island eyaleti ile ilgili kayıtları içerir. Boş bir arama kutusunda **Search (Ara)** düğmesine tıklarsanız varsayılan seçenek olan ilk 50 girişi alırsınız.

Bir arama terimi girmeniz arama alt yapısına gitmesi gereken bir hedef verir. Bölgesel bir ad girmeyi deneyin. "Roger Williams", Rhode Island'ın ilk valisiydi. Çok sayıda parka, binaya ve okula onun adı verildi.

![][9]

Ayrıca, bu terimlerden herhangi birini de deneyebilirsiniz:

* Pawtucket
* Pembroke
* kaz + şapka

## <a name="next-steps"></a>Sonraki adımlar
Bu, NodeJS ve USGS veri kümesini temel alan ilk Azure Search öğreticisidir. Zaman içinde, özel çözümlerinizde kullanmak isteyebileceğiniz ek arama özellikleri göstermek için bu öğreticiyi genişleteceğiz.

Zaten Azure Search ile ilgili belirli bir altyapınız varsa öneri araçlarını (yazarken tamamlanan veya otomatik tamamlanan sorgular ), filtreleri ve çok yönlü gezinmeyi denemek için, bu örneği dayanak olarak kullanabilirsiniz. Ayrıca, numaralar ekleyerek ve belgeleri gruplayarak arama sonuçları sayfasını da geliştirebilirsiniz. Böylece, kullanıcılar sonuç sayfalarında gezinebilir.

Azure Search'ü ilk kez mi kullanıyorsunuz? Neler yapabileceğinizi anlamak için diğer öğreticileri denemenizi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. Daha fazla bilgiye erişmek için [Video ve Öğretici listemiz](search-video-demo-tutorial-list.md)'deki bağlantıları görüntüleyebilirsiniz.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png



<!--HONumber=Nov16_HO2-->


