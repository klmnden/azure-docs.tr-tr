---
title: Windows sanal masaüstü önizlemesi nedir?  - Azure
description: Windows sanal masaüstü Önizleme genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 0d606a489a069c4265088d8e07301693dc2f1c83
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64694267"
---
# <a name="what-is-windows-virtual-desktop-preview"></a>Windows sanal masaüstü önizlemesi nedir? 

Artık genel önizlemede, Windows sanal masaüstü Önizleme bulutta çalışan masaüstü ve uygulama sanallaştırma hizmet kullanılabilir.

Azure'da Windows sanal masaüstü çalıştırdığınızda neler yapabileceğinizi aşağıda verilmiştir:

* Tam bir Windows 10 ile ölçeklenebilirlik sunan bir çoklu oturum Windows 10 dağıtımı ayarlama
* Office 365 ProPlus sanallaştırmanızı ve birden çok kullanıcı sanal senaryolarda çalıştırmak için en iyi duruma getirme
* Windows 7 sanal masaüstlerini ücretsiz genişletilmiş güvenlik güncelleştirmeleriyle sağlayın
* Herhangi bir bilgisayarda, var olan Uzak Masaüstü Hizmetleri (RDS) ve Windows Server Masaüstü ve uygulamaları Getir
* Hem Masaüstü ve uygulamaları sanallaştırmayı
* Windows 10, Windows Server ve Windows 7'yi Masaüstü ve birleştirilmiş bir yönetim deneyimi ile uygulamaları yönetme

## <a name="key-capabilities"></a>Temel işlevler

Sanal Masaüstü ile Windows, ölçeklenebilir ve esnek bir ortamı ayarlayabilirsiniz:

* Tam Masaüstü Sanallaştırma ortamı, Azure aboneliğinizdeki herhangi ek bir ağ geçidi sunucuları çalıştırmak zorunda kalmadan oluşturun.
* Çeşitli iş yüklerine uyum sağlamak, gerektiği birçok havuzlarını barındıran olarak yayımlayın.
* Üretim iş yükleri için kendi görüntünüzü getirin veya Azure Galerisi'ndeki test edebilirsiniz.
* Havuza alınmış, çoklu oturum kaynakları ile maliyetleri azaltın. Yeni Windows 10 Enterprise çok oturumu özelliği ile özel Windows Server Windows sanal masaüstü ve Uzak Masaüstü oturumu ana bilgisayarı (RDSH) rolü için istediğiniz büyük ölçüde sanal makineler ve işletim sistemi (OS) sayısını yükü hala azaltabilirsiniz Kullanıcılarınız için aynı kaynakları sağlama.
* Tek tek sahipliğini kişisel (kalıcı) masaüstleri aracılığıyla sağlar.

Dağıtın ve sanal masaüstlerini yönetme:

* Konak havuzlarını yapılandırın, uygulama grupları oluşturma, kullanıcıları atama ve kaynakları yayımlamak için Windows sanal masaüstü PowerShell ve REST arabirimleri kullanır.
* Tam masaüstü veya tek tek ana havuzundan uzak uygulamaları yayımlama, değişik kullanıcı kümeleri için tek tek uygulama grupları oluşturun veya bile kullanıcılar görüntülerinin sayısını azaltmak için birden çok uygulama gruplara atayın.
* Ortamınızı yönetmek gibi yerleşik Temsilcili erişim roller atayabilir ve çeşitli yapılandırma ya da kullanıcı hatalar anlamak için tanılama toplamak için kullanın.
* Hataları gidermek için yeni tanılama hizmeti kullanın.
* Yalnızca görüntü ve sanal makineler, altyapı yönetin. Uzak Masaüstü Hizmetleri, yalnızca sanal makineler Azure aboneliğinizde yaptığınız gibi Uzak Masaüstü rolleri kişisel yönetmeniz gerekmez.

Atama ve kullanıcılar için sanal masaüstlerinizi bağlanın:

