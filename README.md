# How To Setup A Private Nuget Package Repo For MonkeyLoader
Prerequisites
- [DotNet8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
- [Git](https://git-scm.com/)
- Your own [Git server](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server) or a git hosting service such as [GitHub](github.com)

# Getting Source & Compiling
You will need to clone the Repo and Compile the Debug version of the application

Create a folder and open a command prompt in it,  you can select the address bar and type cmd
![alt text](https://i.imgur.com/zoSNN8Z.png "Address Bar Selected")

![alt text](https://i.imgur.com/w5tYZNQ.png "cmd in Adress Bar")

![alt text](https://i.imgur.com/iSUJaeb.png "cmd prompt in folder")

To clone the repo run `git clone https://github.com/MonkeyModdingTroop/ReferencePackageGenerator.git`
Once you have the source downloaded you need to compile the code, move to the directory with the .csproj file (`cd ReferencePackageGenerator`) and run `dotnet build` to build the project

# Generate Json File For Publishing

In the project folder find this directory `MonkeyLoader.ReferencePackageGenerator\bin\Debug\net8.0`
you should see the following

![alt text](https://i.imgur.com/TYZudQd.png "Folder Of Compiled Code")

Now last thing we need to do it Generate the Json File open the folder with the .csproj which should be ReferencePackageGenerator\MonkeyLoader.ReferencePackageGenerator
in the new prompt type dotnet run

![alt text](https://i.imgur.com/PJEMLC7.png "CMD Prompt Showing Json Created")

Now open the Json file in any text editor you'd like
you should see something like

```
{
  "DocumentationPath": null,
  "Authors": [],
  "DllTargetPath": "G:\\ResoModding\\ReferencePackageGenerator-master\\MonkeyLoader.ReferencePackageGenerator\\Public",
  "ExcludePatterns": [
    "Microsoft\\..+",
    "System\\..+",
    "Mono\\..+",
    "UnityEngine\\..+"
  ],
  "IconPath": null,
  "IconUrl": null,
  "IncludePatterns": [
    ".+\\.dll",
    ".+\\.exe"
  ],
  "NupkgTargetPath": "G:\\ResoModding\\ReferencePackageGenerator-master\\MonkeyLoader.ReferencePackageGenerator\\Packages",
  "PackageIdPrefix": "",
  "ProjectUrl": null,
  "PublishTarget": null,
  "Recursive": false,
  "RepositoryUrl": null,
  "SourcePath": "G:\\ResoModding\\ReferencePackageGenerator-master\\MonkeyLoader.ReferencePackageGenerator",
  "Tags": [],
  "TargetFramework": null,
  "VersionBoost": "0.0",
  "VersionOverrides": null
}
```

The main things we need to edit will be
```
"DllTargetPath"
"NupkgTargetPath"
"SourcePath"
"Authors"
"PublishTarget"
```

In authors put your account name in a list. `"Authors": ["TumnusB"]`, as an example

Dll Target Path is where the DLLs will get copied to with some code

NupkgTargetPath is where the generated nupkg will get placed

Source Path is where we will get the DLLs from resonite which you can find via steam, right click on resonite and then manage and browse local files

![alt text](https://i.imgur.com/hA6RuNG.png "How to get to local files of Resonite")

Once you have the folder open you need to navigate to Resonite_Data\Managed

![alt text](https://i.imgur.com/KNX99JO.png "Example File Path")

Once here click the address bar and copy the path and paste it to SourcePath
Finally we need to add some Keys to the PublishTarget

```
    "PublishTarget": {
        "ApiKey": "ghp-xxxxxxx",
        "Publish": true,
        "Source": "https://nuget.pkg.github.com/TumnusB/index.json"
    },
```

Replace source with your own user and the ApiKey with your own access key. Make sure the key has the write:packages permission
(If you're using github, you can add the key here with [this](https://github.com/settings/tokens/new?description=Resonite_Nuget&scopes=repo,write:packages))

<i>Optionally add a prefix to the packages such as Resonite via editing the "PackageIdPrefix": "", key</i>

# Finally Run The .EXE And Pass It The Json File

Copy the JSON into the same folder as the .exe to make life easier and then open a CMD prompt in that folder then run the program with the json file as an argument `ReferencePackageGenerator.exe Resonite.Unity.json`

![alt text](https://i.imgur.com/Qho4W5z.png "Image of EXE and Json")

Once you set up like this you should be able to just hit enter and it will start to publish the pakages

# Adding Packages to your Repo

`dotnet nuget push FilePaath/FileName.nupkg -s https://nuget.pkg.github.com/TumnusB/index.json -k ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
Replace my user and token with your own

# Adding Repo To Your Project

Once you have a Project set up you can run the following command in the folder of the .csproj
`dotnet nuget add source https://nuget.pkg.github.com/TumnusB/index.json --name github-TumnusB --username gh-TumnusB --password ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

Replace my user and token with your own

For more info on setting up your own NuGet feed on github you can take a look at [this](https://gist.github.com/xt0rted/3d77fe4853c490c4532377839adf0acf)
