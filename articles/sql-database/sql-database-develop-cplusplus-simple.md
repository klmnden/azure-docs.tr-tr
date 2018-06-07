---
title: C ve C++ kullanarak SQL veritabanına bağlama | Microsoft Docs
description: Örnek kod bu hızlı başlangıç modern C++ ile uygulama ve bulutta bir güçlü ilişkisel veritabanı Azure SQL Database tarafından yedeklenen oluşturmak için kullanın.
services: sql-database
author: edmacauley
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.devlang: cpp
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: edmacauley
ms.openlocfilehash: c37fdaa9f7aa2a0d243fe6cbc175060156967c61
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644707"
---
# <a name="connect-to-sql-database-using-c-and-c"></a>C ve C++ kullanarak SQL veritabanına bağlanma
Bu post Azure SQL Veritabanına bağlanmaya C ve C++ geliştiricileri en amaçlamaktadır. En iyi ilginize yakalar bölüme atlayabilirsiniz şekilde bölümlere ayrılmıştır. 

## <a name="prerequisites-for-the-cc-tutorial"></a>C/C++ öğreticisi için Önkoşullar
Aşağıdaki öğelere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* [Visual Studio](https://www.visualstudio.com/downloads/). Yapı ve bu örneği çalıştırmak için C++ dili bileşenleri yüklemeniz gerekir.
* [Visual Studio Linux geliştirme](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Linux'ta geliştiriyorsanız, Visual Studio Linux uzantısını yüklemeniz gerekir. 

## <a id="AzureSQL"></a>Azure SQL Database ve SQL Server sanal makineler
Azure SQL Microsoft SQL Server'da yerleşik olarak bulunur ve yüksek kullanılabilirlik, kullanıcı ve ölçeklenebilir hizmet sağlamak için tasarlanmıştır. Çalıştıran şirket içi, özel bir veritabanı üzerinde SQL Azure kullanarak çok sayıda avantajları vardır. SQL Azure ile yükleyin, ayarlamak, bakımını yapmak veya veritabanınızı ancak yalnızca içeriği ve veritabanınızın yapısını yönetmek gerekmez. Biz hata toleransı ve artıklık tüm yerleşiktir gibi veritabanlarıyla endişe hakkında tipik şey. 

Azure şu anda SQL server iş yüklerini barındırmak için iki seçenek vardır: Azure SQL veritabanı, veritabanı hizmeti ve SQL server üzerinde sanal makine (VM) olarak. Biz bu iki arasındaki farklar hakkında ayrıntılı bilgi içine Azure SQL veritabanı maliyet tasarrufu yararlanmak yeni bulut tabanlı uygulamalar için en iyi sonuç ve bulut Hizmetleri performans iyileştirmesinden dışında almazsınız. Geçiş veya şirket içi uygulamalarınızı buluta genişletme düşünüyorsanız, Azure sanal makinesinde SQL server sizin için daha iyi çıkışı çalışabilir. Örneği için bu makalenin basit tutmak için bir Azure SQL veritabanı oluşturalım. 

## <a id="ODBC"></a>Veri erişim teknolojileri: ODBC ve OLE DB
Azure SQL Veritabanına bağlanma farklı ve şu anda veritabanlarına bağlanmak için iki yolu vardır: ODBC (açık veritabanı bağlantısı) ve OLE DB (Nesne Bağlama ve Katıştırma veritabanı). Son yıllarda Microsoft ile hizalı [yerel ilişkisel veri erişimi için ODBC](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC görece basit ve ayrıca çok daha hızlı OLE DB ' dir. Burada yalnızca uyarısıyla ODBC eski bir C tarzı API kullanma ' dir. 

## <a id="Create"></a>1. adım: Azure SQL veritabanınızı oluşturma
Bkz: [sayfa Başlarken](sql-database-get-started-portal.md) bir örnek veritabanı oluşturmak konusunda bilgi almak için.  Alternatif olarak, bu izleyebilirsiniz [kısa iki dakikalık video](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) Azure portalını kullanarak bir Azure SQL veritabanı oluşturma.

## <a id="ConnectionString"></a>2. adım: Get bağlantı dizesi
Azure SQL veritabanınızı sağlandıktan sonra bağlantı bilgilerini belirlemek ve güvenlik duvarı erişim için istemci IP eklemek için aşağıdaki adımları gerçekleştirmek gerekir. 

İçinde [Azure portal](https://portal.azure.com/), Azure SQL veritabanınızı ODBC bağlantı dizesi kullanarak Git **veritabanı bağlantı dizelerini Göster** veritabanınız için genel bakış bölümünde bir parçası olarak listelenen: 

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

İçeriğini kopyalayın **ODBC (Node.js içerir) [SQL kimlik doğrulaması]** dize. Bu dize daha sonra bizim C++ ODBC komut satırı yorumlayıcı bağlanmak için kullanırız. Bu dize sürücü, sunucu ve diğer veritabanı bağlantı parametrelerini gibi ayrıntıları sağlar. 

## <a id="Firewall"></a>3. adım: IP Güvenlik Duvarı'na ekleyin.
Veritabanı sunucunuz için güvenlik duvarı bölümüne gidin ve ekleme, [istemci IP Güvenlik Duvarı'nda aşağıdaki adımları kullanarak](sql-database-configure-firewall-settings.md) biz başarılı bir bağlantı kurmak emin olmak için: 

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Bu noktada, Azure SQL DB yapılandırmış ve C++ kodunuzdan bağlanmaya hazır olursunuz. 

## <a id="Windows"></a>4. adım: bir Windows C/C++ uygulamasından bağlanma
Kolayca bağlanabilir, [Bu örneği kullanarak Windows üzerinde ODBC kullanarak Azure SQL DB](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) Visual Studio ile oluşturur. Örnek bizim Azure SQL Veritabanına bağlanmak için kullanılan bir ODBC komut satırı yorumlayıcı uygular. Bu örnek, bir veritabanı kaynağı adı (DSN) dosyasından komut satırı bağımsız değişkeni olarak ya da daha önce kopyaladığınız biz Azure portalından ayrıntılı bağlantı dizesini alır. Bu proje için özellik sayfası açmak ve aşağıda gösterildiği gibi bir komut bağımsız değişkeni bağlantı dizesini yapıştırın: 

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Bu veritabanı bağlantı dizesi bir parçası olarak veritabanınız için doğru kimlik doğrulama ayrıntıları sağladığınızdan emin olun. 

Uygulama oluşturma başlatın. Başarılı bir bağlantı doğrulama aşağıdaki pencere görmeniz gerekir. Bazı temel SQL komutları gibi bile çalıştırabilirsiniz **tablo oluşturma** veritabanı bağlantınızı doğrulamak için:

![SQL komutları](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Alternatif olarak, hiçbir komut bağımsız değişkenleri sağlandığında, başlatılan Sihirbazı'nı kullanarak bir DSN dosyası oluşturabilirsiniz. Bu seçeneği de deneyin öneririz. Otomasyon ve kimlik doğrulama ayarlarınızı koruma için bu DSN dosyası kullanabilirsiniz: 

![DSN dosyası oluşturma](./media/sql-database-develop-cplusplus-simple/datasource.png)

Tebrikler! Şimdi başarıyla C++ ve ODBC Windows kullanarak Azure SQL bağlı. Aynı işlemi Linux platformuna gerçekleştirmek için okumaya devam edebilirsiniz. 

## <a id="Linux"></a>5. adım: bir Linux C/C++ uygulamasından bağlanma
Haber henüz heard olmayan olasılığına Visual Studio şimdi C++ Linux uygulama geliştirmenize olanak sağlar. Bu yeni senaryoda hakkında bilgi edinebilirsiniz [Linux geliştirmesi için Visual C++](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blogu. Linux için derleme, Linux distro çalıştığı bir uzak makine gerekir. Yoksa, kullanılabilir bir hızlı şekilde kullanmaya ayarlayabileceğiniz [Linux Azure sanal makineleri](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Bu öğretici için bize ayarlanan bir Ubuntu 16.04 Linux dağıtım olduğunu varsayalım. Adımlar burada da Ubuntu 15.10, Red Hat 6 ve Red Hat 7 uygulamalıdır. 

Aşağıdaki adımları için distro SQL ve ODBC için gerekli kitaplıkları yükleyin:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Visual Studio'yu başlatın. Altında Araçlar -> Seçenekler -> platformlar arası bağlantı Yöneticisi ->, bir bağlantı Linux kutunuzun ekleyin: 

![Araçlar Seçenekler](./media/sql-database-develop-cplusplus-simple/tools.png)

SSH üzerinden bağlantı kurulduktan sonra boş bir proje (Linux) şablonu oluşturun: 

![Yeni Proje şablonu](./media/sql-database-develop-cplusplus-simple/template.png)

Daha sonra ekleyebilirsiniz bir [yeni C kaynak dosyası ve bu içerikle değiştirin](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). ODBC API SQLAllocHandle, SQLSetConnectAttr komutu ve SQLDriverConnect kullanarak başlatmak ve veritabanı bağlantısı olması gerekir. Gibi Windows ODBC örnekle, SQLDriverConnect çağrısı Azure portalından önceden kopyaladığınız, veritabanı bağlantı dizesi parametrelerinden ayrıntılarla değiştirmeniz gerekiyor. 

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Derleme eklemek için önce yapılacak son şey **odbc** kitaplığı bağımlılık olarak: 

![ODBC giriş kitaplık olarak ekleme](./media/sql-database-develop-cplusplus-simple/lib.png)

Uygulamanızı başlatmak için Linux konsolundan getirmek **hata ayıklama** menüsü: 

![Linux Konsolu](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Bağlantınızın başarılı olduysa, şimdi Linux konsolunda yazdırılan geçerli veritabanı adını görmeniz gerekir: 

![Linux konsol penceresi çıktısı](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Tebrikler! Öğreticiyi başarıyla tamamladınız ve şimdi, Azure SQL DB C++ içinden Windows ve Linux platformlarına bağlanabilir.

## <a id="GetSolution"></a>Tam C/C++ Öğreticisi çözümünü edinme
Github'da bu makaledeki tüm örnekleri içeren GetStarted çözümünü bulabilirsiniz:

* [ODBC C++ Windows örnek](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), Azure SQL bağlanmak için Microsoft C++ ODBC örneği indirin
* [ODBC C++ Linux örnek](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), Azure SQL ile bağlantı için Linux C++ ODBC örnek indirme

## <a name="next-steps"></a>Sonraki adımlar
* Gözden geçirme [SQL veritabanı geliştirme genel bakış](sql-database-develop-overview.md)
* Daha fazla bilgi [ODBC API Başvurusu](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL Veritabanının tüm özelliklerini](https://azure.microsoft.com/services/sql-database/) keşfedin

