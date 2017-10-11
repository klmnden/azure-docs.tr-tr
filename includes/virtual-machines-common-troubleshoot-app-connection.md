Başlattığınızda veya bir Azure sanal makine (VM) üzerinde çalışan bir uygulamaya Bağlan çeşitli nedenleri vardır. Çalışmıyor uygulama nedenleri veya beklenen bağlantı noktalarında dinleme, engellenen dinleme bağlantı noktası veya ağ doğru uygulamaya geçirme trafiği kuralları. Bu makalede bulmak ve sorunu düzeltmek için sistemli bir yaklaşım açıklanmaktadır.

RDP veya SSH kullanarak, VM için bağlantı sorunları yaşıyorsanız, aşağıdaki makaleler birine ilk bakın:

* [Uzak Masaüstü bağlantıları için Windows tabanlı Azure sanal makine sorunlarını giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)
* [Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantı sorunlarını giderme](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../articles/resource-manager-deployment-model.md). Bu makale her iki modelin de nasıl kullanıldığını kapsıyor olsa da, Microsoft en yeni dağıtımların Resource Manager modelini kullanmasını önermektedir.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="quick-start-troubleshooting-steps"></a>Hızlı Başlangıç sorun giderme adımları
Bir uygulamaya bağlantı sorunlarınız varsa, aşağıdaki genel sorun giderme adımlarını deneyin. Her adımdan sonra uygulamayı yeniden bağlanmayı deneyin:

* Sanal makineyi yeniden başlatın
* Uç nokta yeniden / kuralları güvenlik duvarı / ağ güvenlik grubu (NSG) kuralları
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
* Farklı bir Azure sanal ağı gibi farklı bir konumdan bağlanma
* Sanal makineyi yeniden dağıtın
  * [Windows VM yeniden dağıtın](../articles/virtual-machines/windows/redeploy-to-new-node.md)
  * [Linux VM yeniden dağıtın](../articles/virtual-machines/linux/redeploy-to-new-node.md)
* Sanal makine oluşturun

