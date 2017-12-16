---
title: "Microsoft Azure giriş | Microsoft Docs"
description: "Microsoft Azure yeni misiniz? Temel bir özeti isteğe bağlı olarak Hizmetleri ile nasıl yararlı örnekler sunar."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: f52252aca0ce89d6a86e620a97e749461181016f
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="introducing-microsoft-azure"></a>Microsoft Azure Tanıtımı
Microsoft Azure Microsoft'un uygulama için genel bulut platformudur.  Bu makalede bulut ilgili herhangi bir şey tanımadığınız olsa bile bir temel Azure, temelleri anlamak için size hedefidir bilgi işlem.

**Bu makaleyi okuyun nasıl**

Aşırı yüklenmiş kolaydır azure her zaman artıyor.  Bu makalede ilk listelenir ve ek hizmetler gidin temel Hizmetleri ile başlatın. Tek başına ek hizmetler kullanamaz, ancak Azure'da çalışan bir uygulama çekirdek temel Hizmetleri oluşturan anlamına gelmez.

**Geri bildirim gönderin**

Görüşleriniz önemlidir. Bu makalede Azure etkili genel bakış vermesi gerekir. Yoksa, sayfanın sonundaki açıklamalar bölümünde bize bildirin. Görmeyi beklediğiniz ve makale iyileştirmek nasıl bazı ayrıntı sağlayacaktır.  

## <a name="the-components-of-azure"></a>Azure bileşenleri
Azure Yönetim Portalı'nda ve gibi çeşitli görsel yardımlar kategorilere Hizmetleri grupları [Azure bilgi grafiği nedir](https://azure.microsoft.com/documentation/infographics/azure/) . Yönetim Portalı'nı yönetmek için kullandığınız olduğundan çoğu (ancak tüm) Hizmetleri.

Bu makalede kullanacağı bir **farklı Kuruluş** benzer işlevine bağlı hizmetleri hakkında konuşun ve büyük olanları parçası olan önemli alt hizmetlerini çağırın.  

