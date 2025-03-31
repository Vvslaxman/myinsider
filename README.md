# Myinsider-Mavericks

- Started with cloning the azure devops repository which i was given access permissions inside Visual Studio.
- This project was using MVC+Entity Framework Tools and .NET framework 4.5(which will be present inside the windows OS, doesnt require any sdk specifically)
- Opened the project working directory inside the Visual Studio 2022 and started running process.
- Changed directory to C:\Users\Administrator\source\repos\Mavericks\MyInsider\Application that contained:
```bash
MyInsider\Application
  -DataUploadUtility
  -FileEncryptDecrypt
  -InsiderTrading
  -InsiderTradingDAL
  -InsiderTradingEncryption
  -INSIDERTRADINGEXCELWRITER
  -INSIDERTRADINGMASSUPLOAD
  -InsiderTradingSSO
  -RestrictedListCRM
  -InsiderTrading.sln
```
- First directly clicked restore all packages and update command inside visual studio's package manager, which caused many git changes so I deleted and removed everything.
- Cloned and opened again, installed nuget CLI to use it for installing same exact configuration of package.json available in above given directory folders.
- Redirected and opened each directory which ever is having package.json and installed them using nuget.
- When cloned packages.json was only present in maybe 3 folders inside Application folder and after this process each of them contained updated exact required packages.json.
- So i now i tried to build the main InsiderTrading folder but got below error:
```markdown
Path
C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading
PM> dotnet clean Build started 3/28/2025 10:52:33 AM. 1>Project "C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading\InsiderTrading.csproj" on node 1 (Clean target(s)). 1>C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading\InsiderTrading.csproj(3319,3): error MSB4019: The imported project "C:\Program Files\dotnet\sdk\9.0.201\Microsoft\VisualStudio\v17.0\WebApplications\Microsoft.WebApplication.targets" was not found. Confirm that the expression in the Import declaration "$(VSToolsPath)\WebApplications\Microsoft.WebApplication.targets", which evaluated to "C:\Program Files\dotnet\sdk\9.0.201\Microsoft\VisualStudio\v17.0\WebApplications\Microsoft.WebApplication.targets", is correct, and that the file exists on disk. 1>Done Building Project "C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading\InsiderTrading.csproj" (Clean target(s)) -- FAILED.
Build FAILED.
   "C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading\InsiderTrading.csproj" (Clean target) (1) ->
     C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading\InsiderTrading.csproj(3319,3): error MSB4019: The imported project "C:\Program Files\dotnet\sdk\9.0.201\Microsoft\VisualStudio\v17.0\WebApplications\Microsoft.WebApplication.targets" was not found. Confirm that the expression in the Import declaration "$(VSToolsPath)\WebApplications\Microsoft.WebApplication.targets", which evaluated to "C:\Program Files\dotnet\sdk\9.0.201\Microsoft\VisualStudio\v17.0\WebApplications\Microsoft.WebApplication.targets", is correct, and that the file exists on disk.

0 Warning(s)
1 Error(s)
Time Elapsed 00:00:00.11
```
The error message indicates that the project file InsiderTrading.csproj is trying to import a target file Microsoft.WebApplication.targets from a path that does not exist. This is likely because the project is targeting the .NET Framework and is being built using the .NET SDK, which does not include the necessary targets for web applications.
To resolve this issue, you should build the project using MSBuild from the Visual Studio Developer Command Prompt instead of using dotnet CLI. Here are the steps:
1.	Open the Developer Command Prompt for Visual Studio. You can find it in the Start menu under Visual Studio.
2.	Navigate to the project directory:  cd C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\InsiderTrading
3.	Clean the project using MSBuild:   msbuild /t:Build
4.	Build the project using MSBuild:   msbuild /t:Build
5.	Next we need to run using msbuild /t:Run. 

- Even after doing above it resulted in 284 Warnings and 78 errors
- C:\Users\Administrator\Source\Repos\Mavericks\MyInsider\Application\INSIDERTRADINGEXCELWRITER\ExcelWriter\CommonOpenXML.cs
- This was mainly due to some verison changing of DocumentFormat.OpenXml, the one that installed was a latest 3.3 version but our project was built using an older version 2.5, so i downgraded it buy uninsatlling latest and installing 2.5 version. Thought this would resolve the issue but got that two are present inside the same and both are being utilised
  
```bash
The type 'UInt32Value' exists in both 'DocumentFormat.OpenXml.Framework, Version=3.3.0.0, Culture=neutral, PublicKeyToken=8fb06cb64d019a17' and 'DocumentFormat.OpenXml, Version=2.5.5631.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
```
this was inside packages.config: 
<?xml version="1.0" encoding="utf-8"?> 
  <packages> 
    <package id="DocumentFormat.OpenXml" version="2.5" targetFramework="net48"  /> 
    <package id="DocumentFormat.OpenXml" version="3.3" targetFramework="net46"  />
  </packages>

removed that 3.3 one and still the issue wasn't resolved.
To eradicate it we need to go into each folder .csproj file and remove latest version and make sure only old one is present-
```bash
<ItemGroup>
    <Reference Include="DocumentFormat.OpenXml, Version=2.5.5631.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35, processorArchitecture=MSIL">
      <HintPath>..\packages\DocumentFormat.OpenXml.2.5\lib\DocumentFormat.OpenXml.dll</HintPath>
    </Reference>
</ItemGroup>
```
- Now if i clean and build the errors got reduced to 1, which indicated i need to clean+build a folder other than InsiderTrading that is responsible for error.
- Then again i tried to run the solution using msbuild but again build got failed.
- Next i did Cleaning+Building of each project folder using the Visual studio interface, got zero errors and many warnings. So I run the solution using the Run button present inside interface.
- Now receiving the connection sql error, so i restored the Vigilante_master DB and encrypted string with MD5 and replaced connection string.
- Next cleared the 
