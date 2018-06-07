---
title: Yükseltme/SKU için daha yüksek SKU'ları PrimaryNodeType geçirmek için yordamı | Microsoft Docs
description: Yükseltme/SKU için PowerShell komutlarını ve CLI komutları kullanarak daha yüksel SKU'lar için PrimaryNodeType geçirmek öğrenin.
services: service-fabric
documentationcenter: .net
author: v-rachiw
manager: navya
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2018
ms.author: v-rachiw
ms.openlocfilehash: 96956543a44b6d5d967e3bae3fd833b08baf3d6f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642742"
---
# <a name="upgrademigrate-the-sku-for-primary-node-type-to-higher-sku"></a>Yükseltme/SKU birincil düğüm türü için daha yüksek SKU geçirme

Bu makalede yükseltme/SKU, birincil düğüm türü service fabric kümesi için daha yüksek SKU Azure PowerShell kullanarak nasıl geçirileceği açıklanmaktadır

## <a name="add-a-new-virtual-machine-scale-set"></a>Yeni bir sanal makine ölçek kümesi Ekle

Yeni sanal makine ölçek kümesi ve yük dengeleyici dağıtın. Yeni sanal makine ölçek kümesi Service Fabric uzantı yapılandırması (özellikle düğüm türü), eski ölçek kümesi aynı yükseltmeye çalıştığınız olmalıdır. Service Fabric explorer'ın yeni düğümleriniz kullanılabilir olduğunu doğrulayın

#### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki örnek, güncelleştirilmiş Resource Manager şablonu dağıtmak için Azure PowerShell'i kullanır. *template.json* adlı kaynak grubunu kullanarak *myResourceGroup*:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile template.json -TemplateParameterFile parameters.json
```

#### <a name="azure-cli"></a>Azure CLI

Güncelleştirilmiş Resource Manager şablonu dağıtmak için aşağıdaki komutu Azure Service Fabric CLI kullanan *template.json* adlı kaynak grubunu kullanarak *myResourceGroup*:

```CLI
az group deployment create --resource-group myResourceGroup --template-file template.json --parameters parameters.json
```

Yeni sanal makine ölçek kümesi kaynağı mevcut kümede birincil düğüm türü eklemek için json şablonunu değiştirmek için örnek bakın

```json
        {
            "apiVersion": "[variables('vmssApiVersion')]",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[parameters('vmNodeType2Name')]",
            "location": "[parameters('computeLocation')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType2Name')))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
            ],
            "properties": {
                "overprovision": "[parameters('overProvision')]",
                "upgradePolicy": {
                    "mode": "Automatic"
                },
                "virtualMachineProfile": {
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "[concat(parameters('vmNodeType2Name'),'_ServiceFabricNode')]",
                                "properties": {
                                    "type": "ServiceFabricNode",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                                    },
                                    "publisher": "Microsoft.Azure.ServiceFabric",
                                    "settings": {
                                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                                        "dataPath": "D:\\\\SvcFab",
                                        "durabilityLevel": "Bronze",
                                        "enableParallelJobs": true,
                                        "nicPrefixOverride": "[parameters('subnet0Prefix')]",
                                        "certificate": {
                                            "thumbprint": "[parameters('certificateThumbprint')]",
                                            "x509StoreName": "[parameters('certificateStoreValue')]"
                                        }
                                    },
                                    "typeHandlerVersion": "1.0"
                                }
                            },
                            {
                                "name": "[concat('VMDiagnosticsVmExt','_vmNodeType2Name')]",
                                "properties": {
                                    "type": "IaaSDiagnostics",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                        "storageAccountEndPoint": "https://core.windows.net/"
                                    },
                                    "publisher": "Microsoft.Azure.Diagnostics",
                                    "settings": {
                                        "WadCfg": {
                                            "DiagnosticMonitorConfiguration": {
                                                "overallQuotaInMB": "50000",
                                                "EtwProviders": {
                                                    "EtwEventSourceProviderConfiguration": [
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Actors",
                                                            "scheduledTransferKeywordFilter": "1",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableActorEventTable"
                                                            }
                                                        },
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Services",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                            }
                                                        }
                                                    ],
                                                    "EtwManifestProviderConfiguration": [
                                                        {
                                                            "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                            "scheduledTransferLogLevelFilter": "Information",
                                                            "scheduledTransferKeywordFilter": "4611686018427387904",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricSystemEventTable"
                                                            }
                                                        }
                                                    ]
                                                }
                                            }
                                        },
                                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                                    },
                                    "typeHandlerVersion": "1.5"
                                }
                            }
                        ]
                    },
                    "networkProfile": {
                        "networkInterfaceConfigurations": [
                            {
                                "name": "[concat(parameters('nicName'), '-2')]",
                                "properties": {
                                    "ipConfigurations": [
                                        {
                                            "name": "[concat(parameters('nicName'),'-',2)]",
                                            "properties": {
                                                "loadBalancerBackendAddressPools": [
                                                    {
                                                        "id": "[variables('lbPoolID2')]"
                                                    }
                                                ],
                                                "loadBalancerInboundNatPools": [
                                                    {
                                                        "id": "[variables('lbNatPoolID2')]"
                                                    }
                                                ],
                                                "subnet": {
                                                    "id": "[variables('subnet0Ref')]"
                                                }
                                            }
                                        }
                                    ],
                                    "primary": true
                                }
                            }
                        ]
                    },
                    "osProfile": {
                        "adminPassword": "[parameters('adminPassword')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "computernamePrefix": "[parameters('vmNodeType2Name')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[parameters('sourceVaultValue')]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateStore": "[parameters('certificateStoreValue')]",
                                        "certificateUrl": "[parameters('certificateUrlValue')]"
                                    }
                                ]
                            }
                        ]
                    },
                    "storageProfile": {
                        "imageReference": {
                            "publisher": "[parameters('vmImagePublisher')]",
                            "offer": "[parameters('vmImageOffer')]",
                            "sku": "[parameters('vmImageSku')]",
                            "version": "[parameters('vmImageVersion')]"
                        },
                        "osDisk": {
                            "caching": "ReadOnly",
                            "createOption": "FromImage",
                            "managedDisk": {
                                "storageAccountType": "[parameters('storageAccountType')]"
                            }
                        }
                    }
                }
            },
            "sku": {
                "name": "[parameters('vmNodeType2Size')]",
                "capacity": "[parameters('nt2InstanceCount')]",
                "tier": "Standard"
            },
            "tags": {
                "resourceType": "Service Fabric",
                "clusterName": "[parameters('clusterName')]"
            }
        },
