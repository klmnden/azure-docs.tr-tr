| Kaynak | Varsayılan Sınır |
| --- | :--- |
| Küme başına en fazla düğüm | 100 |
| Düğüm başına en fazla pod ([Kubenet ile temel ağ][basic-networking]) | 110 |
| Düğüm başına en fazla pod ([Azure CNI ile gelişmiş ağ][advanced-networking]) | 30<sup>1</sup> |
| Abonelik başına en fazla küme | 20<sup>2</sup> |

<sup>1</sup> Bu değer ARM şablon dağıtımı üzerinden özelleştirilebilir. Örnekleri [burada][arm-deployment-example] görebilirsiniz.<br />
<sup>2</sup> Sınır yükseltme isteği için bir [Azure destek isteği][azure-support] oluşturun.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
