---
title: Azure Search SQL VM bağlantı | Microsoft Docs
description: Şifreli bağlantıları etkinleştirmek ve bir Azure Search oluşturucuda gelen bağlantılara SQL Server'a Azure sanal makine (VM) izin verecek Güvenlik Duvarı'nı yapılandırın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: heidist
ms.openlocfilehash: 34c5d1999625d1728e884adb794af235ba415c26
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Bir Azure VM üzerinde SQL Server bir Azure Search dizin oluşturucu arasında bağlantı yapılandırma
İçinde belirtildiği gibi [dizin oluşturucuları kullanarak Azure Search'te Azure SQL veritabanına bağlanma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), dizin oluşturucular karşı oluşturma **Azure Vm'lerde SQL Server** (veya **SQL Azure VM'ler** kısaca) Azure araması tarafından desteklenen ancak ilk halletmeniz için birkaç güvenlikle ilgili önkoşulları vardır. 

**Görev süresi:** yaklaşık 30 dakika zaten bir sertifika VM'de yüklü.

## <a name="enable-encrypted-connections"></a>Şifreli bağlantıları etkinleştir
Azure arama şifreli kanal tüm dizin oluşturucu istekleri için ortak bir internet bağlantısı üzerinden gerektirir. Bu bölümde bu çalışmasını sağlamak için adımları listelenir.

1. Konu adı tam etki alanı adıdır (FQDN) Azure VM doğrulamak için sertifika özelliklerini denetleyin. Özelliklerini görüntülemek için CertUtils gibi bir araç veya Sertifikalar ek bileşenini kullanabilirsiniz. VM hizmet dikey penceresinin 's Essentials bölümünden de FQDN alabilirsiniz **genel IP adresi/DNS ad etiketi** alanında bulunan [Azure portal](https://portal.azure.com/).
   
   * Yeni kullanılarak oluşturulan VM'ler için **Resource Manager** şablonu, FQDN biçimlendirilmiş olarak `<your-VM-name>.<region>.cloudapp.azure.com`. 
   * Eski VM'ler olarak oluşturulan bir **Klasik** VM, FQDN biçimlendirilmiş olarak `<your-cloud-service-name.cloudapp.net>`. 
2. SQL Server Kayıt Defteri Düzenleyicisi'ni (regedit) kullanarak sertifikayı kullanacak şekilde yapılandırın. 
   
    SQL Server Configuration Manager bu görev için genellikle kullanılsa da, bu senaryo için kullanamazsınız. Azure VM'de FQDN'sini (etki alanı yerel bilgisayar veya onu katıldığı ağ etki alanı olarak tanımladığı) VM tarafından belirlenen FQDN eşleşmediği için içeri aktarılan sertifika bulamaz. Adları eşleşmediğinde regedit sertifikasını belirtmek için kullanın.
   
   * Regedit içinde bu kayıt defteri anahtarına gidin: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     `[MSSQL13.MSSQLSERVER]` Bölümü sürümü ve örnek adı göre değişir. 
   * Değerini **sertifika** anahtarını **parmak izi** VM'ye alınan SSL sertifikasının.
     
     Parmak izi diğerlerinden bazı daha iyi almak için birkaç yolu vardır. Buradan kopyalarsanız **sertifikaları** ek bileşenini MMC'de, büyük olasılıkla bir görünmez başında karakter çeker [Bu destek makalesinde açıklandığı gibi](https://support.microsoft.com/kb/2023869/), bir bağlantı çalıştığında hatayla sonuçlanır. Birkaç geçici çözüm bu sorunu düzeltmek için mevcut. Üzerinden Al ve ilk karakter regedit anahtar değeri alanında başında karakter kaldırmak için parmak izi yeniden yazmak için en kolay yoludur. Alternatif olarak, parmak izi kopyalamak için farklı bir aracı kullanabilirsiniz.
3. Hizmet hesabı izinleri verin. 
   
    SQL Server hizmet hesabı uygun özel anahtara SSL sertifikasının izni emin olun. Bu adım overlook, SQL Server başlatılmaz. Kullanabileceğiniz **sertifikaları** ek bileşenini veya **CertUtils** bu görev için.
4. SQL Server hizmetini yeniden başlatın.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL Server bağlantısı VM'yi Yapılandır
Azure Search tarafından gerekli şifreli bir bağlantı kurduktan sonra var. ek yapılandırma adımları Azure Vm'lerde SQL Server iç Henüz yapmadıysanız, bu makaleler birini kullanarak yapılandırmayı tamamlamak için sonraki adım içerir:

* İçin bir **Resource Manager** VM bkz [bir SQL Server sanal makinesine bağlanma Kaynak Yöneticisi'ni kullanarak azure'da](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* İçin bir **Klasik** VM bkz [bir SQL Server sanal makinesine bağlanma Klasik Azure üzerinde](../virtual-machines/windows/classic/sql-connect.md).

Özellikle, her makalede, "Internet üzerinden bağlanma" bölümüne bakın.

## <a name="configure-the-network-security-group-nsg"></a>Ağ güvenlik grubu (NSG) yapılandırma
NSG ve karşılık gelen Azure uç noktası veya erişim denetim listesi (ACL) Azure VM'nizi diğer taraflar için erişilebilir kılmak için yapılandırma olağan dışı bir durum değil. SQL Azure VM'nize bağlanmak için kendi uygulama mantığını izin vermek için bu önce yaptıktan yüksektir. Bir Azure Search bağlantı SQL Azure VM için farklı değildir. 

Aşağıdaki bağlantıları VM dağıtımlar için NSG yapılandırma hakkında yönergeler sağlar. ACL kendi IP adresini temel alan bir Azure SEarch uç nokta için bu yönergeleri kullanın.

> [!NOTE]
> Arka plan için bkz: [bir ağ güvenlik grubu nedir?](../virtual-network/virtual-networks-nsg.md)
> 
> 

* İçin bir **Resource Manager** VM bkz [ARM dağıtımları için Nsg'ler oluşturma](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 
* İçin bir **Klasik** VM bkz [Klasik dağıtımlar için Nsg'ler oluşturma](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP adresleme sorunu ve olası geçici çözümler farkında olması durumunda, kolayca üstesinden birkaç yol açabilir. Kalan bölümler ACL IP adresleri ile ilgili sorunlar işleme için öneriler sağlar.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Arama hizmeti IP adresine erişimi kısıtlama
SQL Azure Vm'leriniz için bağlantı isteklerini açık yapmak yerine ACL arama hizmetinizdeki IP adresine erişimi kısıtlama öneririz. FQDN ping atılarak, IP adresini kolayca bulabilirsiniz (örneğin, `<your-search-service-name>.search.windows.net`) search hizmetinizin.

#### <a name="managing-ip-address-fluctuations"></a>IP adresi dalgalanmaları yönetme
Arama hizmetinizi yalnızca bir arama birimi (diğer bir deyişle, bir çoğaltma ve bir bölüm) varsa, IP adresi rutin hizmet yeniden başlatıldığında, search hizmetinizin IP adresine sahip varolan bir ACL geçersiz kılmalarını sırasında değiştirir.

Sonraki bağlantı hatayı önlemek için bir yol birden fazla çoğaltma ve bir bölüm Azure Search'te kullanmaktır. Bunun yapılması maliyetini artırır, ancak aynı zamanda bir IP adresi sorununu çözer. Birden fazla arama birimi varsa, Azure Search'te IP adreslerini değiştirmeyin.

Başarısız ve NSG ACL'leri yeniden yapılandırmak bağlantıya izin ikinci bir yaklaşımdır. Ortalama, birkaç haftada değiştirmek için IP adreslerini bekleyebilirsiniz. Denetimli bir sık aralıklarla dizin yapan müşteriler için bu yaklaşım uygun olabilir.

Bir üçüncü uygulanabilir (ancak özellikle güvenli değil) arama hizmetinizi burada sağlanan Azure bölgesinin IP adresi aralığını belirtmek için yaklaşımdır. İçinden ortak IP adresleri için Azure kaynaklarını ayrılır IP aralıkları listesi, yayımlanan [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Azure Search portal IP adreslerini içerir
Bir dizin oluşturucu oluşturmak için Azure portalını kullanıyorsanız, Azure Search portal mantığı oluşturulma zamanı sırasında Ayrıca, SQL Azure VM erişimi olmalıdır. Azure arama portal IP adreslerini ping atılarak bulunabilir `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Sonraki adımlar
Göz önünden yapılandırma ile artık bir SQL Server Azure VM'de bir Azure Search Dizin Oluşturucu veri kaynağı olarak belirtebilirsiniz. Bkz: [dizin oluşturucuları kullanarak Azure Search'te Azure SQL veritabanına bağlanma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) daha fazla bilgi için.

