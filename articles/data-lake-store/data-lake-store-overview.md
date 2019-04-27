---
title: Azure Data Lake depolama Gen1 nedir? | Microsoft Docs
description: Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) ve diğer veri depolarına kıyasla sağladığı değeri genel bakış
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 518c129aedf3161ab761d09139e0c4d988dd2cbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60885571"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 nedir?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake depolama Gen1 bir büyük veri analizi iş yükleri için kuruluş çapında hiper ölçekli depodur. Azure Data Lake, işletimsel ve keşifsel analiz için herhangi bir boyuta, türe ve alma hızına sahip olan verileri tek bir konumda yakalamanıza olanak sağlar.

Data Lake depolama Gen1 hadoop'tan (HDInsight kümesiyle kullanılabilir) erişilebilir WebHDFS ile uyumlu REST API'lerini kullanarak. Bu depolanan veriler üzerinde analiz sağlamak için tasarlanmıştır ve veri analizi senaryoları için performans için ayarlanmıştır. Data Lake depolama Gen1 tüm kurumsal düzeydeki özellikleri içerir: güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Temel işlevler

Data Lake depolama Gen1 anahtar özelliklerinin bazıları aşağıda verilmiştir.

### <a name="built-for-hadoop"></a>Hadoop için geliştirilmiştir

Data Lake depolama Gen1 Hadoop dağıtılmış dosya sistemi (HDFS) ile uyumlu olan ve Hadoop ekosistemiyle çalışan bir Apache Hadoop dosya sistemidir. Var olan HDInsight uygulamaları veya WebHDFS API'sini kullanan hizmetler, Data Lake depolama Gen1 ile kolayca tümleştirilebilir. Data Lake depolama Gen1 ayrıca bir uygulamalar için WebHDFS ile uyumlu REST arabirimini kullanıma sunar.

Data Lake depolama Gen1 içinde depolanan verileri kolayca çözümleyebilirsiniz MapReduce veya Hive gibi Hadoop analitik çerçeveler kullanılarak. Azure HDInsight kümeleri sağlama ve bunları doğrudan Data Lake depolama Gen1 içinde depolanan verilere erişmek için yapılandırabilirsiniz.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake depolama Gen1 sınırsız depolama sağlar ve verileri analiz için çeşitli depolayabilirsiniz. Bu hesap boyutları, dosya boyutları veya bir veri gölü içinde depolanabilen veri miktarı herhangi bir sınır koymaz. Tek tek dosyaları kilobayttan petabayta kadar boyutu değişebilir. Birden çok kopya oluşturularak veriler arızaya depolanır. Data lake içinde veri depolanabilir süre sınırı yoktur.

### <a name="performance-tuned-for-big-data-analytics"></a>Büyük veri analizi için performans ayarı yapılmıştır

Data Lake depolama Gen1 sorgulanması ve büyük miktarlarda verinin çözümlenmesi için devasa bir işlem hacmi gerektiren büyük ölçekli analiz sistemlerini çalıştırmak üzere tasarlanmıştır. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusu üzerinde dağıtır. Bu, veri analizinin gerçekleştirilmesi için dosyanın paralel olarak okunması sırasında okuma verimini artırır.

### <a name="enterprise-ready-highly-available-and-secure"></a>Kurumsal kullanıma hazır: Yüksek oranda kullanılabilir ve güvenli

Data Lake depolama Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıklarınız, herhangi bir beklenmeyen arızaya karşı koruma sağlamak üzere yedekli kopyaların oluşturulmasıyla sağlam bir şekilde depolanır.

