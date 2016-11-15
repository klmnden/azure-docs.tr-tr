---
title: Azure RemoteApp ile SQL Azure | Microsoft Belgeleri
description: "Azure RemoteApp ile SQL Azure kullanmayı öğrenin."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 08/15/2016
ms.author: elizapo
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: cfd3da08a8c8674e686ae2933db331809fb0e34d


---
# <a name="sql-azure-with-azure-remoteapp"></a>Azure RemoteApp ile SQL Azure
> [!IMPORTANT]
> Azure RemoteApp kullanımdan kaldırılıyor. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Genellikle, müşteriler kendi Windows uygulamalarını Azure RemoteApp ile bulutta barındırma istediklerinde, tam bir bulut dağıtımı için SQL sunucuları gibi verilerini de buluta geçirmek isterler. Bu, Azure RemoteApp kullanarak her zaman herhangi bir cihazdan erişilebilir tümü bulutta barındırılan çözüme olanak sağlar. Aşağıda, bu işlemde size yardımcı olacak adımlarla birlikte bağlantı ve kılavuzlar verilmiştir.  

## <a name="migrate-your-sql-data"></a>SQL verilerinizi geçirme
[SQL Server veritabanını Azure SQL Database’e geçirme](../sql-database/sql-database-cloud-migrate.md) ile başlayın. 

## <a name="configure-azure-remoteapp"></a>Azure RemoteApp’i yapılandırma
Windows uygulamanızı Azure RemoteApp’te barındırma Aşağıda çok üst düzey adım adım açıklama bulunmaktadır:

1. [Azure RemoteApp şablonu VM](remoteapp-imageoptions.md) oluşturun. 
2. Gerekli uygulamayı VM'ye yükleyin.
3. SQL DB’ye bağlanacak şekilde uygulamayı yapılandırın ve çalıştığını doğrulayın.
4. Sysprep ve VM’yi kapatın. Azure ile kullanmak için bunu bir görüntü olarak yakalayın. **Not:** Uygulamanın sysprep işlemiyle veritabanı bağlantısı bilgilerini tutabileceğinden emin olmanız gerekir. Uygulama DB bağlantısı bilgilerini tutamıyorsa, bağlantı dizesini nasıl belirtebileceğimizi denetlemek için uygulamanın satıcısına başvurmak isteyebilirsiniz.
5. SQL Azure dağıtımınızın bulunduğu uygun coğrafi konumu seçerek özel görüntüyü Azure RemoteApp kitaplığınıza aktarın. 
6. Yukarıdaki şablonu kullanarak aynı veri merkezindeki bir RemoteApp koleksiyonunu SQL Azure dağıtımınız olarak dağıtın ve uygulamayı yayımlayın. Aynı veri merkezindeki Azure RemoteApp’i SQL Azure dağıtımınız olarak dağıtmak en hızlı bağlantıyı hızını ve düşük gecikmeyi sağlar. 

## <a name="app-and-sql-configuration-considerations"></a>Uygulama ve SQL yapılandırmada dikkat edilmesi gerekenler
RemoteApp ile Azure SQL kullanırken dikkate alınması gereken birkaç nokta vardır:

[Azure SQL veritabanı güvenlik duvarı yapılandırmayı](../sql-database/sql-database-firewall-configure.md) öğrenin. Makaleden bir alıntı, “Başlangıçta, Azure SQL Database sunucusuna tüm erişiminiz güvenlik duvarı tarafından engellenir” demektedir. Azure SQL Database sunucunuzu kullanmaya başlamak için, Klasik Portal’a gitmeli ve Azure SQL Database sunucunuza erişiminizi etkinleştirecek olan bir ya da daha fazla sunucu düzeyi güvenlik duvarı kuralını belirlemelisiniz. İnternet’ten gelen hangi IP adresi aralıklarının izinli olduğuna ve Azure uygulamalarının Azure SSQL Database unucunuza bağlanmaya çalışıp çalışamayacaklarına ilişkin güvenlik duvarı kurallarını belirleyin.”

Ayrıca, bir bilgisayar İnternet'ten, veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı, isteğin kaynak IP adresini tam sunucu düzeyi ve (gerekliyse) veritabanı düzeyi güvenlik duvarı kurallarına karşı denetler. “İstek IP adresi sunucu düzeyi güvenlik duvarı kurallarında belirtilen aralıklardan biri içinde ise, Azure SSQL Database sunucunuza erişim izni verilir.” Dolayısıyla, yalnızca IP adresi kaynaklarını değil IP Aralıklarını da kullanabiliriz.

IP aralığını belirtmek için, [Nasıl yapılır: Azure Portal’ı kullanarak SQL Database güvenlik duvarı ayarlarını yapılandırma](../sql-database/sql-database-configure-firewall-settings.md) adım adım yönergelerini izleyin. SQL Güvenlik Duvarı kurallarını yapılandırırken, lütfen Azure RemoteApp koleksiyonu için belirtilen alt ağın IP aralığını belirtin. Bu, dinamik olarak atanan IP Adresleri olmasına rağmen, ARA sunucularının SQL DB’ye bağlanmasına izin vermelidir.

## <a name="troubleshooting"></a>Sorun giderme
Azure’da ya da şirket içinde barındırılan bir SQL veritabanına bağlanan Azure RemoteApp’te barındırılan bir istemci uygulamasını kullanma deneyimi yavaş ise, bunun birkaç nedeni olabilir.  

* Cihazınızdan Azure’a olan ağ gecikmesi yüksektir. En iyi performans için mümkün olan en iyi ve en hızlı ağ bağlantısına yaklaşın. Cihazlarınızın Azure veri merkezine olan gecikmesini test etmek için genel araç olarak [azurespeed.com](http://azurespeed.com/)’u kullanın.  
* Azure RemoteApp’te barındırılan istemci uygulaması gerilim altında. Premium faturalama gibi farklı bir fatura planı seçmek performansı iyileştirir. Başka bir çözüm, uygulamanızın kullandığı kaynakları izlemektir: etkin bir oturum sırasında SAS ekranını başlatacak olan ctrl-alt-end tuşlarına basın, Görev Yöneticisi'ni seçin ve uygulamanız için kaynak kullanımını gözleyin 
* SQL server gerilim altından veya en iyi hale getirilmemiş. Sorun giderme için SQL yönergelerini izleyin. 




<!--HONumber=Nov16_HO2-->


