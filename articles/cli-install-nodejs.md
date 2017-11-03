---
title: "1.0 Azure CLI'yı yükleme | Microsoft Docs"
description: "Mac, Linux ve Windows Azure hizmetlerini kullanmaya başlamak Azure CLI 1.0 yükleyin"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: 63b35ed25b809a16b61b685fd35aa67474b0a369
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-azure-cli-10"></a>1.0 Azure CLI'yı yükleme
> [!div class="op_single_selector"]
> * [PowerShell](/powershell/azure/overview)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> Bu konu, nodeJs üzerinde inşa edilmiş ve çok sayıda Resource Manager dağıtım etkinliklerini yanı sıra tüm Klasik dağıtım API çağrıları destekleyen Azure CLI 1.0 yüklemeyi açıklar. Kullanmanız gereken [Azure CLI 2.0](/cli/azure/overview) yeni veya forward-looking CLI dağıtım ve yönetimi için.

Hızlı bir şekilde arabirimi oluşturmak ve Microsoft Azure kaynakları yönetmek için açık kaynak kabuk tabanlı komut kümesini kullanmak için Azure komut satırı (Azure CLI 1.0) yükleyin. Bu platformlar arası araçları bilgisayarınıza yüklemek için birkaç seçeneğiniz vardır:

* **npm paket** -Linux dağıtım ya da OS en son Azure CLI 1.0 paketini yüklemek için npm (JavaScript için Paket Yöneticisi) çalıştırın. Node.js ve bilgisayarınızda npm gerektirir.
* **Yükleyici** -Mac veya Windows kolay yükleme için bir yükleyici indirin.
* **Docker kapsayıcısı** - hazır Çalıştır Docker kapsayıcısı içinde son CLI kullanmaya başlayabilirsiniz. Docker ana bilgisayarınızda gerektirir.

Daha fazla seçenekleri ve arka plan için proje deposu bakın [GitHub](https://github.com/azure/azure-xplat-cli).

Azure CLI 1.0 yüklendikten sonra [Azure aboneliğinizle bağlanmak](xplat-cli-connect.md) çalıştırıp **azure** çalışmak için komut satırı arabiriminden (Bash, Terminal, komut istemi vb.) komutları, Azure kaynakları.

## <a name="option-1-install-an-npm-package"></a>Seçenek 1: bir npm paket yükleme
CLI bir npm paket yüklemek için indirilir ve yüklenir emin olun [en son Node.js ve npm](https://nodejs.org/en/download/package-manager/). Daha sonra çalıştırın **npm yükleme** azure CLI paketini yüklemek için:

```bash
npm install -g azure-cli
```

Linux dağıtımları hakkında kullanmanız gerekebilir **sudo** başarıyla çalışması için **npm** komutu, şu şekilde:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Yükleme veya güncelleştirme Node.js ve Linux dağıtımı veya işletim sistemi npm ihtiyacınız varsa, en son Node.js LTS sürüm (4.x) yüklemenizi öneririz. Eski bir sürümünü kullanıyorsanız, yükleme hataları alabilirsiniz.

Tercih ederseniz, en son Linux karşıdan [tar dosya] [ linux-installer] yerel olarak npm paket için. Daha sonra indirilen npm paket aşağıdaki gibi yükleyin (Linux dağıtımları kullanmanız gerekebilir üzerinde **sudo**):

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Seçenek 2: bir yükleyici kullanın
Mac veya Windows bilgisayarı kullanıyorsanız, aşağıdaki CLI yükleyicileri indirme için kullanılabilir:

* [Mac OS X yükleyici][mac-installer]
* [Windows MSI][windows-installer]

> [!TIP]
> Windows üzerinde de indirebilirsiniz [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9828653) CLI yükleme. Bu yükleyici CLI yükledikten sonra ek Azure SDK ve komut satırı araçlarını yüklemek için seçeneği sunar.

## <a name="option-3-use-a-docker-container"></a>Seçenek 3: bir Docker kapsayıcısı kullanın
Bilgisayarınızda olarak ayarladıysanız, bir [Docker](https://docs.docker.com/engine/understanding-docker/) konak, en son Azure CLI 1.0 Docker kapsayıcısı içinde çalıştırabilirsiniz. Aşağıdaki komutu çalıştırın (Linux dağıtımları kullanmanız gerekebilir üzerinde **sudo**):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Azure CLI 1.0 komutlarını çalıştırın
Azure CLI 1.0 yüklendikten sonra çalıştıracak **azure** komutu, komut satırı kullanıcı arabiriminden (Bash, Terminal, komut istemi vb.). Örneğin, Yardım komutu çalıştırmak için aşağıdakini yazın:

```azurecli
azure help
```

> [!NOTE]
> Bazı Linux dağıtımları üzerinde benzer bir hata iletisi alabilirsiniz `/usr/bin/env: ‘node’: No such file or directory`. Bu hata /usr/bin/nodejs sırasında yüklenen Node.js son yüklemeleri gelir. Sorunu gidermek için şu komutu çalıştırarak /usr/bin/node sembolik bağlantı oluşturun:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Azure CLI 1.0, yüklü sürümünü görmek için aşağıdaki komutu yazın:

```azurecli
azure --version
```

Artık hazırsınız! Kendi kaynakları ile çalışmak için tüm CLI komutlara erişmek için [Azure CLI üzerinden Azure aboneliğinize bağlanmak](xplat-cli-connect.md).

> [!NOTE]
> Azure CLI ilk kullandığınızda Microsoft kullanım bilgilerini toplamasına izin vermek isteyip istemediğinizi soran bir ileti görürsünüz. Katılım gönüllüdür. Katılmayı seçerseniz, çalıştırarak herhangi bir zamanda durdurabilirsiniz `azure telemetry --disable`. Katılım herhangi bir zamanda etkinleştirmek için çalıştırın `azure telemetry --enable`.

## <a name="update-the-cli"></a>CLI’yı güncelleştirme
Microsoft Azure CLI güncelleştirilmiş sürümlerini sık serbest bırakır. İşletim sisteminizi Yükleyicisi'ni kullanarak CLI yeniden yükleyin veya son Docker kapsayıcısı çalıştırın. Veya, en son Node.js ve yüklü npm varsa, aşağıdakini yazarak güncelleştirmeniz (kullanmanız gerekebilir Linux dağıtımları hakkında **sudo**).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Sekme tamamlama etkinleştir
Sekme tamamlama CLI komutlarının Mac ve Linux için desteklenir.

İçinde zsh etkinleştirmek için çalıştırın:

```bash
echo '. <(azure --completion)' >> .zshrc
```

Bash'te etkinleştirmek için çalıştırın:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Sonraki adımlar
* [CLI üzerinden Azure aboneliğinize bağlanmak](xplat-cli-connect.md) oluşturmak ve Azure kaynaklarınızı yönetmek için.
* Projeye katkıda ya da Azure CLI, yükleme kaynak kodu, rapor sorunları hakkında daha fazla bilgi için ziyaret [Azure CLI için GitHub depo](https://github.com/azure/azure-xplat-cli).
* Azure CLI veya Azure kullanma hakkında sorularınız varsa, ziyaret [Azure forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
