---
title: Azure’da Active Directory Federasyon Hizmetleri | Microsoft Belgeleri
description: Bu belgede AD FS’yi yüksek kullanılabilirlik için Azure’a dağıtma hakkında bilgi edineceksiniz.
keywords: azure’da AD FS dağıtma, azure adfs dağıtma, azure adfs, azure ad fs, adfs dağıtma, ad fs dağıtma, azure’da adfs, azure’da adfs dağıtma, azure’da AD FS dağıtma, adfs azure, AD FS’ye giriş, Azure, Azure’da AD FS, iaas, ADFS, adfs’yi azure’a taşıma
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 76ed05d55389e2c05b38fe1f2c239f544c6a5d38
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Azure'da Active Directory Federasyon Hizmetlerini dağıtma
AD FS basitleştirilmiş, güvenli kimlik federasyonu ve Web’de çoklu oturum açma (SSO) özellikleri sağlar. Azure AD veya O365 ile federasyon, kullanıcıların şirket içi kimlik bilgilerini kullanarak kimlik doğrulaması yapmasını ve buluttaki tüm kaynaklara erişmesini sağlar. Sonuç olarak, hem şirket içindeki hem de buluttaki kaynaklara erişimi sağlamak için yüksek oranda kullanılabilir bir AD FS altyapısına sahip olunması önemlidir. AD FS'nin Azure’da dağıtılması en az çaba ile yüksek kullanılabilirlik elde etmeye yardımcı olabilir.
AD FS'yi Azure’da dağıtmanın çeşitli avantajları vardır ve birkaç tanesi aşağıda listelenmiştir:

* **Yüksek Kullanılabilirlik** - Azure Kullanılabilirlik Kümeleri’nin gücü ile yüksek oranda kullanılabilir bir altyapıya sahip olabilirsiniz.
* **Kolay Ölçeklendirme** – Daha fazla performans mı gerekiyor? Azure’da yalnızca birkaç tıklama ile daha güçlü makinelere kolayca geçiş yapın
* **Çapraz Coğrafi Yedeklilik** – Azure Coğrafi Yedeklilik ile altyapınızın dünya çapında yüksek oranda kullanılabilir olduğundan emin olabilirsiniz
* **Kolay Yönetim** – Azure portalındaki oldukça basit yönetim seçenekleri ile altyapınızın yönetimi çok kolay ve zahmetsizdir 

