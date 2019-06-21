---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: beece95164f0d82b1aa7f22d56f4dce02f4bb38c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188385"
---
Azure CLI, macOS, Linux ve Windows, Azure kaynaklarını oluşturmak ve yönetmek sağlar. Bu makalede oluşturmak ve sanal makineleri (VM'ler) yönetmek için en yaygın komutlardan bazıları ayrıntılı olarak açıklanmaktadır.

Bu makalede Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltmeniz gerekirse bkz [Azure CLI yükleme](/cli/azure/install-azure-cli). Ayrıca [Cloud Shell](/azure/cloud-shell/quickstart) tarayıcınızdan.

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Azure CLI’daki Temel Azure Resource Manager komutları
Daha ayrıntılı belirli komut satırı anahtarları ve seçenekleri konusunda yardım için çevrimiçi Komut Yardımını ve seçeneklerini yazarak kullanabileceğiniz `az <command> <subcommand> --help`.

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

### <a name="get-vm-info"></a>VM bilgilerini alma
| Görev | Azure CLI komutları |
| --- | --- |
| VM'leri listeleme | `az vm list` |
| VM hakkında bilgi alma | `az vm show --resource-group myResourceGroup --name myVM` |
| VM kaynaklarının kullanımını alma | `az vm list-usage --location eastus` |
| Tüm kullanılabilir VM boyutlarını alma | `az vm list-sizes --location eastus` |

## <a name="disks-and-images"></a>Diskleri ve görüntüleri
| Görev | Azure CLI komutları |
| --- | --- |
| Bir VM’ye veri diski ekleme | `az vm disk attach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk --size-gb 128 --new` |
| Bir VM’den veri diski kaldırma | `az vm disk detach --resource-group myResourceGroup --vm-name myVM --disk myDataDisk` |
| Bir diski yeniden boyutlandırma | `az disk update --resource-group myResourceGroup --name myDataDisk --size-gb 256` |
| Bir diskin anlık görüntüsünü alma | `az snapshot create --resource-group myResourceGroup --name mySnapshot --source myDataDisk` |
| Bir VM görüntüsü oluşturma | `az image create --resource-group myResourceGroup --source myVM --name myImage` |
| Görüntüden VM oluşturma | `az vm create --resource-group myResourceGroup --name myNewVM --image myImage` |


## <a name="next-steps"></a>Sonraki adımlar
CLI komutlarının ek örnekler için bkz: [oluşturma ve Azure CLI ile Linux sanal makineleri yönetme](../articles/virtual-machines/linux/tutorial-manage-vm.md) öğretici.

