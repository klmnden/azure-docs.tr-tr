<properties
   pageTitle="Bir şirketin İnternet etki alanını Traffic Manager etki alanına yönlendirme | Microsoft Azure"
   description="Bu makale, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirmenize yardımcı olacaktır."
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Bir şirketin İnternet etki alanını Azure Traffic Manager etki alanına yönlendirme

Şirketinizin etki alanı adını bir Traffic Manager etki alanına yönlendirmek için İnternet DNS sunucunuzdaki DNS kaynak kaydını değiştirerek şirketinizin etki alanı adını Traffic Manager profilinizin etki alanı adına eşleyecek CNAME kaynak türünü kullanın. Traffic Manager profilinin Yapılandırma sayfasındaki **Genel** bölümünde Traffic Manager etki alanı adını görebilirsiniz.

Örneğin, www.contoso.com şirket etki alanı adını Traffic Manager etki alanı adı olan contoso.trafficmanager.net'e yönlendirmek için DNS kaynağı kaydınızı aşağıdaki şekilde güncelleştirmeniz gerekir:

    www.contoso.com IN CNAME contoso.trafficmanager.net

*www.contoso.com* adresi için yapılan tüm trafik istekleri artık *contoso.trafficmanager.net* adresine yönlendirilir.

>[AZURE.IMPORTANT] *contoso.com* gibi ikinci düzey bir etki alanını Traffic Manager etki alanına yönlendiremezsiniz. Bu, ikinci düzey etki alanı adları için CNAME kayıtlarına izin vermeyen DNS protokolünün bir sınırlamasıdır.

## Sonraki adımlar

[Traffic Manager yönlendirme yöntemleri](traffic-manager-routing-methods.md)

[Traffic Manager - Bir profili devre dışı bırakma, etkinleştirme veya silme](disable-enable-or-delete-a-profile.md)

[Traffic Manager - Bir uç noktayı devre dışı bırakma veya etkinleştirme](disable-or-enable-an-endpoint.md)



<!----HONumber=Jun16_HO2-->


