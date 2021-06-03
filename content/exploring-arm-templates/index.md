---
title: "Exploring ARM Templates"
date: 2021-06-01T20:49:16+01:00
draft: false
categories: -undefined
tags:
  - ARM
  - Azure
  - Bicep
---

ARM templates exist in JSON format with properties that you need to fill. Basic properties are all the same but they are a few that are mandatory like $schema.

`$schema` describes properties available in the template. The schema has a date in it indicating the current version of the schema you're working in.

`contentVersion` is a versioning system in the template for development purposes however if we're using Git it's not that important to keep this field up to date.

The most important section is the `resources` section, which is an array of JSON object definitions for resources. Each object describes a service that will deployed. Multiple resources can be deployed using a single template.

Additionally, you have the parameters section. It includes a definition of input parameters for template parametrization. This section is optional. This defines input parameters for the template.

Variables allow you to calculate something dynamically. This allows you to calculate properties during the execution of the template itself based on your input parameters and other variables.

The outputs section allows you to return variables from template execution as template output.

The functions section allows you to create custom JSON expression functions defined by the user. 

Azure Resource Manager speaks to resource providers. This is why you have to keep the apiVersion updated, but with caution so it doesn't lead to any breaking changes.

SKU is a combination of the performance and replication. SKU is short for 'Stock-keeping-Unit'.

It basically stands for an item which is on sale, in layman language. 

In terms of the Microsoft Azure cloud, they basically signify a purchasable SKU under a product. It has a bunch of different shapes of the product. 

Old properties becomes sku when you are calling resources with new apiVersion. 

Bicep does not like single-line objects or single-line arrays. 

Square brackets are for invoking functions within ARM templates. We can use these functions to parametrize our templates and these are evaluated during deployment.

The reason you cannot use concat in bicep is because it's not recommended by the spec. Here you can see the [differences between ARM Template syntax and the native Bicep equivalent](https://github.com/Azure/bicep/blob/main/docs/arm2bicep.md).

For [retriving storage access keys](https://blog.eldert.net/retrieve-azure-storage-access-keys-in-arm-template/) in an ARM template.

The listkeys function requires two inputs, the resource ID of the Storage account and the API version to use. We retrieve the resource ID using the resourceId function, while the API version is available for reference on Microsoft Docs.

The date calls a static API version, and then we can add this to retrieve the primary key during the creation of the API connection. 

### Dealing with Microsoft.Logic Templates

[WorkflowProperties](https://docs.microsoft.com/en-us/azure/templates/microsoft.logic/2019-05-01/workflows?tabs=json#workflowproperties-object) object.

If you set the logicapp state property to the “Disabled” state, the LA will not start unless you specifically activate it. Therefore we set it as enabled for it to automatically run.




## Key Resources

1. When creating Azure Resource Manager templates (ARM templates), you need to understand what resource types are available, and what values to use in your template. The ARM template reference documentation provides these values.

This basically tells you whether you are allowed to use a certain property or not within your resource object.

https://docs.microsoft.com/en-us/azure/templates/

You can see that for each resource type we have all the available versions and we can see all the available properties for a specific resource provider.

Mandatory properties will have 'required' beside them.

2. There is a GitHub repository called Azure Quickstart Templates. It's a list of templates for all the resource providers in Azure with a lot of samples for each one of them.

https://github.com/Azure/azure-quickstart-templates

3. To learn more about parameters syntax in bicep, including how to add metadata, this is an excellent resource: https://ravichaganti.com/blog/bicep-basics-beyond-basics-parameters/


## List of useful az commands

[Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/) is the holy grail.