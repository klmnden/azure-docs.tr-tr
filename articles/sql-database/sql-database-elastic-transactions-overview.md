---
title: Bulut veritabanlarında dağıtılmış işlemler
description: Azure SQL Database esnek veritabanı işlemleri genel bakış
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.custom: scale out apps
ms.workload: On Demand
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: c0dfc8e2b71e0d81943ef8306c58421ff1d78c6c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646958"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Bulut veritabanlarında dağıtılmış işlemler
Esnek veritabanı işlemleri için Azure SQL veritabanı (SQL DB), SQL DB birkaç veritabanlarında span işlemleri çalıştırmanızı sağlar. Esnek veritabanı işlemleri için SQL DB ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve tanıdık programlama deneyimi kullanarak ile tümleştirmek [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) sınıfları. Kitaplık almak için bkz: [.NET Framework 4.6.1 (Web Yükleyicisi)](https://www.microsoft.com/download/details.aspx?id=49981).

Şirket içinde böyle bir senaryo, Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) çalıştıran genellikle gereklidir. MSDTC Azure hizmet olarak Platform uygulama için kullanılabilir olmadığından, dağıtılmış işlemleri koordine yeteneği artık SQL Veritabanına doğrudan tümleştirilmiştir. Uygulamaları dağıtılmış işlemler başlatmak için bir SQL veritabanına bağlanabilir ve veritabanlarından birini saydam dağıtılmış işlem aşağıdaki resimde gösterildiği gibi koordine. 

  ![Dağıtılmış işlemler Azure SQL Database esnek veritabanı işlemleri kullanma ][1]

## <a name="common-scenarios"></a>Genel senaryolar
Esnek veritabanı işlemleri için SQL DB uygulamaların birkaç farklı SQL veritabanlarında depolanan verileri atomik değişiklik yapmasına olanak sağlar. C# ve .NET istemci-tarafı geliştirme deneyimlerinde Önizleme odaklanır. T-SQL kullanarak bir sunucu tarafı deneyimi daha sonraki bir zamana planlanmaktadır.  
Esnek veritabanı işlemleri, aşağıdaki senaryolarda hedefler:

* Azure çok veritabanı uygulamalarında: farklı veri türleri farklı veritabanlarında bulunan şekilde bu senaryoyla veri dikey SQL DB birkaç veritabanlarında genelinde bölümlenmiş. Bazı işlemler iki veya daha fazla veritabanlarında tutulur verilerde yapılan değişiklikleri gerektirir. Uygulama esnek veritabanı işlemleri değişiklikleri veritabanları arasında koordine etmek ve kararlılık sağlamak için kullanır.
* Azure parçalı veritabanı uygulamalarında: Bu senaryo ile veri katmanı kullanan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) ya da yatay SQL DB içinde birçok veritabanı arasında veri bölümlemek için self-parçalama. Bir belirgin kullanım değişiklikleri kiracılar span zaman parçalı bir çok kiracılı uygulama için atomik değişiklikleri gerçekleştirmek için gereken bir durumdur. Örneği için bir kiracı bir aktarımı hem bulunan farklı veritabanlarını başka bir düşünün. Hangi sırayla genellikle aynı Kiracı için kullanılan birkaç veritabanları arasında uzatmak bazı atomik işlemleri gerektiği anlamına gelir büyük bir kiracı için kapasite ihtiyaçlarını karşılamak için hassas parçalama buna ikinci bir durumdur. Veritabanları arasında çoğaltılan verilere başvurmak için atomik güncelleştirmeleri buna üçüncü bir durumdur. Atomik, hizmetteki, işlem bu satırları boyunca önizlemeyi kullanarak birçok veritabanı arasında şimdi Eşgüdümlü.
  Esnek veritabanı işlemleri iki aşamalı yürütme veritabanları arasında hareket kararlılık sağlamak için kullanın. Tek bir işlem içinde her defasında 100'den az veritabanları gerektiren işlemler için iyi bir tercihtir olur. Bu sınırlamaları uygulanmaz ancak bir performans ve başarı oranları bu sınırları aşıldığında düşmesine esnek veritabanı işlemleri için beklemeniz gerekir.

## <a name="installation-and-migration"></a>Yükleme ve yükseltme
SQL veritabanına esnek veritabanı işlemleri için özellikleri System.Data.dll ve System.Transactions.dll .NET kitaplıklarına Güncelleştirmeler aracılığıyla sağlanır. DLL'leri emin olun, iki aşamalı yürütme kararlılık sağlamak üzere gerektiğinde kullanılır. Esnek veritabanı işlemleri kullanarak uygulamaları geliştirmeye başlamak için Yükle [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) veya sonraki bir sürümü. .NET framework'ün daha eski bir sürümünde çalışan, işlemler için Dağıtılmış bir işlem yükseltmek başarısız olur ve bir özel durum oluşturuldu.