```

## <a name="remove-old-virtual-machine-scale-set"></a>Eski sanal makine ölçek kümesi Kaldır

1. Eski sanal makine ölçek düğümünü kaldırmak için amacıyla kümesi VM örneklerini devre dışı bırakın. Bu işlemin tamamlanması uzun sürebilir. Aynı anda devre dışı bırakabilir ve sıraya alınması. Tüm düğümleri devre dışı bırakılana kadar bekleyin. 
#### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnek, düğüm örneği adlı devre dışı bırakmak için Azure Service Fabric PowerShell kullanır *NTvm1_0* adlandırılmış kümesi eski sanal makine ölçek gelen *NTvm1*:
```powershell
Disable-ServiceFabricNode -NodeName NTvm1_0 -Intent RemoveNode
```
#### <a name="azure-cli"></a>Azure CLI
Adlı düğüm örneği devre dışı bırakmak için aşağıdaki komutu Azure Service Fabric CLI kullanan *NTvm1_0* adlandırılmış kümesi eski sanal makine ölçek gelen *NTvm1*:
```CLI
sfctl node disable --node-name "_NTvm1_0" --deactivation-intent RemoveNode
```
2. Tam ölçek kümesini kaldırın. Ölçeği sağlama durumu ayarla başarılı kadar bekleyin
#### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnek, tam ölçeği adlandırılmış Ayarla kaldırmak için Azure PowerShell'i kullanır. *NTvm1* adlı kaynak grubundan *myResourceGroup*:
```powershell
Remove-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName NTvm1
```
#### <a name="azure-cli"></a>Azure CLI
Tam ölçeği adlandırılmış Ayarla kaldırmak için aşağıdaki komutu Azure Service Fabric CLI kullanan *NTvm1* adlı kaynak grubundan *myResourceGroup*:

```CLI
az vmss delete --name NTvm1 --resource-group myResourceGroup
```

## <a name="remove-load-balancer-related-to-old-scale-set"></a>Yük Dengeleyici eski ölçek kümesine ilgili Kaldır

Yük Dengeleyici eski ölçek kümesine ilgili kaldırın. Bu adım, küme için kapalı kalma süresi kısa bir süre neden olur

#### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki örnek, yük dengeleyici adlı kaldırmak için Azure PowerShell'i kullanır. *LB myCluster NTvm1* eski ölçeği adlı kaynak grubundan ayarla ilgili *myResourceGroup*:

```powershell
Remove-AzureRmLoadBalancer -Name LB-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>Azure CLI

