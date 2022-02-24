## Introducción
Para el despliegue de Infraestructura, Ferrovial Servicios cuenta con un pipeline construido para este fin, el cual permite la integración de módulos de Terraform externos. Además, poseé controles para asegurar el cumplimiento de los estándares establecidos por el área de Infraestructura.
A continuación, se enseñarán los cambios que se deben realizar en el código del pipeline para permitir esta integración y cuales son los controles a cumplir para lograr un despliegue exitoso.

# Definiciones por parte de Ferrovial Servicios

## Modificación en el código del pipeline
Como prerrequisito para la utilización del pipeline, es necesario contar con un Resource Group  y un Storage Account, dentro de Azure, exclusivo para alojar los state files de Terraform del despliegue. Además, es necesario definir el Service Principal creado que tendrá los permisos necesarios para ejecutar el despliegue de la infraestructura.
Estos valores, se definen dentro del archivo custom-variables.yaml como se muestra a continuación:

```yaml
variables:
# WARNING: To be modified by Ferrovial technical team ONLY
 
# Pipeline resources
 pool: 'ubuntu-latest'
 azure_service_connection_name: 'SPN_E-SERV-INFRA-DEV_Devops' #Service Principal name
 debugfoldercontent: false # For debugging purpose only
 proceedplanscan: true # Enable/disable Terraform plan scan
 workingDirectory: '$(Build.SourcesDirectory)'
 environment: 'dev' # Must indicate what type of environment will be deployed 
# dev[X], prod[X], qa[X], stg[X]
 
# Terraform state file details
 tf_resource_group_name: 'RG-NAME'
 tf_storage_account_name: 'Unique Storage Account name'
 tf_location: 'Location Storage Account'
 tf_sku_name: 'SKU Storage Account'
 tf_container_name: 'Blob Storage name'
```

## Modificación dentro del Proyecto
Como último paso, es necesario crear el Service Connection dentro del Proyecto para poder acceder a los secretos que se utilizarán mediante Pipeline Variable Group.

### Service Connection
Para realizar la configuración del Service Connection, nos dirigimos a ***Project Settings*** → ***Pipelines*** → ***Service connections*** → ***New service connection*** → ***Azure Resource Manager*** → ***Service principal (manual)***. Luego, con los datos de Service Principal, completamos el formulario.

El Service Principal a utilizar debe tener acceso al KeyVault que alojará las credenciales del Service Principal que se utilizará en el Proyecto, es decir, pueden pertenecer a diferentes Tenant, Subscription, Resource Group, etc.

### Pipeline Variable groups
Una vez configurado el Service Connection correspondiente, procedemos a configurar dentro de ***Pipelines*** → ***Library*** → ***Variable groups*** los secretos que se utilizarán en el pipeline para poder construir el ambiente a deployar, pertenecientes al Service Principal del Proyecto.
El nombre del Variable group debe ser fv-azure-kv-secrets, ya que de esta manera se referencia en el pipeline. En caso de querer cambiar dicho nombre, se requiere actualizar el código del Golden Pipeline.

<img src="../images/pipeline_usage.png" width="800">

