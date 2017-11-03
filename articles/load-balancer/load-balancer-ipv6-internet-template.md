---
title: "Bir Internet'e yönelik Yük Dengeleyici IPv6 - Azure şablonu ile dağıtma | Microsoft Docs"
description: "IPv6 desteği Azure yük dengeleyici ve yük dengeli sanal makineleri dağıtmak nasıl."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
keywords: "IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT"
ms.assetid: 2998e943-13fc-4ea9-a68c-875e53a08db3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 90439d792eac618671a9de9938302d8930c986d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Bir şablonu kullanarak bir Internet'e yönelik Yük Dengeleyici çözümü IPv6 ile dağıtma

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram yük dengeleme çözümü gösterir bu makalede açıklanan örnek şablon kullanılarak dağıtılan.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Bu senaryoda aşağıdaki Azure kaynakları oluşturulur:

* atanan IPv4 ve IPv6 adreslerinin her VM için bir sanal ağ arabirimi
* bir IPv4 ve IPv6 genel IP adresi olan bir Internet'e yönelik Yük Dengeleyici
* iki Yük Dengeleme kuralları özel uç noktaları için ortak VIP'ler eşlemek için
* iki VM içeren bir kullanılabilirlik kümesi
* iki sanal makine (VM)

## <a name="deploying-the-template-using-the-azure-portal"></a>Azure portalını kullanarak şablonu dağıtma

Bu makalede, yayımlanan bir şablon başvuran [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) Galerisi. Galeriden şablonunu indirebilir ya da Azure galerisinden doğrudan dağıtımda Başlat. Bu makalede, yerel bilgisayarınıza şablon yüklediğiniz varsayılır.