Data Lake depolama Gen1 ayrıca depolanan veriler için kurumsal düzeyde güvenlik sağlar. Daha fazla bilgi için [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tüm veriler

Data Lake depolama Gen1 önceki dönüştürmeler gerek kalmadan kendi yerel biçiminde herhangi bir veri depolayabilirsiniz. Data Lake depolama Gen1, veriler yüklenmeden önce tanımlanacak bir şema kadar verilerin yorumlanmasını ve şema analiz zamanında tanımlamak için ayrı analitik çerçeveye bırakır gerektirmez. İsteğe bağlı boyut ve biçimlerdeki dosyaların depolama olanağı, yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işlemek Data Lake depolama Gen1 için mümkün kılar.

Veriler için Data Lake depolama Gen1 kapsayıcılar olan temelde klasörleri ve dosyaları. SDK'ları, Azure portalı ve Azure PowerShell'i kullanarak depolanan veriler üzerinde çalışır. Verilerinizi bu arabirimleri kullanarak depoya yerleştirdiğiniz ve ilgili kapsayıcıları kullanarak, her türden veriyi depolayabilirsiniz. Data Lake depolama Gen1, depoladığı verilerin türüne göre veri herhangi bir özel işlem gerçekleştirmez.

## <a name="DataLakeStoreSecurity"></a>Verilerin güvenliğini sağlama

Data Lake depolama Gen1 kullanan Azure Active Directory (Azure AD) kimlik doğrulaması ve erişim için verilerinizi listeleri (ACL) erişimi yönetmek üzere denetler.

| Özellik | Açıklama |
| --- | --- |
| Kimlik Doğrulaması |Data Lake depolama Gen1 Data Lake depolama Gen1 içinde depolanan tüm veriler için kimlik ve erişim yönetimi için Azure AD ile tümleştirilir. Data Lake depolama Gen1 avantajlar tüm Azure AD tümleştirme nedeniyle çok faktörlü kimlik doğrulaması, koşullu erişim, rol tabanlı erişim denetimi, uygulama kullanımını izleme, güvenlik izleme ve uyarı verme gibi özellik ve benzeri. Data Lake depolama Gen1 REST arabirimi içinde kimlik doğrulaması için OAuth 2.0 protokolünü destekler. Bkz: [Data Lake depolama Gen1 kimlik doğrulaması](data-lakes-store-authentication-using-azure-active-directory.md).|
| Erişim denetimi |Data Lake depolama Gen1 WebHDFS protokolünün kullanıma sunulan POSIX stili izinleri destekleyerek erişim denetimi sağlar. ACL'ler Kök klasörde, alt klasörler ve dosyaları tek tek etkinleştirebilirsiniz. ACL'ler Data Lake depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1'deki erişim denetimi](data-lake-store-access-control.md). |
| Şifreleme |Data Lake depolama Gen1 ayrıca hesapta depolanan veriler için şifreleme sağlar. Bir Data Lake depolama Gen1 hesabı oluşturulurken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini tercih ya da şifrelemeyi kabul seçebilirsiniz. Daha fazla bilgi için [şifreleme Data Lake depolama Gen1](data-lake-store-encryption.md). Şifreleme tabanlı yapılandırma sağlama konusunda yönergeler için bkz. [Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md). |

Data Lake depolama Gen1 güvenli verilerde konusunda yönergeler için bkz: [Azure Data Lake depolama Gen1 verileri güvenli hale getirme](data-lake-store-secure-data.md).

## <a name="application-compatibility"></a>Uygulama uyumluluğu

Data Lake depolama Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşenler ile uyumludur. Ayrıca, diğer Azure hizmetleriyle de tümleşir. Data Lake depolama Gen1 açık kaynak bileşenler ile nasıl kullanabileceğiniz hakkında daha fazla bilgi ve diğer Azure hizmetleri öğrenmek için aşağıdaki bağlantıları kullanın:

- Bkz: [uygulamaların ve hizmetlerin Azure Data Lake depolama Gen1 ile uyumlu](data-lake-store-compatible-oss-other-applications.md) açık kaynaklı uygulamaları Data Lake depolama Gen1 ile birlikte çalışabilen bir listesi.
- Bkz: [diğer Azure hizmetleriyle tümleştirme](data-lake-store-integrate-with-other-services.md) Data Lake depolama Gen1 diğer Azure hizmetleriyle geniş bir senaryoları etkinleştirmek için nasıl kullanılacağını anlamak için.
- Bkz: [Data Lake depolama Gen1 kullanma senaryoları](data-lake-store-data-scenarios.md) veri alma, veri işleme, veri indirme ve veri görselleştirme gibi senaryolarda Data Lake depolama Gen1 kullanmayı öğrenin.

## <a name="data-lake-storage-gen1-file-system"></a>Data Lake depolama Gen1 dosya sistemi

Data Lake depolama Gen1 AzureDataLakeFilesystem dosya sistemi erişilebilir (adl: / /) Hadoop ortamlarında (HDInsight kümesiyle kullanılabilir). Uygulamaları ve hizmetleri kullanan adl: / / daha fazla geçerli olarak WebHDFS şu anda kullanılamayan performans iyileştirmelerini avantajlarından yararlanabilirsiniz. Sonuç olarak, Data Lake depolama Gen1 sağlar ya da esnekliği yaptığınız adl kullanmanın önerilen seçenek ile en iyi performansı kullanın: / / veya doğrudan WebHDFS API'yi kullanmaya devam ederek var olan kod güncelleştirin. Azure HDInsight, Data Lake depolama Gen1 üzerinde en iyi performansı sağlamak üzere AzureDataLakeFilesystem tam olarak yararlanır.

Data Lake depolama Gen1 kullanarak verilerinize erişebilirsiniz `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Data Lake depolama Gen1 verilere erişim hakkında daha fazla bilgi için bkz: [depolanan verilerin özelliklerini görüntüleme](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Sonraki adımlar

- [Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md)
- [Data Lake depolama Gen1 ile çalışmaya başlama .NET SDK'sını kullanma](data-lake-store-get-started-net-sdk.md)
- [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)