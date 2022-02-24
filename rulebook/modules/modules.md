## Definición de módulos y variables de Terraform
Para utilizar el pipeline de despliegue de Infraestructura, es necesario definir dentro de él, todos los módulos que se utilizarán con su correspondientes variables que lo componen.
Estos módulos y variables se utilizarán para la construcción del Terraform Plan, que luego se desplegará utilizando Terraform apply.


## Implementación de módulos y variables
A continuación, indicaremos los pasos a seguir para la implementación de los módulos y sus variables:

1.  Editamos el archivo infra.tf, donde se agregarán los módulos y sus variables. Por ejemplo:
```hcl
module "Module reference name" {
 source = "git::https://MODULE GIT REPOSITORY"
 # Variable definition
 # Place where module’s variables should be defined
 resource_group_name     = var.resource_group_name
 resource_group_location = var.resource_group_location
 # ...
 # Mandatory tags - More details in next section
 tag_organization       = var.tag_organization
 tag_cloudprovider      = var.tag_cloudprovider
 tag_project            = var.tag_project
 tag_projectid          = var.tag_projectid
 tag_owner              = var.tag_owner
 tag_dataclassification = var.tag_dataclassification
 tag_criticality        = var.tag_criticality
 tag_costcenter         = var.tag_costcenter
 tag_account            = var.tag_account
 tag_environment        = var.tag_environment
 tag_region             = var.tag_region
}
```

2. Luego, dentro del archivo variable.tf, definimos el tipo de variable que se utilizará, siguiendo los lineamos de Terraform. Por ejemplo:
```hcl
variable "resource_group_name" {
 type    = string
 default = ""
}
variable "resource_group_location" {
 type    = string
 default = ""
}
variable "tag_project" {
 type    = string
 default = ""
}
variable "tag_owner" {
 type    = string
 default = ""
}
variable "tag_environment" {
 type    = string
 default = ""
}
```

3. Por último, se asignará el valor correspondiente a las variables que se utilizarán en el archivo infra.tfvars. Por ejemplo:

```hcl
#EXAMPLE RESOURCE GROUP VARS
resource_group_name     = "Resource-Group-Name"
resource_group_location = "Example-westeurope"
#Mandatory tags definition
tag_organization       = "null"
tag_cloudprovider      = "null"
```

