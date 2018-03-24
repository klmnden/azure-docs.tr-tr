---
title: Windows Server Active Directory, Azure sanal makinelerde dağıtmak için yönergeler | Microsoft Docs
description: AD etki alanı Hizmetleri ve şirket içi AD Federasyon Hizmetleri dağıtma biliyorsanız, Azure sanal makinelerde nasıl çalıştığını öğrenin.
services: active-directory
documentationcenter: ''
author: femila
manager: mtillman
editor: ''
ms.assetid: 04df4c46-e6b6-4754-960a-57b823d617fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2018
ms.author: femila
ms.openlocfilehash: c2d58e056cdb285be51d259492e11e6ae37b253e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Windows Server Active Directory Azure sanal makinelerde dağıtmak için yönergeler
Bu makalede dağıtma Windows Server Active Directory etki alanı Hizmetleri (AD DS) ve Active Directory Federasyon Hizmetleri (AD FS) Microsoft Azure sanal makinelerde dağıtmak ve şirket arasındaki önemli farklılıklar açıklanır.

## <a name="scope-and-audience"></a>Kapsam ve hedef kitle
Makaleyi bu zaten içi Active Directory dağıtımı ile karşılaştı yöneliktir. Microsoft Azure sanal makineleri/Azure sanal ağlar ve geleneksel şirket içi Active Directory dağıtımlarında Active Directory dağıtımı arasındaki farklar kapsar. Azure sanal makineler ve Azure sanal ağlar bir altyapı olarak-kuruluşlar için sunumu bulut bilgi işlem kaynakları yararlanmak için hizmet (Iaas) parçasıdır.

