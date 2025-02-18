# GM-PS-IE Simulations

These runs are for exploring coupling between the plasmasphere physics
module (currently occupied by DGCPM) with the GM module (multifluid
BATS-R-US.)

## PLASCIRC_POC
This is the proof-of-concept run to test if we can create psuedo-plumes
in multifluid BATS.  It uses #MAGNETOSPHERE and #POLARBOUNDARY to restrict
IB densities about the equator.  This grows into a plasmasphere that forms
a plume under southward IMF conditions.

## PSGMIE_SIMPLE1WAY
This run is nearly the same as the PlasCirc_PoC runs in entryMF, but
GM is only single fluid and the IE potential is used to drive DGCPM.
Don't forget your test satellites!

## PSGMIE_POC
This is the proof-of-concept simulation for PS->GM coupling.  It is the same
as PlasCirc_PoC except that PS is turned on and is coupling into one of
the GM fluids.

## SWMF Configuration:
```
Config.pl -install=BATSRUS,DGCPM,Ridley_serial
Config.pl -v=GM/BATSRUS,IE/Ridley_serial,PS/DGCPM
Config.pl -o=GM:e=MultiSwIono
make SWMF PIDL
```

# GM-IM/CIMI Simulations

Use the MHD equation file: `ModEquationRecircPe.f90`
Place this file into `GM/BATSRUS/srcEquation`.
This will configure BATS-R-US to use two fluids: Hp and HpPs.
The first fluid a combined solar/polar ion outflow fluid representing
a "typical" MHD simulation.  The second fluid is for recirculating
plasmasphere material.

## Install Legacy Configuration
After Noverber 8, 2021 updates were made to the SWMF which made it neccesary to use a legacy version to continue devleoping the GM-IM coupling.
The following repositories must be cloned explicitly  
Repository                    Branch Head  
MSTEM-QUDA/SWMF               b46608d44daa197a91a2661daf045f18bbbbbf61  
MSTEM-QUDA/util               9595434666951ec26493db2c45245ca1fc79ea02  
MSTEM-QUDA/share              02c4cab7df3ba2a5fa4515ef0d05e466ac1fa815  
MSTEM-QUDA/BATSRUS            d34938b7dc9e135aefa11bde35ad3bf4f8b2c4f7  
MSTEM-QUDA/Ridley_serial      a4ac0f4689a5bf0fffbc566f4c444e5030c2dac2  
mudtop/CIMI2 Branch: ReHpPs   8fa216bf8535faa8583cc53df5b28b0bc18b320e  
  
use git checkout to checkout the apporiate branch heads for the above repositories.  
minor modification to file: GM/BATSRUS/src/ModAMR.f90 masked_amr_criteria -> is_masked_amr_criteria
three refrences need to be replaced.  
  
## SWMF Configuration:
```
Config.pl -install=BATSRUS,CIMI2,Ridley_serial
Config.pl -v=GM/BATSRUS,IE/Ridley_serial,IM/CIMI2
cp [Path to this repository]/swmf_runfiles/psphere_coupled/ModEquationRecircPe.f90 [Path to SWMF install directory]/GM/BATSRUS/srcEquation/
Config.pl -o=GM:e=RecircPe,IM:EarthReHpsH,GridExpanded
make SWMF PIDL
```
## Remake SWMF Macro - QUDA
```
make clean
./Config.pl -uninstall
./Config.pl -install
./Config.pl -v=GM/BATSRUS,IE/Ridley_serial,IM/CIMI2
./Config.pl -o=GM:e=RecircPe,IM:EarthReHpsH,GridExpanded
make SWMF PIDL
```
## Remake SWMF Macro - Full
```
make clean
./Config.pl -uninstall
./Config.pl -install=BATSRUS,CIMI2,Ridley_serial
./Config.pl -v=GM/BATSRUS,IE/Ridley_serial,IM/CIMI2
./Config.pl -o=GM:e=RecircPe,IM:EarthReHpsH,GridExpanded
make SWMF PIDL
```