Yükleme tamamlandıktan sonra SQL DB bağlantılarla System.Transactions içinde dağıtılmış işlem API'leri kullanabilirsiniz. Bu API'leri kullanarak mevcut MSDTC uygulamalarınız varsa, yalnızca .NET 4.6 için mevcut uygulamalarınızı 4.6.1 yükledikten sonra yeniden Framework. Projelerinizi .NET 4.6 hedefliyorsanız, otomatik olarak yeni Framework sürümü güncelleştirilmiş DLL'leri kullanır ve SQL DB bağlantıları ile birlikte API çağrılarının dağıtılmış işlem şimdi başarılı olur.

Esnek veritabanı işlem MSDTC yüklemek gerekmez unutmayın. Bunun yerine, esnek veritabanı işlemleri doğrudan tarafından ve SQL DB içinde yönetilir. MSDTC dağıtımını dağıtılmış işlemler SQL DB ile kullanmak için gerekli olmadığından bu bulut senaryolarında önemli ölçüde basitleştirir. 4. Bölüm esnek veritabanı işlemleri ve bulut uygulamalarınıza Azure ile birlikte gerekli .NET framework dağıtma konusunda daha ayrıntılı olarak açıklanmaktadır.

## <a name="development-experience"></a>Geliştirme deneyimi
### <a name="multi-database-applications"></a>Birden çok veritabanı uygulamaları
Aşağıdaki örnek kod tanıdık programlama deneyimi ile .NET System.Transactions kullanır. TransactionScope sınıfı .NET ortam bir işlemde oluşturur. (Bir "ortam" geçerli iş parçacığında bulunan bir işlemdir.) İşlemde tüm bağlantıları TransactionScope içinde açılır. Farklı veritabanlarına katılırsanız, işlem otomatik olarak bir dağıtılmış işlem için yükseltilmiş. İşlem sonucunu bir yürütme belirtmek için tamamlamak için kapsam ayarlayarak denetlenir.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Parçalı veritabanı uygulamaları
Esnek veritabanı işlemleri için SQL DB da destek burada esnek veritabanı istemci kitaplığının OpenConnectionForKey yöntemi genişletilmiş veri bağlantıları'nı açmak için kullandığınız dağıtılmış işlemler Eşgüdümleme katmanı. Burada, birkaç farklı parçalama anahtar değerleri arasında değişiklikler için işlem tutarlılığı garanti gerektirmelidir. Farklı parçalama anahtar değerleri barındırma parça bağlantıları OpenConnectionForKey kullanarak aracılı. İşlem Güvenceleri sağlayarak bir dağıtılmış işlem gerektirir, genel durumda farklı parça bağlantıları olabilir. Aşağıdaki kod örneği, bu yaklaşım gösterilmektedir. Bu, shardmap adlı bir değişken parça eşleme esnek veritabanı istemci Kitaplığı'ndan temsil etmek için kullanıldığını varsayar:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET yüklemesi için Azure bulut Hizmetleri
Azure birkaç teklifleri .NET uygulamalarını barındırmasını sağlar. Farklı teklifleri karşılaştırması kullanılabilir [Azure App Service, Cloud Services ve sanal makineler karşılaştırması](../app-service/choose-web-site-cloud-service-vm.md). Konuk işletim sistemi sunumun .NET 4.6.1 esnek işlemleri için gereken daha küçükse 4.6.1 için konuk işletim sistemini yükseltme yapmanız gerekir. 

