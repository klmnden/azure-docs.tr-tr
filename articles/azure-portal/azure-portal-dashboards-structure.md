---
title: "Azure panolar yapısını | Microsoft Docs"
description: "Bu makale, Azure Pano JSON yapısını açıklar."
services: azure-portal
documentationcenter: 
author: adamab
manager: timlt
editor: tysonn
ms.service: azure-portal
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/01/2017
ms.author: adamab
<<<<<<< HEAD
ms.openlocfilehash: 694b5bd1ddfbaa4c973e9f55bce1c94ffd89c3dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: f71ff9383f20a1a75fd2c1cf4dc3aaf049d970cf
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="the-structure-of-azure-dashboards"></a>Azure panolar yapısı
Bu belge, örnek olarak aşağıdaki Panoyu kullanarak bir Azure panonun yapısı size yol göstermektedir:

![Örnek Pano](./media/azure-portal-dashboards-structure/sample-dashboard.png)

Paylaşılan beri [Azure panolar olan kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), bu panoyu JSON olarak temsil edilebilir.  Aşağıdaki JSON yukarıda görselleştirilen Pano temsil eder.

```json

{
    "properties": {
        "lenses": {
            "0": {
                "order": 0,
                "parts": {
                    "0": {
                        "position": {
                            "x": 0,
                            "y": 0,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "## Azure Virtual Machines Overview\r\nNew team members should watch this video to get familiar with Azure Virtual Machines.",
                                        "title": "",
                                        "subtitle": ""
                                    }
                                }
                            }
                        }
                    },
                    "1": {
                        "position": {
                            "x": 3,
                            "y": 0,
                            "rowSpan": 4,
                            "colSpan": 8
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
                                        "title": "Test VM Dashboard",
                                        "subtitle": "Contoso"
                                    }
                                }
                            }
                        }
                    },
                    "2": {
                        "position": {
                            "x": 0,
                            "y": 2,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [],
                            "type": "Extension[azure]/HubsExtension/PartType/VideoPart",
                            "settings": {
                                "content": {
                                    "settings": {
                                        "title": "",
                                        "subtitle": "",
                                        "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
                                        "autoplay": false
                                    }
                                }
                            }
                        }
                    },
                    "3": {
                        "position": {
                            "x": 0,
                            "y": 4,
                            "rowSpan": 3,
                            "colSpan": 11
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Percentage CPU",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "4": {
                        "position": {
                            "x": 0,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Operations/Sec",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "5": {
                        "position": {
                            "x": 3,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Disk Read Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Disk Write Bytes",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "6": {
                        "position": {
                            "x": 6,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 3
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "queryInputs",
                                    "value": {
                                        "timespan": {
                                            "duration": "PT1H",
                                            "start": null,
                                            "end": null
                                        },
                                        "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
                                        "chartType": 0,
                                        "metrics": [
                                            {
                                                "name": "Network In",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            },
                                            {
                                                "name": "Network Out",
                                                "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                            }
                                        ]
                                    }
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart",
                            "settings": {}
                        }
                    },
                    "7": {
                        "position": {
                            "x": 9,
                            "y": 7,
                            "rowSpan": 2,
                            "colSpan": 2
                        },
                        "metadata": {
                            "inputs": [
                                {
                                    "name": "id",
                                    "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                                }
                            ],
                            "type": "Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart",
                            "asset": {
                                "idInputName": "id",
                                "type": "VirtualMachine"
                            },
                            "defaultMenuItemId": "overview"
                        }
                    }
                }
            }
        },
        "metadata": {
            "model": {
                "timeRange": {
                    "value": {
                        "relative": {
                            "duration": 24,
                            "timeUnit": 1
                        }
                    },
                    "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
                }
            }
        }
    },
    "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/dashboards/providers/Microsoft.Portal/dashboards/aa9786ae-e159-483f-b05f-1f7f767741a9",
    "name": "aa9786ae-e159-483f-b05f-1f7f767741a9",
    "type": "Microsoft.Portal/dashboards",
    "location": "eastasia",
    "tags": {
        "hidden-title": "Created via API"
    }
}

```

## <a name="common-resource-properties"></a>Genel kaynak özellikleri

Şimdi JSON ilgili bölümleri bölün.  Üst düzey özellikleri __kimliği__, __adı__, __türü__, __konumu__, ve __etiketleri__ özellikleri Tüm Azure kaynak türleri arasında paylaşılan. Diğer bir deyişle, bunlar çok Panodaki içerikle yapmanız gerekmez.

