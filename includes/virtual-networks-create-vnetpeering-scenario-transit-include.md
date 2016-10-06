## Hizmet Zincirleme - eşlenen VNet üzerinden aktarım

Sistem yollarının kullanımı dağıtımınız için trafiği otomatik olarak kolaylaştırsa da, bir sanal gereç yoluyla paketlerin yönlendirilmesini denetlemek isteyeceğiniz durumlar vardır.
Bu senaryoda, bir abonelikte aşağıdaki diyagramda açıklandığı gibi HubVNet ve VNet1 adlı iki VNet vardır. Ağ Sanal Gerecini (NVA) VNet HubVNet’ dağıtırsınız. HubVNet ile VNet1 arasında VNet eşlemesi kurulduktan Kullanıcı Tanımlı Rotalar ayarlayabilir ve HubVNet içinde NVA’ya sonraki atlamayı belirtebilirsiniz.

![NVA Geçişi](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Kolaylık olması için buradaki tüm VNet'lerin aynı abonelikte olduğu varsayılır. Ancak, çapraz abonelik senaryosunda da işe yarar.

Geçiş yönlendirmeyi etkinleştirmeye yönelik temel özellik "İletilen Trafiğe İzin Ver" parametresidir. Bu parametre eşlenen VNet içindeki NVA’ya trafik almaya/göndermeye izin verir.  


<!--HONumber=Sep16_HO4-->


