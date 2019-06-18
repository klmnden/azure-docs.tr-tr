---
title: Azure arama dizini oluşturma - azure SQL sanal makinesi sanal makine bağlantısı arama
description: Şifrelenmiş bağlantıları etkinleştirmek ve bağlantılar için SQL Server Azure sanal makinesinde (VM) üzerinde Azure Search dizin oluşturucu izin vermek için Güvenlik Duvarı'nı yapılandırın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/04/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 90e5a133bac519cbc5ab2d7b112d51a019e8f698
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60871289"
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Bir Azure sanal makinesinde SQL Server için Azure Search dizin oluşturucu arasında bağlantı yapılandırma
Belirtilen [Azure Search dizin oluşturucuları kullanarak Azure SQL veritabanına bağlanma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), dizin oluşturucular karşı oluşturma **Azure vm'lerde SQL Server** (veya **SQL Azure Vm'leri** kısaca) desteklenir Azure Search tarafından ancak ilk halletmeniz için birkaç güvenlikle ilgili Önkoşullar vardır. 

Bir VM'de SQL Server için Azure Search bağlantılarını ortak bir internet bağlantısı olduğundan. Tüm güvenlik önlemlerini normalde bu bağlantılar için izlemeniz gerekir burada da geçerli olur:

+ Bir sertifikadan bir [sertifika yetkilisi sağlayıcısının](https://en.wikipedia.org/wiki/Certificate_authority#Providers) Azure VM'de SQL Server örneğinin tam etki alanı adı.
+ VM üzerinde sertifikayı yükleyin ve ardından etkinleştirin ve sanal makinede bu makaledeki yönergeleri kullanarak şifrelenmiş bağlantılar yapılandırın.

## <a name="enable-encrypted-connections"></a>Şifrelenmiş bağlantılarını etkinleştirme
Azure arama, tüm dizin oluşturucu istekleri için ortak bir internet bağlantısı üzerinden şifrelenmiş bir kanal gerektirir. Bu bölümde, bunun çalışmasını sağlamak için adımları listelenir.

1. Konu adı tam etki alanı adıdır (FQDN) Azure VM'nin doğrulamak için sertifika özelliklerini denetleyin. Özelliklerini görüntülemek için CertUtils gibi bir araç veya Sertifikalar ek bileşenini kullanabilirsiniz. VM hizmet dikey penceresinin Essentials bölümünden de FQDN alabilirsiniz **genel IP adresi/DNS ad etiketi** alanına [Azure portalında](https://portal.azure.com/).
   
   * Yeni kullanılarak oluşturulan VM'ler için **Resource Manager** şablonu, FQDN olarak biçimlendirildiğinde `<your-VM-name>.<region>.cloudapp.azure.com`
   * Olarak oluşturulmuş eski Vm'leri için bir **Klasik** VM, FQDN olarak biçimlendirildiğinde `<your-cloud-service-name.cloudapp.net>`.

2. SQL Server'ın Kayıt Defteri Düzenleyicisi'ni (regedit) kullanarak sertifikayı kullanacak şekilde yapılandırın. 
   
    SQL Server Configuration Manager bu görev için sık sık kullanılsa da, bu senaryo için kullanamazsınız. Azure VM'de FQDN'sini (etki alanı yerel bilgisayarda veya katılmış ağ etki alanı tanımladığı) VM tarafından belirlenen şekilde bir FQDN eşleşmediği için içeri aktarılan sertifikayı bulmaz. Adları eşleşmediğinde regedit sertifikasını belirtmek için kullanın.
   
   * Regedit içinde bu kayıt defteri anahtarına gidin: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     `[MSSQL13.MSSQLSERVER]` Bölümü sürümü ve örnek adı göre değişir. 
   * Değerini **sertifika** anahtarını **parmak izi** VM'ye aktardığınız SSL sertifikası.
     
     Parmak izi, diğerlerine biraz daha iyi almak için birkaç yolu vardır. Buradan kopyalarsanız **sertifikaları** ek bileşeninde MMC, büyük olasılıkla görünmez önde gelen bir karakter oluşturan çeker [Bu destek makalesinde açıklandığı şekilde](https://support.microsoft.com/kb/2023869/), bağlantı denediklerinde hatayla sonuçlanır . Birkaç geçici çözüm bu sorunu düzeltmek için mevcut. Üzerinden geri alın ve sonra da ilk karakteri regedit anahtar değer alanında önde gelen karakter kaldırmak için parmak izi yeniden yazmak için en kolay yoldur. Alternatif olarak, parmak izini kopyalayın için farklı bir aracı kullanabilirsiniz.

3. Hizmet hesabı için izinler verir. 
   
    SQL Server hizmet hesabı SSL sertifikasının özel anahtarı üzerinde uygun izin verilir emin olun. Bu adım kolayca gözden kaçabilir, SQL Server başlatılmaz. Kullanabileceğiniz **sertifikaları** ek bileşenini veya **CertUtils** bu görev için.
    
4. SQL Server hizmetini yeniden başlatın.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>VM'de SQL Server bağlantısı yapılandırma
Azure Search tarafından gerekli şifreli bağlantı kurduktan sonra var. ek yapılandırma adımları için Azure vm'lerde SQL Server iç Henüz yapmadıysanız, bu makale bunlardan birini kullanarak yapılandırmayı tamamlamak için sonraki adım olan:

* İçin bir **Resource Manager** VM bkz [Resource Manager'ı kullanarak azure'da SQL Server sanal makinesine bağlanma](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* İçin bir **Klasik** VM bkz [Klasik Azure üzerinde SQL Server sanal makinesine bağlanma](../virtual-machines/windows/classic/sql-connect.md).

Özellikle, her bir makaleye "internet üzerinden bağlanma" bölümüne bakın.

## <a name="configure-the-network-security-group-nsg"></a>Ağ güvenlik grubu (NSG) yapılandırma
Azure VM diğer taraflar için erişilebilir hale getirmek için NSG ve karşılık gelen Azure uç noktası veya erişim denetimi listesi (ACL) yapılandırmak alışılmadık bir durum değildir. Önce SQL Azure VM'nize bağlanmak için kendi uygulama mantığını izin verecek şekilde yaptığınızdan yüksektir. Bir Azure Search bağlantı, SQL Azure VM için farklı bir değildir. 

Aşağıdaki bağlantıları VM dağıtımları için NSG yapılandırma yönergeleri sağlar. ACL IP adresine göre bir Azure Search uç nokta için bu yönergeleri kullanın.

> [!NOTE]
> Arka plan bilgileri için bkz. [bir ağ güvenlik grubu nedir?](../virtual-network/security-overview.md)
> 
> 

* İçin bir **Resource Manager** VM bkz [ARM dağıtımları için Nsg'ler oluşturma](../virtual-network/tutorial-filter-network-traffic.md). 
* İçin bir **Klasik** VM bkz [Klasik dağıtımlar için Nsg'ler oluşturma](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP adresleme sorun ve olası geçici çözümlerin farkında, kolayca üstesinden bazı zorluklar çıkarabilir. Kalan bölümler IP adresleri ACL sorunları işleme için öneriler sağlar.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Arama hizmeti IP adresi için erişimi kısıtlama
SQL Azure Vm'leriniz için bağlantı isteklerini açık yapmak yerine ACL arama hizmetinizdeki IP adresine erişimi kısıtlama kesinlikle öneririz. FQDN ping göndererek, IP adresini kolayca bulabilirsiniz (örneğin, `<your-search-service-name>.search.windows.net`) arama hizmetinizin.

#### <a name="managing-ip-address-fluctuations"></a>IP adresi dalgalanmaları yönetme
Arama hizmetiniz, yalnızca bir arama birimi (diğer bir deyişle, bir çoğaltma ve bir bölüm) varsa, rutin hizmet yeniden başlatıldığında, arama hizmetinizin IP adresi ile var olan bir ACL geçersiz kılmalarını sırasında IP adresi değişir.

Bir yolu, sonraki bağlantı hatayı önlemek için birden fazla çoğaltma ve bir bölüm Azure Search'te kullanmaktır. Bunun yapılması maliyetini artırır, ancak aynı zamanda bir IP adresi sorunu çözer. Birden fazla arama birimi varsa, IP adresleri Azure arama'yı değiştirmeyin.

Başarısız ve NSG ACL'lerinde sonra yeniden bağlanmaya izin ver ikinci bir yaklaşımdır. Ortalama olarak haftada birkaç değiştirmek için IP adresleri bekleyebilirsiniz. Denetlenen bir seyrek olarak dizinleme yapan müşteriler için bu yaklaşım uygun olabilir.

Bir üçüncü uygulanabilir (ancak özellikle güvenli olmayan) burada arama hizmetiniz sağlandıktan Azure bölgesinin IP adresi aralığını belirtmek için yaklaşımdır. İçinden genel IP adresleri Azure kaynaklarına ayrılan IP aralıklarının Listesi sayfasında yayımlanır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Azure Search portal IP adreslerini içerir
Bir dizin oluşturucu oluşturmak için Azure portalını kullanıyorsanız, Azure Search portal mantığı oluşturma zamanı sırasında SQL Azure VM erişimi de gerekir. Azure search portal IP adresleri, ping göndererek bulunabilir `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Sonraki adımlar
Ortada yapılandırma ile artık SQL Server Azure sanal makinesinde Azure Search dizin oluşturucu için veri kaynağı olarak belirtebilirsiniz. Bkz: [Azure Search dizin oluşturucuları kullanarak Azure SQL veritabanına bağlanma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) daha fazla bilgi için.

