# 🦍 CIS vSphere


![Preview](./docs/images/preview.gif)


> A tool to assess the compliance of a VMware vSphere environment against the CIS Benchmark for VMware vSphere.

> Forked from [karimhabush/cis-vsphere](https://github.com/karimhabush/cis-vsphere) and adapted for my needs

## Requirements
* VMware PowerCLI 12.0.0 or higher
* VMware vSphere 7.0
* Read access to the vCenter or ESXi host

## Usage

1. Clone the repo and navigate to the folder: 
```bash
git clone https://github.com/bwdbethke/cis-vsphere.git
cd cis-vsphere
```
2. Install PowerCLI : 
```powershell
Install-Module -Name VMware.PowerCLI -Scope CurrentUser -Force
```
3. Run the script :
```powershell
.\src\cis-vsphere.ps1
```

3. (Alternatively) Run the script and save the output to a file :
```powershell
.\src\cis-vsphere.ps1 *> CIS_output_vcsa.txt
```

### Notes 

* To verify the patches, you will need to update the `patches.json` file with the latest patches for your environment.
* version numbers can be found at https://esxi-patches.v-front.de/ESXi-7.0.0.html or retrieved similarly to:

```powershell
$BuildNumber = "20325353"
Add-EsxSoftwareDepot https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
$Image = Get-EsxImageProfile | Where Name -Like "*$BuildNumber-standard*"
$Image.VibList | Sort-Object | Select-Object Name,Version | ConvertTo-Json
```
NOTE: Add-EsxSoftwareDepot and Get-EsxImageProfile are slow to get the data and complete, so give them time to finish 😉

NOTE2: the values for build 19482537 and 20325353 are saved as "patches_<_buildnumber_>.json". Copy the desired
build version contents into the generic `patches.json` file prior to running. If you're using an OEM customized ISO, 
then your version numbers will be different.