## <a name="design-principles"></a>Tasarım ilkeleri
![Dağıtım tasarımı](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Yukarıdaki diyagramda AD FS altyapınızı Azure’a dağıtmaya başlamak için önerilen temel topoloji gösterilmektedir. Topolojinin çeşitli bileşenlerinin ardında yatan ilkeler aşağıda listelenmiştir:

* **DC / ADFS Sunucuları**: 1.000’den az kullanıcınız varsa AD FS rolünü etki alanı denetleyicilerinize yükleyebilirsiniz. Etki alanı denetleyicileri üzerinde bir performans etkisi oluşmasını istemiyorsanız ya da 1.000’den fazla kullanıcınız varsa AD FS’yi ayrı sunuculara dağıtabilirsiniz.
* **WAP Sunucusu** – Kullanıcıların şirket ağında olmadıkları zamanlarda da AD FS’ye ulaşabilmeleri için Web Uygulaması Proxy sunucularının dağıtılması gerekir.
* **DMZ**: Web Uygulaması Proxy sunucuları DMZ’ye yerleştirilir ve DMZ ile dahili alt ağ arasında YALNIZCA TCP/443 erişimine izin verilir.
* **Yük Dengeleyiciler**: AD FS ve Web Uygulaması Proxy sunucularının yüksek kullanılabilirliğini sağlamak için AD FS sunucuları için bir dahili yük dengeleyici ve Web Uygulaması Proxy sunucuları için Azure Load Balancer kullanılması önerilir.
* **Kullanılabilirlik Kümeleri**: AD FS dağıtımınıza yedeklilik sağlamak isterseniz, benzer iş yükleri için bir Kullanılabilirlik Grubunda iki veya daha fazla sanal makineyi gruplandırmanız önerilir. Bu yapılandırma planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar
* **Depolama Hesapları**: İki depolama hesabınızın olması önerilir. Tek bir depolama hesabına sahip olunması tek bir hata noktası oluşturulmasına yol açabilir ve depolama hesabının arıza yaptığı nadir senaryolarda dağıtımın kullanılamaz hale gelmesine neden olabilir. İki depolama hesabı her bir hata satırı için bir depolama hesabını ilişkilendirmenize yardımcı olur.
* **Ağ ayrımı**: Web Uygulaması Proxy sunucuları ayrı bir DMZ ağına dağıtılmalıdır. Bir sanal ağı iki alt ağa bölebilir ve ardından Web Uygulaması Proxy sunucularını yalıtılmış bir alt ağa dağıtabilirsiniz. Her bir alt ağın ağ güvenlik grubu ayarlarını yapılandırabilir ve yalnızca iki alt ağ arasında gerekli iletişime izin verebilirsiniz. Aşağıda her dağıtım senaryosu için daha fazla bilgi verilmiştir

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Azure’a AD FS dağıtma adımları
Bu bölümde belirtilen adımlar aşağıda gösterilen AD FS altyapısını Azure’a dağıtmaya ilişkin yönergeleri ana hatlarıyla vermektedir.

### <a name="1-deploying-the-network"></a>1. Ağı dağıtma
Yukarıda özetlendiği gibi tek bir sanal ağda iki alt ağ oluşturabilir veya birbirinden tamamen farklı iki sanal ağ (VNet) oluşturabilirsiniz. Bu makalede tek bir sanal ağın dağıtımına ve bu sanal ağı iki alt ağa bölmeye odaklanılacaktır. İki ayrı VNet iletişim için VNet'ten VNet’e ağ geçidi gerektirdiğinden bu yaklaşım şu anda daha kolay bir yaklaşımdır.

**1.1 Sanal ağ oluşturma**

![Sanal ağ oluşturma](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)

Azure portalında sanal ağı seçtiğinizde sanal ağı ve bir alt ağı tek tıklamayla hemen dağıtabilirsiniz. INT alt ağı da tanımlanır ve sanal makinelerin eklenmesi için hazırdır.
Sonraki adımda ağa başka bir alt ağ (örn. DMZ alt ağı) eklenir. DMZ alt ağı oluşturmak için

* Yeni oluşturulan ağı seçin
* Özellikler menüsünde alt ağ seçin
* Alt ağ panelinde ekle düğmesine tıklayın
* Alt ağ oluşturmak için alt ağ adı ve adres alanı bilgilerini girin

![Alt ağ](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)

![Alt ağ DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. Ağ güvenlik grupları oluşturma**

Ağ güvenlik grubu (NSG), bir Sanal Ağ üzerindeki VM örneklerinize ağ trafiğinin gitmesine izin veren veya trafiği reddeden Erişim Denetimi Listesi (ACL) kurallarının bir listesini içerir. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur.
Bu kılavuzda dahili ağ ve DMZ için birer tane olmak üzere iki NSG oluşturulacaktır. Bunlar sırasıyla NSG_INT ve NSG_DMZ olarak etiketlenecektir.

![NSG oluşturma](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

NSG oluşturulduktan sonra 0 gelen ve 0 giden kural olacaktır. İlgili sunuculardaki roller yüklenip çalışır duruma geldikten sonra gelen ve giden kurallar istenilen güvenlik düzeyine uygun olarak oluşturulabilir.

![NSG başlatma](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

NSG’ler oluşturulduktan sonra NSG_INT öğesini INT alt ağı ile, NSG_DMZ öğesini DMZ alt ağı ile ilişkilendirin. Örnek bir ekran görüntüsü aşağıda verilmiştir:

![NSG yapılandırma](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Alt ağ panelini açmak için Alt Ağlar’a tıklayın
* NSG ile ilişkilendirilecek alt ağa tıklayın 

Yapılandırmadan sonra Alt Ağlar paneli aşağıdaki gibi görünmelidir:

![NSG’den sonra alt ağlar](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Şirket içi bağlantı oluşturma**

Etki alanı denetleyicisini (DC) Azure’a dağıtmak için şirket içine bağlantı gerekir. Azure, şirket içi altyapınızı Azure altyapınıza bağlamak için çeşitli bağlantı seçenekleri sunar.

* Noktadan siteye
* Sanal Ağ Sitesinden siteye
* ExpressRoute

ExpressRoute kullanılması önerilir. ExpressRoute, Azure veri merkezleri ile şirketinizde veya bir birlikte bulundurma ortamında bulunan altyapı arasında özel bağlantılar oluşturmanızı sağlar. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bunlar İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve yüksek güvenlik sağlar.
ExpressRoute kullanılması önerilse de kuruluşunuz için en uygun olan bağlantı yöntemini seçebilirsiniz. ExpressRoute ve ExpressRoute kullanan çeşitli bağlantı seçenekleri hakkında daha fazla bilgi için [ExpressRoute teknik genel bakış](https://aka.ms/Azure/ExpressRoute) makalesini okuyun.

### <a name="2-create-storage-accounts"></a>2. Depolama hesabı oluşturma
Yüksek kullanılabilirliği sürdürmek ve tek bir depolama hesabına bağımlılığı önlemek için iki depolama hesabı oluşturabilirsiniz. Her bir kullanılabilirlik kümesindeki makineleri iki gruba ayırın ve ardından her grubu ayrı bir depolama hesabına atayın.

![Depolama hesabı oluşturma](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Kullanılabilirlik kümeleri oluşturma
Her rol (DC/AD FS ve WAP) için her biri en az 2 makine içeren kullanılabilirlik kümeleri oluşturun. Bunun yapılması her rol için daha yüksek kullanılabilirlik elde edilmesine yardımcı olur. Kullanılabilirlik kümeleri oluşturulurken aşağıdakilere karar vermek önemlidir:

* **Hata Etki Alanları**: Aynı hata etki alanındaki sanal makineler aynı güç kaynağı ve fiziksel ağ anahtarını paylaşır. En az 2 hata etki alanı önerilir. Varsayılan değer 3’tür ve bu dağıtımda olduğu gibi bırakılabilir
* **Güncelleme etki alanları**: Aynı güncelleme etki alanına ait makineler bir güncelleme sırasında birlikte yeniden başlatılır. En az 2 güncelleme etki alanına sahip olmak istiyorsunuz. Varsayılan değer 5’tir ve bu dağıtımda olduğu gibi bırakılabilir

![Kullanılabilirlik kümeleri](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Aşağıdaki kullanılabilirlik kümelerini oluşturun

| Kullanılabilirlik Kümesi | Rol | Hata etki alanları | Güncelleme etki alanları |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Sanal makineleri dağıtma
Sonraki adım altyapınızdaki farklı rolleri barındıran sanal makinelerin dağıtılmasıdır. Her kullanılabilirlik kümesinde en az iki makine olması önerilir. Temel dağıtım için dört sanal makine oluşturun.

| Makine | Rol | Alt ağ | Kullanılabilirlik kümesi | Depolama hesabı | IP Adresi |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Statik |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Statik |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statik |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statik |

Fark etmiş olabileceğiniz gibi hiçbir NSG belirtilmemiştir. Bunun nedeni Azure’un alt ağ düzeyinde NSG kullanmanıza olanak sağlamasıdır. Bu durumda alt ağ veya ağ arabirimi nesnesi ile ilişkili tek NSG’yi kullanarak makine ağ trafiğini denetleyebilirsiniz. [Ağ Güvenlik Grubu (NSG) nedir?](https://aka.ms/Azure/NSG) makalesinde daha fazla bilgi bulabilirsiniz.
DNS yönetiyorsanız statik IP adresi önerilir. Azure DNS kullanabilir ve etki alanınızın DNS kayıtlarında makinelere Azure FQDN'lerine göre bakabilirsiniz.
Dağıtım tamamlandıktan sonra sanal makine bölmeniz aşağıdaki gibi görünmelidir:

![Dağıtılmış Sanal Makineler](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Etki alanı denetleyicisi / AD FS sunucularını yapılandırma
 Gelen bir isteğin kimliğini doğrulamak için AD FS’nin etki alanı denetleyicisiyle iletişim kurması gerekir. Kimlik doğrulaması amacıyla Azure’dan şirket içi DC’ye maliyetli bir yolculuk yapmak yerine etki alanı denetleyicisinin bir çoğaltmasının Azure’a dağıtılması önerilir. Yüksek kullanılabilirlik elde etmek için en az 2 etki alanı denetleyicisinden oluşan bir kullanılabilirlik kümesi oluşturmanız önerilir.

| Etki alanı denetleyicisi | Rol | Depolama hesabı |
|:---:|:---:|:---:|
| contosodc1 |Çoğaltma |contososac1 |
| contosodc2 |Çoğaltma |contososac2 |

* İki sunucuyu DNS ile çoğaltma etki alanı denetleyicileri olarak yükseltme
* Sunucu yöneticisi aracılığıyla AD FS rolünü yükleyerek AD FS sunucularını yapılandırın.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. İç Yük Dengeleyici’yi (ILB) Dağıtma
**6.1. ILB oluşturma**

Bir ILB dağıtmak için Azure portalında Yük Dengeleyiciler’i seçin ve ekle (+) öğesine tıklayın.

> [!NOTE]
> Menünüzde **Yük Dengeleyiciler** seçeneğini görmüyorsanız portalın sol alt kısmındaki **Gözat**’a tıklayın ve **Yük Dengeleyiciler**’i görene kadar kaydırın.  Ardından sarı yıldıza tıklayarak menünüze ekleyin. Bundan sonra yeni yük dengeleyici simgesini seçerek paneli seçin ve yük dengeleyici yapılandırmasına başlayın.
> 
> 

![Yük dengeleyiciye göz atma](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Ad**: Yük dengeleyiciye uygun bir ad verin
* **Düzen**: Bu yük dengeleyici AD FS sunucularının önüne yerleştirileceğinden ve YALNIZCA dahili ağ bağlantılarına yönelik olduğundan "Dahili" seçeneğini belirleyin
* **Sanal Ağ**: AD FS sanal ağınızı dağıttığınız sanal ağı seçin
* **Alt Ağ**: Burada dahili alt ağı seçin
* **IP Adresi ataması**: Statik

![İç yük dengeleyici](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)

Oluştur’a tıklayıp ILB’yi dağıttıktan sonra yük dengeleyiciler listesinde ILB’yi görmeniz gerekir:

![ILB’den sonra yük dengeleyiciler](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)

Sonraki adım arka uç havuzunun ve arka uç araştırmasının yapılandırılmasıdır.

**6.2. ILB arka uç havuzunu yapılandırma**

Yeni oluşturulan ILB’yi Yük Dengeleyiciler panelinden seçin. Ayarlar paneli açılır. 

1. Ayarlar panelinden arka uç havuzlarını seçin
2. Arka uç havuzu ekleme panelinde sanal makine ekle seçeneğine tıklayın
3. Kullanılabilirlik kümesi seçebileceğiniz bir panel açılır
4. AD FS kullanılabilirlik kümesi seçme

![ILB arka uç havuzunu yapılandırma](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)

**6.3. Araştırmayı yapılandırma**

ILB ayarları panelinde Araştırmalar’ı seçin.

1. Ekle'ye tıklayın
2. Araştırmanın ayrıntılarını belirtin a. **Ad**: Araştırmanın adı b. **Protokol**: TCP c. **Bağlantı noktası**: 443 (HTTPS) d. **Aralık**: 5 (varsayılan değer) – ILB’nin arka uç havuzunda makineleri araştıracağı aralıktır. **Sağlıksız eşik sınırı**: 2 (varsayılan değer) – Aşıldığında ILB’nin arka uç havuzundaki bir makineyi duyarsız olarak duyuracağı ve trafik göndermeyi durduracağı ardışık araştırma hataları eşiğidir.

![ILB araştırmasını yapılandırma](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)

**6.4. Yük dengeleme kuralları oluşturma**

Trafiği etkin bir şekilde dengelemek için ILB’nin yük dengeleme kuralları ile yapılandırılması gerekir. Bir yük dengeleme kuralı oluşturmak için 

1. ILB ayarlar panelinden Yük dengeleme kuralını seçin
2. Yük dengeleme kuralı panelinde Ekle’ye tıklayın
3. Yük dengeleme kuralı ekleme panelinde a. **Ad**: Kural için bir ad belirtin b. **Protokol**: TCP seçin c. **Bağlantı noktası**: 443 d. **Arka uç bağlantı noktası**: 443 e. **Arka uç havuzu**: Daha önce AD FS kümesi için oluşturduğunuz havuzu seçin f. **Araştırma**: AD FS sunucuları için daha önce oluşturduğunuz araştırmayı seçin

![ILB dengeleme kurallarını yapılandırma](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. ILB ile DNS güncelleme**

DNS sunucunuza gidin ve ILB için bir CNAME oluşturun. CNAME, ILB’nin IP adresini işaret eden IP adresine sahip federasyon hizmetine yönelik olmalıdır. Örneğin, ILB DIP adresi 10.3.0.8 ve yüklü federasyon hizmeti fs.contoso.com ise 10.3.0.8’i işaret eden fs.contoso.com için bir CNAME oluşturun.
Bunun yapılması fs.contoso.com ile ilgili tüm iletişimlerin ILB’de sona ermesini ve uygun şekilde yönlendirilmesini sağlar.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Web Uygulaması Proxy sunucusunu yapılandırma
**7.1. Web Uygulaması Proxy sunucularını AD FS sunucularına ulaşacak şekilde yapılandırma**

Web Uygulaması Proxy sunucularının ILB’nin arkasında AD FS sunucularına ulaşabildiğinden emin olmak için %systemroot%\system32\drivers\etc\hosts dizininde ILB için bir kayıt oluşturun. Ayırt edici ad (DN) federasyon hizmetinin adı olmalıdır, örneğin fs.contoso.com. IP girişi ise ILB'nin IP adresi olmalıdır (örnekte olduğu gibi 10.3.0.8).

**7.2. Web Uygulaması Proxy rolünü yükleme**

Web Uygulaması Proxy sunucularının ILB’nin arkasındaki AD FS sunucularına ulaşabildiğinden emin olmak için sonraki adımda Web Uygulaması Proxy sunucularını yükleyebilirsiniz. Web Uygulaması Proxy sunucularının etki alanına katılması gerekmez. Uzaktan Erişim rolünü seçerek Web Uygulaması Proxy rollerini iki Web Uygulaması Proxy sunucusuna yükleyin. Sunucu yöneticisi WAP yüklemesini tamamlamak için size yol gösterecektir.
WAP dağıtımı hakkında daha fazla bilgi için [Web Uygulaması Proxy Sunucusunu Yükleme ve Yapılandırma](https://technet.microsoft.com/library/dn383662.aspx) makalesini okuyun.

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  İnternet’e Yönelik (Ortak) Yük Dengeleyiciyi dağıtma
**8.1.  İnternet’e Yönelik (Genel) Yük Dengeleyici oluşturma**

Azure portalında Yük dengeleyiciler’i seçin ve ardından Ekle’ye tıklayın. Yük dengeleyici oluşturma panelinde aşağıdaki bilgileri girin

1. **Ad**: Yük dengeleyicinin adı
2. **Düzen**: Genel – Bu seçenek Azure’a bu yük dengeleyicinin genel erişime açık olması gerektiğini söyler.
3. **IP Adresi**: Yeni bir IP adresi (dinamik) oluşturun

![İnternet'e yönelik yük dengeleyici](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Dağıtımdan sonra yük dengeleyici, Yük dengeleyiciler listesinde görünür.

![Yük dengeleyici listesi](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)

**8.2. Genel IP’ye bir DNS etiketi atama**

Paneli yapılandırma için açmak üzere Yük dengeleyiciler panelinde yeni oluşturulan yük dengeleyici girişine tıklayın. DNS etiketini genel IP için yapılandırmak üzere aşağıdaki adımları izleyin:

1. Genel IP adresine tıklayın. Genel IP paneli ve ayarları açılır
2. Yapılandırma’ya tıklayın
3. Bir DNS etiketi belirtin. Bu etiket contosofs.westus.cloudapp.azure.com gibi herhangi bir yerden erişebileceğiniz genel DNS etiketi olur. Federasyon hizmetinin dış DNS’ine (like fs.contoso.com), dış yük dengeleyicinin DNS etiketine (contosofs.westus.cloudapp.azure.com) çözümlenen bir giriş ekleyebilirsiniz.

![İnternet'e yönelik yük dengeleyiciyi yapılandırma](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![İnternet'e yönelik yük dengeleyiciyi yapılandırma (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. İnternet’e Yönelik (Genel) Yük Dengeleyici için arka uç havuzunu yapılandırma** 

İnternet’e Yönelik (Genel) Yük Dengeleyicinin arka uç havuzunu WAP sunucularının kullanılabilirlik kümesi olarak yapılandırmak için dahili yük dengeleyici oluşturma ile aynı adımları izleyin. Örneğin, contosowapset.

![İnternet’e Yönelik Yük Dengeleyicinin arka uç havuzunu yapılandırma](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)

**8.4. Araştırmayı yapılandırma**

WAP sunucularının arka uç havuzuna ait araştırmayı yapılandırmak için dahili yük dengeleyiciyi yapılandırma adımlarının aynısını izleyin.

![İnternet'e Yönelik Yük Dengeleyici araştırmasını yapılandırma](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)

**8.5. Yük dengeleme kuralları oluşturma**

TCP 443 yük dengeleme kuralını yapılandırmak için ILB’deki adımların aynısını izleyin.

![İnternet’e Yönelik Yük Dengeleyicinin dengeleme kurallarını yapılandırma](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Ağ güvenliğini sağlama
**9.1. Dahili alt ağ güvenliğini sağlama**

Genel olarak, dahili alt ağınızın güvenliğini verimli bir şekilde sağlamak için aşağıdaki kuralları uygulamanız gerekir (aşağıda listelenen sırayla)

| Kural | Açıklama | Akış |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |DMZ’den HTTPS iletişimine izin ver |Gelen |
| DenyInternetOutbound |İnternet erişimi yok |Giden |

![INT access rules (inbound)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

<!--
[comment]: <> (![INT access rules (inbound)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png))
[comment]: <> (![INT access rules (outbound)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
-->

**9.2. DMZ alt ağ güvenliğini sağlama**

| Kural | Açıklama | Akış |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |İnternet’ten DMZ’ye HTTPS’ye izin verir |Gelen |
| DenyInternetOutbound |HTTPS dışında İnternet’e giden her şey engellenir |Giden |

![EXT access rules (inbound)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

<!--
[comment]: <> (![EXT access rules (inbound)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png))
[comment]: <> (![EXT access rules (outbound)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))
-->

> [!NOTE]
> İstemci kullanıcı sertifikası kimlik doğrulaması (X509 kullanıcı sertifikaları kullanan clientTLS kimlik doğrulaması) gerekliyse AD FS, gelen erişim için TCP bağlantı noktası 49443’ün etkinleştirilmesini gerektirir.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. AD FS oturum açmayı test etme
AD FS’yi test etmenin en kolay yolu IdpInitiatedSignon.aspx sayfasının kullanılmasıdır. Bunu yapabilmek için AD FS özelliklerinde IdpInitiatedSignOn seçeneğinin etkinleştirilmesi gerekir. AD FS kurulumunuzu doğrulamak için aşağıdaki adımları izleyin

1. AD FS sunucusunda PowerShell ile aşağıdaki cmdlet’i çalıştırarak etkinleştirin.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Herhangi bir dış makineden https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx öğesine erişin  
3. AD FS sayfasını aşağıdaki gibi görmeniz gerekir:

![Oturum açma sayfasını test etme](./media/active-directory-aadconnect-azure-adfs/test1.png)

Oturum açma başarılı olduğunda aşağıdaki gibi bir başarı iletisi gösterilir:

![Test başarılı](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Azure'da AD FS dağıtma şablonu
Şablon, Etki Alanı Denetleyicileri, AD FS ve WAP'nin her biri için ikişer tane olmak üzere 6 makine kurulumu dağıtır.

[Azure Dağıtım Şablonu'nda AD FS](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Bu şablonu dağıtırken var olan bir sanal ağı kullanabilir veya yeni bir sanal ağ oluşturabilirsiniz. Dağıtımı özelleştirmek için kullanabileceğiniz çeşitli parametreler, parametrenin dağıtım işlemindeki kullanım tanımı ile birlikte aşağıda listelenmektedir. 

| Parametre | Açıklama |
|:--- |:--- |
| Konum |Kaynakların dağıtılacağı bölge, ör. Doğu ABD. |
| StorageAccountType |Oluşturulan Depolama Hesabının türü |
| VirtualNetworkUsage |Var olan sanal ağın kullanılıp kullanılmayacağını veya yeni bir sanal ağın oluşturulup oluşturulmayacağını belirtir |
| VirtualNetworkName |Oluşturulacak Sanal Ağın adı (hem var olan hem de yeni sanal ağ kullanımında zorunludur) |
| VirtualNetworkResourceGroupName |Var olan sanal ağın bulunduğu kaynak grubunun adını belirtir. Var olan bir sanal ağ kullanılırken bu, dağıtımın var olan sanal ağın kimliğini bulabilmesi için zorunlu bir parametre olur |
| VirtualNetworkAddressRange |Yeni sanal ağın adres aralığı (yeni bir sanal ağ oluşturuyorsanız zorunludur) |
| InternalSubnetName |İç alt ağın adı; her iki sanal ağ kullanım seçeneğinde de (yeni veya var olan) zorunludur |
| InternalSubnetAddressRange |Etki Alanı Denetleyicilerini ve AD FS sunucularını içeren iç alt ağın adres aralığı. (Yeni bir sanal ağ oluşturuyorsanız zorunludur). |
| DMZSubnetAddressRange |Windows uygulaması ara sunucularını içeren DMZ alt ağının adres aralığı, yeni bir sanal ağ oluşturuyorsanız zorunludur. |
| DMZSubnetName |İç alt ağın adı; her iki sanal ağ kullanım seçeneğinde de (yeni veya var olan) zorunludur. |
| ADDC01NICIPAddress |İlk Etki Alanı Denetleyicisinin iç IP adresi; bu IP adresi, statik olarak DC'ye atanır ve İç alt ağ içinde geçerli bir IP adresi olmalıdır |
| ADDC02NICIPAddress |İkinci Etki Alanı Denetleyicisinin iç IP adresi; bu IP adresi, statik olarak DC'ye atanır ve İç alt ağ içinde geçerli bir IP adresi olmalıdır |
| ADFS01NICIPAddress |İlk AD FS sunucusunun iç IP adresi; bu IP adresi, statik olarak AD FS sunucusuna atanır ve İç alt ağ içinde geçerli bir IP adresi olmalıdır |
| ADFS02NICIPAddress |İkinci AD FS sunucusunun iç IP adresi; bu IP adresi, statik olarak AD FS sunucusuna atanır ve İç alt ağ içinde geçerli bir IP adresi olmalıdır |
| WAP01NICIPAddress |İlk WAP sunucusunun iç IP adresi; bu IP adresi, statik olarak WAP sunucusuna atanır ve DMZ alt ağı içinde geçerli bir IP adresi olmalıdır |
| WAP02NICIPAddress |İkinci WAP sunucusunun iç IP adresi; bu IP adresi, statik olarak WAP sunucusuna atanır ve DMZ alt ağı içinde geçerli bir IP adresi olmalıdır |
| ADFSLoadBalancerPrivateIPAddress |AD FS yük dengeleyicisinin iç IP adresi; bu IP adresi, statik olarak yük dengeleyiciye atanır ve İç alt ağ içinde geçerli bir IP adresi olmalıdır |
| ADDCVMNamePrefix |Etki Alanı Denetleyicileri için Sanal Makine adı ön eki |
| ADFSVMNamePrefix |AD FS sunucuları için Sanal Makine adı ön eki |
| WAPVMNamePrefix |WAP sunucuları için Sanal Makine adı ön eki |
| ADDCVMSize |Etki Alanı Denetleyicilerinin sanal makine boyutu |
| ADFSVMSize |AD FS sunucularının sanal makine boyutu |
| WAPVMSize |WAP sunucularının sanal makine boyutu |
| AdminUserName |Sanal makinelerin yerel Yöneticisinin adı |
| AdminPassword |Sanal makinelerin yerel Yönetici hesabının parolası |

## <a name="additional-resources"></a>Ek kaynaklar
* [Kullanılabilirlik Kümeleri](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [İç yük dengeleyici](https://aka.ms/Azure/ILB/Internal)
* [İnternet'e Yönelik Yük Dengeleyici](https://aka.ms/Azure/ILB/Internet)
* [Depolama hesapları](https://aka.ms/Azure/Storage)
* [Azure Sanal Ağları](https://aka.ms/Azure/VNet)
* [AD FS ve Web Uygulaması Proxy Bağlantıları](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Sonraki adımlar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
* [Azure AD Connect kullanarak AD FS’yi yapılandırma ve yönetme](active-directory-aadconnectfed-whatis.md)
* [Azure Traffic Manager ile Azure’da yüksek kullanılabilirliğe sahip çapraz coğrafi AD FS dağıtımı](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

