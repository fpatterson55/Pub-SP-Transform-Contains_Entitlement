Product: SailPoint Identity Security Cloud

Feature: Transform - To detect an existing Active Directory entitlement.

This transform evaluates an Active Directory Group membership to see if it is present. If it is, it will return a value of true otherwise it will return false.

This is open-source software licensed under the MIT License.

Possible use cases: 
1) A transform needs to evaluate if a specific entitlement is currently present on a user. 
2) Helpdesk needs to quickly evaluate if persons have an O365 license which is based on an AD group membership without having to lookup in AD. This data is 
    synchronized to the Help Desk Connected System.
3) An report or certification needs data based on a specific entitlement being available.


Steps to modify in the template and identity profile:
1) Change the name field to match your use Case / purpose, representing the transform name to be shown in the Identity Profile Mapping.
2) Modify the memberOf.contains to match the AD group naming that applies. If your group name is "All Managers" you would modify it to be:
    "(memberOf.contains(\"All Managers\"))",
3) Modify the sourceName row to the name of your Source specific to the environment in your tenant.
4) If the output needs to be something other than "true" or "false", modify the table in the transform with the proper values.
5) Create the Identity Mapping attribute if it is not already present. After creating the transform, apply the transform to the Identity Mapping and preview the change on a few users.


```
{
    "name": "has_O365_License",
    "type": "lookup",
    "attributes": {
        "requiresPeriodicRefresh": true,
        "input": {
            "type": "firstValid",
            "attributes": {
                "values": [
                    {
                        "attributes": {
                            "accountPropertyFilter": "(memberOf.contains(\"O365_Licensing_Tier\"))",
                            "attributeName": "sAMAccountName",
                            "sourceName": "Active Directory - Dev"
                        },
                        "type": "accountAttribute"
                    },
                    "FALSE"
                ]
            }
        },
        "table": {
            "FALSE": "false",
            "default": "true"
        }
    }
}
```