![Azure bileşenleri](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Şekil: Azure Azure veri merkezlerinde çalışan Internet'ten erişilebilen uygulama hizmetleri sağlar.*

## <a name="management-portal"></a>Yönetim Portalı
Azure adında bir web arabirimi olan [Yönetim Portalı](http://manage.windowsazure.com) erişmek ve çoğu, ancak tüm Azure özelliklerini yönetmek yöneticilerin sağlar.  Microsoft, beta yeni UI portalında genellikle daha eski bir devre dışı bırakmadan önce yayımlar. Daha yeni bir adlı ["Azure Portal"](https://portal.azure.com/).

Aynı zamanda hem portalları etkin olduğunda genellikle uzun bir çakışma yoktur. Çekirdek Hizmetleri her iki portallarında görünür, ancak tüm işlevselliği hem de kullanılabilir. Daha yeni hizmetleri yeni portalı ilk ve eski Hizmetleri'nde gösterebilir ve işlevselliği, yalnızca daha eski bir mevcut olabilir.  Burada eski Portalı'nda, bir değer bulamazsanız, daha yeni bir tam tersini denetleyip iletisidir.

## <a name="compute"></a>İşlem
Bulut platformu mu en temel şeyler uygulamalarını yürütmek biridir. Her Azure işlem modellerin yürütmek için kendi rolü vardır.

Bu teknolojiler ayrı ayrı kullanın veya uygulamanızın doğru temeli oluşturmak için gerektiği şekilde birleştirin. Hangi sorunları bağlıdır seçtiğiniz yaklaşım çözmeye çalıştığınız.

### <a name="azure-virtual-machines"></a>Azure Sanal Makineler
![Azure sanal makineleri ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Şekil: Azure sanal makineleri size bulutta sanal makine örnekleri üzerinde tam denetim.*

Standart bir görüntü veya birinden sağladığınız olup olmadığını, isteğe bağlı bir sanal makine oluşturma olanağı çok kullanışlı olabilir. Yaygın olarak altyapı (Iaas) olarak bilinen bu yaklaşım, Azure sanal makineleri sağladıkları olur. Şekil 2 nasıl bir sanal makine (VM) çalışır, bir birleşimini gösterir ve bir VHD oluşturma.  

Bir VM oluşturmak için hangi VHD kullanın ve VM boyutu belirtin.  Sonra VM çalıştığı zaman için ödeme yaparsınız. Kullanılabilir VHD tutmak için bir en az depolama ücret olmasına rağmen dakika ve yalnızca çalışırken, ücret ödersiniz. Azure hisse senedi başlatmak için önyüklenebilir bir işletim sistemi ("görüntüleri" olarak adlandırılır) VHD'ler Galerisi sunar. Bunlar Windows Server ve Linux, SQL Server, Oracle ve diğerleri gibi Microsoft ve ortak seçenekleri içerir. VHD ve görüntüleri oluşturmak ve bunları kendiniz yüklemek boş. Hatta bunları çalışan Vm'leriniz erişmek ve yalnızca verileri içeren VHD karşıya yükleyebilirsiniz.

VHD geldiği olduğunda, bir VM çalışırken yapılan tüm değişiklikler kalıcı olarak depolayabilirsiniz. Bu VHD'den VM oluşturduğunuz sonraki sefer şeyler kaldığınız yerden yukarı seçin. Sanal makineleri yedekleme VHD'leri biz hakkında daha sonra konuşun Azure Storage bloblarında içinde depolanır.  Vm'leriniz donanım ve disk hatası nedeniyle kayboluyor olmaz sağlamak için artıklığı alma anlamına gelir. Azure dışında değiştirilmiş VHD'yi kopyalayın, sonra yerel olarak çalıştırmak mümkündür.

Bir veya daha fazla sanal makinelerdeki nasıl olmadan önce oluşturduğu veya artık sıfırdan oluşturmaya karar bağlı olarak, uygulamanızı çalışır.

Bulut için bu oldukça genel yaklaşım, birçok farklı sorunu gidermek için kullanılabilir.

**Sanal makine senaryoları**

1. **Geliştirme ve Test** -bunu kullanmayı bitirdiğinizde, bilgisayarı kapat bir uygun maliyetli geliştirme ve test platformu oluşturmak için kullanabilirsiniz. Ayrıca, hangi dilleri ve istediğiniz kitaplıkları kullanan uygulamaları çalıştırmak oluşturabilir ve. Bu uygulamaları Azure sağlayan ve ayrıca SQL Server ya da bir veya daha fazla sanal makinelerde çalışan başka bir DBMS kullanmayı seçebilirsiniz veri yönetim seçeneklerinden herhangi birini kullanabilirsiniz.
2. **Uygulamaları Azure (yükseltme ve shift) taşıma** -"Yükseltme ve shift" başvuruyor büyük nesne taşımak için bir forklift kullanırken yaptığınız gibi uygulamanızın çok taşıma için.  "Azure için shift" "Yerel merkeziniz, VHD'yi Yükselt" ve orada çalıştırın.  Genellikle diğer sistemlerde bağımlılıkları kaldırmak için biraz çalışmanız gerekir. Varsa çok seçenek 3 yerine tercih edebilirsiniz.  
3. **Veri merkeziniz genişletmek** -SharePoint veya diğer uygulamaları çalıştıran kullanım Azure VM'ler, şirket içi veri merkezinizin bir uzantısı olarak. Bunu desteklemek için Windows etki alanlarında Active Directory Azure Vm'lerde çalıştırarak bulutta oluşturmak mümkündür. Azure Virtual Network (daha sonra belirtilen), yerel ağınızın ve ağınızdaki Azure'da birbirine bağlamak için kullanabilirsiniz.

### <a name="web-apps"></a>Web Apps
![Azure Web Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Şekil: Azure Web uygulamaları bir Web sitesi uygulama bulutta temel web sunucusunu yönetmek zorunda kalmadan çalışır.*

Web siteleri ve web uygulamalarını bulutta kişiler yapmak en yaygın özelliklerinden biri çalıştırılır. Azure sanal makineleri bu sağlar, ancak hala, bir veya daha fazla sanal makineleri ve temel işletim sistemlerini yönetme ile ilgili sorumluluk bırakır. Bulut Hizmetleri web rolleri bunu ancak dağıtma ve bunların Bakımı yönetimsel iş hala alır.  Ne yalnızca bir Web sitesi başka bir kişiye burada istediğiniz sizin için yönetimsel iş ilgilenir?

Bu, tam olarak Web Apps sağladıkları olur. Bu işlem modeli API'leri yanı sıra Azure yönetim portalını kullanarak bir yönetilen web ortamı sunar. Var olan bir Web sitesi uygulamasını Web uygulamalarına değişmeden taşıyabilir veya doğrudan bulutta yeni bir tane oluşturabilirsiniz. Bir Web sitesi çalışmaya başladıktan sonra ekleme ya da Yük Dengeleme isteklerini arasında gezinmek için Azure Web Apps güvenmek örneklerini dinamik olarak kaldırın. Azure uygulamaları, Web sitenizin bir sanal makinedeki diğer sitelerle çalıştığı, paylaşılan bir seçeneği ve kendi VM çalıştırmak bir site sağlayan standart bir seçenek sunar. Standart seçeneği ayrıca, gerekirse (güç bilgisayar) örneklerinizi boyutunu artırın olanak tanır.

Geliştirme için Web Apps .NET, PHP, Node.js, Java ve Python birlikte SQL Database ve Azure veritabanı için MySQL ilişkisel depolama için destekler. WordPress, Joomla ve Drupal gibi çeşitli popüler uygulamalar için yerleşik destek de sağlar. Hedef, genel bulut ortamında Web siteleri ve web uygulamaları oluşturmak için düşük maliyetli, ölçeklenebilir ve kapsamlı ve kullanışlı bir platform sağlamaktır.

**Web uygulamaları senaryoları**

Web uygulamaları, şirketler, geliştiriciler ve web tasarımı kurumları için kullanışlı olması için tasarlanmıştır. Şirketler için bu durum Web sitelerini çalıştırmak için bir kolay yönetmek, ölçeklenebilir, yüksek güvenlikli ve yüksek oranda kullanılabilir çözümüdür. Bir Web sitesini ayarlamak gerektiğinde, Azure Web Apps ile başlatmak ve bulut hizmetlerine kullanılabilir olmayan bir özellik ihtiyacınız sonra devam etmek en iyisidir. Sonuna seçenekler arasından seçim yardımcı olabilecek daha fazla bağlantılar için "İşlem" bölümüne bakın.

### <a name="cloud-services"></a>Cloud Services
![Azure bulut hizmeti](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Şekil: Azure bulut hizmetlerine bir hizmet (PaaS) ortamı olarak bir Platform üzerinde yüksek düzeyde ölçeklenebilir özel kodu çalıştırmak için bir yer sağlar.*

Çok sayıda eşzamanlı kullanıcıyı desteklemek, çok yönetim gerektirmez ve hiçbir zaman arıza bir bulut uygulaması oluşturmak istediğinizi varsayalım. Bir yerleşik yazılım satıcısı olabilir örneğin, karar yazılım (SaaS) hizmet olarak kapsayacak şekilde bulut uygulamalarınızda birinin sürümünü oluşturarak. Veya hızlı büyüyecektir beklediğiniz bir tüketici uygulaması oluşturma başlangıç olabilir. Azure üzerinde oluşturuyorsanız, hangi yürütme modeli kullanmalısınız?

Bu tür bir web uygulaması oluşturma Azure Web Apps verir, ancak bazı kısıtlamalar vardır. İsteğe bağlı yazılımlar yükleyemezsiniz anlamına gelen yönetim erişimi, örneğin, sahip değilsiniz. Azure sanal makineleri size esneklik, yönetimsel erişim dahil olmak üzere çok sayıda ve kesinlikle bunu çok ölçeklenebilir bir uygulama oluşturmak için kullanabilirsiniz, ancak kendiniz güvenilirlik ve yönetim pek çok görünüşünün işlemeye sahip olacaksınız. Ne istiyorsunuz gerekir, ancak aynı zamanda güvenilirlik ve Yönetim için gereken işlerin çoğunu işleyen denetimi sunan bir seçenektir.

Bu, tam olarak Azure bulut Hizmetleri tarafından sağlanan olur. Bu teknoloji açıkça ölçeklenebilir, güvenilir desteklemek için tasarlanmıştır ve düşük Yönetim uygulamaları ve kullanıcının ne sık (PaaS) hizmet olarak Platform çağırdı örneği. Kullanmak için C#, Java, PHP, Python, Node.js veya başka bir şey gibi seçin teknolojisini kullanan bir uygulama oluşturun. Kodunuzu sonra sanal makinelerde (örnek adlandırılır) yürüten bir Windows Server sürümünü çalıştırıyor.

Ancak bu Vm'lere sahip Azure sanal makineler oluşturma gördüğünüzden farklıdır. Tek şey, Azure için kendisini işletim sistemi düzeltme eklerini yükleme gibi şeyler yapmaktan bunları yönetir ve otomatik olarak yeni dağıtım, görüntüleri düzeltme eki uygulandı. Bu, uygulamanızın web veya çalışan rolü örnekleri durumda koruyun döndürmemelidir anlamına gelir; Bunun yerine, bir sonraki bölümde açıklanan Azure veri yönetim seçenekleri de tutulmalıdır. Azure herhangi devreden yeniden, bu sanal makineleri de izler. Bulut hizmetleri otomatik olarak talebi karşılamak üzere daha fazla veya daha az örneği oluşturmak için ayarlayabilirsiniz. Bu, artan kullanım işlemek ve daha az kullanım olduğunda geri, kadar ödeme olmayan şekilde sonra ölçeği olanak sağlar.

Bir örneğini oluştururken seçmek için iki rol vardır, her ikisi de Windows Server tabanlı. İkisi arasındaki temel fark, çalışan rolü örneği çalışmazken bir web rolü örneği IIS çalıştıran ' dir. Ancak, her ikisi de aynı şekilde yönetilir ve her ikisi de kullanmak bir uygulama için yaygın bir durumdur. Örneğin, bir web rolü örneği kullanıcılardan gelen istekleri kabul, sonra işleme için bir çalışan rolü örneği geçirebilirsiniz. Uygulamanızı yukarı veya aşağı ölçeklendirme için Azure her iki rolün daha fazla örneğini oluşturmak veya var olan örneklerini kapatmak isteyebilir. Ve Azure sanal makineler için benzer, yalnızca süredir her web veya çalışan rolü örneğinin çalıştığından ücret ödersiniz.

**Senaryoları bulut Hizmetleri**

Bulut Hizmetleri platform Azure Web uygulamaları tarafından sağlanan daha fazla denetime ihtiyacınız ancak temel işletim sistemi üzerinde denetim gerekmeyen büyük ölçeklendirme desteklemek idealdir.

#### <a name="choosing-a-compute-model"></a>İşlem modeli seçme
Sayfa [Azure Web uygulamaları, bulut Hizmetleri ve sanal makineler karşılaştırması](app-service/choose-web-site-cloud-service-vm.md) daha ayrıntılı bir işlem modeli seçin hakkında bilgi sağlar.

## <a name="data-management"></a>Veri Yönetimi
Uygulamaların veri ve farklı veri türleri farklı türdeki uygulamalar gerekir. Bu nedenle, Azure veri depolamak ve yönetmek için birçok farklı yol sağlar. Çok sayıda depolama seçeneğinin Azure sağlar, ancak tüm çok dayanıklı depolama için tasarlanmıştır.  Bu seçeneklerin hiçbirini ile her zaman Azure veri merkezinde--Azure coğrafi yedeklilik başka bir veri merkezine en az 300 mil hemen yedeklemek için kullanmak izin verirseniz 6 arasında eşitlenmiş olarak saklanmasını verileriniz 3 kopyalar.     

### <a name="in-virtual-machines"></a>Sanal makineler
Azure sanal makineler ile oluşturulmuş VM'deki SQL Server ya da başka bir DBMS çalıştırma yeteneğini zaten belirtilmiş. Bu seçenek ilişkisel sistemleri için sınırlı olmadığını unutmayın; aynı zamanda MongoDB ve Cassandra gibi NoSQL teknolojileri çalıştırmak boş. Veritabanı sisteminizdeki çalışıyor BT basit çoğaltır ne kendi veri merkezlerinde için kullanılan dileriz- ancak aynı zamanda bu DBMS yönetim işleme gerektirir.  Diğer Seçenekler'de, sizin için yönetim tümünün veya daha fazla Azure işler.

Yeniden oluşturun veya karşıya herhangi ek veri diski ve sanal makine durumu yedeklenen (biz hakkında daha sonra konuşun) blob Depolama tarafından.  

### <a name="azure-sql-database"></a>Azure SQL Database
![Azure depolama SQL veritabanı](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Şekil: Azure SQL Database, bulutta bir yönetilen ilişkisel veritabanı hizmetidir.*

İlişkisel depolama için Azure SQL veritabanı özelliği sağlar. Sizi kandırmaya adlandırma izin vermeyin. Bu Windows Server üzerinde çalışan SQL Server tarafından sağlanan tipik bir SQL veritabanı farklıdır.  

SQL Azure adıysa, Azure SQL veritabanı tüm anahtar özellikler atomik işlemleri dahil olmak üzere bir ilişkisel veritabanı yönetim sisteminin veri bütünlüğü, ANSI SQL sorguları ve tanıdık programlama modelini ile birden çok kullanıcı tarafından eş zamanlı veri erişimi sağlar. SQL Server, SQL veritabanı Entity Framework kullanılarak erişilebilir gibi ADO.NET, JDBC ve tanıdık diğer veri teknolojileri erişin. Ayrıca, T-SQL dili, SQL Server Management Studio gibi SQL Server araçları ile birlikte çoğunu destekler. Herkes için SQL Server (veya başka bir ilişkisel veritabanı) aşina SQL veritabanı kullanarak basittir.

Ancak SQL veritabanı değil yalnızca bir PaaS hizmet DBMS bulut bu kullanıcının. Verileriniz hala denetlemek ve kimlerin erişebileceğini, ancak SQL veritabanı donanım altyapısını yönetme ve veritabanı ve işletim sistemi yazılım otomatik olarak güncel tutma gibi yönetim grunt iş ilgilenir. SQL veritabanı aynı zamanda yüksek kullanılabilirlik sağlar, otomatik yedeklemeler, zaman içinde nokta geri yükleme ve kopya coğrafi bölgeler arasında çoğaltabilirsiniz.  

**SQL veritabanı için senaryolar**

İlişkisel depolama gerektiren (herhangi bir işlem modelini kullanarak) bir Azure uygulaması oluşturuyorsanız, SQL veritabanı iyi bir seçenek olabilir. Vardır; bu nedenle diğer senaryolar bolca bulut dışında çalışan uygulamalar da bu hizmet, yine de kullanabilirsiniz. Örneğin, SQL veritabanında depolanan verileri Masaüstü, dizüstü bilgisayarlar, tabletler ve telefonlar gibi farklı istemci sistemlerden erişilebilir. Ve yerleşik yüksek kullanılabilirlik çoğaltması aracılığıyla sağladığından, SQL veritabanı kullanarak kapalı kalma süresini en aza indirmenize yardımcı olabilir.

### <a name="tables"></a>Tablolar
![Azure depolama tabloları](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Şekil: Azure tabloları verileri depolamak için düz bir NoSQL yol sağlar.*

Bu özellik farklı koşulları kaydının parçası "Azure depolama" olarak adlandırılan daha büyük bir özellik olarak da adlandırılır. "Tablo", "Azure tabloları" veya "depolama tabloları" görürseniz, bu tüm aynı şeydir.  

Ve adına göre kafası yok: Bu teknoloji ilişkisel depolama sağlamaz. Aslında, bu bir anahtar/değer deposu olarak adlandırılan bir NoSQL yaklaşım örneğidir. Azure tabloları dize, tamsayı ve tarihleri gibi çeşitli türlerdeki özelliklerini depolamak bir uygulama sağlar. Bir uygulama, bu grubu için benzersiz bir anahtar sağlayarak bir grup özellik sonra alabilirsiniz. While karmaşık işlemleri birleştirmeler desteklenmez gibi tablolar belirtilmiş veri hızlı erişim sağlar. Ayrıca veri terabayt kadar tutmak için tek bir tablo ile çok ölçeklenebilir. Ve bunların Basitlik eşleşen, tablolar SQL veritabanının ilişkisel depolama kullanan genellikle daha ucuz.

**Tablolar için senaryolar**

Oluşturmak istediğinizi varsayalım hızlı erişim için gereken bir Azure uygulama belki de bunu çok sayıda yazılan veriler, ancak bu veriler üzerinde karmaşık SQL sorguları gerçekleştirmek gerekmez. Örneğin, her kullanıcı için müşteri profil bilgilerini depolamak için gereken bir tüketici uygulaması oluşturduğunuzu düşünün. Uygulamanız için çok fazla veri izin vermesi gerekir, ancak bu veriler, depolama ötesinde ile basit şekilde alma olmaz şekilde çok popüler olacak. Tam olarak Azure tabloları mantıklı olduğu senaryo türü budur.

### <a name="blobs"></a>Bloblar
![Azure Storage Bloblarında](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Şekil: Azure BLOB'ları yapılandırılmamış ikili veriler sağlar.*  

Azure BLOB'ları (yeniden "Blob Depolama" ve "Depolama BLOB'lar" yalnızca aynı şeydir) yapılandırılmamış ikili verileri depolamak için tasarlanmıştır. Tablolar gibi BLOB'lar uygun maliyetli depolama sağlar ve tek bir blob 1 TB (bir terabayt) büyük olabilir. Azure uygulamalarını, bir Azure örneği bağlanan bir Windows dosya sistemi için kalıcı depolama sağlama BLOB'lar olanak sağlayan Azure sürücüler de kullanabilirsiniz. Uygulamanın normal Windows dosyalar görür, ancak içeriği gerçekte bir blob'a depolanır.

BLOB storage (sanal makineler dahil) diğer Azure, birçok özellik tarafından kullanılan, kesinlikle iş yüklerinizi çok işleyebilmesi için.

**BLOB'lar için senaryolar**

Video, büyük dosyalar veya diğer ikili bilgilerini depolayan bir uygulama için basit, ucuz depolama BLOB'ları kullanabilirsiniz. BLOB'ları de yaygın olarak, biz hakkında daha sonra konuşur içerik teslim ağı gibi diğer hizmetler ile birlikte kullanılır.  

### <a name="import--export"></a>İçeri Aktarmak / Dışarı Aktarmak
![Azure alma dışarı aktarma hizmeti](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Şekil: Azure alma / verme veya daha hızlı ve ucuz toplu verileri için Azure içeri veya dışarı aktarma için fiziksel bir sabit sürücü sevk olanağı sağlar.*  

Bazen, Azure'da çok miktarda veri taşımak istediğiniz. Uzun, belki de gün sürebilir ve çok miktarda bant genişliği kullanın. Bu durumda, Azure içeri/Bitlocker şifrelenmiş 3,5" SATA sabit sürücüler için doğrudan burada Microsoft verileri blob depolama alanına sizin için aktaracak Azure veri merkezlerinde sevk olanak tanıyan dışarı aktarma, kullanabilirsiniz.  Yükleme tamamlandıktan sonra Microsoft sizin sürücüleri gelir.  Ayrıca, büyük miktarlarda verileri Blob depolama biriminden sabit sürücüleri dışarı aktarılabilir ve sizin posta ile gönderilen olduğunu isteyebilir.

**Senaryoları için İçeri Aktar / dışarı aktarma**

* **Büyük veri geçişi** -büyük miktarlarda verinin Azure'a yüklemek istediğiniz (terabayt) kullandığınız zaman, içeri/dışarı aktarma hizmeti çok görülür daha hızlı ve Internet üzerinden aktarılması daha ucuz belki de. Verileri BLOB'ları eklendiğinde, tablo depolamaya veya bir SQL veritabanı gibi diğer formları içine işleyebilir.
* **Arşivlenen veri kurtarma** -Microsoft aktarımı miktarda veri depolanan Azure Blob depolamada bir depolama aygıtına gönderdiğiniz ve sonra sahip cihaz teslim istediğiniz bir konuma geri büyük sağlamak için içeri/dışarı aktarma kullanabilirsiniz. Bu biraz zaman alır çünkü olağanüstü durum kurtarma için iyi bir seçenek değil. Hızlı erişim gerekmeyen arşivlenmiş veriler için en iyisidir.

### <a name="file-service"></a>Dosya hizmeti
![Azure dosya hizmeti](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Şekil: Azure Dosya Hizmetleri SMB sağlar \\ \\server\share yolları bulutta çalışan uygulamalar için.*

Şirket içi, büyük miktarlarda dosya depolama sunucu ileti bloğu (SMB) protokolünü kullanarak üzerinden erişilebilir olması yaygın görülen bir \\ \\Server\share biçimi. Azure bulutta bu protokolü kullanmanıza olanak sağlayan bir hizmet artık sahiptir. Azure'da çalışan uygulamalar dosya tanıdık dosya sistemi ReadFile ve WriteFile gibi API'leri kullanarak VM'ler arasında paylaşmak için bunu kullanabilirsiniz. Ayrıca, dosyaları aynı anda bir sanal ağ da ayarladığınızda paylaşımları şirket içi erişmenize olanak sağlayan bir REST arabirimi aracılığıyla da erişilebilir. Azure dosyaları dayanıklılık, ölçeklenebilirlik ve coğrafi artıklık Azure depolama alanına yerleşik aynı kullanılabilirlik devralır şekilde blob hizmeti üstünde oluşturulur.

**Azure dosyaları için senaryolar**

* **Var olan uygulamaları buluta geçiş** -geçirmek, daha kolay uygulama bölümleri arasında veri paylaşmak için dosya paylaşımları kullanan uygulamaları buluta şirket. Her VM dosya paylaşımına bağlanır ve ardından onu okuyabilir ve bir şirket içi dosya karşı paylaştığınız gibi dosyaları yazma.
* **Uygulama ayarları paylaşılan** -dağıtılmış uygulamalar için genel bir desen yapılandırma dosyaları burada bunlar erişilebilir birçok farklı sanal makinelerden merkezi bir konumda olmalıdır. Bu yapılandırma dosyalarını bir Azure dosya paylaşımında depolanan ve tüm uygulama örnekleri tarafından okunur. Ayarları, dünya çapında erişime izin yapılandırma dosyalarını REST arabirimi aracılığıyla da yönetilebilir.
* **Tanılama paylaşımı** - kaydetme ve tanılama dosyalarında günlükler, ölçümler gibi paylaşma ve kilitlenme dökümleri. Bu dosyalar sahip olmanız SMB ve REST arabirimi aracılığıyla kullanılabilir uygulamaların işleme ve Tanılama verileri analiz etmek için çeşitli analiz araçları kullanmasını sağlar.
* **Geliştirme/Test/Debug** - bulutta, sanal makinelerde geliştiriciler veya Yöneticiler çalışırken sık ihtiyaç duydukları birtakım Araçlar ve yardımcı programlar. Yükleme ve her sanal makinede bu yardımcı programları dağıtmak zaman alıcıdır. Azure dosyaları ile bir geliştirici veya yönetici, sık kullandığınız araçları bir dosya paylaşımında depolayabilir ve bunlara herhangi bir sanal makineye bağlanmak.

## <a name="networking"></a>Ağ
Azure, bugün dünya genelinde yaygın birçok veri merkezlerinde çalışır. Bir uygulamayı çalıştırmak veya veri depolamak, bir veya daha fazla kullanmak için bu veri merkezlerinde seçebilirsiniz. Bu veri merkezlerine services'ı kullanarak çeşitli şekillerde bağlanabilir.

### <a name="virtual-network"></a>Sanal Ağ
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Şekil: Sanal ağlar sağlar bulut özel bir ağda farklı hizmetler birbirleriyle iletişim kurabilirsiniz ya da şirket içi kaynaklara ayarladığınız varsa bir VPN bağlantısı şirketler arası.*  

Genel buluta kullanmak için bir yararlı nı kendi veri merkezinizde bir uzantısı olarak işlemek için yoludur.

İsteğe bağlı VM'ler oluşturabileceğinden, sonra bunları kaldırmak (ve ödeme Durdur) bunlar artık gerekmediğinde, yalnızca istediğinizde gücünün olabilir. Ve Azure sanal makineler, SharePoint, Active Directory ve diğer tanıdık şirket içi yazılım çalışan sanal makineler oluşturmanıza olanak tanır olduğundan, bu yaklaşım zaten sahip uygulamalar ile çalışabilirsiniz.

Bu gerçekten kullanışlı hale getirmek için yine de bu uygulamalar, kendi veri merkezinizde çalışıyormuş gibi ele almanız mümkün olması için kullanıcılarınızın gelmelidir. Bu, tam olarak hangi Azure Virtual Network sağlar olur. Bir VPN ağ geçidi aygıtı kullanarak, bir yönetici yerel ağınız ve Azure sanal ağında dağıtılan Vm'leriniz arasında sanal özel ağ (VPN) ayarlayabilirsiniz. Bulut Vm'lerinde kendi IP v4 adresi atamak için kendi ağı üzerinde olmalarına görünürler. Kuruluşunuzdaki kullanıcılar uygulamaları erişim yerel olarak çalışan gibi bu VM'lerin içerebilir.

Planlama ve uygun bir sanal ağ oluşturma hakkında daha fazla bilgi için bkz: [sanal ağ](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>Express Route
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Şekil: ExpressRoute bir Azure sanal ağı kullanır, ancak daha hızlı ayrılmış satırları yerine genel Internet üzerinden bağlantılar yönlendirir.*  

Daha fazla bant genişliği ya da Azure sanal bağlantı sağlayan ağ güvenlikten gerekiyorsa, ExpressRoute bakabilirsiniz. Bazı durumlarda, ExpressRoute de tasarruf. Azure sanal ağında hala gerekir ancak genel Internet üzerinden geçmez adanmış bir bağlantı Azure ve sitenizi arasındaki bağlantıyı kullanır. Bu hizmeti kullanmak için bir ağ hizmeti sağlayıcısı veya bir exchange sağlayıcısı ile bir anlaşması olması gerekir.

Bu nedenle daha fazla zaman bağlantı gerektiren bir expressroute bağlantı ayarlama ve planlama, siteden siteye VPN ile başlayın, ardından bir ExpressRoute bağlantısıyla geçirmek isteyebilirsiniz.

ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute teknik genel bakış](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Şekil: Azure trafik Yöneticisi hizmetinize akıllı kurallara göre genel trafiği yönlendirmek sağlar.*

Azure uygulamanız birden çok veri merkezlerinde çalıştığından, kullanıcılardan gelen istekleri akıllıca uygulama örneklerinde yönlendirmek için Azure trafik Yöneticisi'ni kullanabilirsiniz. Ayrıca, Internet'ten erişilebilir olduğu sürece Azure üzerinde çalışmayan hizmetlerine trafiğini yönlendirebilir.  

Yalnızca tek bir parça dünya kullanıcıların bir Azure uygulamasıyla yalnızca bir Azure veri merkezinde çalıştırabilirsiniz. Bir uygulama dünya genelinde dağılmış kullanıcılarla ancak, birden çok veri merkezlerinde, tüm hatta belki de çalıştırmak daha yüksektir. Bu ikinci durumda, bir sorun yüz: nasıl, akıllıca doğrudan uygulama örnekleri kullanıcılara? Çoğu zaman, büyük olasılıkla bunu büyük olasılıkla her en iyi yanıt süresi verecektir bu yana kendisine, en yakın veri merkezine erişmek için her bir kullanıcı istersiniz. Ancak ne uygulama örneğini aşırı yüklenmiş veya kullanılamaz olur? Bu durumda, kendi isteği otomatik olarak başka bir veri merkezine doğrudan iyi olacaktır. Bu, tam olarak hangi Azure trafik Yöneticisi tarafından yapılır olur.

Bir uygulama sahibi kullanıcıların isteklerini veri merkezlerini nasıl yönlendirilmesi gerektiğini belirten kuralları tanımlar ve ardından bu kurallar taşımak için trafik Yöneticisi'üzerinde kullanır. Örneğin, kullanıcılar en yakın Azure veri merkezi normalde yönlendirilebilir, ancak diğer veri merkezleri yanıt saati kendi varsayılan veri merkezi yanıt saati aştığında, başka birine gönderilen. Çok sayıda kullanıcı içeren genel dağıtılmış uygulamalar için bu gibi sorunları işlemek için yerleşik bir hizmet olması yararlı olacaktır.

Trafik Yöneticisi için rota kullanıcılar hizmet uç noktaları için dizin adı hizmeti (DNS) kullanır, ancak o bağlantı yapıldıktan sonra daha fazla trafiği trafik Yöneticisi ile geçmez. Bu trafik Yöneticisi, hizmet iletişimleri yavaşlatabilir bir performans sorunu engeller.

## <a name="developer-services"></a>Geliştirici Hizmetleri
Azure geliştiricilere yardımcı olan araçlar sunar ve BT uzmanları oluşturup uygulamalarını bulutta.  

### <a name="azure-sdk"></a>Azure SDK
Geri 2008'de, yalnızca .NET development Azure ilk yayım öncesi sürümü desteklenmiyor. Bugün, Bununla birlikte, Azure uygulamalarını neredeyse herhangi bir dilde oluşturabilirsiniz. Microsoft, şu anda .NET, Java, PHP, Node.js, Ruby ve Python için dile özgü SDK'ları sağlar. C++ gibi herhangi bir dil için temel destek sağlayan genel bir Azure SDK yoktur.  

Bu SDK'ları oluşturmak, dağıtmak ve Azure uygulamaları yönetmenize yardımcı olur. Ya da kullanılabilir gelen [www.microsoftazure.com](https://azure.microsoft.com/downloads/) veya GitHub ve Visual Studio ve Eclipse ile kullanılabilir. Azure de geliştiricilerin, Linux ve Macintosh sistemleri Azure uygulamalarından dağıtmak için araçlar da dahil olmak üzere tüm Düzenleyicisi veya geliştirme ortamı ile kullanabileceğiniz komut satırı araçları sunar.

Azure uygulamalar oluşturmanıza yardımcı yanı sıra bu SDK'ları da yardımcı istemci kitaplıkları, yazılım oluşturmak sağlamak Azure hizmetlerini kullanır. Örneğin, okuyan ve Azure BLOB'ları yazan bir uygulama oluşturmak veya Azure Yönetimi arabirimi aracılığıyla Azure uygulamalarını dağıtan bir aracı oluşturun.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services, Azure uygulamaları geliştirmek için yardımcı olan bir sayı Hizmetleri kapsayan bir pazarlama adıdır.

-Karışıklığı önlemek için Visual Studio barındırılan veya Web tabanlı bir sürümü sağlamaz. Visual Studio çalışan yerel kopyasını yine gerekir. Ancak, çok yararlı olabilecek diğer birçok araçlar sağlar.

Sürüm denetimi ve iş öğesi izleme sunar Team Foundation Service adlı bir barındırılan kaynak denetim sistemi içermez.  Tercih ederseniz, Git sürüm denetimi için bile kullanabilirsiniz. Ve project tarafından kullandığınız kaynak denetim sistemi değişebilir. Sınırsız özel takım projeleri erişilebilen herhangi bir yere dünyada oluşturabilirsiniz.  

Visual Studio Team Services yük test etme hizmeti sağlar. Visual Studio bulut vm'lerinde oluşturulan yük testlerini çalıştırabilirsiniz. Yük testi ile istediğiniz kullanıcıların toplam sayısını belirtin ve Visual Studio Team Services, otomatik olarak gereken sanal makineleri Döndür ve yük testleri yürütün kaç aracıları gereklidir belirler. MSDN abone değilseniz, her ay sınama yük boş kullanıcı dakika binlerce alın.

Visual Studio Team Services aynı zamanda sürekli tümleştirme derlemeler gibi özellikleri, Kanban panolarına ve sanal takım odaları Çevik Geliştirme desteği sağlar.

**Visual Studio Team Services senaryoları**

Visual Studio Team Services yoktur ve tüm dünyada işbirliği yapmak için ihtiyaç duyan şirketler için iyi bir seçenek olan altyapı Bunun yerinde zaten sahip. Dakika cinsinden Kurulum alın, kaynak denetim sistemi seçin ve kod yazma ve o gün oluşturmaya başlayın.  Takım araçları yer için eşgüdüm sağlar ve işbirliği ve ek araçlar sınamak ve uygulamanızı hızlı bir şekilde ayarlamak için gereken analiz sağlayın.

Ancak bir şirket içi sistem zaten olan kuruluşlar yeni projeler Visual Studio Team Services aygıtını daha verimli olup olmadığını test edebilirsiniz.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Şekil: Application Insights izleyiciler performans ve kullanım canlı web veya aygıt uygulamanızın.*

Mobil aygıtlar, masaüstü bilgisayarlar veya web tarayıcıları - çalıştırıldığını - uygulamanızı yayımladıysanız, Application Insights nasıl çalıştığını ve hangi kullanıcıların ile yaptıklarını söyler. Kilitlenmeler ve yavaş yanıt sayısını tutar, uyarı, rakamları kabul edilemez eşikleri arası ve yardımcı sorunların tanılanmasına.

Yeni bir özellik geliştirirken kullanıcılarla başarısını ölçmek için plan yapın. Kullanım modelleri analiz etme tarafından ne müşterileriniz için en iyi çalıştığını anlamak ve her geliştirme döngüsü uygulamanızı geliştirin.

Azure üzerinde barındırılan rağmen uygulamalar, hem şirket hem de Azure dışı geniş ve artan bir dizi için Application Insights çalışır. J2EE ve ASP.NET web uygulamaları kapsanan, yanı sıra iOS, Android, OSX ve Windows uygulamaları. Analiz ve Azure Application Insights hizmetinde görüntülenecek uygulamasıyla, yerleşik bir SDK telemetri gönderilir.

Daha özelleştirilmiş analytics istiyorsanız, telemetri akışına bir veritabanı veya Power BI veya diğer araçlar verin.

**Uygulama Öngörüler senaryoları**

Bir uygulama geliştirmektedir. Bir web uygulaması veya bir aygıt uygulama veya cihaz uygulaması web arka ucuna sahip olabilir.

* Yayımlandıktan sonra uygulamanızın performansı ayarlamak ya da test olarak kullanılırken yükleyin.  Application Insights telemetri yüklü tüm örneklerden toplar ve yanıt sürelerini, istek ve özel durum sayısı, bağımlılık yanıt sürelerini ve diğer performans göstergelerini grafiklerle sunar. Bu, uygulamanızın performansı ayarlamak yardımcı olur. Daha fazla rapor için kodu ekleyin ihtiyacınız olursa belirli veri.
* Algılama ve canlı uygulamanızdaki sorunları tanılayın. Kabul edilebilir eşikleri performans göstergelerini arası, e-posta ile uyarıları alabilirsiniz. Örneğin bir özel durum nedeniyle isteği görmek belirli kullanıcı oturumlarını araştırabilirsiniz.
* Her yeni özellik başarısını değerlendirmek için kullanım izler. Yeni kullanıcı hikayesi tasarlarken, ne kadar kullanılır ve olup kullanıcıların beklenen hedeflerine ulaşması ölçmek için plan yapın. Application Insights, web sayfası görünümleri gibi temel kullanım verilerini verir ve kullanıcı deneyimi daha ayrıntılı izlemek için kodu ekleyin.

### <a name="automation"></a>Otomasyon
Hiç kimse aynı süreç defalarca yapılması zaman kaybı istemeyebilir. Azure Otomasyon oluşturmak, izlemek, yönetmek ve kaynakları Azure ortamınızda dağıtmak bir yol sağlar.  

Otomasyon kapsar altında Windows PowerShell iş akışları (ve yalnızca normal PowerShell) kullanan "runbook'lar" kullanır. Runbook'ları, kullanıcı etkileşimi olmadan yürütülecek yöneliktir. PowerShell iş akışı denetim noktaları yol adresindeki kaydedilmesi için bir komut dosyası durumunu sağlar. Bir hata oluşursa, bir komut dosyası baştan başlamanız gerekmez. Son denetim onu yeniden başlatabilirsiniz. Bu çok fazla olası her hata işleme betik yapmaya çalışırken iş kaydeder.

**Otomasyon senaryoları**

Azure Otomasyonu, Azure el ile uzun süreli, hatasız ve sık tekrarlanan görevleri otomatik hale getirmek için iyi bir seçimdir.

### <a name="api-management"></a>API Management
Oluşturma ve Internet üzerindeki uygulama programlama arabirimleri (API) yayımlama uygulamaları hizmetleri sağlamak üzere genel bir yoludur. Bu Hizmetleri (örneğin, hava durumu verileri) satılabilir varsa, bir kuruluş için ücret karşılığında aynı hizmetlere erişmek diğer üçüncü tarafların izin verebilirsiniz. Daha fazla iş ortakları için ölçeklendirme gibi genellikle en iyi duruma getirme ve erişimi denetlemek gerekir.  Bazı iş ortakları, farklı bir biçimde veri bile gerekebilir.

Azure API Management API'leri ortakları, çalışanlar ve üçüncü taraf geliştiriciler için güvenli bir şekilde ve ölçekte yayımlama kuruluşlarda kolaylaştırır. Farklı bir API uç sağlar ve önbelleğe alma, dönüştürme, azaltma, erişim denetimi ve analiz toplama gibi hizmetleri sağlarken gerçek uç noktasını çağırmak için bir proxy olarak görev yapar.

**API yönetim senaryoları**

Şirketinizin sahip bir cihaz kümesi düşünelim tüm aygıtları her kamyon yola sahip bir sevkiyat şirket verileri--Örneğin, merkezi bir hizmete geri arama gerekiyor.  Kesinlikle şirket, güvenilir bir şekilde tahmin etmek ve teslimat saatleri güncelleştirme verebilmesi kendi kamyonlar izlemek için bir sistemi ayarlamak isteyebilirsiniz. Bu, sahip kaç kamyonlar bilmeniz ve uygun şekilde planlamanız.  Her kamyon, konumlandırma ve hız veriler ve belki de daha fazla ile merkezi bir konuma geri çağıran bir aygıtı gerekir.

Bir müşteri sevkiyat şirketin olur konumlandırma bu veri alma de yararlanabilir.  Müşteri bu ürünleri ne kadar seyahat, burada bunlar, ne kadar bunlar (ne bunlar dağıtmayı Ücretli ile birleştirilmiş varsa) belirli yollar ödeme takılı zorunda öğrenmek için kullanabilirsiniz. Nakliye şirketi zaten bu veriler toplar, birçok müşteri için ödeme.  Ancak müşteriler veri vermek için bir yol sağlamak üzere Nakliye şirketi gerekiyor. Erişim müşterilere sağladıkları sonra verileri ne sıklıkla sorgulanan üzerinde denetim olmayabilir. Bunlar, hangi verilerin erişebilecek kişileri hakkında kurallar vermeniz gerekir. Bu kuralların hepsi, kendi API'nin oluşturulması gerekir. API Management burada yardımcı olabilecek budur.  

## <a name="identity-and-access"></a>Kimlik ve Erişim
Kimliğine sahip çalışan çoğu uygulamayı bir parçasıdır. Kimin bir kullanıcı bilerek bu kullanıcı ile nasıl etkileşim kurması gerektiğine karar bir uygulama sağlar. Azure kimlik yanı sıra zaten kullanıyor olabileceğiniz kimlik depolarına ile tümleştirmek izlenmesine yardımcı olmak için hizmetleri sağlar.

### <a name="active-directory"></a>Active Directory
Çoğu Dizin Hizmetleri gibi Azure Active Directory Kullanıcıları ve ait oldukları kuruluşlar hakkında bilgi depolar. Oturum açmasına olanak sağlar ve ardından bunları kimliğini kanıtlamak için uygulamalarına sunabilir belirteçleri ile sağlar. Ayrıca, Windows Server Active Directory'yle yerel ağınıza çalışan kullanıcı bilgileri eşitleme sağlar. Azure Active Directory tarafından kullanılan veri biçimleri ve mekanizmaları ile Windows Server Active Directory'de kullanılanlarla aynı değildir, ancak gerçekleştirdiği işlevleri oldukça benzer.

Azure Active Directory bulut uygulamaları tarafından birincil olarak kullanılmak üzere tasarlanmış anlamak önemlidir. Örneğin veya diğer bulut platformlarda Azure üzerinde çalışan uygulamalar tarafından kullanılabilir. Ayrıca, Office 365'te olanlar gibi Microsoft'un kendi bulut uygulamaları tarafından kullanılır. Ancak, veri merkeziniz Azure sanal makineler ve Azure Virtual Network kullanarak buluta genişletmek istiyorsanız, Azure Active Directory doğru seçim değildir. Bunun yerine, Windows Server Active Directory sanal makinelerinde çalışan istersiniz.

Uygulamalar, içerdiği bilgilere erişim izin vermek için Azure Active Directory RESTful bir Azure Active Directory grafik API'si sağlar. Bu API, tüm platform erişim dizin nesneleri ve aralarındaki ilişkiler üzerinde çalışan uygulamalar sağlar.  Örneğin, yetkili bir uygulama, bir kullanıcı, ait olduğu grupları ve diğer bilgiler hakkında bilgi edinmek için bu API kullanabilir. Uygulamaları bunları kişiler arasındaki bağlantıları daha akıllıca çalışmak grafik veren kullanıcılar their sosyal arasındaki ilişkileri de görebilirsiniz.

Başka bir özelliği, bu hizmet, Azure Active Directory erişim denetimi, Facebook, Google, Windows Live ID ve diğer popüler kimlik sağlayıcılardan gelen kimlik bilgileri kabul etmek bir uygulama için kolaylaştırır. Her bu sağlayıcıları tarafından kullanılan protokoller ve farklı veri biçimleri anlamak için uygulama istemeden yerine, erişim denetimi bunların tümünü tek bir ortak biçimine çevirir. Ayrıca, bir veya daha fazla Active Directory etki alanı oturum açmayı kabul bir uygulama sağlar. Örneğin, bir SaaS uygulaması sağlayan bir satıcı kullanıcıların her biri kendi müşteriler çoklu oturum açma uygulamaya vermek için Azure Active Directory erişim denetimi kullanabilirsiniz.

Dizin Hizmetleri, bilgi işlem içi çekirdek masaüstünün oluşturulur. Bunlar ayrıca bulutta önemli olduğunu şaşırtıcı olmamalıdır.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Şekil: Çok faktörlü kimlik doğrulaması birden fazla tür kimlik doğrulamak, uygulamanız için işlevsellik sağlar.*

Güvenlik her zaman önemlidir. Çok faktörlü kimlik doğrulaması (MFA) yalnızca kullanıcıların kendilerini hesaplarına erişim güvence altına yardımcı olur. MFA (olarak da bilinen iki öğeli kimlik doğrulama veya "2FA") kullanıcılara iki kimlik doğrulama kullanıcı oturum açmaları ve işlemleri için bu üç yöntem sağlamanız gerekir.

* (Genellikle parola) bildiğiniz bir şey
* (Kolayca, bir telefon gibi yineleniyor değil güvenilir bir cihaz) sahip olduğunuz şey
* (Biyometri) olan bir şey

Bu nedenle bunları da mobil uygulama, bir telefon araması veya SMS mesajı parolalarını birlikte kendi kimliğini doğrulamak için bir kullanıcı oturum açtığında gerektirebilir. Varsayılan olarak, Azure Active Directory kullanıcı oturum açma işlemleri için parola kullanımını, yalnızca kimlik doğrulama yöntemi olarak destekler. MFA SDK kullanarak Azure AD ile birlikte veya özel uygulamalar ve dizinler MFA kullanabilirsiniz. Sizin de multi-Factor Authentication sunucusu kullanarak şirket içi uygulamalar ile birlikte kullanabilirsiniz.

**MFA senaryoları**

Oturum açma koruma banka oturum açmalar ve kaynak koduna erişim izinsiz girişi maliyeti yüksek finansal veya bir fikri özelliğine sahip olduğu gibi hassas hesapları.   

## <a name="mobile"></a>Cep telefonu
Bir mobil aygıt için bir uygulama oluşturuyorsanız, Azure bulutta veri depolamak, kullanıcıların kimliğini doğrulamak ve büyük bir bölümünü özel kod yazmaya gerek kalmadan anında iletme bildirimleri göndermek yardımcı olabilir.

Sanal makineler, bulut Hizmetleri veya Web uygulamaları kullanarak bir mobil uygulama arka kesinlikle oluşturabilirsiniz, ancak Azure Hizmetleri kullanarak temel alınan hizmet bileşenlerini yazma çok daha az zaman harcayabilir.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Şekil: Mobil uygulamaları ile mobil cihazları arabirim uygulamaları tarafından genellikle gerekli işlevselliği sağlar.*

Azure mobil uygulamalar, bir mobil uygulama için bir arka uç oluştururken kaydedebileceğiniz pek çok yararlı işlevleri zaman sağlar. Basit sağlama ve yönetim bir SQL veritabanında depolanan verilerin yapmanıza izin verir. Sunucu tarafı kodu ile kolayca blob depolama veya MongoDB gibi ek veri depolama seçeneklerini kullanabilirsiniz. Bazı durumlarda, bunun yerine bildirim hub'ları sonraki bölümde açıklandığı gibi kullanabilmenize rağmen Mobile Apps bildirimleri için destek sağlar.  Hizmet, mobil uygulamanız işlerini halletmek için çağırabilir bir REST API de vardır. Mobile Apps, aynı zamanda Microsoft ve Active Directory Kullanıcıları ve aynı zamanda diğer iyi bilinen kimlik sağlayıcıları gibi Facebook, Twitter ve Google kimlik doğrulama yeteneği sağlar.   

Hizmet veri yolu ve çalışan rolleri gibi diğer Azure hizmetlerini kullanmak ve şirket içi sistemlere bağlanın. Ek işlevsellik sağlamak için bile 3 taraf eklentiler (ör. SendGrid e-posta için) Azure Mağazası'ndan kullanmasını sağlayabilirsiniz.

Android, iOS, HTML/JavaScript, Windows Phone ve Windows mağazası için yerel istemci kitaplıkları, tüm önemli mobil platformlar uygulamaları geliştirmek kolaylaştırır. Bir REST API farklı platformlarda uygulamalarıyla mobil hizmetler veri ve kimlik doğrulama işlevselliği kullanmanıza olanak sağlar. Cihazlar arasında tutarlı bir kullanıcı deneyimi sağlayabilmesi için tek bir mobil hizmette birden çok istemci uygulamaları yedekleyebilirsiniz.

Azure yüksek ölçüde ölçeklendirmeyi zaten desteklediğinden, uygulamanızı daha popüler hale geldiğinde trafiği işleyebilir.  İzleme ve günlüğe kaydetme sorunları gidermek ve performans yönetmeye yardımcı olmak için desteklenir.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Şekil: Bildirim hub'ları ile mobil cihazları arabirim uygulamaları tarafından genellikle gerekli işlevselliği sağlar.*

Azure Mobile Apps bildirimleri yapmak üzere kod yazabilirsiniz olsa da, bildirim hub'ları dakika içinde yüksek oranda kişiselleştirilmiş anında iletme bildirimlerinin milyonlarca yayın için optimize edilmiştir.  Mobil taşıyıcı veya aygıt üreticisi gibi ayrıntıları hakkında endişelenmeniz gerekmez. Tek tek veya kullanıcılar tek bir API çağrısı ile milyonlarca hedefleyebilirsiniz.

Bildirim hub'ları ile herhangi bir arka uçtan çalışacak şekilde tasarlanmıştır. Azure Mobile Apps, özel bir arka uç üzerinde herhangi bir sağlayıcıyı çalıştıran bulutta veya şirket içi arka uç kullanabilirsiniz.

**Bildirim hub'ı senaryoları** burada oynatıcıları sürdü kapatır mobil oyun yazıyorsanız, player 1 Tamamlandı kendi dönüş player 2 bildir gerekebilir. Tüm yapmanız gereken budur, yalnızca Mobile Apps kullanabilirsiniz. Ancak, 100.000 kullanıcı varsa, oyun ve hassas ücretsiz teklif herkes, bildirim hub'ları daha iyi bir seçimdir birer göndermek istiyor.

Düşük gecikme süresine sahip olayları ve ürün duyuru bildirimleri milyonlarca kullanıcıyı sporting son dakika haberleri gönderebilirsiniz. Çalışanlar sürekli olarak e-posta veya diğer uygulamalar hakkında bilgi sahibi olmak için kontrol zorunda kalmamak için kuruluşların çalışanlarına satış fırsatları gibi yeni saat hassas iletişimlerle ilgili bildirebilir. Bir-zamanlı çok faktörlü kimlik doğrulaması için gerekli parolaları da gönderebilirsiniz.

## <a name="back-up"></a>Yedekleme
Her kuruluş, yedekleme ve veri geri yükleme gerekiyor. Azure backup ve uygulamanızı bulutta veya şirket içi olup olmadığını geri yüklemek için kullanabilirsiniz. Azure backup türüne bağlı olarak yardımcı olması için farklı seçenekleri sunar.

### <a name="site-recovery"></a>Site Recovery
Azure Site Recovery (önceki adıyla Hyper-V kurtarma Yöneticisi), çoğaltma ve kurtarma siteleri arasında iletişime geçerek önemli uygulamaların korunmasına yardımcı olabilir. Site kurtarma Hyper-v, VMWare veya SAN kendi ikincil site, bir barındırma sağlayıcısının site veya Azure göre uygulamaların korunmasına ve gider ve derleme ve ikincil konumunuz yönetme karmaşıklığını önlemek için yeteneği sağlar. Azure verileri şifreler ve iletişimleri ve çalışmıyorken veri şifrelemesi çok etkinleştirme seçeneğiniz vardır.

Hizmetlerinizin durumunu kesintisiz olarak izler ve düzenli kurtarma Hizmetleri birincil veri merkezindeki bir site kesinti durumunda yardımcı otomatikleştirin. Sanal makineler, hizmetin hızlı şekilde geri yüklenmesine yardımcı olmak için karmaşık çok katmanlı iş yükleri için bile düzenlenmiş bir halde görüntülenebilir.

Site Recovery, Hyper-V çoğaltma, System Center ve SQL Server Always On gibi mevcut teknolojiler ile çalışır. Kullanıma [Azure Site Recovery genel bakış](site-recovery/site-recovery-overview.md) daha fazla ayrıntı için.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Şekil: Azure yedekleme verileri şirket içi Windows sunucularından bulutunu yedekler.*  

Azure yedekleme verileri buluta Windows Server çalıştıran şirket içi sunucularından yedekler. Windows Server 2012, Windows Server 2012 Essentials veya System Center 2012 - Data Protection Manager yedekleme Araçları'ndan doğrudan Yedeklemelerinizin yönetebilirsiniz. Alternatif olarak, özelleştirilmiş bir yedekleme aracısı kullanabilirsiniz.

Veriler Aktarımdan önce yedekler şifrelendiği daha güvenli ve şifrelenmiş olarak Azure depolanır ve karşıya yüklediğiniz bir sertifika tarafından korunan. Hizmeti, Azure depolama alanında bulunan aynı yedekli ve yüksek oranda kullanılabilir veri koruması kullanır.  Tam veya artımlı yedeklemeleri dosyaları ve klasörleri düzenli bir zamanlamaya göre veya hemen yedekleyebilirsiniz. Verileri buluta yedeklendiğinden sonra yetkili kullanıcıların herhangi bir sunucuya yedeklemeleri kolayca kurtarabilirsiniz. Ayrıca, depolama ve veri aktarımı maliyeti yönetilebilir yapılandırılabilir veri bekletme ilkeleri, veri sıkıştırma ve veri azaltma aktarım sunar.

**Azure yedekleme için senaryolar**

Windows Server veya System Center zaten kullanıyorsanız, Azure yedekleme sunucuları dosya sistemi, sanal makineler ve SQL Server veritabanlarını yedeklemek için doğal bir çözümdür.  Şifrelenmiş, seyrek ve sıkıştırılmış dosyaları ile çalışır. Bazı sınırlamalar vardır, size gereken şekilde [Azure Backup önkoşulları denetleyin](http://technet.microsoft.com/library/dn296608.aspx) ilk.

## <a name="messaging-and-integration"></a>Mesajlaşma ve Tümleştirme
Ne yapmakta olduğu olsun, kod sık diğer kodu ile etkileşimde olmalıdır.  Bazı durumlarda, gereken tek şey temel kuyruğa alınan Mesajlaşma. Diğer durumlarda, daha karmaşık etkileşimler gereklidir. Azure, bu sorunları çözmek için birkaç farklı şekilde sağlar. Şekil 5 seçenekler gösterilmektedir.

### <a name="queues"></a>Kuyruklar
![Azure Service Bus geçişi](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Şekil: Sıraları uygulama bölümleri arasında Kuplaj gevşek izin ve ölçeklendirme kolaylaştırabilirsiniz.*  

Queuing olan basit bir fikir: bir uygulama bir ileti sıraya koyar ve bu iletiyi sonunda başka bir uygulama tarafından okunur. Uygulamanızı basit bu hizmet gerekiyorsa, Azure kuyrukları en iyi seçim olabilir.

Zaman içinde Azure büyüdü yöntemi nedeniyle, Azure depolama kuyruklar ve hizmet veri yolu kuyrukları benzer sıraya alma hizmetleri sağlar. Neden diğer üzerinde kullanmak istediğiniz nedenleri oldukça teknik yazıda kapsanan [Azure kuyruklar ve hizmet veri yolu kuyrukları karşılaştırılan ve Contrasted -](http://msdn.microsoft.com/library/azure/hh767287.aspx).  Birçok senaryoda ya da çalışır.

**Sıra senaryoları**

Bir genel sıraların bugün web rol örneği aynı bulut Hizmetleri uygulama içinde çalışan rol örneği ile iletişim kurmasına izin vermek için kullanılır.

Örneğin, video paylaşımı için bir Azure uygulaması oluşturduğunuzu varsayalım. Uygulamayı kullanıcılara karşıya yükleme ve C# ' ta çeşitli biçimlere karşıya yüklenen video çeviren uygulanan bir çalışan rolü ile birlikte izleme videolar olanak sağlayan bir web rolü çalışan PHP kodunun oluşur.

Bir web rolü örneği bir kullanıcıdan yeni bir video aldığında, bir blob'a video depolamak sonra çalışan rolü bu yeni video nerede bulacağını söyleyen bir kuyruk aracılığıyla ileti göndermek. Bir çalışan rolü örneği BT değil hangi biri olacak önemli sonra iletiyi sıradan okuyun ve gerekli video çevirileri arka planda gerçekleştirmek.

Zaman uyumsuz işleme sağlayan bir uygulama bu şekilde yapılandırılması ve web rolü örnekleri ve çalışan rolü örnekleri sayısı bağımsız olarak değiştirilebilir olduğundan, ayrıca uygulama ölçek kolaylaştırır. Yukarı ve aşağı çalışanı rollerinin sayısını ölçeklendirmek için tetikleyici olarak sıra boyutu de kullanabilirsiniz. Çok yüksek ve daha fazla rolü ekleyin. Alt aldığında paradan tasarruf için roller çalışan sayısını azaltabilir.  

Web ve çalışan rolleri kullanmasanız bile, uygulamanızın birçok farklı bölümleri arasında aynı bu deseni kullanabilirsiniz.  İsteğe bağlı olarak yukarı ve aşağı sırasının her iki tarafında bölümleri ölçeklendirmenizi sağlar ve işleme süresi gerektirir.

### <a name="service-bus"></a>Service Bus
Buluttaki bir mobil cihaz veya başka bir yere, veri merkezinizdeki çalıştırdıkları olup olmadığını uygulamaların etkileşim gerekir. Azure Service Bus neredeyse çalışan uygulamalar her yerden veri değişimi olanak hedefidir.

Daha önce açıklanan sıraları yanı sıra (birebir), hizmet veri yolu ayrıca diğer iletişim yöntemleri sağlar.

#### <a name="service-bus-relay"></a>Service Bus Geçişi
![Azure Service Bus geçişi](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Şekil: Service Bus geçişi bir Güvenlik Duvarı'nın farklı yüze uygulamalar arasında iletişim sağlar.*

Hizmet veri yolu, güvenlik duvarları üzerinden etkileşim kurmak için güvenli bir yol sağlayarak, geçiş hizmeti üzerinden doğrudan iletişim sağlar. Service Bus geçişlerini uygulamaların bulutta yerine yerel olarak barındırılan bir uç nokta aracılığıyla iletileri değiş tokuş ederek iletişim kurmasına olanak sağlar.

**Service Bus geçiş senaryoları**

Service Bus ile iletişim kurmak uygulamaları Azure uygulamalarını veya bazı başka bulut platformunda çalışan yazılımı olabilir. Bunlar, bulut dışında ancak çalışan uygulamalar da olabilir. Örneğin, kendi veri merkezi içinde bilgisayarlarda ayırma Hizmetleri uygulayan bir uçak düşünün. Uçak hizmetlerin havaalanları, ayırma Aracısı Terminal check-in kiosk dahil olmak üzere birçok istemcilere kullanıma ve belki de müşterilerin telefonları da gerekir. Bu hizmet veri yolu Bunu yapmak için çeşitli uygulamalar arasında kabaca eşleşmiş etkileşimler oluşturma kullanabilir.

#### <a name="service-bus-topics-and-subscriptions"></a>Hizmet Veri Yolu Konuları ve Abonelikleri
![Azure Service Bus konuları](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Şekil: İletileri ve diğer uygulamalar abone olmak için belirli bir ölçüte uyan iletileri alacak şekilde göndermek birden fazla uygulama Service Bus konu başlıklarına verir.*

Service Bus konuları ve abonelikleri adlı Yayımla ve abone ol bir mekanizma sağlar. Diğer uygulamalar bu konuda abonelikleri oluşturabilirsiniz, ancak publish-subscribe ile bir uygulama bir konu başlığına ileti gönderebilir. Bu uygulamalar, aynı iletiyi birden çok alıcılar tarafından okunabilmesini izin vererek kümesi arasında bir çok iletişimi sağlar.

**Service Bus konuları ve abonelikleri senaryoları**

Dilediğiniz zaman burada tüm önemli birçok iletileri vardır, ancak çeşitli aşağı akış sistemleri bu iletişimi, hizmet veri yolu konusu farklı alt kümelerini dinlemek yeterlidir ve abonelikleri iyi bir seçenektir ayarladığınız.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk Hizmetleri](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Şekil: BizTalk Services XML iletileri biçimleri bulutta dönüştürme yeteneği sağlar.*

Bazen farklı ileti biçimi kullanarak iletişim sistemlerini bağlanmanız gerekir. Farklı veritabanı şemaları ve XML biçimleri, ortak bir standart kullanılabilir olduğunda bile messaging sahip işletmeler için yaygın bir durumdur. Çok fazla özel kod yazmak yerine, çeşitli sistemleri tümleştirmek için şirket içi BizTalk Server'ı kullanabilirsiniz.  Azure BizTalk Services hizmetinin ancak bulutta aynı tür sağlar. Yalnızca ne kullanın ve şirket içi olurdu gibi ölçek hakkında endişelenmeniz değil için ödeme.

**Senaryoları BizTalk Hizmetleri**

İşletmeden işletmeye (B2B) etkileşimleri çeviri bu tür sık gerektirir.  Örneğin, Uçaklar oluşturma şirket sipariş bölümlerine çeşitli bölümlerini sağlayıcıdan gerekir. Birçok bölümleri sağlayıcıdan sahip olur.  Bu sipariş doğrudan uçak oluşturucular sistemlerden Üreticiler sistemlere gitmek için otomatik.  Hiçbiri İş çekirdek sistemlerini ve ileti formatları değiştirmek ister ve bu biçimleri aynı olduğunu çok olası değil. BizTalk Services iletiler alabilir ve her iki yönde yeni biçim arasında çeviri. Uçak tedarikçi Çevir çalışmaya yapabilirsiniz veya çeşitli tedarikçileri olabilir, bağlı olarak daha fazla denetim ve gerekli çeviri miktarını isteyen.     

## <a name="compute-assistance"></a>Yardım işlem
Azure her zaman çalışması gerekmez Hizmetleri için desteği sağlar.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Şekil: Azure zamanlayıcı işleri belirli bir süre için belirli bir zamanda zamanlamak için bir yol sağlar.*

Bazen uygulamalar yalnızca belirli bir süre sonunda çalıştırmanız gerekir. Azure üzerinde bu uygulamanın yalnızca 24 x 7 verilerinin işlemek bekleyen çalışmaya devam izin vererek yerine uygulama türü ile paradan tasarruf edebilirsiniz. Azure Zamanlayıcı bir uygulama süresi veya bir takvim bir aralık tabanlı çalıştırılması gereken zaman zamanlamanıza olanak sağlar. Güvenilir ve ağ, makine ve veri merkezi hataları olsa bile bir işlemin çalıştığını doğrulayın. Scheduler REST API, bu eylemleri yönetmek için kullanın.

Zamanlanmış bir uyarı oluştuğunda, Zamanlayıcı belirli bir uç noktası için HTTP veya HTTPS iletileri gönderir veya bir ileti bir depolama kuyruğu koyabilirsiniz.  Bu nedenle erişilebilir bir uç nokta olması ya da sahip bir depolama kuyruğu izlemek uygulamanızın olması gerekir. İletiyi alır sonra için programlanmış hangi eylemi gerçekleştirebilirsiniz.

**Zamanlayıcı senaryoları**

* Yinelenen uygulama eylemleri: örnek olarak, bir hizmet düzenli aralıklarla veri twitter'dan almak ve normal akışa verileri toplayın.
* Günlük Bakım: günlük işleme veya temizleme, yedekleme gerçekleştirme ve diğer aralıklı görev zamanlama.
* Gece çalıştırmak görevler.
* Web uygulamaları görevleri günlük ayıklanması, yedeklemelerin ve diğer bakım görevlerini gerçekleştirme günlükleri, ister. Bir yönetici, örneğin kendi veritabanını sonraki 9 ay boyunca her gün 1 sabah yedeklemek seçebilirsiniz.

Scheduler API'si oluşturma, güncelleştirme, silme, görüntülemek ve iş koleksiyonları ve zamanlanmış işler programlı olarak yönetmek sağlar.

## <a name="performance"></a>Performans
Performans her zaman bir uygulama için önemlidir. Uygulamaları, aynı verileri tekrar tekrar erişim eğilimindedir. Performansı artırmak için bir bunu almak için gereken süreyi en aza uygulamaya yakın bu verilerin bir kopyasını tutmak için yoludur. Azure Bunu yapmak için farklı hizmetler sağlar.

### <a name="azure-caching"></a>Azure Önbelleği
![Azure Önbelleği](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Şekil: Bir Azure uygulama verilerini bellekte önbelleğe ve hatta çok sayıda çalışan rolleri arasında bölmek**

Azure'nın veri yönetimi hizmetlerini SQL Database, tablolar veya BLOB hiçbirinde depolanan verilere-oldukça hızlıdır. Henüz bellekte depolanan verilere erişme daha hızlıdır. Bu nedenle, bir bellek içi kopya sık erişilen veri tutma uygulama performansını iyileştirebilir. Bunu yapmak için Azure'nın bellek içi önbelleğe alma kullanabilirsiniz.

Bulut Hizmetleri uygulama verileri bu önbellekte depolamak sonra kalıcı depolama alanına erişmek gerek kalmadan alma. Önbellek uygulamanızın VM'ler içinde sürdürülebilir veya yalnızca önbelleğe alma için ayrılmış VM'ler tarafından sağlanması. Her iki durumda da önbellek dağıtılabilir, verilerle bir Azure veri merkezinde birden çok VM üzerinden yayılan içeriyor.

Azure zamanla gölgeye farklı önbellek teknolojileri sayısına sahip. Bunlar sunuldu sırayla yoktur paylaşılan, rol yönetilen ve Redis önbelleği. Paylaşılan önbelleğe alma eski bir teknolojidir ve yeni uygulamalar ile oluşturmamalısınız. Yönetilen önbellek rol içi önbelleği, ancak Azure Yönetim Portalı dışında yönetilen hizmet olarak aynı özelliklere sahiptir. Redis önbelleği önizlemede değil. Redis uygulama özelliklerin en büyük sayı olan ve yeni önbelleğe alma kodu yazarken önerilir.

**Azure önbelleği senaryoları**

Bir ürün kataloğu art arda okuyan bir uygulama önbelleğe alma bu tür kullanmanın avantajları, örneğin, bu yana verileri daha hızlı kullanılabilir olması. Kilitleme, salt okunur verileri yanı sıra okuma/yazma ile kullanıldığını izin vererek teknolojisi de destekler. Ve ASP.NET uygulamaları hizmetin yalnızca bir yapılandırma değişikliği ile oturum verilerini depolamak için kullanabilirsiniz.

### <a name="content-delivery-network"></a>Content Delivery Network
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Şekil: Blob kopyalarını dünyanın sitelerde önbelleğe alınabilir.**

Tüm dünyada kullanıcılar tarafından erişileceği blob verilerini depolamak gerektiğini varsayalım. Belki de en son World fincanı eşleşme örneği için sürücü güncelleştirmesi veya popüler bir e-kitap video olur. Birden çok Azure veri merkezlerinde verilerin bir kopyasını saklamak yardımcı olur, ancak çok sayıda kullanıcı varsa, bu yeterli muhtemelen gerekmez. Daha iyi performans için Azure CDN kullanabilirsiniz.

CDN dünya genelinde kopyalarını Azure BLOB Depolama her özellikli siteleri düzinelerce sahiptir. İlk kez kullanıcı world bazı parçası olarak belirli bir blob eriştiğinde içerdiği bilgi bu Coğrafya yerel CDN depolama Azure veri merkezinde kopyalanır. Bundan sonra dünya bu bölümünden erişimleri CDN önbelleğe blob kopyalama kullanır-tamamen en yakın Azure veri merkezine git gerekmez. Dünyanın kullanıcılar tarafından sık erişilen verileri daha hızlı erişim sonucudur.

**CDN senaryoları**

CDN videosunu dünya çapında elde etmek için Media Services ile kullanmak için yaygın bir durumdur. Video genellikle büyük ve çok miktarda bant genişliği gerektirir.  Media Services hakkında başka bir yerde bu sayfada açıklandı.

## <a name="big-data-and-big-compute"></a>Büyük veri ve Big Compute
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![Hdınsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Şekil: Hdınsight, büyük miktarlarda veri toplu işleme ile yardımcı olur.**

Birçok yıldır ilişkisel verileri ilişkisel DBMS ile oluşturulmuş bir veri ambarında depolanan veri analizi toplu gerçekleştirilmedi. Bu tür bir İş analizi hala çok önemlidir ve gelen için uzun bir süredir olacaktır. Ancak ne verileri analiz etmek istediğiniz ilişkisel veritabanları yalnızca işleyemiyor kadar büyük değil mi? Ve veri ilişkisel olmayan varsayalım? Bir veri merkezinde, örneğin, veya algılayıcılar geçmiş olay verilerden veya başka bir olay günlüklerini olabilir. Bu gibi durumlarda, büyük veri sorun olarak bilinir vardır. Başka bir yaklaşım gerekir.

Hadoop büyük veri çözümleme için bugün baskın teknolojisidir. Apache kaynak projeyi açın, bu teknoloji Hadoop dağıtılmış dosya sistemi (HDFS) kullanarak verileri depolar, sonra bu verileri çözümlemek için MapReduce işleri oluşturmalarını sağlar. HDFS veri büyük veri izin vererek, her bir MapReduce işi çalıştırır parçalarını paralel olarak işlenmesi sonra birden çok sunucu arasında yayar.

Hdınsight Apache Hadoop tabanlı Azure'nın hizmetin adıdır. Hdınsight kümesinde veri depolamak ve arasında birden çok VM dağıtmak HDFS olanak sağlar. Ayrıca bir MapReduce işi mantığını bu VM'ler arasında yayar. Şirket içi Hadoop ile veri işlenen yerel olarak mantığı ve çalıştığı üzerindeki veriler aynı VM'e gibi- ve daha iyi performans için paralel. Hdınsight, ayrıca Azure depolama kasası (BLOB'ları kullanan ASV içinde), veri depolayabilirsiniz.  ASV kullanarak Hdınsight kümenize kullanılmadığında silin, ancak hala bulutta verilerinizi korumak için paradan tasarruf sağlar.

Hdınsight Hive veya Pig dahil olmak üzere diğer bileşenleri Hadoop ekosistemi de destekler. Microsoft ayrıca, Hdınsight tarafından üretilen verilerle çalışmak HiveODBC bağdaştırıcısı gibi geleneksel BI araçları kullanarak kolaylaştıran bileşenleri ve Excel ile iş Veri Gezgini oluşturmuştur.

### <a name="high-performance-computing-big-compute"></a>Yüksek performanslı bilgi işlem (Big Compute)
Bulut platformu kullanmak için en cazip yollarından biri, yüksek performanslı bilgi işlem (HPC) ve diğer "Big Compute" uygulamaları çalıştırmaktır. Örnekler endüstri standardı ileti geçirme arabirimi (MPI) yanı sıra, finansal risk modelleri gibi sözde utandırıcı derecede paralel uygulamaları kullanmak için oluşturulan özel mühendislik uygulamalar içerir.

Big Compute özünü kodu aynı anda birçok makinelerde yürütüyor. Çoğu sanal makineleri çalıştıran Bunun anlamı, Azure üzerinde tüm bazı sorunu çözmek için paralel çalışan aynı anda makineleri. Bunu yapmak için kaynaklar ve uygulamalar, yani zamanlamak için işlerini Bu örnekler arasında dağıtmak için bazı yolu gerektirir. Microsoft'un ücretsiz HPC Pack ve diğer bilgi işlem küme çözümleri Azure, yararlanarak kapasite isteğe bağlı bir şirket içi işlem kümesi ya da çalışma Big Compute uygulamalarını bulutta tamamen eklemek için Azure işlem ve altyapı Hizmetleri içinde de gerçekleştirebilirsiniz.

Azure, bir aralık VM örneği boyutlarının CPU çekirdekleri, bellek, disk kapasitesi ve diğer özellikleri farklı uygulamaların gereksinimlerini karşılamak için farklı yapılandırmalarıyla sağlar. Yakın zamanda sunulan A8 ve A9 örnekleri çalışma iyi birçok işlem yoğun iş yükleri ve paralel MPI uygulamaları özellikle, yüksek hız, çok çekirdekli CPU ve büyük miktarlarda bellek sahip oldukları için. Belirli yapılandırmalarında örnekleri düşük gecikme süreli ve yüksek işleme uygulama ağı en yüksek verimlilik paralel MPI uygulamaları için doğrudan uzak bellek erişimi (RDMA) teknolojisini içeren bulutta yararlanın.

Azure Big Compute uygulama geliştiricileri ve iş ortakları ayrıca bir kümesini hesaplama özellikleri, hizmetler, mimari seçim ve geliştirme araçları sunar. Azure özel veri iş akışları içeren özel Big Compute iş akışları destekler ve iş ve görev binlerce için ölçeklenebilir desen zamanlama çekirdek işlem.

## <a name="media"></a>Medya
![Azure medya Hizmetleri](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Şekil: Media Services, video ve diğer medya istemcilere dünyanın sağlamak üzere uygulamalar için bir platformdur.**

Video Internet trafiğini büyük bir bölümünü bugün yapar ve söz konusu yüzdesi yarın daha da büyük olacaktır. Henüz video Web'de sağlayan basit değil. Birçok şifreleme algoritması ve kullanıcının ekranının ekran çözünürlüğünü gibi değişkenleri vardır. Video da gibi pek çok kişiyle çevrimiçi bir filmi izlemek istediğiniz karar verdiğinizde bir Cumartesi gece ani talep WINS'e sahip eğilimindedir.

Kendi popülerliği verildiğinde, birçok yeni uygulama kullanım videonun oluşturulacak sonuç güvenli değil. Henüz bunların tümünün aynı sorunları ve bu sorunları kendi yapar üzerinde her birini yaparak bazıları hiçbir algılama çözmek gerekir. Kullanmak pek çok uygulama yaygın çözümleri sağlayan bir platform oluşturmak daha iyi bir yaklaşımdır. Ve bu platformu bulutta oluşturma Temizle bazı avantajları vardır. Kullandıkça Öde temelinde geniş çapta kullanılabilir olabilir ve ekran uygulamaları genellikle yüz isteğe bağlı sonuçlarındaki işleyebilir.

Azure Media Services, bu sorunu giderir. Yaşam oluşturma ve video ve diğer medya kullanarak uygulamaları çalıştırma kişiler için daha kolay hale getirmek bulut bileşenleri kümesi sağlar.

Aşağıdaki şekilde gösterildiği gibi Media Services, video ve diğer medya ile çalışan uygulamalar için bir bileşen kümesi sağlar. Örneğin, bir ortam içerir video medya (nerede depolandığı Azure BLOB'ları) hizmetlerine karşıya yüklemek için bileşeni, çeşitli video ve ses biçimlerini destekleyen bir kodlama bileşeni, dijital hak yönetimi sağlayan bir içerik koruma bileşeni, video akışa ads eklemek için bir bileşen, akış bileşenleri ve daha fazla alma. Microsoft iş ortaklarını da platformu için bileşenleri sağlayın, sonra bu bileşenleri dağıtma ve onların adına faturalandırmak Microsoft sahip.

Bu platform kullanan uygulamaları Azure veya başka bir yerde çalıştırabilirsiniz. Örneğin, bir masaüstü uygulaması bir video üretim ev görüntü Media Services için karşıya yükleme kullanıcılarına sağlayabilir için daha sonra işlemek, çeşitli yollarla. Alternatif olarak, Azure üzerinde çalışan bir bulut tabanlı içerik yönetim hizmeti Hizmetleri'ni işlemek ve video dağıtmak için medya kullanır. Yerde çalışır ve bunu yapar, her bir uygulama kullanmak için gereken hangi bileşenlerin RESTful arabirimleri aracılığıyla erişme seçer.

Ne üreten dağıtmak için bir uygulamayı Azure CDN, başka bir CDN kullanan veya yalnızca BITS doğrudan kullanıcılara gönderin. Ancak, var. aldığında görüntü Media Services'i kullanarak oluşturulan Windows, Macintosh, HTML 5, iOS, Android, Windows Phone, Flash ve Silverlight dahil olmak üzere çeşitli istemci sistemleri tarafından tüketilebilir. Modern medya uygulamaları oluşturmayı kolaylaştırmak için belirtilir.

**Başvuruları**

Media Services nasıl çalıştığını daha görsel görünümünü için karşıdan [Azure Media Services posteri][Azure Media Services Poster].

## <a name="commerce"></a>Ticaret
Hizmet olarak yazılım neden uygulamaları nasıl oluşturuyoruz dönüştürme. Bu ayrıca nasıl biz uygulamaları satmak dönüştürme. Bulutta bir SaaS uygulaması yaşadığı olduğundan, olası müşterilerine çevrimiçi çözümleri görünmelidir anlamlı olur. Ve bu değişikliği veri uygulamalar için de geçerlidir. Neden kişiler buluta piyasada veri kümeleri için görünüyor döndürmemelidir? Microsoft bu sorunları ile her ikisi de adresleri [Azure Marketi](https://azure.microsoft.com/marketplace/).

![Azure ticaret](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Şekil: Azure Market ve Azure depolama bulmanıza ve Azure uygulamalarını ve ticari veri kümeleri satın alın ve bunları Azure uygulamalarınızı bir parçası olarak kullanın olanak tanır.**

İkisi arasındaki farkı Market dışında Azure Yönetim Portalı olduğu, ancak mağazası portalı içinde erişilebilir olduğunu. Olası müşteriler kendi gereksinimlerini karşılayacak Azure uygulamalarını bulmak için arama yapabilirsiniz. Müşteriler, demografik veriler, finansal verileri, coğrafi veriler ve benzeri ticari veri kümeleri için de arayabilirsiniz. Bunlar bir şey istedikleri bulduğunuzda, bunlar, ya da satıcıdan doğrudan Market veya deposu web konumu veya bazı durumlarda Yönetim Portalı'ndan erişebilir. Uygulamaları, web arama sonuçlarını erişim onların Market üzerinden Bing arama API de kullanabilirsiniz.

**Ticaret senaryoları**

SendGrid, e-posta göndermenize olanak sağlayan Azure depolama için kullanılan bir uygulamadır. Güvenilir teslim ve istatistikler gibi ek işlevsellik sağlar.  Bu uygulama ve ilgili hizmetler satın yerine, böyle bir altyapı kendiniz yapılandırmak deneyin.  

## <a name="getting-started"></a>Başlarken
Büyük-resmi sahip olduğunuza göre sonraki adıma ilk Azure uygulamanızı yazmaktır. Dilinizi seçin [uygun SDK'sı Al](https://azure.microsoft.com/en-us/downloads/)ve bunun için gidin. Bulut bilgi işlem varsayılandır yeni--hemen kullanmaya başlayın.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