Daha fazla bilgi için bkz: [sorun giderme uç noktası bağlantısı (RDP/SSH/HTTP, vb. hataları)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Ayrıntılı sorun giderme genel bakış
Bir Azure sanal makine üzerinde çalışan bir uygulama erişim sorunlarını giderme için dört temel konu vardır.

![sorun giderme uygulaması başlatılamıyor](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. Azure sanal makine üzerinde çalışan uygulama.
   * Uygulamanın düzgün çalışıyor mu?
2. Azure sanal makine.
   * VM düzgün çalışmasını ve isteklere yanıt nedir?
3. Azure ağı uç noktaları.
   * Hizmet uç noktaları Klasik dağıtım modelinde sanal makineler için bulut.
   * Ağ güvenlik grupları ve Resource Manager dağıtım modelinde sanal makineler için gelen NAT kuralları.
   * Kullanıcılar bir akışından VM/uygulamanın beklenen bağlantı noktalarında trafiği?
4. Internet sınır cihazı.
   * Güvenlik duvarı kuralları yerinde doğru akan trafik engelliyor?

Uygulama siteden siteye VPN veya ExpressRoute bağlantısı üzerinden erişen istemci bilgisayarlar için sorunlara neden olabilecek ana uygulama ve Azure sanal makinesini alanlardır.

Sorun ve kendi düzeltme kaynağını belirlemek için aşağıdaki adımları izleyin.

## <a name="step-1-access-application-from-target-vm"></a>Adım 1: hedef VM uygulamaya erişim
Uygun istemci programını uygulamayla üzerinde çalıştığı sanal makineden erişmeyi deneyin. Yerel ana bilgisayar adı, yerel IP adresi ya da geri döngü adresine (127.0.0.1) kullanın.

![doğrudan sanal makineden uygulama Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Örneğin, uygulama bir web sunucusu ise, VM'de bir tarayıcı açın ve VM üzerinde barındırılan bir web sayfasına erişmeyi deneyin.

Uygulama erişebiliyorsanız Git [2. adım](#step2).

Uygulama erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Uygulamanın hedef sanal makinede çalışıyor.
* Uygulamanın, beklenen TCP ve UDP bağlantı noktaları dinliyor.

Hem Windows hem de Linux tabanlı sanal makineleri kullanmak **netstat - a** etkin dinleme bağlantı noktalarını göstermek için komutu. Uygulamanızı dinleniyor olması beklenen bağlantı noktaları için çıktıyı inceleyin. Uygulamayı yeniden başlatın veya gerektiğinde beklenen bağlantı noktalarını kullanacak ve uygulama yerel olarak yeniden erişmeye şekilde yapılandırın.

## <a id="step2"></a>2. adım: aynı sanal ağdaki başka bir VM'den uygulamaya erişim
Farklı bir VM ancak VM ana bilgisayar adı veya Azure tarafından atanan genel, özel veya sağlayıcı IP adresini kullanarak aynı sanal ağda uygulamaya erişmeyi deneyin. Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için bulut hizmetini genel IP adresi kullanmayın.

![farklı bir sanal makineden uygulama Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Uygulama bir web sunucusu ise, örneğin, aynı sanal ağda farklı bir VM üzerinde bir tarayıcıdan web sayfası erişmeyi deneyin.

Uygulama erişebiliyorsanız Git [adım 3](#step3).

Uygulama erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Gelen istek ve yanıt giden trafiği hedef VM konak güvenlik duvarını izin verir.
* Yetkisiz erişim algılama ya da ağ izleme hedef VM çalıştıran yazılım trafiğe izin verdiğinden.
* Bulut Hizmetleri uç noktaları veya ağ güvenlik grupları trafiğe izin:
  * [Klasik model - Cloud Services'ı Yönet uç noktaları](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager modeli - ağ güvenlik gruplarını yönetme](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
* VM'nizi yolundaki arasında test VM ve yük dengeleyici veya güvenlik duvarı gibi VM çalıştıran ayrı bir bileşen trafiğe izin verdiğinden.

Bir Windows tabanlı sanal makinede güvenlik duvarı kuralları, uygulamanızın gelen ve giden trafik hariç olup olmadığını belirlemek için Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı'nı kullanın.

## <a id="step3"></a>Adım 3: Sanal ağ dışındaki erişim uygulama
Uygulamanın sanal ağ dışındaki bir bilgisayardan uygulamanın çalıştığı VM erişmeyi deneyin. Farklı bir ağ özgün istemci bilgisayarınız kullanın.

![sanal ağ dışındaki bir bilgisayardan uygulama Başlat](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Uygulama bir web sunucusu ise, örneğin, sanal ağında olmayan bir bilgisayarda çalışan bir tarayıcıdan web sayfası erişmeyi deneyin.

Uygulama erişemiyorsanız, aşağıdaki ayarları doğrulayın:

* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için:
  
  * VM için uç nokta yapılandırması gelen trafiği, özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını izin verdiğinden emin olun.
  * Erişim denetim listelerini (ACL'ler) uç Internet'ten gelen trafiği engellemediğini doğrulayın.
  * Daha fazla bilgi için bkz: [ayarlamak yukarı uç noktaları için bir sanal makine nasıl](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Resource Manager dağıtım modeli kullanılarak oluşturulan VM'ler için:
  
  * VM için gelen NAT kuralı yapılandırmasını gelen trafiği, özellikle Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktası numaralarını izin verdiğinden emin olun.
  * Ağ güvenlik grupları gelen talep ve giden yanıt trafiği izin emin olun.
  * Daha fazla bilgi için bkz. [Ağ Güvenlik Grubu (NSG) nedir?](../articles/virtual-network/virtual-networks-nsg.md)

Sanal makine ya da uç nokta yük dengelenmiş bir küme üyesi ise:

* Araştırma Protokolü (TCP veya UDP) ve bağlantı noktası numarasının doğru olduğundan emin olun.
* Araştırma protokolü ve bağlantı noktası olup olmadığını yük dengeli kümesi protokolü ve bağlantı noktası farklı:
  * Uygulama araştırma Protokolü (TCP veya UDP) ve bağlantı noktası üzerinde dinleme yaptığını doğrulayın (kullanmak **netstat – a** VM hedefte).
  * Hedef VM konak güvenlik duvarını gelen araştırma isteğinin ve giden araştırma yanıtı trafiğine izin verdiğinden emin olun.

Uygulama erişebiliyorsanız, Internet kenar Cihazınızı izin verdiğinden emin olmak:

* Azure sanal makinesi, istemci bilgisayardan giden uygulama isteği akışına.
* Azure sanal makineden gelen uygulama yanıt trafiği.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Adım 4 uygulama erişemiyorsanız kullanan IP doğrulama ayarlarını denetlemek için. 

Daha fazla bilgi için bkz: [izlemeye genel bakış Azure ağ](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Ek kaynaklar
[Uzak Masaüstü bağlantıları için Windows tabanlı Azure sanal makine sorunlarını giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md)

[Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantı sorunlarını giderme](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md)