Azure uygulama hizmetleri için konuk işletim sistemi yükseltmeleri şu anda desteklenmemektedir. Azure Virtual Machines için yalnızca VM oturum ve en son .NET framework için yükleyiciyi çalıştırın. Azure bulut Hizmetleri için başlangıç görevleri, dağıtımınızın daha yeni bir .NET sürüm yüklemesine eklemeniz gerekir. Kavramlar ve adımlar belgelenmiştir [bir bulut hizmet rolü .NET yükleme](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

.NET 4.6.1 yükleyici .NET 4.6 için Azure bulut hizmetlerine Yükleyici daha önyükleme işlemi sırasında daha fazla geçici depolama alanı gerektirebilir unutmayın. Yüklemenin başarılı olmak için Azure bulut hizmetinize LocalResources bölüm ve ortam ayarları, başlangıç görevinin ServiceDefinition.csdef dosyanızdaki geçici depolama artırmak aşağıdaki örnekte gösterildiği gibi gerekir:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Birden çok sunucudaki işlemleri
Esnek veritabanı işlemleri, Azure SQL veritabanında mantıksal farklı sunucular arasında desteklenir. Mantıksal sunucu sınırları işlemler arası olduğunda, Katılımcı sunucular ilk karşılıklı iletişim ilişkisi girilmesi gerekir. İletişim ilişki kurulduktan sonra iki sunuculardan herhangi biri herhangi bir veritabanında diğer sunucusundan veritabanlarıyla esnek işlemlerde yer alabilir. İkiden fazla mantıksal sunucularının genişleme işlemleri ile iletişim ilişkisi mantıksal sunucularının herhangi bir çifti için yerinde olması gerekir.

Esnek veritabanı işlemleri için sunucular arası iletişim ilişkilerini yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın:

* **AzureRmSqlServerCommunicationLink yeni**: Azure SQL DB mantıksal iki sunucu arasında yeni bir iletişim ilişki oluşturmak için bu cmdlet'i kullanın. Her iki sunucuyu diğer sunucusuyla işlemler başlatabilir yani simetrik ilişkidir.
* **Get-AzureRmSqlServerCommunicationLink**: Varolan iletişimi ilişkileri ve özellikleri almak için bu cmdlet'i kullanın.
* **Remove-AzureRmSqlServerCommunicationLink**: Use this cmdlet to remove an existing communication relationship. 

## <a name="monitoring-transaction-status"></a>İşlem durumunu izleme
Dinamik Yönetim görünümlerini (Dmv'leri) SQL DB durumunu izleyin ve ilerleme durumunu, devam eden esnek veritabanı işlemleri için kullanın. İşlemler için ilgili tüm Dmv'leri SQL DB dağıtılmış işlemlere ilgilidir. Karşılık gelen listesini Dmv'leri burada bulabilirsiniz: [işlem ilgili dinamik yönetim görünümlerini ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Bu Dmv'leri özellikle yararlı olur:

* **sys.DM\_tran\_etkin\_işlemleri**: geçerli durumda etkin işlemlerin ve durumlarını listeler. UOW (iş birimi) sütun aynı dağıtılmış işlem ait farklı alt işlemleri tanımlayabilirsiniz. Tüm işlemler aynı dağıtılmış işlem dahilinde aynı UOW değeri taşır. Bkz: [DMV belgelerine](https://msdn.microsoft.com/library/ms174302.aspx) daha fazla bilgi için.
* **sys.DM\_tran\_veritabanı\_işlemleri**: işlem günlüğünde yerleştirme gibi işlemleri hakkında ek bilgi sağlar. Bkz: [DMV belgelerine](https://msdn.microsoft.com/library/ms186957.aspx) daha fazla bilgi için.
* **sys.DM\_tran\_kilitleri**: şu anda devam eden işlemler tarafından tutulan kilitleri hakkında bilgi sağlar. Bkz: [DMV belgelerine](https://msdn.microsoft.com/library/ms190345.aspx) daha fazla bilgi için.

## <a name="limitations"></a>Sınırlamalar
Aşağıdaki sınırlamalar şu anda SQL veritabanına esnek veritabanı işlemleri için geçerlidir:

* Yalnızca işlemleri SQL DB veritabanları arasında desteklenir. Diğer [X / açmak XA](https://en.wikipedia.org/wiki/X/Open_XA) kaynak sağlayıcıları ve veritabanları SQL DB dışına esnek veritabanı işlemleri katılmak olamaz. Esnek veritabanı işlemleri arasında şirket içinde SQL Server ve Azure SQL veritabanları uzatma edilemez olduğunu anlamına gelir. Şirket içi dağıtılmış işlemler için MSDTC kullanmaya devam edin. 
* Yalnızca istemci Eşgüdümlü işlemleri bir .NET uygulamasında desteklenir. T-SQL BEGIN TRANSACTION dağıtılmış gibi sunucu tarafı desteği, planlanmış, ancak henüz kullanılamıyor. 
* WCF hizmetleri üzerinden işlemleri desteklenmez. Örneğin, bir işlem yürüten bir WCF Hizmeti yöntemine sahip. Bir işlem kapsamı içinde çağrı kapsayan başarısız olur olarak bir [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Sonraki adımlar
Sorular için lütfen bize üzerinde ulaşmak [SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik istekler için lütfen bunları Ekle [SQL veritabanı geri bildirim Forumunda](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



