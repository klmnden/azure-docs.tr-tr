#Azure Ağı

Azure uygulamalarına ve verilerine bağlanmanın en kolay yolu sıradan bir İnternet bağlantısıdır. Ancak bu basit çözüm her zaman en iyi yaklaşım değildir. Azure, kullanıcıları Azure veri merkezlerine bağlamaya yönelik teknolojiler de sağlar.  Bu makalede bu teknolojiler gözden geçirilmektedir. 

##İçindekiler Tablosu      
- [Azure Sanal Ağ](#Vnet)
- [Azure Trafik Yöneticisi](#TrafficMngr)

<a name="Vnet"></a>
##Azure Sanal Ağ

Azure, Microsoft veri merkezlerinde çalışan sanal makineler (VM) oluşturmanızı sağlar. Kuruluşunuzun, firmanızdaki çalışanlar tarafından kullanılacak kurumsal uygulamaları veya diğer yazılımları çalıştırmak için bu sanal makineleri kullanmak istediğini varsayalım. Örneğin, bulutta bir SharePoint grubu oluşturmak veya bir stok yönetimi uygulaması çalıştırmak istiyor olabilirsiniz. Kullanıcılarınız açısından hayatı mümkün olduğunca kolaylaştırmak için bu uygulamalara sanki kendi veri merkezinizde çalışıyormuş gibi erişim sağlanabilmesini isteyeceksiniz.

Bu tür sorunun standart bir çözümü vardır: Sanal özel ağ (VPN) oluşturun. Günümüzde her ölçekten kuruluş, örneğin şubelerindeki bilgisayarları ana şirketteki veri merkezine bağlamak için bunu yapmaktadır. Bu aynı yaklaşım, Şekil 1'de gösterildiği gibi Azure sanal makineleriyle de işe yarayabilir.

<a name="Fig1"></a>
  
![01_Networking][01_Networking]

**Şekil 1: Azure Sanal Ağ, bulut ortamında şirket içi veri merkezinize bağlı bir sanal ağ oluşturmanıza olanak sağlar.**

Şekilde gösterildiği gibi, Azure Sanal Ağ bir grup sanal makine etrafında *sanal ağ veya VNET* adı verilen mantıksal bir sınırı Azure veri merkezi içinde oluşturmanıza izin verir. Sonra bu VNET ile yerel ağınız arasında bir IPsec bağlantısı kurmanızı sağlar.  VNET'teki sanal makineleri oluşturmak için Azure Sanal Makineleri, Azure Bulut Hizmetleri veya ikisi birden kullanılabilir. Diğer bir deyişle, Azure'un Hizmet Olarak Altyapı (IaaS) teknolojisi veya Hizmet Olarak Platform (PaaS) teknolojisi kullanılarak oluşturulabilirler.
Hangi tercihte bulunursanız bulunun IPsec bağlantısını oluşturmak için bir VPN ağ geçidi cihazı, yerel ağınıza takılan özelleştirilmiş donanım ve ayrıca ağ yöneticinizin hizmetleri gerekir. Bu bağlantı hazır olduğunda VNET'inizde çalışan Azure sanal makineleri kuruluşunuzdaki ağın diğer herhangi bir parçası gibi görünür.

[Şekil 1](#Fig1)'de gösterildiği gibi, Azure sanal makineleri için IP adreslerini kendi ağınızda kullanılan aynı IP adresi alanından atarsınız. Burada gösterilen, özel IP adreslerinin kullanıldığı senaryoda buluttaki sanal makineler sadece bir başka IP alt ağıdır. Yerel ağınızda çalışan yazılımlar bu sanal makineleri, aynı geleneksel VPN'lerde olduğu gibi yerel olarak görür. Ayrıca, bu bağlantı IP düzeyinde gerçekleştiğinden her iki taraftaki sanal ve fiziksel makinelerin herhangi bir işletim sistemiyle çalışabildiğine dikkat edilmesi önem taşır. Windows Server veya Linux çalıştıran Azure sanal makineleri Windows, Linux veya diğer sistemleri çalıştıran şirket içi makinelerle etkileşimde bulunabilir. Ayrıca, bulut sanal makinelerini ve içerdikleri uygulamaları yönetmek için, System Center ve diğerlerini de kapsayan ana akım yönetim araçlarını kullanmak da mümkündür.

Azure Sanal Ağ kullanımı birçok durumda mantığa uygundur. Daha önce belirtildiği gibi bu yaklaşım, kurumsal kullanıcıların bulut uygulamalarına daha kolay erişebilmesini sağlar. Bu kullanım kolaylığının önemli bir yönü, çalıştırdıkları uygulamalarda çoklu oturum açma olanağını kullanıcılara sağlamak amacıyla Azure sanal makinelerini var olan şirket içi Active Directory etki alanının bir parçası haline getirme kabiliyetidir. Tercih etmeniz halinde, bulutta da bir Active Directory etki alanı oluşturabilir ve sonra bu etki alanını şirket içi ağınıza bağlayabilirsiniz.

Azure veri merkezinde VNET oluşturulması istendiğinde ulaşılabilen geniş bir kaynak havuzuna etkinlikle erişiminizi sağlar. Sanal makineleri istendiğinde oluşturabilir, çalıştıkları sürece ödeme yapabilir ve artık ihtiyacınız kalmadığında bunları kaldırabilirsiniz (ve ödeme yapmayı bırakabilirsiniz). Geliştirme ekiplerinin yeni yazılım oluşturması gibi, önceden yapılandırılmış makineye hızlı erişime gerek duyulan senaryolar için bunun yararı olabilir. Gerek duydukları kaynakları yerel bir yöneticinin ayarlamasını beklemek yerine bu kaynakları genel bulut ortamında kendileri oluşturabilirler. 

Üstelik Sanal Ağ, Azure sanal makinelerinin şirket içi kaynaklarda nasıl yerel gibi görünmesini sağlıyorsa, bunun tersi de geçerlidir: Azure VNET'inizde çalışan uygulamalar artık, yerel ağınızda çalışan yazılımları yerel gibi görür. Örneğin, bulutta çalıştırılmasının daha düşük maliyetli olacağına karar verdiğinizden, var olan bir şirket içi uygulamayı Azure'a taşımak istediğinizi varsayalım. Ancak, bu uygulamanın kullandığı verilerin kanunen şirket içinde depolanması gerekiyorsa ne olacak? Böyle bir durumda Sanal Ağı kullanılması bulut uygulamasının şirket içi bir veri tabanı sistemini yerel gibi görmesini sağlar ve bu sisteme erişim dolaysız olur. Tercihiniz hangi senaryo olursa olsun sonuç aynıdır: Azure kendi veri merkezinizin bir uzantısı olur.

<a name="TrafficMngr"></a>
##Azure Trafik Yöneticisi

Başarılı bir Azure uygulaması oluşturduğunuzu düşünelim. Bu uygulamanız dünya çapında birçok ülkede çok sayıda kişi tarafından kullanılıyor. Bu harika bir sonuç, ancak çoğu zaman olduğu gibi başarı yeni sorunları beraberinde getirir. Örneğin, uygulamanız büyük bir olasılıkla dünyanın farklı kesimlerinde çok sayıda Azure veri merkezinde çalışıyor. Kullanıcılarınızın her zaman en iyi deneyimi yaşamasını sağlamak için kullanıcı isteği trafiğini bu veri merkezleri geneline akıllı bir şekilde nasıl yönlendirirsiniz?

Azure Trafik Yöneticisi bu sorunu çözmek üzere tasarlanmıştır. Şekil 2'de bunun nasıl yapıldığı gösterilmektedir.

<a name="Fig3"></a>
   
![03_TrafficManager][03_TrafficManager]
   
**Şekil 2: Azure Trafik Yöneticisi, kullanıcılardan gelen istekleri bir uygulamanın farklı Azure veri merkezlerinde çalışan örneklerine akıllı bir biçimde yönlendirir.**

Bu örnekte uygulamanız ikisi ABD'de, biri Avrupa'da ve biri de Asya'da olmak üzere dört veri merkezine yayılmış sanal makinelerde çalışmaktadır. Berlin'deki bir kullanıcının uygulamaya erişmek istediğini varsayalım. Trafik Yöneticisi kullanıyorsanız olacaklar aşağıdaki gibidir.

Her zaman olduğu gibi, kullanıcı sistemi uygulamanın DNS adını arar (1. Adım). Bu sorgu Azure DNS sistemi yönlendirilir (2. Adım) ve sonra da sistem uygulama için Trafik Yöneticisi ilkesini arar. Her bir ilke, gerek bir grafik arabirimi gerekse REST API'si aracılığıyla belirli bir Azure uygulamasının sahibi tarafından oluşturulur. İlke nasıl oluşturulursa oluşturulsun, üç yük dengeleme seçeneğinden birini belirtir:

- **Performans:** Tüm istekler, kullanıcı sisteminden en düşük gecikmeyle veri merkezine gönderilir. 
- **Yük Devretme:** Veri merkezi kullanılabilir durumda olduğu sürece, tüm istekler bu ilkeyi oluşturanın belirttiği veri merkezine gönderilir. Bu örnekte istekler, ilkeyi oluşturanın tanımladığı öncelik sırasına göre diğer veri merkezlerine yönlendirilmektedir.
- **Hepsini Bir Kez Deneme:** Tüm istekler uygulamanın çalıştığı tüm veri merkezlerine eşit olarak yayılır.

Doğru ilkeye sahip olduğunda, Trafik Yöneticisi üç seçenekten hangisinin belirtildiğine bağlı olarak bu isteğin gitmesi gereken veri merkezini hesaplar (3. Adım). Daha sonra seçilen veri merkezinin konumunu kullanıcıya döndürür (4. Adım) ve kullanıcı da uygulamanın bu örneğine erişim sağlar (5. Adım).

Bunun işe yaraması için Trafik Yöneticisi, her bir veri merkezinde uygulamanın hangi örneklerinin hazır ve çalışır durumda olduğunun mevcut tablosuna sahip olmalıdır. Trafik Yöneticisi bunu mümkün kılmak için uygulamanın her bir kopyasına düzenli aralıklarla HTTP GET ile ping uygular ve sonra da yanıt alıp alamadığını kaydeder. Bir uygulama örneği yanıt vermeyi bırakırsa, ping'lere yanıt vermeye devam edinceye kadar Trafik Yöneticisi kullanıcıları bu uygulama örneğine yönlendirmeyi durdurur. 

Her uygulama Trafik Yöneticisi'ne gerek duyulacak kadar büyük veya global değildir. Uygun olan uygulamalar içinse bu hizmet çok yararlı olabilir.

[01_Networking]: ./media/azure-networking/Networking_01Networking.png
[03_TrafficManager]: ./media/azure-networking/Networking_03TrafficManager.png



