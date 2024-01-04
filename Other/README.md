# Other

## M Three Sixty Five

***NOTE: This is the challenge portal that will start the deployable container environment for the "M Three Sixty Five" challenge set below.***
  
***There is no flag for this challenge itself.***
  
***Connect with SSH, with username `user` and SSH password `userpass`. Your syntax may look like: `ssh user@chal.ctf.games -p [PORTNUMBER]`***
  
***When you connect to the session for the very first time, you will be authenticated into a Microsoft 365 environment. WARNING: Once you disconnect, you will need to restart your container to reauthenticate** **Press the `Start` button on the top-right to begin this challenge.***

### General Info

*Welcome to our hackable M365 tenant! Can you find any juicy details, like perhaps the **street address** this organization is associated with?*

### Walkthrough

Connecting to the tenant, we are given a custom PowerShell prompt which mentions [AADInternals](https://aadinternals.com/aadinternals/).

![General Info - AADInternals Banner](general_info_aadinternals_banner.png)

To get the street address of the organisation, we run `Get` (ref: [Get-AADIntTenantOrganisationInformation](https://aadinternals.com/aadinternals/#get-aadinttenantorganisationinformation-ad))

```powershell
> Get-AADIntTenantOrganisationInformation
```

```

```
### Conditional Access

*This tenant looks to have some odd [Conditional Access Policies](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/overview). Can you find a weird one?*

### Walkthrough

Reading the provided policies and cross-referencing the cmdlets within [AADInternals](https://aadinternals.com/aadinternals/), we can use [Get-AADIntConditionalAccessPolicies](https://aadinternals.com/aadinternals/#get-aadintconditionalaccesspolicies-a) to view these policies:

```powershell
> Get-AADIntConditionalAccessPolicies
```

![Conditional Access - Flag](conditional_access_flag.png)

```
flag{d02fd5f79caa273ea535a526562fd5f7}
```

### Teams

*We observed saw some sensitive information being shared over a Microsoft Teams message! Can you track it down?*

### Walkthrough

To view the latest Team's message, we can use [Get-AADIntTeamsMessages](https://aadinternals.com/aadinternals/#get-aadintteamsmessages-t).

```powershell
> Get-AADIntTeamsMesages
```

![](teams_flag.png)

```
flag{f17cf5c1e2e94ddb62b98af0fbbd46e1}
```
### The President

*One of the users in this environment seems to have unintentionally left some information in their account details. Can you track down The President?*

### Walkthrough