### <a name="the-id-property"></a>ID özelliği

Konu için Azure kaynak kimliği [adlandırma kuralları Azure kaynaklarınızın](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Portal bir Pano oluşturduğunda, genellikle bir GUID biçiminde bir kimliği seçer, ancak programlı olarak oluşturduğunuzda, geçerli bir ad kullanmak boş. 

### <a name="the-name-property"></a>Name özelliği
Kaynak abonelik, kaynak türü veya kaynak grubu bilgileri içermez kimliği segment adıdır. Esas olarak, kaynak kimliği son segmenti değil.

### <a name="the-type-property"></a>Tür özelliği
Tüm panolar türlerinin __Microsoft.Portal/dashboards__.

### <a name="the-location-property"></a>Konum özelliği
Diğer kaynaklar bir çalışma zamanı bileşeni panonuz yok.  Panolar için konumun Panodaki JSON gösterimi depolar birincil coğrafi konumu gösterir. Değer kullanılarak getirilen konum kodlarından birini olmalıdır [konumları API abonelikleri kaynakta](https://docs.microsoft.com/rest/api/resources/subscriptions).

### <a name="the-tags-property"></a>Etiketler özelliği
Etiketler, kaynağınız rasgele ad değer çifti tarafından düzenlemenize olanak sağlayan Azure kaynaklarınızın ortak bir özelliktir. Panolar için bir olduğundan özel etiket adı verilen __gizli başlık__. Panonuz doldurulmuş bu özellik varsa, panonuz Portalı'nda görünen adı olarak kullanılır. Azure kaynak kimlikleri yeniden adlandırılamaz ancak etiketleri kullanabilirsiniz. Bu etiket panonuz renamable görünen adı için bir yol sağlar.

`"tags": { "hidden-title": "Created via API" }`

### <a name="the-properties-object"></a>Özellikleri nesnesi
Özellikleri nesnesi iki özellikler içeren __lensleri__ ve __meta verileri__. __Odaklar__ özelliği döşeme hakkında bilgileri (paketini içerir Panoda parçaların).  __Meta veri__ özelliği olduğu için gelecekteki olası özellikler.

### <a name="the-lenses-property"></a>Lenslerin özelliği
__Odaklar__ özelliği, Pano içerir. Lenslerin Bu örnekte nesne Not "0" olarak adlandırılan tek bir özellik içerir. Lenslerin panolarında şu anda uygulanmamaktadır gruplandırma kavramıdır. Şimdilik, tüm panoları "0" olarak adlandırılan bu tek özellik yeniden Mercek nesnesinde sahip.

### <a name="the-lens-object"></a>Mercek nesnesi
Nesne "0" altında iki özellikler içeren __sipariş__ ve __bölümleri__.  Panolar, geçerli sürümde __sipariş__ olduğu her zaman 0. __Bölümleri__ özelliği, tek tek parçaları (paketini tanımlayan bir nesne içerir Panoda bölmeleri).

__Bölümleri__ nesne özelliğinin adı bir sayı olduğu her bölüm için bir özellik içerir. Bu sayı önemli değildir. 

### <a name="the-part-object"></a>Bölümü nesnesi
Her bireysel bölümü nesnesi bir __konumu__, ve __meta verileri__.

### <a name="the-position-object"></a>Konum nesnesi
__Konumu__ özelliğinin bir parçası olarak ifade edilen için boyut ve konum bilgileri içerir __x__, __y__, __rowSpan__ve __colSpan__. Kılavuz birimler cinsinden değerlerdir. Bu kılavuz birimleri, Pano, aşağıda gösterildiği gibi Özelleştir modunda olduğunda görünür. İki kılavuz birimleri genişliğini sağlamak için bir kutucuğa isterseniz, konumu obejct şöyle sonra bir kılavuz birim yüksekliğini ve bir konum üst sol alt köşe panonun:

`location: { x: 0, y: 0, rowSpan: 2, colSpan: 1 }`

![Grid-birimleri](./media/azure-portal-dashboards-structure/grid-units.png)

### <a name="the-metadata-object"></a>Meta veri nesnesi
Meta veri özelliği her bir parçası olan, bir nesne olarak adlandırılan tek gerekli özelliği içeriyor __türü__. Bu dize portal söyler göstermek için hangi döşeme. Bizim örnek Pano kutucukları bu türünü kullanır:


1. `Extension/Microsoft_Azure_Monitoring/PartType/MetricsChartPart`– Göstermek için kullanılan ölçümleri izleme
1. `Extension[azure]/HubsExtension/PartType/MarkdownPart`– Metin veya resim listeleri, bağlantılar, temel biçimlendirmesiyle ile göstermek için kullanılan vb.
1. `Extension[azure]/HubsExtension/PartType/VideoPart`– YouTube, Channel9 ve başka türden herhangi bir html video etiketinde çalışır video videolar göstermek için kullanılır.
1. `Extension/Microsoft_Azure_Compute/PartType/VirtualMachinePart`– Adını ve bir Azure sanal makine durumunu göstermek için kullanılır.

Her bölümü kendi yapılandırma türü. Olası yapılandırma özelliklerini adlı __girişleri__, __ayarları__, ve __varlık__. 

### <a name="the-inputs-object"></a>Giriş nesnesi
Giriş nesnesi genellikle bir kutucuk bir kaynak örneğine bağlar bilgileri içerir.  Bizim örnek Pano sanal makine bölümünde bağlama express için Azure kaynak kimliği kullanan tek bir giriş içerir.  Bu kaynak kimliği biçimi tüm Azure kaynaklarına arasında tutarlıdır.

```json
"inputs":
[
    {
        "name": "id",
        "value": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
    }
]

```
Ölçümleri grafik bölümü görüntülenmesini metric(s) hakkında bilgi yanı sıra bağlamak için kaynak yoğunlaştıracaklardır tek bir giriş var. Ağ giriş ve çıkış ağ ölçümleri gösteren döşeme için giriş aşağıdadır.

```json
“inputs”:
[
    {
        "name": "queryInputs",
        "value": 
        {
            "timespan": 
            {
                "duration": "PT1H",
                "start": null,
                "end": null
           },
            "id": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1",
            "chartType": 0,
            "metrics": 
            [
                {
                    "name": "Network In",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                },
                {
                    "name": "Network Out",
                    "resourceId": "/subscriptions/6531c8c8-df32-4254-d717-b6e983273e5d/resourceGroups/contoso/providers/Microsoft.Compute/virtualMachines/myVM1"
                }
            ]
        }
    }
]

```

### <a name="the-settings-object"></a>Ayarlar nesnesi
Ayarlar nesnesini bir bölümü yapılandırılabilir öğelerini içerir.  Bizim örnek Panoda Markdown bölümü yapılandırılabilir başlık ve alt başlık yanı sıra içerik özel markdown depolamak için ayarları kullanır.

```json
"settings": 
{
    "content": 
    {
        "settings": 
        {
            "content": "This is the team dashboard for the test VM we use on our team. Here are some useful links:\r\n\r\n1. [Getting started](https://www.contoso.com/tsgs)\r\n1. [Troubleshooting guide](https://www.contoso.com/tsgs)\r\n1. [Architecture docs](https://www.contoso.com/tsgs)",
            "title": "Test VM Dashboard",
            "subtitle": "Contoso"
        }
    }
}

```

Benzer şekilde, video döşeme yürütmek için video gösteren bir işaretçi içeren, kendi ayarları, bir Otomatik Yürüt'ü ayarı ve isteğe bağlı başlık bilgilerini içerir.

```json
"settings": 
{
   "content": 
    {
        "settings": 
        {
            "title": "",
            "subtitle": "",
            "src": "https://www.youtube.com/watch?v=YcylDIiKaSU&list=PLLasX02E8BPCsnETz0XAMfpLR1LIBqpgs&index=4",
            "autoplay": false
        }
    }
}

```

### <a name="the-asset-object"></a>Varlık nesnesi
(Varlıklar olarak adlandırılır) ilk sınıfı yönetilebilir portal nesnelere bağlı kutucuklar varlık nesnesini ifade bu ilişkisine sahip.  Bizim örnek panosunda, sanal makine kutucuğu bu varlık açıklaması içerir.  __İdInputName__ özellik kimliği içeren bu durumda kaynak kimliği varlık için benzersiz tanımlayıcı giriş portal söyler. Çoğu Azure kaynak türleri Portalı'nda tanımlanan varlıklar vardır.

`"asset": {    "idInputName": "id",    "type": "VirtualMachine"    }`