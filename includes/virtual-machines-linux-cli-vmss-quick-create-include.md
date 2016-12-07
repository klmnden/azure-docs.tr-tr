Daha önce yapmadıysanız [Azure aboneliği ücretsiz deneme sürümünü](https://azure.microsoft.com/pricing/free-trial/) ve [Azure hesabınıza bağlı](../articles/xplat-cli-connect.md) [Azure CLI](../articles/xplat-cli-install.md)’yı alabilirsiniz. Azure CLI’nin Resource Manager’da şu şekilde olduğundan emin olun:

```azurecli
azure config mode arm
```

Şimdi `azure vmss quick-create` komutunu kullanarak ölçek kümenizi oluşturabilirsiniz. Aşağıdaki örnek, `myResourceGroup` adlı kaynak grubunun içinde 5 VM örneğine sahip `myVMSS` adlı ölçek kümesi oluşturur:

```azurecli
azure vmss quick-create -n myVMSS -g myResourceGroup -l westus \
    -u ops -p P@ssw0rd! \
    -C 5 -Q Canonical:UbuntuServer:14.04.4-LTS:latest
```

Konum veya görüntü urn’sini özelleştirmek istiyorsanız `azure location list` ve `azure vm image {list-publishers|list-offers|list-skus|list|show}` komutlarına bakın.

Bu komut döndürüldükten sonra, ölçek kümesi oluşturulacaktır. Bu ölçekte, yük dengeleyicideki bağlantı noktası 50.000+i’yi VM i’deki bağlantı noktası 22’yle eşleyen NAT kurallarına sahip yük dengeleyici olacaktır. Bu nedenle, yük dengeleyicinin FQDN’sini belirledikten sonra VM’lerimize ssh aracılığıyla bağlanabileceğiz:

```bash
# (if you decide to run this as a script, please invoke using bash)

# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g negatvmssrg | grep negatvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is 
# associated to it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LOAD-BALANCER-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g negatvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g negatvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can use ssh on port 50,000+i to connect to VM i (where i is 0-indexed)
#
# example to connct via ssh into VM "0":
ssh -p 50000 negat@$FQDN
```

<!--HONumber=Nov16_HO2-->


