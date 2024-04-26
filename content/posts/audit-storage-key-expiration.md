---
title: "Audit Storage Key Expiration Policy"
summary: I need to know if my Storage Access Keys have an Expiration Policy or not
date: 2024-04-23
aliases: ["/audit-storage-key-expiration"]
tags: ["AzurePolicy", "CustomPolicy", "Azure"]
---

## Azure Policy for Auditing for Storage Account Access Key Expirations

You need expiration policies on your Storage Account access keys. Here's a policy that will audit your subscriptions to make sure they exist!

### Custom Policy

```json {linenos=true}
{
    "properties": {
        "displayName": "Audit - Azure Storage Account Key Expiration Policy",
        "policyType": "Custom",
        "mode": "Indexed",
        "description": "Protect your Storage Account by ensuring an access key expiration policy exists",
        "metadata": {
            "version": "1.0.0-preview",
            "category": "Storage"
        }
    },
    "parameters": {
        "effectType": {
            "type": "string",
            "metadata": {
                "displayName": "Policy Effect",
                "description": "The Policy Effect associated with this Policy Definition"
            },
            "defaultValue": "AuditIfNotExists"
        },
        "keyExpirationPeriodInDays":  {
            "type": "string",
            "metadata": {
                "displayName": "Expiration Period in Days",
                "description": "How long before the key expires"
            },
            "minValue": 
        }
    },
    "policyRule": {
        "if": {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        },
        "then": {
            "effect": "[parameters('effect')]",
            "details": {
                "type": "Microsoft.Storage/storageAccounts/keyPolicy",
                "existenceCondition": {
                    "allOf"
                }
            }
        }
    }
}
```
### Instructions

To add policies via the Azure CLI, save the above as a json document and run this command:

`az policy definition create --name 'Audit Storage Account Key Expiration' --rules ./AuditStorageKeyExpiration.json --subscription <id or name>`