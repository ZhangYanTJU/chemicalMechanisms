# Intro
| fuelName      | CH4 |
| --------------------          | ------------------------------------------------- |
| species       | 16       |
| reactions     | 35        |

# pre-processing
```
dos2unix chem-smook.inp
dos2unix therm-smook-all.dat
dos2unix grimech30_transport.dat
sed -i -e 's/[ \t]*$//g' chem-smook.inp # remove spaces and tab at end of line
sed -i -e 's/[ \t]*$//g' therm-smook-all.dat # remove spaces and tab at end of line
sed -i -e 's/[ \t]*$//g' grimech30_transport.dat # remove spaces and tab at end of line
sed -i '/^$/d' chem-smook.inp # remove blank lines
sed -i '/^$/d' therm-smook-all.dat # remove blank lines
sed -i '/^$/d' grimech30_transport.dat # remove blank lines
sed -i 's/[a-z]/\u&/g' chem-smook.inp #Captical
sed -i 's/[a-z]/\u&/g' therm-smook-all.dat #Captical
sed -i 's/[a-z]/\u&/g' grimech30_transport.dat #Captical
echo "" >> chem-smook.inp # add a blank line at the end of chem-smook.inp
sed -i 1,2d therm-smook-all.dat # delete the first 2 lines
sed -i '1i \ \ \ 200.000  1000.000  5000.000' therm-smook-all.dat # add a line
sed -i '1i THERMO ALL' therm-smook-all.dat # add a line
sed -i 's/./ /79' therm-smook-all.dat # let $79 column be a space
```



# OpenFOAM
```
chemkinToFoam chem-smook.inp therm-smook-all.dat ../transportProperties CH4_smook.OFchem CH4_smook.OFtherm
```
You will get `CH4_smook.OFchem` and `CH4_smook.OFtherm`.
In thermophysicalProperties:
```
chemistryReader foamChemistryReader;
foamChemistryFile "grimech30.OFchem";
foamChemistryThermoFile "grimech30.OFthermo";
```

# FlameMaster
```
ScanMan -i chem-smook.inp -t therm-smook-all.dat -m grimech30_transport.dat -f chemkin -o CH4_smook.pre
rm chem-smook.inp.h chem-smook.inp.chmech chem-smook.inp.chthermo chem-smook.inp.chtrans
```
You will get `CH4_smook.pre`

# Cantera
```
python -m cantera.ck2cti --input=chem-smook.inp --thermo=therm-smook-all.dat --transport=grimech30_transport.dat --output=CH4_smook.cti
```
You will get `CH4_smook.cti`

# Source

https://link.springer.com/book/10.1007%2FBFb0035362