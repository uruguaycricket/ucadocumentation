## Utilización de tags mandatorios
La utilización de tags dentro de los módulos es obligatoria por estándares de Ferrovial Servicios.
Los tags a utilizar son los siguientes:

| Tag                   | Descripción                                                       | Ejemplo |
| --------------------- | ----------------------------------------------------------------- | ------------- |
| Organization          | Organization Identifier                                           | <pre>Ferrovial Servicios</pre>  |
| CloudProvider         | The reduced name of the cloud provider                            | Azure, AWS, GCP  |
| Project               | The reduced name of the project                                   | Digital Transformation |
| ProjectID             | Project ID                                                        | IT-XXXXX, IO-XXXXX |
| Owner                 | Owner contact email                                               | x@x.com |
| DataClassification    | Sensitivity of data hosted by this resource.                      | Non-business, Public, General, Confidential, Highly confidential |
| Criticality           | Business impact of the resource or supported workload             |Low, Medium, High, Business unit-critical, Mission-critical |
| CostCenter            | Accounting cost center associated with this resource              | 55332, N/A |
| Account               | Resource Subscription Account                                     | Globant-RG-DevSecOps-Dev |
| Environment           | Deployment environment of the application, workload, or service   | Prod, Dev, QA, Stage, Test |
| Region                | Geographical zone                                                 | US-EAST-1 (AWS), West Europe (AZ) |


La columna Tag, representa el nombre exacto que se espera sea utilizado para la definición de la dupla *<font color="red">Key</font>: <font color="blue">“Value”</font>*. Por ejemplo, *<font color="red">CloudProvider</font>: <font color="blue">“Azure”</font>*

## Implementación de tags mandatorios
La implementación de estos tags obligatorios dentro del pipeline se realiza de la siguiente manera:

1. Dentro del archivo infra.tf se deben definir el uso de los tags en los módulos a utilizar. Por ejemplo:
```hcl
module "module reference name" {
 source                  = "source module git"
 ...
 tag_project     = var.tag_project
 tag_owner       = var.tag_owner
 tag_environment = var.tag_environment
 ...
}
```

2. Luego, en el archivo ***infra.tfvars***, realizar la asignación de los valores a utilizar. Por ejemplo:

```hcl
tag_project     = "Project name"
tag_owner       = "Owner email address"
tag_environment = "Environment"
```

3. Para finalizar, validar que en el archivo ***variable.tf*** se encuentren la especificaciones de las variables, incluído en el pipeline.

```hcl
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

