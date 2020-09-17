# Nuget

## Setup nuget source
```
nuget sources add -Name WhateverNameYouWant -Source https://nuget.server.here -UserName admin -password pas$w0rd
nuget setApiKey api-key-here -Source WhateverNameYouSet
```

## Update nuget source
```
nuget sources update -Name WhateverNameYouSet -UserName admin -password pas$w0rd
```

## Create & Push Nuget file
You can either build against Release in Visual Studio / Rider and push or you can run

```
dotnet pack -c Release --include-symbols
```

and then push

```
dotnet nuget push yourNugetFile.nupkg -s YourSouceNameHere
```