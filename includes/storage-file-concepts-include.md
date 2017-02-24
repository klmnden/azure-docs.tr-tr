## <a name="what-is-azure-file-storage"></a>Azure File Storage nedir?
File Storage standart SMB 2.1 veya SMB 3.0 protokolünü kullanan uygulamalar için paylaşılan depolama alanı sağlar. Microsoft Azure Virtual Machines ve Cloud Services bağlı paylaşımlar üzerinden uygulama bileşenleri arasında dosya verileri paylaşabilir ve şirket içi uygulamalar, File Storage API’si üzerinden dosya verilerine erişebilir.

Azure Virtual Machines veya Cloud Services’ta çalışan uygulamalar, bir masaüstü uygulamasının tipik bir SMB paylaşımına bağlandığı şekilde buluta, dosya verilerine erişmek amacıyla File Storage paylaşımı bağlayabilir. Ardından herhangi bir sayıda Azure sanal makinesi veya rolü eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

SMB protokolünü kullanan Azure’da File Storage paylaşımı standart dosya paylaşımı olduğu için Azure’da çalışan uygulamalar dosyanın G/Ç API’leri üzerinden paylaşımdaki veriye erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. BT Uzmanları Azure uygulamalarını yönetmenin bir parçası olarak File Storage paylaşımlarını oluşturmak, bunları bağlamak ve yönetmek için PowerShell.cmdlet’leri kullanabilir. Bu kılavuzda her ikisinin de örnekleri gösterilmektedir.

File Storage’ın yaygın kullanımları şunlardır:

* Pahalı yeniden yazmalar olmadan Azure Virtual Machines veya Cloud Services’ta çalıştırılacak dosya paylaşımlarına bağlı şirket içi uygulamaların geçirilmesi
* Yapılandırma dosyalarında olduğu gibi paylaşılan uygulama ayarlarının depolanması
* Paylaşılan konumda günlükler, ölçümler ve kilitlenme dökümleri gibi tanılama verilerinin depolanması 
* Azure Virtual Machines veya Cloud Services’ın geliştirilmesi ya da yönetilmesi için gereken araçların ve yardımcı programların depolanması

## <a name="file-storage-concepts"></a>File Storage kavramları
File Storage’da şu bileşenler bulunur:

![files-concepts](./media/storage-file-concepts-include/files-concepts.png)

* **Depolama Hesabı:** Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/storage-scalability-targets.md).
* **Paylaşım:** File Storage paylaşımı Azure’da SMB dosyası paylaşımıdır. 
  Tüm dizinler ve dosyalar üst paylaşımda oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir; paylaşım da 5 TB toplam kapasiteye kadar dosya paylaşımıyla sınırsız sayıda dosya depolayabilir.
* **Dizin:** Dizinlerin isteğe bağlı hiyerarşisi. 
* **Dosya:** Paylaşımda bir dosya. Bir dosyanın boyutu 1 TB'ye kadar olabilir.
* **URL biçimi:** Dosyalar şu URL biçimi kullanılarak adreslenebilir:   
  https://`<storage
  account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
  
  Aşağıdaki örnek URL yukarıdaki diyagramda yer alan dosyalardan birini belirtmek için kullanılabilir:  
  `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

Dosya paylaşımlarının, dizinlerin ve dosyaların adlandırılması hakkında ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](http://msdn.microsoft.com/library/azure/dn167011.aspx).


<!--HONumber=Nov16_HO3-->


