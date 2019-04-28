---
title: Bir Internet'e yönelik Yük Dengeleyici IPv6 - Azure şablonu ile dağıtma
titlesuffix: Azure Load Balancer
description: Azure Load Balancer ve yük dengeli VM'ler için IPv6 desteği dağıtma
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6, azure yük dengeleyici, ikili yığın, genel IP, yerel IPv6, mobil veya IOT
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 4a8c7309a07238ef3410e42c3d631ad525f023cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61216853"
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Şablon kullanarak bir Internet'e yönelik Yük Dengeleyici çözümü IPv6 ile dağıtma

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)



Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Yük Dengeleme çözümü Aşağıdaki diyagramda gösterilmektedir bu makalede açıklanan örnek şablonu kullanarak dağıtılıyor.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Bu senaryoda aşağıdaki Azure kaynakları oluşturacak:

* Atanmış IPv4 ve IPv6 adresleri ile her VM için bir sanal ağ arabirimi
* bir Internet'e yönelik Yük Dengeleyiciyle bir IPv4 ve IPv6 genel IP adresi
* İki Yük Dengeleme kuralları, genel VIP özel uç noktalar için eşlemek için
* iki sanal makine içeren kullanılabilirlik kümesi
* İki sanal makine (VM)

## <a name="deploying-the-template-using-the-azure-portal"></a>Azure portalını kullanarak şablonu dağıtma

Bu makalede, yayımlanmış bir şablonu başvuran [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) Galerisi. Galeri şablonunu indirin veya doğrudan galerisinden azure'da dağıtımı başlatın. Bu makalede, şablonu yerel bilgisayarınıza yüklediğiniz varsayılır.

