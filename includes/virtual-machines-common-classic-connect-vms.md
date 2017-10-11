

![Sanal makinelerde tek başına bulut hizmeti](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Bir sanal ağ, sanal makineleriniz yerleştirirseniz kaç bulut Hizmetleri için kullanmak istediğiniz yük dengeleme ve kullanılabilirlik kümeleri karar verebilirsiniz. Ayrıca, şirket içi ağ ve sanal ağı şirket içi ağınıza bağlanmak ağlardaki sanal makineler aynı şekilde düzenleyebilirsiniz. Örnek aşağıda verilmiştir:

![Bir sanal ağdaki sanal makineler](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Sanal ağlar, Azure sanal makinelere bağlanmak için önerilen yoldur. En iyi uygulama olarak, her katman, uygulamanızın ayrı bulut hizmetinde yapılandırmaktır. Ancak, en fazla abonelik başına 200 bulut Hizmetleri içinde kalması için aynı bulut hizmetine bazı sanal makinelerden farklı bir uygulama katmanları birleştirebilirsiniz gerekebilir. Bu ve diğer sınırları gözden geçirmek için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Sanal makineleri sanal bir ağa bağlan
Bir sanal ağdaki sanal makinelere bağlanmak için:

1. Sanal ağ oluşturma [Azure portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve 'Klasik dağıtım' belirtin.
2. Bulut Hizmetleri ve Yük Dengeleme tasarımınız kullanılabilirlik kümeleri için yansıtmak üzere dağıtımınız için kümesi oluşturun. Azure portalında tıklatın **yeni > işlem > bulut hizmeti** her bir bulut hizmeti için.

  Bulut hizmetinin ayrıntılarını dolgu olarak aynı seçin _kaynak grubu_ sanal ağ ile kullanılır.

3. Her yeni bir sanal makine oluşturmak için tıklatın **yeni > işlem**, ardından uygun VM görüntüsünü seçin **öne çıkan uygulamalar**.

  VM'deki **Temelleri** dikey penceresinde, aynı seçin _kaynak grubu_ sanal ağ ile kullanılır.

  ![Bir sanal ağ kullanırken VM temel bilgileri dikey penceresi](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. VM'i dolgu olarak **ayarları**, doğru seçin _bulut hizmeti_ veya _sanal ağ_ VM için.

  Azure seçiminize bağlı diğer öğesini seçer.

  ![Bir sanal ağ kullanırken VM ayarlar dikey penceresi](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Sanal makineleri bir tek başına bulut hizmetine bağlanmak
Bir tek başına bulut hizmeti sanal makinelere bağlanmak için:

1. Bulut hizmeti oluşturma [Azure portal](http://portal.azure.com). Tıklatın **yeni > işlem > bulut hizmeti**. Veya, ilk sanal makine oluşturduğunuzda, dağıtımınız için bulut hizmeti oluşturabilir.
2. Sanal makine oluşturduğunuzda, bulut hizmeti ile kullanılan aynı kaynak grubunu seçin.

  ![Bir sanal makine var olan bir bulut hizmetine Ekle](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  VM ayrıntılarını doldurun gibi ilk adımda oluşturduğunuz bulut hizmeti adını seçin.

  ![Bir sanal makine için bir bulut hizmeti seçme](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