* Kendilerine atandıktan sonra kullanıcıların yayımlanan Windows Masaüstü ve uygulamalar için kullanıcıların bağlanmak için herhangi bir Windows sanal masaüstü istemcisi başlatabilirsiniz. Herhangi bir CİHAZDAN Cihazınızda yerel uygulama veya Windows sanal masaüstü HTML5 web istemcisi aracılığıyla bağlanın.
* Gelen bağlantı noktalarının bırakmak zorunda hizmet ters bağlantı aracılığıyla kullanıcılar güvenli bir şekilde oluşturun.

## <a name="requirements"></a>Gereksinimler

Windows sanal masaüstü bağlantısı kurma ve kullanıcılarınızın Windows Masaüstü ve uygulamalar için başarıyla bağlanması için gereken birkaç nokta vardır.

Windows sanal masaüstü yalnızca şu anda aşağıdaki iki işletim sistemi destekler:

* Windows 10 Enterprise çok oturum
* Windows Server 2016

Aşağıdaki işletim sistemleri için destek eklemek için bu nedenle sahip olduğunuzdan emin olun planlıyoruz [uygun lisansları](https://azure.microsoft.com/pricing/details/virtual-desktop/) Masaüstü ve uygulamaları dağıtmayı planladığınız göre kullanıcılarınız için:

|İşletim Sistemi|Gerekli lisans|
|---|---|
|Windows 10 Enterprise çok oturum veya Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Yazılım Güvencesine sahip RDS istemci erişim lisansı (CAL)|

Altyapınızı Windows sanal masaüstü desteklemek için aşağıdakiler gerekir:

* An [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
* Bir Windows Server Active Directory Eşitleme ile Azure Active Directory. Bu aracılığıyla etkinleştirilebilir:
  * Azure AD Connect
  * Azure AD Domain Services
* İçeriyor ya da Windows Server Active Directory'ye bağlı bir sanal ağ içeren bir Azure aboneliği
  
Windows sanal masaüstü için oluşturduğunuz Azure sanal makineleri olması gerekir:

* [Etki alanına katılmış standart](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) veya [hibrit AD'ye katılmış](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). Sanal makineler, Azure AD'ye katılmış olamaz.
* Aşağıdaki desteklenen işletim sistemi görüntüleri birini çalıştırıyor:
  * Windows 10 Enterprise çok oturum
  * Windows Server 2016

>[!NOTE]
>Bir Azure aboneliğine ihtiyacınız varsa, [bir aylık ücretsiz deneme için kaydolun](https://azure.microsoft.com/free/). Azure ücretsiz deneme sürümünü kullanıyorsanız, Windows Server Active Directory'nizi Azure Active Directory ile eşitlenmiş tutmak için Azure AD Domain Services'ı kullanmalısınız.

Windows sanal masaüstü, Windows Masaüstü ve uygulamaları kullanıcılara ve azure'da hizmet olarak Microsoft tarafından barındırılan yönetim çözümü sunun oluşur. Genel Önizleme boyunca herhangi bir Azure bölgesinde sanal makinelerinde (VM'ler) masaüstü ve uygulamaları dağıtılabilir ve yönetim çözümü ve bu VM'ler için verileri Amerika Birleşik Devletleri (Doğu ABD 2 bölge) alacağını. Genel Önizleme süresince bu hizmeti test ederken ABD'ye veri aktarımı neden olabilir. Tüm Azure bölgelerinde genel kullanıma sunulduğunda başlangıç yönetim çözümü ve veri yerelleştirmeye ölçeğini genişletmek başlayacağız.

En iyi performans için ağınıza aşağıdaki gereksinimleri karşıladığından emin olun:

* Konak havuzları dağıtıldığı Azure bölgesi için istemcinin ağ üzerinden gidiş dönüş (RTT) gecikmesi 150 MS'den az olmalıdır.
* Masaüstü ve uygulamaları barındıran VM'ler yönetim hizmetine bağlanırken ağ trafiğini dış ülke Kenarlıklar akış.
* Ağ performansı için en iyi duruma getirmek için yönetim hizmeti ile aynı Azure bölgesinde üzere bu oturumu konağın VM'lerin birlikte öneririz.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için. Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bir kiracı oluşturmanız gerekir. Bir kiracı oluşturma hakkında daha fazla bilgi için Kiracı oluşturma öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Windows sanal masaüstü önizlemesinde bir kiracı oluşturma](tenant-setup-azure-active-directory.md)
