# How To Setup A Private Nuget Package Repro For MonkeyLoader Using Github

**You Need  Github Account If That Wasnt Obvious**

# Setting Up A Token For Nuget Package

Click this link [New personal access token (classic)](https://github.com/settings/tokens/new?description=Resonite_Nuget&scopes=repo,write:packages)`

Should bring you to a site that looks like this

![Alt text](https://i.imgur.com/gPfx0BF.png "PAT Settings")

The Note should say Resonite_Nuget as this is what I set it to by default, you may want to change the key expiration.


The correct values are already set which is write:packages which is the persmission to allow packages to be uploaded, at the bottom of the page is a green Generate Token Button

![Alt text](https://i.imgur.com/aZtkfHQ.png "Generate Token Button")

The Next Page you will be given your token note this down somewhere as once you leave the page you will need to generate it as the token is only shown once on creation the page also hilights this.

![Alt text](https://i.imgur.com/j4GomVl.png "Generated Token")
(Note This Token Is Deleted For Security)

# Getting Source & Compiling

Once you have your token you need to clone the Repo and Compile the Debug version of the application at time of this Readme (20/08/2024)

I advise installing [Visual Studio](https://visualstudio.microsoft.com/) and [DotNet8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)

When installing Visual Studio make sure to select .Net Desktop Development
![alt text](https://i.imgur.com/5TwtDSF.png "Dot Net Installer Desktop Dev Option")

you can do this if you have Github Desktop or [git-scm](https://git-scm.com/download/win)

then run git clone https://github.com/MonkeyModdingTroop/ReferencePackageGenerator.git

If you dont know how to use git you can just download the source as a .zip by pressing code and then download zip as seen below

![Alt text](https://i.imgur.com/2iu6dGS.png "Download Zip")

Once you have the source downloaded open the .sln and you should be greeted with a screen like this

![Alt text](https://i.imgur.com/FxSZkAV.png "VS Display")

At the top of the leave the the Debug and Any CPU as they are and click build then Build Solution
![Alt text](https://i.imgur.com/eeZVd0X.png "VS Display")

and wait for the compiler to do its thing you will see some warning but we can ignore them

# Generate Json File For Publishing

where ever you downloaded the project find the folder

\MonkeyLoader.ReferencePackageGenerator\bin\Debug\net8.0

should see the following

![alt text](https://i.imgur.com/TYZudQd.png "Folder Of Compiled Code")

Now last thing we need to do it Generate the Json File open the folder with the .csproj which should be RootFolder\ReferencePackageGenerator\MonkeyLoader.ReferencePackageGenerator

open a command prompt here nd do donnet run, you can select the address bar and type cmd
![alt text](https://i.imgur.com/zoSNN8Z.png "Address Bar Selected")

![alt text](https://i.imgur.com/w5tYZNQ.png "cmd in Adress Bar")

![alt text](https://i.imgur.com/iSUJaeb.png "cmd prompt in folder")

in the new prompt type dotnet run

![alt text](https://i.imgur.com/PJEMLC7.png "CMD Prompt Showing Json Created")

Now open the Json file in any text editor youd like i use [Notepad++](https://notepad-plus-plus.org/)

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
"DllTargetPath":
"NupkgTargetPath":
"SourcePath":
"Authors":[]
"PublishTarget": null,
 ```

Authors you just need to dd your github name so its should be     "Authors": ["TumnusB"], as an example

Dll Target Path is where the DLLs will get copied to with some code

NupkgTargetPath is where the generated nupkg will get placed

Source Path is where we will get the DLLs from resonite which you can find via steam, right click on resonite and then manage and browse local files

![alt text](https://i.imgur.com/hA6RuNG.png "How to get to local files of Resonite")

once you have the folder open you need to navigate to Resonite_Data\Managed
![alt text](https://i.imgur.com/KNX99JO.png "Example File Path")

once here click the address bar and copy the path and paste it to SourcePath

Finally we need to add some Keys to the PublishTarget

```
    "PublishTarget": {
        "ApiKey": "ghp-xxxxxxx (that you definitely copied down and didnt lose)",
        "Publish": true,
        "Source": "https://nuget.pkg.github.com/TumnusB/index.json"
    },
```
Replace TumnusB with your git hub account

<b>Optionally add a prefix to the packages such as Resonite via editing the "PackageIdPrefix": "", key</b>

# Finally Run The .EXE And Pass It The Json File

I would copy the Json into the same folder as the .exe to make life easier aand then open a CMD prompt as before in the folder the command should look like

```
ReferencePackageGenerator.exe Resonite.Unity.json
```

![alt text](https://i.imgur.com/Qho4W5z.png "Image of EXE and Json")

once you set up like this you should be able to just hit enter and it will start to publish the pakages

# Adding Packages to your Repo
```
dotnet nuget push FilePaath/FileName.nupkg -s https://nuget.pkg.github.com/TumnusB/index.json -k ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
Replace my user with yours

# Adding Repo To Your Project

Once you have a Project set up you can run the following command in the folder of the .csproj
```
dotnet nuget add source https://nuget.pkg.github.com/TumnusB/index.json --name github-TumnusB --username gh-TumnusB --password ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Replace my user with yours

[For more info on the git nuget stuff look at this as this helped me get set up as well as Banane9](https://gist.github.com/xt0rted/3d77fe4853c490c4532377839adf0acf)