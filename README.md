# Lenovo ThinkSystem 430-16i NVMe Support
This repository is a guide on how I flashed the firmware and bios on a Lenovo 430-16i (LSI 9400-16i) to enable U.2 NVMe support. By default, Lenovo disables NVMe on this card but it uses the idential chip to the 9400-16i so it can be crossflashed with that card's bios/firmware. These cards are inexpensive and can support multiple U.2 enterprise NVMe using a special cable.

## What You Need
1.  Your Lenovo ThinkSystem 430-16i
2.  Firmware/BIOS packages from Lenovo/Broadcom website
3.  U.2 Enabler cable

## Instructions for Updating Lenovo ThinkSystem 430-16i
1. Go to Lenovo's website and download the firmware and STORCLI utility package:
````
https://support.lenovo.com/us/en/search?query=mpt3.5.430&SearchType=Customer%20search&searchLocation=Masthead
Look for lnvgy_fw_storehba_mpt3.5.430-24.05.00.00-2-a_windows_x86-64
Make sure to sort by 'newest'
````
2. Run the exe and hit 'extract'. You now have a bunch of files in the folder.

![image](https://github.com/user-attachments/assets/b36bcfdd-572d-40b9-91be-a67869d0e824)

3. Open CMD with admin privileges. Navigate to the folder you have extracted the files.

4. Identify the cX ID of the card you want to flash using the extracted STORCLI utility:
````
storcli show all
````
![image](https://github.com/user-attachments/assets/f7d9cdf4-cd02-40fd-8e39-460a4367ff62)


5. Check the current firmware/bios version of the card:
````
storcli /c0 show
````

6. Flash the firmware:
````
storcli /c0 download file=16i_24_00_07_00.fw
````

7. Flash the EFI BIOS:
````
storcli /c0 download efibios file=mpt35sas_x64_24_00_05_00.rom
````

8. Flash the BIOS:
````
storcli /c0 download bios file=mpt35sas_legacy_9_47_03_00.rom
````

9. Make sure card shows correct information:
````
storcli /c0 show
````

10. Restart the computer. LSI Storage Authority should now display the correct firmware.

## Instructions for Flashing Lenovo ThinkSystem 430-16i to 9400-16i 
1. Go to Broadcom's website and download the firmware package:
````
https://www.broadcom.com/support/download-search?pg=Storage+Adapters,+Controllers,+and+ICs&pf=Legacy+Host+Bus+Adapters&pn=HBA+9400-16i+Tri-Mode+Storage+Adapter&pa=&po=&dk=&pl=&l=true
Look for 9400_16i_Pkg_P24_SAS_SATA_NVMe_FW_BIOS_UEFI
````
2. Navigate to the folder you have extracted the previous STORCLI utility. Open CMD with admin privileges. From the new '9400_16i_Pkg_P24_SAS_SATA_NVMe_FW_BIOS_UEFI' folder copy Firmware\HBA_9400-16i_Mixed_Profile, SAS35BIOS_Rel\mpt35sas_legacy.rom and UEFI_BSD_HII_SAS3.5_IT_X64\Signed\mpt35sas_x64.rom to the folder with STORCLI.

3. Identify the cX ID of the card you want to flash using the extracted STORCLI utility:
````
storcli show all
````

4. Check the current firmware/bios version of the card:
````
storcli /c0 show
````

5. Flash the firmware:
````
storcli /c0 download file=HBA_9400-16i_Mixed_Profile.bin
````
![FW](https://github.com/user-attachments/assets/bf416ad0-14f2-432e-9c4a-62eb016e5fae)

6. Flash the EFI BIOS:
````
storcli /c0 download efibios file=mpt35sas_x64.rom
````
![EFI](https://github.com/user-attachments/assets/cf714708-c3fd-45b8-86be-82d20be9b247)

7. Flash the BIOS:
````
storcli /c0 download bios file=mpt35sas_legacy.rom
````
![BIOS](https://github.com/user-attachments/assets/bff7252e-2289-4a1f-a3ce-1978d64750bb)

8. Make sure card shows correct information:
````
storcli /c0 show
````

9. Restart the computer.

![image](https://github.com/user-attachments/assets/a9b650b2-b1e7-433d-bdd7-e779612efab5)


## Cables and Installation
To get it working with a U.2 drive you need a special U.2 enabler cable. From the Broadcom guide they look like this:
![image](https://github.com/user-attachments/assets/bb3a3996-78fb-4ee5-8b65-ca437fbb3d51)

However, I purchased generic ones from Amazon. Be very careful, some sellers will specifically tell you on Amazon/Ebay if the cable does NOT work with the LSI 94XX.
````
https://a.co/d/2YVVmCa

DiLiVing LSI94xx Series Dedicated 2*MiniSAS-HD 8X to 2*U.2 NVMe SSD,SFF-8643 72Pin to 2*SFF-8639 68Pin Cable 75cm(Broadcom MPN 05-50065-00/05-50064-00)
````


### Performance Metrics
Please make sure that the HBA does not overheat during use. It is designed to have constant airflow and does not give you temperature information. If it overheats it will not work properly. My motherboard comes with two temperature probes. I attached one to the inside of the heatsink on the ThinkSystem 430-16i near the chip.
![image](https://github.com/user-attachments/assets/70020f9a-0be4-49d9-8681-6c775a29a65f)

With a software RAID 0 of two Intel P4510 U.2 drives I get:
![image](https://github.com/user-attachments/assets/eaffa31b-9870-4b4a-9388-613efd1bfbbf)


