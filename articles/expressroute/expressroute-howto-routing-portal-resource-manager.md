<properties
   pageTitle="Azure Portalını kullanarak ExpressRoute bağlantı hattı için yönlendirmeyi yapılandırma | Microsoft Azure"
   description="Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="cherylmc"/>

# Bir ExpressRoute bağlantı hattı için yönlendirmeyi oluşturma ve değiştirme



> [AZURE.SELECTOR]
[Azure Portalı - Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell - Klasik](expressroute-howto-routing-classic.md)



Bu makalede, bir ExpressRoute bağlantı hattı için Azure portalı ve Resource Manager dağıtım modeli kullanarak yönlendirme yapılandırması oluşturma ve yönetme için adım adım yönergeler sağlanmıştır.

**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Yapılandırma önkoşulları

- Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
- Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Aşağıda açıklanan cmdlet’leri çalıştırmanız için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen Katman 3 hizmetleri (genellikle MPLS gibi bir IPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir. 


>[AZURE.IMPORTANT] Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşlemelerini yapılandırmadan önce lütfen hizmet sağlayıcınıza başvurun.

Bir ExpressRoute bağlantı hattı için bir, iki veya üç eşlemenin tamamını (Azure özel, Azure ortak ve Microsoft) yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. 

## Azure özel eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

    - Birincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
    - İkincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
    - Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
    - Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515’i kullanmadığınızdan emin olun.
    - Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır**.


3. Aşağıda gösterildiği gibi Azure Private eşleme satırını seçin.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
    

4. Özel eşleme oluşturun. Aşağıdaki resimde bir yapılandırma örneği gösterilir.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

    
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey göreceksiniz.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)
    

### Azure özel eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure özel eşleme özelliklerini görüntüleyebilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)


### Azure özel eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. 

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### Azure özel eşlemeyi silmek için

Aşağıda gösterilen silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)


## Azure ortak eşleme

Bu bölümde bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### Azure ortak eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Daha ileriye devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

    - Birincil bağlantı için bir /30 alt ağı. 
    - İkincil bağlantı için bir /30 alt ağı. 
    - Bu eşlemeyi kurman için kullanılan tüm IP adresleri geçerli ortak IPv4 adresleri olmalıdır.
    - Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
    - Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
    - Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır**.

3. Aşağıda gösterildiği gibi Azure public eşleme satırını seçin.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
    

4. Ortak eşlemeyi yapılandırın. Aşağıdaki resimde bir yapılandırma örneği gösterilir.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

    
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey göreceksiniz.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)
    

### Azure ortak eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure ortak eşleme özelliklerini görüntüleyebilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)


### Azure ortak eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. 

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### Azure ortak eşlemesini silmek için

Aşağıda gösterilen silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)


## Microsoft eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Daha ileriye devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

    - Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
    - İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
    - Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
    - Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
    - **Tanıtılan ön ekler: ** BGP oturumunda tanıtmayı planladığınız tüm ön eklerin listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
    - **Müşteri ASN’si:** Eşleme AS numarasına kayıtlı olmayan ön ekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz. **Bu isteğe bağlıdır**.
    - **Yönlendirme Kayıt Defteri Adı:** AS numarası ve ön eklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz. **Bu isteğe bağlıdır.**
    - Kullanmayı seçerseniz bir MD5 karma değeri. **Bu isteğe bağlıdır.**
    
3. Aşağıda gösterildiği gibi yapılandırmak istediğiniz eşlemeyi seçebilirsiniz. Microsoft eşleme satırını seçin.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
    

4.  Microsoft eşlemesini yapılandırın. Aşağıdaki resimde bir yapılandırma örneği gösterilir.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)

    
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin. 

    Bağlantı hattınız bir doğrulama gerekli durumuna geçerse, ön eklerin sahipliğine ait kanıtı destek ekibimize göstermek için bir destek bileti açmanız gerekir.  
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)


    Aşağıda gösterildiği gibi destek biletini doğrudan portal üzerinden açabilirsiniz   
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


6. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey göreceksiniz.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)
    

### Microsoft eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure ortak eşleme özelliklerini görüntüleyebilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)


### Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. 


![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)


### Microsoft eşlemesini silmek için

Aşağıda gösterilen silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)



## Sonraki adımlar

Sonraki adım, [ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-arm.md).

-  ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).

-  Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).

-  Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).




<!--HONumber=Aug16_HO1-->


