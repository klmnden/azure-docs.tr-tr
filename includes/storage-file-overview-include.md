## <a name="overview"></a>Genel Bakış
Azure File Storage, standart [Sunucu İleti Blogu (SMB) Protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) kullanılarak bulutta dosya paylaşımı sunan bir hizmettir. SMB 2.1 ve SMB 3.0 desteklenir. Azure File Storage, Azure’a dosya paylaşımı kullanan eski uygulamaları maliyetli yeniden yazdırmaya ihtiyaç duymadan ve hızla taşıyabilmenizi sağlar. Azure Virtual Machines’de, Cloud Services’da veya şirket içi istemcilerde çalışan uygulamalar, bir masaüstü uygulamanın tipik SMB paylaşımı bağladığı gibi buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

Dosya depolama paylaşımı, standart SMB dosya paylaşımı olduğundan Azure'da çalışan uygulamalar, dosya sisteminin G/Ç API'leri üzerinden paylaşımdaki verilere erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. BT Uzmanları Azure uygulamalarını yönetmenin bir parçası olarak File Storage paylaşımlarını oluşturmak, bunları bağlamak ve yönetmek için PowerShell.cmdlet’leri kullanabilir.

[Azure Portal](https://portal.azure.com)’ı, Azure Storage PowerShell cmdlet’lerini, Azure Storage istemcisi kitaplıklarını veya Azure Storage REST API’sini kullanarak Azure dosya paylaşımları oluşturabilirsiniz. Ayrıca, bu dosya paylaşımları SMB paylaşımları olduğundan bunlara standart ve benzer dosya sistemi API’leriyle erişebilirsiniz.



<!--HONumber=Jan17_HO3-->


