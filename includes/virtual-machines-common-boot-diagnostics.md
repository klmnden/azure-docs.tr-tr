İki hata ayıklama özelliklerini artık mevcut desteği: konsol çıkışı ve ekran görüntüsü için Azure sanal makineleri Resource Manager dağıtım modelini destekler. 

Azure veya hatta platform görüntülerden birini önyükleme için kendi görüntünüzü duruma getirilirken bir sanal makine önyüklenebilir olmayan bir duruma neden alır birçok nedeni olabilir. Bu özellikler kolayca tanılamak ve sanal makinelerinizi önyükleme arızalardan kurtarmak etkinleştirin.

Linux sanal makineleri için Portalı'ndan konsol oturum çıktısını kolayca görüntüleyebilirsiniz:

![Azure portalına](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Ancak, Windows ve Linux sanal makineler için Azure Ayrıca, bir ekran görüntüsünü hiper yönetici VM'den görmenizi sağlar:

![Hata](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Bu özelliklerin her ikisi de tüm bölgelerdeki Azure sanal makineler için desteklenir. Not, ekran görüntüleri ve çıktının depolama hesabınızda görünmesi 10 dakika sürebilir.

## <a name="common-boot-errors"></a>Sık karşılaşılan önyükleme hataları

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Bir işletim sistemi bulunamadı](https://support.microsoft.com/help/4010142)
- [Önyükleme hatası veya INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Yeni bir sanal makine üzerinde tanılamayı etkinleştir
1. Azure Portal'dan yeni bir sanal makine oluştururken, seçin **Azure Resource Manager** gelen dağıtım modeli açılır:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. İçinde **ayarları**, etkinleştirme **önyükleme tanılama**ve ardından bu tanılama dosyaları yerleştirmek istediğiniz depolama hesabını seçin.
 
    ![VM oluşturma](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > Premium depolama hesabı önyükleme Tanılama özelliğini desteklemiyor. Premium depolama hesabı için önyükleme tanılaması kullanıyorsanız, VM başlattığınızda StorageAccountTypeNotSupported hatayı alabilirsiniz.
    >
    > 

3. Bir Azure Resource Manager şablonu dağıtıyorsanız, sanal makine kaynağınız gidin ve tanılama profil bölümü ekleyin. "2015-06-15" API sürümü üst bilgisini kullanmayı unutmayın.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Tanılama profili, bu günlükleri yerleştirmek istediğiniz depolama hesabını seçmenize olanak sağlar.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Önyükleme tanılaması etkin bir örnek sanal makine dağıtmak için burada bizim depodaki denetleyin.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Varolan sanal makinesindeki önyükleme tanılaması etkinleştir 

Önyükleme Tanılaması, var olan bir sanal makinede etkinleştirmek için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](https://portal.azure.com)ve ardından sanal makineyi seçin.
2. İçinde **destek + sorun giderme**seçin **önyükleme tanılama** > **ayarları**, durumunu değiştir **üzerinde**ve ardından bir depolama hesabı seçin. 
4. Önyükleme tanılama seçeneğinin seçili olduğundan emin olun ve ardından değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Etkili olması için VM'yi yeniden başlatın.


