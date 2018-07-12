* **vCenter Ekle** menüsünde vSphere konağı veya vCenter sunucusu için bir kolay ad belirtin ve ardından sunucunun IP adresini ya da FQDN’sini belirtin. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın. VMware vCenter veya vSphere ESXi sunucusuna bağlanacak hesabı seçin. **Tamam**’a tıklayın.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > vCenter veya konak sunucusu üzerinde yönetici ayrıcalıklarına sahip olmayan bir hesapla VMware vCenter sunucusunu veya VMware vSphere konağını ekliyorsanız, hesabın şu ayrıcalıklarının etkin olduğundan emin olun: Veri Merkezi, Veri Deposu, Klasör, Konak, Ağ, Kaynak, Sanal makine ve vSphere Dağıtılmış Anahtarı. Ayrıca, VMware vCenter sunucusunda Depolama görünümleri ayrıcalığının etkin olması gerekir.
