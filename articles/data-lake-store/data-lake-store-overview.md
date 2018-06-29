---
title: Azure Data Lake Storage Gen1 genel bakış | Microsoft Docs
description: Hangi veri Gölü depolama Gen1'ni (daha önce Azure Data Lake Store bilinir) ve diğer veri depolarına kıyasla sağladığı değeri anlama
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 4dff8f4ff9fc324d48391c0399677b64824493c6
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036454"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1 genel bakış

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Store, büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alım hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

> [!TIP]
> Azure Data Lake Store hizmetini keşfetmeye başlamak için [Data Lake Store öğrenme yolunu](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) kullanın.
> 
> 

Azure Data Lake Store'a WebHDFS ile uyumlu REST API'leri kullanılarak Hadoop'tan (HDInsight kümesiyle kullanılabilir) erişilebilir. Bu, depolanan veriler üzerinde analizi olanaklı kılmak için özel olarak tasarlanmış ve veri analizi senaryolarına yönelik performans için ayarlanmıştır. Sağlandığı şekilde, gerçek kurumsal kullanım vakaları için temel önem taşıyan güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik gibi tüm kurumsal düzeydeki özellikleri içerir.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Azure Data Lake'in temel işlevleri arasında aşağıdakiler yer alır.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir
Azure Data Lake deposu, Hadoop Dağıtılmış Dosya Sistemi (HDFS) ile uyumlu olan ve Hadoop ekosistemi ile çalışan bir Apache Hadoop dosya sistemidir.  WebHDFS API'sini kullanan var olan HDInsight uygulamalarınız veya hizmetleriniz, Data Lake Store ile kolayca tümleştirilebilir. Ayrıca, Data Lake Store uygulamalar için WebHDFS ile uyumlu bir REST arabirimini kullanıma sunar.

Data Lake Store içinde depolanan veriler, MapReduce veya Hive gibi Hadoop analitik çerçeveler kullanılarak kolayca çözümlenebilir. Microsoft Azure HDInsight kümeleri, Data Lake Store'da depolanan verilere doğrudan erişmek üzere sağlanabilir ve yapılandırılabilir.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar
Azure Data Lake Store, sınırsız depolama sağlar ve analiz için çeşitli verilerin depolanmasına uygundur. Hesap boyutları, dosya boyutları veya bir veri gölü içinde depolanabilen veri miktarı için herhangi bir sınırlama uygulamaz. Ayrı dosyalar kilobayttan petabayta kadar uzanan boyutlarda olabilir ve bu da herhangi bir tür verinin depolanması için harika bir seçim olmasını sağlar. Birden çok kopya oluşturularak veriler sağlam bir şekilde depolanır ve verilerin data lake içinde depolanmasına yönelik bir süre sınırı mevcut değildir.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır
Azure Data Lake Store, büyük miktarlarda verilerin sorgulanması ve çözümlenmesi için çok büyük verim gerektiren büyük ölçekli analitik sistemlerin çalıştırılması için geliştirilmiştir. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal kullanıma hazırdır: Yüksek kullanılabilirliğe sahip ve güvenlidir
Azure Data Lake Store, endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır. Kuruluşlar Azure Data Lake'i kendi çözümlerinde, var olan bir veri platformunun önemli bir parçası olarak kullanabilir.