1. Azure portalı ve Vm'leri ve bir Azure aboneliğinde ağ kaynakları oluşturma izni olan bir hesapla oturum açın. Ayrıca, mevcut kaynaklar kullanmakta olduğunuz sürece, hesabın bir kaynak grubu ve bir depolama hesabı oluşturma izni gerekir.
2. Tıklayın "+ Yeni" öğesinden menü sonra arama kutusuna "Şablon" türü. "Şablon dağıtımı" araması sonuçlarından seçin.

    ![lb-ipv6-portal-step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Her şey dikey penceresinde "Şablon dağıtımı" tıklayın

    ![lb-ipv6-portal-step3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. "Oluştur" a tıklayın

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. "Şablonu Düzenle" yi tıklatın. Varolan içeriğini silin ve tüm içeriğini (başlangıç ve bitiş {}) olan şablon dosyası içinde kopyala/yapıştır sonra "." Kaydet

    > [!NOTE]
    > Microsoft Internet Explorer kullanıyorsanız, yapıştırdığınızda Windows Pano erişmesine izin vermek isteyen bir iletişim kutusu görüntülenir. "Erişime izin ver" tıklayın

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. "Parametreleri Düzenle" yi tıklatın. Parametreler dikey penceresinde şablon parametreleri bölümünde yönergeler uyarınca değerleri belirtin ve parametreleri dikey penceresini kapatmak için farklı "Kaydet"'i tıklatın. Özel dağıtım dikey penceresinde, aboneliğinizi, mevcut bir kaynak grubunu seçin veya oluşturun. Bir kaynak grubu oluşturuyorsanız, ardından kaynak grubu için bir konum seçin. Ardından, **yasal koşulları**, ardından **satın alma** için yasal koşullar. Azure kaynak dağıtmaya başlar. Tüm kaynakları dağıtmak için birkaç dakika sürer.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Bu parametreler hakkında daha fazla bilgi için bkz. [şablon parametreleri ve değişkenleri](#template-parameters-and-variables) bu makalenin devamındaki bölümü.

7. Şablon tarafından oluşturulan kaynakları görmek için Gözat'a tıklayın, "Kaynak grupları" konusuna bakın ve ardından tıklayarak kadar listede aşağı kaydırın.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Kaynak grupları dikey penceresinde, 6. adımda belirttiğiniz kaynak grubu adına tıklayın. Dağıtılan tüm kaynakların bir listesini görürsünüz. Tüm iyi geçti, "Son dağıtım altında" "Başarılı" olmalıdır Aksi takdirde, kullandığınız hesap, gerekli kaynakları oluşturma izni olduğundan emin olun.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > 6. adım tamamlandıktan hemen sonra kaynak gruplarınızın göz atarsanız, "Son dağıtım" kaynakları dağıtılırken "Dağıtma" durumunu görüntüler.

9. Kaynak listesinden "myIPv6PublicIP"'a tıklayın. Bir IPv6 adresi IP adresi altında olduğunu ve DNS adını 6. adımda dnsNameforIPv6LbIP parametresi için belirtilen değer olduğunu görürsünüz. Internet istemcileri için erişilebilir genel IPv6 adresi ve ana bilgisayar adı kaynaktır.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Şablon başarıyla dağıtıldığını, aşağıdaki görevleri tamamlayarak bağlantı doğrulayabilirsiniz:

1. Azure portalında oturum açın ve her şablon dağıtımı tarafından oluşturulan VM'lerin bağlanın. İpconfig'i çalıştırın bir Windows Server VM dağıttıysanız/bir komut isteminden tüm. VM'ler IPv4 ve IPv6 adresleri olduğunu görürsünüz. Linux Vm'leri dağıttıysanız, Linux dağıtımınız için sağlanan yönergeleri kullanarak dinamik IPv6 adreslerini almak için Linux işletim sistemi yapılandırmanız gerekir.
2. IPv6 İnternet'e bağlı bir istemciden yük dengeleyici genel IPv6 adresi için bir bağlantı başlatır. Yük dengeleyicinin iki VM arasında dengeleme olduğunu doğrulamak için VM'lerin her bir web sunucusu Microsoft Internet Information Services (IIS) gibi yükleyebilirsiniz. Varsayılan web sayfasını her sunucuda "Server0" veya "SUNUCU1" benzersiz şekilde tanımlamak için metin içerebilir. Ardından, IPv6 İnternet'e bağlı bir istemci bir Internet tarayıcısı açın ve ana bilgisayar adına göz her VM için uçtan uca IPv6 bağlantısı onaylamak için yük dengeleyicinin dnsNameforIPv6LbIP parametresi için belirtilen. Yalnızca tek bir sunucu web sayfasını görürseniz, tarayıcı önbelleğini temizlemeniz gerekebilir. Birden çok özel tarama oturumu açın. Her sunucusundan bir yanıt görmeniz gerekir.
3. IPv4 İnternet'e bağlı bir istemciden, yük dengeleyicinin genel IPv4 adresi için bir bağlantı başlatır. Yük Dengeleyici, Yük Dengeleme iki sanal makine olduğundan emin olmak için 2. adım ayrıntılı olarak IIS, kullanarak test edebilirsiniz.
4. Her bir sanal makineden bir IPv6 ya da IPv4 bağlı Internet cihaz giden bir bağlantı başlatır. Her iki durumda da, hedef cihaz tarafından görülen kaynak IP yük dengeleyicinin genel IPv4 veya IPv6 adresi ' dir.

> [!NOTE]
> IPv4 ve IPv6 için ICMP, Azure ağında engellenir. Sonuç olarak, her zaman ping gibi ICMP Araçlar başarısız. Bağlantıyı test etmek için Telnet veya PowerShell Test-NetConnection cmdlet'i gibi bir TCP alternatif kullanın. Aşağıdaki diyagramda gösterilen IP adreslerini görebileceğiniz değer örnekleri olduğuna dikkat edin. IPv6 adresleri dinamik olarak atanmış olduğundan, aldığınız adresleri farklılık gösterir ve bölgeye göre farklılık gösterebilir. Ayrıca, yük dengeleyicinin arka uç havuzundaki özel IPv6 adresleri değerinden farklı bir önek ile başlayan üzerinde genel IPv6 adresi yaygındır.

## <a name="template-parameters-and-variables"></a>Şablon parametreleri ve değişkenleri

Bir Azure Resource Manager şablonu, birden çok değişkenleri ve ihtiyaçlarınıza özelleştirebilirsiniz parametreleri içerir. Değişkenleri değiştirmek için bir kullanıcı istemediğiniz sabit değerleri için kullanılır. Parametreler için değerleri şablonu dağıtırken bir kullanıcıdan istediğiniz kullanılır. Örnek şablonu, bu makalede açıklanan senaryo için yapılandırılır. Bu ortamınızın ihtiyaçlarına göre özelleştirebilirsiniz.

Bu makalede kullanılan örnek şablonu, aşağıdaki değişkenleri ve parametreleri içerir:

| Parametre / değişken | Notlar |
| --- | --- |
| adminUsername |Sanal makineler ile oturum açmak için kullanılan yönetici hesabının adını belirtin. |
| adminPassword |Sanal makineler ile oturum açmak için kullanılan yönetici hesabının parolasını belirtin. |
| dnsNameforIPv4LbIP |Yük dengeleyicinin genel adı atamak istediğiniz DNS ana bilgisayar adı belirtin. Bu ad, load balancer'ın genel IPv4 adresine çözümler. Ad küçük harfli olması ve regex eşleşmesi gerekir: ^ [a-z] [bir-z0 - 9-]{1,61}[a-z0-9] $. |
| dnsNameforIPv6LbIP |Yük dengeleyicinin genel adı atamak istediğiniz DNS ana bilgisayar adı belirtin. Bu ad, load balancer'ın genel IPv6 adresi çözümler. Ad küçük harfli olması ve regex eşleşmesi gerekir: ^ [a-z] [bir-z0 - 9-]{1,61}[a-z0-9] $. Bu, aynı adı taşıyan bir IPv4 adresi olabilir. Bir istemci Azure döndürür, bu adı için bir DNS sorgusu gönderdiğinde A ve AAAA kaydeder olduğunda ad paylaşılır. |
| vmNamePrefix |VM adı öneki belirtin. Şablonu bir sayı ekler (0, 1, vb.) Vm'leri oluşturulduğunda adı. |
| nicNamePrefix |Ağ arabirimi adı öneki belirtin. Şablonu bir sayı ekler (0, 1, vb.) ağ arabirimi oluşturulurken adı. |
| storageAccountName |Mevcut bir depolama hesabı adını girin veya yeni bir şablon tarafından oluşturulacak adını belirtin. |
| availabilitySetName |Ardından kullanılabilirlik kümesindeki VM'ler ile kullanılacak adını girin |
| addressPrefix |Sanal ağ adres aralığı tanımlamak için kullanılan adres ön eki |
| subnetName |Sanal ağ için alt ağda adı oluşturuldu |
| subnetPrefix |Alt ağ adres aralığı tanımlamak için kullanılan adres ön eki |
| vnetName |VM'ler tarafından kullanılan sanal ağ için bir ad belirtin. |
| ipv4PrivateIPAddressType |Ayırma yöntemini için özel IP adresi (statik veya dinamik) kullanılır. |
| ipv6PrivateIPAddressType |Ayırma yöntemini için özel IP adresi (dinamik) kullanılır. IPv6, yalnızca dinamik ayırma destekler. |
| numberOfInstances |Şablon tarafından dağıtılmasını yük dengeli örnek sayısı |
| ipv4PublicIPAddressName |Yük Dengeleyici genel IPv4 adresi ile iletişim kurması için kullanmak istediğiniz DNS adını belirtin. |
| ipv4PublicIPAddressType |Genel IP adresi (statik veya dinamik) kullanılan ayırma yöntemi |
| Ipv6PublicIPAddressName |Yük Dengeleyici genel IPv6 adresi ile iletişim kurması için kullanmak istediğiniz DNS adını belirtin. |
| ipv6PublicIPAddressType |Ayırma yöntemini genel IP adresi (dinamik) için kullanılır. IPv6, yalnızca dinamik ayırma destekler. |
| lbname adlı |Yük Dengeleyici adını belirtin. Bu ad portalında görüntülenen veya CLI veya PowerShell komutuyla başvururken kullanılır. |

Şablondaki kalan değişkenleri Azure kaynakları oluştururken, atanan türetilmiş değerleri içerir. Bu değişkenler değiştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

JSON söz dizimi ve bir şablon bir yük dengeleyici özelliklerini bkz [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers).
