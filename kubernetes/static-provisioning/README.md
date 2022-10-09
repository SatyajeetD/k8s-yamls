# Static Provisioning
Required in case existing pavilion volume needs to be attached to container. Especially it has noformat flag which is useful in case the old volume data needs to be re-used.

Here are the steps:
## Edit pv.yaml for volume properties and then create pv.
   kubectl create -f pv.yaml

## Confirm pv has been created successfully
   kubectl get pv <pv_name>

## Edit pvc.yaml for pv name and then create pvc.
   kubectl create -f pvc.yaml

## Confirm pvc has been created successfully
   kubectl get pvc <pv_name>

## Edit pod.yaml for your application container and pvc name. Then create pod.
   kubectl create -f pod.yaml
