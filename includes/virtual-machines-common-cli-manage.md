Azure CLI 2.0 oluşturup macOS, Linux ve Windows Azure kaynaklarınızı yönetmek sağlar. Bu makalede bazı oluşturmak ve sanal makineleri (VM'ler) yönetmek için en sık kullanılan komutlar ayrıntılarını verir.

Bu makale Azure CLI Sürüm 2.0.4 gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Aynı zamanda [bulut Kabuk](/azure/cloud-shell/quickstart) tarayıcınızdan.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Azure CLI’daki Temel Azure Resource Manager komutları
Daha ayrıntılı belirli komut satırı anahtarları ve seçenekleri ile ilgili Yardım için çevrimiçi komut Yardımı ve Seçenekler yazarak kullanabileceğiniz `az <command> <subcommand> --help`.

### <a name="create-vms"></a>VM oluşturma
| Görev | Azure CLI komutları |
| --- | --- |
| Kaynak grubu oluşturma | `az group create --name myResourceGroup --location eastus` |
| Linux VM oluşturma | `az vm create --resource-group myResourceGroup --name myVM --image ubuntults` |
| Windows VM oluşturma | `az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter` |

### <a name="manage-vm-state"></a>VM durumunu yönetme
| Görev | Azure CLI komutları |
| --- | --- |
| VM başlatma | `az vm start --resource-group myResourceGroup --name myVM` |
| VM durdurma | `az vm stop --resource-group myResourceGroup --name myVM` |
| Bir VM’yi serbest bırakma | `az vm deallocate --resource-group myResourceGroup --name myVM` |
| Bir VM’yi yeniden başlatma | `az vm restart --resource-group myResourceGroup --name myVM` |
| Bir VM’yi yeniden dağıtma | `az vm redeploy --resource-group myResourceGroup --name myVM` |
| VM silme | `az vm delete --resource-group myResourceGroup --name myVM` |

### <a name="get-vm-info"></a>VM bilgilerini al
| Görev | Azure CLI komutları |
| --- | --- |
| VM'leri listeleme | `az vm list` |
| VM hakkında bilgi alma | `az vm show --resource-group myResourceGroup --name myVM` |
| VM kaynaklarının kullanımını alma | `az vm list-usage --location eastus` |
| Tüm kullanılabilir VM boyutlarını alma | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Diskleri ve görüntüleri
| Görev | Azure CLI komutları |
| --- | --- |
| Bir VM’ye veri diski ekleme | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new ` |
| Bir VM’den veri diski kaldırma | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Bir diski yeniden boyutlandırma | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Bir diskin anlık görüntüsünü alma | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Bir VM görüntüsü oluşturma | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| VM görüntüsünü oluşturma | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Sonraki adımlar
CLI komutları ek örnekler için bkz: [oluşturma ve yönetme Linux VM'ler Azure CLI ile](../articles/virtual-machines/linux/tutorial-manage-vm.md) Öğreticisi.

