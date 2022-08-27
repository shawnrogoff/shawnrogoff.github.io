---
title: Useful Commands I've Used
date: 2022-08-23 11:00:00 -500  
categories: [docs]
tags: [commands, environment] # TAG names should always be lowercase
---

# Save Environment Variable On Your Machine
 This could be useful to save Jwt Token KEY locally. Remember to make a note to do the same on host machine.

- Open Command Prompt as Admin
- run command: setx KEY "\<key value>" /M

**The /M makes it a system variable, and not local

### To retrieve this value (in C#):

```c#
var key = Environment.GetEnvironmentVarialbe("KEY");
```

---

# Entity Framework
### Migrations & Database Updates:

```shell_session
dotnet ef migrations add <MigrationName>
```

```shell_session
dotnet ef database update
```

---