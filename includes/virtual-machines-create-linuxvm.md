---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ae29451e3f7ec263f296e69656a5c66045334687
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61126728"
---
1. Listelenen adımları kullanarak Azure aboneliğinizde oturum açın [Klasik Azure clı'dan Azure'a bağlanma](/cli/azure/authenticate-azure-cli).

2. Klasik dağıtım modunda olduğunuzdan emin olmak için aşağıdakileri yapın:

    ```azurecli
    azure config mode asm
    ```

3. Kullanılabilir görüntülerden yüklemek istediğiniz Linux görüntüsünü bulmak için aşağıdakileri yapın:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    Bir Windows komut istemi penceresinde grep yerine **find** komutunu kullanın.
   
4. Önceki listeden Linux görüntüsü ile VM oluşturmak için `azure vm create` komutunu kullanın. Bu adımda bir bulut hizmeti ve depolama hesabı oluşturulur. Bu VM’yi bir `-c` seçeneği ile var olan bir bulut hizmetine de bağlamanız gerekir. `-e` seçeneği ile Linux sanal makinesinde oturum açmak için bir SSH uç noktası oluşturun. Aşağıdaki örnekte, `West US` konumunda `Ubuntu-14_04_4-LTS` görüntüsü kullanılarak `myVM` adlı bir VM oluşturulur ve `ops` kullanıcı adı eklenir:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Bir Linux sanal makinesi için `vm create` içinde `-e` seçeneğini belirtmeniz gerekir. Sanal makine oluşturulduktan sonra SSH etkinleştirilemez. SSH hakkında daha ayrıntılı bilgi için bkz. [Azure’da Linux ile SSH kullanma](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. `azure vm show` komutunu kullanarak VM’nin özniteliklerini doğrulayabilirsiniz. Aşağıdaki örnekte `myVM` adlı VM’nin bilgileri listelenmiştir:

    ```azurecli   
    azure vm show myVM
    ```

6. VM’nizi aşağıdaki gibi `azure vm start` komutuyla başlatın:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Sonraki adımlar
Tüm bu Azure Klasik CLI sanal makine komutlarıyla ilgili daha fazla bilgi için okuma [Klasik Azure CLI kullanarak Klasik dağıtım API'si ile](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

