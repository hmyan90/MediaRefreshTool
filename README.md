
## RefreshMedia.ps1
Powershell script for refreshing Windows 10 Media with Dynamic Updates and adding additional Language Packs and Feature On Demands Offline.

## Preparation before running
Before run the script, please download all dynamic update packages, Feature On Demand ISO, Language Pack ISO, 
and place them in directory $packagesPath before continue.
* Download latest SSU package, e.g. windows10.0-kb4499728-x64.msu, put it in $packagesPath/SSU/windows10.0-kb4493510-x64.msu
* Download latest LCU package, e.g. windows10.0-kb4497934-x64.msu, put it in $packagesPath/LCU/windows10.0-kb4497934-x64.msu
* Download latest SafeOS package, e.g Windows10.0-KB4499728-x64.cab, put it in $packagesPath/SafeOS/Windows10.0-KB4499728-x64.cab
* Download latest SetupDU package, e.g. Windows10.0-KB4499543-x64.cab, put it in $packagesPath/SetupDU/Windows10.0-KB4499728-x64.cab
* Download OEM Feature On Demand ISO, e.g. 17763.1.180914-1434.rs5_release_amd64fre_FOD-PACKAGES_OEM_PT1_amd64fre_MULTI.iso, 
  and put it in $packagesPath/FOD/17763.1.180914-1434.rs5_release_amd64fre_FOD-PACKAGES_OEM_PT1_amd64fre_MULTI.iso
* Download OEM Language Pack ISO, e.g. 17763.1.180914-1434.rs5_release_CLIENTLANGPACKDVD_OEM_MULTI.iso, 
  and put it in $packagesPath/LP/17763.1.180914-1434.rs5_release_CLIENTLANGPACKDVD_OEM_MULTI.iso

## Description 
The script will do the following four steps sequentially

#### Preparation
* Check input parameters and package layout are okay.
* Check local environment has enough disk space for working. We need about 15GB. 
* Export Winre.wim from install.wim 

#### Patch DU 
* Patch SSU, SafeOS and LCU to Windows Preinstallation environment
* Patch SSU, SafeOS and LCU to Windows Setup environment
* Patch SSU, SafeOS and LCU to Windows Recovery environment
* Patch SSU, SafeOS and LCU to Windows Main OS

#### Patch FoD
* Add capabilities to Windows Main OS

#### Patch LangPack
* Add Language Packs to Windows Recovery environment
* Add Recovery languages to Windows Main OS

## Usage
.\RefreshMedia.ps1 [-Media] <String> [-Index] <Int32> [-packagesPath] <String> [-Target] <String> [[-capabilityList] <String[]>] [[-langList] <String[]>] [[-logPath] <String>] [<CommonParameters>]

-media <String>
    Specifies the location of media that needs to be refreshed.
        
-index <Int32>
    Specifies the edition/index that need to build at last. We will only refresh this edition/index with DU contents, FoDs and 
    LangPacks.
    
-packagesPath <String>
    Specifies the location of all DUs, FoDs and LPs. This folder should contain below optional subdirectories:
        SSU/
        LCU/
        SafeOS/
        SetupDU/
        LP/
        FOD/
    
-target <String>
    Specifies the location to store the refreshed media. This directory will also be used as working directory, 
    so initially script will check to make sure this directory is empty and there are enough space for working.
    
-capabilityList <String[]>
    Specifies which capabilities need to be installed.
    
-langList <String[]>
    Specifies which languages need to be installed. This will search for Language Packs and Recovery Language. 
    We will not support Language Interface Packs (LIPs) for this version.
    
-logPath <String>
    Specifies the location of log file.

## Example
.\RefreshMedia.ps1 -media "E:\MediaRefreshTest\old media" -index 1 -packagesPath "E:\MediaRefreshTest\packages" -target "E:\MediaRefreshTest\new media" 
                   -capabilityList "Language.Basic~~~fr-FR~0.0.1.0","Tools.DeveloperMode.Core~~~~0.0.1.0" -langList "fr-fr","zh-cn"