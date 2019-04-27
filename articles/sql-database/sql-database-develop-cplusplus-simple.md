---
title: C ve C++ kullanarak SQL veritabanına bağlanma | Microsoft Docs
description: Örnek kod, bu hızlı başlangıçta C++ ile uygulama ve güçlü bir ilişkisel veritabanı bulutta Azure SQL veritabanı ile desteklenen modern oluşturmak için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: cpp
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/12/2018
ms.openlocfilehash: 00a3904bd78f3bb76266c726af28582770b23921
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60724089"
---
# <a name="connect-to-sql-database-using-c-and-c"></a>C ve C++ kullanarak SQL veritabanına bağlanma

Bu gönderinin C ve C++ geliştiricileri için Azure SQL DB bağlanmaya yöneliktir. En iyi ilgi yakalayan bölüme atlayabilirsiniz şekilde bölümlere ayrılmıştır.

## <a name="prerequisites-for-the-cc-tutorial"></a>C/C++ öğreticisi için Önkoşullar

Aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* [Visual Studio](https://www.visualstudio.com/downloads/). Derleme ve bu örneği çalıştırmak için C++ dil bileşenleri yüklemeniz gerekir.
* [Visual Studio Linux geliştirme](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Linux üzerinde geliştiriyorsanız Visual Studio Linux uzantısı da yüklemeniz gerekir.

## <a id="AzureSQL"></a>Azure SQL veritabanı ve SQL Server sanal makinelerinde
Azure SQL, Microsoft SQL Server'da yerleşik olarak bulunur ve yüksek kullanılabilirlik, yüksek performanslı ve ölçeklenebilir bir hizmet sağlamak üzere tasarlanmıştır. SQL Azure, şirket içinde çalışan kendi özel bir veritabanı üzerinde kullanmanın birçok avantajı vardır. SQL Azure ile yükleyin, ayarlama, sürdürmek veya veritabanınızı ancak yalnızca içeriği ve yapısı veritabanınızın yönetmek gerekmez. Biz hata toleransı ve yedeklilik tüm yerleşik gibi veritabanlarıyla endişe tipik şeyler.

Azure, şu anda SQL server iş yüklerini barındırmak için iki seçenek vardır: Azure SQL veritabanı, SQL server üzerinde sanal makineleri (VM) ve hizmet olarak veritabanı. Biz bu ikisi arasındaki farklar hakkında daha fazla ayrıntıya, en iyi sonucu maliyet tasarruflarından faydalanmak yeni bulut tabanlı uygulamalar için Azure SQL veritabanı olan ve bulut Hizmetleri performans iyileştirmesinden almazsınız. Geçiş veya şirket içi uygulamalarınızı buluta genişletme düşünüyorsanız, Azure sanal makinesinde SQL server için size daha iyi kullanıma işe yarayabilir. Bu makale için basit bir anlatım gözetildiği için bir Azure SQL veritabanı oluşturalım.

## <a id="ODBC"></a>Veri erişim teknolojileri: ODBC ve OLE DB
Azure SQL DB'ye bağlanmanın farklı değildir ve şu anda veritabanlarına bağlanmak için iki yolu vardır: ODBC (açık veritabanı bağlantısı) ve OLE DB (nesne bağlama ve katıştırma veritabanı). Son yıllarda Microsoft birlikte hizalanır [yerel ilişkisel veri erişimi için ODBC](https://blogs.msdn.microsoft.com/sqlnativeclient/20../../microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC görece basit ve ayrıca hızlıdır OLE DB ' dir. Burada yalnızca uyarı ODBC eski C stili API kullanmasıdır.

## <a id="Create"></a>1. adım:  Azure SQL veritabanınızı oluşturma
Bkz: [Başlarken sayfası](sql-database-single-database-get-started.md) örnek veritabanını oluşturma hakkında bilgi edinmek için.  Alternatif olarak, bu izleyebilirsiniz [kısa iki dakikalık video](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) Azure portalını kullanarak bir Azure SQL veritabanı oluşturmak için.

## <a id="ConnectionString"></a>2. adım:  Bağlantı dizesini alma
Azure SQL veritabanınızı sağlandıktan sonra bağlantı bilgilerini belirlemek ve istemci IP Güvenlik Duvarı erişim eklemek için aşağıdaki adımları gerçekleştirmek gerekir.

İçinde [Azure portalında](https://portal.azure.com/)ODBC bağlantı dizesi kullanarak Azure SQL veritabanına gidin **veritabanı bağlantı dizelerini Göster** veritabanınıza ilişkin genel bakış bölümünde bir parçası olarak listelenir:

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

İçeriğini kopyalayın **ODBC (Node.js içerir) [SQL kimlik doğrulaması]** dize. Bu dize daha sonra bizim C++ ODBC komut satırı yorumlayıcı bağlanmak için kullanırız. Bu dize, sürücü, sunucu ve diğer veritabanı bağlantı parametreleri gibi ayrıntıları sağlar.

## <a id="Firewall"></a>3. adım:  IP Güvenlik Duvarı ekleme
Veritabanı sunucunuz için güvenlik duvarı bölümüne gidin ve ekleme, [aşağıdaki adımları kullanarak güvenlik duvarı istemci IP'si](sql-database-configure-firewall-settings.md) biz başarılı bir bağlantı kurmak emin olmak için:

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Bu noktada, Azure SQL DB yapılandırdıktan ve C++ kodunuz içinden bağlanmaya hazırsınız.

## <a id="Windows"></a>4. adım: Bir Windows C/C++ uygulamasından bağlanma
Kolayca bağlanabilirsiniz, [ODBC bu örneğini kullanarak Windows üzerinde kullanarak Azure SQL DB](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) Visual Studio ile oluşturur. Örnek, Azure SQL Veritabanına bağlanmak için kullanılan bir ODBC komut satırı yorumlayıcı uygular. Bu örnek, bir veritabanı kaynak adı (DSN) dosyasından bir komut satırı bağımsız değişkeni olarak ya da biz daha önce Azure portaldan kopyaladığınız ayrıntılı bağlantı dizesini alır. Bu proje için özellik sayfası açmak ve burada gösterildiği gibi bir komut bağımsız değişkeni bağlantı dizesini yapıştırın:

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Veritabanı bağlantı dizesini bir parçası olarak veritabanınız için doğru kimlik doğrulama ayrıntıları sağladığınızdan emin olun.

Derlemek için uygulamayı başlatın. Doğrulama başarılı bir bağlantı aşağıdaki pencere görmeniz gerekir. Hatta gibi bazı temel SQL komutlarını çalıştırabilirsiniz **tablosu oluşturma** , veritabanı bağlantısını doğrulamak için:

![SQL Komutları](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Alternatif olarak, komut satırı bağımsız değişkenlerini sağlandığında, başlatılan Sihirbazı'nı kullanarak bir DSN dosyası oluşturabilirsiniz. Bu seçenek de deneyin öneririz. Otomasyon ve kimlik doğrulama ayarlarınızı korumak için bu DSN dosyası kullanabilirsiniz:

![DSN dosyası oluşturma](./media/sql-database-develop-cplusplus-simple/datasource.png)

Tebrikler! Şimdi başarıyla Windows üzerinde C++ ve ODBC kullanarak Azure SQL bağlı. Linux platformu için de aynısını yapın için okumaya devam edebilirsiniz.

## <a id="Linux"></a>5. adım: Bir Linux C/C++ uygulamasından bağlanma
Haber henüz heard kullanmadığınız durumda Visual Studio artık, C++ Linux uygulama geliştirmenize olanak tanır. Bu yeni senaryoda okuyabilirsiniz [Linux geliştirme için Visual C++](https://blogs.msdn.microsoft.com/vcblog/20../../visual-c-for-linux-development/) blogu. Linux için derleme, Linux distro çalıştığı bir uzak makine gerekir. Yoksa, kullanılabilir bir kolayca kullanarak ayarlayabilirsiniz [Linux Azure sanal makineleri](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu öğretici için bize ayarlanmış bir Ubuntu 16.04 Linux dağıtım sahip olduğunuzu varsaymaktadır. Buradaki adımları da 15.10 Ubuntu, Red Hat 6 ve Red Hat 7'yi uygulamanız gerekir.

Aşağıdaki adımlarda, SQL ve ODBC için dağıtım için gerekli kitaplıkları yükleyin:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Visual Studio'yu başlatın. Altında Araçlar -> Seçenekler -> Çoklu Platform -> Bağlantı Yöneticisi, bir bağlantı için Linux box'ınızı ekleyin:

![Araçlar Seçenekler](./media/sql-database-develop-cplusplus-simple/tools.png)

SSH üzerinden bağlantı kurulduktan sonra bir boş proje (Linux) şablonu oluşturun:

![Yeni Proje şablonu](./media/sql-database-develop-cplusplus-simple/template.png)

Daha sonra ekleyebilirsiniz bir [yeni C kaynak dosyası ve söz konusu içeriklerle değiştirin](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). ODBC API SQLAllocHandle, SQLSetConnectAttr komutu ve SQLDriverConnect kullanarak başlatmak ve bir veritabanı bağlantısı olması gerekir.
Gibi Windows ODBC örnekle SQLDriverConnect çağrı ayrıntılarını Azure portaldan daha önce kopyaladığınız kendi veritabanı bağlantı dizesi parametreleri ile değiştirmeniz gerekir.

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Derleme eklemek için önce yapılacak son şey **odbc** kitaplığı bağımlılık olarak:

![ODBC bir giriş kitaplık olarak ekleme](./media/sql-database-develop-cplusplus-simple/lib.png)

Uygulamanızı başlatmak için Linux konsoldan getirmek **hata ayıklama** menüsü:

![Linux Konsolu](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Bağlantınız başarılı olursa Linux konsolunda yazdırılan geçerli veritabanı adı artık görmeniz gerekir:

![Linux konsol penceresi çıktısı](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Tebrikler! Öğreticiyi tamamladınız ve artık Azure SQL DB için C++'dan Windows ve Linux platformlarında bağlanabilirsiniz.

## <a id="GetSolution"></a>C/C++ Öğreticisi tam çözümünü edinme
Github'da bu makaledeki tüm örnekleri içeren GetStarted çözümünü bulabilirsiniz:

* [ODBC C++ Windows örnek](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), Azure SQL'e bağlanmak için Windows C++ ODBC örneği indirin
* [ODBC C++ Linux örnek](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), Azure SQL'e bağlanmak için Linux C++ ODBC örneği indirin

## <a name="next-steps"></a>Sonraki adımlar
* Gözden geçirme [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md)
* Daha fazla bilgi [ODBC API Başvurusu](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL Veritabanının tüm özelliklerini](https://azure.microsoft.com/services/sql-database/) keşfedin

