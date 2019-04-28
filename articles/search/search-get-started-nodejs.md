---
title: Azure Search'te Node.js - Azure Search ile çalışmaya başlama
description: Programlama diliniz olarak Node.js kullanarak, Azure'da barındırılan bir bulut arama hizmeti üzerinde arama uygulaması derleme konusunu inceleyin.
author: jj09
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/26/2017
ms.author: jjed
ms.custom: seodec2018
ms.openlocfilehash: 1b37b3c52abd3750c3452a46bdf5b0c5954de4dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61289213"
---
# <a name="get-started-with-azure-search-in-nodejs"></a>Node.js'de Azure Search kullanmaya başlama
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Arama deneyimi için Azure Search kullanan özel bir Node.js arama uygulaması derlemeyi öğrenin. Bu öğretici, bu alıştırmada kullanılan nesneleri ve işlemleri oluşturmak için [Azure Search Hizmeti REST API'si](https://msdn.microsoft.com/library/dn798935.aspx)'ni kullanır.

Bu kodu geliştirmek ve test etmek için [Node.js](https://Nodejs.org) ve NPM, [Sublime Text 3](https://www.sublimetext.com/3) ve Windows 8.1'de Windows PowerShell kullandık.

Bu örneği çalıştırmak için, [Azure portalında](https://portal.azure.com) oturum açabileceğiniz bir Azure Search hizmetine sahip olmanız gerekir. Adım adım yönergeler için bkz. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md).

## <a name="about-the-data"></a>Veriler hakkında
Bu örnek uygulama, [Birleşik Devletler Jeoloji Hizmetleri (USGS)](https://geonames.usgs.gov/domestic/download_data.htm)'nin, veri kümesi boyutunu küçültmek için Rhode Island eyaletinde filtrelenen verilerini kullanır. Akarsular, göller ve zirveler gibi jeolojik özelliklerin yanı sıra, hastaneler ve okullar gibi önemli binaları döndüren bir arama uygulaması derlemek için bu verileri kullanacağız.

Bu uygulamada **DataIndexer** programı, filtrelenmiş USGS veri kümesini ortak bir Azure SQL Veritabanı'den alan bir [Oluşturucu](https://msdn.microsoft.com/library/azure/dn798918.aspx) yapısı kullanarak dizini derler ve yükler. Veri kaynağına yönelik kimlik bilgileri ve bağlantı bilgileri program kodu içinde sağlanır. Ek yapılandırma gerekli değildir.

> [!NOTE]
> Ücretsiz fiyatlandırma katmanının 10.000 belge limiti altında kalmak için, bu veri kümesi üzerinde filtre uyguladık. Standart katmanı kullanırsanız bu limit uygulanmaz. Her fiyatlandırma katmanının kapasitesi hakkında ayrıntılı bilgi için bkz: [Search hizmet limitleri](search-limits-quotas-capacity.md).
> 
> 

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Azure Search hizmetinizin hizmet adını ve api anahtarını bulma
Hizmeti oluşturduktan sonra, URL'yi veya `api-key`'i almak için portala dönün. Search hizmetinize yönelik bağlantılar, çağrı kimliğini doğrulamak için hem URL hem de `api-key` sahibi olmanızı gerektirir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Atlama çubuğunda, aboneliğiniz için sağlanan tüm Azure Search hizmetlerini listelemek için **Search hizmeti**'ne tıklayın.
3. Kullanmak istediğiniz hizmeti seçin.
4. Hizmet panosunda, yönetici anahtarlarına erişim için anahtar simgesi gibi önemli bilgiler için kutucuklar görürsünüz.
5. Hizmet URL'sini, bir yönetici anahtarını ve bir sorgu anahtarını kopyalayın. Üçüne de daha sonra, bunları config.js. dosyasına eklerken ihtiyacınız olur.

## <a name="download-the-sample-files"></a>Örnek dosyalarını indirme
Örneği indirmek için aşağıdaki yaklaşımlardan birini kullanın.

1. [search-node-indexer-demo](https://github.com/Azure-Samples/search-node-indexer-demo) öğesine gidin.
2. **ZIP'i İndir**'e tıklayın, .zip dosyasını kaydedin ve ardından içerdiği tüm dosyaları ayıklayın.

Sonraki tüm dosya değişiklikleri ve çalıştırma deyimleri bu klasördeki dosyalara uygulanır.

## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Config.js dosyasını Search hizmetinizin URL'si ve api anahtarı ile güncelleştirme
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
USGS veri kümesi, Rhode Island eyaleti ile ilgili kayıtları içerir. Boş bir arama kutusunda **Ara** düğmesine tıklarsanız varsayılan seçenek olan ilk 50 girişi alırsınız.

Bir arama terimi girmeniz arama alt yapısına gitmesi gereken bir hedef verir. Bölgesel bir ad girmeyi deneyin. "Roger Williams", Rhode Island'ın ilk valisiydi. Çok sayıda parka, binaya ve okula onun adı verildi.

![][9]

Ayrıca, bu terimlerden herhangi birini de deneyebilirsiniz:

* Pawtucket
* Pembroke
* kaz + şapka

## <a name="next-steps"></a>Sonraki adımlar
Bu, Node.js ve USGS veri kümesini temel alan ilk Azure Search öğreticisidir. Zaman içinde, özel çözümlerinizde kullanmak isteyebileceğiniz ek arama özellikleri göstermek için bu öğreticiyi genişleteceğiz.

Zaten Azure Search ile ilgili belirli bir altyapınız varsa öneri araçlarını (yazarken tamamlanan veya otomatik tamamlanan sorgular ), filtreleri ve çok yönlü gezinmeyi denemek için, bu örneği dayanak olarak kullanabilirsiniz. Ayrıca, numaralar ekleyerek ve belgeleri gruplayarak arama sonuçları sayfasını da geliştirebilirsiniz. Böylece, kullanıcılar sonuç sayfalarında gezinebilir.

Azure Search'ü ilk kez mi kullanıyorsunuz? Neler yapabileceğinizi anlamak için diğer öğreticileri denemenizi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. 

<!--Image references-->
[1]: ./media/search-get-started-Nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-Nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-Nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-Nodejs/AzSearch-Nodejs-configjs.png
[9]: ./media/search-get-started-Nodejs/rogerwilliamsschool.png
