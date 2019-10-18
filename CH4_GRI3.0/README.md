# Intro
| fuelName      | CH4 |
| --------------------          | ------------------------------------------------- |
| species       | 53       |
| reactions     | 325        |

# pre-processing
```
dos2unix grimech30.dat
dos2unix thermo30.dat
dos2unix transport.dat
sed -i -e 's/[ \t]*$//g' grimech30.dat # remove spaces and tab at end of line
sed -i -e 's/[ \t]*$//g' thermo30.dat # remove spaces and tab at end of line
sed -i -e 's/[ \t]*$//g' transport.dat # remove spaces and tab at end of line
sed -i '/^$/d' grimech30.dat # remove blank lines
sed -i '/^$/d' thermo30.dat # remove blank lines
sed -i '/^$/d' transport.dat # remove blank lines
sed -i 's/[a-z]/\u&/g' grimech30.dat #Captical
sed -i 's/[a-z]/\u&/g' thermo30.dat #Captical
sed -i 's/[a-z]/\u&/g' transport.dat #Captical
echo "" >> grimech30.dat # add a blank line at the end of grimech30.dat
sed -i 1,2d thermo30.dat # delete the first 2 lines
sed -i '1i \ \ \ 200.000  1000.000  5000.000' thermo30.dat # add a line
sed -i '1i THERMO ALL' thermo30.dat # add a line
sed -i 's/./ /79' thermo30.dat # let $79 column be a space
```



# OpenFOAM
```
chemkinToFoam grimech30.dat thermo30.dat ../transportProperties CH4_GRI3.0.OFchem CH4_GRI3.0.OFtherm
```
You will get `CH4_GRI3.0.OFchem` and `CH4_GRI3.0.OFtherm`.
In thermophysicalProperties:
```
chemistryReader foamChemistryReader;
foamChemistryFile "grimech30.OFchem";
foamChemistryThermoFile "grimech30.OFthermo";
```

# FlameMaster
```
ScanMan -i grimech30.dat -t thermo30.dat -m transport.dat -f chemkin -o CH4_GRI3.0.pre
rm grimech30.dat.h grimech30.dat.chmech grimech30.dat.chthermo grimech30.dat.chtrans
```
You will get `CH4_GRI3.0.pre`

# Cantera
```
python -m cantera.ck2cti --input=grimech30.dat --thermo=thermo30.dat --transport=transport.dat --output=CH4_GRI3.0.cti
```
You will get `CH4_GRI3.0.cti`

# Source

http://combustion.berkeley.edu/gri-mech/version30/text30.html