Olmayanlar için AD dağıtımı ile tanıdık, bkz: [AD DS dağıtım kılavuzu](https://technet.microsoft.com/library/cc753963) veya [AD FS dağıtımınızı planlama](https://technet.microsoft.com/library/dn151324.aspx) uygun şekilde.

Bu makalede, okuyucu aşağıdaki kavramlarını olduğunu varsayar:

* Windows Server AD DS dağıtım ve Yönetimi
* Dağıtım ve bir Windows Server AD DS altyapısı desteklemek için DNS yapılandırması
* Windows Server AD FS dağıtımı ve Yönetimi
* Dağıtma, yapılandırma ve Windows Server AD FS belirteçleri tüketebileceği bağlı olan taraf uygulamaları (Web siteleri ve web Hizmetleri) yönetme
* Bir sanal makine, sanal diskler ve sanal ağları yapılandırma gibi genel sanal makine kavramları

Bu makale, hangi Windows Server AD DS veya AD FS kısmen dağıtılan şirket içi ve kısmen Azure sanal makinelerinde dağıtılan bir karma dağıtım senaryosu için gereksinimleri vurgular. Belge önce şirket içi ve tasarım ve dağıtım etkileyen önemli karar noktaları karşı Azure sanal makinelerde Windows Server AD DS ve AD FS çalıştıran arasında önemli farklılıkları kapsar. Kağıt geri kalanı daha ayrıntılı karar noktalarının her biri için kılavuzları ve yönergeleri için çeşitli dağıtım senaryolarında uygulamak nasıl açıklanmaktadır.

Bu makalede yapılandırmasını tartışılmaz [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), bulut uygulamaları için kimlik yönetimi ve erişim denetimi özellikleri sağlayan REST tabanlı bir hizmet olduğu. Azure Active Directory (Azure AD) ve Windows Server AD DS ancak, günümüzün karma BT için bir kimlik ve erişim yönetimi çözümü sağlamak için birlikte çalışmak üzere tasarlanmıştır ortamları ve modern uygulamalar. Windows Server AD DS ve Azure AD arasındaki ilişkileri ve farkları anlamanıza yardımcı olması için aşağıdakileri göz önünde bulundurun:

1. Şirket içi veri merkezini buluta genişletmek için Azure kullanırken Windows Server AD DS bulutta Azure sanal makinelerinde çalıştırabilirsiniz.
2. Azure AD, kullanıcılara çoklu oturum açma yazılım olarak-hizmet (SaaS) uygulamaları sağlamak için kullanabilirsiniz. Örneğin, Microsoft Office 365 bu teknolojisini kullanır ve Azure veya Bulut platformlarıyla üzerinde çalışan uygulamalar de kullanabilirsiniz.
3. Azure AD (kendi erişim denetimi hizmeti), Facebook, Google, Microsoft ve diğer kimlik sağlayıcılardan kimliklerden Bulut veya şirket içinde barındırılan uygulamalara kullanarak kullanıcıların oturum izin vermek için kullanabilirsiniz.

Bu farklılıklar hakkında daha fazla bilgi için bkz: [Azure kimlik](fundamentals-identity.md).

## <a name="related-resources"></a>İlgili kaynaklar
İndirme ve çalıştırma [Azure Sanal Makine Hazırlık Değerlendirmesi](https://www.microsoft.com/download/details.aspx?id=40898). Değerlendirme, otomatik olarak şirket içi ortamınız inceleyebilir ve ortam için Azure geçirmenize yardımcı olması için bu konuda bulunan Kılavuzu göre özelleştirilmiş bir rapor oluşturabilir.

Ayrıca ilk öğreticileri, kılavuzları ve aşağıdaki konular ele videolar gözden geçirmenizi öneririz:

* [Azure portalında yalnızca bulut sanal ağ yapılandırma](../virtual-network/quick-create-portal.md)
* [Azure portalında bir siteden siteye VPN yapılandırma](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md)
* [Azure üzerinde Active Directory etki alanı Denetleyicisi'nin çoğaltmasını yükleme](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure BT Uzmanı Iaas: (01) sanal makine temelleri](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure BT Uzmanı Iaas: oluşturma (05) sanal ağlar ve şirket içi bağlantılar](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Giriş
Windows Server Active Directory Azure sanal makinelerde dağıtmak için temel gereksinimler çok az farklılıklar şirket içi sanal makinelerde (ve bazı azami ölçüde, fiziksel makineleri) dağıtılması nedeniyle. Örneğin, Windows Server AD DS söz konusu olduğunda, Azure sanal makinelerde dağıtmak etki alanı denetleyicileri (DC'ler) bir varolan çoğaltmaları varsa Kurumsal etki alanında/ormanda, diğer herhangi bir ek Windows Server Active Directory site davranır gibi Azure dağıtım büyük ölçüde aynı şekilde işlenebilir sonra şirket içi. Diğer bir deyişle, alt ağlar tanımlanmalıdır Windows Server AD DS, oluşturulmuş bir site, bu siteye bağlı ve uygun site bağlantıları kullanarak diğer sitelere bağlı alt ağların. Ancak, tüm Azure dağıtımlar için ortaktır ve bazı, belirli dağıtım senaryosu göre farklılık bazı farklar vardır. İki temel farklar aşağıda özetlenen:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure sanal makineler, şirket içi Kurumsal ağa bağlanma gerekebilir.
Azure sanal makineleri geri bir şirket içi Kurumsal ağa bağlanan bir siteden siteye veya site noktası sanal özel ağ (VPN) bileşeni Azure sanal makineleri sorunsuz bir şekilde bağlanabiliyor içerir ve şirket içi makineler Azure sanal ağı gerektirir. Bu VPN bileşen şirket içi etki alanı üyesi bilgisayarlar, etki alanı denetleyicilerini özel olarak Azure sanal makinelerinde barındırılan bir Windows Server Active Directory etki alanına erişmek de sağlayabilir. Ancak, VPN başarısız olursa, kimlik doğrulama ve Windows Server Active Directory bağımlı diğer işlemleri de başarısız olacak olduğunu dikkate almak önemlidir. Kullanıcılar için mümkün olsa da varolan kullanarak oturum kimlik bilgileri, tüm-eş önbelleğe veya biletleri henüz verilebilecek veya eski duruma gelmiş olan istemci-sunucu kimlik doğrulama denemeleri başarısız olur.

Bkz: [sanal ağ](http://azure.microsoft.com/documentation/services/virtual-network/) video tanıtımı ve de dahil olmak üzere adım adım öğreticiler listesi için [Azure portalında bir siteden siteye VPN yapılandırma](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Ayrıca, Windows Server Active Directory ile şirket içi ağ bağlantısına sahip olmayan bir Azure sanal ağı dağıtabilir. Ancak, bu konudaki yönergeleri IP adresleme, Windows Server için temel özelliklerini sağladığından, Azure sanal ağı kullanılan varsayalım.
> 
> 

### <a name="static-ip-addresses-can-be-configured-with-azure-powershell"></a>Statik IP adreslerini Azure PowerShell ile yapılandırılabilir
Dinamik adresler varsayılan olarak ayrılmış, ancak bunun yerine bir statik IP adresi atamak istiyorsanız, Set-AzureStaticVNetIP cmdlet'ini kullanın. Bu cmdlet'i VM kapatma/restart hizmet onarma ve kalıcı statik bir IP adresi ayarlar. Daha fazla bilgi için bkz: [sanal makineler için statik iç IP adresi](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/). Ayrıca bir statik IP adresi Azure portalında, VM oluşturulurken aşağıda gösterildiği gibi yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir statik genel IP adresiyle bir VM oluşturma](../virtual-network/virtual-network-deploy-static-pip-arm-portal.md).

![bir VM oluştururken, statik IP adresi eklemek için adım ekran görüntüsü](media/active-directory-deploying-ws-ad-guidelines/static-ip.png)

## <a name="BKMK_Glossary"></a>Terimleri ve tanımları
Bu makalede başvuran çeşitli Azure teknolojileri koşullarına kapsamlı olmayan bir listesi verilmiştir.

* **Azure sanal makineleri**: Geleneksel neredeyse tüm çalışan sanal makineler dağıtmak müşterilerin sağlayan Azure Iaas Sunumda şirket içi sunucu iş yükü.
* **Azure sanal ağı**: oluşturmak ve azure'da sanal ağlar yönetmek ve güvenli bir şekilde bunları kendi için bağlantı müşteriler sağlayan Azure ağ hizmetinde ağ altyapısı sanal özel ağ (VPN) kullanarak şirket içi.
* **Sanal IP adresi**: belirli bir bilgisayar veya ağ arabirim kartı için bağlı olmayan bir internet'e yönelik IP adresi. Bulut Hizmetleri için bir Azure VM yönlendirildiği ağ trafiği almak için bir sanal IP adresi atanır. Bir sanal IP adresi, bir veya daha fazla Azure sanal makineler içeren bir bulut-hizmeti özelliğidir. Ayrıca bir Azure sanal ağı bir veya daha fazla bulut hizmeti içerebileceğini unutmayın. Sanal IP adresleri yerel Yük Dengeleme yetenekleri sağlar.
* **Dinamik IP adresi**: Bu, yalnızca iç IP adresidir. Bu bir statik IP adresi olarak (Set-AzureStaticVNetIP cmdlet'ini kullanarak) DC/DNS sunucusu rolleri barındıran sanal makineleri için yapılandırılması gerekir.
* **Hizmet düzeltme**: içinde Azure otomatik olarak döndüren bir hizmet çalışır duruma yeniden hizmet başarısız olduğunu algıladıktan sonra işlem. Düzeltme hizmet kullanılabilirliğini ve esnekliğini destekleyen Azure'nın özelliklerinden biridir. Olası olsa da, bir VM üzerinde çalışan bir DC için olay iyileştirme hizmet aşağıdaki sonucu için planlanmamış bir yeniden başlatma benzer, ancak birkaç yan etkileri vardır:
  
  * VM'deki sanal ağ bağdaştırıcısı değiştirir
  * Sanal ağ bağdaştırıcısının MAC adresini değiştirir
  * VM işlemci/CPU Kimliğini değiştirir
  * Sanal ağ bağdaştırıcısının IP yapılandırması VM sanal bir ağa bağlı olduğundan ve VM'ın IP adresi statik sürece değiştirmez.
  
  Bu davranışların hiçbiri Windows Server Active Directory çünkü MAC adresi veya işlemci/CPU Kimliği hiçbir bağımlılığa sahiptir ve yukarıda açıklandığı şekilde bir Azure sanal ağ üzerinde çalıştırılacak Azure ile ilgili tüm Windows Server Active Directory dağıtımlar için önerilen etkiler.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Windows Server Active Directory etki alanı denetleyicilerini sanallaştırma güvenli mi?
Azure sanal makinelerde Windows Server Active Directory DCs dağıtmaya DC'leri şirket içi bir sanal makinede çalışan olarak aynı yönergeleri tabidir. Yedekleme ve geri yükleme DC'ler için yönergeleri takip sürece sanallaştırılmış DC'ler çalıştıran güvenli bir uygulamadır. Kısıtlamaları ve sanallaştırılmış DC'ler çalıştırmak için yönergeleri hakkında daha fazla bilgi için bkz: [Hyper-v çalıştıran etki alanı denetleyicileri](https://technet.microsoft.com/library/dd363553).

Hiper yöneticilere sağlamak veya Windows Server Active Directory dahil olmak üzere birçok dağıtılmış sistemler için sorunlara neden olabilecek teknolojiler trivialize. Örneğin, bir fiziksel sunucuda bir disk kopyalama ya da SAN'lar ve benzeri kullanarak da dahil, bir sunucu durumunu geri almak için desteklenmeyen yöntemleri kullanın, ancak, bir fiziksel sunucuda bunu bir hiper yönetici, bir sanal makine anlık görüntü geri daha çok daha zordur. Azure aynı istenmeyen koşulunda sonuçlanabilir işlevselliği sunar. Örneğin, VHD dosyaları DC'lerin bunları geri anlık görüntü geri yükleme özelliklerini kullanmak için benzer bir durumda sağladığından düzenli yedeklemeler gerçekleştirmek yerine kopyaladığınız değil.

Bu tür düzeyine kalıcı olarak divergent durumları arasında DC'leri açabilir USN balonları tanıtır. Sorunları gibi neden olabilir:

* Kalan nesneler
* Tutarsız parolaları
* Tutarsız öznitelik değerleri
* Şema Yöneticisi geri alınması durumunda şema uyuşmazlığı

DC'lerin nasıl etkilediği hakkında daha fazla bilgi için bkz: [USN ve USN geri alma](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Windows Server 2012 ile başlayan [ek güvenlik önlemleri AD DS'ye yerleşiktir](https://technet.microsoft.com/library/hh831734.aspx). Temel alınan hiper yönetici platformunun VM-Generationıd'yi destekleyen sürece bu korumalar, sanallaştırılmış etki alanı denetleyicilerine daha önce bahsedilen sorunlara karşı korumaya yardımcı olmak. Azure, Windows Server 2012 ya da daha sonra Azure sanal makineleri çalıştıran etki alanı denetleyicileri ek güvenlik önlemleri sahip VM-Generationıd'yi destekleyen.

> [!NOTE]
> Kapatılır ve etki alanı denetleyicisi rolünü kullanmak yerine konuk işletim sistemi içinde Azure'da çalışan bir VM yeniden **Kapat** Azure Portalı hakkında seçeneği. Bugün, bir VM kapatma için portalı kullanarak VM bırakılmasına neden olur. Deallocated VM ücretlerinin yansıtılmasını değil avantajına sahiptir, ancak bir DC için istenmeyen olduğu ayrıca VM-Generationıd, sıfırlar. VM-Generationıd sıfırladığınızda, çağırma kimliği AD DS veritabanının da sıfırlanır, RID havuzu atılır ve SYSVOL'ü yetkisiz olarak işaretlenir. Daha fazla bilgi için bkz: [Active Directory etki alanı Hizmetleri (AD DS) sanallaştırmasına giriş](https://technet.microsoft.com/library/hh831734.aspx) ve [güvenli şekilde sanallaştırılmasını DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).
> 
> 

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Neden Azure sanal makineler üzerinde Windows Server AD DS dağıtma?
Çoğu Windows Server AD DS dağıtım senaryoları dağıtım Azure Vm'lerinde olarak uymaktadır. Örneğin, bir şirket Asya'da uzak bir konumdaki kullanıcıların kimliklerini doğrulamak için gereken Avrupa'da olduğunu varsayalım. Şirketin önceden dağıtım sonrası sunucuları yönetmek için bunları ve sınırlı uzmanlık dağıtmak için maliyet nedeniyle Windows Server Active Directory DC'leri Asya'da dağıtılmamış. Sonuç olarak, kimlik doğrulama isteklerini Asya Avrupa'da DC'ler tarafından yetersiz sonuçlarıyla sunulur. Bu durumda, bir DC Asya'da Azure veri merkezi içinde çalıştırılması gereken belirtilen bir VM üzerinde dağıtabilirsiniz. Doğrudan uzak konuma bağlı Azure sanal ağını Bu DC ekleme kimlik doğrulama performansını artırır.

Azure da aksi takdirde maliyetli olağanüstü durum kurtarma (DR) sitelere adındaki çok uygundur. Göreceli olarak düşük maliyetli az sayıda etki alanı denetleyicileri ve Azure üzerinde tek bir sanal ağa barındırma çekici alternatif temsil eder.

Son olarak, bir ağ uygulaması, Windows Server Active Directory gerektiriyor, ancak şirket içi ağ veya şirket Windows Server Active Directory üzerinde hiçbir bağımlılık içeriyor SharePoint gibi Azure üzerinde dağıtmak isteyebilirsiniz. Bu durumda, ayrı bir ormanda SharePoint karşılamak için Azure üzerinde dağıtma sunucunun gereksinimleri en iyi. Yeniden bağlanma şirket içi ağ ve kurumsal Active Directory'ye gerektiren ağ uygulamaları dağıtma de desteklenir.

> [!NOTE]
> Bir katman 3 bağlantısı sağlar olduğundan, bir Azure sanal ağı ve bir şirket içi ağ arasında bağlantı sağlar VPN bileşeni de Azure sanal ağında Azure sanal makineleri olarak çalışan DC'ler yararlanmak içi çalıştıran üye sunucuları etkinleştirebilirsiniz. Ancak VPN kullanılamıyorsa, arasındaki iletişimi bilgisayarların şirket içi ve Azure tabanlı etki alanı denetleyicileri, kimlik doğrulama ve diğer çeşitli hatalar ortaya çıkan çalışmayacaktır.  
> 
> 

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Arasında veya şirket içinde Azure sanal makineler üzerinde Windows Server Active Directory etki alanı denetleyicileri dağıtma çelişir
* Tek bir VM'ye birden fazla içeren hiçbir Windows Server Active Directory dağıtım senaryosu için Azure sanal ağı için IP adresi tutarlılığını kullanmak gereklidir. Bu kılavuz DC'leri bir Azure sanal ağ üzerinde çalıştırıyorsanız varsaydığını unutmayın.
* İle şirket içi DC'leri gibi statik IP adresleri önerilir. Statik bir IP adresi yalnızca Azure PowerShell kullanarak yapılandırılabilir. Bkz: [VM'ler için statik iç IP adresi](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) daha fazla ayrıntı için. İzleme sistemleri ya da konuk işletim sistemi içinde statik IP adresi yapılandırması için denetleme diğer çözümleri varsa, VM ağ bağdaştırıcısı özelliklerini aynı statik IP adresi atayabilirsiniz. Ancak, ağ bağdaştırıcısı VM hizmet onarma uğradığında veya portalda kapatılır ve adresini serbest bıraktı atılacak unutmayın. Bu durumda, statik IP adresi Konuk içinde sıfırlanması gerekir.
* Bir sanal ağ üzerinde sanal makineleri dağıtma kapsıyor (gerektiren bir şirket içi ağ bağlantısını geri veya değil); sanal ağ yalnızca bu olanağı sağlar. Azure ile şirket içi ağınız arasında özel iletişim için bir sanal ağ oluşturmanız gerekir. VPN bitiş noktası şirket içi ağ üzerinde dağıtmanız gerekir. VPN Azure'dan şirket içi ağ açıldı. Daha fazla bilgi için bkz: [Virtual Network'e genel bakış](../virtual-network/virtual-networks-overview.md) ve [Azure Portalı'nda bir siteden siteye VPN yapılandırma](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Bir seçenek [bir noktadan siteye VPN Oluştur](../vpn-gateway/vpn-gateway-point-to-site-create.md) ayrı Windows tabanlı bilgisayarlar doğrudan bir Azure sanal ağa bağlanmak kullanılabilir.
> 
> 

* Olup bir sanal oluşturduğunuz bağımsız olarak değil, Azure ücretleri çıkış trafiği ancak değil giriş için ağ veya. Windows Server Active Directory Tasarım seçeneklerinin çeşitliliğine ne kadar çıkış trafiği bir dağıtım tarafından oluşturulan etkileyebilir. Giden çoğaltmıyor çünkü Örneğin, bir salt okunur etki alanı denetleyicisi dağıtma (RODC) çıkış trafiği sınırlar. Ancak bir RODC dağıtmanız kararı DC karşı yazma işlemleri gerçekleştirmek için gereken karşı ağırlıklı gerekiyor ve [Uyumluluk](https://technet.microsoft.com/library/cc755190) uygulamaları ve Hizmetleri sitedeki ile RODC'ler sahip. Trafiği ücretlere hakkında daha fazla bilgi için bkz: [Azure bir bakışta fiyatlandırma](http://azure.microsoft.com/pricing/).
* Tam varken denetim hangi sunucu üzerinden VM'ler şirket içi, RAM gibi için kullanılacak kaynakları boyutu disk ve vb. Azure üzerinde önceden yapılandırılmış sunucu boyutları listesinden seçmeniz gerekir. Bir DC için bir veri diski işletim sistemi diski ek olarak Windows Server Active Directory veritabanını depolamak için gereklidir.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Azure sanal makinelerde Windows Server AD FS dağıtabilir miyim?
Evet, Azure sanal makinelerde Windows Server AD FS dağıtabilirsiniz ve [AD FS dağıtımı için en iyi uygulamaları](https://technet.microsoft.com/library/dn151324.aspx) şirket içi Uygula eşit olarak Azure üzerinde AD FS dağıtımı için. Ancak bazı en iyi uygulamaları gibi Yük Dengeleme ve yüksek kullanılabilirlik, AD FS kendisini ne sunar ötesinde teknolojileri gerektirir. Temel alınan altyapısı tarafından sağlanmalıdır. Şimdi bu en iyi uygulamaları gözden geçirin ve nasıl bunlar Azure Vm'leri ve Azure sanal ağını kullanarak elde edilebilir bakın.

1. **Hiç güvenlik belirteci hizmeti (STS) sunucuları İnternete doğrudan kullanıma sunar.**
   
    STS sorunları için güvenlik belirteçleri, bu önemlidir. Sonuç olarak, aynı koruma düzeyine sahip bir etki alanı denetleyicisi ile AD FS sunucuları gibi STS sunucuları değerlendirilmelidir. Bir STS biri riske girerse, kötü niyetli kullanıcılar, potansiyel olarak bağlı olan taraf uygulamaları ve kuruluşların güvenen, diğer STS sunuculara istediği talepleri içeren erişim belirteçleri sahipsiniz.
2. **AD FS sunucuları aynı ağ içindeki tüm kullanıcı etki alanları için Active Directory etki alanı denetleyicileri dağıtın.**
   
    AD FS sunucuları, kullanıcıların kimliklerini doğrulamak için Active Directory etki alanı Hizmetleri'ni kullanın. AD FS sunucuları aynı ağ etki alanı denetleyicilerinde dağıtılması önerilir. Azure ağı ve şirket içi ağınız arasında bağlantı bozuluyor ve daha düşük gecikme süresi ve oturum açma performans artışı sağlar durumunda bu iş sürekliliği sağlar.
3. **Yüksek kullanılabilirlik ve Yük Dengeleme için birden çok AD FS düğüme dağıtın.**
   
    Görev açısından kritik güvenlik belirteçleri sıklıkla olmayan gerektiren uygulamalar için çoğu durumda, AD FS sağlayan bir uygulama hatası kabul edilebilir değil. Sonuç olarak, ve AD FS, iş açısından önemli uygulamaları erişmek için kritik yolunda şimdi bulunduğu için AD FS hizmeti birden çok AD FS proxy'si ve AD FS sunucuları yüksek oranda kullanılabilir olması gerekir. Dağıtım isteklerinin elde etmek için yük dengeleyici genellikle AD FS proxy'si ve AD FS sunucularının önüne dağıtılır.
4. **İnternet erişimi için bir veya daha fazla Web uygulaması proxy'si düğümleri dağıtın.**
   
    Kullanıcılar AD FS hizmeti tarafından korunan uygulamalar erişim gerektiğinde, AD FS hizmeti internet'ten kullanılabilir olması gerekir. Bu Web uygulaması proxy'si hizmeti dağıtarak sağlanır. Yüksek kullanılabilirlik amaçları için birden fazla düğüm dağıtmak ve Yük Dengeleme için önerilir.
5. **Web uygulaması proxy'si düğümlerden iç ağ kaynaklarına erişimini.**
   
    Dış kullanıcıların internet'ten AD FS erişmesine izin vermek için Web uygulaması proxy'si düğümleri (veya Windows Server'ın önceki sürümlerindeki AD FS Proxy) dağıtmak için gerekir. Web uygulaması proxy düğümleri doğrudan Internet'e sunulur. Etki alanına katılmış olması gerekli değildir ve TCP bağlantı noktaları 443 ve 80 yalnızca AD FS sunucularına erişimi gerekir. Diğer tüm bilgisayarlarda (özellikle de etki alanı denetleyicileri) iletişimi engellendi önerilir.
   
    Genellikle arşivlenmiş şirket içi DMZ yoluyla budur. Güvenlik duvarları, çevre ağından şirket içi ağa (diğer bir deyişle, belirtilen IP adresleri ve belirtilen bağlantı noktası üzerinden trafiğe izin verilir ve diğer tüm trafik engellendi yalnızca) trafiği kısıtlamak için bir beyaz liste modu kullanın.

Aşağıdaki diyagramda gösterildiği geleneksel bir şirket içi AD FS dağıtımı.

![Geleneksel şirket içi Active Directory Federasyon Hizmetleri dağıtım diyagramı](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Azure yerel sağlamadığından, ancak, tam özellikli bir güvenlik duvarı özelliği, diğer seçenekleri trafiği kısıtlamak için kullanılması gerekir. Aşağıdaki tabloda, her bir seçeneğin ve avantajları ve dezavantajları gösterilmektedir.

| Seçenek | Avantajı | Olumsuz |
| --- | --- | --- |
| [Azure ağ ACL'leri](../virtual-network/virtual-networks-acl.md) |Daha az maliyetli ve daha basit ilk yapılandırma |Yeni Vm'leri bir dağıtımına eklenen ek ağ ACL yapılandırması gerekli |
| [Barracuda NG güvenlik duvarı](https://www.barracuda.com/products/ngfirewall) |Beyaz liste modu işlem ve ağ ACL yapılandırma gerektirir |Artan maliyetini ve karmaşıklığını ilk kurulumu |

Bu durumda AD FS dağıtmak için üst düzey adımları aşağıda belirtilmiştir:

1. VPN kullanarak şirket içi bağlantılar ile bir sanal ağ oluşturma veya [ExpressRoute](http://azure.microsoft.com/services/expressroute/).
2. Sanal ağ etki alanı denetleyicilerinde dağıtın. Bu adım isteğe bağlıdır ancak önerilir.
3. Sanal ağ üzerinde AD FS sunucuları etki alanına katılmış dağıtın.
4. Oluşturma bir [iç yük dengeli kümesi](http://azure.microsoft.com/blog/internal-load-balancing/) , AD FS sunucularını içerir ve sanal ağ (dinamik bir IP adresi) içinde yeni bir özel IP adresi kullanır.
   
   1. Özel (dinamik) IP adresini iç yük dengeli küme işaret edecek şekilde FQDN oluşturmak için DNS güncelleştirin.
5. Bir bulut hizmeti (veya ayrı bir sanal ağ) Web uygulaması proxy'si düğümlerin oluşturun.
6. Bulut hizmeti ya da sanal ağ Web uygulaması proxy'si düğümlerin dağıtma
   
   1. Web uygulaması proxy'si düğümleri içeren bir dış yük dengelenmiş küme oluşturun.
   2. Dış DNS adını (FQDN) bulut hizmeti ortak IP adresine (sanal IP adresi) işaret edecek şekilde güncelleştirin.
   3. AD FS proxy'si iç yük dengeli Küme AD FS sunucuları için karşılık gelen FQDN'si kullanacak şekilde yapılandırın.
   4. Talep tabanlı web siteleri, talep sağlayıcıları için dış FQDN'si kullanacak şekilde güncelleştirin.
7. AD FS sanal ağda herhangi bir makineye Web uygulaması Ara sunucusu arasında erişimi kısıtlayın.

Trafiği kısıtlamak için Azure iç yük dengeleyici için yük dengeli kümesi yalnızca trafiği 80 ve 443 numaralı TCP bağlantı noktaları için yapılandırılması gerekir ve diğer tüm trafik yük dengelenmiş küme iç dinamik IP adresine bırakılır.

![ADFS ağ ACL'leri TCP 443 + 80 diyagramı izin verilir.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

AD FS sunucularına trafiği yalnızca aşağıdaki kaynaklar tarafından izin verilen:

* Azure iç yük dengeleyici.
* Yönetici şirket ağındaki IP adresi.

> [!WARNING]
> Tasarım, Web uygulaması proxy'si düğümleri Azure sanal ağındaki herhangi bir VM veya şirket içi ağ konumlarına ulaşmasını engellemeniz gerekir. Bu, Expressroute bağlantıları için şirket içi Gereci veya siteden siteye VPN bağlantıları için VPN cihazı güvenlik duvarı kurallarını yapılandırarak yapılabilir.
> 
> 

Bu seçenek bir dezavantajı, iç yük dengeleyici, AD FS sunucuları ve sanal ağa ekleniyor başka herhangi bir sunucuya birden çok aygıtlar için ağ ACL'leri yapılandırmak için gerekiyor. Herhangi bir aygıt için trafiği kısıtlamak için ağ ACL'leri yapılandırmadan dağıtımına eklediyseniz, tüm dağıtım risk altında olabilir. Web uygulaması proxy'si düğümleri IP adreslerini her zamankinden değiştirirseniz, ağ ACL'leri sıfırlamanız gerekir (proxy'leri yani kullanacak şekilde yapılandırılmalıdır [dinamik statik IP adresleri](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS ağ ACL'leri ile azure'da.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Başka bir seçenek kullanmaktır [Barracuda NG Güvenlik Duvarı'nı](https://www.barracuda.com/products/ngfirewall) Gereci AD FS proxy sunucuları ve AD FS sunucuları arasındaki trafiği denetlemek. Bu seçenek, güvenlik ve yüksek kullanılabilirlik için en iyi yöntemlerle uyumlu ve Barracuda NG güvenlik duvarı gerecini bir beyaz liste modu, güvenlik duvarı yönetimi sağlar ve doğrudan Azure sanal ağı üzerinde yüklü olması gerektiğinden ilk Kurulumdan sonra daha az yönetim gerektirir. Yeni bir sunucu dağıtımına eklenen her zaman ağ ACL'leri yapılandırma gereksinimini ortadan kaldırır. Ancak bu seçenek ilk dağıtım karmaşıklığı ve maliyeti ekler.

Bu durumda, iki sanal ağ bir yerine dağıtılır. Bunları VNet1 ve VNet2 arayacağız. VNet1 proxy'leri ve Sts'ler ve ağ bağlantısı kurumsal ağ vnet2'yi içerir. VNet1 bu nedenle özelliği fiziksel olarak (neredeyse barındırabilir) vnet2'yi ve, buna karşılık, Kurumsal ağdan yalıtılmış. VNet1 ardından vnet2'yi için bağlı aktarım bağımsız ağ mimarisi (TINA) olarak bilinen özel bir tünel oluşturma teknolojisini kullanarak. Her bir Barracuda NG Güvenlik Duvarı'nı kullanarak sanal ağları TINA tünel bağlı — her sanal ağlar bir Barracuda.  Yüksek kullanılabilirlik için her sanal ağ üzerindeki iki Barracudas dağıtmanız önerilir; bir etkin, diğer pasif. Bunlar bize Azure içi geleneksel DMZ'deki çalışmasını taklit etmek üzere izin veren saldırısından son derece Zengin özellikleri sunar.

![Azure üzerinde güvenlik duvarı sahip ADFS.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Daha fazla bilgi için bkz: [AD FS: Talep kullanan şirket içi ön uç uygulamasını internet Genişlet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Alternatif hedef Office 365 SSO tek başına ise, AD FS dağıtımı
AD dağıtma için başka bir alternatif var. amacınız yalnızca oturum açma Office 365 için etkinleştirmek istiyorsanız FS değerlerinin. Bu durumda, yalnızca DirSync Parola Eşitleme ile şirket içi dağıtabilir ve bu yaklaşım, AD FS veya Azure gerektirmediği için en az dağıtım karmaşıklığını ile aynı Nihai sonuç elde.

Aşağıdaki tabloda, oturum açma işlemleri ile ve AD FS dağıtma olmadan nasıl çalıştığı karşılaştırır.

| Office 365 tek AD FS ve DirSync kullanarak oturum | Office 365 aynı oturum DirSync + Parola Eşitleme ile açma |
| --- | --- |
| 1. Kullanıcı bir kurumsal ağda oturum açtığında ve Windows Server Active Directory kimlik doğrulaması. |1. Kullanıcı bir kurumsal ağda oturum açtığında ve Windows Server Active Directory kimlik doğrulaması. |
| 2. Kullanıcı Office 365 erişmeyi dener (ben @contoso.com). |2. Kullanıcı Office 365 erişmeyi dener (ben @contoso.com). |
| 3. Office 365 kullanıcı Azure AD ile yeniden yönlendirir. |3. Office 365 kullanıcı Azure AD ile yeniden yönlendirir. |
| 4. Azure AD kullanıcı kimlik doğrulaması yapamaz ve AD FS şirket içi güvenle yok anlar olduğundan, kullanıcının AD FS için yönlendirir. |4. Azure AD Kerberos biletleri doğrudan kabul edemez ve kullanıcı kimlik bilgilerini girin istekleri için bir güven ilişkisi yok. |
| 5. Kullanıcı bir Kerberos anahtarı için AD FS STS gönderir. |5. Kullanıcı aynı şirket içi parolayı girer ve Azure AD bunları kullanıcı adı ve DirSync tarafından eşitlendiği parola karşı doğrular. |
| 6. AD FS gerekli belirteci biçimi/talepleri için Kerberos bileti dönüştürür ve kullanıcının Azure AD ile yeniden yönlendirir. |6. Azure AD kullanıcı Office 365'e yönlendirir. |
| 7. Azure AD kullanıcının kimliğini doğrular (başka bir dönüşüm oluşur). |7. Kullanıcı Office 365 ve Azure AD belirteci kullanarak OWA oturum açabilir. |
| 8. Azure AD kullanıcı Office 365'e yönlendirir. | |
| 9. Kullanıcı sessizce Office 365'e oturum açmış. | |

Parola Eşitleme senaryosu (AD FS) ile DirSync ile Office 365 çoklu oturum açma "aynı burada"aynı"yalnızca kullanıcıları aynı şirket içi kimlik bilgilerini Office 365 erişirken yeniden girmeniz gerekir anlamına gelir oturum tarafından" değiştirilir. Bu veri sonraki komut istemlerini azaltmaya yardımcı olmak için kullanıcının tarayıcı tarafından anımsanabileceğini unutmayın.

### <a name="additional-food-for-thought"></a>Ek Yemek için düşünce
* Bir Azure sanal makinesinde bir AD FS proxy'si dağıtırsanız, AD FS sunucularına bağlantısı gereklidir. Şirket içi farklıysa, kullanıcıların AD FS sunucuları ile iletişim kurmak Web uygulaması proxy'si düğümleri izin vermek için sanal ağ tarafından sağlanan siteden siteye VPN bağlantısı yararlanan önerilir.
* Bir Azure sanal makinesi bir AD FS sunucusuna dağıtırsanız, Windows Server Active Directory etki alanı denetleyicileri, öznitelik depoları ve yapılandırma veritabanlarının bağlantısı gereklidir ve ayrıca bir ExpressRoute veya siteden siteye VPN bağlantısı Azure sanal ağı şirket içi ağ arasındaki gerektirebilir.
* Ücretler Azure sanal makinelerden (çıkış trafiği) tüm trafiğe uygulanır. Maliyet yönlendirmeli faktörü ise, AD FS sunucuları içi çıkmadan Azure Web uygulaması Ara sunucusu düğümlerinde dağıtmak için önerilir. AD FS sunucuları da Azure sanal makinelerde dağıtılırsa, şirket içi kullanıcıların kimliklerini doğrulamak için ek ücrete ücrete tabi. Çıkış trafiği olsun veya olmasın, ExpressRoute veya siteden siteye VPN bağlantısı yaptıran bakılmaksızın bir maliyet oluşturur.
* AD FS sunucuları, yüksek kullanılabilirlik için Dengeleme Azure'nın yerel sunucu yükü kullanmaya karar verirseniz, Yük Dengeleme bulut hizmeti içindeki sanal makinelerin sistem durumunu belirlemek için kullanılan araştırmalar sağladığını unutmayın. Varsayılan araştırmalar yanıt Aracısı Azure sanal makinelerde mevcut olmadığından (aksine, web veya çalışan rolleri) Azure sanal makineler söz konusu olduğunda, özel bir araştırma kullanılması gerekir. Kolaylık olması için özel bir TCP araştırması kullanabilirsiniz; bunun yalnızca sanal makine sistem durumunu belirlemek için (TCP Eşitlemeye kesimi gönderilen ve TCP Eşitlemeye ACK kesimle yanıt verdi) bir TCP bağlantısı başarıyla kuruldu gerekir. Özel araştırma sanal makinelerinizi etkin olarak dinleyen tüm TCP bağlantı noktasını kullanacak biçimde yapılandırabilirsiniz.

> [!NOTE]
> Doğrudan Internet'e (örneğin, bağlantı noktası 80 ve 443 numaralı) bağlantı noktalarını aynı kümesini kullanıma sunmak için gereken makineler aynı bulut hizmetine paylaşamaz. Bu nedenle, özel bir bulut oluşturmak önerilir hizmet olasılığını önlemek için Windows Server AD FS sunucularınız için bir uygulama için bağlantı noktası koşulları ve Windows Server AD FS arasında çakışıyor.
> 
> 

## <a name="deployment-scenarios"></a>Dağıtım senaryoları
Aşağıdaki bölümde dikkate alınması gereken önemli noktalar için dikkat çekmek için sıradan dağıtım senaryoları özetlenmektedir. Her senaryo Etkenler ve kararları dikkate alınması gereken hakkında daha fazla bilgi için bağlantılar içerir.

1. [AD DS: Kurumsal ağ bağlantısına yönelik bir gereksinim ile AD DS algılayan bir uygulama dağıtma](#BKMK_CloudOnly)
   
    Örneğin, bir Internet'e yönelik SharePoint hizmeti bir Azure sanal makineye dağıtılır. Uygulama bağımlılıkları şirket ağı kaynaklarına sahiptir. Uygulamayı Windows Server AD DS gerektirmez, ancak şirket Windows Server AD DS gerektirmez.
2. [AD FS: bir talep kullanan şirket içi ön uç uygulamasını internet Genişlet](#BKMK_CloudOnlyFed)
   
    Örneğin, Internet'ten erişilebilir olmasını başarıyla dağıtıldı şirket içi ve şirket kullanıcılar tarafından kullanılan bir talep kullanan uygulama gerekir. Uygulamanın hem şirket kimliklerini kullanılarak iş ortakları tarafından hem de mevcut kurumsal kullanıcılar tarafından doğrudan Internet üzerinden erişilen gerekir.
3. [AD DS: şirket ağına bağlantısı gerektiren Windows Server AD DS algılayan bir uygulama dağıtma](#BKMK_HybridExt)
   
    Örneğin, Windows tümleşik kimlik doğrulamasını destekler ve Windows Server AD DS yapılandırma ve kullanıcı profili verilerini depo olarak kullanan LDAP algılayan bir uygulama bir Azure sanal makineye dağıtılır. Uygulamanın var olan şirket Windows Server AD DS yararlanır ve çoklu oturum açma sağlamak için iyi bir şeydir. Uygulama talep kullanan değil.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Kurumsal ağ bağlantısına yönelik bir gereksinim ile AD DS algılayan bir uygulama dağıtma
![Yalnızca bulutta AD DS dağıtımı](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Şekil 1**

#### <a name="description"></a>Açıklama
SharePoint bir Azure sanal makinesi üzerinde dağıtılır ve uygulamanın şirket ağı kaynaklarına hiçbir bağımlılığı yoktur. Uygulamanın ancak Windows Server AD DS gerektiren *değil* şirket Windows Server AD DS gerektirir. Kullanıcılar ayrıca Azure sanal makinelerde bulutta barındırılan Windows Server AD DS etki alanı uygulamasına aracılığıyla kendi kendine sağlanan olduğundan Kerberos ya da federasyon güvenleri gereklidir.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Senaryo konuları ve nasıl teknoloji alanları senaryosuyla
* [Ağ topolojisi](#BKMK_NetworkTopology): şirket içi bağlantılar (siteden siteye bağlantı olarak da bilinir) olmadan Azure sanal ağı oluşturun.
* [DC dağıtım yapılandırmasını](#BKMK_DeploymentConfig): yeni bir etki alanı denetleyicisi yeni, tek etki alanı, bir Windows Server Active Directory ormanına dağıtın. Bunun yanı sıra Windows DNS sunucusu dağıtılmalıdır.
* [Windows Server Active Directory site topolojisi](#BKMK_ADSiteTopology): (tüm bilgisayarları Default-First-Site-Name olacaktır) varsayılan Windows Server Active Directory sitesini kullanın.
* [IP adresi ve DNS](#BKMK_IPAddressDNS):
  
  * Statik bir IP adresi için DC kümesi AzureStaticVNetIP Azure PowerShell cmdlet'ini kullanarak ayarlayın.
  * Yükleyin ve Azure ile ilgili etki alanı denetleyicileri Windows Server DNS yapılandırın.
  * Adı ve IP adresi DC ve DNS sunucu rollerini barındıran VM sanal ağ özelliklerini yapılandırın.
* [Genel katalog](#BKMK_GC): ormandaki ilk DC bir genel katalog sunucusu olmalıdır. Bir tek etki alanı ormanda herhangi bir ek iş DC gelen genel katalog gerektirmediğinden ek DC'leri de GC'ler yapılandırılması gerekir.
* [Windows Server AD DS veritabanını ve SYSVOL yerleşimini](#BKMK_PlaceDB): Windows Server Active Directory veritabanı, günlükler ve SYSVOL için depolamak üzere Azure VM'ler çalıştıran DC'ler için bir veri diski ekleyin.
* [Yedekleme ve geri yükleme](#BKMK_BUR): sistem durumu yedeklemeleri depolamak istediğiniz belirler. Gerekirse, başka bir veri diski yedeklemelerini depolamak için DC VM ekleyin.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: Talep kullanan şirket içi ön uç uygulamasını internet Genişlet
![Şirket içi bağlantılar ile Federasyon](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Şekil 2**

#### <a name="description"></a>Açıklama
Doğrudan Internet'ten erişilebilir olmasını başarıyla dağıtıldı şirket içi ve şirket kullanıcılar tarafından kullanılan bir talep kullanan uygulama gerekir. Uygulama verilerini depolayan bir SQL veritabanı için bir web ön uç olarak görev yapar. Uygulama tarafından kullanılan SQL sunucuları şirket ağında da bulunur. İki Windows Server AD FS Sts'ler ve bir yük dengeleyici dağıtılan Kurumsal kullanıcılara erişim sağlamak için şirket içi olmuştur. Uygulamayı şimdi doğrudan Internet üzerinden hem kurumsal kimlikleri kullanılarak iş ortakları ve mevcut şirket kullanıcıları tarafından ayrıca erişilmesi gerekir.

Basitleştirmek ve bu yeni gereksinimi dağıtım ve yapılandırma gereksinimlerini karşılamak için bir çaba içinde iki ek web ön uçlar ve iki görünür duruma belirlenir Windows Server AD FS proxy sunucular, Azure sanal makinelerde yüklü olmalıdır. Tüm dört sanal makineleri doğrudan Internet'e açık ve Azure sanal ağın siteden siteye VPN özelliği kullanarak şirket içi ağ bağlantısını sağlanacaktır.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Senaryo konuları ve nasıl teknoloji alanları senaryosuyla
* [Ağ topolojisi](#BKMK_NetworkTopology): Azure sanal ağı oluşturmak ve [şirket içi bağlantılar yapılandırmak](../vpn-gateway/vpn-gateway-site-to-site-create.md).
  
  > [!NOTE]
  > Her Windows Server AD FS sertifikaları için sertifika şablonunu ve elde edilen sertifikaların içinde tanımlanmış bir URL Azure üzerinde çalışan Windows Server AD FS örnekler tarafından ulaşılabildiğinden emin olun. Bu şirket içi bağlantılar PKI altyapınızı bölümlerine gerektirebilir. Örnek CRL'nin endpoint LDAP tabanlı ise ve özel olarak şirket içinde barındırılan sonra bağlantı gerekir şirketler arası için. Bu arzu değilse, CRL Internet üzerinden erişilebilen bir CA tarafından verilen sertifikaların kullanılabilmesi için gerekli olabilir.
  > 
  > 
* [Bulut Hizmetleri Yapılandırması](#BKMK_CloudSvcConfig): iki yük dengeli sanal IP adresleri sağlayın iki bulut Hizmetleri sırada olduğundan emin olun. İlk bulut hizmetin sanal IP adresi, 80 ve 443 numaralı bağlantı noktalarını iki Windows Server AD FS proxy VM yönlendirilir. Windows Server AD FS proxy VM'ler, bu ön Windows Server AD FS Sts'ler şirket içi yük dengeleyici IP adresine işaret edecek şekilde yapılandırılır. İkinci bulut hizmetin sanal IP adresi, web ön uç 80 ve 443 numaralı bağlantı noktalarını yeniden çalışan iki VM yönlendirilir. Yük Dengeleyici yalnızca çalışan Windows Server AD FS proxy ve web ön uç VM'ler trafiğini yönlendirir emin olmak için özel bir araştırma yapılandırın.
* [Federasyon sunucusu yapılandırması](#BKMK_FedSrvConfig): yapılandırma Windows Server AD FS federasyon sunucusu bulutta oluşturulan Windows Server Active Directory ormanı için güvenlik belirteçleri oluşturmak için (STS). Talep sağlayıcı güven ilişkileri kimliklerden kabul istediğinizden farklı iş ortakları ile Federasyon ayarlamak ve belirteçleri oluşturmak istediğiniz farklı uygulamalarla bağlı olan taraf güven ilişkilerini yapılandırın.
  
    Windows Server AD FS federasyon dekiler doğrudan Internet bağlantısı yalıtılmış kalırken Çoğu senaryoda, bir Internet'e yönelik kapasite güvenlik amacıyla Windows Server AD FS proxy sunucuları dağıtılır. Dağıtım senaryonuz bağımsız olarak sanal bir IP adresiyle bir genel olarak sunulan IP adresi ve iki Windows Server AD FS STS örnekleri veya proxy örnekleri arasında yük dengelemek için mümkün olan bağlantı noktasına sağlayan bulut hizmetiniz yapılandırmanız gerekir.
* [Windows Server AD FS yüksek kullanılabilirlik Yapılandırması](#BKMK_ADFSHighAvail): bir Windows Server AD FS grubunu yük devretme için en az iki sunucu ile dağıtmak ve Yük Dengeleme için önerilir. Windows İç Veritabanı (WID) için Windows Server AD FS yapılandırma verilerini kullanmayı göz önünde ve gruptaki sunucular üzerinden gelen istekleri dağıtmak için Azure iç Yük Dengeleme özelliğini kullanın.

Daha fazla bilgi için bkz: [AD DS dağıtım kılavuzu](https://technet.microsoft.com/library/cc753963).

### <a name="BKMK_HybridExt"></a>3. AD DS: şirket ağına bağlantısı gerektiren Windows Server AD DS algılayan bir uygulama dağıtma
![Şirket içi AD DS dağıtımı](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Şekil 3**

#### <a name="description"></a>Açıklama
LDAP algılayan bir uygulama, bir Azure sanal makineye dağıtılır. Windows tümleşik kimlik doğrulamasını destekler ve Windows Server AD DS yapılandırma ve kullanıcı profili verilerini depo olarak kullanır. Uygulamanın var olan şirket Windows Server AD DS yararlanır ve çoklu oturum açma sağlamak için belirtilir. Uygulama talep kullanan değil. Kullanıcıların doğrudan Internet'ten uygulamaya erişmek de gerekir. Performans ve maliyet için en iyi duruma getirmek için şirket etki alanının parçası olan iki ek etki alanı denetleyicisi Azure'da uygulaması yanında dağıtılması belirlenir.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Senaryo konuları ve nasıl teknoloji alanları senaryosuyla
* [Ağ topolojisi](#BKMK_NetworkTopology): sahip Azure sanal ağı oluşturmak [şirketler arası bağlantı](../vpn-gateway/vpn-gateway-site-to-site-create.md).
* [Yükleme yöntemi](#BKMK_InstallMethod): Şirket Windows Server Active Directory etki alanından çoğaltma DC'leri dağıtın. Çoğaltma için DC, Windows Server AD DS'nı VM'e yükleyin ve isteğe bağlı olarak yeni DC yükleme sırasında çoğaltılması veri miktarını azaltmak için yükleme gelen medya (IFM) özelliğini kullanın. Bir öğretici için bkz: [Azure üzerinde Active Directory etki alanı Denetleyicisi'nin çoğaltmasını yükleme](active-directory-install-replica-active-directory-domain-controller.md). IFM kullansanız bile, sanal DC şirket içi yapı ve tüm sanal sabit Disk (VHD) Windows Server AD DS yüklemesi sırasında çoğaltmak yerine buluta taşımak için daha etkili olabilir. Güvenliğiniz için Azure'a kopyalandıktan sonra şirket içi ağdan VHD silmeniz önerilir.
* [Windows Server Active Directory site topolojisi](#BKMK_ADSiteTopology): yeni bir Azure site Active Directory Siteleri ve Hizmetleri oluşturun. Azure sanal ağı temsil eder ve alt siteye eklemek için bir Windows Server Active Directory alt ağ nesnesi oluşturun. Yeni Azure sitesi ve Azure sanal ağ VPN bitiş noktası denetlemek ve Windows Server Active Directory Azure gelen ve giden trafiği en iyi duruma getirmek için bulunduğu site içeren yeni bir site bağlantısı oluşturun.
* [IP adresi ve DNS](#BKMK_IPAddressDNS):
  
  * Statik bir IP adresi için DC kümesi AzureStaticVNetIP Azure PowerShell cmdlet'ini kullanarak ayarlayın.
  * Yükleyin ve Azure ile ilgili etki alanı denetleyicileri Windows Server DNS yapılandırın.
  * Adı ve IP adresi DC ve DNS sunucu rollerini barındıran VM sanal ağ özelliklerini yapılandırın.
* [Coğrafi olarak dağıtılan DC'leri](#BKMK_DistributedDCs): ek sanal ağlar gerektiği gibi yapılandırın. Active Directory site topolojisi Active Directory Siteleri buna göre oluşturmak istediğiniz daha farklı Azure bölgeleri için karşılık gelen coğrafyalara Dc'lerine gerekiyorsa.
* [Salt okunur DC'leri](#BKMK_RODC): dağıttığınız RODC Azure sitedeki, gerçekleştirmek için gereksinimlerinize bağlı olarak yazma işlemleri DC karşı ve uygulamaların ve hizmetlerin uyumluluk RODC'ler ile sitedeki. Uygulama uyumluluğu hakkında daha fazla bilgi için bkz: [salt okunur etki alanı denetleyicileri uygulama uyumluluk Kılavuzu](https://technet.microsoft.com/library/cc755190).
* [Genel katalog](#BKMK_GC): GC'ler çok etki alanlı ormanda hizmeti oturum açma istekleri için gereklidir. Azure sitedeki bir GC dağıtmayın, kimlik doğrulama isteklerini diğer sitelerde sorguları GC'ler neden çıkış trafiği ücrete neden. Bu trafiğini en aza indirmek için Active Directory Siteleri ve Hizmetleri Azure site için evrensel grup üyeliğini önbelleğe almayı etkinleştirebilirsiniz.
  
    Bir GC dağıtırsanız, böylece Azure sitedeki GC kaynak DC olarak aynı kısmi etki alanı bölümlerini çoğaltmak için gereken diğer GC'ler tarafından tercih edilen değil site bağlantıları ve site bağlantı maliyetleri yapılandırın.
* [Windows Server AD DS veritabanını ve SYSVOL yerleşimini](#BKMK_PlaceDB): Windows Server Active Directory veritabanı, günlükler ve SYSVOL için depolamak üzere Azure Vm'lerinde çalışan DC'ler için bir veri diski ekleyin.
* [Yedekleme ve geri yükleme](#BKMK_BUR): sistem durumu yedeklemeleri depolamak istediğiniz belirler. Gerekirse, başka bir veri diski yedeklemelerini depolamak için DC VM ekleyin.

## <a name="deployment-decisions-and-factors"></a>Dağıtım kararlarını ve Etkenler
Bu tabloda Yukarıdaki senaryoların ve aşağıdaki daha ayrıntılı bilgi için bağlantılar ile birlikte dikkate alınması gereken ilgili kararları etkilenen Windows Server Active Directory teknoloji alanları özetlenmektedir. Bazı teknoloji alanlarının her dağıtım senaryosu için uygun olmayabilir ve bazı teknoloji alanları diğer teknoloji alanları dağıtım senaryosu için daha önemli olabilir.

Herhangi bir ek çoğaltma gereksinimi oluşturmayacağından Örneğin, bir sanal ağ üzerinde çoğaltma DC dağıtmak ve tek bir etki alanı ormanınız varsa, genel katalog sunucusu dağıtmak seçerek bu durumda dağıtım senaryosu için kritik olmaz. Orman birkaç etki alanı varsa, diğer yandan, ardından sanal ağ üzerinde bir genel katalog dağıtma kararı kullanılabilir bant genişliğini, performans, kimlik doğrulama, dizin aramaları ve benzeri etkileyebilir.

| Windows Server Active Directory teknoloji alanı | Kararları | Etkenler |
| --- | --- | --- |
| [Ağ topolojisi](#BKMK_NetworkTopology) |Bir sanal ağ oluşturma? |<li>Corp kaynaklara erişmek için gereksinimler</li> <li>Kimlik Doğrulaması</li> <li>Hesap yönetimi</li> |
| [DC dağıtım yapılandırması](#BKMK_DeploymentConfig) |<li>Ayrı bir ormanda herhangi bir güveni olmadan dağıtmak?</li> <li>Federasyon ile yeni bir orman dağıtabilir?</li> <li>Windows Server Active Directory orman güveni ile yeni bir orman veya Kerberos dağıtmak?</li> <li>Corp ormanı çoğaltma DC dağıtarak genişletmek?</li> <li>Yeni alt etki alanı veya etki alanı ağacı dağıtarak Corp ormanı genişletmek?</li> |<li>Güvenlik</li> <li>Uyumluluk</li> <li>Maliyet</li> <li>Esneklik ve hataya dayanıklılık</li> <li>Uygulama uyumluluğu</li> |
| [Windows Server Active Directory site topolojisi](#BKMK_ADSiteTopology) |Nasıl ile Azure sanal trafiği iyileştirmek ve maliyeti en aza indirmek için ağ alt ağlar, siteler ve site bağlantıları yapılandırabilirim? |<li>Alt ağ ve site tanımları</li> <li>Site bağlantı özelliklerini ve bildirim değiştirme</li> <li>Çoğaltma sıkıştırma</li> |
| [IP adresi ve DNS](#BKMK_IPAddressDNS) |IP adresleri ve ad çözümleme İlkesi nasıl yapılandırılır? |<li>Statik IP adresi atamak için kullanım kümesi AzureStaticVNetIP cmdlet'ini kullanın</li> <li>Windows Server DNS sunucusu yüklemek ve adı ve IP adresi DC ve DNS sunucu rollerini barındıran VM sanal ağ özelliklerini yapılandırın</li> |
| [Coğrafi olarak dağıtılan DC'leri](#BKMK_DistributedDCs) |Ayrı sanal ağlarda DC'lere çoğaltmak nasıl? |Active Directory site topolojisi Active Directory Siteleri buna göre oluşturmak istediğiniz daha farklı Azure bölgeleri için karşılık gelen coğrafyalara Dc'lerine gerekiyorsa. [Sanal ağ bağlantısı için sanal ağ yapılandırma](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) ayrı sanal ağlar üzerindeki etki alanı denetleyicileri arasında çoğaltmak için. |
| [Salt okunur DC'leri](#BKMK_RODC) |Salt okunur veya yazılabilir DC'ler kullanır? |<li>HBI/PII öznitelikleri filtre</li> <li>Filtre gizli</li> <li>Sınır giden trafik</li> |
| [Genel katalog](#BKMK_GC) |Genel katalog yüklensin mi? |<li>Tek etki alanlı orman için tüm DC'leri GC'ler olun</li> <li>Çoklu etki alanı ormanı için GC'ler kimlik doğrulaması için gereklidir</li> |
| [Yükleme yöntemi](#BKMK_InstallMethod) |Azure'da DC yükleme nasıl? |Ya da: <li>Windows PowerShell veya Dcpromo kullanarak AD DS'yi yükleme</li> <li>VHD'yi bir şirket içi sanal DC taşıyın</li> |
| [Windows Server AD DS veritabanını ve SYSVOL yerleşimi](#BKMK_PlaceDB) |Windows Server AD DS veritabanı, günlükler ve SYSVOL depolamak nerede? |Dcpromo.exe varsayılan değerlerini değiştirin. Bu kritik Active Directory dosyalarını *gerekir* yazma önbelleğini uygulayan işletim sistemi disklerinde yerine yerleştirilen Azure veri disklerde. |
| [Yedekleme ve Geri Yükleme](#BKMK_BUR) |Koruma ve veri kurtarmak nasıl? |Sistem durumu yedeklemeleri oluşturma |
| [Federasyon sunucusu yapılandırma](#BKMK_FedSrvConfig) |<li>Bulutta Federasyon ile yeni bir orman dağıtabilir?</li> <li>AD FS şirket içi dağıtma ve bulutta bir proxy kullanıma?</li> |<li>Güvenlik</li> <li>Uyumluluk</li> <li>Maliyet</li> <li>İş ortakları tarafından uygulamalara erişim</li> |
| [Bulut Hizmetleri Yapılandırması](#BKMK_CloudSvcConfig) |Bir bulut hizmeti örtük olarak bir sanal makine oluşturmak ilk kez dağıtılır. Ek bulut hizmetlerini dağıtın gerekiyor mu? |<li>Bir VM veya VM'ler Internet'e doğrudan Etkilenme gerektiriyor mu?</li> <li> Hizmet, Yük Dengeleme gerektiriyor mu?</li> |
| [Ortak ve özel IP adresi (sanal IP ve dinamik IP) için Federasyon sunucusu gereksinimleri](#BKMK_FedReqVIPDIP) |<li>Windows Server AD FS örneği doğrudan Internet üzerinden erişilmesi gerekiyor mu?</li> <li>Bulutta dağıtılan uygulama kendi Internet'e yönelik IP adresi ve bağlantı noktası gerektiriyor mu?</li> |Dağıtımınızda gerekli her sanal IP adresi için bir bulut hizmeti oluşturun |
| [Windows Server AD FS yüksek kullanılabilirlik yapılandırması](#BKMK_ADFSHighAvail) |<li>Windows Server AD FS sunucu grubumu kaç düğümler?</li> <li>Windows Server AD FS proxy grubumu dağıtmak için kaç tane düğümleri?</li> |Dayanıklılık ve hataya dayanıklılık |

### <a name="BKMK_NetworkTopology"></a>Ağ topolojisi
IP adresi tutarlılığını ve Windows Server AD DS DNS gereksinimlerini karşılamak için önce oluşturmak ise gerekli bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ve sanal makinelerinizi eklemektir. Kendi oluşturma sırasında isteğe bağlı olarak Azure sanal makineler için şirket içi makineler saydam bağlanır, şirket içi Kurumsal ağa bağlanma genişletmeye karar vermeniz gerekir — bu geleneksel VPN teknolojilerini kullanarak elde edilir ve VPN uç noktasının şirket ağına kenarında gösterilmesine gerektirir. Diğer bir deyişle, VPN şirket ağına değil tam tersini Azure'dan başlatılır.

Ek ücretlere her VM için geçerli standart ücretler dışında şirket içi ağınıza bir sanal ağ genişletirken geçerli olduğunu unutmayın. Özellikle, Azure sanal ağ geçidinin CPU süresi ve VPN üzerinden şirket içi makinelerle iletişim kurar her bir VM tarafından oluşturulan çıkış trafiği için ücret vardır. Ağ trafiği ücretlere hakkında daha fazla bilgi için bkz: [Azure bir bakışta fiyatlandırma](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>DC dağıtım yapılandırması
Azure üzerinde çalıştırmak istediğiniz gereksinimleri hizmeti için etki alanı denetleyicisi yapılandırma biçiminizi bağlıdır. Örneğin, yeni ormandaki bir, kavram, yeni bir uygulama veya dizin hizmetleri ancak özgü olmayan iç şirket kaynaklarına erişim gerektiren bazı bir kısa vadeli projeyi test etmek için kendi şirket, yalıtılmış bir orman dağıtabilirsiniz.

Bir avantaj olarak DC ile çoğaltmıyor ayrı bir ormanda doğrudan maliyetlerini azaltır, daha az giden ağ trafiğinin sistem kendisi tarafından oluşturulan kaynaklanan DC'leri şirket içi. Ağ trafiği ücretlere hakkında daha fazla bilgi için bkz: [Azure bir bakışta fiyatlandırma](http://azure.microsoft.com/pricing/).

Başka bir örnek olarak, bir hizmet için gizlilik gereksinimleri vardır, ancak, iç Windows Server Active Directory erişimi hizmet bağlı varsayalım. Bulut hizmeti için verileri barındırmak için izin verilen, Azure ile ilgili iç ormanınız için yeni bir alt etki alanı dağıtabilirsiniz. Bu durumda, yeni alt etki alanı (olmadan gizlilik endişelere yardımcı olmak için genel katalog) için bir DC dağıtabilirsiniz. Bir çoğaltma DC dağıtımı ile birlikte bu senaryo, şirket içi DC'leri bağlanabilirlik için bir sanal ağ gerektirir.

Yeni bir orman oluşturursanız, kullanılıp kullanılmayacağını seçmesine [Active Directory güvenleri](https://technet.microsoft.com/library/cc771397) veya [federasyon güvenleri](https://technet.microsoft.com/library/dd807036). Uyumluluk, güvenlik, uyumluluk, maliyet ve dayanıklılık tarafından dikte gereksinimleri dengeleyin. Örneğin, yararlanmak için [seçime bağlı kimlik doğrulaması](https://technet.microsoft.com/library/cc755844) Azure üzerinde yeni bir orman dağıtmak ve şirket içi orman ve bulut ormanı arasında bir Windows Server Active Directory güveni oluşturmak isteyebilirsiniz. Ancak, talep kullanan uygulama ise, Active Directory orman güvenleri yerine federasyon güvenleri dağıtabilirsiniz. Başka bir faktör, şirket içi Windows Server Active Directory buluta genişleterek daha fazla veri çoğaltmak ya da kimlik doğrulama ve sorgu yük sonucunda fazla giden trafiği oluşturmak için maliyet olacaktır.

Kullanılabilirlik ve hataya dayanıklılık için gereksinimleri da tercih ettiğiniz etkiler. Örneğin, bağlantı kesilirse, Kerberos güven veya bir Federasyon yararlanan uygulamalar Azure üzerinde yeterli altyapı dağıttığınız sürece tüm büyük olasılıkla tamamen ayrılır güven. Çoğaltma DC'leri gibi alternatif dağıtım yapılandırmaları (yazılabilir veya RODC'ler) bağlantı kesintileri tolerans mümkün olma olasılığını artırmak.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory site topolojisi
Siteler ve site bağlantıları trafiği iyileştirmek ve maliyeti en aza indirmek için doğru tanımlamanız gerekir. Siteleri, site bağlantıları ve alt ağları DC'leri ve kimlik doğrulama trafik akışını arasındaki çoğaltma topolojisi etkiler. Aşağıdaki trafiği ücretlere göz önünde bulundurun ve dağıtmak ve DC'leri dağıtım senaryonuz gereksinimlerine göre yapılandırın:

* Ağ geçidinin kendisi için saat başına nominal bir ücret yoktur:
  
  * Başlatılan ve uygun gördüğünüz şekilde durduruldu
  * Durmuşsa, Azure Vm'leri ve şirket ağından yalıtılır
* Gelen trafik ücretsizdir
* Giden trafik seçili göre [Azure bir bakışta fiyatlandırma](http://azure.microsoft.com/pricing/). Şirket içi siteleri ve bulut siteler arasında site bağlantı özellikleri şu şekilde en iyi duruma getirebilirsiniz:
  
  * Birden çok sanal ağlar kullanıyorsanız, site bağlantıları ve bunların maliyetleri uygun şekilde Windows Server AD DS hizmeti ücret ödemeden aynı düzeyde sağlayabilen bir üzerinden Azure site öncelik önlemek için yapılandırın. Ayrıca, tüm (varsayılan olarak etkindir) bağlantı (BASL) seçeneğinden site köprüsü devre dışı bırakma düşünebilirsiniz. Bu sitelere yalnızca doğrudan bağlı başka bir işlemle çoğaltmak sağlar. Geçişli bağlı sitelerdeki DC'leri artık birbirleriyle doğrudan çoğaltamaz, ancak yaygın bir site veya siteler arasında çoğaltılması gerekir. Ara siteler için herhangi bir nedenle kullanılamaz duruma gelirse, siteler arasında bağlantı kullanılabilir olsa bile arasında DC'leri geçişli bağlı sitelerdeki çoğaltmayı gerçekleşmez. Son olarak, geçişli çoğaltma davranışı bölümlerini arzu kalır Burada, site uygun site bağlantıları ve şirket içi, kurumsal ağ alanları gibi siteler içeren bağlantı köprüleri oluşturun.
  * [Site bağlantı maliyetleri yapılandırma](https://technet.microsoft.com/library/cc794882) uygun şekilde istenmeyen trafiğini engellemek için. Örneğin, varsa **deneyin sonraki en yakın siteye** ayarı etkinleştirildiğinde, sanal ağ sitesi olmadığından sonraki en yakın Azure site şirket ağına bağlanan site bağlantı nesnesinin ilişkili maliyet artırarak emin olun.
  * Site bağlantısı yapılandırma [aralıkları](https://technet.microsoft.com/library/cc794878) ve [zamanlamaları](https://technet.microsoft.com/library/cc816906) tutarlılığı gereksinimlerine ve nesne değişikliklerini hızına göre. Çoğaltma zamanlamasını gecikme Toleransı ile hizalar. Çoğaltma aralığı azaltıldığında varsa yeterli nesne değişikliği oranı maliyetleri kaydedebilmeniz için bir değer yalnızca en son durumunu DC'leri çoğaltılır.
* Maliyetleri en aza bir öncelik ise, çoğaltma zamanlanmış ve değişiklik bildirimi etkinleştirilmemiş emin olun. Siteler arasında çoğaltma yapılırken varsayılan yapılandırma budur. Bu RODC giden değişiklikler çoğaltılmaz olduğundan sanal ağ üzerinde bir RODC dağıtıyorsanız önemli değildir. Ancak yazılabilir DC dağıtırsanız, gereksiz sıklığı güncelleştirmeleri çoğaltmak için site bağlantısını yapılandırılmamış emin olmanız gerekir. Bir genel katalog sunucusu (GC) dağıtırsanız, GC içeren her bir site bir site bağlantı veya GC daha düşük maliyetli Azure sitede sahip site bağlantıları ile bağlantılı bir sitedeki DC bir kaynaktan etki alanı bölümlerini çoğaltır emin olun.
* Hala daha fazla çoğaltma sıkıştırma algoritması değiştirerek siteler arasında çoğaltma tarafından oluşturulan ağ trafiğini azaltmak mümkündür. Sıkıştırma algoritması REG_DWORD kayıt defteri girişi HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator sıkıştırma algoritması tarafından denetlenir. Varsayılan değer 3, hangi Xpress sıkıştırma algoritması karşılık gelen ' dir. Değeri için MSZip algoritma Değişeni bir 2'ye değiştirebilir. Çoğu durumda bu sıkıştırma artırır, ancak bunu CPU kullanımı ödün verme pahasına yapar. Daha fazla bilgi için bkz: [nasıl Active Directory çoğaltma topolojisi works](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP adresi ve DNS
Azure sanal makineler "adreslerini DHCP kiralık" varsayılan olarak ayrılır. Azure sanal ağı dinamik adresler sahip bir sanal makine sanal makine ömrü boyunca kalıcı hale getirmek için Windows Server AD DS gereksinimleri karşılıyor.

Sonuç olarak, Azure üzerinde bir dinamik adres kullandığınızda, bir statik IP adresi kira süresi için yönlendirilebilir ve kira süresi için bulut hizmeti kullanım ömrü eşit olduğundan kullanarak etkisi de sahipsiniz.

Ancak, VM kapatma ise dinamik adresi serbest bırakıldı. IP adresi serbest gelen önlemek için [kümesi AzureStaticVNetIP statik bir IP adresi atamak için kullandığınız](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Ad çözümlemesi için kendi dağıtma (veya var olan yararlanan) bir DNS sunucusu altyapısında; Azure tarafından sağlanan DNS Windows Server AD DS Gelişmiş ad çözünürlük gereksinimlerini karşılamıyor. Örneğin, bunu değil dinamik SRV kayıtlarını desteklemesi ve benzeri. Ad çözümlemesi DC'leri ve etki alanına katılmış istemciler için önemli yapılandırma öğesidir. DC'lerin kaynak kayıtlarını kaydetme ve diğer DC'nin kaynak kayıtları çözme yeteneğinin olması gerekir.
Hataya dayanıklılık ve Performans nedeniyle, Windows Server DNS hizmeti Azure üzerinde çalışan DC'ler yüklemek için idealdir. Ardından adı ve DNS sunucusunun IP adresi ile Azure sanal ağ özelliklerini yapılandırın. Sanal ağ üzerindeki diğer VM'ler başlattığınızda, DNS istemci çözümleyici ayarlarına DNS sunucusu dinamik IP adresi ayırma bir parçası olarak yapılandırılır.

> [!NOTE]
> Şirket içi bilgisayarlar doğrudan Internet üzerinden Azure üzerinde barındırılan bir Windows Server AD DS Active Directory etki alanına katılamaz. Active Directory ve etki alanına katılma işlemi için pratik doğrudan oluşturmak için bağlantı noktası gereksinimleri gerekli bağlantı noktalarını kullanıma ve etkisi, internet tüm bir DC.
> 
> 

Sanal makineleri otomatik olarak başlangıç veya bir ad değişikliği olduğunda kendi DNS adını kaydettirin.

Bu örnek ve ilk VM sağlamak ve üzerinde AD DS'yi yüklemek nasıl oluşturulduğunu gösteren başka bir örnek hakkında daha fazla bilgi için bkz: [Microsoft Azure üzerinde yeni bir Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md). Windows PowerShell'i kullanma hakkında daha fazla bilgi için bkz: [Azure PowerShell yükleme](/powershell/azureps-cmdlets-docs) ve [Azure Yönetimi cmdlet'leri](/powershell/module/azurerm.compute/#virtual_machines).

### <a name="BKMK_DistributedDCs"></a>Coğrafi olarak dağıtılan DC'leri
Azure farklı sanal ağlar üzerindeki birden çok DC'leri barındırdığında avantajları sağlar:

* Çok siteli hataya dayanıklılık
* Fiziksel olarak yakın olmayı şube ofisleri (daha düşük gecikme süresi)

Sanal ağlar arasında doğrudan iletişim yapılandırma hakkında daha fazla bilgi için bkz: [sanal ağ bağlantısı sanal ağa yapılandırma](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Salt okunur DC'leri
Salt okunur veya yazılabilir DC'leri dağıtılıp dağıtılmayacağını seçmeniz gerekir. Çünkü, bunları fiziksel denetime sahip olmaz ancak RODC'ler fiziksel güvenlik şube ofisleri gibi risk olduğu konumlarda dağıtılması için tasarlanmış RODC'ler dağıtma inclined.

Azure bir şube ofisi fiziksel güvenlik riskini mevcut olmayan, ancak RODC'ler hala sağladıkları özellikleri barındırabilir çok farklı nedenlerle bu ortamlar için oldukça uygun olduğundan daha uygun maliyetli olması kanıtlamak. Örneğin, RODC'ler giden çoğaltma varsa ve gizli anahtarları (Parolalar) seçmeli olarak doldurabilir. Dezavantajı, bu Sırları eksikliği isteğe bağlı bir kullanıcı olarak doğrulamak için giden trafiği gerektirebilir veya bilgisayarın kimliğini doğrular. Ancak gizli seçerek önceden doldurulmaz ve önbelleğe alınmış.

RODC için hassas verileri içeren öznitelikler öznitelik kümesi (SK'lar) filtre eklemek için RODC'ler içinde ve HBI ve PII sorunları çevresinde ek bir avantaj sağlar. SK'lar RODC'ler için çoğaltılmamış öznitelikleri özelleştirilebilir kümesidir. İzin verilmez veya PII veya HBI Azure üzerinde depolamak istemediğiniz durumunda SK'lar koruyucu olarak kullanabilirsiniz. Daha fazla bilgi için [RODC filtrelenmiş öznitelik kümesi [(https://technet.microsoft.com/library/cc753459)].

Uygulamaları ile kullanmayı planlıyorsanız RODC'ler uyumlu olduğundan emin olun. Çoğu Windows Server Active Directory özellikli uygulamalar da RODC'ler ile çalışır, ancak bazı uygulamalar inefficiently gerçekleştirin veya yazılabilir DC erişiminiz yoksa başarısız. Daha fazla bilgi için bkz: [salt okunur DC'leri uygulama uyumluluk Kılavuzu](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Genel katalog
Bir genel katalog (GC) yüklenip yüklenmeyeceğini seçmeniz gerekir. Tek etki alanlı bir ormanınız tüm DC'leri genel katalog sunucuları olarak yapılandırmanız gerekir. Hiçbir ek çoğaltma trafiğini olacağından maliyetleri artmaz.

Bir çok etki alanlı ormanda GC'ler evrensel grup üyeliği kimlik doğrulama işlemi sırasında genişletmek gereklidir. Bir GC dağıtmayın, Azure üzerinde bir DC kimlik doğrulaması iş yüklerini sanal ağda dolaylı olarak her kimlik doğrulama girişimi sırasında GC'ler şirket içi sorgulamak için giden bağlantı kimlik doğrulaması trafiği oluşturur.

Her etki alanı (bölüm içinde) barındırmak için GC'ler ile ilişkili maliyetleri daha az tahmin edilebilir. İş yükü Internet'e hizmeti barındıran ve Windows Server AD DS karşı kullanıcıların kimliğini doğrular, maliyetleri tamamen öngörülemeyen olabilir. Kimlik doğrulaması sırasında bulut site dışında GC sorguları azaltmaya yardımcı olmak için [evrensel grup üyeliğini önbelleğe'almayı etkinleştir](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Yükleme yöntemi
Sanal ağ DC'ler yükleme seçmeniz gerekir:

* Yeni DC'leri yükseltin. Daha fazla bilgi için bkz: [bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md).
* Bir şirket içi sanal DC VHD buluta taşıyın. Bu durumda, şirket içi sanal DC ","kopyalanan"veya"kopyalanan."değil taşınacağını" sağlamanız gerekir.

Yalnızca Azure sanal makineleri DC'ler için (Azure "web" veya "alt" rolü aksine VM'ler) kullanın. Dayanıklı ve dayanıklılık durumunun bir DC için gereklidir. Azure sanal makineleri DC'leri gibi iş yükleri için tasarlanmıştır.

SYSPREP, dağıtmak veya DC'leri kopyalamak için kullanmayın. DC'leri kopyalama olanağı yalnızca başında Windows Server 2012 ' dir. Kopyalama özelliği, temel alınan hiper yöneticide VMGenerationID için desteği gerektirir. Üçüncü taraf sanallaştırma yazılım satıcıları gibi Hyper-V'de, Windows Server 2012 ve Azure sanal ağlar her ikisi de VMGenerationID, destekler.

### <a name="BKMK_PlaceDB"></a>Windows Server AD DS veritabanını ve SYSVOL yerleşimi
Windows Server AD DS veritabanının, günlükler ve SYSVOL konumu seçin. Azure veri disklerde dağıtılmalıdır.

> [!NOTE]
> Azure veri diski 4 TB ile sınırlıdır.
> 
> 

Veri disk sürücüleri değil önbellek yazma varsayılan yapın. Bir VM'ye bağlı olan veri disk sürücüleri yazma önbelleği kullanın. Sanal makinenin işletim sistemi perspektifinden işlem tamamlanmadan önce yazan yapar emin önbelleğe alma yazma dayanıklı Azure depolama alanına taahhüt eder. Biraz daha yavaş yazma ödün verme pahasına dayanıklılık sağlar.

Arka planda yazma disk önbelleği DC tarafından yapılan varsayımları geçersiz kılar çünkü bu Windows Server AD DS için önemlidir. Yazma önbelleğini devre dışı bırakmak Windows Server AD DS çalışır ancak bunu vermenizin GÇ sistem sürücü kadar. Hata yazma önbelleğini devre dışı bırakmak için belirli koşullar altında nesneleri ve diğer sorunları kalan içinde kaynaklanan USN geri alma çıkarabilir.

Sanal DC'leri için en iyi uygulama olarak aşağıdakileri yapın:

* Konak önbelleği tercihi Azure veri diskte hiçbiri için ayarı. Bu, AD DS işlemleri için yazma önbelleğini sorunları önler.
* Veritabanı, günlükler ve SYSVOL ya da aynı depolama veri diski veya ayrı veri disklerinin. Genellikle, ayrı bir disk işletim sistemi kendisi için kullanılan diskten budur. Anahtar takeaway, Windows Server AD DS veritabanını ve SYSVOL'ü bir Azure işletim sistemi disk türü depolanmalıdır değil, ' dir. Varsayılan olarak, AD DS yükleme işlemi için Azure önerilmez % systemroot % klasöründe bu bileşenleri yükler.

### <a name="BKMK_BUR"></a>Yedekleme ve geri yükleme
Ne olduğu ve yedekleme ve geri yükleme bir DC için genel desteklenmiyor ve daha belirgin olarak bu bir VM'de çalıştıran farkında olun. Bkz: [yedekleme ve geri yükleme hakkında önemli noktalar sanallaştırılmış DC'ler için](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Sistem durumu yedeklemeleri yalnızca Windows Server AD DS, Windows Server Yedekleme gibi yedekleme gereksinimleri, özellikle farkındadır yedekleme yazılımı kullanarak oluşturun.

Kopyalamayın veya VHD dosyaları DC'lerin düzenli yedeklemeler gerçekleştirmek yerine klonlayın. Bir geri yükleme şimdiye kadar Windows Server 2012 ve desteklenen bir hiper yönetici olmadan kopyalanan ya da kopyalanmış VHD'lerin USN balonları tanıtılacaktır bunları yapmak, gerekli olmalıdır.

### <a name="BKMK_FedSrvConfig"></a>Federasyon sunucusu yapılandırma
Windows Server AD FS federasyon sunucuları (Sts'ler) yapılandırma kısmen olup Azure üzerinde dağıtmak istediğiniz uygulamaları, şirket içi ağınızdaki kaynaklara erişmek gereksinimlerine göre değişir.

Uygulamalar aşağıdaki ölçütleri karşılamıyorsa, şirket içi ağınızdan yalıtım uygulamalarda dağıtabilirsiniz.

* SAML güvenlik belirteçleri kabul
* Internet'e exposable
* Şirket içi kaynaklara erişim yok

Bu durumda, Windows Server AD FS Sts'ler gibi yapılandırın:

1. Ayrı bir tek etki alanlı ormanda Azure üzerinde yapılandırın.
2. Windows Server AD FS federasyon sunucusu grubu yapılandırarak orman federe erişim sağlar.
3. Windows Server AD FS (Federasyon sunucusu grubu ve Federasyon sunucusu proxy'si grubu) şirket içi ormanda yapılandırın.
4. Şirket içi ve Windows Server AD FS Azure örnekleri arasında bir federasyon güven ilişkisi oluşturun.

Uygulamalar şirket kaynaklarına erişimi gerekiyorsa, diğer yandan, Windows Server AD FS Azure üzerinde bu uygulama ile aşağıdaki gibi yapılandırabilirsiniz:

1. Şirket içi ağlar ve Azure arasında bağlantı yapılandırın.
2. Windows Server AD FS federasyon sunucusu grubu şirket içi ormanda yapılandırın.
3. Azure üzerinde bir Windows Server AD FS federasyon sunucusu proxy'si grubu yapılandırın.

Bu yapılandırma, şirket içi kaynakları, bir çevre ağında uygulamalarla Windows Server AD FS yapılandırmaya benzer riskini azaltma avantajına sahiptir.

İşletmeler için işbirliği gerekirse her iki durumda da, güven ilişkileri daha fazla kimlik sağlayıcıları ile kurup olduğunu unutmayın.

### <a name="BKMK_CloudSvcConfig"></a>Bulut Hizmetleri Yapılandırması
Bulut Hizmetleri, bir VM İnternete doğrudan kullanıma sunmak veya Internet'e yönelik Yük dengeli bir uygulamasını kullanıma sunmak istiyorsanız gereklidir. Her bir bulut hizmeti tek bir yapılandırılabilir sanal IP adresi sağladığından, bu mümkündür.

### <a name="BKMK_FedReqVIPDIP"></a>Ortak ve özel IP adresi (sanal IP ve dinamik IP) için Federasyon sunucusu gereksinimleri
Her Azure sanal makine dinamik bir IP adresi alır. Dinamik bir IP adresi özel bir adresi yalnızca Azure içinde erişilebilir değil. Çoğu durumda, ancak bu, Windows Server AD FS dağıtımları için sanal bir IP adresi yapılandırmak için gerekli olacaktır. Sanal IP internet Windows Server AD FS bitiş noktaları kullanıma sunmak gereklidir ve Federal iş ortakları ve istemcileri tarafından kimlik doğrulaması ve devam eden yönetimi için kullanılacaktır. Bir sanal IP adresi, bir veya daha fazla Azure sanal makineler içeren bir bulut hizmeti özelliğidir. Azure ve Windows Server AD FS dağıtılan talep kullanan uygulamaya Internet'e yönelik ve Paylaşım ortak bağlantı noktaları varsa, her biri kendi sanal bir IP adresi gerektirir ve bu nedenle uygulama için bir bulut hizmeti ve Windows Server AD FS için ikinci bir oluşturmak için gerekli olacaktır.

Dinamik IP adresi ve şartları sanal IP adresi tanımları için bkz: [terimleri ve tanımları](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS yüksek kullanılabilirlik yapılandırması
Tek başına Windows Server AD FS Federasyon Hizmetleri dağıtmak mümkün olsa da, AD FS STS ve üretim ortamları için proxy'leri için en az iki düğüme sahip bir gruba dağıtmak için önerilir.

Bkz: [AD FS 2.0 dağıtım topolojisi hakkında önemli noktalar](https://technet.microsoft.com/library/gg982489) içinde [AD FS 2.0 Tasarım Kılavuzu](https://technet.microsoft.com/library/dd807036) en iyi hangi dağıtım yapılandırma seçenekleri belirli gereksinimlerinize göre karar vermek için.

> [!NOTE]
> Azure üzerinde Windows Server AD FS bitiş noktaları için dengelemesini alabilmek için aynı bulut hizmeti Windows Server AD FS grubunu tüm üyelerini yapılandırmak ve HTTPS bağlantı noktası (varsayılan 443) ve Azure HTTP (varsayılan 80) için Yük Dengeleme özelliğini kullanın. Daha fazla bilgi için bkz: [Azure yük dengeleyici araştırmasını](https://msdn.microsoft.com/library/azure/jj151530).
> Windows Server Ağ Yükü Dengeleme (NLB), Azure üzerinde desteklenmiyor.
> 
> 

