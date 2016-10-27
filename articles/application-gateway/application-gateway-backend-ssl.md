<properties
   pageTitle="Application Gateway’de SSL İlkesi ve uçtan uca SSL’yı etkinleştirme | Microsoft Azure"
   description="Bu sayfada, Application Gateway uçtan uca SSL desteği için genel bir bakış sunulmuştur."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amsriva"/>


# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Application Gateway’de SSL İlkesi ve uçtan uca SSL’yi etkinleştirme

## <a name="overview"></a>Genel Bakış

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Bu, web sunucularının maliyetli şifreleme/şifre çözme ek yükünden kurtulmasını sağlar. Ancak arka uç sunucularıyla şifrelenmemiş iletişim, bazı müşteriler için kabul edilebilir bir seçenek değildir. Bunun nedeni, güvenlik/uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için artık uçtan uca SSL şifrelemesini desteklemektedir.

Uçtan uca SSL, hassas verileri arka uca şifrelenmiş olarak güvenli bir şekilde aktarmanızı ve tanımlama bilgisi benzeşimi, URL tabanlı yönlendirme, sitelere dayalı yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleme imkanı gibi Application Gateway’in sunduğu Katman 7 yük dengeleme özelliklerinin avantajlarından yararlanmanızı sağlar.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Uçtan uca SSL, BackendHTTPSetting’de protokol ayarı Https yapılarak etkinleştirilir. Bu ayar, daha sonra arka uç havuzuna uygulanır. Arka uç sunucusunda uçtan uca SSL’nin etkin olduğu her arka uç sunucusunun, güvenli iletişime izin veren bir sertifikayla yapılandırılması gerekir.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

Bu örnekte, uçtan uca SSL kullanılarak https://contoso.com için istekler HTTP üzerinden ContosoServerPool’a ve https://fabrikam.com için istekler HTTPS üzerinden FabrikamServerPool’a yönlendirilebilir.

## <a name="end-to-end-ssl-and-white-listing-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine yüklemeniz gerekir. Yalnızca bilinen ve güvenilir listeye alınan arka uçla bağlantı kurulmasına izin verilir ve geri kalanlar ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bu gibi sertifikaların da kullanılabilmesi için önce uygulama ağ geçidiyle güvenilir listeye alınması gerekir.

## <a name="application-gateway-ssl-policy"></a>Application Gateway SSL İlkesi

Application Gateway, kullanıcı tarafından yapılandırılabilen SSL anlaşma ilkelerini de destekler. Bu ilkeler, müşterilere uygulama ağ geçidi üzerinde daha ayrıntılı denetim imkanı sağlar.

1. SSL 2.0 ve 3.0, tüm Application Gateway’ler için devre dışı bırakılmaya zorlanır. Bunlar tamamen yapılandırılabilir değildir.
2. SSL ilkesi tanımı, şu 3 protokolden herhangi birini değiştirebilmenizi sağlar: TLSv1_0, TLSv1_1, TLSv1_2.
3. Hiçbir SSL ilkesi tanımlanmazsa üçü de (TLSv1_0, TLSv1_1, TLSv1_2) etkinleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL ve SSL ilkesi hakkında bilgi edindikten sonra arka uca şifrelenmiş biçimde trafik gönderme özelliğine sahip uygulama ağ geçidi oluşturmak için [Uygulama ağ geçidinde uçtan uca SSL'yi etkinleştirme](application-gateway-end-to-end-ssl-powershell.md) bölümüne gidin.



<!--HONumber=Oct16_HO3-->