1. Azure portalı ve VM'ler ve ağ kaynakları içinde bir Azure aboneliği oluşturmak için izinlere sahip bir hesapla oturum açın. Ayrıca, var olan kaynakların kullandığınız sürece, hesap bir kaynak grubu ve bir depolama hesabı oluşturmak için izniniz gerekiyor.
2. Tıklayın "+ Yeni" menü silip arama kutusuna "Şablon" türü. "Şablon dağıtımı" Arama sonuçlarından seçin.

    ![IPv6 portal Adım2 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Her şeyi içinde dikey penceresinde "Şablon dağıtımı"'i tıklatın

    ![lb-IPv6-portal-adım 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. "Oluştur" seçeneğini tıklatın

    ![IPv6 portal step4 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. "Şablonu Düzenle" seçeneğini tıklatın. Varolan içeriğini silin ve tüm içeriğini (başlangıç ve bitiş {}) olan şablon dosyası kopyala/yapıştır sonra "." Kaydet

    > [!NOTE]
    > Microsoft Internet Explorer kullanıyorsanız, Yapıştır açtığınızda Windows panosuna erişmesine izin vermek isteyen bir iletişim kutusu görüntülenir. "Erişime izin ver."'i tıklatın

    ![IPv6 portal step5 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. "Parametreleri Düzenle" seçeneğini tıklatın. Parametreler dikey şablonu parametreleri bölümünde rehberliğinde değerleri belirtin ve parametre dikey penceresini kapatmak için farklı "Kaydet"'i tıklatın. Özel dağıtım dikey penceresinde, aboneliğiniz, varolan bir kaynak grubu seçin veya oluşturun. Bir kaynak grubu oluşturuyorsanız, kaynak grubu için bir konum seçin. Bundan sonra öğesini **yasal koşulları**, ardından **satın alma** yasal koşulları için. Azure kaynakları dağıtmaya başlar. Tüm kaynakları dağıtmak için birkaç dakika sürer.

    ![IPv6 portal step6 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Bu parametreler hakkında daha fazla bilgi için bkz: [şablon parametreleri ve değişkenleri](#template-parameters-and-variables) bu makalenin sonraki bölümlerinde bölümü.

7. Şablon tarafından oluşturulan kaynakları görmek için Gözat'ı tıklatın, "Kaynak grupları" konusuna bakın ve ardından tıklatın kadar listede aşağı kaydırın.

    ![IPv6 portal step7 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Kaynak grupları dikey penceresinde, 6. adımda belirttiğiniz kaynak grubu adını tıklatın. Dağıtılan tüm kaynakların bir listesini görürsünüz. Tüm iyi olursa, "Başarılı" özelliğini "Son dağıtım altında" yazması gerekir Aksi takdirde, kullandığınız hesabın gerekli kaynaklar oluşturma izni olduğundan emin olun.

    ![IPv6 portal step8 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > 6. adım tamamlandıktan hemen sonra kaynak grupları göz atarsanız, "Son dağıtım" kaynakları dağıtılırken "Dağıtma" durumunu görüntüler.

9. Kaynak listesinde "myIPv6PublicIP"'i tıklatın. Bir IPv6 adresi IP adresi altında olduğunu ve DNS adını adım 6 dnsNameforIPv6LbIP parametresi için belirtilen değer olduğunu görürsünüz. Internet istemcileri için erişilebilir olan genel IPv6 adresi ve ana bilgisayar adı kaynaktır.

    ![IPv6 portal step9 lb](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Bağlantı doğrula

Şablonu başarıyla dağıtıldığını, aşağıdaki görevleri tamamlayarak bağlantısını doğrulayabilirsiniz:

1. Azure portalında oturum açın ve her bir şablon dağıtımı tarafından oluşturulan VM'ler bağlanın. İpconfig komutunu çalıştırmak bir Windows Server VM dağıttıysanız/bir komut isteminden tüm. Sanal makineleri hem IPv4 hem de IPv6 adresleri olduğunu görürsünüz. Linux VM'ler dağıttıysanız, Linux işletim sistemi Linux dağıtımınız için sağlanan yönergeleri kullanarak dinamik IPv6 adreslerini almak için yapılandırmanız gerekir.
2. IPv6 Internet'e bağlı bir istemciden yük dengeleyicinin genel IPv6 adresi için bir bağlantı başlatır. Yük Dengeleyici arasında iki VM Dengeleme olduğunu onaylamak için VM'lerin her birinde bir web sunucusu Microsoft Internet Information Services (IIS) gibi yükleyebilir. Her bir sunucuda varsayılan web sayfası "Server0" veya "SUNUCU1" benzersiz şekilde tanımlamak için metin içerebilir. Ardından, bir IPv6 Internet'e bağlı istemci bir Internet tarayıcısında açın ve ana bilgisayar için göz uçtan uca IPv6 bağlantısı her VM onaylamak için yük dengeleyicinin dnsNameforIPv6LbIP parametresi için belirtilen. Yalnızca tek bir sunucudan web sayfasını görürseniz, tarayıcı önbelleğiniz temizlemeniz gerekebilir. Birden çok özel tarayıcı oturumu açın. Her sunucusundan bir yanıt görmeniz gerekir.
3. IPv4 Internet'e bağlı bir istemciden yük dengeleyicinin genel IPv4 adresi için bir bağlantı başlatır. Yük Dengeleyici Yük Dengeleme iki VM olduğunu onaylamak için 2. adım ayrıntılı olarak IIS, kullanarak test edebilirsiniz.
4. Her sanal makineden bir IPv6 ya da IPv4 bağlı Internet aygıtına giden bir bağlantı başlatır. Her iki durumda da, hedef aygıt tarafından görülen kaynak IP yük dengeleyici genel IPv4 veya IPv6 adresi değil.

> [!NOTE]
> IPv4 ve IPv6 için ICMP Azure ağında engellendi. Sonuç olarak, her zaman ping gibi ICMP Araçlar başarısız. Bağlantıyı sınamak için bir TCP alternatif TCPing veya PowerShell Test-NetConnection cmdlet'i gibi kullanın. Diyagramda gösterildiği IP adreslerini görebilirsiniz değer örnekleri olduğuna dikkat edin. IPv6 adresleri dinamik olarak atanmış olduğundan, aldığınız adresleri farklılık gösterir ve bölgeye göre farklılık gösterebilir. Ayrıca, yük dengeleyici arka uç havuzundaki özel IPv6 adresleri daha farklı bir önek ile başlayan genel IPv6 adresi yaygındır.

## <a name="template-parameters-and-variables"></a>Şablon parametreleri ve değişkenleri

Bir Azure Resource Manager şablonu birden fazla değişkenleri ve ihtiyaçlarınızı özelleştirebilirsiniz parametreleri içerir. Değişkenleri değiştirmek için bir kullanıcı istemediğiniz sabit değerler için kullanılır. Parametreler için değerler şablonu dağıtırken sağlamak için kullanıcının istediğiniz kullanılır. Örnek şablon bu makalede açıklanan senaryo için yapılandırılır. Bu ortamınızın gereksinimlerini için özelleştirebilirsiniz.

Bu makalede kullanılan örnek şablonu aşağıdaki değişkenleri ve parametreleri içerir:

| Parametre / değişken | Notlar |
| --- | --- |
| adminUsername |Sanal makineler ile oturum açmak için kullandığınız yönetici hesabının adını belirtin. |
| Admınpassword |Sanal makineler ile oturum açmak için kullandığınız yönetici hesabının parolasını belirtin. |
| dnsNameforIPv4LbIP |Yük dengeleyicinin genel adı atamak istediğiniz DNS ana bilgisayar adı belirtin. Bu ad yük dengeleyicinin genel IPv4 adresine çözümler. Adı küçük harfli olması ve regex eşleşmesi gerekir: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Yük dengeleyicinin genel adı atamak istediğiniz DNS ana bilgisayar adı belirtin. Bu ad yük dengeleyicinin genel IPv6 adresine çözümler. Adı küçük harfli olması ve regex eşleşmesi gerekir: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Bu bir IPv4 adresi adıyla aynı olabilir. Bir istemci Azure döndürür, bu adı için bir DNS sorgusu gönderdiğinde A ve AAAA kaydeder adı zaman paylaşılır. |
| vmNamePrefix |VM adı öneki belirtin. Şablonu bir numara ekler (0, 1, vb.) VM'ler oluşturulduğunda adı. |
| nicNamePrefix |Ağ arabirimi adı öneki belirtin. Şablonu bir numara ekler (0, 1, vb.) ağ arabirimleri oluşturulduğunda adı. |
| storageAccountName |Varolan bir depolama hesabı adı girin veya yeni bir şablon tarafından oluşturulacak adını belirtin. |
| availabilitySetName |VM ile birlikte kullanılmak üzere ayarlanmış kullanılabilirlik adını girin |
| addressPrefix |Sanal ağ adres aralığı tanımlamak için kullanılan adres ön eki |
| subnetName |VNet için alt ağda adı oluşturuldu |
| subnetPrefix |Alt ağ adres aralığı tanımlamak için kullanılan adres ön eki |
| vnetName |VM'ler tarafından kullanılan sanal ağ adını belirtin. |
| ipv4PrivateIPAddressType |Özel IP adresini (statik veya dinamik) kullanılan ayırma yöntemi |
| ipv6PrivateIPAddressType |Ayırma yöntemi için özel IP adresi (dinamik) kullanılır. IPv6, yalnızca dinamik ayırma destekler. |
| numberOfInstances |Şablon tarafından dağıtılan yük dengeli örnek sayısı |
| ipv4PublicIPAddressName |Yük Dengeleyici genel IPv4 adresi ile iletişim kurmak için kullanmak istediğiniz DNS adını belirtin. |
| ipv4PublicIPAddressType |Ortak IP adresini (statik veya dinamik) kullanılan ayırma yöntemi |
| Ipv6PublicIPAddressName |Yük Dengeleyici genel IPv6 adresi ile iletişim kurmak için kullanmak istediğiniz DNS adını belirtin. |
| ipv6PublicIPAddressType |Genel IP adresi (dinamik) için kullanılan ayırma yöntemi. IPv6, yalnızca dinamik ayırma destekler. |
| lbName |Yük Dengeleyici adını belirtin. Bu ad portalında gösterilen veya CLI veya PowerShell komutuyla başvururken kullanılır. |

Şablondaki kalan değişkenleri Azure kaynakları oluşturduğunda, atanmış olan türetilmiş değerlerini içerir. Bu değişkenleri değiştirmeyin.
