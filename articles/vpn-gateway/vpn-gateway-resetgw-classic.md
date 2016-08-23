<properties
   pageTitle="Azure VPN Gateway’i Sıfırlama | Microsoft Azure"
   description="Bu makalede Azure VPN Gateway’i sıfırlama adımları anlatılmaktadır. Makale klasik dağıtım modeli kullanılarak oluşturulmuş VPN ağ geçitleri için geçerlidir."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="cherylmc"/>

# PowerShell kullanarak bir Azure VPN Gateway sıfırlama


Bu makalede PowerShell cmdlet'lerini kullanarak Azure VPN Gateway’i sıfırlama işleminde size yol gösterir. Bu yönergeler klasik dağıtım modeli için geçerlidir.  Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir VPN ağ geçidini veya sanal ağları sıfırlamak için henüz bir cmdlet veya REST API’si mevcut değildir. Bunlar devam etmektedir. Azure Portal’da sanal ağınızı görüntüleyerek VPN ağ geçidinizin klasik dağıtım modeli kullanılarak oluşturulup oluşturulmadığını anlayabilirsiniz. Klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar Azure Portal’ın Sanal Ağ (klasik) bölümünde gösterilir.

Bir veya daha fazla S2S VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. *Reset-AzureVNetGateway* cmdlet’ini kullandığınızda ağ geçidi yeniden başlatılır ve ardından şirketler arası yapılandırmalar ağ geçidine yeniden uygulanır. Ağ geçidi zaten sahip olduğu ortak IP adresini tutar. Bu durum VPN yönlendirici yapılandırmasını Azure VPN ağ geçidi için yeni bir ortak IP adresi ile güncelleştirmenizin gerekli olmadığı anlamına gelir.  


Ağ geçidinizi sıfırlamadan aşağıda her bir IPSec siteden siteye (S2S) VPN tüneli için listelenen anahtar öğeleri doğrulayın. Öğelerdeki uyuşmazlık S2S VPN tünellerinin bağlantısının kesilmesiyle sonuçlanır. Şirket içi ve Azure VPN ağ geçitleriniz için yapılandırmaların doğrulanması ve düzeltilmesi sizi ağ geçitleri üzerinde çalışan diğer bağlantılar için gereksiz yeniden başlatmalardan ve kesintilerden korur.

Ağ geçidinizi sıfırlamadan önce aşağıdaki öğeleri doğrulayın.

- Hem Azure VPN ağ geçidi hem de şirket içi VPN ağ geçidi için İnternet IP adresleri (VIP) hem Azure hem de şirket içi VPN ilkelerinde doğru şekilde yapılandırılmıştır.
- Önceden paylaşılan anahtar Azure ve şirket içi VPN ağ geçitlerinde aynı olmalıdır.
- Şifreleme, karma algoritmaları ve PSF (Kusursuz İletme Gizliliği) gibi belirli IPsec/IKE yapılandırmalarını uygularsanız hem Azure hem de şirket içi VPN ağ geçitlerinin aynı yapılandırmalara sahip olduğundan emin olun.


## PowerShell kullanarak VPN Gateway sıfırlama

Azure VPN ağ geçidini sıfırlamaya yönelik PowerShell cmdlet’i *Reset-AzureVNetGateway* ’dir. Her bir Azure VPN ağ geçidi etkin bekleme yapılandırmasında çalışan iki VM örneğinden oluşur. Komut yayınlandıktan sonra Azure VPN ağ geçidinin o anda etkin olan örneği hemen yeniden başlatılır. Etkin örnekten (yeniden başlatılan) bekleme örneğine yük devretme sırasında kısa bir boşluk oluşur. Boşluk bir dakikadan az olmalıdır. 

Aşağıdaki örnekte "ContosoVNet" adlı sanal ağ için Azure VPN ağ geçidi sıfırlanmaktadır.
 
        Reset-AzureVNetGateway –VnetName “ContosoVNet” 

        Error          :
        HttpStatusCode : OK
        Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
        Status         : Successful
        RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
        StatusCode     : OK


İlk yeniden başlatma sonrasında bağlantı geri kazanılmazsa ikinci sanal makine örneğini (yeni etkin ağ geçidi) yeniden başlatmak için aynı komutu yeniden verin. Arka arkaya iki yeniden başlatma istenirse her iki sanal makine örneğinin (etkin ve bekleme) yeniden başlatılma süresi biraz daha uzun olur. Bu durum VPN bağlantısı üzerinde sanal makinelerin yeniden başlatmaları tamamlaması için 2 ila 4 dakikaya varan daha uzun bir boşluğa neden olur.

İki yeniden başlatma sonrasında hala şirketler arası bağlantı sorunları yaşıyorsanız lütfen Klasik Azure Portalı’ndan bir destek talebi açarak Microsoft Azure Desteği ile iletişim kurun.

## Sonraki adımlar
    
 Bu cmdlet hakkında daha fazla bilgi için bkz. [PowerShell başvurusu](https://msdn.microsoft.com/library/azure/mt270366.aspx).









<!--HONumber=Aug16_HO1-->