Adlı yük dengeleyici kaldırmak için aşağıdaki komutu Azure Service Fabric CLI kullanan *LB myCluster NTvm1* eski ölçeği adlı kaynak grubundan ayarla ilgili *myResourceGroup*:

```CLI
az network lb delete --name LB-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="remove-public-ip-related-to-old-scale-set"></a>Eski ölçek kümesine ilgili genel IP Kaldır

Genel IP adresinin mağaza DNS ayarları eski ölçeği değişkene ayarla ilgili sonra ortak IP adresini kaldırın

#### <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki örnek, adlı ortak IP adresinin DNS ayarlarını depolamak için Azure PowerShell'i kullanır. *LBIP myCluster NTvm1* değişkeni ve Kaldır IP adresi:

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn
Remove-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

#### <a name="azure-cli"></a>Azure CLI

Aşağıdaki komutu adlı ortak IP adresinin DNS ayarlarını almak için Azure Service Fabric CLI kullanan *LBIP myCluster NTvm1* ve IP adresini kaldırın:

```CLI
az network public-ip show --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
az network public-ip delete --name LBIP-myCluster-NTvm1 --resource-group myResourceGroup
```

## <a name="update-public-ip-address-related-to-new-scale-set"></a>Yeni ölçek kümesine ilgili güncelleştirme genel IP adresi

Yeni ölçeği eski ölçek kümesine ilgili ortak IP adresinin DNS ayarlarla ayarla ilgili genel IP adresi DNS ayarlarını güncelleştirme

#### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnek, adlı ortak IP adresinin DNS ayarlarını güncelleştirmek için Azure PowerShell'i kullanır. *LBIP myCluster NTvm3* önceki adımda değişkenleri depolanan DNS ayarları:

```powershell
$PublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm3  -ResourceGroupName myResourceGroup
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP
```

#### <a name="azure-cli"></a>Azure CLI

Adlı ortak IP adresinin DNS ayarlarını güncelleştirmek için aşağıdaki komutu Azure Service Fabric CLI kullanan *LBIP myCluster NTvm3* eski DNS ayarlarını genel IP toplanan önceki adımda ile:

```CLI
az network public-ip update --name LBIP-myCluster-NTvm3 --resource-group myResourceGroup --dns-name myCluster
```

## <a name="remove-knowledge-of-service-fabric-node-from-fm"></a>Service fabric düğümünün bilgi FM Kaldır

Service Fabric kapalı düğümleri kümeden kaldırdığını bildirin. (Sanal makine ölçek kümesi eski tüm VM örnekleri için bu komutu çalıştırmak) (Eski sanal makine ölçek kümesi dayanıklılık düzeyi, Gümüş veya altın idiyse, bu adım gerekli olmayabilir. İtibaren adım sistem tarafından otomatik olarak gerçekleştirilir.)

#### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnek, düğüm adlı service fabric bildirmek için Azure Service Fabric PowerShell kullanır *NTvm1_0* kaldırıldı:

```powershell
Remove-ServiceFabricNodeState -NodeName NTvm1_0
```

#### <a name="azure-cli"></a>Azure CLI

Düğüm adlı service fabric bildirmek için aşağıdaki komutu Azure Service Fabric CLI kullanan *NTvm1_0* kaldırıldı:

```CLI
sfctl node remove-state --node-name _NTvm1_0
```