Data Lake Store ayrıca, depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için bkz. [Azure Data Lake Store'da verilerin güvenliğini sağlama](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tüm Veriler
Azure Data Lake Store tüm verileri yerel biçiminde, olduğu gibi ve önceden yapılması gereken herhangi bir dönüştürme işlemi gerektirmeden depolayabilir. Data Lake Store, veriler yüklenmeden önce bir şema tanımlanmasını gerektirmez ve böylece, verilerin yorumlanmasını ve şema tanımlanmasını analiz sırasında ayrı analitik çerçeveye bırakır. İsteğe bağlı boyut ve biçimlerdeki dosyaların depolanabilmesi, Data Lake Store'un yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemesini mümkün kılar.

Verilere yönelik Azure Data Lake Store kapsayıcıları esasen klasör ve dosyadır. SDK'ları, Azure Portal'ı ve Azure PowerShell'i kullanarak depolanan veriler üzerinde işlem yaparsınız. Verilerinizi bu arabirimleri kullanarak depoya yerleştirdiğiniz ve ilgili kapsayıcıları kullandığınız sürece, istediğiniz türde veriyi depolayabilirsiniz. Data Lake Store, depoladığı verilerin türüne göre herhangi bir özel veri işleme işlemi gerçekleştirmez.

## <a name="DataLakeStoreSecurity"></a>Azure Data Lake Store'da verilerin güvenliğini sağlama
Azure Data Lake Store, verilerinize erişimi yönetmek üzere kimlik doğrulaması ve erişim denetim listeleri (ACL'ler) için Azure Active Directory'yi kullanır.

| Özellik | Açıklama |
| --- | --- |
| Kimlik Doğrulaması |Azure Data Lake Store, Azure Data Lake Store içinde depolanan tüm verilere yönelik kimlik ve erişim yönetimi için Azure Active Directory (AAD) olanağıyla tümleştirilir. Tümleştirme sonucunda Azure Data Lake, çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımını izleme, güvenlik izlemesi ve uyarısı vb. özellikler de dahil olmak üzere tüm AAD özelliklerinden faydalanır. Azure Data Lake Store, REST arabirimi içinde kimlik doğrulaması için OAuth 2.0 protokolünü destekler. Bkz. [Data Lake Store kimlik doğrulaması](data-lakes-store-authentication-using-azure-active-directory.md)|
| Erişim denetimi |Azure Data Lake Store, WebHDFS protokolünün kullanıma sunduğu POSIX tipi izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. Data Lake Store bağlanımda ACL’lerin nasıl çalıştığı üzerine daha fazla bilgi için bkz. [Data Lake Store’da erişim denetimi](data-lake-store-access-control.md). |
| Şifreleme |Data Lake Store, hesapta depolanan veriler için şifreleme özelliği de sağlar. Şifreleme ayarlarını bir Data Lake Store hesabı oluşturduğunuz sırada siz belirlersiniz. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Daha fazla bilgi için bkz. [Data Lake Store'da şifreleme](data-lake-store-encryption.md). Şifreleme tabanlı yapılandırma sağlama talimatları için bkz. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md). |

Data Lake Store'da verilerin güvenliği sağlama konusunda daha fazla bilgi edinmek mi istiyorsunuz? Aşağıdaki bağlantıları izleyin.

* Data Lake Store'da verilerin güvenliğini sağlamaya yönelik talimatlar için, bkz. [Azure Data Lake Store'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md).
* Videoyu mu tercih ediyorsunuz? Data Lake Store'da depolanan verilerin güvenliğini sağlama konulu [bu videoyu izleyin](https://mix.office.com/watch/1q2mgzh9nn5lx).

## <a name="applications-compatible-with-azure-data-lake-store"></a>Azure Data Lake Store ile uyumlu uygulamalar
Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir. Bu da Data Lake Store'u veri depolama ihtiyaçlarınız için ideal bir seçenek yapar. Data Lake Store'un hem açık kaynak bileşenlerle hem de diğer Azure hizmetleriyle nasıl kullanılabileceği konusunda daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Data Lake Store ile birlikte çalışabilen açık kaynak uygulamaların listesi için bkz. [Azure Data Lake Store ile uyumlu uygulamalar ve hizmetler](data-lake-store-compatible-oss-other-applications.md).
* Daha geniş bir senaryo aralığını mümkün kılmak üzere Data Lake Store'un diğer Azure hizmetleriyle nasıl kullanılabileceğini anlamak için bkz. [Diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md).
* Data Lake Store'u veri alma, veri işleme, veri indirme ve veri görselleştirme gibi senaryolarda nasıl kullanacağınızı öğrenmek için bkz. [Data Lake Store'u kullanmaya yönelik senaryolar](data-lake-store-data-scenarios.md).

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Azure Data Lake Store dosya sistemi (adl://) nedir? 
Data Lake Store'a, Hadoop ortamlarında (HDInsight kümesinde kullanılabilir) yeni dosya sistemi olan AzureDataLakeFilesystem (adl://) üzerinden erişilebilir. adl:// kullanan uygulamalar ve hizmetler, geçerli olarak WebHDFS için kullanılabilir olmayan ek performans iyileştirmelerinden faydalanabilir. Sonuç olarak, Data Lake Store size, önerilen seçenek olan adl:// kullanımı ile en iyi performansı yakalamayı veya doğrudan WebHDFS API'yi kullanmaya devam ederek var olan kodu koruma esnekliği sağlar. Azure HDInsight, Data Lake Store üzerinde en iyi performansı sağlamak üzere AzureDataLakeFilesystem'ı bütünüyle kullanır.

Data Lake Store'daki verilerinize `adl://<data_lake_store_name>.azuredatalakestore.net` kullanarak erişebilirsiniz. Data Lake Store'daki verilere nasıl erişileceği konusunda daha fazla bilgi için bkz. [Depolanan verilerin özelliklerini görüntüleme](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portalı'nı kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [.NET SDK'yı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-net-sdk.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)